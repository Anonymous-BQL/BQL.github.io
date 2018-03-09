## Spring security web认证流程

Authentication

 Authentication是一个接口，用来表示用户认证信息的，在用户登录认证之前相关信息会封装为一个Authentication具体实现类的对象，在登录认证成功之后又会生成一个信息更全面，包含用户权限等信息的Authentication对象，然后把它保存在SecurityContextHolder所持有的SecurityContext中，供后续的程序进行调用，如访问权限的鉴定等

### 1. SecurityContextPersistenceFilter

> 负责从SecurityContextRepository获取或存储SecurityContext,在web应用中，就是在请求到来时从session中取出securityContext，请求结束时，将securityContext保存到session中

### 2. LogoutFilter

> 监控一个实际为退出功能的URL，并且在匹配的时候完成用户的退出功能。

### 3. UsernamePasswordAuthenticationFilter

> 监控一个使用用户名和密码基于form认证的URL，并在URL匹配的情况下尝试认证该用户。

### 4. DefaultLoginPageGeneratingFilter

> 监控一个要进行基于forn或OpenID认证的URL，并生成展现登录form的HTML

### 5. BasicAuthenticationFilter

> 监控HTTP 基础认证的头信息并进行处理

### 6. RequestCacheAwareFilter

> 用于用户登录成功后，重新恢复由于登录而被打断的请求。

### 7. SecurityContextHolderAwareRequestFilter

> 用一个扩展了HttpServletRequestWrapper的子类,包装HttpServletRequest。它为请求处理器提供了额外的上下文信息

### 8. AnonymousAuthenticationFilter

> 如果用户到这一步还没有经过认证，将会为这个请求关联一个认证的token，标识此用户是匿名的。

### 9. SessionManagementFilter

> 根据认证的安全实体信息跟踪session，保证所有关联一个安全实体的session都能被跟踪到。

### 10.  ExceptionTranslationFilter

>  ExceptionTranslationFilter是用来处理来自AbstractSecurityInterceptor抛出的AuthenticationException和AccessDeniedException的。当ExceptionTranslationFilter捕获到的是AuthenticationException时将调用AuthenticationEntryPoint引导用户进行登录；如果捕获的是AccessDeniedException，但是用户还没有通过认证，则调用AuthenticationEntryPoint引导用户进行登录认证，否则将返回一个表示不存在对应权限的403错误码

### 11. AbstractSecurityInterceptor

> AbstractSecurityInterceptor 是Spring Security用于拦截请求进行权限鉴定的，其拥有两个具体的子类，拦截方法调用的 MethodSecurityInterceptor 和拦截URL请求的 FilterSecurityInterceptor。

### 12. SecurityContextHolder

> 根据三种不同的策略MODE_THREADLOCAL、MODE_INHERITABLETHREADLOCAL和MODE_GLOBAL 来保存SecurityContext web中使用第一个策略，将SecurityContext保存在ThreadLocal中

### 13. AuthenticationManager

> AuthenticationManager的默认实现是ProviderManager，而且它不直接自己处理认证请求，而是委托给其所配置的AuthenticationProvider列表，然后会依次使用每一个AuthenticationProvider进行认证，如果有一个AuthenticationProvider认证后的结果不为null，则表示该AuthenticationProvider已经认证成功，之后的AuthenticationProvider将不再继续认证。然后直接以该AuthenticationProvider的认证结果作为ProviderManager的认证结果。

### 14. AuthenticationProvider

> Spring Security内部的DaoAuthenticationProvider内部使用UserDetailsService来负责加载UserDetails，并将UserDetails封装到Authentication中

### 15. UserDetailsService

> 登录认证的时候Spring Security会通过UserDetailsService的loadUserByUsername()方法获取对应的UserDetails进行认证，认证通过后会将该UserDetails赋给认证通过的Authentication的principal，然后再把该Authentication存入到SecurityContext中。

