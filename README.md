##  Basic setting 

        terraform version (Terraform v0.14.2)
        kubectl version --client (16+)
        
        
        export GOOGLE_APPLICATION_CREDENTIALS="/Users/sheelava/msashishgit/forage-ce-infra/service_accounts/qwiklabs-gcp-03-key.json"
        
        ** Ensure service account has owner access on GCP project
        gcloud init

##  Setting up infra using terraform dev workspace 
            
        --- outside ANZ n/w ---
        gcloud container get-server-config  (to know avaialble gke node version)
        

        
        terraform init 
        terraform workspace new dev
        terraform workspace select dev  (if on some other workspace)
        terraform plan
        terraform apply
        terraform show
        terraform state list (List and show Terraform state file)
        terraform state show
        
##  Destroy        
        terraform destroy -auto-approve
        terraform destroy - target=module.cloudsql  (for module)
s