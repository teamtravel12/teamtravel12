# graindb - immutable steel database for redox os + kubernetes

**title**: "distributed immutable database system inspired by datomic and datascript, implemented in steel for redox os containerization"  
**copyright © 3x39** | https://github.com/3x39  
**authors**: kae3g (kj3x39, @risc.love) + teamtreasure02  
**date**: october 28, 2025  
**grainorder**: zxbdgb  
**assigned to**: teamtreasure02 (steel database team)

---

## abstract

hey there! let me tell you about something we're building - a database that *never forgets*.

you know how traditional databases let you update and delete records? what if instead, every change was preserved forever, and you could travel back in time to see any previous state? that's what **datomic** and **datascript** do in clojure-land!

now we're bringing that same immutable, time-traveling philosophy to **steel** (rust-hosted lisp), designed specifically for:
- **redox os**: the rust-based operating system
- **kubernetes**: distributed container orchestration
- **grainorder**: using our unique 6-character codes as entity ids
- **pure steel**: all queries and transactions in lisp!

**think of it like git for your data**: every change is a commit, you can branch/merge, and time-travel is built-in! 🌾⚡

---

## background (why we need this)

### the problem with traditional databases

let's start with what's wrong with sql/nosql databases...

**mysql, postgres, mongodb**:
```sql
-- traditional update: old value is LOST forever!
UPDATE users SET email = 'new@example.com' WHERE id = 123;
-- ❌ previous email is gone! can't time-travel!

-- traditional delete: data VANISHES!
DELETE FROM users WHERE id = 123;
-- ❌ can't audit who deleted it or when!
```

**problems**:
1. **no history**: old values disappear
2. **no audit trail**: can't see who changed what when
3. **no time-travel**: can't query "what did the database look like yesterday?"
4. **hard to debug**: "why did this value change?" - no answer!
5. **complex caching**: have to manually invalidate caches

**question**: have you ever wished you could undo a database change? or see the entire history of a record? that's what graindb gives you! 🕰️

### what datomic/datascript do (the inspiration)

**datomic** (by rich hickey, creator of clojure):
- **immutable facts**: every piece of data is an immutable fact
- **temporal queries**: query any point in history
- **datalog**: logical query language (like prolog)
- **eavt model**: entity-attribute-value-time tuples
- **acid transactions**: atomic, consistent, isolated, durable

**datascript** (in-memory clojure):
- same philosophy as datomic
- but runs in-browser (clojurescript)
- perfect for client-side apps
- immutable data structures

**example** (clojure/datomic):
```clojure
;; add a fact
(d/transact conn [{:db/id -1
                   :user/name "alice"
                   :user/email "alice@example.com"}])

;; later, "update" (actually: add new fact!)
(d/transact conn [{:db/id 123
                   :user/email "alice@newdomain.com"}])

;; time-travel query: what was alice's email yesterday?
(d/q '[:find ?email
       :in $ ?name
       :where [?e :user/name ?name]
              [?e :user/email ?email]]
     (d/as-of db yesterday)
     "alice")
;; => "alice@example.com"

;; query now:
(d/q '[:find ?email ...]  ; same query
     db-now
     "alice")
;; => "alice@newdomain.com"
```

**see the magic?** both values exist! you can query any point in time! 🕰️✨

### what makes graindb different (our novel contribution)

graindb takes datomic/datascript and:

1. **uses steel instead of clojure**:
   - rust ffi for performance
   - embeddable in any rust app
   - perfect for redox os (rust operating system!)

2. **uses grainorder for entity ids**:
   - instead of auto-increment integers: `123, 124, 125...`
   - we use grainorder codes: `xbdghj, xbdghk, xbdghl...`
   - benefits: bounded (1.2m max), pronounceable, sortable

3. **designed for kubernetes**:
   - distributed from day one
   - each pod can have local graindb
   - sync via event log (like kafka)
   - eventual consistency built-in

4. **redox os compatible**:
   - pure rust implementation (steel is rust!)
   - no system calls that redox doesn't support
   - microkernel-friendly architecture
   - capability-based security model

5. **steel-native queries**:
   - write queries in lisp, not sql!
   - datalog-inspired but scheme syntax
   - composable, first-class functions

**question**: does it make sense why this is powerful? immutability + time-travel + rust + kubernetes = modern distributed database! 🌾

---

## how it works (detailed architecture)

### core concept: eavt tuples (the foundation)

everything in graindb is an **eavt tuple**:
- **e**: entity id (grainorder code)
- **a**: attribute (keyword like `:user/name`)
- **v**: value (any steel value)
- **t**: transaction id (when it was added)

**example eavt tuples**:
```
[xbdghj :user/name "alice" 1000]
[xbdghj :user/email "alice@example.com" 1000]
[xbdghj :user/age 30 1000]
[xbdghj :user/email "alice@newdomain.com" 2000]  ← new fact! old one still exists
```

**see how it works?** the email "update" is actually a *new fact* at time `2000`. the old fact at time `1000` is still there!

**steel representation**:
```steel
;; an eavt tuple is just a vector
(define fact1 (vector 'xbdghj ':user/name "alice" 1000))
(define fact2 (vector 'xbdghj ':user/email "alice@example.com" 1000))

;; or as a struct for performance
(struct eavt (e a v t) #:transparent)
(define fact3 (eavt 'xbdghj ':user/age 30 1000))
```

### the four indexes (how we query fast!)

to make queries fast, graindb maintains **four sorted indexes**:

1. **eavt**: entity → attribute → value → time
   - use: "get all attributes for entity xbdghj"
   - example: `(get-entity 'xbdghj)` → returns all facts about xbdghj

2. **aevt**: attribute → entity → value → time
   - use: "find all entities with attribute :user/name"
   - example: `(find-all-users)` → returns all entities with :user/name

3. **avet**: attribute → value → entity → time
   - use: "find entities where :user/name = 'alice'"
   - example: `(find-user-by-name "alice")` → fast lookup!

4. **vaet**: value → attribute → entity → time (for references!)
   - use: "find all entities that reference xbdghj"
   - example: `(find-references 'xbdghj)` → backlinks!

**why four indexes?** each one makes a different query pattern fast! it's like having four different "views" of the same data.

**steel implementation** (in-memory sorted btree):

```steel
;; graindb core data structure
(struct graindb
  (eavt-index    ; btree (entity → attribute → value → time)
   aevt-index    ; btree (attribute → entity → value → time)
   avet-index    ; btree (attribute → value → entity → time)
   vaet-index    ; btree (value → attribute → entity → time)
   max-tx        ; current transaction id
   schema)       ; attribute definitions
  #:transparent)

;; create new empty database
(define (make-graindb)
  (graindb (make-btree)    ; eavt
           (make-btree)    ; aevt
           (make-btree)    ; avet
           (make-btree)    ; vaet
           0               ; initial tx
           (make-hash)))   ; schema

;; ⬆️ ASCENDING INDEX SCAN (oldest → newest facts)
;; use for: historical analysis, audit trails, replay
(define (index-scan-ascending index start-key)
  ;; returns iterator that yields facts in chronological order
  (let ([iter (btree-scan index start-key)])
    (iterator-map (lambda (entry)
                    (btree-entry-value entry))
                  iter)))

;; ⬇️ DESCENDING INDEX SCAN (newest → oldest facts)
;; use for: current state queries, "what's the latest value?"
(define (index-scan-descending index start-key)
  ;; returns iterator that yields facts in reverse chronological order
  (let ([iter (btree-scan-reverse index start-key)])
    (iterator-map (lambda (entry)
                    (btree-entry-value entry))
                  iter)))

;; example: get entity's current state (newest facts first!)
(define (get-entity-current db entity-id)
  (let* ([eavt (graindb-eavt-index db)]
         [prefix (list entity-id)]
         [facts (index-scan-descending eavt prefix)])
    ;; take first fact for each attribute (newest!)
    (dedupe-by-attribute facts)))

;; example: get entity's full history (oldest facts first!)
(define (get-entity-history db entity-id)
  (let* ([eavt (graindb-eavt-index db)]
         [prefix (list entity-id)]
         [facts (index-scan-ascending eavt prefix)])
    ;; return ALL facts in chronological order
    (iterator->list facts)))
```

**question**: see how ascending vs descending changes what we're querying? ascending = "tell me the story" (history), descending = "what's happening now?" (current state)! 🕰️

### transactions (adding data atomically)

in graindb, you don't "update" data - you **transact new facts**!

**steel api**:
```steel
;; transact! adds new facts to the database
;; input: list of fact maps
;; output: new database value + transaction report
(define (transact! db tx-data)
  ;; 1. allocate new transaction id
  (let* ([new-tx-id (+ (graindb-max-tx db) 1)]
         
         ;; 2. expand tx-data into eavt tuples
         [eavts (expand-tx-data tx-data new-tx-id)]
         
         ;; 3. add to all four indexes
         [new-db (fold-left add-fact-to-indexes db eavts)]
         
         ;; 4. update max-tx
         [final-db (struct-copy graindb new-db
                                [max-tx new-tx-id])])
    
    ;; return new db + report
    (values final-db
            (make-tx-report new-tx-id eavts))))

;; add a single eavt tuple to all indexes
(define (add-fact-to-indexes db eavt)
  (let* ([e (eavt-e eavt)]
         [a (eavt-a eavt)]
         [v (eavt-v eavt)]
         [t (eavt-t eavt)])
    (struct-copy graindb db
      ;; add to eavt index: [e a v t]
      [eavt-index (btree-insert (graindb-eavt-index db)
                                (list e a v t)
                                eavt)]
      
      ;; add to aevt index: [a e v t]
      [aevt-index (btree-insert (graindb-aevt-index db)
                                (list a e v t)
                                eavt)]
      
      ;; add to avet index: [a v e t]
      [avet-index (btree-insert (graindb-avet-index db)
                                (list a v e t)
                                eavt)]
      
      ;; add to vaet index (if v is a ref): [v a e t]
      [vaet-index (if (reference? db a v)
                      (btree-insert (graindb-vaet-index db)
                                    (list v a e t)
                                    eavt)
                      (graindb-vaet-index db))])))

;; example usage:
(define db0 (make-graindb))

;; transaction 1: add alice
(define-values (db1 report1)
  (transact! db0
    '({:db/id xbdghj
       :user/name "alice"
       :user/email "alice@example.com"
       :user/age 30})))

;; transaction 2: "update" alice's email (actually: add new fact!)
(define-values (db2 report2)
  (transact! db1
    '({:db/id xbdghj
       :user/email "alice@newdomain.com"})))

;; db0, db1, db2 are all immutable values!
;; you can keep them all around and query any of them!
```

**the magic**: `db0`, `db1`, `db2` are all **persistent data structures**! changing one doesn't affect the others. structural sharing makes this efficient!

**question**: does it feel like git? each transaction is like a commit - you can keep all versions! 🌾

### queries (datalog in steel!)

graindb uses a **datalog-inspired query language** written in steel:

**query structure**:
```steel
(q db
   '[:find ?name ?email           ; what to return
     :where [?e :user/name ?name] ; pattern matching
            [?e :user/email ?email]])
```

**query engine implementation**:
```steel
;; simplified query engine
(define (q db query)
  (match query
    [`(:find ,@find-vars
       :where ,@where-clauses)
     
     ;; 1. convert where clauses to index lookups
     (let* ([clauses (map (lambda (clause)
                            (compile-clause db clause))
                          where-clauses)]
            
            ;; 2. find optimal join order (most selective first)
            [ordered-clauses (optimize-join-order clauses)]
            
            ;; 3. execute joins
            [bindings (execute-joins db ordered-clauses)]
            
            ;; 4. project find-vars
            [results (map (lambda (binding)
                            (map (lambda (var)
                                   (hash-ref binding var))
                                 find-vars))
                          bindings)])
       
       ;; return result set
       results)]))

;; ⬆️ ASCENDING QUERY (historical data, oldest first)
;; use for: audit logs, timeline views, data archaeology
(define (q-ascending db query as-of-tx)
  ;; query database as it existed at tx `as-of-tx`
  ;; return results sorted chronologically (oldest first)
  (let* ([filtered-db (filter-db-before-tx db as-of-tx)]
         [raw-results (q filtered-db query)]
         [with-timestamps (annotate-with-tx-times raw-results)])
    ;; sort by transaction time ascending
    (sort with-timestamps
          (lambda (r1 r2)
            (< (result-tx r1) (result-tx r2))))))

;; ⬇️ DESCENDING QUERY (current data, newest first)
;; use for: dashboard views, "latest updates", current state
(define (q-descending db query)
  ;; query current database state
  ;; return results with newest facts first
  (let* ([raw-results (q db query)]
         [with-timestamps (annotate-with-tx-times raw-results)])
    ;; sort by transaction time descending
    (sort with-timestamps
          (lambda (r1 r2)
            (> (result-tx r1) (result-tx r2))))))

;; example queries:

;; 1. find all users (current state, newest first)
(q-descending db
  '[:find ?name ?email
    :where [?e :user/name ?name]
           [?e :user/email ?email]])
;; => (("zara" "zara@example.com")    ; most recent user
;;     ("bob" "bob@example.com")
;;     ("alice" "alice@newdomain.com"))

;; 2. find user by name (current state)
(q db
  '[:find ?email
    :where [?e :user/name "alice"]
           [?e :user/email ?email]])
;; => (("alice@newdomain.com"))  ; latest email

;; 3. find user by name (as of transaction 1000)
(q-ascending db
  '[:find ?email
    :where [?e :user/name "alice"]
           [?e :user/email ?email]]
  1000)  ; time-travel!
;; => (("alice@example.com"))  ; old email!

;; 4. find all email changes for alice (history, oldest first)
(q-ascending db
  '[:find ?email ?tx
    :where [?e :user/name "alice"]
           [?e :user/email ?email ?tx]])
;; => (("alice@example.com" 1000)      ; first email
;;     ("alice@newdomain.com" 2000))   ; second email

;; 5. join query: find users and their posts (newest first)
(q-descending db
  '[:find ?user-name ?post-title
    :where [?user :user/name ?user-name]
           [?post :post/author ?user]
           [?post :post/title ?post-title]])
```

**powerful features**:
- **time-travel**: query any past state with `as-of-tx`
- **joins**: multiple where clauses automatically join
- **logic variables**: `?e`, `?name`, etc. unify across clauses
- **ascending/descending**: control sort order for different use cases!

**question**: can you see how this is more powerful than sql? you get time-travel for free, and queries are just data (can be built programmatically)! 🕰️✨

### schema (defining attributes)

graindb lets you define a **schema** for attributes:

```steel
;; define attribute schema
(define user-schema
  '{:user/name
    {:db/valueType :db.type/string
     :db/cardinality :db.cardinality/one
     :db/doc "user's display name"}
    
    :user/email
    {:db/valueType :db.type/string
     :db/cardinality :db.cardinality/one
     :db/unique :db.unique/identity  ; unique constraint!
     :db/doc "user's email address"}
    
    :user/tags
    {:db/valueType :db.type/keyword
     :db/cardinality :db.cardinality/many  ; multi-valued!
     :db/doc "user's interest tags"}
    
    :user/friends
    {:db/valueType :db.type/ref  ; reference to another entity!
     :db/cardinality :db.cardinality/many
     :db/doc "user's friends (entity refs)"}})

;; install schema
(define db-with-schema
  (install-schema db0 user-schema))
```

**schema features**:
- **value types**: string, number, keyword, ref, etc.
- **cardinality**: one (single value) or many (set of values)
- **unique constraints**: `:db.unique/identity` or `:db.unique/value`
- **references**: `:db.type/ref` for entity-to-entity relationships
- **documentation**: `:db/doc` explains each attribute

---

## time-travel queries (the killer feature!)

### as-of queries (point-in-time)

**query the database as it existed at any transaction**:

```steel
;; database now
(q db-current
  '[:find ?email
    :where [?e :user/name "alice"]
           [?e :user/email ?email]])
;; => (("alice@newdomain.com"))

;; database yesterday (transaction 1000)
(q (as-of db-current 1000)
  '[:find ?email
    :where [?e :user/name "alice"]
           [?e :user/email ?email]])
;; => (("alice@example.com"))

;; implementation:
(define (as-of db tx-id)
  ;; filter database to only include facts where t <= tx-id
  (filter-db-by-predicate db
    (lambda (eavt)
      (<= (eavt-t eavt) tx-id))))
```

### since queries (what changed?)

**query all changes since a transaction**:

```steel
;; what changed since transaction 1000?
(q (since db-current 1000)
  '[:find ?e ?a ?v ?tx
    :where [?e ?a ?v ?tx]])
;; => ((xbdghj :user/email "alice@newdomain.com" 2000)
;;     (xbdghk :user/name "bob" 2001)
;;     ...)

;; implementation:
(define (since db tx-id)
  ;; filter database to only include facts where t > tx-id
  (filter-db-by-predicate db
    (lambda (eavt)
      (> (eavt-t eavt) tx-id))))
```

### history queries (complete audit trail)

**get complete history of an entity**:

```steel
;; all facts ever asserted about alice (ascending order!)
(q-ascending (history db-current)
  '[:find ?a ?v ?tx
    :where [?e :user/name "alice"]
           [?e ?a ?v ?tx]])
;; => ((:user/name "alice" 1000)
;;     (:user/email "alice@example.com" 1000)
;;     (:user/age 30 1000)
;;     (:user/email "alice@newdomain.com" 2000)  ; see the change!
;;     (:user/age 31 3000))                      ; birthday!

;; implementation:
(define (history db)
  ;; return database view that includes ALL facts, not just latest
  ;; (normal queries only show latest fact per [e a])
  (struct-copy graindb db
    [filter-mode 'history]))  ; special mode
```

**question**: can you imagine using this for audit compliance? "show me every change to this user's data" - instant answer! 📜

---

## distributed architecture (kubernetes + redox os)

### deployment model

**each kubernetes pod runs its own graindb instance**:

```
┌──────────────────────────────────────────────────────────────┐
│                        kubernetes cluster                     │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌───────────┐   ┌───────────┐   ┌───────────┐             │
│  │  Pod 1    │   │  Pod 2    │   │  Pod 3    │             │
│  │           │   │           │   │           │             │
│  │ graindb   │   │ graindb   │   │ graindb   │             │
│  │ (local)   │   │ (local)   │   │ (local)   │             │
│  │           │   │           │   │           │             │
│  │ redox os  │   │ redox os  │   │ redox os  │             │
│  └─────┬─────┘   └─────┬─────┘   └─────┬─────┘             │
│        │               │               │                   │
│        └───────────────┼───────────────┘                   │
│                        │                                   │
│                  ┌─────▼──────┐                            │
│                  │ event log  │                            │
│                  │ (kafka)    │                            │
│                  └────────────┘                            │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

**how it works**:
1. each pod has **local graindb** (fast reads!)
2. writes go to **event log** (kafka/redpanda)
3. all pods **replay events** to stay in sync
4. **eventual consistency**: all pods converge to same state

### steel implementation (event sourcing):

```steel
;; event log entry
(struct db-event
  (tx-id      ; transaction id
   tx-data    ; list of eavt tuples
   timestamp  ; wall-clock time
   source-pod) ; which pod originated this
  #:transparent)

;; publish transaction to event log
(define (publish-transaction! event-log db-event)
  ;; send to kafka topic "graindb-events"
  (kafka-produce event-log
                 "graindb-events"
                 (serialize-event db-event)))

;; subscribe to event log and apply transactions
(define (subscribe-and-replay! db event-log)
  ;; consume from kafka
  (kafka-consume event-log
                 "graindb-events"
                 (lambda (event-bytes)
                   (let* ([event (deserialize-event event-bytes)]
                          [tx-data (db-event-tx-data event)])
                     ;; apply transaction locally
                     (set! db (apply-transaction db tx-data))))))

;; ⬆️ ASCENDING REPLAY (rebuild from beginning)
;; use for: initializing new pods, disaster recovery
(define (replay-ascending event-log from-offset)
  ;; start with empty database
  (let ([db (make-graindb)])
    ;; replay events in chronological order
    (kafka-consume-from event-log
                        "graindb-events"
                        from-offset
                        (lambda (event)
                          (set! db (apply-event db event))))
    db))

;; ⬇️ DESCENDING QUERY (latest state first)
;; use for: catching up quickly, showing recent changes
(define (get-recent-events event-log n)
  ;; get last n events (newest first)
  (kafka-consume-latest event-log
                        "graindb-events"
                        n
                        'descending))

;; example: initialize new pod
(define new-pod-db
  (replay-ascending event-log 0))  ; replay from beginning

;; example: show recent activity
(define recent-changes
  (get-recent-events event-log 100))  ; last 100 events
```

### redox os compatibility

**why redox os?** it's a **rust operating system** with:
- **microkernel**: minimal, secure kernel
- **capability-based security**: processes have explicit permissions
- **rust all the way**: from kernel to userspace

**graindb on redox**:
```steel
;; graindb uses only redox-compatible syscalls
(define (persist-to-disk db path)
  ;; use redox file:// scheme
  (let ([fd (open (string-append "file://" path) 'write)])
    (write-bytes fd (serialize-db db))
    (close fd)))

;; memory-mapped storage (zero-copy!)
(define (mmap-graindb path)
  (let* ([fd (open path 'read)]
         [size (file-size fd)]
         [mmap (mmap-file fd 0 size)])
    ;; deserialize directly from mmap'd memory
    (deserialize-db-from-bytes mmap)))
```

**benefits**:
- **security**: capability-based access control
- **reliability**: microkernel is harder to crash
- **rust ecosystem**: seamlessly integrates with graindb (also rust!)

**question**: can you see how this all fits together? local databases + event log + redox os = distributed, secure, rust-native storage! 🦀🌾

---

## grainorder integration (entity ids)

### why grainorder for entity ids?

instead of auto-increment integers (`1, 2, 3...`) or uuids (`550e8400-e29b-...`), graindb uses **grainorder codes**!

**benefits**:
- **bounded**: exactly 1,235,520 possible entities (known limit!)
- **pronounceable**: "x-b-d-g-h-j" can be spoken
- **sortable**: alphabetical order = creation order
- **no vowels**: prevents accidental words/profanity
- **distributed-friendly**: no central counter needed!

**steel implementation**:
```steel
;; entity id allocator using grainorder
(struct graindb-with-allocator
  (db              ; underlying graindb
   next-grainorder ; next available entity id
   used-ids)       ; set of allocated ids
  #:transparent)

;; allocate new entity id
(define (allocate-entity-id! allocator)
  (let* ([current (graindb-with-allocator-next-grainorder allocator)]
         [next (next-grainorder current)])  ; from grainorder module
    (if (not next)
        (error "grainorder exhausted! reached 1,235,520 entities")
        (begin
          (set-graindb-with-allocator-next-grainorder! allocator next)
          (set-insert! (graindb-with-allocator-used-ids allocator) current)
          current))))

;; create entity with auto-allocated id
(define (create-entity! allocator attributes)
  (let* ([entity-id (allocate-entity-id! allocator)]
         [db (graindb-with-allocator-db allocator)]
         [tx-data (cons {:db/id entity-id}
                        attributes)])
    (transact! db (list tx-data))))

;; example usage:
(define allocator
  (make-graindb-with-allocator
    (make-graindb)
    'xbdghj  ; start from first grainorder
    (make-hash-set)))

;; create entities
(define-values (db1 alice-id)
  (create-entity! allocator
    {:user/name "alice"
     :user/email "alice@example.com"}))
;; alice-id => 'xbdghj

(define-values (db2 bob-id)
  (create-entity! allocator
    {:user/name "bob"
     :user/email "bob@example.com"}))
;; bob-id => 'xbdghk

;; ⬆️ ASCENDING ENTITY SCAN (oldest entities first)
;; use for: batch processing, exports, migrations
(define (scan-all-entities-ascending db)
  ;; iterate through eavt index in grainorder sequence
  ;; xbdghj, xbdghk, xbdghl, ... (creation order!)
  (let ([eavt (graindb-eavt-index db)])
    (btree-scan eavt '())))  ; scan from beginning

;; ⬇️ DESCENDING ENTITY SCAN (newest entities first)
;; use for: "recently added users", dashboards
(define (scan-recent-entities-descending db n)
  ;; iterate through eavt index in reverse grainorder
  ;; ..., xbdghl, xbdghk, xbdghj (newest first!)
  (let ([eavt (graindb-eavt-index db)])
    (take n (btree-scan-reverse eavt '()))))

;; example: show 10 most recent users
(define recent-users
  (scan-recent-entities-descending db2 10))
;; => entities with ids: xbdghl, xbdghk, xbdghj, ... (newest first!)
```

---

## comparison with other databases

### graindb vs postgres

| feature | postgres | graindb |
|---------|----------|---------|
| mutability | ✅ mutable (update/delete) | ❌ immutable (append-only) |
| time-travel | ❌ no (unless using triggers) | ✅ built-in (as-of, since, history) |
| query language | sql | datalog (steel) |
| transactions | acid | acid |
| distributed | ❌ complex (needs pgpool/citus) | ✅ native (event sourcing) |
| language | c | steel (rust) |
| entity ids | auto-increment / uuid | grainorder |

**when to use postgres**: need mutable data, existing sql ecosystem  
**when to use graindb**: need audit trail, time-travel, immutability

### graindb vs datomic

| feature | datomic | graindb |
|---------|---------|---------|
| language | clojure | steel (scheme) |
| hosting | jvm | rust (standalone) |
| license | proprietary | open source (tbd) |
| backend | storage service | local + event log |
| query | datalog | datalog (steel) |
| time-travel | ✅ yes | ✅ yes |
| kubernetes | ❌ not designed for | ✅ designed for |
| redox os | ❌ no (jvm) | ✅ yes (rust) |

**when to use datomic**: jvm ecosystem, need commercial support  
**when to use graindb**: rust/steel stack, kubernetes, redox os

### graindb vs datascript

| feature | datascript | graindb |
|---------|------------|---------|
| target | browser (clojurescript) | server (rust) |
| persistence | localstorage | disk / event log |
| distributed | ❌ single-machine | ✅ multi-pod |
| query | datalog | datalog (steel) |
| performance | good (in-memory) | excellent (rust + btree) |

**when to use datascript**: browser apps, clojurescript  
**when to use graindb**: server apps, distributed systems, rust

---

## implementation roadmap

### phase 1: core database (foundation)

**goal**: in-memory graindb with basic queries

```steel
;; milestone 1.1: eavt data structure
(struct eavt (e a v t) #:transparent)
(define (make-graindb) ...)

;; milestone 1.2: four indexes (btree)
(define (add-to-indexes db eavt) ...)

;; milestone 1.3: transactions
(define (transact! db tx-data) ...)

;; milestone 1.4: basic queries
(define (q db query) ...)
```

**deliverable**: `graindb-core.scm` with tests

### phase 2: time-travel (killer feature)

**goal**: as-of, since, history queries

```steel
;; milestone 2.1: as-of
(define (as-of db tx-id) ...)

;; milestone 2.2: since
(define (since db tx-id) ...)

;; milestone 2.3: history
(define (history db) ...)
```

**deliverable**: time-travel query engine

### phase 3: persistence (durability)

**goal**: save/load database from disk

```steel
;; milestone 3.1: serialization
(define (serialize-db db path) ...)
(define (deserialize-db path) ...)

;; milestone 3.2: write-ahead log (wal)
(define (append-to-wal! wal tx-data) ...)

;; milestone 3.3: recovery
(define (recover-from-wal wal-path) ...)
```

**deliverable**: persistent graindb

### phase 4: distributed (kubernetes)

**goal**: multi-pod deployment with event sourcing

```steel
;; milestone 4.1: event log integration
(define (publish-transaction! event-log event) ...)
(define (subscribe-and-replay! db event-log) ...)

;; milestone 4.2: kubernetes deployment
;; - helm chart
;; - configmap for schema
;; - statefulset for pods

;; milestone 4.3: redox os support
;; - test on redox
;; - capability-based file access
```

**deliverable**: distributed graindb on kubernetes

### phase 5: optimization (performance)

**goal**: production-ready performance

- query optimizer (join reordering)
- index statistics
- caching layer
- compression

**deliverable**: benchmarked, optimized graindb

---

## example application: user management

let's build a complete example to see it all in action!

```steel
;; ========================================
;; graindb user management example
;; ========================================

;; 1. create database with schema
(define user-db
  (let ([db (make-graindb)])
    (install-schema db
      '{:user/name
        {:db/valueType :db.type/string
         :db/cardinality :db.cardinality/one}
        :user/email
        {:db/valueType :db.type/string
         :db/cardinality :db.cardinality/one
         :db/unique :db.unique/identity}
        :user/role
        {:db/valueType :db.type/keyword
         :db/cardinality :db.cardinality/one}})))

;; 2. create allocator
(define allocator
  (make-graindb-with-allocator user-db 'xbdghj (make-hash-set)))

;; 3. add users (transactions 1000-1002)
(define-values (db1 alice-id)
  (create-entity! allocator
    {:user/name "alice"
     :user/email "alice@example.com"
     :user/role :admin}))

(define-values (db2 bob-id)
  (create-entity! allocator
    {:user/name "bob"
     :user/email "bob@example.com"
     :user/role :user}))

(define-values (db3 charlie-id)
  (create-entity! allocator
    {:user/name "charlie"
     :user/email "charlie@example.com"
     :user/role :user}))

;; 4. "update" alice's role (transaction 1003)
(define-values (db4 report4)
  (transact! db3
    (list {:db/id alice-id
           :user/role :super-admin})))

;; 5. queries!

;; 5a. find all users (current state, descending = newest first)
(q-descending db4
  '[:find ?name ?role
    :where [?e :user/name ?name]
           [?e :user/role ?role]])
;; => (("charlie" :user)         ; newest
;;     ("bob" :user)
;;     ("alice" :super-admin))   ; shows updated role!

;; 5b. find alice's email (current)
(q db4
  '[:find ?email
    :where [?e :user/name "alice"]
           [?e :user/email ?email]])
;; => (("alice@example.com"))

;; 5c. what was alice's role before the update? (time-travel!)
(q (as-of db4 1002)  ; before transaction 1003
  '[:find ?role
    :where [?e :user/name "alice"]
           [?e :user/role ?role]])
;; => ((:admin))  ; old value!

;; 5d. audit trail: all of alice's role changes (ascending = oldest first)
(q-ascending (history db4)
  '[:find ?role ?tx
    :where [?e :user/name "alice"]
           [?e :user/role ?role ?tx]])
;; => ((:admin 1000)        ; first role
;;     (:super-admin 1003)) ; updated role

;; 5e. who changed roles since transaction 1002?
(q (since db4 1002)
  '[:find ?name ?old-role ?new-role
    :where [?e :user/name ?name]
           [?e :user/role ?old-role 1002]  ; role at tx 1002
           [?e :user/role ?new-role 1003]]) ; role at tx 1003
;; => (("alice" :admin :super-admin))

;; 6. persistence
(serialize-db db4 "/data/graindb/users.db")

;; 7. recovery
(define recovered-db
  (deserialize-db "/data/graindb/users.db"))

;; same data!
(equal? db4 recovered-db)
;; => #t
```

**question**: can you see how powerful this is? full audit trail, time-travel, immutability - all with simple steel code! 🌾✨

---

## conclusion (why this matters)

graindb brings together:
- **immutability** (from datomic/datascript)
- **rust performance** (via steel)
- **kubernetes-native** (distributed by design)
- **redox os compatible** (pure rust stack)
- **grainorder entity ids** (bounded, human-friendly)
- **time-travel queries** (audit compliance built-in)
- **steel-native** (lisp queries, composable!)

**use cases**:
- **audit compliance**: full history of every change
- **distributed systems**: kubernetes pods with local databases
- **embedded systems**: redox os with graindb
- **grain network**: store graincards with temporal awareness!
- **microservices**: each service gets its own graindb

**next steps**:
1. implement core in steel (phase 1)
2. test time-travel queries (phase 2)
3. add persistence (phase 3)
4. deploy on kubernetes (phase 4)
5. optimize and benchmark (phase 5)

---

**copyright © 3x39**  
**authors: kae3g + teamtreasure02**  
**assigned to: teamtreasure02 (steel database team)**  
**status: design specification - ready for implementation**

now == next + 1 🌾⚡🕰️🦀✨

