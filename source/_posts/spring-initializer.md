---
title: spring-initializer
date: 2019-12-20 14:18:24
tags:
---

1. An ApplicationStartingEvent is sent at the start of a run, but before any processing except the registration of listeners and initializers.
2. An ApplicationEnvironmentPreparedEvent is sent when the Environment to be used in the context is known, but before the context is created.
3. An ApplicationPreparedEvent is sent just before the refresh is started, but after bean definitions have been loaded.
4. An ApplicationReadyEvent is sent after the refresh and any related callbacks have been processed to indicate the application is ready to service requests.
5. An ApplicationFailedEvent is sent if there is an exception on startup.


### 1. SharedMetadataReaderFactoryContextInitializer

- initialize 함수가 호출되면 applicationContext에 PostProcessor로 CachingMetadataReaderFactoryPostProcessor를 등록한다.

#### CachingMetadataReaderFactoryPostProcessor
```java
		@Override
		public int getOrder() {
			// Must happen before the ConfigurationClassPostProcessor is created
			return Ordered.HIGHEST_PRECEDENCE;
		}
```
	
```java
public interface Ordered {
	int HIGHEST_PRECEDENCE = Integer.MIN_VALUE;
	int LOWEST_PRECEDENCE = Integer.MAX_VALUE;
	int getOrder();
}
```
- ConfigurationClassPostProcessor가 생성되기전 반드시 수행되어야 하는 프로세서이다.
- HIGHEST_PRECEDENCE의 값은 Integer.MIN_VALUE
- register 수행시 BeanDefinitionRegistry에 BeanDefinition에 
  "org.springframework.boot.autoconfigure.internalCachingMetadataReaderFactory"으로 BeanDefinition 등록

####  