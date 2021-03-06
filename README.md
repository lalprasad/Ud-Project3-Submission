# Ensuring Quality Releases (Quality Assurance)

### High Level Steps for the project

- Creation of a resource group and storage account in azure to store a terraform state file.
- Publishing the provided package called FakeRestAPI as an artifact.
- Build the following azure resources using terraform:
	- Resource group
	- App service & App service plan
	- Network interface & Network security group
	- Public IP address
	- Linux based Virtual machine, Virtual network and Disc

- Deployment of FakeRestAPI as an azure app service.
- Running of postman/newman data validation tests against the http://dummy.restapiexample.com API 
- Publishing a selenium script (written in python) as an artifact.
- Installing selenium on the virtual machine created via execution of terraform and use it to run functional tests against the https://www.saucedemo.com website
- Perform stress and endurance test using Jmeter and upload the results as artifact.
- Setting up of email based alerting for the app service (manually in azure portal).
- Setting up custom logging in log analytics to gather selenium logs from the VM (manually in azure portal).

Note
1. All required inputs (including the public key for the VM, created via puttygen) is configured as variables in pipelines. 
2. Storagekey variable is kept as a 'placeholder', as it will be updated with the real value when the pipeline runs.
3. Replacetokens task of the pipeline takes care of replacing the variables in terraform files (.tf and .tfvars) during run time.
	
	
Creation of ServicePrincipal (with contributor role) is done with below command:
	```
	az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription ID>"
	```

From the results of the above command, we obtain the values for below variables as

	- appId is the client_id defined above.
	- password is the client_secret defined above.
	- tenant is the tenant_id defined above.


In-order for selenium based tasks to run on VM, we need to manually configure the VM to allow the pipeline to connect to it. In the Environments section of Pipeline (select Linux VM), we need to copy the registration key and execute in the VM (connect via  the private key corresponding to the public key used for creating the VM)



The project is organized into 3 stages. Refer the below screen shots for tasks in each of the stages

	1. Build
	2. Deploy
	3. Post-Deployment
	
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Azure%20Devops%20Pipeline/Jobs_Stages.PNG)

### Screen Shots

Create Storage for persisting state for Terraform
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Terraform/Create%20Storage%20for%20terraform.PNG)
Successful Terraform Init
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Terraform/Terraform%20Init.PNG)
Sucecssful Terraform Apply
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Terraform/Terraform%20Apply.PNG)
Note: All terraform files are uploaded in the github

Publish Newman test results as artifact
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Postman/Publish%20Newman%20test%20results%20as%20artifact.PNG)
Run Newman Postman Test
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Postman/Run%20Newman_postman%20test.PNG)

Successful Triggering of Email alert for HTTP 404 alerts for App services
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Log%20Analytics/Alert%20Enabling%20Email.PNG)
Custom Logging for selenium.logs
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Log%20Analytics/Custom_Alerting_Selenium_log_file.PNG)
Logs visible for log analytics workspace
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Log%20Analytics/Log%20Analytics.PNG)


Unfortunatly Jmeter Stress and Endurance tests are not working properly at my end. Hence attaching the sample results.
Sample Jmeter Dashboard
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/JMeter/Sample%20Jmeter%20Dashboard.PNG)
Successful completion of jmeter installation
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/JMeter/Install%20Jmeter%205_2_1.PNG)
Completion of Sample Endurance Test
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/JMeter/Successfully%20Run%20Endurance%20test.PNG)
Successful run of sample Jmeter test
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/JMeter/Successfully%20Run%20Jmeter%20Sample%20test.PNG)
Selenium Test Results
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Selenium/Selenium%20Test%20results.PNG)
Sample Test Results including pipeline execution status
![alt text](https://github.com/lalprasad/Ud-Project3-Submission/blob/main/Screenshots%20and%20Logs/Azure%20Devops%20Pipeline/Sample%20Test%20Results.PNG)

Azure Devops Pipeline yaml file,terraform files etc uploaded in git repo for review.

### References

	1. https://www.srijan.net/blog/manual-api-testing-using-postman
	2. https://medium.com/@gabriel.starczewski/jmeter-and-azure-pipelines-55f0594239ac
	3. https://medium.com/swlh/running-jmeter-load-tests-and-publishing-jmeter-report-within-azure-devops-547b4b986361
	4. https://azuredevopslabs.com/labs/vstsextend/selenium/
	5. https://cloudskills.io/blog/terraform-azure-devops
	6. https://medium.com/devcom/how-to-do-a-proper-website-stress-testing-using-jmeter-bd92171be4e3
	
