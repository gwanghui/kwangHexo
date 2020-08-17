---
title: spring-boot-configuration-properties
date: 2020-06-29 11:01:57
tags:
---
### @ConfigurationProperties
- Spring Mail 서비스로 메일 전송하는 기능을 만드려고 하다가 application.yml과 연동되는 Property 값들을 정의하는 방법부터 정리하고자 한다.
```java
@ConfigurationProperties(prefix = "spring.mail")
public class MailProperties {
	private String host;
	private Integer port;
	private String username;
	private String password;
}
```

```yaml
spring:
    mail:
      host:
      port:
      username:
      password:      
```

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass({ MimeMessage.class, MimeType.class, MailSender.class })
@ConditionalOnMissingBean(MailSender.class)
@Conditional(MailSenderCondition.class)
@EnableConfigurationProperties(MailProperties.class)
@Import({ MailSenderJndiConfiguration.class, MailSenderPropertiesConfiguration.class })
public class MailSenderAutoConfiguration {
	static class MailSenderCondition extends AnyNestedCondition {
		MailSenderCondition() {
			super(ConfigurationPhase.PARSE_CONFIGURATION);
		}
		@ConditionalOnProperty(prefix = "spring.mail", name = "host")
		static class HostProperty {

		}
		@ConditionalOnProperty(prefix = "spring.mail", name = "jndi-name")
		static class JndiNameProperty {

		}

	}

}
```

- @EnableConfigurationProperties 로 사용할 MailProperties.class를 선언한 뒤 MailSenderJndiConfiguration과 MailSenderPropertiesConfiguration.class의
  설정을 가져와 처리한다.
- 이 값을 사용하기 위해서는 Bean으로 선언되어 있기 때문에 주입 받아서 사용하면 된다.

```java
@Service
public class EMailService {
    @Autowired
    public JavaMailSender javaMailSender;
    
    @Autowired
    public MailProperties mailProperties;

    public void sendSimpleMessageMail(String to, String subject, String content) {
       // javaMailSender.send()
    }
}
```