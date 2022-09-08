Example:
- OCI Devops
  - Creation of a VCN, subnet and compute
  - Installation of NGINX on that compute
  - Copy the file index.html in /usr/share/nginx/html/

Usage:
- Login to the OCI cloud homepage.
- Create a notification topic
  - Go to Menu - Developers Services / Applicaition Integration / Notifications
  - Click "Create Topic"
    - Name: TopicDevops
    - Create
- Create a remote terraform.tfstate
  - On your machine create a empty file. For ex with the command:
    ````
    touch terraform.tfstate
    ````
  - Go to Menu - Storage / Buckets
  - Create bucket 
    - Give a name. ex: terraform-bucket
     - Create
  - In the bucket upload the terraform.tfstate
  - Right click on the ... at the end of the uploaded file
  - Click Create Pre-Authenticated Request
  - Choose an expiration date in a far future.
    - Copy the URL: ex: https://objectstorage.eu-frankfurt-1.oraclecloud.com/p/xxxxxxxxxx/n/xxxxx/b/terraform-bucket/o/terraform.tfstate
- Create a devops project
  Go to Menu - Developers Services / DevOps / Projects
  - Click Create DevOps Project
    - Project name: oci-devops-instance-nginx
    - Select the Topic (TopicDevops)
    - Click "Create devops project"
  - In the homepage, click "Enable log" and follow the wizard
- Create a repository
  - Click Code repositories
  - Create Repository
    - name: oci-devops-instance-nginx
  - Click create
  - In the create screeen, look at the documentation (***) to create a connection with SSH to the git repository
    - In short, with your user, top/right icon, user name, create a API KEY
    - Start the cloud shell (icon at the top)
    ```
    mkdir .oci
    vi .oci/oci_api_key.pem
    (copy paste the private API key)
    ...
    -----BEGIN RSA PRIVATE KEY-----
    ...
    -----END RSA PRIVATE KEY-----
    ...
    vi .ssh/config
    (see doc ***)
    ...
    Host devops.scmservice.*.oci.oraclecloud.com
      User oracleidentitycloudservice/xxx@xxxx.com@xxxx
      IdentityFile ~/.oci/oci_api_key.pem 
    ...
    cd $HOME
    git clone https://github.com/mgueury/oci-devops-instance-nginx.git
    cd oci-devops-instance-nginx
    git remote set-url origin ssh://...( see *** ) 
    git pull origin --allow-unrelated-histories
    (exit vi :q)
    ````
  - Edit the file compute.tf (vi or the Cloud editor)
    - Look for terraform.tfstate and replace the URL by the one created above
    - ideally, you should replace the certificate id_devops_rsa and id_devops_rsa.pub
    ````
    rm id_devops_rsa id_devops_rsa.pub
    ssh-keygen -t rsa -f id_devops_rsa -N ''
    ````
    - commit the change to the git repository
    ````
    git add *
    git commit -m "tfstate url"
    git push origin
    ````
    
- Create a pipeline
  - Add a stage
    - type Managed Build
    - Name: build
    - Primary Code repository - Click Select
      - Type oci code repository
      - Select the repositoty
      - Click 'Save'
    - Click 'Add' 
  - Click parameters:
    - TF_VAR_tenancy_ocid (ex: ocid1.tenancy.oc1..aaaaaaaa4w...)
    - TF_VAR_compartment_ocid (ex: ocid1.compartment.oc1..aaaaaaaa...)
    - TF_VAR_region (ex: eu-frankfurt-1)
- Back to the Build Pipeline tab
  - Click Start Manual Run
- Check the output and the compute/instance console

````
ssh opc@xxx.xxx.xxx.xxx -i id_devops_rsa
curl http://localhost
<html>
<head>
 <title>DevOps</title>
</head>
<body>
  <h2>Hello from DevOps</h2>
</body>
</html>
````
