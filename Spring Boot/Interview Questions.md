### 1. How to configure OAuth2 in spring boot application?

#### **Steps to Configure OAuth2 in a Spring Boot Application**

#### 1. **Add Dependencies**

In your `pom.xml` (for Maven) or `build.gradle` (for Gradle), you need to add the necessary dependencies for **Spring Security** and **OAuth2 Client**.

##### **For Maven**:

```
<dependencies>
    <!-- Spring Boot Starter Web for web-based applications -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot Starter Security for OAuth2 support -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-oauth2-client</artifactId>
    </dependency>

    <!-- Optional: Thymeleaf for templates -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>

    <!-- For testing -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>

```


#### 2. **Configure OAuth2 Client Properties**

Spring Boot uses application properties or YAML to configure OAuth2. You’ll need to add the client credentials (client ID and secret) and provider information (e.g., Google's OAuth2 endpoints) to your `application.properties` or `application.yml`.

##### **For Google OAuth2** (in `application.properties`):

```
# OAuth2 client settings for Google
spring.security.oauth2.client.registration.google.client-id=YOUR_GOOGLE_CLIENT_ID
spring.security.oauth2.client.registration.google.client-secret=YOUR_GOOGLE_CLIENT_SECRET
spring.security.oauth2.client.registration.google.scope=profile,email
spring.security.oauth2.client.registration.google.redirect-uri={baseUrl}/login/oauth2/code/{registrationId}
spring.security.oauth2.client.registration.google.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.google.client-name=google
spring.security.oauth2.client.provider.google.token-uri=https://oauth2.googleapis.com/token
spring.security.oauth2.client.provider.google.authorization-uri=https://accounts.google.com/o/oauth2/v2/auth
spring.security.oauth2.client.provider.google.user-info-uri=https://www.googleapis.com/oauth2/v3/userinfo
spring.security.oauth2.client.provider.google.jwk-set-uri=https://www.googleapis.com/oauth2/v3/certs

```

##### **For YAML (`application.yml`)**:

```
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: YOUR_GOOGLE_CLIENT_ID
            client-secret: YOUR_GOOGLE_CLIENT_SECRET
            scope: profile, email
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
            authorization-grant-type: authorization_code
            client-name: google
        provider:
          google:
            token-uri: https://oauth2.googleapis.com/token
            authorization-uri: https://accounts.google.com/o/oauth2/v2/auth
            user-info-uri: https://www.googleapis.com/oauth2/v3/userinfo
            jwk-set-uri: https://www.googleapis.com/oauth2/v3/certs

```

#### 3. **Set Up Spring Security Configuration**

To enable OAuth2 login in Spring Security, you'll need to configure it in your security configuration class.

Create a class like `SecurityConfig.java` and annotate it with `@Configuration` and `@EnableWebSecurity`. The `HttpSecurity` object will configure OAuth2 login and logout behavior.

```
package com.example.demo.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/", "/home", "/login", "/error").permitAll() // Allow public access to these URLs
                .anyRequest().authenticated()  // Require authentication for any other URLs
            .and()
            .oauth2Login()  // Enable OAuth2 Login
                .loginPage("/login")  // Custom login page if needed
            .and()
            .logout()
                .permitAll();  // Allow logout for all
    }
}

```

#### 4. **Create a Controller for OAuth2 Login Flow**

Create a controller to handle the login and the user profile page after authentication. You can display the user's details by calling the `OAuth2AuthenticationToken`.

```
package com.example.demo.controller;

import org.springframework.security.core.Authentication;
import org.springframework.security.oauth2.core.user.OAuth2User;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home() {
        return "home";  // Home page (publicly accessible)
    }

    @GetMapping("/login")
    public String login() {
        return "login";  // Custom login page (if needed)
    }

    @GetMapping("/profile")
    public String profile(Authentication authentication, Model model) {
        OAuth2User user = (OAuth2User) authentication.getPrincipal();
        model.addAttribute("user", user);
        return "profile";  // Show user profile after login
    }
}

```


#### 5. **Configure Google OAuth2 Credentials**

To integrate with Google's OAuth2 service, you need to create OAuth2 credentials on the Google Developer Console:

1. Visit Google Cloud Console.
2. Go to **APIs & Services > Credentials**.
3. Create a new OAuth 2.0 **Client ID** under **OAuth 2.0 Client IDs**.
4. Configure the **Authorized redirect URI** as: `http://localhost:8080/login/oauth2/code/google`.
5. After creating the credentials, copy the **Client ID** and **Client Secret**, and add them to your `application.properties` or `application.yml`.


---

### **Conclusion**

By following these steps, you can easily configure OAuth2 authentication in your Spring Boot application using Google as an identity provider. The Spring Security OAuth2 support takes care of the OAuth2 flow for you, allowing for smooth integration of third-party authentication mechanisms like Google, Facebook, GitHub, and others. This configuration ensures that your Spring Boot application can authenticate users securely and access their basic information in a simplified manner.

You can extend this setup for other OAuth2 providers by modifying the `application.properties` and adding different OAuth2 configurations for each provider.

