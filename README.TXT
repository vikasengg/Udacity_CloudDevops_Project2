Instructions:
1. Unzip the folder as Project2 folder. 

2. Create the Network Stack: 
a) Update the Network Parameter file "Project2/network/network-params.json" for below inputs
    i) EnvironmentName: This will be used for appending the name while creating all the resources in the network Stack
    ii) CIDR Blocks for VPC, Private & Public Subnets
b) Execute the Project2/create_stack.sh with parameter being passed as "network"
    >> ./create_stack.sh network

3. Create the Bastion Instance in Public Network to be SSH from Local machine/your laptop: 
a) Create a S3 bucket with no Public access and upload the app server Key file into S3 bucket
b) Update the Bastion Parameter file "Project2/Bastion/bastion-params.json" for below inputs
    i) EnvironmentName: This will be used for appending the name while creating all the resources in the bastion Stack
    ii) myIPaddress : Your machine's/Laptop's Public I.P. Address 
    iii) myBastionEC2keyName: Bastion Key pair file name 
    iV) myAppserkeyName: EC2 Server Key Pair file name
    v)  myS3Bucket: This should be your bucket name along with location where the app server key file is placed.
c) Execute the Project2/create_stack.sh with parameter being passed as "bastion"
    >> ./create_stack.sh bastion

NOTE: I am not sure why but some of the times I wasn't able to do SSH using my public IP address.. not sure where I missed thing or if I changed someone drastically... 
However giving the CIDR block security attached to the instance as all 0's it does works... 


4. Create the EC2 Instance in private network to be SSHable only from Bastion Instance & listening on port 80 for incoming request from Load Balancer: 
a) Update the server Parameter file "Project2/server/server-params.json" for below inputs
    i) EnvironmentName: This will be used for appending the name while creating all the resources in the server Stack
    ii) AppserverMinSize: This parameter will be used in Auto scaling group for minimum servers required.  
    iii) AppserverMaxSize: This parameter will be used in Auto scaling group for maximum servers required.
    iV)  myAppserkeyName: EC2 Server Key Pair file name
    v) Server AMI and its instance Type
c) Execute the Project2/create_stack.sh with parameter being passed as "server"
    >> ./create_stack.sh server

5. Testing the application. 
a) Pick the DNS name from the OUTPUT variable of the server Stack for the Load Balancer.. With the current Execution the DNS name is 
http://udaci-AppSe-1CXC5P4GO4TQ2-296139249.us-west-2.elb.amazonaws.com
b) The page will be displayed as desired. 

6. If there are any updates required to be done for any of the 3 stacks created, please use the update_stack.sh script created above with the required parameters. 

7. Finally delete the stacks in the reverse order in which they had been created above. 
a) Server Stack
b) Bastion Stack
c) Network Stack