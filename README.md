# Dev_Ops-on-GCP
## step 1 (SETUP PROJECT AND NETWORK FIREWALL)
   - Create GCP account 
   - create project and attach that with billing account
   - Create custom VPC for this project ( enable API for compute engine ) and enable private google access
   - create Firewall Rule for port 22, 443, 80. source ip --> https://www.whatismyip.com/ and 35.235.240.0/20 for IAP
   - create instance tempelate. to create all vm from this tempelate 
## step 2 (JENKINS, TOMCAT SERVER ON CE, GITHUB CODE)
   - 'INSTALL JENKINS on Compute Engine and reserve static ip for this CE'
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
## step 4 (GIT PLUGIN FOR JENKINS)
   - go to jenkins-server terminla 
      - yum install git -y
   - go to jenkins UI and,   
   - `manage jenkins` > `Manage plugins` > `available` > search GitHub and install it
   - `manage jenkins` > `global tool configuration` > `git`
     - Name :git
       Path to Git executable : /bin/git
## step 5 (MAVEN PLUGIN FOR JENKINS)
   - go to jenkins-server terminla 
      - cd /opt
      - wget https://muug.ca/mirror/apache-dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
      - tar -xvzf apache-maven-3.6.3-bin.tar.gz 
      - mv apache-maven-3.6.3 maven
      - cd maven
      - vi ~/.bash_profile
        - ``` 
            # .bash_profile
            # Get the aliases and functions
            if [ -f ~/.bashrc ]; then
                    . ~/.bashrc
            fi
            # User specific environment and startup programs
            M2_HOME=/opt/maven
            M2=/opt/maven/bin
            JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.282.b08-1.el7_9.x86_64
            PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2:M2_HOME
            export PATH
      - exit
      - sudo su -
      - mvn --version      
   - go to jenkins UI and,   
       - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`
       - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Integration`      --> install both
   - Configure maven path
       - `Manage Jenkins` > `Global Tool Configuration` > `Maven`    
           - Name: M2_HOME
             MAVEN_HOME: /opt/maven
## step 6 (FIRST MAVEN BUILD JOB)
   - `New Item` > `maven project` > `build` 
   - `source code management` > `Git` > add link of hello world github repo > branch */main
   - `Build` > `Goals and options` > clean install package 
   - build job and see output

### This creat war file and now we need host this.
## step 7 (TOMCAT SERVER TO HOST WAR FILE)
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
        - INSTALL TOMCAT ON COMPUTE ENGINE
            - yum install -y wget
            - cd /opt
            - wget https://apache.mirror.colo-serv.net/tomcat/tomcat-8/v8.5.64/bin/apache-tomcat-8.5.64.tar.gz
            - tar -xvzf apache-tomcat-8.5.64.tar.gz
            - mv apache-tomcat-8.5.64 tomcat
            - cd tomcat/bin
            - ./startup.sh 
            - get URL of CE and add :8080 to see tomcat UI
            - add user
                 - find / -name context.xml --> to see all files with context.xml and edit all under webapp
                 - comment these two lines in all files
                      <!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
                             allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />               -->
            - ./shutdown.sh             
            - ./startup.sh 
            - now you can login using user and password, we are going to set it now
              - cd ..
              - vi conf/tomcat-users.xml
              - add these users
                ``` 
                <role rolename="manager-gui"/>
                <role rolename="manager-script"/>
                <role rolename="manager-jmx"/>
                <role rolename="manager-status"/>
                <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
                <user username="deployer" password="deployer" roles="manager-script"/>
                <user username="tomcat" password="s3cret" roles="manager-gui"/>
              - now go to UI and type username="tomcat" password="s3cret"
## step 8 (INTEGRATE TOMCAT ON JENKINS TO DEPLOY WAR FILE)
        - On jenkins UI install deploy to container plugin
            - `New Item` > `maven Project` 
            - in git add path for github repo
            - in build
                 - goals and option : clean install package
            - post build actions: Deploy war/ear to container
               - WAR/EAR files: **/*.war
               - container: tomcat: 8.x
               - now add user id and password for user which we made previously and also provide url for tomcat      
            - build job 
            - ## if got error see your firewall rule and make sure that jenkins server external ip allowed for tomcat server 
## step 9 (AUTOMATIC BUILD USING poll SCM)      
        - in previous job just select poll SCM under build triggers and do * * * * * (For every minute )      
        - now when there is change in code it will automatically deployed on tomcat server       