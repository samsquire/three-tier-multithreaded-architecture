# three-tier-multithreaded-architecture

This is a design for multithreaded servers that you can adopt for scalability.

* Please email sam@samsquire.com or add a PR if you want to add notes

# diagram

![NonblockingRuntime.drawio.png](NonblockingRuntime.drawio.png)

* Splits work into `app threads/IO threads/CPU threads`
* Use work stealing thread pools for heavy CPU tasks
* Use lightweight threads for event loops, parallelism and request coordination
* Use `liburing` or `epoll` for IO scalability
* Use multiproducer multiconsumer lockless ringbuffers for communication.

 # App threads

These are **Controllers**

These are lightweight threads that are multiplexed over kernel threads and they can yield.

# IO Threads

* When a socket is ready to send data, we would wait for `EPOLLOUT` event and then send any queued data to send.
