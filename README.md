##  Basic setting 

        terraform version (Terraform v0.14.2)
        kubectl version --client (16+)
        
        export GOOGLE_APPLICATION_CREDENTIALS="/Users/sheelava/msashishgit/forage-ce-infra/service_accounts/qwiklabs-gcp-03-key.json"
        
        ** Ensure service account has owner access on GCP project
        gcloud init

##  Setting up infra using terraform dev workspace 
            
        --- outside ANZ n/w ---
        gcloud container get-server-config  (to know avaialble gke node version)

        For change in project Cleanup .terraform directory, lock and state files
        terraform init 
        terraform workspace new dev
        terraform workspace select dev  (if on some other workspace)
        terraform plan
        terraform apply
        terraform show
        terraform state list (List and show Terraform state file)
        terraform state show
        
        --- if you want to change project and SA ---
        say to qwiklabs-gcp-00-aa77c6f5d862
            <ensure proxy is unset and off n/w>
                gcloud config unset proxy/port
                gcloud config unset proxy/type
                gcloud config unset proxy/address
            gcloud init 
            edit variables.tf
            export GOOGLE_APPLICATION_CREDENTIALS=
        
##  Destroy        
        terraform destroy -auto-approve
        terraform destroy - target=module.cloudsql  (for module)
