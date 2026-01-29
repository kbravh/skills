# Deployment Checklists & Commands

## Pre-Deployment Checklist

### Before Every Migration Deploy

- [ ] Schema change is additive (no removals in same deploy as additions)
- [ ] Application code handles both old and new structure
- [ ] Migration tested in staging environment
- [ ] Data migration script ready (if needed between deploys)
- [ ] Rollback plan documented

### Expand Phase Checklist

- [ ] New column/table created
- [ ] New column is nullable OR has safe default
- [ ] Application writes to both old and new
- [ ] Application reads from new with fallback to old
- [ ] No errors in logs after deploy

### Migrate Phase Checklist

- [ ] All existing data backfilled to new structure
- [ ] Verified no null values in new required columns
- [ ] Application fully uses new structure for reads
- [ ] Old structure still receiving writes (for rollback safety)

### Contract Phase Checklist

- [ ] All application instances updated to new code
- [ ] Old structure no longer referenced in codebase
- [ ] Sufficient time since migrate phase (1-2 weeks recommended)
- [ ] Backups verified
- [ ] Old column/table removed

## Rollback Strategy

The expand-and-contract pattern enables safe rollbacks at each phase:

| Phase | Rollback Action | Data Impact |
|-------|-----------------|-------------|
| **Expand** | Deploy previous code version | None - old structure intact |
| **Migrate** | Revert to reading from old structure | None - data still in both |
| **Contract** | Requires data restore from backup | Data loss possible |

**Recommended:** Keep old structures for 1-2 weeks after all code migrated before contract phase.

### Rollback Commands

```bash
# View migration history
npx prisma migrate status

# Mark failed migration as rolled back (manual intervention needed)
npx prisma migrate resolve --rolled-back "migration_name"

# Mark migration as applied (skip execution)
npx prisma migrate resolve --applied "migration_name"
```

## Commands Reference

### Development

```bash
# Create and apply migration
npx prisma migrate dev --name descriptive_name

# Generate Prisma Client (after schema changes)
npx prisma generate

# Reset database (DESTROYS DATA)
npx prisma migrate reset

# Open database browser
npx prisma studio
```

### Production

```bash
# Apply pending migrations (NEVER use migrate dev)
npx prisma migrate deploy

# View migration status
npx prisma migrate status

# Recovery: mark as applied
npx prisma migrate resolve --applied "migration_name"

# Recovery: mark as rolled back
npx prisma migrate resolve --rolled-back "migration_name"
```

### Data Migration Scripts

Run backfill scripts **between deploys**, not during migrations:

```bash
# Via ts-node
npx ts-node scripts/backfill-display-name.ts

# Via production environment
NODE_ENV=production npx ts-node scripts/backfill-display-name.ts
```

## Timing Guidelines

| Phase | When to Proceed |
|-------|-----------------|
| Expand → Migrate | After deploy is stable, monitoring shows no errors |
| Migrate → Contract | 1-2 weeks after all instances use new structure |

**Why wait before contract?**
- Allows time to catch edge cases
- Enables rollback without data restore
- Gives time for any cached/queued operations to complete

## Verification Queries

### Check for Null Values Before Making Required

```sql
SELECT COUNT(*) FROM "User" WHERE "displayName" IS NULL;
```

### Verify Backfill Completion

```sql
SELECT
  COUNT(*) as total,
  COUNT("displayName") as backfilled,
  COUNT(*) - COUNT("displayName") as remaining
FROM "User";
```

### Check for Orphaned Data

```sql
-- Before removing old column
SELECT * FROM "User" WHERE "userName" != "displayName";
```
