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

### OAuth2AuthorizedClientRepository

```java
public final class AuthenticatedPrincipalOAuth2AuthorizedClientRepository implements OAuth2AuthorizedClientRepository {
	private final AuthenticationTrustResolver authenticationTrustResolver = new AuthenticationTrustResolverImpl();
	private final OAuth2AuthorizedClientService authorizedClientService;
	private OAuth2AuthorizedClientRepository anonymousAuthorizedClientRepository = new HttpSessionOAuth2AuthorizedClientRepository();

	@Override
	public <T extends OAuth2AuthorizedClient> T loadAuthorizedClient(String clientRegistrationId, Authentication principal,
																		HttpServletRequest request) {
		if (this.isPrincipalAuthenticated(principal)) {
			return this.authorizedClientService.loadAuthorizedClient(clientRegistrationId, principal.getName());
		} else {
			return this.anonymousAuthorizedClientRepository.loadAuthorizedClient(clientRegistrationId, principal, request);
		}
	}
	@Override
	public void saveAuthorizedClient(OAuth2AuthorizedClient authorizedClient, Authentication principal,
										HttpServletRequest request, HttpServletResponse response) {
		if (this.isPrincipalAuthenticated(principal)) {
			this.authorizedClientService.saveAuthorizedClient(authorizedClient, principal);
		} else {
			this.anonymousAuthorizedClientRepository.saveAuthorizedClient(authorizedClient, principal, request, response);
		}
	}

	@Override
	public void removeAuthorizedClient(String clientRegistrationId, Authentication principal,
										HttpServletRequest request, HttpServletResponse response) {
		if (this.isPrincipalAuthenticated(principal)) {
			this.authorizedClientService.removeAuthorizedClient(clientRegistrationId, principal.getName());
		} else {
			this.anonymousAuthorizedClientRepository.removeAuthorizedClient(clientRegistrationId, principal, request, response);
		}
	}

	private boolean isPrincipalAuthenticated(Authentication authentication) {
		return authentication != null &&
				!this.authenticationTrustResolver.isAnonymous(authentication) &&
				authentication.isAuthenticated();
	}
}
```

- AuthenticationTrustResolver에 질의해 인증된 사용자라면 service에 요청하고 아니면 anonymousAuthorizedClientRepository에 요청한다.
- OAuth2AuthorizationCodeAuthenticationToken으로 인증을 진행한다.
- Session Attribute Name = org.springframework.security.oauth2.client.web.HttpSessionOAuth2AuthorizedClientRepository.AUTHORIZED_CLIENTS 
- 내부에 anonymousAuthorizedClientRepository로 HttpSessionOAuth2AuthorizedClientRepository을 가지고 있다.
- 해당 Session Attribute Name으로 AuthorizedClient들을 저장하고 있다.


### AuthenticationTrustResolver

```java
public class AuthenticationTrustResolverImpl implements AuthenticationTrustResolver {
	private Class<? extends Authentication> anonymousClass = AnonymousAuthenticationToken.class;
	private Class<? extends Authentication> rememberMeClass = RememberMeAuthenticationToken.class;

	Class<? extends Authentication> getAnonymousClass() {
		return anonymousClass;
	}

	Class<? extends Authentication> getRememberMeClass() {
		return rememberMeClass;
	}

	public boolean isAnonymous(Authentication authentication) {
		if ((anonymousClass == null) || (authentication == null)) {
			return false;
		}

		return anonymousClass.isAssignableFrom(authentication.getClass());
	}

	public boolean isRememberMe(Authentication authentication) {
		if ((rememberMeClass == null) || (authentication == null)) {
			return false;
		}

		return rememberMeClass.isAssignableFrom(authentication.getClass());
	}

	public void setAnonymousClass(Class<? extends Authentication> anonymousClass) {
		this.anonymousClass = anonymousClass;
	}

	public void setRememberMeClass(Class<? extends Authentication> rememberMeClass) {
		this.rememberMeClass = rememberMeClass;
	}
}
```
- Anonymous, RememberMe 를 구현하고 할당이 가능한지에 따라 True, False를 반환한다.


### OAuth2AuthorizedClientService

``` java
public final class InMemoryOAuth2AuthorizedClientService implements OAuth2AuthorizedClientService {
	private final Map<OAuth2AuthorizedClientId, OAuth2AuthorizedClient> authorizedClients;
	private final ClientRegistrationRepository clientRegistrationRepository;

	public InMemoryOAuth2AuthorizedClientService(ClientRegistrationRepository clientRegistrationRepository) {
		Assert.notNull(clientRegistrationRepository, "clientRegistrationRepository cannot be null");
		this.clientRegistrationRepository = clientRegistrationRepository;
		this.authorizedClients = new ConcurrentHashMap<>();
	}

	public InMemoryOAuth2AuthorizedClientService(ClientRegistrationRepository clientRegistrationRepository,
													Map<OAuth2AuthorizedClientId, OAuth2AuthorizedClient> authorizedClients) {
		Assert.notNull(clientRegistrationRepository, "clientRegistrationRepository cannot be null");
		Assert.notEmpty(authorizedClients, "authorizedClients cannot be empty");
		this.clientRegistrationRepository = clientRegistrationRepository;
		this.authorizedClients = new ConcurrentHashMap<>(authorizedClients);
	}

	@Override
	@SuppressWarnings("unchecked")
	public <T extends OAuth2AuthorizedClient> T loadAuthorizedClient(String clientRegistrationId, String principalName) {
		Assert.hasText(clientRegistrationId, "clientRegistrationId cannot be empty");
		Assert.hasText(principalName, "principalName cannot be empty");
		ClientRegistration registration = this.clientRegistrationRepository.findByRegistrationId(clientRegistrationId);
		if (registration == null) {
			return null;
		}
		return (T) this.authorizedClients.get(new OAuth2AuthorizedClientId(clientRegistrationId, principalName));
	}

	@Override
	public void saveAuthorizedClient(OAuth2AuthorizedClient authorizedClient, Authentication principal) {
		Assert.notNull(authorizedClient, "authorizedClient cannot be null");
		Assert.notNull(principal, "principal cannot be null");
		this.authorizedClients.put(new OAuth2AuthorizedClientId(authorizedClient.getClientRegistration().getRegistrationId(),
				principal.getName()), authorizedClient);
	}

	@Override
	public void removeAuthorizedClient(String clientRegistrationId, String principalName) {
		Assert.hasText(clientRegistrationId, "clientRegistrationId cannot be empty");
		Assert.hasText(principalName, "principalName cannot be empty");
		ClientRegistration registration = this.clientRegistrationRepository.findByRegistrationId(clientRegistrationId);
		if (registration != null) {
			this.authorizedClients.remove(new OAuth2AuthorizedClientId(clientRegistrationId, principalName));
		}
	}
}
```

- ClientRegistrationRepository를 가지고 있고, 해당 Repository에 CRUD를 하는 기능들을 가지고 있다.
- Bean으로 선언되어 있어 Application레벨에서 사용이 용이하게 되어있다.

### OAuth2AuthorizedClientManager

```java
public final class DefaultOAuth2AuthorizedClientManager implements OAuth2AuthorizedClientManager {
	private final ClientRegistrationRepository clientRegistrationRepository;
	private final OAuth2AuthorizedClientRepository authorizedClientRepository;
	private OAuth2AuthorizedClientProvider authorizedClientProvider = context -> null;
	private Function<OAuth2AuthorizeRequest, Map<String, Object>> contextAttributesMapper = new DefaultContextAttributesMapper();

	@Nullable
	@Override
	public OAuth2AuthorizedClient authorize(OAuth2AuthorizeRequest authorizeRequest) {
		Assert.notNull(authorizeRequest, "authorizeRequest cannot be null");

		String clientRegistrationId = authorizeRequest.getClientRegistrationId();
		OAuth2AuthorizedClient authorizedClient = authorizeRequest.getAuthorizedClient();
		Authentication principal = authorizeRequest.getPrincipal();

		HttpServletRequest servletRequest = getHttpServletRequestOrDefault(authorizeRequest.getAttributes());
		Assert.notNull(servletRequest, "servletRequest cannot be null");
		HttpServletResponse servletResponse = getHttpServletResponseOrDefault(authorizeRequest.getAttributes());
		Assert.notNull(servletResponse, "servletResponse cannot be null");

		OAuth2AuthorizationContext.Builder contextBuilder;
		if (authorizedClient != null) {
			contextBuilder = OAuth2AuthorizationContext.withAuthorizedClient(authorizedClient);
		} else {
			ClientRegistration clientRegistration = this.clientRegistrationRepository.findByRegistrationId(clientRegistrationId);
			Assert.notNull(clientRegistration, "Could not find ClientRegistration with id '" + clientRegistrationId + "'");
			authorizedClient = this.authorizedClientRepository.loadAuthorizedClient(
					clientRegistrationId, principal, servletRequest);
			if (authorizedClient != null) {
				contextBuilder = OAuth2AuthorizationContext.withAuthorizedClient(authorizedClient);
			} else {
				contextBuilder = OAuth2AuthorizationContext.withClientRegistration(clientRegistration);
			}
		}
		OAuth2AuthorizationContext authorizationContext = contextBuilder
				.principal(principal)
				.attributes(attributes -> {
					Map<String, Object> contextAttributes = this.contextAttributesMapper.apply(authorizeRequest);
					if (!CollectionUtils.isEmpty(contextAttributes)) {
						attributes.putAll(contextAttributes);
					}
				})
				.build();

		authorizedClient = this.authorizedClientProvider.authorize(authorizationContext);
		if (authorizedClient != null) {
			this.authorizedClientRepository.saveAuthorizedClient(authorizedClient, principal, servletRequest, servletResponse);
		} else {
			// In the case of re-authorization, the returned `authorizedClient` may be null if re-authorization is not supported.
			// For these cases, return the provided `authorizationContext.authorizedClient`.
			if (authorizationContext.getAuthorizedClient() != null) {
				return authorizationContext.getAuthorizedClient();
			}
		}

		return authorizedClient;
	}
}
```

- 내부에 ClientRegistrationRepository로는 InMemoryClientRegistrationRepository를 가지고 있다.
- 내부에 AuthorizedClientRepository로 AuthenticatedPrincipalOAuth2AuthorizedClientRepository를 가지고 있다.
- 내부에 AuthenticationProvider로 DelegatingOAuth2AuthorizedClientProvider를 가지고 있다.
    - DelegatingOAuth2AuthorizedClientProvider는 Config에서 선언한 GrantType에 해당하는 Provider들을 전부 가지고 있다.
- The DefaultOAuth2AuthorizedClientManager is designed to be used within the context of a HttpServletRequest. 
  When operating outside of a HttpServletRequest context, use AuthorizedClientServiceOAuth2AuthorizedClientManager instead.

### OAuth2AuthorizedClientProvider

- 각 GrantType에 따른 인증 방식이 들어있다.
- 인증을 하는 조건이 맞지 않으면 Throw Exception을 던진다.

### OAuth2AuthorizationRequestRedirectFilter

```Java
public class OAuth2AuthorizationRequestRedirectFilter extends OncePerRequestFilter {
	public static final String DEFAULT_AUTHORIZATION_REQUEST_BASE_URI = "/oauth2/authorization";
	private final ThrowableAnalyzer throwableAnalyzer = new DefaultThrowableAnalyzer();
	private final RedirectStrategy authorizationRedirectStrategy = new DefaultRedirectStrategy();
	private OAuth2AuthorizationRequestResolver authorizationRequestResolver;
	private AuthorizationRequestRepository<OAuth2AuthorizationRequest> authorizationRequestRepository =
		new HttpSessionOAuth2AuthorizationRequestRepository();
	private RequestCache requestCache = new HttpSessionRequestCache();

	public OAuth2AuthorizationRequestRedirectFilter(ClientRegistrationRepository clientRegistrationRepository) {
		this(clientRegistrationRepository, DEFAULT_AUTHORIZATION_REQUEST_BASE_URI);
	}

	public OAuth2AuthorizationRequestRedirectFilter(ClientRegistrationRepository clientRegistrationRepository, String authorizationRequestBaseUri) {
		Assert.notNull(clientRegistrationRepository, "clientRegistrationRepository cannot be null");
		Assert.hasText(authorizationRequestBaseUri, "authorizationRequestBaseUri cannot be empty");
		this.authorizationRequestResolver = new DefaultOAuth2AuthorizationRequestResolver(
				clientRegistrationRepository, authorizationRequestBaseUri);
	}

	public OAuth2AuthorizationRequestRedirectFilter(OAuth2AuthorizationRequestResolver authorizationRequestResolver) {
		Assert.notNull(authorizationRequestResolver, "authorizationRequestResolver cannot be null");
		this.authorizationRequestResolver = authorizationRequestResolver;
	}

	public final void setAuthorizationRequestRepository(AuthorizationRequestRepository<OAuth2AuthorizationRequest> authorizationRequestRepository) {
		Assert.notNull(authorizationRequestRepository, "authorizationRequestRepository cannot be null");
		this.authorizationRequestRepository = authorizationRequestRepository;
	}

	public final void setRequestCache(RequestCache requestCache) {
		Assert.notNull(requestCache, "requestCache cannot be null");
		this.requestCache = requestCache;
	}

	@Override
	protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException {

		try {
			OAuth2AuthorizationRequest authorizationRequest = this.authorizationRequestResolver.resolve(request);
			if (authorizationRequest != null) {
				this.sendRedirectForAuthorization(request, response, authorizationRequest);
				return;
			}
		} catch (Exception failed) {
			this.unsuccessfulRedirectForAuthorization(request, response, failed);
			return;
		}

		try {
			filterChain.doFilter(request, response);
		} catch (IOException ex) {
			throw ex;
		} catch (Exception ex) {
			// Check to see if we need to handle ClientAuthorizationRequiredException
			Throwable[] causeChain = this.throwableAnalyzer.determineCauseChain(ex);
			ClientAuthorizationRequiredException authzEx = (ClientAuthorizationRequiredException) this.throwableAnalyzer
				.getFirstThrowableOfType(ClientAuthorizationRequiredException.class, causeChain);
			if (authzEx != null) {
				try {
					OAuth2AuthorizationRequest authorizationRequest = this.authorizationRequestResolver.resolve(request, authzEx.getClientRegistrationId());
					if (authorizationRequest == null) {
						throw authzEx;
					}
					this.sendRedirectForAuthorization(request, response, authorizationRequest);
					this.requestCache.saveRequest(request, response);
				} catch (Exception failed) {
					this.unsuccessfulRedirectForAuthorization(request, response, failed);
				}
				return;
			}

			if (ex instanceof ServletException) {
				throw (ServletException) ex;
			} else if (ex instanceof RuntimeException) {
				throw (RuntimeException) ex;
			} else {
				throw new RuntimeException(ex);
			}
		}
	}

	private void sendRedirectForAuthorization(HttpServletRequest request, HttpServletResponse response, OAuth2AuthorizationRequest authorizationRequest) throws IOException {

		if (AuthorizationGrantType.AUTHORIZATION_CODE.equals(authorizationRequest.getGrantType())) {
			this.authorizationRequestRepository.saveAuthorizationRequest(authorizationRequest, request, response);
		}
		this.authorizationRedirectStrategy.sendRedirect(request, response, authorizationRequest.getAuthorizationRequestUri());
	}

	private void unsuccessfulRedirectForAuthorization(HttpServletRequest request, HttpServletResponse response,	Exception failed) throws IOException {

		if (logger.isErrorEnabled()) {
			logger.error("Authorization Request failed: " + failed.toString(), failed);
		}
		response.sendError(HttpStatus.INTERNAL_SERVER_ERROR.value(), HttpStatus.INTERNAL_SERVER_ERROR.getReasonPhrase());
	}

	private static final class DefaultThrowableAnalyzer extends ThrowableAnalyzer {
		protected void initExtractorMap() {
			super.initExtractorMap();
			registerExtractor(ServletException.class, throwable -> {
				ThrowableAnalyzer.verifyThrowableHierarchy(throwable, ServletException.class);
				return ((ServletException) throwable).getRootCause();
			});
		}
	}
}
```

### OAuth2AuthorizationCodeGrantFilter
```text

0. WebAsyncManagerIntegrationFilter 
1. SecurityContextPersistenceFilter@6814 
2. HeaderWriterFilter
3. CsrfFilter
4. LogoutFilter 

5. OAuth2AuthorizationRequestRedirectFilter 

6. UsernamePasswordAuthenticationFilter 
7. RequestCacheAwareFilter 
8. SecurityContextHolderAwareRequestFilter 
9. AnonymousAuthenticationFilter 

10. OAuth2AuthorizationCodeGrantFilter 

11. SessionManagementFilter 
12. ExceptionTranslationFilter 
13. FilterSecurityInterceptor 

```

### authorizationRedirectStrategy : DefaultRedirectStrategy

### DefaultOauth2AuthorizationRequestResolver