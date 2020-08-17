---
title: java-stream-parallel
date: 2020-02-19 21:12:14
tags:
---
Java 8의 변경 사항중 하나인 lambda를 효과적으로 사용할 수 있도록 collection들은 stream을 제공한다.
stream interface는 collection을 파이프식으로 처리 가능하도록 고차함수로 그 구조를 추상화 했다.

스트림을 사용하면서, 반복적인 형번환 소스코드를 간편하게 처리 할 수 있게 되었고, 가독성 또한 높아졌다. 만
또한 병렬연산을 쉽게 지원할 수 있게 도와주니 매력적으로 보인다.

하지만 Thread Pool을 직접 지정하는 이전 소스코드에 비해 Java 내부에 존재하는 공유  CommonPorkJoinPool 을 사용하기 때문에
여러개의 Thread가 일을 수행하는 Web Project의 경우에는 공유 Thread Pool의 Thread가 모자랄수 있다.
Java는 내부적으로 Parallel Stream이 CommonPorkJoinPool을 가지고 있고 내부 Thread의 수를 해당 머신의 프로세서의 개수로 처음에 만들어 시작한다.

그래서 Property로 Thread의 수를 지정하거나 ForkJoinPool을 명시적으로 사용해 parallelStream을 처리할 수 있게 할 수 있다.

1) Property 설정
```java
System.setProperty("java.util.concurrent.ForkJoinPool.common.parallelism","6");
```

2) 명시적 ForkJoinPool 사용
```java
ForkJoinPool forkjoinPool = new ForkJoinPool(5);
forkjoinPool.submit(() -> {
	list.parallelStream().forEach(index -> {
		System.out.printIn("Thread : " + Thread.currentThread().getName()
             + ", index + ", " + new Date());
		try{
			Thread.sleep(500);
		} catch (InterruptedException e){
		}
	});
}).get();
```

ForkJoinPool 생성자에 Thread 개수를 지정해서 사용할 수 있다.

ForkJoinPool
![](/images/java/stream/fork_join.jpg)

기본적으로는 ExecutorService의 구현체이지만,  다른 점은 각 thread들이 개별 큐를 가지게 되며, 
다음 그림의 B처럼 아무런 task가 없으면 A의 task를 가져와 처리하게 됨으로써 
CPU 자원을 효율적으로 사용할 수 있게 된다.

![](/images/java/stream/fork_join_queue.jpg)

ForkJoinPool의 특성상 나누어지는 Job은 균등하게 처리가 되어야 한다.
Spliterator의 trySplit()을 사용해 작업을 분할 하는데 균등하지 않으면 순차적으로 실행하는 것보다 비효율적일 수 있다.
```java
public Spliterator<T> trySplit() {
    int lo = index, mid = (lo + fence) >>> 1;
    return (lo >= mid)
           ? null
           : new ArraySpliterator<>(array,
                                    lo, index = mid,
                                    characteristics);
}
```

또한, 병렬로 처리되는 작업이 독립적이지 않다면, 수행 성능에 영향이 있을 수 있다.
stream의 중간 단계 연산 중 sorted(), distinct()와 같은 작업을 수행하는 경우에는 
내부적으로 상태에 대한 변수를 각 작업들이 공유(synchronized)하게 되어 있다. 
이럴때 순차적으로 실행하는 경우가 더 효과적일 수 있다.

참고
http://gee.cs.oswego.edu/dl/html/StreamParallelGuidance.html 