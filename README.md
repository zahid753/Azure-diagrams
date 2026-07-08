Enterprise Web Application Hosting (VM-Based Architecture)

A highly available, VM-based web hosting environment on Microsoft Azure, built around a custom golden image, a Key Vault-backed secrets model, and an Application Gateway (WAF_v2) for load-balanced, centralized access.

Overview

This project provisions a full-stack web application (HTML frontend + Node.js backend) on Windows Server VMs, backed by an Azure SQL Database that is reachable only over a Private Endpoint. Instead of hardcoding database credentials, the VM uses a System-Assigned Managed Identity to pull secrets from Key Vault at runtime. Once the baseline server is validated, it is generalized with Sysprep and captured as a reusable custom image, which is then used to provision two additional identical web servers behind an Application Gateway for high availability.

Components:

LayerResourcePurposeNetworkingVirtual Network + 3 SubnetsIsolates Application Gateway, VM, and database tiersSecurityNetwork Security Group (VM subnet)Restricts inbound/outbound traffic to the compute layerComputeWindows Server VM (baseline)Hosts IIS + Node.js backend + HTML frontendImagingSysprep + Custom (Golden) ImageEnables repeatable, identical VM deploymentsCompute (scaled)2x Web VMs (from golden image)Backend pool members for high availabilityLoad BalancingApplication Gateway (WAF_v2)Single public entry point, traffic distribution, WAF protectionDatabaseAzure SQL Server & Database (Standard tier)Application data storeDatabase SecurityPrivate EndpointRemoves public network access to SQLSecretsAzure Key VaultStores SQL server name, DB name, username, passwordIdentity User-assigned Managed Identity (VM)Grants the VM scoped access to retrieve Key Vault secrets