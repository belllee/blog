---
title: ZUUL路由服务遇到的坑
date: 2018-02-26 14:16:22
tags: [zuul,java]
---

项目采用Spring cloud微服务框架，使用ZUUL作为路由服务，在使用过程中遇到了如下问题，记录下来供大家借鉴。
## 1、关于跨域
API需要提供给其他项目使用，由于服务通过zuul，所以zuul需要支持跨域访问。
解决办法：
增加跨域过滤器即可
```java
@Bean
    public CorsFilter corsFilter() {
        final UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        final CorsConfiguration config = new CorsConfiguration();
        config.setAllowCredentials(true);
        config.addAllowedOrigin("*");
        config.addAllowedHeader("*");
        config.addAllowedMethod("*");
        config.setMaxAge(3600L);
        source.registerCorsConfiguration("/**", config);
        return new CorsFilter(source);
    }
```
## 2、文件上传中文名乱码
使用form上传文件，直接post到服务，文件名中文读取正常。但是通过zuul中转后，文件名变成乱码。
解决办法：
1、注意编码，全站api和前端全部要使用utf-8编码。zuul中强制编码为utf-8，参数配置如下：
```yaml
spring:
  http:
    encoding:
      charset: UTF-8
      enabled: true
      force: true
```
2、修改nginx路由设置，在原来的api地址前，统一增加zuul。因为默认上传文件是通过服务自己的controller来处理，增加zuul前缀后，通过zuul servlet来处理，避免了多次跳转，和引入编码错误。nginx配置举例：
```
    location /api/ {
                proxy_pass   http://localhost:9999/zuul/api/;
                proxy_redirect  http://localhost:9999/zuul/api/ /api/;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header REMOTE-HOST $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
                client_max_body_size 500m;
                proxy_connect_timeout 60s;
                proxy_read_timeout 600s;
                proxy_send_timeout 600s;
            }
```
3、增加zuul前缀后会导致header中出现Access-Control-Allow-Credentials等属性重复的异常，需要在zuul中设置header忽略。参数配置如下
```yaml
zuul:
  sensitiveHeaders: Authorization
  ignored-headers: Access-Control-Allow-Credentials,Access-Control-Allow-Origin,Vary,X-Frame-Options
```
