---
title: spring-security-oauth2
date: 2020-03-30 08:12:02
tags:
---

## Spring-Security-oauth2 vs Spring Security 
- Group ID Spring Security 안의 ArtifactID로 oauth2로 이관되면서 Group ID가 security-oauth2 였던 OSS가 Deprecated 되었다.

```yaml
spring:
  thymeleaf:
    cache: false
  security:
    oauth2:
      client:
        registration:
          messaging-client-auth-code:
            provider: keycloak
            client-id: messaging-client
            client-secret: secret
            authorization-grant-type: authorization_code
            redirect-uri: "{baseUrl}/authorized"
            scope: message.read,message.write
          messaging-client-client-creds:
            provider: keycloak
            client-id: messaging-client
            client-secret: secret
            authorization-grant-type: client_credentials
            scope: message.read,message.write
          messaging-client-password:
            provider: keycloak
            client-id: messaging-client
            client-secret: secret
            authorization-grant-type: password
            scope: message.read,message.write
        provider:
          keycloak:
            authorization-uri: http://localhost:8090/auth/realms/oauth2-sample/protocol/openid-connect/auth
            token-uri: http://localhost:8090/auth/realms/oauth2-sample/protocol/openid-connect/token

```

### ClientRegistration
- Oauth 2.0 or OpenID Connect 1.0 Provider 들에 등록된 Client의 정보들을 적게된다.
- client 정보에는 ID, Secret, authorization grant type, redirect URL... etc

```java
public final class ClientRegistration implements Serializable {
	private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;
	private String registrationId;
	private String clientId;
	private String clientSecret;
	private ClientAuthenticationMethod clientAuthenticationMethod = ClientAuthenticationMethod.BASIC;
	private AuthorizationGrantType authorizationGrantType;
	private String redirectUriTemplate;
	private Set<String> scopes = Collections.emptySet();
	private ProviderDetails providerDetails = new ProviderDetails();
	private String clientName;

    public class ProviderDetails implements Serializable {
        private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;
        private String authorizationUri;
        private String tokenUri;
        private UserInfoEndpoint userInfoEndpoint = new UserInfoEndpoint();
        private String jwkSetUri;
        private Map<String, Object> configurationMetadata = Collections.emptyMap();
    }
}
```

1. registrationId: The ID that uniquely identifies the ClientRegistration.
2. clientId: The client identifier.
3. clientSecret: The client secret.
4. clientAuthenticationMethod: The method used to authenticate the Client with the Provider. The supported values are basic, post and none (public clients).
5. authorizationGrantType: The OAuth 2.0 Authorization Framework defines four Authorization Grant types. The supported values are authorization_code, client_credentials and password.
6. redirectUriTemplate: The client’s registered redirect URI that the Authorization Server redirects the end-user’s user-agent to after the end-user has authenticated and authorized access to the client.
7. scopes: The scope(s) requested by the client during the Authorization Request flow, such as openid, email, or profile.
8. clientName: A descriptive name used for the client. The name may be used in certain scenarios, such as when displaying the name of the client in the auto-generated login page.
9. authorizationUri: The Authorization Endpoint URI for the Authorization Server.
10. tokenUri: The Token Endpoint URI for the Authorization Server.
11. jwkSetUri: The URI used to retrieve the JSON Web Key (JWK) Set from the Authorization Server, which contains the cryptographic key(s) used to verify the JSON Web Signature (JWS) of the ID Token and optionally the UserInfo Response.
12. configurationMetadata: The OpenID Provider Configuration Information. This information will only be available if the Spring Boot 2.x property spring.security.oauth2.client.provider.[providerId].issuerUri is configured.
13. (userInfoEndpoint)uri: The UserInfo Endpoint URI used to access the claims/attributes of the authenticated end-user.
14. (userInfoEndpoint)authenticationMethod: The authentication method used when sending the access token to the UserInfo Endpoint. The supported values are header, form and query.
15. userNameAttributeName: The name of the attribute returned in the UserInfo Response that references the Name or Identifier of the end-user.

> Endpoint 별 초기화
>  > OpenID Connect Provider : configuration endpoint
>  > OAuth2.0 : Metadata endpoint

### ClientRegistrationRepository

```java
public final class InMemoryClientRegistrationRepository implements ClientRegistrationRepository, Iterable<ClientRegistration> {
	private final Map<String, ClientRegistration> registrations;

	@Override
	public ClientRegistration findByRegistrationId(String registrationId) {
		Assert.hasText(registrationId, "registrationId cannot be empty");
		return this.registrations.get(registrationId);
	}
}
```

- Default 구현체는 InMemoryClientRegistrationRepository 이다.
- Map으로 ClientRegistration 들을 가지고 있으며, 해당 registrationId 로 ClientRegistration을 찾을수 있는 기능을 제공한다.
- Spring boot 2.x auto-configuration은 spring.security.oauth2.client.registration.[registrationId] 의 형태로 바인드를 자동으로 해놓는다.
- 또한 ClientRegistrationRepository라는 이름으로 Bean 등록을 해놓았기 때문에 Inject 받아서 사용가능하다.

### OAuthAuthorizedClient
- Authorized Client를 표현한다.
- Authorized Client는 보호되는 자원에 대한 Access 권한을 받은 것으로 간주된다.
- Oauth2AuthorizedClient 는 ClientRegistration과 Resource Owner(End-user) 간 OAuth2AccessToken, OAuth2RefreshToken을 연결시켜준다.

```java
public class OAuth2AuthorizedClient implements Serializable {
	private static final long serialVersionUID = SpringSecurityCoreVersion.SERIAL_VERSION_UID;
	private final ClientRegistration clientRegistration;
	private final String principalName;
	private final OAuth2AccessToken accessToken;
	private final OAuth2RefreshToken refreshToken;
}
```

