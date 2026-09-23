# Expand-and-Contract Scenarios

Detailed step-by-step implementations for common migration patterns.

## Scenario 1: Renaming a Column

**Goal:** Rename `userName` to `displayName`

### Deploy 1: Expand (Add New Column)

```prisma
model User {
  id          String  @id @default(cuid())
  userName    String  // Keep old column
  displayName String? // Add new column (nullable initially)
}
```

```bash
npx prisma migrate dev --name add_display_name_column
```

**Application code:** Write to both columns, read from `displayName ?? userName`

### Between Deploys: Migrate Data

```sql
-- Run as data migration script, NOT in Prisma migrate
UPDATE "User" SET "displayName" = "userName" WHERE "displayName" IS NULL;
```

For large tables, batch the update:

```typescript
async function backfillDisplayName() {
  const batchSize = 1000;
  let processed = 0;

  while (true) {
    const result = await prisma.$executeRaw`
      UPDATE "User"
      SET "displayName" = "userName"
      WHERE "displayName" IS NULL
      LIMIT ${batchSize}
    `;

    if (result === 0) break;
    processed += result;
    console.log(`Backfilled ${processed} rows`);
    await new Promise(r => setTimeout(r, 100)); // Rate limit
  }
}
```

### Deploy 2: Make New Column Required

```prisma
model User {
  id          String  @id @default(cuid())
  userName    String  // Still keep old
  displayName String  // Now required (remove ?)
}
```

```bash
npx prisma migrate dev --name make_display_name_required
```

**Application code:** Read only from `displayName`, still write to both

### Deploy 3: Contract (Remove Old Column)

```prisma
model User {
  id          String @id @default(cuid())
  displayName String
}
```

```bash
npx prisma migrate dev --name remove_user_name_column
```

---

## Scenario 2: Changing Column Type (String to Enum)

**Goal:** Change `status: String` to `status: Status` (enum)

### Deploy 1: Expand (Add New Enum Column)

```prisma
enum Status {
  PENDING
  ACTIVE
  INACTIVE
}

model Order {
  id         String  @id @default(cuid())
  status     String  // Keep old string column
  statusEnum Status? // Add new enum column
}
```

**Application code:** Write to both, read from `statusEnum ?? parseStatus(status)`

### Between Deploys: Migrate Data

```typescript
async function backfillStatusEnum() {
  const mapping: Record<string, Status> = {
    'pending': 'PENDING',
    'active': 'ACTIVE',
    'inactive': 'INACTIVE',
  };

  for (const [oldValue, newValue] of Object.entries(mapping)) {
    await prisma.order.updateMany({
      where: { status: oldValue, statusEnum: null },
      data: { statusEnum: newValue },
    });
  }
}
```

### Deploy 2: Switch to Enum, Keep String

```prisma
model Order {
  id         String  @id @default(cuid())
  status     String  // Keep for rollback safety
  statusEnum Status  // Now required
}
```

**Application code:** Read only from `statusEnum`, write to both

### Deploy 3: Contract (Remove String, Rename Enum)

```prisma
model Order {
  id     String @id @default(cuid())
  status Status // Renamed from statusEnum
}
```

Or keep DB column name with `@map`:

```prisma
model Order {
  id     String @id @default(cuid())
  status Status @map("statusEnum")
}
```

---

## Scenario 3: Adding a Non-Nullable Column

**Goal:** Add required `email` column to existing `User` table

### Deploy 1: Add Nullable Column

```prisma
model User {
  id    String  @id @default(cuid())
  name  String
  email String? // Nullable first
}
```

**Application code:** Start collecting email for new users

### Between Deploys: Backfill Data

```typescript
await prisma.user.updateMany({
  where: { email: null },
  data: { email: 'unknown@example.com' }, // Or generate from other data
});
```

### Deploy 2: Make Column Required

```prisma
model User {
  id    String @id @default(cuid())
  name  String
  email String // Now required
}
```

---

## Scenario 4: Renaming a Table

**Goal:** Rename `User` table to `Account`

### Option A: Database-Level Only (@@map)

If keeping database table name but changing Prisma model:

```prisma
model Account {
  id   String @id @default(cuid())
  name String
  @@map("User") // Prisma: Account, DB: User
}
```

Code-only change, no migration needed.

### Option B: Full Table Rename

#### Deploy 1: Create New Table

```prisma
model User {
  id   String @id @default(cuid())
  name String
}

model Account {
  id   String @id @default(cuid())
  name String
}
```

#### Between Deploys: Copy Data

```typescript
const users = await prisma.user.findMany();
for (const user of users) {
  await prisma.account.create({
    data: { id: user.id, name: user.name },
  });
}
```

#### Deploy 2: Update Application

Read from `Account`, write to both

#### Deploy 3: Remove Old Table

```prisma
model Account {
  id   String @id @default(cuid())
  name String
}
```

---

## Scenario 5: Splitting a Table

**Goal:** Extract `address` fields from `User` into separate `Address` table

### Deploy 1: Expand (Create New Table)

```prisma
model User {
  id      String   @id @default(cuid())
  name    String
  // Keep old fields
  street  String?
  city    String?
  country String?
  // Add relation
  address Address?
}

model Address {
  id      String @id @default(cuid())
  street  String
  city    String
  country String
  user    User   @relation(fields: [userId], references: [id])
  userId  String @unique
}
```

### Between Deploys: Migrate Data

```typescript
const usersWithAddress = await prisma.user.findMany({
  where: { street: { not: null }, address: null },
});

for (const user of usersWithAddress) {
  await prisma.address.create({
    data: {
      street: user.street!,
      city: user.city!,
      country: user.country!,
      userId: user.id,
    },
  });
}
```

### Deploy 2: Update Application

Read from `user.address`, write to both `Address` and old fields

### Deploy 3: Contract (Remove Old Fields)

```prisma
model User {
  id      String   @id @default(cuid())
  name    String
  address Address?
}
```

---

## Scenario 6: Merging Tables

**Goal:** Merge `Profile` into `User`

### Deploy 1: Expand (Add Fields to User)

```prisma
model User {
  id      String   @id @default(cuid())
  email   String
  // Add fields from Profile
  bio     String?
  avatar  String?
  profile Profile?
}

model Profile {
  id     String @id @default(cuid())
  bio    String
  avatar String
  user   User   @relation(fields: [userId], references: [id])
  userId String @unique
}
```

### Between Deploys: Copy Data

```typescript
const profiles = await prisma.profile.findMany();
for (const profile of profiles) {
  await prisma.user.update({
    where: { id: profile.userId },
    data: { bio: profile.bio, avatar: profile.avatar },
  });
}
```

### Deploy 2: Update Application

Read from `user.bio`/`user.avatar`, write to both

### Deploy 3: Contract (Remove Profile Table)

```prisma
model User {
  id     String  @id @default(cuid())
  email  String
  bio    String?
  avatar String?
}
```
