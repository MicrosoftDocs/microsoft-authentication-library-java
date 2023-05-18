---
title: Deployment instructions for MSAL Java samples
description: "MSAL Java can be deployed to a number of web and application servers. Although the exact build and deployment steps will depend on your environment and existing set up, here are instructions for running our MSAL Java samples on some popular web/app servers."
---

# Deployment instructions for MSAL Java samples

MSAL Java can be deployed to a number of web and application servers. Although the exact build and deployment steps will depend on your environment and existing set up, here are instructions for running our MSAL Java samples on some popular web/app servers.

## Build .war File Using Maven

All of our samples can be built using Maven. Navigate to the directory containing the pom.xml file for the project (typically the same as the main README if your trying to run our samples), and run the following Maven command:

`mvn clean package`

This should generate a .war file which can be run on a variety of web/app servers

Before you build the package, note that most samples' configuration files and README instructions use `http://localhost:8080` or `https://localhost:8443` as the default redirect URL. These ports work for Apache Tomcat and possibly the built-in app servers when running directly from your IDE, however some enterprise app servers such as Weblogic, JBoss, and Websphere use different default ports. In order to run a sample on these servers you will need to change the configuration in certain files and in the Azure app registration for that sample, and the instructions below include where to make those changes and what the default ports likely are.

## Running on Apache Tomcat

(These instructions assume you have installed Tomcat on your system)

To run the sample on Tomcat:

1. In your Tomcat installation, ensure there is a `<connector>` entry in tomcat/conf/server.xml for the address you want to host your application on
   * By default, our samples just expect to connect to `http://localhost:8080` or `https://localhost:8443`, as defined in the app.homePage value in authentication.properties file

2. Copy the .war file you generated with Maven to the /webapps/ directory in your Tomcat installation, and start the Tomcat server

3. Once Tomcat starts, open your browser and navigate to whatever URL you defined in step 1, and you should be able to access the application

## Running on WebLogic

(These instructions assume you have installed WebLogic and set up some server domain)

Before you can deploy to WebLogic, you will need to make some configuration changes in the sample itself and (re)build the package:

1. In the sample there is likely an `application.properties` or `authentication.properties` file where you configured the client ID, tenant, redirect URL, etc.

2. In the above mentioned file, changed references to `localhost:8080` or `localhost:8443` to the URL/port WebLogic will run on, which by default should be `localhost:7001`

3. You will also need to make the same change in the Azure app registration, where you set it as the 'Redirect URI' in the 'Authentication' tab

To deploy the sample to WebLogic via the web console:

1. Start the WebLogic server with DOMAIN_NAME\bin\startWebLogic.cmd

2. Navigate to the WebLogic web console in your browser, `http://localhost:7001/console`

3. Go to Domain Structure > Deployments, click Install, click upload your files, and find the .war file you built with Maven

4. Select Install this deployment as an application, click Next, click Finish, and then Save
    * Most of the default settings should be fine except that you should name the application to match the 'Redirect URI' you set in sample configuration/Azure app registration, i.e. if the redirect URI is `http://localhost:7001/msal4j-servlet-auth` then you should name the application 'msal4j-servlet-auth'

5. Go back to Domain Structure > Deployments, and Start your application

6. Once the application starts, navigate to `http://localhost:7001/{whatever you named the application}/`, and you should be able to access the application

## Running on JBoss EAP

Before you can deploy to JBoss, you will need to make some configuration changes in the sample itself and (re)build the package:

1. In the sample there is likely an `application.properties` or `authentication.properties` file where you configured the client ID, tenant, redirect URL, etc.

2. In the above mentioned file, changed references to `localhost:8080` or `localhost:8443` to the URL/port JBoss will run on, which by default should be `localhost:9990`

3. You will also need to make the same change in the Azure app registration, where you set it as the 'Redirect URI' in the 'Authentication' tab

To deploy the sample to JBoss EAP via the web console:

1. Start the JBoss server with %JBOSS_HOME%\bin\standalone.bat

2. Navigate to the JBoss web console in your browser, `http://localhost:9990`

3. Go to Deployments, click Add, and upload the .war you built

4. Most of the default settings should be fine except that you should name the application to match the 'Redirect URI' you set in sample configuration/Azure app registration, i.e. if the redirect URI is `http://localhost:9990/msal4j-servlet-auth/` then you should name the application 'msal4j-servlet-auth'

5. Select the .war file you uploaded, click En/Disable, and Confirm to start the application

6. Once the application starts, navigate to `http://localhost:9990/{whatever you named the application}/`, and you should be able to access the application

## Running on Websphere

(These instructions assume you have installed Websphere and set up some server )
Before you can deploy to Websphere, you will need to make some configuration changes in the sample itself and (re)build the package:

1. In the sample there is likely an `application.properties` or `authentication.properties` file where you configured the client ID, tenant, redirect URL, etc.
2. In the above mentioned file, changed references to `localhost:8080` or `localhost:8443` to the URL/port Websphere will run on, which by default should be `localhost:9080`
3. You will also need to make the same change in the Azure app registration, where you set it as the 'Redirect URI' in the 'Authentication' tab

Top deploy the sample using the Websphere's Integrated Solutions Console:

1. In the 'Applications' tab, select 'New Application', then 'New Enterprise Application'

2. Choose the .war you built, then click 'next' until you get to the 'Map context roots for Web modules' installation step (the other default settings should be fine)

3. For the context root, set it to the same value as after the port number in the 'Redirect URI' you set in sample configuration/Azure app registration, i.e. if the redirect URI is `http://localhost:9080/msal4j-servlet-auth/` then the context root should just be 'msal4j-servlet-auth'

4. Click 'Finish', and after the application finishes installing go to the 'Websphere enterprise applications' section of the 'Applications' tab

5. Select the .war you just installed from the list of applications and click 'Start' to deploy

6. One it finishes deploying, navigate to `http://localhost:9080/{whatever you set as the context root}` and you should be able to see the application
