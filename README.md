Example:
- OCI Devops
  - Creation of a VCN, subnet and compute
  - Installation of NGINX on that compute
  - Copy the file index.html in /usr/share/nginx/html/

Usage:
- Create a OCI DevOps project
  Menu - Developers / DevOps project
- Create a project
- Create a repository
- Import this project
- Create a pipeline
  - use a managed build based on the above repository and buid_spec.yaml file
- Add 3 parameters to the pipeline
  - TF_VAR_tenancy_ocid (ex: ocid1.tenancy.oc1..aaaaaaaa4w...)
  - TF_VAR_compartment_ocid (ex: ocid1.compartment.oc1..aaaaaaaa...)
  - TF_VAR_region (ex: eu-frankfurt-1)
- Run the pipeline
- Check the output and the compute/instance console
- 
