# AzureDevOps-CICD-Pipeline-Selfhosted

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


