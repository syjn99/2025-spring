# 0413 - Prepare Midterm

## Producer-consumer problem

```
Semaphore nFullSlots = 0;
Semaphore nEmptySlots = N;
semaphore mutex = 1;

Producer(item) {
    nEmptySlots.wait();
    mutex.wait();
    Enqueue(item);
    mutex.signal();
    nFullSlots.signal();
}

Consumder() {
    nFullSlots.wait();
    mutex.wait();
    item = Dequeue();
    mutex.signal();
    nEmptySlots.signal();
}
```

- why the sequence matters?

### Sequence of waiting

```
Producer(item) {
    mutex.wait();       // CHANGED!
    nEmptySlots.wait(); // CHANGED!
    Enqueue(item);
    mutex.signal();
    nFullSlots.signal();
}

Consumder() {
    nFullSlots.wait();
    mutex.wait();
    item = Dequeue();
    mutex.signal();
    nEmptySlots.signal();
}
```

- 만약 nEmptySlots = 0이라고 가정. (즉 버퍼에 다 가득참)
- Producer가 mutex lock을 획득, nEmptySlots를 기다리는 중
- 그 와중에 Consumer는 nFullSlots.wait()에서 하나를 decrement하고, mutex.wait()을 통해 producer가 lock을 놓길 기다리는 중.
- **데드락 상황!**
- 다른 상황을 보자

```
Producer(item) {
    nEmptySlots.wait();
    mutex.wait();       
    Enqueue(item);
    mutex.signal();
    nFullSlots.signal();
}

Consumder() {
    mutex.wait();       // CHANGED!
    nFullSlots.wait();  // CHANGED!
    item = Dequeue();
    mutex.signal();
    nEmptySlots.signal();
}
```

- nFullSlots가 0이라고 가정. (즉 버퍼에 아무 것도 없는 상황)
- Consumer가 먼저 mutex를 획득. nFullSlots를 기다리는 상황.
- 근데 Producer에서 nFullSlots를 올려줘야 하는데, mutex.wait()에서 하염없이 대기 중임.
- **데드락 상황!**

### Sequence of signaling

```
Producer(item) {
    nEmptySlots.wait();
    mutex.wait();
    Enqueue(item);
    nFullSlots.signal();    // CHANGED!
    mutex.signal();         // CHANGED!
}

Consumder() {
    nFullSlots.wait();
    mutex.wait();
    item = Dequeue();
    mutex.signal();
    nEmptySlots.signal();
}
```

- Consumer가 nFullSlots를 기다리고 있다고 가정하자.
- Producer가 먼저 signal을 했고, Consumer가 깨어남.
- 근데 mutex가 아직 release되지 않았기 때문에, 다시 sleep에 들어가야 함.
- 기존 코드는 항상 enqueue -> mutex release -> signal -> dequeue가 보장



- GPT의 답변

Yes, the order of `signal()` calls is important in concurrent programming. 

Although the final shared data (e.g., a queue) may remain consistent, signaling a condition (e.g., `nFullSlots.signal()`) before releasing the associated lock (e.g., `mutex.signal()`) can lead to subtle race conditions. A thread waiting on that condition may be woken up prematurely, only to be blocked again when it tries to acquire the lock. This introduces unnecessary context switches and breaks the logical order of operations.

The correct order ensures that any signal sent to another thread truly reflects a completed and safe-to-proceed state. For instance, in a Producer-Consumer model, the producer must first complete its enqueue operation and release the lock before notifying the consumer that an item is available. This guarantees that the consumer, once woken up, can immediately and safely access the shared resource.

Therefore, preserving the correct signal order is essential for both correctness and efficiency in concurrent systems.


“논리적 순서를 지킨다는 건, 단순히 지금 돌아가게 만드는 게 아니라, 앞으로의 유지 보수와 확장을 안전하게 보장하는 기반을 다지는 것”

