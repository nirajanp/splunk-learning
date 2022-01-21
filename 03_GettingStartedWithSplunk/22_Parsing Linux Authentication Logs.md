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
