### Importing Data in Splunk
  
  * before we start to build amazing dashboards, first we need to import some data to splunk

#### In this section we will discuss about two data types

  * HTTP Access Logs: it containes records of the requests that are processed by Web-Server
      * for example, if you are visiting a website, inside a website you might be doing various things like adding some contents to the cart, buying some product. all of those logs are stores as part of the HTTP access logs
      * name of the log file: access.log
  * Linux Authentication Logs: authentication success & failure related messages
      * for example: who is logging to your server, how many fail attempts by attackers that were part of the server
      * name of the log file: secure.log

#### Set Source Type

  * the source type is one of the default fields that the Splunk platform assign to all incoming data 
  * at high level overview it tells the platform what kind of data you have, so that it can format the data intelligently during indexing

  * in source type data is not formatted properly. for example, ata of type __HTTP Access logs__ could be in __source type__, in this case we will have to refer to the documentation of __HTTP Access logs__ how data is formatted
  * however if we tell __Splunk__ that this specific data we are importing it is a __HTTP access logs__ then __Splunk__ has its own parser and will show data in better way 


#### Add data to Splunk - Hands ON

  * go to Settings
  * add Data
  * click on upload
  * select file ex: access.log
  * next screen is related to __Source Type__. here we have to set right set o f source type for our data
  * for http access logs select source type as __access_combined__
  * next
  * review
  * submit
  * you should see 'File has been uploaded successfully'

__NOTE: if you are importing linux authentication logs, then select source type as linux_secure__

#### To look up the added data

  * go to _splunk>enterprise_ home
  * go to _search and reporting_
  * go to _data summary_

#### Important Note

  * most of the common types of data are easily parsed by Splunk considering right source type are associated with it
  * in addition to this there are __Splunk add ons__ which are availabel in the marketplaces that can also parse the data for a specific source type
  * for custom data, you can also create your own parser that can parse the data