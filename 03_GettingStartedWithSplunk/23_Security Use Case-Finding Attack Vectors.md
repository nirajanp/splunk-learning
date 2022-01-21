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