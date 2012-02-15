JSAT
====

Jsat intends to improve over the usual JVM thread dump. It recognizes that, usually, groups of threads run very 
similar code, and thus may share a few frames from the bottom of the stacks. Jsat prints stack frames hierarchically,
minimizing repeated output.
Additionally, information from /proc filesystem is collected regarding thread states. Along with JVM
state, OS process state is displayed. This can be especially useful with "Runnable" threads, which can be 
actually be blocked during I/O operations.

