# CQRS + ES

!SLIDE

## Command Query Responsibility Segregation
## Event Sourcing

!SLIDE


!SLIDE

# Some sample code

``` ruby
def method
  puts "Hello, World"
end
```

!SLIDE

# With a Background Image

}}} images/test.png

!NOTES

1) CQRS
- well known is some domains
        - data warehouses
        - HTTP protocol
- classic application (N - tier)
        - same model used for writing and quering
        - complex logic (reading needs differnt logic than writing, e.g. validation vs. filtering, BL vs. PL)
        - list of invoices example (no need to create full Invoice aggregate root for each invoice on list)
- CQRS in single application
        - no need to creating full domain model for displaing simple list
        - presentation objects consume simple DTOs (not complicated domain objects), sometimes it is not possible - we must use domain object for e.g. calculations (normalized db)
- separated database for writing and reading (denormalized)
        - scalability (Tedi wstawka)
        - database engind and data scheme optimal for its purpose
        - example implementation using AR triggers
- pros
        - separated models are simpler than one for both reading and writing
        - performance and scalability
- cons
        - data synchronization and propagation lags
        - not for operations that need both to modify and read data (hard to implement locking)
        - data duplication (two stored + denormalization)
        - more complicated infrastracutre
        - need to maint dual models
- eventual consistency but partition tolerance (CAP theorem)

2) ES
- what it is
        - often connected with CQRS but they are two completely different things (however they fits)
        - way of storing domain date in persistent store
        - attacks same problem as ORM, AR, TS, etc. (persistance)
        - similar to ORM - domain logic is fully separated from persistance concerns
        - contrary to ORM no mapping between domain and store are needed (image)
        - events used both for storing and domain objects (ARs) intercommunication (what is AR)
        - well known is some domains (DBs, filesystems)
        - how it cooperates with CQRS
        - Event Store (write DB)
        - events are simple DTO
- ES example image
        command -> c. handler -> AR -> events --> event bus --> AR
                                                            |-> Event store
- pros
        - keep models consistent
        - models loosely coupled
        - keep track of all changes
        - audit log (e.g. for accounting systems)
        - possibility to replay events thus reconstucting state of domain and read database read database (materialized view, aggregated events)
        - keep track of all chamges in domain
        - easy to get state in any point in time (ease analysis and debugging, reporting)
        - events are immutable and append only
- cons
        - need to manage events versioning and migrations
        - hard to make manual changes in database (not good practice)
        - hard to introduce in legacy app
        - lack of proven tools (contrary to ORM)
        - complicated and not 'stable' (many interpretation + naming chaos)
        - performance (rebuilding domain may be time consuming), snapshots help
        - hard to query (CQRS and read database solved this problem)

3) Sources
http://msdn.microsoft.com/en-us/library/jj554200.aspx (CQRS Journey)
http://martinfowler.com/bliki/CQRS.html (Martin Fowler on CQRS)
http://www.jayway.com/2013/03/08/aggregates-event-sourcing-distilled/ (Minimalistic ES example)
http://www.slideshare.net/cavalle/the-cqrs-diet (CQRS Diet)
