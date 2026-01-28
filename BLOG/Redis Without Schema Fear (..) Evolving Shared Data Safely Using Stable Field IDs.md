Redis is often used as a **shared cache** across multiple services.  
And sooner or later, every team using Redis at scale hits the same wall:

> “We changed the structure of cached data… and now everything is broken.”

This post proposes a **simple, Redis-native approach** to make shared cached data _schema-evolvable_, _backward-compatible_, and _easy to test_ — without introducing heavy gateways, HTTP hops, or forced redeploys.

---

## The problem with shared Redis data

In many systems, Redis is used to store shared entities such as:

- user profiles
- permissions
- session-related data
- organization settings
- 
A common pattern looks like this:

`{   "firstname": "Simone",   "lastname": "Panebianco" }`

This works… until it doesn’t.

### Typical failure modes

- Multiple services read the same Redis data
- One service changes the structure:
    - renames a field
        
    - moves it into a nested object
        
- Other services break
    
- Fix requires:
    
    - cache invalidation
        
    - coordinated redeploys
        
    - or risky migration scripts
        

In practice, Redis schemas become **frozen by fear**.

---

## The root cause

Redis has **no schema**, but our services _implicitly_ rely on one.

We accidentally treat:

- field names
    
- nesting
    
- structure
    

as if they were a **contract**.

They shouldn’t be.

---

## A key idea: separate meaning from structure

Instead of identifying fields by _name_, identify them by **stable semantic IDs**.

Names can change.  
Structure can change.  
Meaning should not.

This idea already exists in many systems:

- Protobuf → field numbers
    
- Avro → field aliases
    
- Databases → column OIDs
    

The insight here is:  
**we can apply the same principle directly to Redis**.

---

## The proposal: stable UUIDs per field (read-only)

### Core rules

1. **Each logical field has a stable UUID**
    
2. UUIDs never change
    
3. Field names and structure may change
    
4. This mechanism is **read-only**
    
5. Writes always use the latest schema
    

---

## Example: original schema

`{   "firstname": {     "type": "string",     "uuid": "33fff247-b414-4250-8e4a-95cc54dbde35"   },   "lastname": {     "type": "string",     "uuid": "a6abd7dc-ef0a-435a-8903-b39216c8c4c2"   } }`

A service consuming this data knows:

`{   "firstname": "33fff247-b414-4250-8e4a-95cc54dbde35",   "lastname": "a6abd7dc-ef0a-435a-8903-b39216c8c4c2" }`

It does **not** depend on Redis field names.

---

## Schema evolution without breaking readers

Later, the Redis structure evolves:

`{   "profile": {     "first_name": {       "type": "string",       "uuid": "33fff247-b414-4250-8e4a-95cc54dbde35"     }   },   "profile": {     "last_name": {       "type": "string",       "uuid": "a6abd7dc-ef0a-435a-8903-b39216c8c4c2"     }   } }`

Nothing breaks.

Services still retrieve data using UUIDs and map them to their **local expected schema**.

---

## How services read data

Each service:

1. Ships with a local schema definition (UUID → logical field)
    
2. Reads Redis data
    
3. Matches fields by UUID
    
4. Returns data in the shape the service expects
    

This mapping happens **in memory**, with no extra network calls.

---

## Why this works well with Redis

Redis is:

- schema-less
    
- mutable
    
- fast
    
- shared across services
    

Which makes schema drift **inevitable**.

This approach:

- accepts drift
    
- makes it safe
    
- removes the need for mass invalidation
    

Old and new data can coexist in the same Redis database indefinitely.

---

## Testing becomes dramatically easier

This is an underrated benefit.

Because all schema logic lives in a **single read library**, you can:

- test schema evolution offline
    
- feed old data + new schema
    
- verify correct mapping in CI
    

No staging environment.  
No production testing.  
No cache flushing.

Schema changes become **unit-testable artifacts**, not deployment gambles.

---

## Cache invalidation becomes optional

Traditionally, schema changes require:

- flushing Redis
    
- migrating data
    
- accepting cold-cache penalties
    

With UUID-based reads:

- old data remains readable
    
- new data is written in the new structure
    
- readers adapt automatically
    

This removes one of the most painful operational problems in Redis-heavy systems.

---

## What this does _not_ solve (by design)

This approach intentionally avoids complexity.

It does **not** handle:

- semantic changes
    
- field splits / merges
    
- type changes
    
- computed fields
    

Those **should** require redeploys.

UUIDs represent _identity_, not _meaning_.

---

## Practical constraints (important)

To keep the system safe:

- UUIDs must never be reused
    
- Type changes require new UUIDs
    
- Deleted fields stay deleted forever
    
- UUIDs belong to **leaf fields**, not containers
    

These rules are simple — and enforceable.

---

## When this approach makes sense

This pattern shines when:

- Redis is shared across multiple services
    
- Data schemas evolve over time
    
- Coordinated redeploys are expensive
    
- Cache invalidation hurts
    

It is especially useful for:

- user caches
    
- auth-related data
    
- permissions
    
- organization metadata
    

---

## Final thoughts

This approach does not turn Redis into a database.  
It does not add heavy infrastructure.  
It does not introduce HTTP gateways.

It simply adds one powerful idea:

> **Field names are not contracts.  
> Meaning is.**

By making meaning explicit and stable, Redis becomes safer, easier to evolve, and far more pleasant to operate at scale.