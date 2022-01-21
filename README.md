# Splunk

## Section 2: Introduction to Splunk & Setting Up Labs

### Deploying Splunk Docker Container

  * once we have Docker up and running, we can launch Splunk Docker container to get started
  * since we want to deploy splunk container in a droplet in digital ocean we need to ssh into that first
  * after creating container use following command
  * cmd: `ssh root@publicIPv4`
  * to deploy splunk container into the droplet use this command
  * cmd: `$ docker run -d -p 8000:8000 -e "SPLUNK_START_ARGS=--accept-license" -e "SPLUNK_PASSWORD=<password>" --name splunk splunk/splunk:latest`

## Section 3: Getting Started with Splunk

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

### Parsing Linux Authentication Logs

  __Adding right source type is important__
  * Whenever we upload a log file to splunk, it is important to set right source type associated with it.
  * This allows Splunk to parse the log accordingly

  __Right source type might not always help__
  * even after choosing the specific source type in logs file, not all files are parse by splunk by default
  * in this case you could use add-ons

  __Add Ons Could be hero then__
  * in this case for Linux Authenticatoin Logs you could add __Linux AddOn__ 
  * in this add on there will be additional parser which helps to parse data accordingly
  * after adding __Linux AddOn__ data should be more readable

### Security Use-Case: Find Attach Vectors

  * At this stage, Linux Authentication logs available in Splunk and are parsed
  * The important part is what to do with these log files
    * just by storing log files in splunk will not help much
    * we could create a dashboard, and alert using these log files.

#### Use Case
  * Compliance auditor has requested you to find certain attack vectors related to SSH logins

  * requirements are below
   1. find the total number of SSH failed login attempts
        cmd: `source="secure.log" action=failure`
   2. find how many failed logins from every IPs
        cmd: `source="secure.log" action=failure | stats count by src`
   3. find List of Countries from which the failed login attempts were made
        cmd: `source="secure.log" action=failure | iplocation src` 
   4. freate a visualization of countries in the world map based on failed logins
        cmd: `source="secure.log" action=failure | iplocation src geostats count by Country` 

__Note: splunk provides lot of search commands that can be used to achieve various use-cases__
  * (List of search command)[https://docs.splunk.com/Documentation/SplunkLight/7.3.6/References/Listofsearchcommands]

### Basics of Search

#### Search for a String, 
  
  * you could use a specific string to search for in the file you imported
  * example: 
    * failed, failure, success
    * fail* 

#### Boolean Expression

  * the Splunk search processing language __(SPL)__ supports the Boolean operators: AND, OR, NOT 
  * Use-Case
    1. Search for all failed login attempts for user root. __SPL__ is root AND failed
    2. search for failed logins for all user except root. __SPL__ is failed NOT root
    3. search failed logins for user admin OR root. __SPL__ is failed admin OR root
  
  * even if you do not explictly write AND operator and just seperate with space then the space will be regarded as AND

#### Search Modes
  
  * you can use the search mode selector to provide a search experience that fits your needs

  * there are 3 primary search modes
    1. fast mode: field discovery is disabled for this mode for better performance
    2. verbose mode: returns all of the field and event data it possibly can, even if it means the search takes longer to complete
    3. smart mode: toggles between verbose search and fast search, based on the type of search you are doing

### Splunk Search Assistant

  * assisting users by showing autofill text while typing

  * Splunk search Assistant has three modes
    * Full
    * Compact
    * None
  * by default, the compace mode is selected but can be changed from Account Settings

  * to change the __Search Assistant__, go to administrator >> preferences >> SPL editor >> select mode

### Splunk Reports

  * Splunk has feature to export the result in xml, json, or excel format

#### Simple Use-Case

  * Security Manager should be informed with list of IP address and Country location from which failed SSH attempts are being made at every 24 hours over email.
    1. for this, at first write query
      query: `source="secure.log" action=failure | iplocation src | table src, Country | uniq`
    2. save it as a Report
    3. name the report 
    4. say _Yes_ to time range picker and save it
    5. message, "Your Report Has Been Created" will be displayed
    6. View the report
    7. chenge schedule of the report

  * all the reports will be available in splunk>enterprise home page under Reports

### Understanding Add-Ons and Apps

  * Add-ons extend the functionality of the Splunk platform and provide required field extractions, lookups, saved searches, and others

#### Splunk Add-on For Data Input

  * lets say there is a Splunk instance and AWS environment and you want to fetch the logs from AWS environment
  * in such cases you cases you could make use of appropriate add-ons which can be used.
  * this add-on has required script that can connect to AWS environment and fetch the requirement, level of logs, and other information and add it as part of splunk env

#### Hardware considerations

  * some of the splunk add-ons and apps are resource heavy
  * running them in instance with lower hardware will lead to Splunk/Server going down or becoming unresponsive

### Installing Splunk Add-On for AWS

  * add-On must support your Splunk version
  * hardware requirements for the Add-On
  * support type

#### High Level overview

  * login to AWS account
  * create a user with ReadOnlyAccess
  * get the access key of this user
  __in splunk__
  * add AWS Add-On 
  * go to __Configurations__ tab
  * add the AWS account's key ID and Secret Key.
  * add what kind of inputs you want from AWS from __Inputs__ tab then __Create New Input__
  * go to __Data Summary__ from splunk homepage and inspect the data

### Overview of Dashboard and Panels

#### Create a custom Dashboard

  * login to __Splunk__ 
  * go to __Search and Reporting__
  * go to __Dashboards__
  * click __Create New Dashboard__
  * give name
  * __Classic Dashboard__ and __Create__
