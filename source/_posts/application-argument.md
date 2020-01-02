---
title: application-argument
date: 2019-12-19 15:00:54
tags:
---
Spring boot 실행할 때 args 를 전달받기 위한 Interface.

Spring boot에서 org.springframework.boot.ApplicationArguments 를 제공하고 있어서 Bean으로 받아서 사용하면 간단하게 쓸 수 있습니다.

Spring boot에서 아무런 설정을 하지 않는다면 DefaultApplicationArguments를 사용해 처리한다.

1. getSourceArgs

 - 입력한 args 그대로 배열로 받아 옵니다.



2. getOptionNames

 - args 앞에 "--" 를 붙이면 옵션으로 인식 합니다. 옵션 args 사용 형식 --NAME=VALUE 

 - "--fruit=apple" 이렇게 args를 사용하면

 - getOptionName는 fruit 처럼 option name 들의 배열을 받아 옵니다.



3. getNonOptionArgs

- "--" 가 없는 경우 NonOption으로 인식합니다.

- "--" 가 없는 args 들의 값들을 받다 옵니다.


![](/images/application-argument/DefaultApplicationArguments.png)


```java
	private final Source source;

	private final String[] args;

	public DefaultApplicationArguments(String[] args) {
		Assert.notNull(args, "Args must not be null");
		this.source = new Source(args);
		this.args = args;
	}
```

내부에 private final로 선언되어 있어 생성자를 통한 할당이 아니라면 바꿀수가 없다. 
또한 Source라고 하는 내부 Class를 가지고 있고, 이는 SimpleCommandLinePropertySource를 상속해 구현하였다.

```java
	private static class Source extends SimpleCommandLinePropertySource {

		Source(String[] args) {
			super(args);
		}

		@Override
		public List<String> getNonOptionArgs() {
			return super.getNonOptionArgs();
		}

		@Override
		public List<String> getOptionValues(String name) {
			return super.getOptionValues(name);
		}

	}
```
![](/images/application-argument/SimpleCommandLineProperty.png)

```java
public class SimpleCommandLinePropertySource extends CommandLinePropertySource<CommandLineArgs> {

	/**
	 * Create a new {@code SimpleCommandLinePropertySource} having the default name
	 * and backed by the given {@code String[]} of command line arguments.
	 * @see CommandLinePropertySource#COMMAND_LINE_PROPERTY_SOURCE_NAME
	 * @see CommandLinePropertySource#CommandLinePropertySource(Object)
	 */
	public SimpleCommandLinePropertySource(String... args) {
		super(new SimpleCommandLineArgsParser().parse(args));
	}

	/**
	 * Create a new {@code SimpleCommandLinePropertySource} having the given name
	 * and backed by the given {@code String[]} of command line arguments.
	 */
	public SimpleCommandLinePropertySource(String name, String[] args) {
		super(name, new SimpleCommandLineArgsParser().parse(args));
	}

	/**
	 * Get the property names for the option arguments.
	 */
	@Override
	public String[] getPropertyNames() {
		return StringUtils.toStringArray(this.source.getOptionNames());
	}

	@Override
	protected boolean containsOption(String name) {
		return this.source.containsOption(name);
	}

	@Override
	@Nullable
	protected List<String> getOptionValues(String name) {
		return this.source.getOptionValues(name);
	}

	@Override
	protected List<String> getNonOptionArgs() {
		return this.source.getNonOptionArgs();
	}

}
```

SimpleCommandLinePropertySource는 CommandLineArgs를 Source를 가지는 CommandLinePropertySource를 상속해 구현하였고,
CommandLineArgs는 내부에 HashMap으로 Option Argument들을, List로 nonOptionArgs를 가지고 있는 Source로 관리된다. 

![](/images/application-argument/CommandLineArgs.png)
