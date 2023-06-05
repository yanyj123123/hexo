---
title: 瑞吉外卖笔记
categories: 
 -java
---

___

## 所遇到的问题

### 问题一：LONG类型精度丢失

由于id是long类型，所以当根据id删除或者更新时候，由前端js传入的id值精度缺失(js的long类型长度为16位，java是18位)，所以导致后端查询不到数据。

<!-- more -->

**解决**：使用对象映射器:基于jackson将Java对象转为json，或者将json转为Java对象然后将long类型统一转为字符串类型

**具体实现**：

###### 方法一

1.增加JacksonObjectMapper配置类

```java
/**
 * 对象映射器:基于jackson将Java对象转为json，或者将json转为Java对象
 * 将JSON解析为Java对象的过程称为 [从JSON反序列化Java对象]
 * 从Java对象生成JSON的过程称为 [序列化Java对象到JSON]
 */
public class JacksonObjectMapper extends ObjectMapper {

    public static final String DEFAULT_DATE_FORMAT = "yyyy-MM-dd";
    public static final String DEFAULT_DATE_TIME_FORMAT = "yyyy-MM-dd HH:mm:ss";
    public static final String DEFAULT_TIME_FORMAT = "HH:mm:ss";

    public JacksonObjectMapper() {
        super();
        //收到未知属性时不报异常
        this.configure(FAIL_ON_UNKNOWN_PROPERTIES, false);

        //反序列化时，属性不存在的兼容处理
        this.getDeserializationConfig().withoutFeatures(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);
        SimpleModule simpleModule = new SimpleModule()
                .addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_TIME_FORMAT)))
                .addDeserializer(LocalDate.class, new LocalDateDeserializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_FORMAT)))
                .addDeserializer(LocalTime.class, new LocalTimeDeserializer(DateTimeFormatter.ofPattern(DEFAULT_TIME_FORMAT)))

                .addSerializer(BigInteger.class, ToStringSerializer.instance)
                .addSerializer(Long.class, ToStringSerializer.instance)
                .addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_TIME_FORMAT)))
                .addSerializer(LocalDate.class, new LocalDateSerializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_FORMAT)))
                .addSerializer(LocalTime.class, new LocalTimeSerializer(DateTimeFormatter.ofPattern(DEFAULT_TIME_FORMAT)));

        //注册功能模块 例如，可以添加自定义序列化器和反序列化器
        this.registerModule(simpleModule);
    }
}

```

2.扩展mvc框架的消息转换器webMvcConfig类

```java
@Configuration
public class WebMvcConfig extends WebMvcConfigurationSupport {
     /**
     * 设置静态资源映射，若添加静态资源映射，可能会导致静态资源无法访问
     * @param registry
     */
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        log.info("开始进行静态资源映射...");
        registry.addResourceHandler("/static/backend/**").addResourceLocations("classpath:/static/backend/");
        registry.addResourceHandler("/static/front/**").addResourceLocations("classpath:/static/front/");
        registry.addResourceHandler("/**")
                .addResourceLocations("classpath:/resources/")
                .addResourceLocations("classpath:/static/");
    }
    /**
    * @Author: 滚筒洗衣机
    * @Description: 扩展mvc框架的消息转换器
    * @DateTime: 2023-6-1 18:04
    * @Params:
    * @Return
    */
    @Override
    protected void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        //创建新的消息转换器
        MappingJackson2HttpMessageConverter messageConverter=new MappingJackson2HttpMessageConverter();
        //设置对象转换器，底层使用jackson将java对象转为json
        messageConverter.setObjectMapper(new JacksonObjectMapper());
        converters.add(0,messageConverter);
    }
}

```

###### 方法二

在实体类相应字段如id字段上加

```java
@JsonFormat(shape=JsonFormat.Shape.STRING)
```

### 二、如何在url中携带参数

使用场景：如编辑时将id携带到编辑界面然后进行更新

解决：使用注解@Pathvariable获取url中的参数，而接口需要使用类似于@Gettmapping("/{id}")的括号表达式
