# Dev_Ops-on-GCP
## step 1 (SETUP PROJECT AND NETWORK FIREWALL)
   - Create GCP account 
   - create project and attach that with billing account
   - Create custom VPC for this project ( enable API for compute engine ) and enable private google access
   - create Firewall Rule for port 22, 443, 80. source ip --> https://www.whatismyip.com/ and 35.235.240.0/20 for IAP
   - create instance tempelate. Network tag --> 'my-server'
## step 2 (JENKINS, TOMCAT SERVER ON CE, GITHUB CODE)
   - 'INSTALL JENKINS'
        - INSTALL JAVA
            - $ sudo su -
            - yum install -y java-1.8*
            - SET HOME PATH FOR JAVA
                - find /usr/lib/jvm/java-1.8* | head -n 3 --> gives path for jre
                - Copy that path (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.282.b08-1.el7_9.x86_64)
                - cd ~
                - vi .bash_profile
                - add path in this. looks like

                        ``` 
                            # .bash_profile
                            # Get the aliases and functions
                            if [ -f ~/.bashrc ]; then
                                    . ~/.bashrc
                            fi
                            # User specific environment and startup programs
                            JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.282.b08-1.el7_9.x86_64
                            PATH=$PATH:$HOME/bin:$JAVA_HOME
                            export PATH
                - exit
                - sudo su -
                - echo $JAVA_HOME --> should be the java path            
        - INSTALL JENKINS 
            - yum install -y wget
            - sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
            - sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
            - yum install -y jenkins
            - service jenkins status  --> it might be stopped
            - service jenkins start  --> to start jenkins service
            - get public url of an instance and add:8080 to use jenkins UI
            - cat /var/lib/jenkins/secrets/initialAdminPassword --> to get password
            - Copy and paste thsi password in jenkins UI
            - Install all plugins
            - then creat user and password
        - SET JAVA HOME PATH IN JENKINS 
            - `Manage Jenkins` > `Global Tool Configuration` > `JDK`
                - NAME: JAVA_HOME
                  JAVA_HOME: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.282.b08-1.el7_9.x86_64 (Path which we get in java installation)
## step 3 (FIRST JENKINS JOB)
   - In jenkins UI
        - `New Item` > `Free style Project` > `build` > `Execute shell`
           -  Type echo "Hello"  --> then save 
        - `Build Now` --> to build job and then see output in console output window
           -  under build history if
            ```
               --> Blue ball = SUCCESS
               --> Red Ball = FAILED
               --> Orange Ball = Partial Success  


