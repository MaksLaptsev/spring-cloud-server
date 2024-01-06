## Простенький spring cloud config server
[application.yml](src/main/resources/application.yml)
```
server:
  port: 8888
spring:
  application:
    name: config-service
  cloud:
    config:
      server:
        git:
          uri: https://github.com/DiabloElf/spring-boot-configs.git
          default-label: master
          skip-ssl-validation: true
          search-paths: "{application}"
```

- git:uri - адрес [репозитория](https://github.com/DiabloElf/spring-boot-configs.git), где лежат нужные конфиги
- default-label: - ветка, с которой будут браться конфиги

## На клиентской части:
- Необходимо добавить зависимость ```implementation 'org.springframework.cloud:spring-cloud-starter-config:4.1.0'```
- application.yml на клиенте:
```
spring:
  application:
    name: SpringBootHW
  profiles:
    active: dev
server:
  port: 8080
---

spring:
  config:
    activate:
      on-profile: dev
    import: optional:configserver:http://localhost:8888/
  cloud:
    config:
      fail-fast: true
```

- name: SpringBootHW - будет именем клиентского сервиса, по этому имени будет идти поиск папки с нужным конфигом в удаленном репозитории
- profiles: active: dev - текущий профиль, согласно которому будут искаться конфиги
- config: import: `optional:configserver:http://localhost:8888/` - адрес spring cloud config server , от которого будут получены необходимые конфиги

- Делалось по этой статье [хабр](https://habr.com/ru/articles/764402/)
