# Dev_Ops-on-GCP
## step 1 (SETUP PROJECT AND NETWORK FIREWALL)
   - Create GCP account 
   - create project and attach that with billing account
   - Create custom VPC for this project ( enable API for compute engine ) and enable private google access
   - create Firewall Rule for port 22, 443, 80. source ip --> https://www.whatismyip.com/ and 35.235.240.0/20 for IAP
   - create instance tempelate. Network tag --> 'my-server'
## step 2 (JENKINS, TOMCAT SERVER ON CE, GITHUB CODE)
   - 'Install Jenkins'
        - Install Java
            - $ sudo su -
            - yum install -y java-1.8*
            - SET HOME PATH FOR JAVA
                - find /usr/lib/jvm/java-1.8* | head -n 3 --> gives path for jre
                - Copy that path (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.282.b08-1.el7_9.x86_64)
                - cd ~
                - vi .bash_profile
                - add path in this. looks like
                            ''' sh# .bash_profile
                            # Get the aliases and functions
                            if [ -f ~/.bashrc ]; then
                                    . ~/.bashrc
                            fi
                            # User specific environment and startup programs
                            JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.282.b08-1.el7_9.x86_64
                            PATH=$PATH:$HOME/bin:$JAVA_HOME
                            export PATH '''
        - Install Jenkins 
   -  


