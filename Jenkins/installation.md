Jenkins

go run main.go &
go run test -v .
git clone <url>

CICD
	CI is code to package
	unit test, Integration test
	
	CD IS delivery
	Continuous Delivery - need approval
	Continuous Deployment - automatically on every push to a branch
	Authentication

sudo apt update -y
sudo apt install openjdk-11-jdk
ADD repo key to system
	wget -q -0 - <url> 
	sudo apt-key add -
	
	sudo sh -c 'echo deb <url>'
	sudo apt update -y
	sudo apt install jenkins
	
	sudo systemctl start jenkins
	systemctl status jenkins
	
	sudo ufw allow 8080   (to open up firewall)
	sudo ufw status		(will be inactive)
	exit
	
	copy paste the IP in the browser. (make sure port is open in Azure portal networking and open the port)

jenkins admin password will be available in /var/lib/jenkins/secrets/initialAdimPas

https://www.jenkins.io/doc/book/installing/linux/

sudo systemctl restart jenkins

make sure JENKINS_HOME is always backed up 
JENKINS_HOME -> configuration files(config.xml)
JENKINS_HOME -> jobs

plugin - thin back up
make sure to have a back_up dir with write permissions

sh 'cd /var/lib/jenkins/workspace/go-test && adminturneddevops/go-webapp-sample &'