
## Sample Spring Boot REST App with CORS, OAuth2, HATEOAS 
* See the `README.adoc` for spring's readme, below is some more useful info. 
* The app can run either as Spring-boot application, or as regular .war file for deployment to an external server. 
* For .war deployments: 
  * The app has been successfully deployed to different servlet containers including Tomcat8 and IBM Liberty server. 
  * The app has been successfully deployed to IBM BlueMIX cloud (instructions below). 


## Install and build with  maven 
* Note, set mvn proxy/settings.xml  
* The project is a multi-module maven project which builds multiple artifacts 

```bash
mvn clean
mvn package 
java -jar ./security/target/security-0.0.1-SNAPSHOT.war  # runs embedded tomcat 
# or copy ./security/target/security-0.0.1-SNAPSHOT.war.original to your target server and rename to a .war file
```


## Build as .war 
* Spring boot applications support running the webapp as both
  * a) a runnable JAR on the command line (by embedding an executable Tomcat as a dependency) OR 
  * b) packaging the webapp as a regular .war file (file ends with .original) for deployment to an external container  (note, If you’re using the Spring Boot build tools i.e. the 'spring-boot-maven-plugin', marking the embedded servlet container dependency as provided will produce an executable war file with the provided dependencies packaged in a lib-provided directory. This means that, in addition to being deployable to a servlet container, you can also run your application using java -jar on the command line). 

The spring-boot-maven-plugin supports both these options. 
* see: https://spring.io/guides/gs/convert-jar-to-war/ (this page links to more detailed instructions summarised below):
  * 1) Package as a war by adding following xml to your pom.xml: `<packaging>war</packaging>`
  * 2) Provide a `SpringBootServletInitializer` subclass and override its configure method. This makes use of Spring Framework’s Servlet 3.0 support and allows you to configure your application when it’s launched by the servlet container. Typically, you update your application’s main class to extend `SpringBootServletInitializer` e.g. 
```java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }

    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }

}
```
  * 3) Ensure that the embedded servlet container doesn’t interfere with the servlet container to which the war file will be deployed. To do so, you need to mark the embedded servlet container dependency as provided. 
```xml
<dependencies>
    <!-- … -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <scope>provided</scope>
    </dependency>
    <!-- … -->
</dependencies>
```


## To deploy this application in the IBM bluemix cloud 
* Bluemix cloud based on CloudFoundary 
* Download/install the 'cf' CLI tool to push/deploy apps to bluemix
* Create a new project (typically a war file, note you don't have to use bluemix git) 
* Set the local http_proxy and https_proxy vars if behind a proxy 
* The basic process is: a) cf login, b) set cf service region, c) configure cf app settings in 'manifest.yml', d) push to bluemix cloud, e) view/manage your deployed apps in bluemix hosting portal 

```bash
cd initial
cf.exe api https://api.eu-gb.bluemix.net
cf.exe login -u <your_ibm_bluemix_account_name> -p <yourpassword>  
vim manifest.yml #<edit config file for the service(s)/settings and webapp name in bluemix>
cf push # push to bluemix cloud
```


* Full example (note, ignore the deploy error - this worked when i cleared up some space on bluemix): 
```bash
[initial]$export http_proxy=http://wwwcache.dl.ac.uk:8080
[initial]$export https_proxy=http://wwwcache.dl.ac.uk:8080
[initial]$
[initial]$
[initial]$cf api https://api.eu-gb.bluemix.net
Setting api endpoint to https://api.eu-gb.bluemix.net...
OK


API endpoint:   https://api.eu-gb.bluemix.net (API version: 2.40.0)
Not logged in. Use 'cf.exe login' to log in.
[initial]$cf.exe login -u <yourbluemixIdAccount> -p <your password>
API endpoint: https://api.eu-gb.bluemix.net
Authenticating...
OK

Targeted org <your.organisation>

Targeted space dev



API endpoint:   https://api.eu-gb.bluemix.net (API version: 2.40.0)
User:           <yourdetails>
Org:            <yourdetails>
Space:          dev
[initial]$


[initial]$cf push
Using manifest file C:\Users\djm76\Documents\programming-vcs\java\spring-rest-sample\gs-rest-service\initial\manifest.yml

Creating app davesSpringWS1 in org <yourdetails> / space dev as <yourdetails>...
OK

Creating route davesSpringWS1.eu-gb.mybluemix.net...
OK

Binding davesSpringWS1.eu-gb.mybluemix.net to davesSpringWS1...
OK

Uploading davesSpringWS1...
Uploading app files from: C:\Users\djm76\Documents\programming-vcs\java\spring-rest-sample\gs-rest-service\initial\target\gs-rest-service-0.1.0.war
Uploading 493.6K, 91 files
Done uploading
OK

Starting app davesSpringWS1 in org <yourdetails> / space dev as <yourdetails>...
FAILED
Server error, status code: 400, error code: 100005, message: You have exceeded your organization's memory limit.

```





