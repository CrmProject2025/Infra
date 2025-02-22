
# microservices

crm-core 8080
email-service 8075
eureka-server 8700 надо поменять на 8761 (default)

utilits

postgres 5432 (local), 5434 (docker)
frontend(react) 3000
jenkins 8090
redis 
promethues grafana
tlk stack


ssh -p 443 -R0:127.0.0.1:8090 qr@a.pinggy.io


curl.exe -sO http://localhost:8090/jnlpJars/agent.jar

java -jar agent.jar -url http://localhost:8090/ -secret 2261966fa63e13ea8949ad4488e16e7980e68280d407c50cb73185351c21916d -name "local-agent" -webSocket -workDir "E:\programs\jenkins\jenkins-nodes"

JenkinsApiToken for github webhook (https://Tework123:115b9ec1c31df6ff89a9caac1185ef459f@rnxxa-95-37-174-199.a.free.pinggy.link/):

115b9ec1c31df6ff89a9caac1185ef459f
