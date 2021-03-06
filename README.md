#Dome9 App for Splunk

Splunk Technology Add-on for Dome9 Version 2.0

This Splunk App is for people who have an account with Dome9 (http://www.Dome9.com) and want to gather their alerts into their on-premis Splunk or Splunk Cloud instance. Unlike the previous version, you will no longer need to have the App for AWS installed in order to pull the data from Dome9. This solution will now use an AWS lambda function and the Splunk HTTP Event Collector (HEC). Here is what you will need in order to get this solution up and running in your AWS environment:
	
	- A Configured Dome9 account that is sending data to an SNS topic in AWS
	- AWS CLI tools (http://docs.aws.amazon.com/cli/latest/userguide/installing.html) 
	- KMS Key (https://aws.amazon.com/blogs/aws/new-key-management-service/) 
	- Access to creating a Lambda function in your AWS account
	- Splunk Token from the HTTP Event Collector (See below for links to documentation on HTTP Event Collector)

Reference Material :

	- http://dev.splunk.com/view/event-collector/SP-CAAAE7T - Video on setting up a Lambda function with Splunk.
	- http://docs.splunk.com/Documentation/Splunk/6.4.1/Data/UsetheHTTPEventCollector - Setup Splunk HTTP Event Collector 
	- https://gist.github.com/glennblock/0d5e6384d93449d3e7c6 - Information on how to properly setup props.conf

Here's a list of files that are added to your $SPLUNK_HOME/etc/apps/splunk_app_dome9 folder:

	- default/props.conf

##Splunk Cloud Customers 
Contact Support to have the Dome9 App for Splunk installed on your environment.  You will also need to specify in the case that you need to enable the HTTP Event Collector.  They will send you the URL and token that you will need for the following steps. Make sure to specify the sourcetype=aws-dome9 and the index=main.)


##Splunk On-Prem Customers 
Steps to configure your deployment :
- Step 1 - Download and install the Splunk App for Dome9 from Splunkbase (https://splunkbase.splunk.com/app/3203)
- Step 2 - Create HTTP Event Collector (HEC) Token Creation 
	- Log into your Splunk instance and click on Settings -> Data Inputs -> HTTP Event Collector.  Click on Create New Token. Name the input "Dome9 Input" then click Next. 
	
	- Select the correct source type "aws:dome9", leave the Default Index as "Default." Click Review, then Submit. 
	- Copy the Token Value and keep it ready. 
	- Finally, enable the token by clicking on "Global Settings" and enable the tokens.

		- Now create a new input and generate the associated token.  Once you have this token, copy it and have it available for the next step. 

- Step 2 - Create the AWS Lambda Function
	- Log into your AWS Console and navigate to the Lambda Function service. 
	- Click on "Create a Lambda function"
	- In the Filter box, type in "splunk-logging" Click on the result.
		- Give your Lambda function a name, I would suggest using "GetDataFromDome9"
		-	(Watch this video for additional help setting up the Lambda function with Splunk http://dev.splunk.com/view/event-collector/SP-CAAAE7T) 
		- Encrypt your Splunk HEC token using the AWS CLI tools and the KMS key generated per the video. 
		
	- With the lambda function setup, now go to your SNS topics and subscribe your newly created lambda function to the topic collecting data from Dome9.
	
- Step 3 - Testing 
	- Log into your Dome9 account and send out test alerts by using the alerting feature in Dome9. 
