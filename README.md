# DevSecOp

## Getting Started

Make sure Jenkins is installed and running, then change Jenkin's listening port to any port number apart from 8080, 8080 would be reserved for ZAP

### SonarQube
Adapated from [Jenkins-SonarQube Integration](https://medium.com/@amitvermaa93/jenkins-sonarqube-integration-129f5c49c4ca) by [Amit Verma](https://medium.com/@amitvermaa93)  

Before you start SonarQube, we recommend creating volumes to store SonarQube data, logs, temporary data and extensions. If you don't do that, you can loose them when you decide to update to newer version of SonarQube or upgrade to a higher SonarQube edition. Commands to create the volumes:

```bash
$> docker volume create --name sonarqube_data
$> docker volume create --name sonarqube_extensions
$> docker volume create --name sonarqube_logs
$> docker volume create --name sonarqube_temp
``` 

After that you can start the SonarQube server (this example uses the Community Edition):
```bash
$> docker run \
    -v sonarqube_data:/opt/sonarqube/data \
    -v sonarqube_extensions:/opt/sonarqube/extensions \
    -v sonarqube_logs:/opt/sonarqube/logs \
    -v sonarqube_temp:/opt/sonarqube/temp \
    --name="sonarqube" -p 9000:9000 sonarqube
```
[Check here for more detailed configuration options](https://github.com/SonarSource/docker-sonarqube/blob/master/examples.md#run-sonarqube-using-docker-commands)

#### SonarQube Integration with Jenkins
#### Step 1. 
Open SonarQube server- Go to Administration > click on Security > Users > Click on Tokens (Image 1)> Generate token with some name > Copy the token (Image 2), it will be used in Jenkins for Sonar authentication.

![Image 1](https://miro.medium.com/max/824/1*6QROeqXR8rxA36FcAhLCBw.png)  
Image 1  
![Image 2](https://miro.medium.com/max/634/1*sBkaHFrHgTWWwMjiTpg2Qg.png)  
Image 2  

#### Step 2. 
Setup SonarQube with Jenkins- Go to Manage Jenkins > Configure system > SonarQube server section > Add SonarQube > Name it, provide Server Url as http://<IP>:<port> > and authentication token copied from SonarQube Server > Apply and Save  

![Image 3](https://miro.medium.com/max/824/1*VIPfmzWA5IJXyLF2pqAHOQ.png) 

#### Step 3.
Step 3. Install SonarQube plugin to Jenkins. Go to Manage Jenkins > Manage Plugins > Available > Search for SonarQube Scanner> Install.

![Image 4](https://miro.medium.com/max/824/1*dUbYg1JQEcCamPv5bgWVyA.png) 

#### Step 4.
Download SonarScanner for your OS [here](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/) if you don’t have one already, then configure Sonar Scanner in Jenkins : Go to Mange Jenkins > Global Tool Configuration > Scroll for SonarQube Scanner > Add sonar scanner > name it, uncheck if you already have sonar else it will automatically download for you and your sonar scanner setup will be done(in my case I already have) > provide path to sonar runner home as in below image  

![Image 5](https://miro.medium.com/max/824/1*0h6C5ZolRGgBb80ghgaVfQ.png) 


### OWASP Dependency Checker

Adapted from [Integrating OWASP Dependency Check with Jenkins to CI/CD](https://gowthamr1.medium.com/integrating-owasp-dependency-check-with-jenkins-to-ci-cd-6f00e263fa78) by [Gowtham R](https://gowthamr1.medium.com)  

Integrating the dependency checker with Jenkins

#### Step 1: 
Download the OWASP-dependency-check plugin from plugin manager (Manage Jenkins -> Manage Plugins -> Available)

![Image 6](https://miro.medium.com/max/824/1*PHuqmlfTjb7-VBcDgoTWiQ.png) 

#### Step 2:
Post successful installation and jenkins service restart, navigate to Global tools configuration (Manage Jenkins -> Global Tools Configuration) to configure dependency check.
Click on ‘Add Dependency Check’ and provide name to convenience. The version by default will point to the latest release.

![Image 7](https://miro.medium.com/max/824/1*tkl60l7AmyKjOYMhJQj-5A.png)  

Note: If installation is not done properly, then the URL to download archive can be provided using ‘Extract *.zip/*.tar.gz’ option from Add installer dropdown.
From here, the dependency check can be directly invoked in a project or a pipeline script can be used to invoke it.

