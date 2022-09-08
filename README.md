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
  - Copy the SSH connection to the repository:
    ex: ssh://devops.scmservice.eu-frankfurt-1.oci.oraclecloud.com/namespaces/abcdefgh/projects/oci-devops-instance-nginx/repositories/oci-devops-instance-nginx
  - Start the cloud console :
    - 
    
- Import this project
- Create a pipeline
  - use a managed build based on the above repository and buid_spec.yaml file
- Add 3 parameters to the pipeline
  - TF_VAR_tenancy_ocid (ex: ocid1.tenancy.oc1..aaaaaaaa4w...)
  - TF_VAR_compartment_ocid (ex: ocid1.compartment.oc1..aaaaaaaa...)
  - TF_VAR_region (ex: eu-frankfurt-1)
- Run the pipeline
- Check the output and the compute/instance console
