#ldap-java-appsys-intergrate.md

## springboot itergrate ldap
https://spring.io/guides/gs/authenticating-ldap/

public void configure(AuthenticationManagerBuilder auth) throws Exception {
    auth
      .ldapAuthentication()
        .userDnPatterns("uid={0},ou=people")
        .groupSearchBase("ou=groups")
        .contextSource()
          .url("ldap://localhost:8389/dc=springframework,dc=org")
          .and()
        .passwordCompare()
          .passwordEncoder(new BCryptPasswordEncoder())
          .passwordAttribute("userPassword");
  }



## springmvc intergrate ldap