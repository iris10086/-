# Druid自定义配置

先覆盖DataSource

## 后台监控

```java
@Bean
public ServletRegistrationBean statViewServlet() {
    ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
    // 添加IP白名单
    servletRegistrationBean.addInitParameter("allow", "127.0.0.1");
    // 添加IP黑名单，当白名单和黑名单重复时，黑名单优先级更高
    servletRegistrationBean.addInitParameter("deny", "192.168.25.123");
    // 添加控制台管理用户
    servletRegistrationBean.addInitParameter("loginUsername", "druid");
    servletRegistrationBean.addInitParameter("loginPassword", "123456");
    // 是否能够重置数据
    servletRegistrationBean.addInitParameter("resetEnable", "false");
    return servletRegistrationBean;
}
```

