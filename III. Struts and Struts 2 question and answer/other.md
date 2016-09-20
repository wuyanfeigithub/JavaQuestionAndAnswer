###Struts 2 Result Type 详解
在struts2-default.xml中定义如下：
  <result-types>
              <result-type name="chain" class="com.opensymphony.xwork2.ActionChainResult"/>
              <result-type name="dispatcher" class="org.apache.struts2.result.ServletDispatcherResult" default="true"/>
              <result-type name="freemarker" class="org.apache.struts2.views.freemarker.FreemarkerResult"/>
              <result-type name="httpheader" class="org.apache.struts2.result.HttpHeaderResult"/>
              <result-type name="redirect" class="org.apache.struts2.result.ServletRedirectResult"/>
              <result-type name="redirectAction" class="org.apache.struts2.result.ServletActionRedirectResult"/>
              <result-type name="stream" class="org.apache.struts2.result.StreamResult"/>
              <result-type name="velocity" class="org.apache.struts2.result.VelocityResult"/>
              <result-type name="xslt" class="org.apache.struts2.views.xslt.XSLTResult"/>
              <result-type name="plainText" class="org.apache.struts2.result.PlainTextResult" />
              <result-type name="postback" class="org.apache.struts2.result.PostbackResult" />
  </result-types>

type=chain: 
This result invokes an entire other action, complete with it's own interceptor stack and result.
example:
<!-- START SNIPPET: example -->
 <package name="public" extends="struts-default">
     <!-- Chain creatAccount to login, using the default parameter -->
     <action name="createAccount" class="...">
      <result type="chain">login</result>;
     </action>

     <action name="login" class="...">
        <!-- Chain to another namespace -->
         <result type="chain">
             <param name="actionName">dashboard</param>
            <param name="namespace">/secure</param>
         </result>
     </action>
</package>

 <package name="secure" extends="struts-default" namespace="/secure">
     <action name="dashboard" class="...">
         <result>dashboard.jsp</result>
     </action>
 </package>
<!-- end SNIPPET: example -->

type=dispatcher: default=true,说明默认采用的是这个类型。 渲染页面的作用
Includes or forwards to a view (usually a jsp). Behind the scenes Struts will use a RequestDispatcher, where the target servlet/JSP receives the same request/response objects as the original servlet/JSP. Therefore, you can pass data between them using request.setAttribute() - the Struts action is available.
<!-- START SNIPPET: example -->
 <result name="success" type="dispatcher">
   <param name="location">foo.jsp</param>
  </result>
<!-- END SNIPPET: example -->

type=freemarker:
Renders a view using the Freemarker template engine.
 <!-- START SNIPPET: example -->

	<result name="success" type="freemarker">foo.ftl</result>

<!-- END SNIPPET: example -->

type=httpheader:
 A custom Result type for setting HTTP headers and status by optionally evaluating against the ValueStack.
This result can also be used to send an error to the client. All the parameters can be evaluated against the ValueStack.
<!-- START SNIPPET: example -->
<result name="success" type="httpheader">
  <param name="status">204</param>
  <param name="headers.a">a custom header value</param>
  <param name="headers.b">another custom header value</param>
</result>
<result name="proxyRequired" type="httpheader">
  <param name="error">305</param>
  <param name="errorMessage">this action must be accessed through a proxy</param>
</result>
<!-- END SNIPPET: example -->

type=redirect:
Calls the {@link HttpServletResponse#sendRedirect(String) sendRedirect} method to the location specified. The response is told to redirect the browser to the specified location (a new request from the client). The consequence of doing this means that the action (action instance, action errors, field errors, etc) that was just executed is lost and no longer available. This is because actions are built on a single-thread model. The only way to pass data is through the session or with web parameters (url?name=value) which can be OGNL expressions.
<!-- START SNIPPET: example -->
<!--
  The redirect URL generated will be:
  /foo.jsp#FRAGMENT
-->
<result name="success" type="redirect">
  <param name="location">foo.jsp</param>
  <param name="parse">false</param>
  <param name="anchor">FRAGMENT</param>
</result>
<!-- END SNIPPET: example -->

type=redirectAction:
This result uses the {@link ActionMapper} provided by the ActionMapperFactory to redirect the browser to a URL that invokes the  specified action and (optional) namespace. This is better than the  {@link ServletRedirectResult} because it does not require you to encode the  URL patterns processed by the {@link ActionMapper} in to your struts.xml configuration files. This means you can change your URL patterns at any point  and your application will still work. It is strongly recommended that if you are redirecting to another action, you use this result rather than the standard redirect result.

<!-- START SNIPPET: example -->
<package name="public" extends="struts-default">
    <action name="login" class="...">
        <!-- Redirect to another namespace -->
        <result type="redirectAction">
            <param name="actionName">dashboard</param>
            <param name="namespace">/secure</param>
        </result>
    </action>
</package>

<package name="secure" extends="struts-default" namespace="/secure">
    <-- Redirect to an action in the same namespace -->
    <action name="dashboard" class="...">
        <result>dashboard.jsp</result>
        <result name="error" type="redirectAction">error</result>
    </action>
    <action name="error" class="...">
        <result>error.jsp</result>
    </action>
</package>

<package name="passingRequestParameters" extends="struts-default" namespace="/passingRequestParameters">
   <!-- Pass parameters (reportType, width and height) -->
   <!--
   The redirectAction url generated will be :
   /genReport/generateReport.action?reportType=pie&amp;width=100&amp;height=100#summary
   -->
   <action name="gatherReportInfo" class="...">
      <result name="showReportResult" type="redirectAction">
         <param name="actionName">generateReport</param>
         <param name="namespace">/genReport</param>
         <param name="reportType">pie</param>
         <param name="width">100</param>
         <param name="height">100</param>
         <param name="empty"></param>
         <param name="suppressEmptyParameters">true</param>
         <param name="anchor">summary</param>
      </result>
   </action>
</package>

<!-- END SNIPPET: example -->

type=stream:
A custom Result type for sending raw data (via an InputStream) directly to the HttpServletResponse. Very useful for allowing users to download content.

<!-- START SNIPPET: example -->
<result name="success" type="stream">
  <param name="contentType">image/jpeg</param>
  <param name="inputName">imageStream</param>
  <param name="contentDisposition">attachment;filename="document.pdf"</param>
  <param name="bufferSize">1024</param>
</result>
<!-- END SNIPPET: example -->

type=velocity:
Using the Servlet container's {@link JspFactory}, this result mocks a JSP  execution environment and then displays a Velocity template that will be streamed directly to the servlet output.
<!-- START SNIPPET: example -->
<result name="success" type="velocity">
  <param name="location">foo.vm</param>
</result>
<!-- END SNIPPET: example -->

type=xslt:
XSLTResult uses XSLT to transform an action object to XML. The recent version  has been specifically modified to deal with Xalan flaws. When using Xalan you may notice that even though you have a very minimal stylesheet like this one
<!-- START SNIPPET: description.example -->
<result name="success" type="xslt">
  <param name="location">foo.xslt</param>
  <param name="matchingPattern">^/result/[^/*]$</param>
  <param name="excludingPattern">.*(hugeCollection).*</param>
</result>
<!-- END SNIPPET: description.example -->

type=plainText:
 A result that send the content out as plain text. Useful typically when needed to display the raw content of a JSP or Html file for example.
 <!-- START SNIPPET: example -->

<action name="displayJspRawContent" >
  <result type="plainText">/myJspFile.jsp</result>
</action>

<action name="displayJspRawContent" >
  <result type="plainText">
     <param name="location">/myJspFile.jsp</param>
     <param name="charSet">UTF-8</param>
  </result>
</action>

<!-- END SNIPPET: example -->
 
type=postback:
A result that renders the current request parameters as a form which immediately submits a <a href="http://en.wikipedia.org/wiki/Postback">postback</a> to the specified destination.

<!-- START SNIPPET: example -->
<action name="registerThirdParty" >
  <result type="postback">https://www.example.com/register</result>
</action>
<action name="registerThirdParty" >
  <result type="postback">
    <param name="namespace">/secure</param>
    <param name="actionName">register2</param>
  </result>
</action>
<!-- END SNIPPET: example -->


