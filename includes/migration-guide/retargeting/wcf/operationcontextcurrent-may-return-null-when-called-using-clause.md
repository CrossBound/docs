### OperationContext.Current may return null when called in a using clause

|   |   |
|---|---|
|Details|<xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> may return <code>null</code> and a <xref:System.NullReferenceException> may result if all of the following conditions are true:<ul><li>You retrieve the value of the <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> property in a method that returns a <xref:System.Threading.Tasks.Task> or <xref:System.Threading.Tasks.Task%601>.</li><li>You instantiate the <xref:System.ServiceModel.OperationContextScope> object in a <code>using</code> clause.</li><li>You retrieve the value of the <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> property within the <code>using statement</code>. For example:</li></ul><pre><code>using (new OperationContextScope(OperationContext.Current))<br />{<br />OperationContext context = OperationContext.Current;      // OperationContext.Current is null.<br />// ...<br />}</code></pre>|
|Suggestion|To address this issue, you can do the following:<ul><li>Modify your code as follows to instantiate a new non-<code>null</code> <xref:System.ServiceModel.OperationContext.Current%2A> object:</li></ul><pre><code>OperationContext ocx = OperationContext.Current;<br />using (new OperationContextScope(OperationContext.Current))<br />{<br />OperationContext.Current = new OperationContext(ocx.Channel);<br />// ...<br />}</code></pre><ul><li>Install the latest update to the .NET Framework 4.6.2, or upgrade to a later version of the .NET Framework. This disables the flow of the <xref:System.Threading.ExecutionContext> in <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> and restores the behavior of WCF applications in the .NET Framework 4.6.1 and earlier versions. This behavior is configurable; it is equivalent to adding the following app setting to your configuration file:</li></ul><pre><code>&lt;appSettings&gt;<br />&lt;add key=&quot;Switch.System.ServiceModel.DisableOperationContextAsyncFlow&quot; value=&quot;true&quot; /&gt;<br />&lt;/appSettings&gt;</code></pre>If this change is undesirable and your application depends on execution context flowing between operation contexts, you can enable its flow as follows:<pre><code>&lt;appSettings&gt;<br />&lt;add key=&quot;Switch.System.ServiceModel.DisableOperationContextAsyncFlow&quot; value=&quot;false&quot; /&gt;<br />&lt;/appSettings&gt;</code></pre>|
|Scope|Edge|
|Version|4.6.2|
|Type|Retargeting|
|Affected APIs|<ul><li><xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType></li></ul>|
