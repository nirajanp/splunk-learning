### Deploying Splunk Docker Container

  * once we have Docker up and running, we can launch Splunk Docker container to get started
  * since we want to deploy splunk container in a droplet in digital ocean we need to ssh into that first
  * after creating container use following command
  * cmd: `ssh root@publicIPv4`
  * to deploy splunk container into the droplet use this command
  * cmd: `$ docker run -d -p 8000:8000 -e "SPLUNK_START_ARGS=--accept-license" -e "SPLUNK_PASSWORD=<password>" --name splunk splunk/splunk:latest`