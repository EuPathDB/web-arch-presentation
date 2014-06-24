**EuPathDB Web Infrastructure and Philosophy**

2011



Press 's' to open presenter mode and view slide notes.

Note: Presenter notes are here.



![EuPathDB Family](eupath-arch/Slide02.png)

As of 2014, now includes HostDB, OrthoMCL.

Note: EuPathDB BRC hosts many projects. We give each
project a unique, short name that we use in casual 
conversation and, most importantly, in naming conventions 
for organizing websites and applications belonging to each project. 




![EuPathDB Extended Family](eupath-arch/Slide03.png)

We also host other independent projects.



![Release Cycle](eupath-arch/Slide04.png)

Code committed from developer sites is validated on integration sites before being propagated to QA and then public websites.

Note: Each developer gets a personal website for sandbox work.

Committed code from all developers is used to build integration sites. This is a check that all code is properly committed and works in the context of all other code changes. Integration sites are rebuilt from source code 
for each commit set.

QA sites are rebuilt nightly from source. These password protected sites are for data provider preview.

beta sites are the same installation base as QA sites but with out password protection. The beta sites are for community preview and are released after all providers have approved their data for release.

"live" sites are released when we are satisfied the sites are ready for general consumption.

The trunk svn code is branched to stablize the released website. Work for the next release continues on svn trunk in developer websites.




![Personal Development Sites](eupath-arch/Slide06.png)

Note: having separate websites for each developer is great for 
independent development and testing. The developer works on an 
isolated website, then commits changes to the source code repository. 

However, problems can arise:

  - changes made by Developer 1 may be incompatible with Developer 2
  - code may be tested against the wrong database
  - subsets of code needed for a new feature may accidentally be omitted from the commit

So we have 'integration' websites ...



![Integration Sites](eupath-arch/Slide08.png)




![QA Sites](eupath-arch/Slide10.png)




![Beta Sites](eupath-arch/Slide12.png)

Note: beta websites share the same code installation as
the QA websites (just so we do not have to have yet 
another set of websites to maintain). Beta websites are 
really just an Apache virutal host that does not 
require a password to accesss.



![Live Sites](eupath-arch/Slide14.png)




![Sense of Scale](eupath-arch/Slide16.png)




![Site List](eupath-arch/Slide17.png)




![Take Home](eupath-arch/Slide18.png)




![What is a Website](eupath-arch/Slide19.png)




![Minimal Tomcat](eupath-arch/Slide20.png)

At a minimum, WDK requires deployment in Tomcat. WDK presents data from databases and from WebService Framework (WSF) processes.



![Tomcat + Apache](eupath-arch/Slide21.png)

A EuPathDB website also needs support for CGI, HTTP rewrites, Access Control and many other features provided by Apache HTTP Server.

Note: All HTTP requests are managed by the Apache HTTP Server (AHS). AHS determines how to 
handle the request, for example passing off to Tomcat or to a CGI script.



![Multiple Tomcats and Virtual Hosts](eupath-arch/Slide22.png)

Note: A single HTTPD server listens on port 80 for all incoming requests.
Requests handled by appropriate NameBasedVirtualHosts configuration.
Requests for the WDK webapp ( eg. /plasmo/) are proxied to Tomcat via mod_proxy or mod_jk apache modules.

Restarting the HTTPD server affects all virtual hosts.

Each Tomcat Instance listens on a dedicate port and hosts one or more webapps.
Tomcat allows individual webapps to be stopped, started, reloaded.

Restarting the Tomcat Instance affects all webapps deployed in that instance but does not affect the 
other Instances.



![Dev site on non-Default Tomcat Instance](eupath-arch/Slide23.png)

A virtual host can be configured to use any webapp in any Tomcat Instance.

Note: A virtual host can be configured to use any webapp in any Tomcat Instance.

This allows development and testing in isolated environments. Useful for testing 

  - Tomcat versions
  - Java versions
  - jdbc drivers
  - jvm options : memory allocation, garbage collection



![Lots of Cats](eupath-arch/Slide24.png)

We have lots of cats.



![Keeping Organized: Naming Conventions](eupath-arch/Slide26.png)

Hundreds of websites, dozens Tomcat instances. How to manage them all?



![http directory layout](eupath-arch/Slide27.png)

A directory for each project. Subdirectory for each virtual host, named after the Tomcat webapp. 
A symbolic link named after the virtual host points to the webapp directory.



![Locating Files By Naming Convention](eupath-arch/Slide28.png)

The naming conventions aids locating source code and configurations.



![Symbolic Link Aids Release, pt1](eupath-arch/Slide29.png)

The symbolic link for www.toxodb.org points to code for Build 10.



![Symbolic Link Aids Release, pt2](eupath-arch/Slide30.png)

To release a new version, the symbolic link for www.toxodb.org is changed to Build 11.



![Keeping Organized: Consistent Deployment](eupath-arch/Slide31.png)

With so many websites and developers it is important that everyone manages their websites the same way.



![Maintenance Utilities](eupath-arch/Slide32.png)

rebuilder and ibuilder wrap the GUS build framework, leverage our naming conventions to establish 
a correct and consistent build environment. 

_(These scripts are not supported for non-EuPathDB projects.)_



![Maintenance Utilities](eupath-arch/Slide33.png)

New websites can be rapidly installed with a custom script that follows our naming conventions. 

_(This script is not supported for non-EuPathDB projects.)_




![Avoid Tomcat Manager UI](eupath-arch/Slide34.png)

Tomcat bundles a Manager application with a web interface. This is too cumbersome to use routinely 
so we don't use this interface. Instead we use a command-line utility...



![Maintenance Utilities](eupath-arch/Slide35.png)

instance_manager is included with EuPathDB's **Tomcat Instance Manager**, available on GitHub.



![Automation](eupath-arch/Slide37.png)

EuPathDB uses several tools to automate tasks related to managing the numerous website deployments.



![Jenkins](eupath-arch/Slide38.png)

Jenkins is a leading Continuous Integration application. Code commits to our Subversion repository 
triggers Jenkins to build and deploy websites.



![Puppet, et al](eupath-arch/Slide39.png)

Puppet allows us to consistently deploy software and configurations across numerous servers. 
Software is installed through standard RPM 
packages. EuPathDB maintains its own YUM repository for custom RPMs.



![Keeping Organized: Tools](eupath-arch/Slide40.png)




![Jenkins, Dashboard](eupath-arch/Slide41.png)

Jenkins keeps records of each build it performs. EuPathDB's /dashboard provides a RESTful API to 
the website state and configuration.

_(/dashboard is currrently not supported for non-EuPathDB projects.)_



![Cattail](eupath-arch/Slide42.png)

_(cattail is not supported for non-EuPathDB projects.)_



![Take Home](eupath-arch/Slide43.png)




![TMTOWTDI](eupath-arch/Slide44.png)

There is More Than One Way To Do It