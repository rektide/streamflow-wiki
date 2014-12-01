### Does StreamFlow make any modifications to Storm?
StreamFlow does not make any modifications to the Storm source code and does not require any special configuration to work.  This means you can point StreamFlow to your existing Storm cluster and immediately begin submitting topologies.

While not a requirement, StreamFlow can benefit from the use of Logstash if installed on your cluster to better view and manage cluster topology log files.

### Does StreamFlow work with any other stream processing technologies?
Currently, StreamFlow only supports integration with Apache Storm.  We do have plans in our road map to explore integration with other technologies such as Apache Spark.

### StreamFlow fails to start due to "java.net.BindException: Address already in use"
The most common reason for this error is a port conflict with StreamFlow and some other application already running on your server.  StreamFlow by default will startup using port `8080`.  If another application is already using this port, you will get see the `java.net.BindException`.

A simple fix is to simply change the `server.port` in the `streamflow.yml` configuration to use a different available port.  Please consult the [Server Configuration](Configuration#server-configuration) section for additional details about server configuration. 

### The StreamFlow UI is returning a 404 error
Check your StreamFlow log file to verify the StreamFlow server is running without errors.  The StreamFlow UI is hosted by an embedded Jetty server in the StreamFlow server application and will not allow UI access if the server is not running or halted due to an uncaught error.

If you experience any repeated ocurrences of errors causing the server to halt, please report this bug to the team for further analysis.
