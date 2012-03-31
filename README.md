JSAT
====

Jsat intends to improve over the usual JVM thread dump. It recognizes that, usually, groups of threads run very 
similar code, and thus may share a few frames from the bottom of the stacks. Jsat prints stack frames hierarchically,
minimizing repeated output.
Additionally, information from /proc filesystem is collected regarding thread states. Along with JVM
state, OS process state is displayed. This can be especially useful with "Runnable" threads, which can 
actually be blocked during I/O operations.

Sample output (from a running Cassandra database):
-------------------------------------------------

    - java.lang.Thread.run(Thread.java:662)
        - com.sun.jmx.remote.internal.ServerCommunicatorAdmin$Timeout.run(ServerCommunicatorAdmin.java:150)
        - java.lang.Object.wait(Native Method)
        + TIMED_WAITING (on object monitor)  
        + S (interruptible sleep) in futex_queues()
            "JMX server connection timeout 88519" tid=0x5d33d000 nid=20293
            "JMX server connection timeout 88518" tid=0x5d336000 nid=20285
            "JMX server connection timeout 88517" tid=0x5d332800 nid=20282
            "JMX server connection timeout 88516" tid=0x5d330000 nid=20276
            "JMX server connection timeout 88515" tid=0x5d316800 nid=20275
            "JMX server connection timeout 88514" tid=0x5d32a800 nid=20274
    
        - java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:907)
            - java.util.concurrent.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:945)
                - java.util.concurrent.LinkedBlockingQueue.poll(LinkedBlockingQueue.java:424)
                - java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:2025)
                - java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:198)
                - sun.misc.Unsafe.park(Native Method)
                    + TIMED_WAITING (parking) on 0x43f22b210 (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
                    + S (interruptible sleep) in futex_queues()
                        "MutationStage:1771" tid=0x5cb1f800 nid=11499
                        "MutationStage:1770" tid=0x2aaaefea2800 nid=31124
                        "MutationStage:1769" tid=0x2abf480b4000 nid=31042
                        "MutationStage:1768" tid=0x5cb21000 nid=31040
                        "MutationStage:1766" tid=0x5a456000 nid=30999
                        "MutationStage:1764" tid=0x5b09d800 nid=30682
                        "MutationStage:1763" tid=0x5cb2e800 nid=30613
                        "MutationStage:1761" tid=0x5cb37000 nid=30521
                        "MutationStage:1760" tid=0x2abf480db000 nid=30429
                        "MutationStage:1759" tid=0x2abf480b6000 nid=30428
                        16 more threads...
    
                    + TIMED_WAITING (parking) on 0x43f22d318 (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
                    + S (interruptible sleep) in futex_queues()
                        "RequestResponseStage:214" tid=0x5cb34800 nid=31041
                        "RequestResponseStage:211" tid=0x5a553800 nid=29798
                        "RequestResponseStage:210" tid=0x5c3e0800 nid=29789
                        "RequestResponseStage:208" tid=0x5bc43000 nid=29707
                        "RequestResponseStage:203" tid=0x5cb35800 nid=10691
                        "RequestResponseStage:201" tid=0x5a52c800 nid=7556
    
                    + TIMED_WAITING (parking) on 0x43f22b210 (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
                    + S (interruptible sleep) 
                        "MutationStage:1762" tid=0x5bda3000 nid=30522
                        "MutationStage:1734" tid=0x2abf480b9000 nid=29704
                        "MutationStage:1733" tid=0x2abf480c7800 nid=29703
                        "MutationStage:1718" tid=0x2abf480b7800 nid=28997
                        "MutationStage:1712" tid=0x2abf480b7000 nid=28980
                        "MutationStage:1700" tid=0x2aaaee865800 nid=28725
    
                - java.util.concurrent.SynchronousQueue.poll(SynchronousQueue.java:874)
                - java.util.concurrent.SynchronousQueue$TransferStack.transfer(SynchronousQueue.java:323)
                - java.util.concurrent.SynchronousQueue$TransferStack.awaitFulfill(SynchronousQueue.java:424)
                - java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:198)
                - sun.misc.Unsafe.park(Native Method)
                    + TIMED_WAITING (parking) on 0x43f2a2548 (a java.util.concurrent.SynchronousQueue$TransferStack)
                    + S (interruptible sleep) in futex_queues()
                        "RMI TCP Connection(idle)" tid=0x5d2f4000 nid=20271
                        "RMI TCP Connection(idle)" tid=0x5d218800 nid=18522
    
                    + TIMED_WAITING (parking) on 0x4a49861c0 (a java.util.concurrent.SynchronousQueue$TransferStack)
                    + S (interruptible sleep) in futex_queues()
                        "pool-2-thread-230" tid=0x2aaaefe37800 nid=16428
    
            - java.util.concurrent.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:947)
                - java.util.concurrent.ScheduledThreadPoolExecutor$DelayedWorkQueue.take(ScheduledThreadPoolExecutor.java:602)
                - java.util.concurrent.ScheduledThreadPoolExecutor$DelayedWorkQueue.take(ScheduledThreadPoolExecutor.java:609)
                - java.util.concurrent.DelayQueue.take(DelayQueue.java:164)
                - java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.awaitNanos(AbstractQueuedSynchronizer.java:2025)
                - java.util.concurrent.locks.LockSupport.parkNanos(LockSupport.java:198)
                - sun.misc.Unsafe.park(Native Method)
                    + TIMED_WAITING (parking) on 0x4a4986c70 (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
                    + S (interruptible sleep) 
                        "GossipTasks:1" tid=0x2aaac8117000 nid=3834
    
                    + TIMED_WAITING (parking) on 0x43f2a24d0 (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
                    + S (interruptible sleep) in futex_queues()
                        "RMI Scheduler(0)" tid=0x56722800 nid=3819
    
                    + TIMED_WAITING (parking) on 0x43f22b228 (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
                    + S (interruptible sleep) 
                        "NonPeriodicTasks:1" tid=0x2aaac81af000 nid=3688
    
                    + TIMED_WAITING (parking) on 0x43f236d60 (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
                    + S (interruptible sleep) 
                        "ScheduledTasks:1" tid=0x2aaac811a800 nid=3676
    
                - java.util.concurrent.SynchronousQueue.take(SynchronousQueue.java:857)
                - java.util.concurrent.SynchronousQueue$TransferStack.transfer(SynchronousQueue.java:323)
                - java.util.concurrent.SynchronousQueue$TransferStack.awaitFulfill(SynchronousQueue.java:422)
                - java.util.concurrent.locks.LockSupport.park(LockSupport.java:158)
                - sun.misc.Unsafe.park(Native Method)
                + WAITING (parking) on 0x43f28e3c8 (a java.util.concurrent.SynchronousQueue$TransferStack)
                + S (interruptible sleep) in futex_queues()
                    "pool-1-thread-1" tid=0x5670b800 nid=3817
    
        - java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:908)
        - java.util.concurrent.ThreadPoolExecutor$Worker.runTask(ThreadPoolExecutor.java:886)
        - org.apache.cassandra.thrift.CustomTThreadPoolServer$WorkerProcess.run(CustomTThreadPoolServer.java:187)
        - org.apache.cassandra.thrift.Cassandra$Processor.process(Cassandra.java:2877)
        - org.apache.thrift.protocol.TBinaryProtocol.readMessageBegin(TBinaryProtocol.java:204)
        - org.apache.thrift.protocol.TBinaryProtocol.readI32(TBinaryProtocol.java:297)
        - org.apache.thrift.protocol.TBinaryProtocol.readAll(TBinaryProtocol.java:378)
        - org.apache.thrift.transport.TTransport.readAll(TTransport.java:84)
        - org.apache.thrift.transport.TFramedTransport.read(TFramedTransport.java:101)
        - org.apache.thrift.transport.TFramedTransport.readFrame(TFramedTransport.java:129)
        - org.apache.thrift.transport.TTransport.readAll(TTransport.java:84)
        - org.apache.thrift.transport.TIOStreamTransport.read(TIOStreamTransport.java:127)
        - java.net.SocketInputStream.read(SocketInputStream.java:129)
        - java.net.SocketInputStream.socketRead0(Native Method)
        + RUNNABLE   
        + S (interruptible sleep) in 0()
            "pool-2-thread-311" tid=0x2aaaef623800 nid=14673
            "pool-2-thread-304" tid=0x2abf480e3000 nid=3879
            "pool-2-thread-302" tid=0x2abf480a7800 nid=30464
            "pool-2-thread-295" tid=0x2aaaee8a5000 nid=26090
            "pool-2-thread-293" tid=0x2abf480b2800 nid=24337
            "pool-2-thread-292" tid=0x2abf480dc000 nid=23777
            "pool-2-thread-290" tid=0x2abf480c3800 nid=20889
            "pool-2-thread-289" tid=0x2abf480cb000 nid=20292
            "pool-2-thread-285" tid=0x2abf480ca000 nid=11051
            "pool-2-thread-284" tid=0x2abf480c9800 nid=11048
            13 more threads...
