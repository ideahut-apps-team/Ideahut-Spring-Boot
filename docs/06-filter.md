[__Ideahut Spring Boot__](./index.md) <img height="20" src="./assets/ideahut.png" alt=""> <img height="20" src="./assets/spring-boot.png" alt="">

# Filter

Http request filter.

## Bean

``` java
// WebMvc
@Bean
FilterRegistrationBean<WebMvcRequestFilter> defaultRequestFilter(
    Environment environment,
    AppProperties appProperties,
    RequestMappingHandlerMapping requestMappingHandlerMapping
) {	
    FilterDefinition filter = ObjectHelper.useOrDefault(
        appProperties.getFilter(), 
        FilterDefinition::new
    );
    return WebMvcHelper.createFilterBean(
        environment,
        new WebMvcRequestFilter()
        .setHeaders(filter.getHeaders())
        .setResultTimeEnable(filter.getResultTimeEnable())
        .setResultTimeUnit(filter.getResultTimeUnit())
        .setHandlerMapping(requestMappingHandlerMapping)
        .setTraceEnable(filter.getTraceEnable())
        .setTraceKey(filter.getTraceKey())
        .initialize(), 
        1, 
        "/*"
    );
}

// WebFlux
@Bean
WebFluxRequestFilter defaultRequestFilter(
    AppProperties appProperties,
    RequestMappingHandlerMapping requestMappingHandlerMapping,
    RootRequestInterceptor rootRequestInterceptor,
    AdminRequestInterceptor adminRequestInterceptor
) {
    FilterDefinition filter = ObjectHelper.useOrDefault(
        appProperties.getFilter(), 
        FilterDefinition::new
    );
    return new WebFluxRequestFilter()
    .setAllowPaths("/**")
    .setHandlerMapping(requestMappingHandlerMapping)
    .setHeaders(filter.getHeaders())
    .setInterceptors(rootRequestInterceptor, adminRequestInterceptor)
    //.setPathMatcher(null)
    .setResultTimeEnable(filter.getResultTimeEnable())
    .setResultTimeUnit(filter.getResultTimeUnit())
    //.setSkipPaths(null)
    .setTraceEnable(filter.getTraceEnable())
    //.setTraceGenerator(null)
    .setTraceKey(filter.getTraceKey())
    ;
}
```

- `setAllowPaths`: path yang di-_filter_.
- `setHeaders`: headers yang akan disertakan di response, bisa berguna untuk CORS.
- `setHandlerMapping`: untuk mendapatkan informasi request mapping.
- `setTraceEnable`: log [MDC](https://logback.qos.ch/manual/mdc.html) diaktifkan atau tidak.
- `setTraceGenerator`: log [MDC](https://logback.qos.ch/manual/mdc.html) trace id generator.
- `setTraceKey`: key yang digunakan di log [MDC](https://logback.qos.ch/manual/mdc.html).
- `setResultTimeEnable`: informasi waktu eksekusi di response result.
- `setResultTimeUnit`: satuan waktu yang digunakan, contoh: SECONDS, MILLISECONDS, dll.

### CORS

``` md
cors:
    "Access-Control-Allow-Credentials": "true"
    "Access-Control-Allow-Origin": "*"
    "Access-Control-Allow-Methods": "*"
    "Access-Control-Max-Age": "360"
    "Access-Control-Allow-Headers": "*"
    "Access-Control-Expose-Headers": "*"
```

##

[__Ideahut Spring Boot__](./index.md) <img height="20" src="./assets/ideahut.png" alt=""> <img height="20" src="./assets/spring-boot.png" alt="">
