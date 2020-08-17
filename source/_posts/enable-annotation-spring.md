---
title: enable-annotation-spring
date: 2020-01-20 20:06:59
tags:
- Java
- Spring
- EnableAnnotation
categories:
- Java
- Spring
---

### Spring Boot에서 Enable로 시작하는 Annotation

- Bean을 생성할 때, 고정 값이 아닌 동적으로 값을 얻어와 빈을 생성해 주어야 하는 경우가 있다. 
    - ex) Url이 DB에 저장되어 있거나 File에 저장되어 있어 이를 읽어와 빈을 생성해야 하는경우

- 선언적으로 빈을 생성하지 못하고 동적으로 빈을 생성해야 할 경우 @Enable* Annotation을 사용해 빈을 선언할 수 있다.

#### @Enable* Annotation 을 사용하는 방법은 3가지이다.
- @Import를 사용하여 정의한 Bean을 불러와서 사용 ex) EnableScheduling

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Import(SchedulingConfiguration.class)
@Documented
public @interface EnableScheduling {
 
}
@Configuration
public class SchedulingConfiguration {
 
 @Bean(name=AnnotationConfigUtils.SCHEDULED_ANNOTATION_PROCESSOR_BEAN_NAME)
 @Role(BeanDefinition.ROLE_INFRASTRUCTURE)
 public ScheduledAnnotationBeanPostProcessor scheduledAnnotationProcessor() {
  return new ScheduledAnnotationBeanPostProcessor();
 }
 
}
```

- @ImportSelector를 사용하여 빈을 생성하는 경우 
    - ImportSelector 인터페이스를 구현하고 Return하는 배열에 클래스의 이름을 적어 반환해주면 된다. Ex) EnableTransaction

```java

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(TransactionManagementConfigurationSelector.class)
public @interface EnableTransactionManagement {

	boolean proxyTargetClass() default false;
	AdviceMode mode() default AdviceMode.PROXY;

	int order() default Ordered.LOWEST_PRECEDENCE;
}

public class TransactionManagementConfigurationSelector extends AdviceModeImportSelector<EnableTransactionManagement> {
	@Override
	protected String[] selectImports(AdviceMode adviceMode) {
		switch (adviceMode) {
			case PROXY:
				return new String[] {AutoProxyRegistrar.class.getName(),
						ProxyTransactionManagementConfiguration.class.getName()};
			case ASPECTJ:
				return new String[] {determineTransactionAspectClass()};
			default:
				return null;
		}
	}

	private String determineTransactionAspectClass() {
		return (ClassUtils.isPresent("javax.transaction.Transactional", getClass().getClassLoader()) ?
				TransactionManagementConfigUtils.JTA_TRANSACTION_ASPECT_CONFIGURATION_CLASS_NAME :
				TransactionManagementConfigUtils.TRANSACTION_ASPECT_CONFIGURATION_CLASS_NAME);
	}
}

```

- @ImportBeanDefinitionRegistrar를 사용한 BeanRegistry에 Bean 등록하기
    - 동적으로 값을 얻어와 빈에 할당하려면 이 방법 뿐이였다.
    - 준비물 : Google Reflections Library, 


```java
public void registerBeanDefinitions(AnnotationMetadata annotationMetadata, BeanDefinitionRegistry registry) {
    String basePackageName = ((StandardAnnotationMetadata) annotationMetadata).getIntrospectedClass().getPackage().getName();
    Set<Class<?>> interfaces = new Reflections(basePackageName).getTypesAnnotatedWith(ProjectFeignClient.class);

    interfaces.forEach(type -> {
      BeanDefinitionBuilder builder = BeanDefinitionBuilder.genericBeanDefinition(ProjectFeignFactoryBean.class);
      builder.setLazyInit(true);
      builder.addPropertyValue("type", type.getName());
      builder.setAutowireMode(2);

      AbstractBeanDefinition definition = builder.getBeanDefinition();
      definition.setPrimary(true);

      BeanDefinitionReaderUtils.registerBeanDefinition(
              new BeanDefinitionHolder(definition, type.getName()), registry
      );
    });
```
    
