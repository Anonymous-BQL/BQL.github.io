## Spring MVC管控流程

*请求经过的流程*

### 1.Filter

*基于servlet filter的内建filter*

### 2.HandleMapping

*根据策略调用不同的 handlemapping 来映射请求获取请求相关的 controller 和 intercepter*

### 3.Intercepter

*HandlerIntercepter，拦截指定 URL 或者 controller 的请求，进行处理*

### 4.before controller

> * 如果是 ViewController，则调用相应的 viewcontroller 进行处理

> * 如果是 method controller：

>> 1. 调用 MessageConverter 转换HTTP body数据

>> 2. Converters 转换HTTP header，url数据

>> 3. 调用InitBind，进行请求数据到model的绑定

>> 4. 将数据转换为 controller 参数

>> 4. 调用 validation 进行参数验证

>> 5. 若转换出现异常，则将异常保存到Errors中

> * 如果是静态资源：

>> 1. 若资源存在，则返回资源

>> 2. 资源不存在且配置了 defaultServlet 则调用容器的 defaultServlet 进行处理

### 5.Controller

> 1.进行请求相应，可以返回和接受任意类型，通过不通过类型参数可以得到需要的数据对象

> 2.调用相应的 ControllerAdvice 进行处理

> 3.若处理出现异常，则调用 HandlerExceptionResolver Bean 或者 @ExceptionHandler 方法，当使用 Rest时 可以使用全局的 ResponseEntityExceptionHandler Bean

> 4.出现异常时，Spring会根据异常的类型对Response status 进行注入

### 6.Translator

*如果需要返回视图，Translator 翻译 Controller 的返回值，不同返回值可能拥有不同的 Translator*

### 7.ViewResolvers

*在 Translator 之后进行处理，也可能在 ContentNegotiatingViewConverter之后处理，需要时，返回视图*

### 8.Response

*经过该请求所对应的所有的 MessageConverters, Intercepter, Filter，并响应客户端*

## 相关问题

> * 多个 Handlermapping 与多个 ViewResolver，通过 order 属性指定优先级，或者通过
  ContentNegotiatingViewConverter 根据客户端Accept类型进行选择





