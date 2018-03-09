## Spring 类型转换

### 1. Converter<S,T>

> 从S类型转换到T类型，属于具体类型的转换

### 2. ConverterFactory<S,R>

> 从S类型到R类型以及R类型的子类，属于类级别类型转换

### 3. GenericConverter

> 更为灵活的类型转换，可以从任意类型转换为某类型，不限制S

### 4. ConverterRegister

> 统一管理Converter与CoverterFactory，GenericConverter

### 5. ConversionService

> 可以实现用来整合ConverterRegister.

> mvc中可以用于注册所有Converter 

>> `<bean id="conversionService"
        class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="example.MyCustomConverter"/>
        </set>
    </property>
</bean>`

*Spring 会将所有的 Converter和 Register，Factory转换为 GenericCoverter， 使用 Service管理*
