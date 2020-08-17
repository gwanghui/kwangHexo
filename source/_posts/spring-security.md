---
title: spring-security
date: 2019-11-22 14:04:46
tags:
---
## Authentication Manager

Spring Security에서는 AuthenticationManager라는 Interface가 존재하고 이를 구현하면 된다.
웹 어플리케이션에서 보통 WebConfig를 설정하기 위해 extends 받는 객체가 WebSecurityConfigurerAdapter이며 
이는 WebSecurityConfigurer를 구현한 것이다. 
Init함수 실행시 HttpSecurity를 생성하고 내부적으로 내부 init 함수 안에서 최초 1번 생성해서 반환하게 된다.

## Provider Manager
Provider Manager는 Request의 Authentication을 다루는 AuthenticationProvider를 관리하기 위한 manager이며, 
각각의 AuthenticationProvider의 구현에 따라 인증할 수 있는 범위가 넓어지며, 
DAO기반, LDAP기반, anonymous기반 등 여러가지 Provider를 관리할 수 있다. 만약 각 Provider에서 처리할 수 없고 null값이 나온다면
Provider Manager는 ProviderNotFoundException을 Throw 해 처리한다.

## Authentication - DaoAuthenticationProvider

Authentication의 간단한 구현체는 DaoAuthenticationProvider이며 이는 프레임워크에서 조기에 지원한 하나의 구현체입니다. 
```java
public class DaoAuthenticationProvider extends AbstractUserDetailsAuthenticationProvider {
	private static final String USER_NOT_FOUND_PASSWORD = "userNotFoundPassword";

	private PasswordEncoder passwordEncoder;

	private volatile String userNotFoundEncodedPassword;
    ## UserDetailService
	private UserDetailsService userDetailsService;

	private UserDetailsPasswordService userDetailsPasswordService; 
```

UserDetailService를 Injection받아 해당 유저가 올바른 유저인지 체크한다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

@Configuration
public class WebConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public UserDetailsService userDetailsService() {
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(User.withUsername("user").password("password").roles("USER").build());
        return manager;
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.
            authorizeRequests()
            .antMatchers("/db-console/**")
            .permitAll().anyRequest().authenticated();
        http.csrf().disable();
        http.headers().frameOptions().disable();

    }
}
```
보통 테스트시에는 InMemoryUserDetailsManager를 UserDetailsService 빈으로 생성해 사용한다.
인증을 성공한 이후 보안상의 문제로 인증된 Authentication 객체를 삭제한다고 한다. 허나 Stateless Application상의 성능 향상을 위해선 Cache가 필수이고
이를 지원하기 위해 AuthenticationProvider에 캐쉬구현을 하거나 복사본을 만들어 놓고, eraseCredentialsAfterAuthentication Property를 disable 할수도 있다고 한다.
자세한 사항은 Javadoc의 구현을 확인하라고 한다. 

### UserDetailService 

 

