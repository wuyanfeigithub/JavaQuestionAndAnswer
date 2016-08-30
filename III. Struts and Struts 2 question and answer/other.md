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

type=dispatcher:


























 <package name="struts-default" abstract="true" strict-method-invocation="true">
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

        <interceptors>
            <interceptor name="alias" class="com.opensymphony.xwork2.interceptor.AliasInterceptor"/>
            <interceptor name="autowiring" class="com.opensymphony.xwork2.spring.interceptor.ActionAutowiringInterceptor"/>
            <interceptor name="chain" class="com.opensymphony.xwork2.interceptor.ChainingInterceptor"/>
            <interceptor name="conversionError" class="org.apache.struts2.interceptor.StrutsConversionErrorInterceptor"/>
            <interceptor name="cookie" class="org.apache.struts2.interceptor.CookieInterceptor"/>
            <interceptor name="cookieProvider" class="org.apache.struts2.interceptor.CookieProviderInterceptor"/>
            <interceptor name="clearSession" class="org.apache.struts2.interceptor.ClearSessionInterceptor" />
            <interceptor name="createSession" class="org.apache.struts2.interceptor.CreateSessionInterceptor" />
            <interceptor name="debugging" class="org.apache.struts2.interceptor.debugging.DebuggingInterceptor" />
            <interceptor name="execAndWait" class="org.apache.struts2.interceptor.ExecuteAndWaitInterceptor"/>
            <interceptor name="exception" class="com.opensymphony.xwork2.interceptor.ExceptionMappingInterceptor"/>
            <interceptor name="fileUpload" class="org.apache.struts2.interceptor.FileUploadInterceptor"/>
            <interceptor name="i18n" class="com.opensymphony.xwork2.interceptor.I18nInterceptor"/>
            <interceptor name="logger" class="com.opensymphony.xwork2.interceptor.LoggingInterceptor"/>
            <interceptor name="modelDriven" class="com.opensymphony.xwork2.interceptor.ModelDrivenInterceptor"/>
            <interceptor name="scopedModelDriven" class="com.opensymphony.xwork2.interceptor.ScopedModelDrivenInterceptor"/>
            <interceptor name="params" class="com.opensymphony.xwork2.interceptor.ParametersInterceptor"/>
            <interceptor name="actionMappingParams" class="org.apache.struts2.interceptor.ActionMappingParametersInteceptor"/>
            <interceptor name="prepare" class="com.opensymphony.xwork2.interceptor.PrepareInterceptor"/>
            <interceptor name="staticParams" class="com.opensymphony.xwork2.interceptor.StaticParametersInterceptor"/>
            <interceptor name="scope" class="org.apache.struts2.interceptor.ScopeInterceptor"/>
            <interceptor name="servletConfig" class="org.apache.struts2.interceptor.ServletConfigInterceptor"/>
            <interceptor name="timer" class="com.opensymphony.xwork2.interceptor.TimerInterceptor"/>
            <interceptor name="token" class="org.apache.struts2.interceptor.TokenInterceptor"/>
            <interceptor name="tokenSession" class="org.apache.struts2.interceptor.TokenSessionStoreInterceptor"/>
            <interceptor name="validation" class="org.apache.struts2.interceptor.validation.AnnotationValidationInterceptor"/>
            <interceptor name="workflow" class="com.opensymphony.xwork2.interceptor.DefaultWorkflowInterceptor"/>
            <interceptor name="store" class="org.apache.struts2.interceptor.MessageStoreInterceptor" />
            <interceptor name="checkbox" class="org.apache.struts2.interceptor.CheckboxInterceptor" />
            <interceptor name="datetime" class="org.apache.struts2.interceptor.DateTextFieldInterceptor" />
            <interceptor name="profiling" class="org.apache.struts2.interceptor.ProfilingActivationInterceptor" />
            <interceptor name="roles" class="org.apache.struts2.interceptor.RolesInterceptor" />
            <interceptor name="annotationWorkflow" class="com.opensymphony.xwork2.interceptor.annotations.AnnotationWorkflowInterceptor" />
            <interceptor name="multiselect" class="org.apache.struts2.interceptor.MultiselectInterceptor" />

            <!-- Basic stack -->
            <interceptor-stack name="basicStack">
                <interceptor-ref name="exception"/>
                <interceptor-ref name="servletConfig"/>
                <interceptor-ref name="prepare"/>
                <interceptor-ref name="checkbox"/>
                <interceptor-ref name="datetime"/>
                <interceptor-ref name="multiselect"/>
                <interceptor-ref name="actionMappingParams"/>
                <interceptor-ref name="params"/>
                <interceptor-ref name="conversionError"/>
            </interceptor-stack>

            <!-- Sample validation and workflow stack -->
            <interceptor-stack name="validationWorkflowStack">
                <interceptor-ref name="basicStack"/>
                <interceptor-ref name="validation"/>
                <interceptor-ref name="workflow"/>
            </interceptor-stack>

            <!-- Sample file upload stack -->
            <interceptor-stack name="fileUploadStack">
                <interceptor-ref name="fileUpload"/>
                <interceptor-ref name="basicStack"/>
            </interceptor-stack>

            <!-- Sample model-driven stack  -->
            <interceptor-stack name="modelDrivenStack">
                <interceptor-ref name="modelDriven"/>
                <interceptor-ref name="basicStack"/>
            </interceptor-stack>

            <!-- Sample action chaining stack -->
            <interceptor-stack name="chainStack">
                <interceptor-ref name="chain"/>
                <interceptor-ref name="basicStack"/>
            </interceptor-stack>

            <!-- Sample i18n stack -->
            <interceptor-stack name="i18nStack">
                <interceptor-ref name="i18n"/>
                <interceptor-ref name="basicStack"/>
            </interceptor-stack>

            <!-- An example of the paramsPrepareParams trick. This stack
                 is exactly the same as the defaultStack, except that it
                 includes one extra interceptor before the prepare interceptor:
                 the params interceptor.

                 This is useful for when you wish to apply parameters directly
                 to an object that you wish to load externally (such as a DAO
                 or database or service layer), but can't load that object
                 until at least the ID parameter has been loaded. By loading
                 the parameters twice, you can retrieve the object in the
                 prepare() method, allowing the second params interceptor to
                 apply the values on the object. -->
            <interceptor-stack name="paramsPrepareParamsStack">
                <interceptor-ref name="exception"/>
                <interceptor-ref name="alias"/>
                <interceptor-ref name="i18n"/>
                <interceptor-ref name="checkbox"/>
                <interceptor-ref name="datetime"/>
                <interceptor-ref name="multiselect"/>
                <interceptor-ref name="params"/>
                <interceptor-ref name="servletConfig"/>
                <interceptor-ref name="prepare"/>
                <interceptor-ref name="chain"/>
                <interceptor-ref name="modelDriven"/>
                <interceptor-ref name="fileUpload"/>
                <interceptor-ref name="staticParams"/>
                <interceptor-ref name="actionMappingParams"/>
                <interceptor-ref name="params"/>
                <interceptor-ref name="conversionError"/>
                <interceptor-ref name="validation">
                    <param name="excludeMethods">input,back,cancel,browse</param>
                </interceptor-ref>
                <interceptor-ref name="workflow">
                    <param name="excludeMethods">input,back,cancel,browse</param>
                </interceptor-ref>
            </interceptor-stack>

            <!-- A complete stack with all the common interceptors in place.
                 Generally, this stack should be the one you use, though it
                 may do more than you need. Also, the ordering can be
                 switched around (ex: if you wish to have your servlet-related
                 objects applied before prepare() is called, you'd need to move
                 servletConfig interceptor up.

                 This stack also excludes from the normal validation and workflow
                 the method names input, back, and cancel. These typically are
                 associated with requests that should not be validated.
                 -->
            <interceptor-stack name="defaultStack">
                <interceptor-ref name="exception"/>
                <interceptor-ref name="alias"/>
                <interceptor-ref name="servletConfig"/>
                <interceptor-ref name="i18n"/>
                <interceptor-ref name="prepare"/>
                <interceptor-ref name="chain"/>
                <interceptor-ref name="scopedModelDriven"/>
                <interceptor-ref name="modelDriven"/>
                <interceptor-ref name="fileUpload"/>
                <interceptor-ref name="checkbox"/>
                <interceptor-ref name="datetime"/>
                <interceptor-ref name="multiselect"/>
                <interceptor-ref name="staticParams"/>
                <interceptor-ref name="actionMappingParams"/>
                <interceptor-ref name="params"/>
                <interceptor-ref name="conversionError"/>
                <interceptor-ref name="validation">
                    <param name="excludeMethods">input,back,cancel,browse</param>
                </interceptor-ref>
                <interceptor-ref name="workflow">
                    <param name="excludeMethods">input,back,cancel,browse</param>
                </interceptor-ref>
                <interceptor-ref name="debugging"/>
            </interceptor-stack>

            <!-- The completeStack is here for backwards compatibility for
                 applications that still refer to the defaultStack by the
                 old name -->
            <interceptor-stack name="completeStack">
                <interceptor-ref name="defaultStack"/>
            </interceptor-stack>

            <!-- Sample execute and wait stack.
                 Note: execAndWait should always be the *last* interceptor. -->
            <interceptor-stack name="executeAndWaitStack">
                <interceptor-ref name="execAndWait">
                    <param name="excludeMethods">input,back,cancel</param>
                </interceptor-ref>
                <interceptor-ref name="defaultStack"/>
                <interceptor-ref name="execAndWait">
                    <param name="excludeMethods">input,back,cancel</param>
                </interceptor-ref>
            </interceptor-stack>

       </interceptors>

        <default-interceptor-ref name="defaultStack"/>

        <default-class-ref class="com.opensymphony.xwork2.ActionSupport" />

        <global-allowed-methods>execute,input,back,cancel,browse,save,delete,list,index</global-allowed-methods>

    </package>
