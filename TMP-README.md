<!-- PROJECT SHIELDS -->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![GNU License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]



<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/AntoineMeheut/JavaVisualVMGuide">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">Java VisualVM Guide</h3>

  <p align="center">
    Analyze a JVM, on a remote server, in real time with a JMX connection.
    <br />
    <a href="https://docs.oracle.com/javase/8/docs/technotes/guides/visualvm/index.html"><strong>Explore the Oracle docs</strong></a>
    <br />
    <br />
    <a href="https://github.com/AntoineMeheut/JavaVisualVMGuide">View Demo</a>
    ·
    <a href="https://github.com/AntoineMeheut/JavaVisualVMGuide/issues">Report Bug</a>
    ·
    <a href="https://github.com/AntoineMeheut/JavaVisualVMGuide/issues">Request Feature</a>
  </p>
</p>



<!-- TABLE OF CONTENTS -->
## Table of Contents

* [About the Project](#about-the-project)
  * [Configure the launch of the JVM](#Configure-the-launch-of-the-JVM)
  * [Get IP address to use](#Get-IP-address-to-use)
  * [Prepare your start-my-application.sh](#Prepare-your-start-my-application.sh)
  * [Run the JAVA Visual VM Tool](#Run-the-JAVA-Visual-VM-Tool)
  * [Connect JAVA Visual VM to the remote server](#Connect-JAVA-Visual-VM-to-the-remote-server)
  * [Analyze the operation of your JVM in real time](#Analyze-the-operation-of-your-JVM-in-real-time)
* [Getting Started](#getting-started)
  * [Installation](#installation)
* [Usage](#usage)
* [Roadmap](#roadmap)
* [Contributing](#contributing)
* [License](#license)
* [Contact](#contact)
* [Acknowledgements](#acknowledgements)



<!-- ABOUT THE PROJECT -->
## About The Project

### Configure the launch of the JVM
You need to create or open a start-my-application.sh script on your server, because we are going to modify the options passed at the start of the JVM, so that it knows that you will use the JMX connection to analyze its operation in remote mode.

The launch line of your JVM will looks like this in the <strat_my_application.sh> script
```
java -jar -Dspring.config.location=$install_dir/conf/<my_application_repository>/application.properties -Dlogback.configurationFile=$install_dir/conf/<my_application_repository>/logback.xml -Dconfig.file.path=$install_dir/conf/<my_application_repository>/business/businessConfiguration.xml $install_dir/jars/<my_application_name.jar> &
```

We will now add a number of new options that I will detail :

* **Option N°1**
```
-Dcom.sun.management.jmxremote
```
Tell the JVM that you will use JMX mode. JMX is the acronym for Java Management Extensions. JMX is a specification that defines an architecture, an API, and services to help you monitor and manage Java resources. JMX makes it possible to set up, using a standard, a system for monitoring and managing an application, a service or a resource without having to make a lot of effort.

* **Option N°2**
```
-Dcom.sun.management.jmxremote.port=9876
```
Indicates the port number on which your JVM will listen. It is this port that we will use from the Visual VM JAVA tool to connect to the JVM we are going to analyze.

* **Option N°3**
```
-Dcom.sun.management.jmxremote.ssl=false
```
Indicates that we will not use SSL mode to connect to our server from a computeur. For development environments we do not need SSL connection. For other environment should be set to true, but it leads to correctly set the server for this type of connection works with the JMX mode.

* **Option N°4**
```
-Dcom.sun.management.jmxremote.authenticate=false
```
Indicates that we will not use login and password to connect to the JVM. For dev environments is not required, but it may be necessary for other environments to set up this type of security.

* **Option N°5**
```
-Djava.rmi.server.hostname=<server_logical_name>
```
Specifies the logical name of the server on which the JVM that you want to monitor is running.

### Get IP address to use
To obtain the logical address of your server, proceed as follows.
```
ifconfig
```

You get inet configuration information from your server, you must copy the IP address and use it with the following command.
```
nslookup <xxx.xxx.xxx.xxx>
```

You get the logical address of your server on the screen, it should be used in preference to IP addresses, because IP addresses can be changed when you change the runtime environment of the application, so that normally your logical address will not change.

### Prepare your start-my-application.sh
So in summary, in your <start_my_application.sh> script, you'll end up with the following launch line for your JVM.

```
java -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=9876 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Djava.rmi.server.hostname=<server_logical_name> -Dspring.config.location=$install_dir/conf/<my_application_repository>/application.properties -Dlogback.configurationFile=$install_dir/conf/<my_application_repository>/logback.xml -Dconfig.file.path=$install_dir/conf/<my_application_repository>/business/businessConfiguration.xml -jar $install_dir/jars/<my_application_name.jar> &
```

### Run the JAVA Visual VM Tool
The JAVA Visual VM tool is available in all versions of Java JDK, to run it you simply need to open a command prompt on your computer and type the following command.

```
jvisualvm
```

You then get the next application.

[![VisualVM application][VisualVM-application]]()

### Connect JAVA Visual VM to the remote server
In the Visual VM application, right click in the white area on the left below Snapshots. This will open a small window where you have to click on the Add JMX connection option.

[![Add JMX Connection][Add-JMX-Connection]]()

You then get a window that will allow you to indicate on which remote machine you want to connect.

[![JMX informations][JMX-informations]]()

WARNING: the Visual VM application will then try to connect to your server and the specified port and will finally try to access the JVM, so you have to have launched your JVM before trying to connect to it.

### Analyze the operation of your JVM in real time
If the remote connection to the JVM on your server is going well, you must obtain a new machine in the remote section of the Visual VM application.

[![New machine][New-machine]]()

To start analyzing how your JVM works, you have to right click on the name of the JVM and choose the menu option called "Open".

[![JVM open][JVM-open]]()

A window with the description of your JVM and these launch parameters will appear on the right.

[![JVM parameters][JVM-parameters]]()

Finally, in the tabs of this window, you need to choose the one called "Monitor", this tab will allow you to have the CPU usage information, memory, classes and threads.

[![JVM monitoring][JVM-monitoring]]()

<!-- GETTING STARTED -->
## Getting Started

To get a local copy up and running follow these simple steps.

### Installation
 
1. Clone the repo
```
git clone https://github.com/AntoineMeheut/JavaVisualVMGuide
```

<!-- USAGE EXAMPLES -->
## Usage

Clone this project to use it for your developments or simply copy/paste the java script for your startmyapplication.sh

<!-- ROADMAP -->
## Roadmap

See the [Project](https://github.com/AntoineMeheut/JavaVisualVMGuide/projects) for a list of proposed features (and known issues).

<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<!-- LICENSE -->
## License

Distributed under the GNU General Public License v3.0 License. See `LICENSE` for more information.

<!-- CONTACT -->
## Contact

Antoine Méheut - [@Linkedin_antoine-meheut](https://www.linkedin.com/in/antoine-meheut)

Project Link: [https://github.com/AntoineMeheut/JavaVisualVMGuide](https://github.com/AntoineMeheut/JavaVisualVMGuide)

<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements

This page was created by using it on au project and informations collected on Oracle VisualVM documentation.

* [Oracle VisualVM](https://docs.oracle.com/javase/8/docs/technotes/guides/visualvm/index.html)

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/AntoineMeheut/JavaVisualVMGuide?color=green
[contributors-url]: https://github.com/AntoineMeheut/JavaVisualVMGuide/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/AntoineMeheut/JavaVisualVMGuide
[forks-url]: https://github.com/AntoineMeheut/JavaVisualVMGuide/network/members
[stars-shield]: https://img.shields.io/github/stars/AntoineMeheut/JavaVisualVMGuide
[stars-url]: https://github.com/AntoineMeheut/JavaVisualVMGuide/stargazers
[issues-shield]: https://img.shields.io/github/issues/AntoineMeheut/JavaVisualVMGuide
[issues-url]: https://github.com/AntoineMeheut/JavaVisualVMGuide/issues
[license-shield]: https://img.shields.io/github/license/AntoineMeheut/JavaVisualVMGuide
[license-url]: https://github.com/AntoineMeheut/JavaVisualVMGuide/blob/master/LICENSE
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=flat-square&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/antoine-meheut
[VisualVM-application]:images/visualVM.png
[Add-JMX-Connection]: images/addJMXConnection.png
[JMX-informations]: images/info_jmx.png
[New-machine]: images/connection.png
[JVM-open]: images/open.png
[JVM-parameters]: images/principal.png
[JVM-monitoring]: images/monitor.png