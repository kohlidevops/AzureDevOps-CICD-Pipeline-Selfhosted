# To Deploy Artifacts on the Self-Hosted Environment using AzureDevOps CICD Pipeline

<img width="591" alt="image" src="https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/e5daf4bd-faed-450f-8552-1ece752ffc07">

**To create a virtual machine in Azure Cloud**

Navigate to portal.azure.com - Then create a new VM with ubuntu22 image

```
It can be your local machine too
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/af8db6a3-cac4-4ab1-8633-2e7472380bbc)

**To add the Agent Pools**

Navigate to dev.azure.com - Then select the Project settings

Choose Pipelines - Agent Pools - Add a new pool

```
Add agent pool
Pool to link - New
Pool type - Self-hosted
Name - MyAgentMachines
Create
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/2a1b3ec6-f2ed-4c34-a42c-c8e80ac5e9ae)

**To add a new Agent**

Project settings -> Pipelines -> Agent pools -> Agents -> New agent

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/63480e43-c8c4-4a22-9b6a-c997db55263a)

Now you can see the "Get the Agent Wizard" and you can use this to install and configure the agent on the target machines. For my case, It is ubuntu virtual machine. So I good to go with Linux.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/79dd2ab3-0ab9-4254-a9bc-0963b7b4a94b)


**To configure the PAT in Azure cloud**

Navigate to the dev.azure.com and select the user settings (top right corner) - create a personal access token (PAT).

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/0c566478-82cb-4281-9d57-3dd7db7db328)

```
Name - mydemoinstance
Organization - myorganization1109
Expiration - 30 days //Depends on your requirement
Scopes - Custom defined
Agent Pools - Read & manage
lets create
//Copy the token-id while creating. We can use it on the azure vm
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/79e42d0b-272c-4f83-9583-31280a0f50e5)

**To configure the agent on the target machine**

SSH to ubuntu VM and perform the below activity.

```
Download the Agent - https://vstsagentpackage.azureedge.net/agent/3.241.0/vsts-agent-linux-x64-3.241.0.tar.gz
Create the Agent -
~/$ mkdir myagent && cd myagent
~/myagent$ tar zxvf ~/Downloads/vsts-agent-linux-x64-3.241.0.tar.gz
Configure the Agent -
~/myagent$ ./config.sh

Enter (Y/N) Accept the Team Explorer Everywhere license agreement now? (press enter for N) > N
>> Connect:
Enter server URL > https://dev.azure.com/myorganization1109
Enter authentication type (press enter for PAT) > 
Enter personal access token > ****************************************************
>> Register Agent:

Enter agent pool (press enter for default) > 
Enter agent name (press enter for myinstance) > 
Scanning for tool capabilities.
Connecting to the server.
Successfully added the agent
Testing agent connection.
Enter work folder (press enter for _work) > 
2024-07-08 11:47:56Z: Settings Saved
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/bb08d96e-5a2d-4f21-8bde-802dc7ce87e2)

If you run run.sh, you could whether its active to listing jobs or not

```
./run.sh
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/70d370f1-398b-4f52-82ea-55cadd37b7fe)

Now Azure DevOps can assign any work to the Agent machine.

To create a new Release Pipeline

Navigate to dev.azure.com - choose Pipelines - new Release Pipeline

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/104e54d6-a436-4045-883d-77e80f632b0e)

Pipeline stage - It should be a empty job (Because we are going to deploy the Artiofacts to on self-hosted environment).

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/9ee0e6cc-1094-47f7-8e29-1544ed8fd826)

name it as Self Hosted Environment and update the Pipeline name as CICD Pipeline Self Hosted

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/b13d6b7f-90d3-4504-87a2-74fc1aa22343)

Job added, but task not yet added. I will add it soon.

Now click on the task - Click on the Agent Job

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/801e4568-df06-458d-91ae-31062b48c2b4)

```
Display name - Agent job
Agent pool - Default //Because we have given Agent Pool name as Default while I configure in Agent machine
Leave others as default and save it
```

Go back the Pipeline and Add an Artifacts. 

//If you need to know how to configure Artifacts, please go through below document to know and configure.

```
https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline
```

Add an Artifact

```
Source type - Build
Project - mydevops
Source (build pipeline) - kohlidevops.DevopsBasics (2)
Default version - Latest
Source alias - _kohlidevops.DevopsBasics (2)
Add it - Save the Pipeline
```

I have successfully added the Artifacts and Release Pipeline as you can see the image below.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/04737c1e-fd93-4604-8341-fb2ed7a6b1d8)

Just create Release Pipeline to check whether Artifacts are deployed on the target machine.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/65176ff7-ecb9-48ca-b022-57aad11ab4d7)

It has been deployed and job is successfully running.

You can check the Artifact logs in dev.azure.com console.

```
2024-07-08T15:59:04.4867017Z ##[section]Starting: Download artifact - _kohlidevops.DevopsBasics (2) - warfile
2024-07-08T15:59:04.5102643Z ==============================================================================
2024-07-08T15:59:04.5103426Z Task         : Download build artifacts
2024-07-08T15:59:04.5103722Z Description  : Download files that were saved as artifacts of a completed build
2024-07-08T15:59:04.5103886Z Version      : 0.239.1
2024-07-08T15:59:04.5104118Z Author       : Microsoft Corporation
2024-07-08T15:59:04.5104376Z Help         : https://docs.microsoft.com/azure/devops/pipelines/tasks/utility/download-build-artifacts
2024-07-08T15:59:04.5104578Z ==============================================================================
2024-07-08T15:59:08.8275520Z Downloading artifacts for build: 29
2024-07-08T15:59:08.8289490Z Downloading items from container resource #/25916366/warfile
2024-07-08T15:59:08.8738379Z Downloading artifact warfile from: https://dev.azure.com/myorganization1109//_apis/resources/Containers/25916366?itemPath=warfile&isShallow=true&api-version=4.1-preview.4
2024-07-08T15:59:08.8739428Z Prefer downloading files through redirect to the content stitcher: true
2024-07-08T15:59:08.8748381Z Downloading warfile/webapp/target/webapp.war to /home/slakshminarayanan/myagent/_work/r1/a/_kohlidevops.DevopsBasics (2)/warfile/webapp/target/webapp.war
2024-07-08T15:59:08.8748755Z Downloaded warfile/webapp/target/webapp.war to /home/slakshminarayanan/myagent/_work/r1/a/_kohlidevops.DevopsBasics (2)/warfile/webapp/target/webapp.war
2024-07-08T15:59:08.8750786Z Total Files: 1, Processed: 1, Skipped: 0, Failed: 0, Download time: 1.009 secs, Download size: 1573Bytes
2024-07-08T15:59:09.6135919Z Successfully downloaded artifacts to /home/slakshminarayanan/myagent/_work/r1/a/_kohlidevops.DevopsBasics (2)/
2024-07-08T15:59:09.6180375Z ##[section]Finishing: Download artifact - _kohlidevops.DevopsBasics (2) - warfile
```

Its downloading warfile/webapp/target/webapp.war to /home/slakshminarayanan/myagent/_work/r1/a/_kohlidevops.DevopsBasics (2)/warfile/webapp/target/webapp.war

```
r1 - release directory
a - artifact directory
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/d7b8c54c-2f3f-451f-bf66-8a27660f3017)

If I check the Agent machine, and check the downloaded path.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/9eb90503-88de-4e88-b63c-8f9899ac999e)

**To deploy application or artifact to configured Local agents if any build changes**

**To install Tomcat 9 on target agent machine**

SSH to ubuntu22 machine and perform below commands

```
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install zip unzip net-tools -y
sudo apt install default-jdk -y
cd /tmp
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz  (or) // https://tomcat.apache.org/download-90.cgi -> binary -> core -> tar.gz file
sudo mkdir /opt/tomcat
sudo tar xzvf apache-tomcat-9*tar.gz -C /opt/tomcat --strip-components=1
sudo chown ubuntu:ubuntu /opt/tomcat
sudo chmod +x /opt/tomcat/bin/*.sh
```

**To create a service for tomcat**

```
sudo nano /etc/systemd/system/tomcat.service

[Unit]
Description=Apache Tomcat Web Application Container
After=network.target
[Service]
Type=forking
User=ubuntu
Group=ubuntu
Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"
ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh
[Install]
WantedBy=multi-user.target

To reload and start the service

sudo systemctl daemon-reload
sudo systemctl start tomcat
sudo systemctl status tomcat
netstat -tnpl   //To ensure the tomcat listening port
```

Now I can able to access the tomcat default webpage with below URL

<img width="845" alt="image" src="https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/a306796a-70b0-4f63-84c7-ebdc0e3488db">

Now we have to copy the artifatcs from the r1/a/ directory to deploy this artifatcs to the agent machine. For this we need to create a one more stage with empty job in the Release Pipeline.

Navigate to your Release Pipeline and Edit the Pipeline to add the new stage with empty job and name it as CopyWARFileToSelfHostedEnvironment

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/5f1e8354-acf0-4511-815d-0c74db6f6ee8)

Your Agent Job should be mapped to 'Agent selection - Default'   //Because we have used Default while we installed Agent on target machine

```
Display name - Agent job
Agent Pool - Default
Leave others as default and save this Agent job
```

<img width="682" alt="image" src="https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/777e905d-daa4-49bf-8cd0-f736ac9ef6ce">

Click on the task and choose '+' symbol in the Agent job - Your Task would be 'Command Line'

Go to your Command Line script and add the below commands to work on it

```
sudo cd /home/slakshminarayanan/myagent/_work/r1/a/_kohlidevops.DevopsBasics\ \(2\)/warfile/webapp/target/   //This is my artifact directory
sudo cp webapp.war /opt/tomcat/webapps/
sudo systemctl restart tomcat.service
```

<img width="784" alt="image" src="https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/243cac3a-24e8-4170-9451-d9e4ed82caa9">

Now save your Pipeline and trigger a Release Pipeline to deploy the Artifatcs to Agent machine and this app expose to the external world using tomcaqt service.

<img width="767" alt="image" src="https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/6c7b1aba-522b-415e-9a8e-f9281aecb6f8">

Edit the Pipeline - Create release

<img width="800" alt="image" src="https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/d12a6eca-ce4e-479c-bfa9-7b7ffaf9612b">

My Release Pipeline has succeeded and its deployed Artifacts to the Agent machine.

<img width="731" alt="image" src="https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/26610c3a-22e8-4cf3-8cf7-c71822f328bb">

If I check with my Agent machine URL, I can see the java application through the browser

<img width="823" alt="image" src="https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline-Selfhosted/assets/100069489/0b3ec55d-c415-4620-ba34-8a5b4185e3e8">

That's it!
