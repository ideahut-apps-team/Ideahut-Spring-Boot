[__Ideahut Spring Boot__](./index.md) <img height="20" src="./assets/ideahut.png" alt=""> <img height="20" src="./assets/spring-boot.png" alt="">

# Advice

- Menangani semua error yang terjadi di aplikasi.
- Menangani body response sebelum dikirim ke klien.
- Sesuaikan dengan tipe spring boot yang digunakan, [WebMvc](https://docs.spring.io/spring-framework/reference/web/webmvc.html) atau [WebFlux](https://docs.spring.io/spring-framework/reference/web/webflux.html).

## WebMvc
``` java
@ControllerAdvice
class AppAdvice extends net.ideahut.springboot.advice.WebMvcAdvice {
	private final ApplicationContext applicationContext;
	private final AppProperties appProperties;
	private final AdminHandler adminHandler;
	@Autowired
	AppAdvice(
		ApplicationContext applicationContext,
		AppProperties appProperties,
		AdminHandler adminHandler
	) {
		this.applicationContext = applicationContext;
		this.appProperties = appProperties;
		this.adminHandler = adminHandler;
	}
	@Override
	protected boolean logAllError() {
		return !Boolean.FALSE.equals(appProperties.getLogAllError());
	}
    // error tidak akan ditangani jika path request ada di list yang didefinisikan di sini
	@Override
	protected Collection<String> exceptionSkipPaths() {
		StringSet skipPaths = new StringSet();
		skipPaths.add(adminHandler.getWebPath() + "/**");
		return skipPaths;
	}
    // body response akan langsung dikirim ke klien, tidak disusun lagi mengikuti standar framework
	@Override
	protected Collection<String> bodyWriteSkipPaths() {
		String actuatorBasePath = FrameworkHelper.getActuatorBasePath(applicationContext);
		StringSet skipPaths = new StringSet();
		ObjectHelper.runIf(!StringHelper.isBlank(actuatorBasePath), () -> skipPaths.add(actuatorBasePath + "/**"));
		return skipPaths;
	}
}
```

## WebFlux
``` java
@ControllerAdvice
class AppAdvice extends net.ideahut.springboot.advice.WebFluxAdvice {
	private final AppProperties appProperties;
	private final AdminHandler adminHandler;
	@Autowired
	AppAdvice(
		AppProperties appProperties,
		AdminHandler adminHandler
	) {
		this.appProperties = appProperties;
		this.adminHandler = adminHandler;
	}
	@Override
	protected boolean logAllError() {
		return !Boolean.FALSE.equals(appProperties.getLogAllError());
	}
    // error tidak akan ditangani jika path request ada di list yang didefinisikan di sini
	@Override
	protected Collection<String> exceptionSkipPaths() {
		StringSet skipPaths = new StringSet();
		skipPaths.add(adminHandler.getWebPath() + "/**");
		return skipPaths;
	}
}
```

##

[__Ideahut Spring Boot__](./index.md) <img height="20" src="./assets/ideahut.png" alt=""> <img height="20" src="./assets/spring-boot.png" alt="">
