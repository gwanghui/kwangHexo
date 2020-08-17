---
title: springboot-run
date: 2019-12-19 14:57:33
tags: SpringBoot
---
Spring Boot를 사용한 프로젝트를 진행하다보니 Spring Boot 및 Security에 대한 전반적인 지식이 필요 해졌고, 
기존 설정들과는 다른 부분들이 존재해 Run을 진행하는 Entry Point 부터 하나씩 분석하기로 한다.

https://www.jetbrains.com/help/idea/class-diagram.html 로 클래스 다이어그램을 추출해 기록할 예정이며,
분석하는 Spring Boot버전은 2.1.1, Tomcat은 9버전이 될 것이다. 

```java
	public ConfigurableApplicationContext run(String... args) {
		StopWatch stopWatch = new StopWatch();
		stopWatch.start();
		ConfigurableApplicationContext context = null;
		Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
		configureHeadlessProperty(); // 1
		SpringApplicationRunListeners listeners = getRunListeners(args); // 2
		listeners.starting();
		try {
			ApplicationArguments applicationArguments = new DefaultApplicationArguments(args); // 1
			ConfigurableEnvironment environment = prepareEnvironment(listeners, applicationArguments);
			configureIgnoreBeanInfo(environment);
			Banner printedBanner = printBanner(environment);
			context = createApplicationContext();
			exceptionReporters = getSpringFactoriesInstances(SpringBootExceptionReporter.class,
					new Class[] { ConfigurableApplicationContext.class }, context);
			prepareContext(context, environment, listeners, applicationArguments, printedBanner);
			refreshContext(context);
			afterRefresh(context, applicationArguments);
			stopWatch.stop();
			if (this.logStartupInfo) {
				new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
			}
			listeners.started(context);
			callRunners(context, applicationArguments);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, exceptionReporters, listeners);
			throw new IllegalStateException(ex);
		}

		try {
			listeners.running(context);
		}
		catch (Throwable ex) {
			handleRunFailure(context, ex, exceptionReporters, null);
			throw new IllegalStateException(ex);
		}
		return context;
	}
```

### 1 Configure HeadLess Property

- SYSTEM_PROPERTY_JAVA_AWT_HEADLESS를 True로 바꿔준다. 
- SYSTEM_PROPERTY_JAVA_AWT_HEADLESS는 비 윈도우 환경에서 GUI컴포넌트를 사용할 수 있게 하는 JAVA System Property

### 2 getRunListeners
- Observer Pattern으로 동록된 모든 Listener 들에게 해당 이벤트가 발생할 경우 전부 실행해준다.

```java
class SpringApplicationRunListeners {

	private final Log log;

	private final List<SpringApplicationRunListener> listeners;

	SpringApplicationRunListeners(Log log, Collection<? extends SpringApplicationRunListener> listeners) {
		this.log = log;
		this.listeners = new ArrayList<>(listeners);
	}

	public void starting() {
		for (SpringApplicationRunListener listener : this.listeners) {
			listener.starting();
		}
	}
}
```

SpringBoot는 Listener를 별도로 등록하지 않을 경우 EventPublishingRunListener를 등록해 사용한다.
 