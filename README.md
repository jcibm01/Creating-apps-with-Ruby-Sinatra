Creating-apps-with-Ruby-Sinatra
===============================

Creating apps with Ruby Sinatra

DISCLAIMER----This routine and info wasn´t created for me!. It´s available in https://www.ng.bluemix.net/docs/#starters/rubysinatra/index.html#rubyhelloworld 
My purpose is to invite all github users to know IBM Bluemix Platform.

IBM® Bluemix™ provides a Ruby Sinatra starter application as a template so that you can add your code and push the changes back to the Bluemix environment. The Ruby Sinatra runtime environment is the container for this type of applications.
About the Ruby Sinatra starter application

This Ruby Sinatra starter application is a boilerplate for Bluemix Ruby Sinatraweb application development.

It displays the current system environment variables and demonstrates how to develop and deploy a Ruby Sinatra web application for Bluemix.
Usage
Use the following steps to start using the Ruby Sinatra starter application from the Bluemix user interface:

    Create a Ruby Sinatra starter application and download the starter code.
        Click Create App to deploy a Ruby Sinatra starter application.
        Provide the App name and Host in the prompt.
        After the application deploys, click the View Quick Start button.
        Download the starter application package.
    Ensure that the cf command line interface is installed.

    This tool is a command line interface for application deployment and management on Bluemix.
    Log in to the Bluemix environment.

    $ cf login -a https://api.ng.bluemix.net -o <your org name> -s <your space name> API endpoint: https://api.ng.bluemix.net Username> <your user ID> Password>******* Authenticating... OK Targeted org <your org name> Targeted space dev API endpoint: https://api.ng.bluemix.net (API version: 2.0.0) User: <your user ID> Org: <your org name> Space: <your space name>

    Modify the application and deploy again.

    In the starter application package, a README file describes each file. Make changes to the code, deploy the application again, then see the effect.

    To deploy your modified application to Bluemix, use the cf command. For example:

    $ cf push <yourappname> -p pathtoApp -m 512M

    Access your application.

    After your application is deployed, open the URL in a browser to see it running.

    http://<yourappname>.mybluemix.net

    Start using services.

    There are many services available in Bluemix to make your application more powerful. You can add any available services to your application. See each service document for more information.

Building a Sinatra application

You can use a Ruby Sinatra application to send and retrieve messages from the Rabbit MQ service in Bluemix. The Rabbit MQ is a community service just for demonstration and you should consider using Elastic MQ in your production environment.

Before you begin, make sure that you have installed the cf command v6 and registered in Bluemix with your IBM ID.
Connecting to Bluemix
You need to log into the Bluemix environment to proceed with application deployment.

 $ cf api https://api.ng.bluemix.net
 $ cf login

After you issue those commands, the connection information of Bluemix is recorded in your system.
Creating a RabbitMQ service instance
You can use the following command to create a RabbitMQ service instance:

$ cf create-service rabbitmq 100 rabbitmq-ouswc

This command creates a RabbitMQ instance with name rabbitmq-ouswc in your space. You can also use cf services to view the newly created service instance.
Uploading the Sinatra application

A sample Ruby Sinatra application might help you get started. This sample application can send messages to the RabbitMQ service instance and retrieve those messages from the queue. You can download the sample Sinatra application from the sinatra-rabbitmq community sample site.
Extract the application to your local directory and issue the following commands to push the application to the Bluemix environment.

 $ cf push -f manifest.yml

Note that the service name in the manifest.yml file must match the one you just created by using the cf create-service command, and you can change the host and name to make sure they are unique throughout the Bluemix environment.
Accessing the application

After the application is deployed into the Bluemix, you can access the application by using its url. For example, http://Sinatra001.mybluemix.net.
Code snippet for connection
In this sample application, the connection information is retrieved from VCAP_SERVICE variable and passed to bunny to establish the connection to the RabbitMQ service instance in Bluemix.

   def amqp_url
    services = JSON.parse(ENV['VCAP_SERVICES'], :symbolize_names => true)
    url = services.values.map do |srvs|
      srvs.map do |srv|
        if srv[:credentials][:url] =~ /^amqp/
          srv[:credentials][:url]
        else
          []
        end
      end
    end.flatten!.first
  ...
   def client
    unless $client
      c = Bunny.new(amqp_url)
      c.start
      $client = c
	...
  end

For more information about the Sinatra application and how it works, see Sinatra Documentation.
