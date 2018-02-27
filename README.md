# mavenStudy
It is using Alura.com examples

This project will take the basic information about Maven and web app building process



01 DOWNLOADING, INSTALLLING AND SET THE RIGHT ENVIRONMENT VARIABLE

So, 1st of all let's download Maven
https://maven.apache.org/download.cgi

Extract the package in any folder you want

Using terminal enter in the maven folder and then, in the bin folder
- cd /usr/local/apache-maven-3.5.2/bin/

Type
- mvn --help
--- It will show all possible commands to use with Maven
- Ok Maven is installed and working

The Environment Variable
- enter in your user folder root
--- cd ~/
- edit the .bash_profile file
--- vim .bashbash_profile
--- Add the following line:
----- export M2_HOME=/usr/local/apache-maven-3.5.2/bin
--- save
--- close and reopen the terminal
--- from anywhere type = mvn --help
----- If it opens the same option from before it means your variable is properly setted.



02 - CREATING A NEW MAVEN PROJECT, COMPILING AND TESTING IT

To Create
in the terminal just type the below command inside the folder you want to create your maven project
- mvn archetype:generate -DartifactId=projectProducts -DgroupId=de.com.studying.maven -DinteractiveMode=false -DarchetypeArtifactId=maven-archetype-quickstart

mvn archetype:generate = means to create a new maven project
-DartifactId=projectProducts = means the project's name
-DgroupId=de.com.studying.maven = means the package structure
-DinteractiveMode=false = means Maven will create without further questions
-DarchetypeArtifactId=maven-archetype-quickstart = means our project will be created based on this quick start project, downloading all necessary things from the internet

P.S.: Important to check that the project was created with the structure main and test folder with an file class example

To Compile
inside your project folder root path
- mvn compile
-P.S.: The 1st time it will take some time, but it is because Maven is downloading some plugins and necessary things due to proper compile our app

To Test
just type = mvn test
it will download all necessary library, such as JUnit, SureFire and so on, and also run the test

P.S.: The pom.xml file is the Maven core file, it means, this file has all the library, configurations, dependencies, scopes and all other information related to your project

If you want to Clean your project, to clean your target folder, just keeping the code you are working on
- mvn clean



03 - GENERATING A PACKAGE AND REPORTS

If you need to create a JAR, it means create a package from your app, just type:
- mvn package
--- It will create a JAR inside our TARGET folder

Imagine if you want to get a report from your test execution, but a report in a HTML view.

mvn surefire-report:report
- It will download every necessary thing and will generate the report using the surefire plugin
- SureFire is a plugin that give us a basic report of our tests and it will create a folder SITE inside our TARGET folder with a HTML report file



04 - MAVEN AND ECLIPSE OR SOME OTHER IDE AND MVN REPOSITORY

Just import your project as a Maven project and point it to your root project path
Open the POM.XML and edit whatwever you want to

Adding New Library Dependencies:
- inside the dependency tag, just add your new library dependency
- in order to know the right information just access the Maven repo and check the library you want
- https://mvnrepository.com/
- search for example for XStream
--- you will find a lot of libraries, just try to get the right one you really need
--- in our case is the XStream Core from XStream company
--- choose the version you want
--- click in the version you want and copy the dependency tag from there  and paste inside your dependency tag in your pom.xml
P.S.: Note that if you are using this in the general dependency area, it means it will be used for our hole app. We can also add dependencies inside our scope, then it will be applied just for a certain scope or if we add that inside our test tag it will be applied just for our tests.
--- Save your changes, you just made in your pom.xml
--- Be aware it will automatically build your project again and download the XStream JAR to your MAVEN DEPENDENCY area in the left side of the Eclipse / other IDE view
P.S.: It will not only download the XStream jar but also other JARs it will need. It is also described in the XStream MVN repo page, below the dependency part, you will find Provided Dependencies and Test Dependencies. Just back there and check this out.
- Just repeat the steps above to use the Hibernate Library and check that after save it will build and download not only the Hibernate.jar but also some other dependencies

P.S.: Now we can see that MAVEN will help us not only to download all necessary things, but it will also manage the dependencies used by our libraries.



05 - MAVEN LOCAL / REMOTE REPOSITORY

While using MAVEN, all the time we use a MVN command it will check in the INTERNET for new versions or dependencies and then, it will run our desired action.

For example, using the terminal, inside your project folder type = mvn test. We can see in the end the the time it spent to run the command and also ever action it did. Try the 2 following commands

mvn test - Check the time it spent.
mvn -o test - Check the time

The 2nd command spent less time, because we said to it, run OFFLINE (-o). We just need to be carefull while using this command because it will not update anything, it will just run with our library versions.

If we don't use the OFFLINE (-o) paramenter it will always hit the central maven repo (http://repo.maven.apache.org/maven2/)

This central repo is what we call as REMOTE repo. But we also have a LOCAL repo. For example, the 1st time you run a maven command it will hit the REMOTE repository to download all necessary things, but after that it stores locally and from the 2nd time on it will use this local repo to run you command.

The LOCAL maven repository is stored at your user root path
cd ~/
We will see the hidden folder called (.m2)
cd .m2/repository/
Here is stored all your local maven repository and it will include all the library versions you already downloaded.

P.S.: We can also add new REMOTE repositoies using the tag <repositories></repositories> in our pom.xml

The POM.XML file supports a lot of configuration, life cycle, dependencies, repos, scopes... We can check all configs at:
https://maven.apache.org/pom.html



06 - MAVEN PHASES / LIFE CYCLE, PLUGINS...

https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html

The Maven Lifecycle is made by phases. The basic phases are:
- validate - validate the project is correct and all necessary information is available
- compile - compile the source code of the project
- test - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
- package - take the compiled code and package it in its distributable format, such as a JAR.
- verify - run any checks on results of integration tests to ensure quality criteria are met
- install - install the package into the LOCAL repository, for use as a dependency in other projects locally
- deploy - done in the build environment, copies the final package to the REMOTE repository for sharing with other developers and projects.

For instance:
- Whe we run the command MVN COMPILE it will run:
--- validate phase + compile
- When we run the command MVN PACKAGE, it will run:
--- validate phase + compile + test + package



07 - MAVEN REPORTS AND PLUGINS

Maven has a lot of plugins and reports that can help us during our project. Let's see some of them

PMD
https://maven.apache.org/plugins/maven-pmd-plugin/
- this is a plugin that help us to check our code quality and search for some possible problems or non good code practices
- mvn pmd:pmd
--- it will run and generate a report inside our project in the path = $project/target/site
- mvn pmd:check
--- it means, not only generate the report, but also FAIL the build if the PMD find any issue.
P.S.: We can add this PMD plugin during our project build process, by adding this info in our pom.xml

PMD inside our BUILD process
- in the pom.xml create the tag <build> it will describe all actions during our BUILD process
- inside the build tag we need to create the tags <plugins> and <plugin>, because that will add the plugins we need with all their tags such as: groupId, artifactId... Here we add the PMD plugin information
- now, we need the tags <executions>, <execution> to say that we plan to execute our plugin
- inside the execution tag we need the <phase>, <goals> and <goal> to say in which phase we need to execute our plugin and with which purpouse.

In the end, of our pom.xml should be something like this:

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-pmd-plugin</artifactId>
            <version>3.9.0</version>
            <executions>
                <execution>
                    <phase>verify</phase>
                    <goals>
                        <goal>check</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>

Now, if we run our MVN VERIFY it will execute our PMD plugin, because we said to execute it during the VERIFY phase with the goal to CHECK our build quality

You can take this information from the USAGE Plugin MAVEN page
- https://maven.apache.org/plugins/maven-pmd-plugin/usage.html

Let's do the same we did for PMD plugin, but now using JACOCO  plugin.

JACOCO
http://www.jacoco.org/jacoco/trunk/doc/maven.html
- it is a plugin to verify the test coverage
- let's also configure that in our pom.xml file as we did before

<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.7.5.201505241946</version>
    <executions>
        <execution>
        <phase>verify</phase>
            <goals>
                <goal>prepare-agent</goal>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>

P.S.: The PREPARE-AGENT goal, means MAVEN will handle when run this and the REPORT goal means it will generate a report from the Jacoco plugin.

USING ECLIPSE TO RUN MAVEN
- We also can run our MVN commands using our Eclipse or any other IDE.
- Right click in the project > Run As > Maven Build ... > A dialogue window will be opened
--- In the goals field you add theMaven's phase you want to run and give a name to it in the top of the window.
--- Click in RUN.
- It will run inside our IDE

Important to note is that some times the Project will show an ERROR regarding the update, just type Maven update and execute it, it shoul fix the problem



08 - MAVEN AND WEB PROJECT CREATION + WEB SERVER CONFIGURATION

Creating a Web project from Eclipse
- Right clicke in the left panel space > New Project > Other > Maven Project > Next
- Select the location > Next
- Select the Archetype, but not the wuick start but the WEBAPP > Next
--- P.S.: It will allow us to work with servlets (tomcat) and other we resources
- Add the group Add (de.products.maven for instance)
- Add the artifact Id (webstore for example)
- Add the Vesion (1.0.0-SNAPSHOT for instance)
- Finish

Project Created.

Webserver Configuration
Now we need a web server. We can use any server, Tomcat, JBoss, Jetty.
Let's use Jetty, just because it is simple and with basic options.
- enter in the Jetty page (https://www.eclipse.org/jetty/)
- in the left side we will find the Mave Plugin topic
- Click on it > click on Jetty:run
--- Then we will find the code to copy and paste in our pom.xml
- open our pom.xml in Eclipse and paste the code from the website inside our build tag, just removing the configuration part from the code we just copied.
- Then, our pom.xml should look like this, in the build tag:
<build>
    <finalName>webstore</finalName>
    <plugins>
        <plugin>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-maven-plugin</artifactId>
            <version>9.4.8.v20171121</version>
        </plugin>
    </plugins>
</build>

Running our JETTY webserver
- enter in your project folder via terminal
- type mvn jetty:run
- in the end it will show the message STARTED JETTY SERVER
- using your browse, go to http://localhost:8080
- it should display the message HELLO WORLD!

P.S.: Be aware that it is a JSP content and for that reason it will work automatically, but if we change our JAVA classes from our back end app we will need to reboot our server or we will need to configure it to automatically search for updates. We will make it soon.

From where this Hello World message is comming from?
- Using Eclipse open your project and go to src>main>webapp>index.jsp
- Edit the Hello World message and Put some other word.
- Save the change
- Back to your browser and refresh the page.
- It will display the new message in the browser

Fixing the JSP message error in Eclipse
- There is a error message being displayed in the JSP file, related to no servelet JAR associated to our project.
--- Our Jetty or our web server will add this servlet to compile our code, but our IDE doesn't know that.
--- Due to fix this error, let's add an servlet api dependency using our project pom.xml
----- Let's edit our pom.xml including the dependency related to JAVA servlet api
----- In the mvnrepository.org search for servlet
----- Choose the one related to JAVA Servlet API version 3.1 or older
----- copy and paste the mvn dependency code in our pom.xml
--- Save it and the error should disapear

Web XML code
- Another important thing is to use in our web.xml file the same configuration related to our servlet api version
- search in the internet for web xml 3.1
- find one result similar to
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
version="3.1">
</web-app>
- Copy and paste it, by replacing the content of the file src/main/weapp/WEB-INF/web.xml
- Save
- Kill the Jetty and start it again by MAVEN command line

The project should be running and without errors.


Creating the JAVA folder to keep the standard
- Using Eclipse, in the left side, right click over the src/main folder.
- Create a new Folder, called JAVA
- P.S.: It automatically put this folder as a source folder of our project. Here is the place to create our classes, txt...


Creating our 1st JAVA class to use in our app
- Inside our src/main/java create a class called ContactServlet.java
- add the following contact inside this class
@WebServlet(urlPatterns = { "/contato" })
public class ContactServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        writer.println("<html><h2>Contact Page</h2></html>");
        writer.close();
    }
}
- P.S.: Just highlighting that the annotation @WebServlet(urlPatterns=) will give us the URL to access our page
- via terminal, reboot our server Jetty
- in our browser access
http://localhost:8080/contact
- it will display the contact page


Automatically Searching for changes in our Servlet class
- in the ContactServlet.java file, change the message to = Talk to Us
- Save it
- go to the browser and refresh the contact page
- the change will not be displayed, unless we reboot our Jetty Or...
- to automatically search for updates add the following parameter to our POM.XML Jetty dependency
<configuration>
    <scanIntervalSeconds>10</scanIntervalSeconds>
</configuration>
- The above instruction will make our web server JETTY search for updates every 10 seconds and it will reboot and compile new classes everytime some changes happe.


Defining a root path to our app to access it in our browser
- Now our app is accessed directly using http://localhost:8080, but imagine we want to define something like:
- http://localhost:8080/store or http://localhost:8080/store/contact
- we can set this parameter in our JETTY dependency configuration via POM.XML
- add the following lines inside your configuration tag in the POM.XML
<webApp>
    <contextPath>/store</contextPath>
</webApp>
- Now our POM.XML is like this:
<plugin>
    <groupId>org.eclipse.jetty</groupId>
    <artifactId>jetty-maven-plugin</artifactId>
    <version>9.4.8.v20171121</version>
    <configuration>
        <scanIntervalSeconds>10</scanIntervalSeconds>
        <webApp>
            <contextPath>/store</contextPath>
        </webApp>
    </configuration>
</plugin>
- We have to restart our server, because we have changed our POM.XML plugin configuration
- via browser, try to access the old address http://localhost:8080/
- it will not work. Now try this:
http://localhost:8080/store



09 - MAVEN, WEB PROJECT AND PRODUCTION DEPLOY

Considering we already know how to add dependencies, scopes, plugins and manu other things. Now it is time to deploy our app in production. So, as we are working with a JAVA app, let's create our WAR (package) and deploy it in production.

To create our package, just add the goal package to our maven execution and run it.

- Using Eclipse > Right click on our project > Choose the option Maven Run... > Add the word PACKAGE in the goal field and Run
- It will generate the WAR file inside the target folder
- P.S.: This file is a kind of zip file including our compiled code and also all used JARs described in our dependency tag from our POM.XML



10 - MAVEN DEPENDENCY IN BETWEEN 2 PROJECTS
Adding a local project as a dependency to another project
- Just add the project information in our dependency tag via POM.XML
- But before generate your package you will need to install this new dependency in your LOCAL Repository
- run the mvn install and only after that run the jetty:run or mvn package
P.S.: Just never try to use your test classes from one project in another one. It will not be wrong from the IDE perspective, but it will throw a 500 in production

If you want to check more about maven dependencies, just check the dependency tree using the below command inside your project
- mvn dependency:tree



11 - MAVEN SCOPES
- The scope tag in our POM.XML inside our dependency tag, defines in which case we will have our dependencies JARs.
- The default scope is COMPILE <scope>compile</scope>, it means the JAR will be available not only to run but also for execution and it will be compiled together with our package (WAR file)
- The scope TEST <scope>test</scope>, means our JAR will be available only for testing, it will not go to production. For example doesn't make sense have our JUnit library in production. Then we set this dependency as test scope.
- The scope PROVIDED <scope>provided</scope>, means the library will be used to compile, but will not be inside our package, because it will be used directly from our Web Server. This is used for example for the JAVA SERVLET library, because it will be available in our web server.

P.S.: An important thing to do is always execute the MVN CLEAN before compile or generate the package, due to make sure our project will not have things are not used.
