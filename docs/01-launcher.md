[__Ideahut Spring Boot__](./index.md) <img height="20" src="./assets/ideahut.png" alt=""> <img height="20" src="./assets/spring-boot.png" alt="">

# Launcher

- Class yang menjadi titik awal untuk menjalankan aplikasi.
- Sesuaikan dengan tipe spring boot yang digunakan, [WebMvc](https://docs.spring.io/spring-framework/reference/web/webmvc.html) atau [WebFlux](https://docs.spring.io/spring-framework/reference/web/webflux.html).

## WebMvc
``` java
@SpringBootApplication
public class Application extends net.ideahut.springboot.launcher.WebMvcLauncher {
	public static class Package {
		private Package() {}
		public static final String LIBRARY		= FrameworkHelper.PACKAGE;
		public static final String APPLICATION	= "net.ideahut.springboot.template";
	}
	private static boolean ready = false;
	private static void setReady(boolean b) { ready = b; }
	public static boolean isReady() { return ready; }
	
	public static void main(String... args) {
		WebLauncher.runApp(Application.class, args);
	}
	// mendapatkan definisi dari konfigurasi
	@Override
	public LauncherDefinition onDefinition(ApplicationContext applicationContext) {
		return FrameworkHelper.getBean(applicationContext, AppProperties.class).getLauncher();
	}
	// inisialisasi dan konfigurasi bean telah selesai dan sukses
	@Override
	public void onReady(ApplicationContext applicationContext) {
		setReady(true);
		NativeConfig.registerToNativeImageAgent(applicationContext);
	}
	// error launcher
	@Override
	public void onError(ApplicationContext applicationContext, Throwable throwable) {
		log.error("Application", throwable);
		System.exit(0);
	}
	// log dari proses launcher
	@Override
	public void onLog(
		LauncherDefinition.Log.Type type, 
		LauncherDefinition.Log.Level level, 
		String message, 
		Throwable throwable
	) {
		level = ObjectHelper.useOrDefault(level, () -> LauncherDefinition.Log.Level.DEBUG);
		log.atLevel(org.slf4j.event.Level.valueOf(level.name())).log(message, throwable);
	}
	@Override
	protected Class<? extends WebLauncher> source() {
		return Application.class;
	}
	
}
```

## WebFlux
``` java
@SpringBootApplication
public class Application extends net.ideahut.springboot.launcher.WebFluxLauncher {
	public static class Package {
		private Package() {}
		public static final String LIBRARY		= FrameworkHelper.PACKAGE;
		public static final String APPLICATION	= "net.ideahut.springboot.template";
	}
	private static boolean ready = false;
	private static void setReady(boolean b) { ready = b; }
	public static boolean isReady() { return ready; }
	
	public static void main(String... args) {
		WebLauncher.runApp(Application.class, args);
	}
	// mendapatkan definisi dari konfigurasi
	@Override
	public LauncherDefinition onDefinition(ApplicationContext applicationContext) {
		return FrameworkHelper.getBean(applicationContext, AppProperties.class).getLauncher();
	}
	// inisialisasi dan konfigurasi bean telah selesai dan sukses
	@Override
	public void onReady(ApplicationContext applicationContext) {
		setReady(true);
		NativeConfig.registerToNativeImageAgent(applicationContext);
	}
	// error launcher
	@Override
	public void onError(ApplicationContext applicationContext, Throwable throwable) {
		log.error("Application", throwable);
		System.exit(0);
	}
	// log dari proses launcher
	@Override
	public void onLog(
		LauncherDefinition.Log.Type type, 
		LauncherDefinition.Log.Level level, 
		String message, 
		Throwable throwable
	) {
		level = ObjectHelper.useOrDefault(level, () -> LauncherDefinition.Log.Level.DEBUG);
		log.atLevel(org.slf4j.event.Level.valueOf(level.name())).log(message, throwable);
	}
}
```
## Properties
- Class 'net.ideahut.springboot.definition.LauncherDefinition'.
```md
launcher:
    # Tunggu semua proses inisialisasi & konfigurasi sampai selesai
	waitAllProcess: true
	
	# menjalankan fitur yang ada di 'net.ideahut.springboot.init.InitHandler'
	init:
	    # meregister obyek-obyek yang digunakan ke dalam mapper
		mapper: true
		# eksekusi 'jakarta.validation.Validator'
		validation: true
		# inisialisasi endpoint (warmup)
		endpoint: true
	
	# daftar log yang diaktifkan
	log:
		initHandler: true
		beanInitialize: true
		beanConfigure: true
		beanShutdown: true
		version: true
		application: true
	
	# Konfigurasi bean
	configure:
	    # max thread untuk BeanConfigure yang terdeteksi secara otomatis dari spring context (selain yang didefinisikan di beans)
		concurrency: 0
		# daftar bean yang dikonfigurasi duluan secara berurutan, karena ada kemungkinan dibutuhkan oleh bean-bean yang lain
		beans:
			- net.ideahut.springboot.entity.EntityTrxManager
			- net.ideahut.springboot.sysparam.SysParamHandler
			- net.ideahut.springboot.audit.AuditHandler
```

##

[__Ideahut Spring Boot__](./index.md) <img height="20" src="./assets/ideahut.png" alt=""> <img height="20" src="./assets/spring-boot.png" alt="">
