Prerequisities: 
Login to your Azure Cloud Provider  
Select Billing Account under hamburger menu 
Create a Billing Account For ex: in this project we created  used Yusuf Acoount billing

Github 

Go to Github and create a repo for your project, dont forget to add .gitignore and README.md files 

This is group project, so add your collaborators into your project with their github names 

After adding them as collaborator, users will be able to add their SSH public keys to github successfully 

Users will be able to clone the project into their locals with git clone command 



RESOURCE GROUP + PROVIDER 
Create a resource group. Configure the Microsoft Azure Provider. 
Steps: 
Create Azure_Three_tier application Folder with .gitignore and README.md files
Under Azure_Three_tier Create a file  provider+rg.tf 
Use resource "azurerm_resource_group" "example"  to create resource group
Use resource provider "azurerm" features to create provider resource

```
# We strongly recommend using the required_providers block to set the
# Azure Provider source and version being used


terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=2.91.0"
    }
  }
}

# Configure the Microsoft Azure Provider
provider "azurerm" {
  features {}
}

# Create a resource group
resource "azurerm_resource_group" "wordpress" {
  name     = "wordpressResourceGroup"
  location = var.location
  tags     = var.tags
}


# Generates a random permutation of alphanumeric characters
resource "random_string" "fqdn" {
  length  = 6
  special = false
  upper   = false
  number  = false
}
```

 

VARIABLE.TF  
In this project we used variables to make our code more dynamic. Create a file variable.tf 
variable "location" {
  description = "The location where resources will be created"
  default     = "East US"
}

variable "tags" {
  description = "A map of the tags to use for the resources that are deployed"
  type        = map(string)

  default = {
    environment = "Test"
  }
}

variable "application_port" {
  description = "The port that you want to expose to the external load balancer"
  default     = 80
}

variable "admin_username" {
  description = "User name to use as the admin account on the VMs that will be part of the VM Scale Set"
  default     = "wordpress"
}

variable "admin_password" {
  description = "Default password for admin account"
  default     = "W0rdpr3ss@p4ss"
}

variable "database_admin_login" {
  default = "wordpress"
}

variable "database_admin_password" {
  default = "w0rdpr3ss@p4ss"
}

 

 

Vnet  module 
Next we created vnet.tf configuration file. Vnet is the Virtual Network which will include subnet that will associated withpublic ip. 

Steps: 
Use resource "azurerm_network_security_group" "example" to create the Vnet. 

Create vnet.tf file in folder with .gitignore and README.md files 

Use google_compute_network resource to create the vpc 

resource "azurerm_virtual_network" "wordpress" {
  name                = "wordpress-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = var.location
  resource_group_name = azurerm_resource_group.wordpress.name
  tags                = var.tags
}

resource "azurerm_subnet" "wordpress" {
  name                 = "wordpress-subnet"
  resource_group_name  = azurerm_resource_group.wordpress.name
  virtual_network_name = azurerm_virtual_network.wordpress.name
  address_prefixes     = ["10.0.2.0/24"]
}

resource "azurerm_public_ip" "wordpress" {
  name                = "wordpress-public-ip"
  location            = var.location
  resource_group_name = azurerm_resource_group.wordpress.name
  allocation_method   = "Static"
  domain_name_label   = random_string.fqdn.result
  tags                = var.tags
}



MAIN.TF  

 

 

 

 

DATABASE.TF  

 

OUTPUT.TF 

 

 
