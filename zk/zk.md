## ZooKeeper as a Queue and Lock Solution for Merritt

- Terry Brady
- Mark Reyes

https://github.com/CDLUC3/mrt-zk

---

## What is ZooKeeper?

TBD - need a good definition

---

## Sample Merritt Job

```
/jobs/jid0000053584: 
/jobs/jid0000053584/bid: bid0000002789
/jobs/jid0000053584/configuration:
{
  "date": "2024-11-05",
  "submitter": "eScholarship auto-submitter v1.1",
  "queuePriority": "52",
  "creator": ...
```

----

## Sample Merritt Batch

```
/batches/bid0000002793: 
/batches/bid0000002793/states: 
/batches/bid0000002793/states/batch-completed: 
/batches/bid0000002793/states/batch-completed/jid0000053980: 
/batches/bid0000002793/states/batch-completed/jid0000053981: 
/batches/bid0000002793/states/batch-processing: 
/batches/bid0000002793/status:
{
  "status": "Completed"
}
/batches/bid0000002793/submission:
{
  "submitter": "eScholarship auto-submitter v1.1",
```

---
 
## ZooKeeper: 3 Key Capabilities

- Persistent Locks
- Ephemeral Locks
- Sequential Node Creation 

---

## Persistent Locks

- Hold/Freeze Queues until released

----

## Persistent Lock Creation - Java

```java
ZooKeeper zk = new ZooKeeper("localhost:8084", 100, null)
zk.create("/my/path", "My Data", 
  ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);
```

----

## Persistent Lock Creation - Ruby

```ruby
zk = ZK.new('localhost:8084')
zk.create("/my/path", data: "My Data")
```

----

## Lock Release - Java

```java
ZooKeeper zk = new ZooKeeper("localhost:8084", 100, null)
if (exists(client, "/my/path")){
  client.delete("/my/path", -1);
}
```

----

## Lock Release - Ruby

```ruby
zk = ZK.new('localhost:8084')
zk.delete("/my/path")
```

---

## Ephemeral Locks

- Exclusive lock while processing
- Guaranteed to release 
  - on completion
  - on failure
  - on abnormal termination of process

----

## Ephemeral Lock - Java

```java
try(ZooKeeper zk = new ZooKeeper("localhost:8084", 100, null)) {
  client.create("/my/path", "My Data", 
    ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.EPHEMERAL);
}
# Lock is released when ZK client is released
```

----

## Ephemeral Lock - Ruby

```ruby
begin
  zk = ZK.new('localhost:8084')
  zk.create("/my/path", data: "My Data", mode: :ephemeral)
end
# Lock is released when ZK client is released
```

---

## Sequential Node Creation

- Priority Queue

----

## Batches

```
/batches/bid0000002789
/batches/bid0000002790
/batches/bid0000002791
```

----

## Jobs

```
/jobs/jid0000053583
/jobs/jid0000053584
/jobs/jid0000053585
```

----

## Sequential Node Creation - Java

```java
ZooKeeper zk = new ZooKeeper("localhost:8084", 100, null)
zk.create("/my/queue", "My Data", 
  ZooDefs.Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT_SEQUENTIAL);
```

----

## Sequential Node Creation - Ruby

```ruby
zk = ZK.new('localhost:8084')
zk.create("/my/path", data: "My Data", 
  mode: :persistent_sequential, ignore: :no_node)
```

---

## Merritt Queue Design

- [State Transitions](https://github.com/CDLUC3/mrt-zk/blob/main/design/states.md)
- [Data Design](https://github.com/CDLUC3/mrt-zk/blob/main/design/data.md)
- [Transition Design](https://github.com/CDLUC3/mrt-zk/blob/main/design/transition.md)
  - _The implementation may have diverged from this document_

---

## Unit Testing Merritt Queue

- Near pairity between the Java and Ruby implementations of the design
- Test case before/after encapsulated in [yaml](https://github.com/CDLUC3/mrt-zk/blob/main/test-cases.yml)

----

## Unit Test

- Function specifies the yaml key it is implementing
- ZK nodes are cleared
- Test code is run
- ZK nodes are serialized to yaml
- Test output is compared to `test-cases.yml`

----

## Sample Test Case

- [Java](https://github.com/CDLUC3/mrt-zk/blob/2.2.1/src/test/java/org/cdlib/mrt/zk/ZKTestIT.java#L410-L424)
- [Ruby](https://github.com/CDLUC3/mrt-zk/blob/2.2.1/src/test/java/org/cdlib/mrt/zk/ZKTestIT.java#L410-L424)
- [Expected Output](https://github.com/CDLUC3/mrt-zk/blob/2.2.1/test-cases.yml#L98-L120)

---

## Thank You

https://github.com/CDLUC3/mrt-zk
