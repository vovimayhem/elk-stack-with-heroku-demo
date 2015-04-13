# Heroku logs & ELK (Logstash + Elasticsearch + Kibana) demo

## Requirements:

 - Docker
 - Docker Compose
 - Docker Machine (if you are not using Linux)
 - Heroku Toolbelt


## See the demo in action:

First, start up the 3 containers (Logstash, Elasticsearch & Kibana). They'll bind
the following localhost ports to each container's service:

 - Port 9208: Elasticsearch HTTP endpoint
 - Port 9308: Elasticsearch discovery endpoint
 - Port 5608: Kibana Web UI
 - Port 15148: Logstash incoming syslog endpoint

```bash

# Navigate to the project folder:
cd [wherever the project is]

# Start up the required containers:
docker-compose up -d

# You can see the logs of the started containers:
docker-compose logs
```

Next, start a heroku log session and re-direct it to the logstash server. Please
note that this is not the recommended way to capture logs from a heroku app in a
production environment - more on that later on -, but it's just to see the ELK
stack in action quickly.

```bash

# Invoke the heroku log stream and redirect the output to our logstash server:

heroku logs -t --app [my app name] >> /dev/tcp/localhost/15148

```

## In production

The best way for this to work in a production environment is [configuring a
  heroku log drain](http://www.joemiller.me/2014/01/31/heroku-logs-drains-and-logstash/) pointing
to a published logstash syslog endpoint:

```bash
heroku drains:add --app [my app name] syslog://logstash.mydomain.tld:1514
```
