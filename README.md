JavaVisualVMGuide
=================

Analyze a JVM, on a remote server, in real time
===============================================

Objectives of this guide
------------------------

1. Configure the launch of the JVM
2. Run the JAVA Visual VM Tool
3. Connect JAVA Visual VM to the remote server
4. Analyze the operation of your JVM in real time

Configure the launch of the JVM
-------------------------------
You need to open the <strat_my_application.sh> script on your server, because we are going to modify the options passed at the start of the JVM, so that it knows that you will use the JMX connection to analyze its operation in remote mode.

The launch line of your JVM will looks like this in the <strat_my_application.sh> script
```bash
java -jar -Dspring.config.location=$install_dir/conf/<my_application_repository>/application.properties -Dlogback.configurationFile=$install_dir/conf/<my_application_repository>/logback.xml -Dconfig.file.path=$install_dir/conf/<my_application_repository>/business/businessConfiguration.xml $install_dir/jars/<my_application_name.jar> &
```

We will now add a number of new options that I will detail.

Option N°1
```bash
-Dcom.sun.management.jmxremote
```
Tell the JVM that you will use JMX mode. JMX is the acronym for Java Management Extensions. JMX is a specification that defines an architecture, an API, and services to help you monitor and manage Java resources. JMX makes it possible to set up, using a standard, a system for monitoring and managing an application, a service or a resource without having to make a lot of effort.

Option N°2
```bash
-Dcom.sun.management.jmxremote.port=9876
```
Indicates the port number on which your JVM will listen. It is this port that we will use from the Visual VM JAVA tool to connect to the JVM we are going to analyze.

Option N°3
```bash
-Dcom.sun.management.jmxremote.ssl=false
```
Indicates that we will not use SSL mode to connect to our server from a computeur. For development environments we do not need SSL connection. For other environment should be set to true, but it leads to correctly set the server for this type of connection works with the JMX mode.

Option N°4
```bash
-Dcom.sun.management.jmxremote.authenticate=false
```
Indicates that we will not use login and password to connect to the JVM. For dev environments is not required, but it may be necessary for other environments to set up this type of security.

Option N°5
```bash
-Djava.rmi.server.hostname=<server_logical_name>
```
Specifies the logical name of the server on which the JVM that you want to monitor is running.

To obtain the logical address of your server, proceed as follows.
```bash
ifconfig
```

You get inet configuration information from your server, you must copy the IP address and use it with the following command.
```bash
nslookup <xxx.xxx.xxx.xxx>
```

You get the logical address of your server on the screen, it should be used in preference to IP addresses, because IP addresses can be changed when you change the runtime environment of the application, so that normally your logical address will not change.

So in summary, in your <start_my_application.sh> script, you'll end up with the following launch line for your JVM.

```bash
java -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=9876 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Djava.rmi.server.hostname=<server_logical_name> -Dspring.config.location=$install_dir/conf/<my_application_repository>/application.properties -Dlogback.configurationFile=$install_dir/conf/<my_application_repository>/logback.xml -Dconfig.file.path=$install_dir/conf/<my_application_repository>/business/businessConfiguration.xml -jar $install_dir/jars/<my_application_name.jar> &
```

Run the JAVA Visual VM Tool
---------------------------
The JAVA Visual VM tool is available in all versions of Java JDK, to run it you simply need to open a command prompt on your computer and type the following command.

```bash
jvisualvm
```

You then get the next application.

![VisualVM application](/images/visualVM.png "VisualVM application")

Connect JAVA Visual VM to the remote server
-------------------------------------------
In the Visual VM application, right click in the white area on the left below Snapshots. This will open a small window where you have to click on the Add JMX connection option.

![Add JMX Connection](/images/addJMXConnection.png "Add JMX Connection")

You then get a window that will allow you to indicate on which remote machine you want to connect.

![JMX informations](/images/info_jmx.png "JMX informations")

WARNING: the Visual VM application will then try to connect to your server and the specified port and will finally try to access the JVM, so you have to have launched your JVM before trying to connect to it.

Analyze the operation of your JVM in real time
----------------------------------------------
If the remote connection to the JVM on your server is going well, you must obtain a new machine in the remote section of the Visual VM application.

![New machine](/images/connection.png "New machine")

To start analyzing how your JVM works, you have to right click on the name of the JVM and choose the menu option called "Open".

![JVM open](/images/open.png "JVM open")

A window with the description of your JVM and these launch parameters will appear on the right.

![JVM parameters](/images/principal.png "JVM parameters")

Finally, in the tabs of this window, you need to choose the one called "Monitor", this tab will allow you to have the CPU usage information, memory, classes and threads.

![JVM monitoring](/images/monitor.png "JVM monitoring")

