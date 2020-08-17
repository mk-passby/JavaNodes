---
title: springboot源码分析
date: 2019-06-22 16:40:16
tags: springcloud
---



# springboot项目的创建

- idea创建：File->New->Project...->Spring Initializr->Default:https://start.spring.io
- web创建：登录https://start.spring.io/  ，选择对应springboot版本，设置group和artifact下载就行\、

注：springboot项目默认没有加载web，需要自己导入maven依赖

```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```



# springboot启动

这里采用的是web下载方式，打开后，找到springboot启动类



```java
package com.mkpassby.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringbootApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootApplication.class, args);
	}

}

```



## springboot注解分析

### @SpringBootApplication

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
...
}
```

主要看有

- @SpringBootConfiguration：声明配置类
- @EnableAutoConfiguration：自动化配置
- @ComponentScan：包扫描



这里@EnableAutoConfiguration是通过将@Import导入AutoConfigurationImportSelector.class注入bean容器中

springboot中大量使用了@import注解：

### @Import注解介绍

@Import支持三种方式的导入：

1. 直接导入一个配置类或者Bean
2. 导入ImportSelector的实现类
3. 导入ImportBeanDefinitionRegistrar的实现类



```java
public class User {
    private String name="aaa";
    public String getName() {
        return name;  }  
    public void setName(String name) {
        this.name = name;  }}
```

```java
@Import(User.class)
public class App {

  public static void main(String[] args) {
    ConfigurableApplicationContext configurableApplicationContext= SpringApplication.run(App.class,args);
    System.out.println(configurableApplicationContext.getBean(User.class));
    System.out.println(configurableApplicationContext.getBean(User.class).getName());
    configurableApplicationContext.close();
  }
}
```

ImportSelector方法的实现，return new String[]｛“com.mkpassby.demo.User”｝

```java
public interface ImportSelector {
  String[] selectImports(AnnotationMetadata var1);
}

```



ImportBeanDefinitionRegistrar和ImportSelector类似，注册额外的bean

```JAVA
public class UserImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

  @Override
  public void registerBeanDefinitions(AnnotationMetadata annotationMetadata,
      BeanDefinitionRegistry beanDefinitionRegistry) {

    beanDefinitionRegistry.registerBeanDefinition("User",new RootBeanDefinition(User.class));
  }
}
```

 



## springboot启动分析

SpringBoot启动由一个main方法调入，由静态run方法启动

```java
@SpringBootApplication
public class SpringbootApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootApplication.class, args);
	}
}
```

run方法中new SpringApplication(),在构造器调用WebApplicationType.deduceFromClasspath()，判定当前应用类型，我这里用的是2.1.6.RELEASE的版本，这里可以看到有一个WebApplicationType.REACTIVE，这个是对webflux非阻塞web框架的支持。

```java
static WebApplicationType deduceFromClasspath() {
		if (ClassUtils.isPresent(WEBFLUX_INDICATOR_CLASS, null) && !ClassUtils.isPresent(WEBMVC_INDICATOR_CLASS, null)
				&& !ClassUtils.isPresent(JERSEY_INDICATOR_CLASS, null)) {
			return WebApplicationType.REACTIVE;
		}
		for (String className : SERVLET_INDICATOR_CLASSES) {
			if (!ClassUtils.isPresent(className, null)) {
				return WebApplicationType.NONE;
			}
		}
		return WebApplicationType.SERVLET;
	}
```

且在构造器中对所有包下的META-INF/spring.factories中配置的ApplicationContextInitializer和ApplicationListener实例化

```java
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
		this.resourceLoader = resourceLoader;
		Assert.notNull(primarySources, "PrimarySources must not be null");
		this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
		this.webApplicationType = WebApplicationType.deduceFromClasspath();
		setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
		setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
		this.mainApplicationClass = deduceMainApplicationClass();
	}
```





实例化SpringApplication完成后，调用run方法，实例化SpringApplicationRunListener，开启监听，准备数据等，其中核心在refreshContext(context)方法。在invokeBeanFactoryPostProcessors()方法中。该方法调用了PostProcessorRegistrationDelegate.invokeBeanFactoryPostProcessors(beanFactory, getBeanFactoryPostProcessors())方法，此处第二个传参getBeanFactoryPostProcessors()传过来的是

```properties
0 = {SharedMetadataReaderFactoryContextInitializer$CachingMetadataReaderFactoryPostProcessor@3037} 
1 = {ConfigurationWarningsApplicationContextInitializer$ConfigurationWarningsPostProcessor@3038} 
2 = {ConfigurationClassPostProcessor@3787} 
```



，这里是通过监听add到list中，此处具体实现需要进一步探究，最终通过postProcessBeanDefinitionRegistry进行BeanDefinition注册的处理。



```java
/**
	 * Build and validate a configuration model based on the registry of
	 * {@link Configuration} classes.
	 */
	public void processConfigBeanDefinitions(BeanDefinitionRegistry registry) {
		List<BeanDefinitionHolder> configCandidates = new ArrayList<>();
		String[] candidateNames = registry.getBeanDefinitionNames();

		for (String beanName : candidateNames) {
			BeanDefinition beanDef = registry.getBeanDefinition(beanName);
			if (ConfigurationClassUtils.isFullConfigurationClass(beanDef) ||
					ConfigurationClassUtils.isLiteConfigurationClass(beanDef)) {
				if (logger.isDebugEnabled()) {
					logger.debug("Bean definition has already been processed as a configuration class: " + beanDef);
				}
			}
            //获取所有标记了@Configuration注解类，封装成BeanDefinitionHolder集合
			else if (ConfigurationClassUtils.checkConfigurationClassCandidate(beanDef, this.metadataReaderFactory)) {
				configCandidates.add(new BeanDefinitionHolder(beanDef, beanName));
			}
		}

		// 没有 @Configuration 返回
		if (configCandidates.isEmpty()) {
			return;
		}

		// 根据@configuration中@Order排序
		configCandidates.sort((bd1, bd2) -> {
			int i1 = ConfigurationClassUtils.getOrder(bd1.getBeanDefinition());
			int i2 = ConfigurationClassUtils.getOrder(bd2.getBeanDefinition());
			return Integer.compare(i1, i2);
		});

		// Detect any custom bean name generation strategy supplied through the enclosing application context
		SingletonBeanRegistry sbr = null;
		if (registry instanceof SingletonBeanRegistry) {
			sbr = (SingletonBeanRegistry) registry;
			if (!this.localBeanNameGeneratorSet) {
				BeanNameGenerator generator = (BeanNameGenerator) sbr.getSingleton(CONFIGURATION_BEAN_NAME_GENERATOR);
				if (generator != null) {
					this.componentScanBeanNameGenerator = generator;
					this.importBeanNameGenerator = generator;
				}
			}
		}

		if (this.environment == null) {
			this.environment = new StandardEnvironment();
		}

		// Parse each @Configuration class
		ConfigurationClassParser parser = new ConfigurationClassParser(
				this.metadataReaderFactory, this.problemReporter, this.environment,
				this.resourceLoader, this.componentScanBeanNameGenerator, registry);

		Set<BeanDefinitionHolder> candidates = new LinkedHashSet<>(configCandidates);
		Set<ConfigurationClass> alreadyParsed = new HashSet<>(configCandidates.size());
		do {
			parser.parse(candidates);
			parser.validate();

			Set<ConfigurationClass> configClasses = new LinkedHashSet<>(parser.getConfigurationClasses());
			configClasses.removeAll(alreadyParsed);

			// Read the model and create bean definitions based on its content
			if (this.reader == null) {
				this.reader = new ConfigurationClassBeanDefinitionReader(
						registry, this.sourceExtractor, this.resourceLoader, this.environment,
						this.importBeanNameGenerator, parser.getImportRegistry());
			}
			this.reader.loadBeanDefinitions(configClasses);
			alreadyParsed.addAll(configClasses);

			candidates.clear();
			if (registry.getBeanDefinitionCount() > candidateNames.length) {
				String[] newCandidateNames = registry.getBeanDefinitionNames();
				Set<String> oldCandidateNames = new HashSet<>(Arrays.asList(candidateNames));
				Set<String> alreadyParsedClasses = new HashSet<>();
				for (ConfigurationClass configurationClass : alreadyParsed) {
					alreadyParsedClasses.add(configurationClass.getMetadata().getClassName());
				}
				for (String candidateName : newCandidateNames) {
					if (!oldCandidateNames.contains(candidateName)) {
						BeanDefinition bd = registry.getBeanDefinition(candidateName);
						if (ConfigurationClassUtils.checkConfigurationClassCandidate(bd, this.metadataReaderFactory) &&
								!alreadyParsedClasses.contains(bd.getBeanClassName())) {
							candidates.add(new BeanDefinitionHolder(bd, candidateName));
						}
					}
				}
				candidateNames = newCandidateNames;
			}
		}
		while (!candidates.isEmpty());

		// Register the ImportRegistry as a bean in order to support ImportAware @Configuration classes
		if (sbr != null && !sbr.containsSingleton(IMPORT_REGISTRY_BEAN_NAME)) {
			sbr.registerSingleton(IMPORT_REGISTRY_BEAN_NAME, parser.getImportRegistry());
		}

		if (this.metadataReaderFactory instanceof CachingMetadataReaderFactory) {
			// Clear cache in externally provided MetadataReaderFactory; this is a no-op
			// for a shared cache since it'll be cleared by the ApplicationContext.
			((CachingMetadataReaderFactory) this.metadataReaderFactory).clearCache();
		}
	}
```





继续跟进postProcessBeanDefinitionRegistry方法，后面调用了org.springframework.context.annotation.ConfigurationClassParser#doProcessConfigurationClass方法,这里对@Configuration类进行了解析，包括@PropertySource，@ComponentScan，@Import，@ImportResource等。

```java
protected final SourceClass doProcessConfigurationClass(ConfigurationClass configClass, SourceClass sourceClass)
			throws IOException {

		if (configClass.getMetadata().isAnnotated(Component.class.getName())) {
			// Recursively process any member (nested) classes first
			processMemberClasses(configClass, sourceClass);
		}

		// Process any @PropertySource annotations
		for (AnnotationAttributes propertySource : AnnotationConfigUtils.attributesForRepeatable(
				sourceClass.getMetadata(), PropertySources.class,
				org.springframework.context.annotation.PropertySource.class)) {
			if (this.environment instanceof ConfigurableEnvironment) {
				processPropertySource(propertySource);
			}
			else {
				logger.info("Ignoring @PropertySource annotation on [" + sourceClass.getMetadata().getClassName() +
						"]. Reason: Environment must implement ConfigurableEnvironment");
			}
		}

		// Process any @ComponentScan annotations
		Set<AnnotationAttributes> componentScans = AnnotationConfigUtils.attributesForRepeatable(
				sourceClass.getMetadata(), ComponentScans.class, ComponentScan.class);
		if (!componentScans.isEmpty() &&
				!this.conditionEvaluator.shouldSkip(sourceClass.getMetadata(), ConfigurationPhase.REGISTER_BEAN)) {
			for (AnnotationAttributes componentScan : componentScans) {
				// The config class is annotated with @ComponentScan -> perform the scan immediately
				Set<BeanDefinitionHolder> scannedBeanDefinitions =
						this.componentScanParser.parse(componentScan, sourceClass.getMetadata().getClassName());
				// Check the set of scanned definitions for any further config classes and parse recursively if needed
				for (BeanDefinitionHolder holder : scannedBeanDefinitions) {
					BeanDefinition bdCand = holder.getBeanDefinition().getOriginatingBeanDefinition();
					if (bdCand == null) {
						bdCand = holder.getBeanDefinition();
					}
					if (ConfigurationClassUtils.checkConfigurationClassCandidate(bdCand, this.metadataReaderFactory)) {
						parse(bdCand.getBeanClassName(), holder.getBeanName());
					}
				}
			}
		}

		// Process any @Import annotations
		processImports(configClass, sourceClass, getImports(sourceClass), true);

		// Process any @ImportResource annotations
		AnnotationAttributes importResource =
				AnnotationConfigUtils.attributesFor(sourceClass.getMetadata(), ImportResource.class);
		if (importResource != null) {
			String[] resources = importResource.getStringArray("locations");
			Class<? extends BeanDefinitionReader> readerClass = importResource.getClass("reader");
			for (String resource : resources) {
				String resolvedResource = this.environment.resolveRequiredPlaceholders(resource);
				configClass.addImportedResource(resolvedResource, readerClass);
			}
		}

		// Process individual @Bean methods
		Set<MethodMetadata> beanMethods = retrieveBeanMethodMetadata(sourceClass);
		for (MethodMetadata methodMetadata : beanMethods) {
			configClass.addBeanMethod(new BeanMethod(methodMetadata, configClass));
		}

		// Process default methods on interfaces
		processInterfaces(configClass, sourceClass);

		// Process superclass, if any
		if (sourceClass.getMetadata().hasSuperClass()) {
			String superclass = sourceClass.getMetadata().getSuperClassName();
			if (superclass != null && !superclass.startsWith("java") &&
					!this.knownSuperclasses.containsKey(superclass)) {
				this.knownSuperclasses.put(superclass, configClass);
				// Superclass found, return its annotation metadata and recurse
				return sourceClass.getSuperClass();
			}
		}

		// No superclass -> processing is complete
		return null;
	}
```

后续就是实例化所有的bean(Instantiate all remaining (non-lazy-init) singletons.),这里由于引入了spring-boot-starter-web依赖，特别说明下RequestMappingHandlerAdapter实例化。

## RequestMappingHandlerAdapter实例化

在实例化RequestMappingHandlerAdapter时会调用org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport#requestMappingHandlerAdapter

```java
@Bean
	public RequestMappingHandlerAdapter requestMappingHandlerAdapter() {
		RequestMappingHandlerAdapter adapter = createRequestMappingHandlerAdapter();
		adapter.setContentNegotiationManager(mvcContentNegotiationManager());
		adapter.setMessageConverters(getMessageConverters());
		adapter.setWebBindingInitializer(getConfigurableWebBindingInitializer());
		adapter.setCustomArgumentResolvers(getArgumentResolvers());
		adapter.setCustomReturnValueHandlers(getReturnValueHandlers());

		if (jackson2Present) {
			adapter.setRequestBodyAdvice(Collections.singletonList(new JsonViewRequestBodyAdvice()));
			adapter.setResponseBodyAdvice(Collections.singletonList(new JsonViewResponseBodyAdvice()));
		}

		AsyncSupportConfigurer configurer = new AsyncSupportConfigurer();
		configureAsyncSupport(configurer);
		if (configurer.getTaskExecutor() != null) {
			adapter.setTaskExecutor(configurer.getTaskExecutor());
		}
		if (configurer.getTimeout() != null) {
			adapter.setAsyncRequestTimeout(configurer.getTimeout());
		}
		adapter.setCallableInterceptors(configurer.getCallableInterceptors());
		adapter.setDeferredResultInterceptors(configurer.getDeferredResultInterceptors());

		return adapter;
	}
```
进入getMessageConverters方法中，这里注意会由configureMessageConverters()方法进入org.springframework.web.servlet.config.annotation.WebMvcConfigurationSupport#getMessageConverters

```java
@Override
	public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
		for (WebMvcConfigurer delegate : this.delegates) {
			delegate.configureMessageConverters(converters);
		}
	}
```





getMessageConverters方法，这里主要是从configureMessageConverters初始化messageConverters，以后后面的extendMessageConverters扩展messageCoverters。

```java
protected final List<HttpMessageConverter<?>> getMessageConverters() {
		if (this.messageConverters == null) {
			this.messageConverters = new ArrayList<>();
			configureMessageConverters(this.messageConverters);
			if (this.messageConverters.isEmpty()) {
				addDefaultHttpMessageConverters(this.messageConverters);
			}
			extendMessageConverters(this.messageConverters);
		}
		return this.messageConverters;
	}

```

这里如果需要扩展Http的请求扩展则可以查看类WebMvcConfigurer接口的注解说明，这里

主要看

``` JAVA
	/**
	 * Configure the {@link HttpMessageConverter HttpMessageConverters} to use for reading or writing
	 * to the body of the request or response. If no converters are added, a
	 * default list of converters is registered.
	 * <p><strong>Note</strong> that adding converters to the list, turns off
	 * default converter registration. To simply add a converter without impacting
	 * default registration, consider using the method
	 * {@link #extendMessageConverters(java.util.List)} instead.
	 * @param converters initially an empty list of converters
	 */
	default void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
	}

	/**
	 * A hook for extending or modifying the list of converters after it has been
	 * configured. This may be useful for example to allow default converters to
	 * be registered and then insert a custom converter through this method.
	 * @param converters the list of configured converters to extend.
	 * @since 4.1.3
	 */
	default void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
	}
```

## 扩展

这里贴出一段测试代码，可调整PostMapping中的produces和consumes查看变化



通过对HttpConvert的修改，我们的代码可以适配成我们想要的入参或者出参，当然，应用场景多用json格式，可以考虑自己封装json对应的转换器，去除掉springboot中默认的json转换器

```java
@Configuration
public class MyWebMvcConfig implements WebMvcConfigurer {
  @Override
  public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
    converters.add(new PropertiesToUserConverter());

  }
  @Override
  public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
    converters.add(new PropertiesToUserConverter());

  }

}
```

```java
/**
 * @program: springboot
 * @description:
 * @author: mk_passby
 * @create: 2019-06-25 22:19
 **/
public class PropertiesToUserConverter extends AbstractHttpMessageConverter<User> {

  public PropertiesToUserConverter() {
    super(MediaType.valueOf("application/properties+person"));
    setDefaultCharset(Charset.forName("UTF-8"));
  }

  @Override
  protected boolean supports(Class<?> aClass) {
    return aClass.isAssignableFrom(User.class);
  }

  //转换入参
  @Override
  protected User readInternal(Class<? extends User> aClass, HttpInputMessage httpInputMessage)
      throws IOException, HttpMessageNotReadableException {
    InputStream inputStream=httpInputMessage.getBody();
    Properties properties=new Properties();
    //请求内容properties转换为User对象
    properties.load(inputStream);
    User user=new User();
    user.setName(properties.getProperty("user.name"));
    return user;
  }

  /***
   * @param user
   * @param httpOutputMessage
   * @throws IOException
   * @throws HttpMessageNotWritableException
   */
  //用properties格式写出去
  @Override
  protected void writeInternal(User user, HttpOutputMessage httpOutputMessage)
      throws IOException, HttpMessageNotWritableException {
    OutputStream outputStream=httpOutputMessage.getBody();
    Properties properties=new Properties();
    properties.setProperty("user.name",user.getName());
    properties.store(new OutputStreamWriter(outputStream,getDefaultCharset()),"from web server");
  }
}
```

```JAVA
@RestController
public class RestControllerDemo {

  @PostMapping(
      value = "user/properties/to/json",
      produces = "application/properties+person",//出参类型Accept
      consumes = "application/properties+person"//入参类型Content-Type
  )
  public User userToProperties(@RequestBody User user) {
    return user;
  }
}
```

用postman模拟请求，结果如下

![1562769528306](springboot请求响应分析\1562769528306.png)

![1562769550293](springboot请求响应分析\1562769550293.png)

# springboot事件机制

### 设计模式

- 观察者模式

  - `java.util.Observable`发布者

  - `java.util.Observer`订阅者

    ```java
    package com.mk.demo;
    
    import java.util.Observable;
    import java.util.Observer;
    
    /**
     * @program: springcloud-demo
     * @description: 观察值demo
     * @author: mk_passby
     * @create: 2020-05-25 22:34
     **/
    public class ObserverDemo {
    
        public static void main(String[] args) {
            ObservableTest observable = new ObservableTest();
            observable.setChanged();
            observable.addObserver(new Observer() {
                @Override
                public void update(Observable o, Object arg) {
                    System.out.println(arg);
                }
            });
            observable.notifyObservers("Hello world");
        }
    
        public static class ObservableTest extends Observable {
    
            @Override
            public synchronized void setChanged() {
                super.setChanged();
            }
        }
    }
    
    ```

    

- 事件监听模式



### spring核心事件

- ApplictionEvent：应用事件
- ApplicationListener：应用监听器

```java
package com.mk.demo;

import org.springframework.context.ApplicationEvent;
import org.springframework.context.ApplicationListener;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

/**
 * @program: springcloud-demo
 * @description: spring事件监听demo
 * @author: mk_passby
 * @create: 2020-05-25 22:49
 **/
public class SpringEvenListenDemo {

    public static void main(String[] args) {
        //Annotation驱动的spring
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        //注册监听
        context.addApplicationListener(new ApplicationListener<ApplicationEventTest>() {
            @Override
            public void onApplicationEvent(ApplicationEventTest event) {
                System.out.println("onApplicationEvent:" + event.getSource());
            }
        });
        //发布事件
        context.refresh();
        context.publishEvent(new ApplicationEventTest("HELLO WORLD"));
        context.publishEvent(new ApplicationEventTest("HELLO 1"));
        context.publishEvent(new ApplicationEventTest("HELLO 2"));
        context.publishEvent(new ApplicationEventTest("HELLO 3"));
        context.publishEvent(new ApplicationEventTest("HELLO 4"));

    }

    private static class ApplicationEventTest extends ApplicationEvent {

        /**
         * Create a new ApplicationEvent.
         *
         * @param source the object on which the event initially occurred (never {@code null})
         */
        public ApplicationEventTest(Object source) {
            super(source);
        }
    }
}

```

### Springboot核心事件

- ApplicationEnvironmentPreparedEvent
- ApplicationPreparedEvent
- ApplicationStartedEvent
- ApplicationReadyEvent
- ApplicationFailedEvent

