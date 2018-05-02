# Kv-037.DevOps Project Demo I
## Deploying Spring PetClinic Sample Application localy using Vagrant

Create a deployment script for the **PetClinic** application. Use **Vagrant** to automate the process of creating the infrastructure for the deployment with **Virtualbox** (*preferably*) or **AWS** provider. As for provisioning you can choose to use **bash**, **python** or **ansbile** in any combination.

- Subtask I - Infrastructure
	* Describe *[two](https://www.vagrantup.com/docs/multi-machine/)* virtual machines using Vagrantfile for deployment of the application (codename **APP_VM**) and the database (codename **DB_VM**) 
	* Preferably use [private networking](https://www.vagrantup.com/docs/networking/private_network.html) feature for easy VM communication
	* VMs should be either Centos 7 (`bento/centos-7.1` or `bento/centos-7.4` box) or Ubuntu 16.04 (`ubuntu/xenial64` box)
	* If not using private networking then **APP_VM** should have port `8080` forwarded to host

- Subtask II - Database
	* Use any [provisioning](https://www.vagrantup.com/docs/provisioning/basic_usage.html) script that you created to install `MySQL 5.7.8` and any dependency on **DB_VM**
	* Customize the mysql database to accept connections only from your vagrant private network subnet
	* Create a non root user and password (codename **DB_USER** and **DB_PASS**) in mysql. Use host environment variable to set these values and pass them to the Vagrantfile using `ENV`
	* Create a database in mysql (codename **DB_NAME**) and grant all privileges for the **DB_USER** to access the database
	* Check if the mysql server is up and running and the database is accessible by created user (using either bash, python or ansible). If the databse is not accessible - break the provisoning process.

- Subtask III - Application
	* Create a non root user (codename **APP_USER**) that will be used to run the application on **APP_VM**
	* Use any provisioner to install `Java JDK 8`, `git` and any dependency on **APP_VM**
	* Clone [this repository](https://github.com/DmyMi/spring-petclinic) to the working folder (codename **PROJECT_DIR**)
	* Use the Maven tool to run tests and package the application. For more info you can use this [5 minutes maven](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) documentation. For convenience the project folder has Maven wrapper script (`mvnw`) that downloads and executes the required Maven binary automaticaly.
	* If testing and packaging is successful, then get the `*.jar` package from `$PROJECT_DIR/target` folder and place it in the **APP_USER** home folder (codename **APP_DIR**).
	* Set environment variables in **APP_VM** (preferable use the same environment variables passed from host machine using `ENV` as in **DB_VM**):
		* `DB_HOST` - IP of the **DB_VM**
		* `DB_PORT` - MySql port (default 3306)
		* `DB_NAME` - MySql database name
		* `DB_USER` - MySql user
		* `DB_PASS` - MySql user's password
	* Run the application with the **APP_USER** using the `java -jar` command
	* Create a script to wait for up to 1 minute to check if the application is up and running. Use application healthcheck endpoint `http://localhost:8080/manage/health` to see if response code is `200` and the status in the response body is up (`{"status":"UP"}`)

If everything is successful - you will see the PetClinic application on `$APP_VM_IP:8080`
	
# Kv-037.DevOps Project Demo II
## Setup Jenkins CI for Spring PetClinic Sample Application

- Fork the Application [repository](https://github.com/DmyMi/spring-petclinic) 
- Setup Jenkins
	* Deploy Jenkins on AWS Instance or Local VM
	* Setup Jenkins plugins (credentials, ssh-credentials, git, maven-plugin, github, ec2, ssh-slaves)
	* Create Jenkins Pipeline Job from project (extra task: Describe job using **Job DSL** syntax to create jobs automatically)
	* Setup job to look for **Jenkinsfile** in your project root directory
	* Setup EC2 plugin to be able to create AWS Workers to run the job.
	* Setup Maven global tools cofiguration
	* Make Sure the AWS Workers have other required tools (ansible, python, etc.)
        
- Describe a pipleline to build and deploy the application
	* Create a **Jenkinsfile** in your project root directory
	* Create pipeline steps to: checkout project from repository, build the application and run tests.
	* Create a step to launch **2** Instances on AWS and wait for them to be fully operational. Use either aws-cli or boto3 library.
	* Create a step to deploy the MySQL database to one of the instances using previously created ansible script
	* Create a step to deploy the application to other instance using previously created application deployment script
	* Create a step to check if the application is up and running
	* If every step is good - finish the job and archive the artifacts (\*.jar)
