Example:
- OCI Devops
  - Creation of a VCN, subnet and compute
  - Installation of NGINX on that compute
  - Copy the file index.html in /usr/share/nginx/html/

Usage:
- Login to the OCI cloud homepage.
- Create a notification topic
  Go to Menu - Developers Services / Applicaition Integration / Notifications
  Click "Create Topic"
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
    git push origin
    ````
    Start also the Cloud text editor (pen icon at the top right) to modify the files if needed.    

- Create a pipeline
  - use a managed build based on the above repository and buid_spec.yaml file
- Add 3 parameters to the pipeline
  - TF_VAR_tenancy_ocid (ex: ocid1.tenancy.oc1..aaaaaaaa4w...)
  - TF_VAR_compartment_ocid (ex: ocid1.compartment.oc1..aaaaaaaa...)
  - TF_VAR_region (ex: eu-frankfurt-1)
  - TF_VAR_tfstate_url (ex: https://objectstorage.eu-frankfurt-1.oraclecloud.com/p/xxxxxxxxxx/n/xxxxx/b/terraform-bucket/o/terraform.tfstate)
- Run the pipeline
- Check the output and the compute/instance console
