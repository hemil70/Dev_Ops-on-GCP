# Dev_Ops-on-GCP
## step 1
   - Create GCP account 
   - create project and attach that with billing account
   - Create custom VPC for this project ( enable API for compute engine ) and enable private google access
   - create Firewall Rule for port 22, 443, 80. source ip --> https://www.whatismyip.com/
   - create instance tempelate. Network tag --> 'my-server'