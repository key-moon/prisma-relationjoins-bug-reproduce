# reproduce repository for 

```
# instal
pnpm i
# setup db
docker compose up -d
pnpm prisma push
# run PoC
pnpm dotenv -e .env -- ts-node script.ts
```

## Client Error

```
PrismaClientKnownRequestError: 
Invalid `prisma.a.findFirst()` invocation in
./script.ts:6:18

  3 const prisma = new PrismaClient()
  4 
  5 async function main() {
→ 6   await prisma.a.findFirst(
The column `t3.name` does not exist in the current database.
    at Ln.handleRequestError (./node_modules/.pnpm/@prisma+client@5.19.0_prisma@5.19.0/node_modules/@prisma/client/runtime/library.js:121:7753)
    at Ln.handleAndLogRequestError (./node_modules/.pnpm/@prisma+client@5.19.0_prisma@5.19.0/node_modules/@prisma/client/runtime/library.js:121:7061)
    at Ln.request (./node_modules/.pnpm/@prisma+client@5.19.0_prisma@5.19.0/node_modules/@prisma/client/runtime/library.js:121:6745)
    at async l (./node_modules/.pnpm/@prisma+client@5.19.0_prisma@5.19.0/node_modules/@prisma/client/runtime/library.js:130:9633) {
  code: 'P2022',
  clientVersion: '5.19.0',
  meta: { modelName: 'A', column: 't3.name' }
}
```

## Database Error

```
db-1  | 2024-08-27 15:04:08.684 UTC [108] ERROR:  column t3.name does not exist at character 296
db-1  | 2024-08-27 15:04:08.684 UTC [108] STATEMENT:  SELECT "t1"."id", "A_bs"."__prisma_data__" AS "bs" FROM "public"."A" AS "t1" LEFT JOIN LATERAL (SELECT COALESCE(JSONB_AGG("__prisma_data__"), '[]') AS "__prisma_data__" FROM (SELECT "t4"."__prisma_data__" FROM (SELECT JSONB_BUILD_OBJECT('id', "t3"."id", 'aId', "t3"."aId") AS "__prisma_data__", "t3"."name" FROM (SELECT "t2".* FROM "public"."B" AS "t2" WHERE "t1"."id" = "t2"."aId" /* root select */) AS "t3" /* inner select */) AS "t4" WHERE ("t4"."id") NOT IN (SELECT "t1"."A" FROM "public"."_BToC" AS "t1" INNER JOIN "public"."C" AS "j1" ON ("j1"."id") = ("t1"."B") WHERE ((NOT "j1"."name" = $1) AND "t1"."A" IS NOT NULL)) /* middle select */) AS "t5" /* outer select */) AS "A_bs" ON true LIMIT $2
```
