## Struts 篇

#### Struts2 Interview Questions and Answers 

1. 简单介绍下Struts2 ?
 
  Apache Struts2的是一个开源框架，在Java中构建Web应用程序。 Struts2的基于OpenSymphony的WebWork的框架。它的高度从Struts1的改进，这使得它更灵活，易于使用和延伸。 Struts2的核心组件是Action，Interceptors和 Result pages。 Struts2提供了许多方法来创建Action类，并通过struts.xml中或通过注释配置。我们可以创造我们自己的常见任务的拦截器。 Struts2的自带了很多的标签和使用OGNL表达式语言。我们可以创造我们自己的类型转换器渲染结果页面。结果页面可以使用JSP和FreeMarker的模板。
  
2.  Struts1 and Struts2 区别，那个更好?
 	
 	<table border=1>
		<thead>
		<tr>
		<th style="text-align:left">组件</th>
		<th style="text-align:left">Struts1</th>
		<th style="text-align:left">Struts2</th>
		</tr>
		</thead>
		<tbody>
		<tr>
		<td style="text-align:left">Action 类</td>
		<td style="text-align:left">
				Struts1 action 类  继承了一个 Abstract Class ，使得的它不具备扩展性。 		
		</td>
		<td style="text-align:left">
			Struts2 action classes 是灵活的 ，我们可以通过implementing Action 接口 和  extending ActionSupport class 去创建它 , 或者仅仅是使用它的 execute() 方法.
		</td>
		</tr>
		<tr>
		<td style="text-align:left">线程安全</td>
		<td style="text-align:left">Struts1 Action 类是单例的 ，非线程安全的,这使得开发人员在处理多线程问题上格外小心.以免其他问题产生。</td>		<td style="text-align:left">Struts2 action 类为每一个request请求获取一个实例，因此没有多线程问题，线程安全的.</td>	</tr>
		<tr>
		<td style="text-align:left">Servlet API 耦合度</td>
		<td style="text-align:left">Struts1 APIs 与 Servlet API， Request、 Response对象是 高耦合的 ，通过调用 action类的 execute() 方法</td>
		<td style="text-align:left">
		Struts2 API 是低耦合的对于 Servlet API 和自动化映射表单bean数据到action class通过 java bean 属性. 如果我们想要使用Servlet API 类, 这里也提供了相应的接口
	</td>
		</tr>
		<tr>
		<td style="text-align:left">易测性</td>
		<td style="text-align:left">Struts1 action 类依赖于Servlet API ,所以不利于测试</td>
		<td style="text-align:left">Struts2 Action 类像标准Java类一样，所以我们可以通过它的实例及对属性设值很轻松的测试</td>
		</tr>
		<tr>
		<td style="text-align:left">Request 参数映射</td>
		<td style="text-align:left">
		Struts1要求我们创建的ActionForm类来绑定请求参数，并且我们需要对它进行配置Struts配置文件
	</td>
		<td style="text-align:left">
		Struts2 request 参数映射可以在action 类的Java Bean 属性上完成，或实现模型驱动接口，提供用于映射Java bean类的名称。
	</td>
		</tr>
		<tr>
		<td style="text-align:left">Tag 标签支持</td>
		<td style="text-align:left">Struts1 使用JSTL标签因此是有限的</td>
		<td style="text-align:left">
		Struts2的OGNL使用，并提供不同类型的用户界面，Control和Data标签。它更灵活和易于使用。
		</td>
		</tr>
		<tr>
		<td style="text-align:left">验证</td>
		<td style="text-align:left">Struts1支持validation通过手动的写validate()方法</td>
		<td style="text-align:left">Struts2 支持包括手动validation以及集成了 Validation框架.</td>
		</tr>
		<tr>
		<td style="text-align:left">Views 渲染</td>
		<td style="text-align:left">Struts1使用标准的JSP技术JSP页面填充值。</td>
		<td style="text-align:left">Struts2 使用 ValueStack 去存储request参数和属性 ，我们可以使用OGNL以及 Struts2标签去访问它.</td>
		</tr>
		<tr>
		<td style="text-align:left">Modules 支持</td>
		<td style="text-align:left">Struts1 modules 是一个复杂的设计并且看起来像一个单独的项目</td>
		<td style="text-align:left">Struts2 提供 “namespace” 配置对packages简单的去创建modules.</td>
		</tr>
		</tbody>
	</table>

	3. Struts2 核心组件?
 		
   Struts2 核心组件:
   * Action 类
   * 拦截器
   * Result 页, JSP of FreeMarker 模版
   * 值栈, OGNL and Tag 库
   
 
 	4.  怎样理解Struts2的拦截器?
I拦截器是Struts2框架的主干. Struts2的拦截器是负责大部分的框架，比如通过请求参数到action类，使用Servlet API的request，response ，可用的session到Action类，验证，i18n支持等

actionInvocation 负责把Action类装入拦截器 ，去释放它. 使用 ActionInvocation最重要的方法就是invoke() 方法 ，一直跟踪拦截器链到执行下一个拦截器或action. This is one of the best example of Chain 它就是Java EE 框架中责任模式的最好例子.
 
	 5. 选择一种设计模式去实现Struts2的拦截器?
Struts2的拦截器都是基于filters的设计模式。在拦截器栈拦截器的调用非常类似于责任设计链模式。
 
	 6. 在 Struts2中有哪几种创建Action classes的方式?
 * 实现 Action接口
 * 使用 Struts2 @Action 注解
 * extending ActionSupport  类
 * 普通的Java类的execute（）方法返回字符串可以被配置为Action类 .
 
 
	 7.  Struts2 action 和 interceptors 都是线程安全的?
 Struts2 Action 类是线程安全的 因为针对每一个request请求操作都拥有自己的实例。.
Sstruts2 interceptors 是单例的, 因此它不是线程安全的，我们需要在实现的时候就要关心避免与共享数据的任何问题.
 
	 8. 哪个类是Struts2的前置控制器?
 Org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter  在每一次处理request请求的开始都要经过这个类，所以它是 Struts2的前置控制器. 早期版本使用的是 org.apache.struts2.dispatcher.FilterDispatcher 作为struts2的前置控制类.
 
	 9. Struts2使用拦截器的好处?
 
一些的拦截器的好处是:
 * 拦截器在实现分离关注点上起到了至关重要的作用
 * Struts2 拦截器是可配置的, 我们能配置到任何一个我们想要拦截的action类.
 * 我们可以创建自定义拦截器去实现一些通用的任务，request 参数日志记录，认证等.
 * 这有助于我们在在单个位置照顾共同的任务，实现了维护成本低.
 * 我们能创建拦截器栈给不同的actions使用
 
 
	 10. 简单介绍下值栈和OGNL?
 值栈是其中应用数据被用于处理客户机请求保存由Struts2的存储区域。数据被存储在使用ThreadLocal的具有特定于特定请求线程值ActionContext中的对象。
O
对象图导航语言（OGNL）是用于操作存储在值栈数据的强大的表达式语言。正如你在架构图看，无论是拦截器和结果页面可以访问存储在使用OGNL ValueStack中的数据。
 
 	11. 介绍下 Struts2一些重要的注解?
 Struts2 中重要的注解:
* @Action to  创建Action 类
* @Actions  可以配置单个Action类去操作多个Action类
* @Namespace and @Namespaces  创建不同的模块
* @Result  返回结果页面
* @ResultPath  配置结果页本地路径

 
 	12.提供您已经使用了一些重要的Struts2常量?

1提供您已经使用了一些重要的Struts2常量:
s. struts.devMode to run our application in development mode. This mode does reload properties files and provides extra logging and debugging feature. It’s very useful while developing our application but we should turn it off while moving our code to production.
2. struts.convention.result.path to configure the location of result pages. By default Struts2 look for result pages at {WEBAPP-ROOT}/{Namespace}/ and we can change the location with this constant.
3. struts.custom.i18n.resources to define global resource bundle for i18n support.
4. struts.action.extension to configure the URL suffix to for Struts2 application. Default suffix is .action but sometimes we might want to change it to .do or something else.

Wwe can configure above constants in struts.xml file like below.
 
	<constant name="struts.devMode" value="true"></constant>

	<constant name="struts.action.extension" value="action,do"></constant>

	<constant name="struts.custom.i18n.resources" value="global"></constant>

	<constant name="struts.convention.result.path" value="/"></constant>
    
   
   	13. 在Struts2中的action mapping中如何使用命名空间?
 
 Struts2 命名空间允许我们配置，创建模块简单化。我们可以用命名空间根据各自的功能给我们的Action类分开，例如管理员，用户，客户等。
 	 14. 哪个拦截器负责映射请求参数Action类的Java Bean的属性？
 
 com.opensymphony.xwork2.interceptor.ParametersInterceptor interceptor is responsible for mapping request parameters to the Action class java bean properties. This interceptor is configured in struts-default package with name “params”. This interceptor is part of basicStack and defaultStack interceptors stack.
	 15. 那个拦截器负责i18n的支持?
 
 com.opensymphony.xwork2.interceptor.I18nInterceptor interceptor is responsible for i18n support in Struts2 applications. This interceptor is configured in struts-default package with name “i18n” and it’s part of i18nStack and defaultStack.
 	16. What is the difference in using Action interface and ActionSupport class for our action classes, which one you would prefer?
Wwe can implement Action interface to create our action classes. This interface has a single method execute() that we need to implement. The only benefit of using this interface is that it contains some constants that we can use for result pages, these constants are SUCCESS, ERROR, NONE, INPUT and LOGIN.
ActionSupport class is the default implementation of Action interface and it also implements interfaces related to Validation and i18n support. ActionSupport class implements Action, Validateable, ValidationAware, TextProvider and LocaleProvider interfaces. We can override validate() method of ActionSupport class to include field level validation login in our action classes.
Depending on the requirements, we can use any of the approaches to create struts 2 action classes, my favorite is ActionSupport class because it helps in writing validation and i18n logic easily in action classes.
 
 
 	17. How can we get Servlet API Request, Response, HttpSession etc Objects in action classes?
Sstruts2 action classes doesn’t provide direct access to Servlet API components such as Request, Response and Session. However sometimes we need these access in action classes such as checking HTTP method or setting cookies in response.
Thats why Struts2 API provides a bunch of *Aware interfaces that we can implement to access these objects. Struts2 API uses dependency injection to inject Servlet API components in action classes. Some of the important Aware interfaces are SessionAware, ApplicationAware, ServletRequestAware and ServletResponseAware.
Yyou can read more about them in How to get Servlet API Session in Struts2 Action Classes tutorial. 
 
 	18. What is the use of execAndWait interceptor?
Sstruts2 provides execAndWait interceptor for long running action classes. We can use this interceptor to return an intermediate response page to the client and once the processing is finished, final response is returned to the client. This interceptor is defined in the struts-default package and implementation is present in ExecuteAndWaitInterceptor class.
Ccheck out Struts2 execAndWait interceptor example to learn more about this interceptor and how to use it. 
 
 	19. What is the use of token interceptor in Struts2?
Oone of the major problems with web applications is the double form submission. If not taken care, double form submission could result in charging double amount to customer or updating database values twice. We can use token interceptor to solve the double form submission problem. This interceptor is defined in struts-default package but it’s not part of any interceptor stack, so we need to include it manually in our action classes.
Rread more at Struts2 token interceptor example. 
 
 	20. How can we integrate log4j in Struts2 application?
Sstruts2 provides easy integration of log4j API for logging purpose, all we need to have is log4j configuration file in the WEB-INF/classes directory.
Yyou can check out the sample project at Struts2 Log4j integration. 
 
 	21. What are different Struts2 tags? How can we use them?
 Struts2 provides a lot of custom tags that we can use in result pages to create views for client request. These tags are broadly divided into three categories- Data tags, Control tags and UI tags.
We can use these tags by adding these in JSP pages using taglib directive.

	<%@ taglib uri="/struts-tags" prefix="s" %>
Some of the important Data tags are property, set, push, bean, action, include, i18n and text tag. Read more at Struts2 Data Tags.
Control tags are used for manipulation and navigation of data from a collection. Some of the important Control tags are if-elseif-else, iterator, append, merge, sort, subset and generator tag. Read more at Struts2 Control Tags.
Struts2 UI tags are used to generate HTML markup language, binding HTML form data to action classes properties, type conversion, validation and i18n support. Some of the important UI tags are form, textfield, password, textarea, checkbox, select, radio and submit tag. Read more about them at Struts2 UI Tags.
 
 	22. What is Custom Type Converter in Struts2?
 Struts2 support OGNL expression language and it performs two important tasks in Struts 2 – data transfer and type conversion.
OGNL is flexible and we can easily extend it to create our own custom converter class. Creating and configuring custom type converter class is very easy, first step is to fix the input format for the custom class. Second step is to implement the converter class. Type converter classes should implement com.opensymphony.xwork2.conversion.TypeConverter interface. Since in web application, we always get the request in form of String and send response in the form of String, Struts 2 API provides a default implementation of TypeConverter interface, StrutsTypeConverter. StrutsTypeConverter contains two abstract methods – convertFromString to convert String to Object and convertToString to convert Object to String.
For implementation details, read Struts2 OGNL Example Tutorial.
 
 	23. How can we write our own interceptor and map it for action?
 We can implement com.opensymphony.xwork2.interceptor.Interceptor interface to create our own interceptor. Once the interceptor class is ready, we need to define that in struts.xml package where we want to use it. We can also create interceptor stack with our custom interceptor and defaultStack interceptors. After that we can configure it for action classes where we want to use our interceptor.
One of the best example of using custom interceptor is to validate session, read more about it at Struts2 Interceptor Tutorial.
	24. What is life cycle of an interceptor?
 Interceptor interface defines three methods – init(), destroy() and intercept(). init and destroy are the life cycle methods of an interceptor. Interceptors are Singleton classes and Struts2 initialize a new thread to handle each request. init() method is called when interceptor instance is created and we can initialize any resources in this method. destroy() method is called when application is shutting down and we can release any resources in this method.
intercept() is the method called every time client request comes through the interceptor.
 
 	25. What is an interceptor stack?
 An interceptor stack helps us to group together multiple interceptors in a package for further use. struts-default package creates some of the mostly used interceptor stack – basicStack and defaultStack. We can create our own interceptor stack at the start of the package and then configure our action classes to use it.
 
 	26. What is struts-default package and what are it’s benefits?
 struts-default is an abstract package that defines all the Struts2 interceptors and commonly used interceptor stack. It is advisable to extend this package while configuring our application package to avoid configuring interceptors again. This is provided to help developers by eliminating the trivial task of configuring interceptor and result pages in our application.
 
 	27. What is the default suffix for Struts2 action URI and how can we change it?
 The default URI suffix for Struts2 action is .action, in Struts1 default suffix was .do. We can change this suffix by defining struts.action.extension constant value in our Struts2 configuration file as:
view sourceprint?

	<constant name="struts.action.extension" value="action,do"></constant>

 
 	28. What is the default location of result pages and how can we change it?
 By default Struts2 looks for result pages in {WEBAPP-ROOT}/{Namespace}/ directory but sometimes we want to keep result pages in another location, we can provide struts.convention.result.path constant value in Struts2 configuration file to change the result pages location.
Another way is to use @ResultPath annotation in action classes to provide the result pages location.
 
 	29. How can we upload files in Struts2 application?
File Upload is one of the common task in a web application. Thats why Struts2 provides built in support for file upload through FileUploadInterceptor. This interceptor is configured in struts-default package and provide options to set the maximum size of a file and file types that can be uploaded to the server.
Read more about FileUpload interceptor at Struts2 File Upload Example. 
 
 	30. What are best practices to follow while developing Struts2 application?
Some of the best practices while developing Struts2 application are:
* Always try to extend struts-default package while creating your package to avoid code redundancy in configuring interceptors.
* For common tasks across the application, such as logging request params, try to use interceptors.
* Always keep action classes java bean properties in a separate bean for code reuse and implement ModelDriven interface.
* If you have custom interceptor that you will use in multiple actions, create interceptor stack for that and then use it.
* Try to divide your application in different modules with namespace configuration based on functional areas.
* Try to use Struts2 tags in result pages for code clarify, if needed create your own type converters.
* Use development mode for faster development, however make sure production code doesn’t run in dev mode.
* Use Struts2 i18n support for resource bundles and to support localization.
* Struts2 provides a lot of places where you can have resource bundles but try to keep one global resource bundle and one for action class to avoid confusion.
* struts-default package configures all the interceptors and creates different interceptor stacks. Try to use only what is needed, for example if you don’t have localization requirement, you can avoid i18n interceptor.

Thats all for the Struts2 interview question and answers, if you come across any important question that I have missed, please let me know through comments. 
 
