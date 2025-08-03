# Terraform Static Website on AWS S3 - Project Documentation

## Project Overview

This Terraform project deploys a static personal portfolio website on AWS S3 with:
- A main portfolio page (index.html)
- A custom 404 error page (error.html)
- A profile image (profile.jpg)
- Proper public access configuration
- Website hosting configuration

## Prerequisites

Before using this Terraform project, ensure you have the following components set up:

### 1. AWS Account
- An active AWS account with proper permissions to:
  - Create and manage S3 buckets
  - Configure IAM policies
  - Manage public access settings

### 2. AWS CLI Installation

#### For Windows:
- Download the AWS CLI installer from [https://aws.amazon.com/cli/](https://aws.amazon.com/cli/)
- Run the installer and follow the prompts

#### For Linux/macOS:
```bash
curl "https://aws.amazon.com/cli/" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Verify installation
aws --version
```

### 3. AWS CLI Configuration
Configure your AWS credentials by running:
```bash
aws configure
```
You'll need to provide:
- AWS Access Key ID
- AWS Secret Access Key
- Default region name (use `us-east-1` for this project)
- Default output format (use `json`)

### 4. Terraform Installation

#### For Linux:
```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

#### For macOS (using Homebrew):
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

Verify Terraform installation:
```bash
terraform -version
```

### 5. Project Files
Ensure your project directory contains these files:
- `main.tf` - Main Terraform configuration file
- `variables.tf` - Terraform variable definitions
- `index.html` - Your portfolio webpage
- `error.html` - Custom 404 error page
- `profile.jpg` - Profile image for your portfolio

These files should be in the same directory when you run Terraform commands. The HTML files should contain your website content, and the profile image should be properly sized for web display.

### Verification Checklist
Before proceeding with Terraform deployment:
- [ ] AWS CLI installed and configured (`aws --version` works)
- [ ] Terraform installed (`terraform -version` works)
- [ ] All project files present in working directory
- [ ] AWS credentials have sufficient permissions
- [ ] Default region set to us-east-1 in AWS configuration

## Terraform Workflow Explanation

### 1. Initialization (`terraform init`)

The workflow begins with initialization which:
- Downloads the required AWS provider (version ~> 6.0)
- Sets up the backend (default local backend in this case)
- Prepares the working directory for other commands

### 2. Planning (`terraform plan`)

The planning phase:
- Analyzes the configuration files
- Compares current state with desired state
- Shows what resources will be created:
  - S3 bucket named "myterraformwebsiteprojectlubhit"
  - Bucket ownership controls
  - Public access configuration
  - Bucket ACL settings
  - Three file uploads (HTML files and image)
  - Website hosting configuration

### 3. Application (`terraform apply`)

When applied, Terraform:
1. Creates the S3 bucket with specified name
2. Configures bucket ownership to "BucketOwnerPreferred"
3. Sets public access permissions (making bucket publicly readable)
4. Applies public-read ACL to the bucket
5. Uploads the three files with proper content types and permissions
6. Configures the bucket for static website hosting:
   - Sets index.html as the index document
   - Sets error.html as the error document

### 4. Verification

After successful deployment:
- The website becomes available at the S3 website endpoint
- index.html serves as the main page
- Any invalid paths show the custom 404 page
- All resources are properly accessible due to public-read ACL

### 5. Destruction (`terraform destroy`)

When no longer needed, running destroy will:
- Delete all uploaded objects from the bucket
- Remove the bucket website configuration
- Delete the S3 bucket itself
- Clean up all other AWS resources created by this configuration

## Key Components Explained

### 1. S3 Bucket Configuration

```terraform
resource "aws_s3_bucket" "mybucket" {
  bucket = var.bucketname
}
```

Creates the foundation S3 bucket with the name from the variable.

### 2. Access Control

The combination of:
- `aws_s3_bucket_ownership_controls`
- `aws_s3_bucket_public_access_block`
- `aws_s3_bucket_acl`

Ensures the bucket has proper public read access while maintaining security best practices.

### 3. File Uploads

Three `aws_s3_object` resources handle uploading:
- index.html (main portfolio page)
- error.html (custom 404 page)
- profile.jpg (profile image)

Each with proper content types and public-read ACL.

### 4. Website Configuration

```terraform
resource "aws_s3_bucket_website_configuration" "website" {
  # Configures S3 static website hosting
  # with index and error documents
}
```

Enables the bucket to serve as a static website with proper document routing.

## Architecture

```
User Request
    ↓
S3 Bucket (Static Website Hosting)
├── index.html (Portfolio Page)
├── error.html (404 Page)
└── profile.jpg (Profile Image)
```

## Usage Instructions

1. Clone the repository
2. Ensure AWS credentials are configured
3. Run `terraform init`
4. Run `terraform plan` to verify changes
5. Run `terraform apply` to deploy
6. Access the website via the S3 website endpoint
7. When done, run `terraform destroy` to clean up

## Customization

To adapt this project:
1. Modify the HTML files for your content
2. Replace profile.jpg with your image
3. Update the bucket name variable
4. Add more S3 objects as needed

This implementation demonstrates infrastructure-as-code principles for deploying static websites on AWS S3 using Terraform, with proper access controls and website configuration.

