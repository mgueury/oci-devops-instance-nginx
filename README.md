Example:
- OCI Devops
  - Creation of a VCN, subnet and compute
  - Installation of NGINX on that compute
  - Copy the file index.html in /usr/share/nginx/html/

Usage:
- Login to the OCI cloud homepage.
- Create a ntification topic
  Go to Menu - Developers Services / Applicaition Integration / Notifications
  Click "Create Topic"
  - Name: TopicDevops
  - Create
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
    cd oci-devops-instance-nginx.git
    git remote set-url origin ssh://...( see *** ) 
    git pull origin --allow-unrelated-histories
    git push origin
    ````
    Start also the Cloud text editor (pen icon at the top right) to modify the files if needed.    

- Create a pipeline
  - use a managed build based on the above repository and buid_spec.yaml file
- Add 3 parameters to the pipeline
  - TF_VAR_tenancy_ocid (ex: ocid1.tenancy.oc1..aaaaaaaa4w...)
  - TF_VAR_compartment_ocid (ex: ocid1.compartment.oc1..aaaaaaaa...)
  - TF_VAR_region (ex: eu-frankfurt-1)
- Run the pipeline
- Check the output and the compute/instance console
