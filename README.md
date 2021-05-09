##  Basic setting 
        
        1) Install Cloud SDK, Terraform, kubectl
            
                terraform version (Terraform v0.14.2)
                kubectl version --client (16+)
                Google Cloud SDK 320.0.0
        
        2) Create service account in GCP project for terraform purpose
        
        3) Grant service account, owner access on GCP project
        
        4) Create service account key credential and set GOOGLE_APPLICATION_CREDENTIALS
            
                export GOOGLE_APPLICATION_CREDENTIALS="/Users/sheelava/msashishgit/forage-ce-infra/service_accounts/qwiklabs-gcp-key.json"
            
        5) Create service account in GCP project for interactions with Cloud SQL
        
        6) Grant access as Cloud SQL Client or Cloud SQL Admin or Cloud SQL Edit based on need
        
        7) Create Cloud SQL service account key 
        
        8) Initialise gcloud connectivity to the project
            
                gcloud init
        

##  Building infra on your GCP project
            
        1) Configurations for firewall, subnetwork and vpc:
                backend/firewall/main.tf
                backend/subnet/main.tf
                backend/vpc/main.tf
        
        2) Configurations for cloudsql
                cloudsql/main.tf
                
        3) Configurations for GKE cluster
                gke/main.tf
                
        4) Main terraform configuration
                main.tf
        
        5) Apply configuration to spinup infra

                terraform init 
                terraform workspace new dev
                terraform workspace select dev  (if on some other workspace)
                terraform plan
                terraform apply
 
               
## Validate infra spin-up using below
        
        1) Basic check
        
                terraform show
                terraform state list (List and show Terraform state file)
                terraform state show
        
        2) Validate sql instance created
            
                gcloud sql instances describe <instance name> --project <project id>
                
        3) Test connection to sql instance
            
                gcloud sql connect <instance name> --user=postgres --quiet
        
        4) Verify k8 objects
        
                kubectl get namespace
                kubectl get pod
                kubectl logs [POD] -c gke-test
                kubectl exec [POD] -- [COMMAND]
  
                
## How to manually configure kubectl:
          
         When we use gcloud commands, it adds cluster details to kubectl config  
                ex: at /users/sheelava/.kube/config
         
         If not, run below to add entry to kubectl config
         
                gcloud container clusters get-credentials <cluster name> --region=<cluster region>
 
        
## Create secrets for use by Application        
        
        1) Create Database secret on GKE
        
            kubectl create secret generic postgres-secret \
                --from-literal=username=postgres \
                --from-literal=password=ShreeGanesh1 \
                --from-literal=database=postgres
        
        2) Create cloussql service account secret on GKE
        
            kubectl create secret generic cloudsql-sa \
            --from-file=service_account.json=service_accounts/cloudsql-sa-key.json
        
        
## If you want to change project 
        
        0) Ensure proxy settings are clean 
            
            <ensure proxy is unset and off n/w>
                gcloud config unset proxy/port
                gcloud config unset proxy/type
                gcloud config unset proxy/address

        1) Reset GOOGLE_APPLICATION_CREDENTIALS with new key
                      
                export GOOGLE_APPLICATION_CREDENTIALS="/Users/sheelava/msashishgit/forage-ce-infra/service_accounts/qwiklabs-gcp-key.json"
                      
        2) Change existing gcloud config OR create a new one using new project id
            
                gcloud init 
                
        3) Terraform cleanup
                        
                Delete .terraform directory, lock and state files
                Change project-id, region in variables.tf
                Re-build infra
            
            
## To know available gke node version
        
                gcloud container get-server-config
        
## To Destroy infrastructure     
        
                terraform destroy -auto-approve
                terraform destroy - target=module.cloudsql  (for module)
