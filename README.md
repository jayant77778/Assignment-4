# Assignment-4

###   QUESTION -1 
Resources :
https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview
###   QUESTION -2 


### Scenario A: VNet with 2 Subnets

1. **Create Virtual Network (VNet)**:
   - Go to Azure Portal > Create a resource > Networking > Virtual network.
   - Enter details: Name: MyVNet, Address space: 10.0.0.0/16, Resource group: YourResourceGroup, Location: YourLocation.
   - Click Create.

2. **Create Subnets**:
   - Inside MyVNet, go to Subnets.
   - Click "+ Subnet".
   - Name: Subnet-1, Address range: 10.0.1.0/24.
   - Click "+ Subnet" again.
   - Name: Subnet-2, Address range: 10.0.2.0/24.
   - Click Save.

3. **Deploy Virtual Machines**:
   - In each subnet, deploy the desired VM:
     - Subnet-1: Linux VM and Windows VM.
     - Subnet-2: SQL DB (assuming Azure SQL Database).

4. **Configure Network Security Group (NSG)**:
   - For each subnet, configure NSG to allow necessary traffic (e.g., RDP, SSH, SQL traffic).

5. **Verify Connectivity**:
   - Test connectivity between VMs within the same subnet.
   - Test connectivity between VMs in different subnets.

### Scenario B: Hub and Spoke Architecture

1. **Create Virtual Networks**:
   - Management VNet: MyManagementVNet, Address space: 10.1.0.0/16.
   - Production VNet: MyProductionVNet, Address space: 10.2.0.0/16.
   - Testing VNet: MyTestingVNet, Address space: 10.3.0.0/16.
   - Developing VNet: MyDevelopingVNet, Address space: 10.4.0.0/16.

2. **Set Up Peering**:
   - For each VNet, set up peering with the Management VNet:
     - Management VNet to Production VNet.
     - Management VNet to Testing VNet.
     - Management VNet to Developing VNet.

3. **Deploy VMs**:
   - In each VNet, deploy VMs according to your requirements.

4. **Verify Connectivity**:
   - From a VM in the Management VNet, try to ping VMs in each of the other VNets.
   - Ensure network security groups (NSGs) allow necessary traffic for communication.

This setup ensures that the Management VNet acts as a central hub, and the other VNets (spokes) can communicate through it, creating a hub-and-spoke architecture.
#   By command line 
Certainly! Below are the commands to achieve both scenarios using Azure CLI:

### Scenario A: VNet with 2 Subnets

```bash
# Create Resource Group
az group create --name MyResourceGroup --location <location>

# Create VNet with 2 Subnets
az network vnet create --resource-group MyResourceGroup --name MyVNet --address-prefixes 10.0.0.0/16 --subnet-name Subnet-1 --subnet-prefix 10.0.1.0/24
az network vnet subnet create --resource-group MyResourceGroup --vnet-name MyVNet --name Subnet-2 --address-prefix 10.0.2.0/24

# Deploy VMs
az vm create --resource-group MyResourceGroup --name LinuxVM --image UbuntuLTS --vnet-name MyVNet --subnet Subnet-1 --admin-username azureuser --admin-password <password>
az vm create --resource-group MyResourceGroup --name WindowsVM --image Win2019Datacenter --vnet-name MyVNet --subnet Subnet-1 --admin-username azureuser --admin-password <password>

# Create SQL Database
# Assuming SQL DB deployment with appropriate Azure SQL Database commands
```

### Scenario B: Hub and Spoke Architecture

```bash
# Create Resource Group
az group create --name MyResourceGroup --location <location>

# Create VNets
az network vnet create --resource-group MyResourceGroup --name ManagementVNet --address-prefixes 10.1.0.0/16
az network vnet create --resource-group MyResourceGroup --name ProductionVNet --address-prefixes 10.2.0.0/16
az network vnet create --resource-group MyResourceGroup --name TestingVNet --address-prefixes 10.3.0.0/16
az network vnet create --resource-group MyResourceGroup --name DevelopingVNet --address-prefixes 10.4.0.0/16

# Create Peering Connections
az network vnet peering create --resource-group MyResourceGroup --name ManagementToProduction --vnet-name ManagementVNet --remote-vnet ProductionVNet --allow-vnet-access
az network vnet peering create --resource-group MyResourceGroup --name ManagementToTesting --vnet-name ManagementVNet --remote-vnet TestingVNet --allow-vnet-access
az network vnet peering create --resource-group MyResourceGroup --name ManagementToDeveloping --vnet-name ManagementVNet --remote-vnet DevelopingVNet --allow-vnet-access

# Deploy VMs in each VNet
# You can use similar az vm create commands as above to deploy VMs in each VNet.

# Verify Connectivity
# SSH into Management VM and ping VMs in other VNets using their private IP addresses.
```

Replace `<location>` and `<password>` with appropriate values. Also, ensure you have Azure CLI installed and authenticated with your Azure account.

###   Question 3 


### Internal Load Balancer:

1. **Navigate to the Azure Portal**:
   - Go to the Azure Portal at portal.azure.com.

2. **Create a New Resource**:
   - Click on "+ Create a resource" in the left menu.

3. **Search for Load Balancer**:
   - In the search bar, type "Load Balancer" and press Enter.

4. **Select Internal Load Balancer**:
   - Click on "Internal Load Balancer" from the search results.

5. **Fill in Load Balancer Details**:
   - Subscription: Choose your subscription.
   - Resource Group: Select or create a resource group.
   - Name: Enter a name for your Internal Load Balancer.
   - Region: Choose the region where you want to deploy the load balancer.
   - Virtual network: Select the virtual network where you want to deploy the load balancer.
   - Subnet: Choose a subnet within the selected virtual network.
   - IP address: Specify the IP address for the internal load balancer.

6. **Configure Backend Pool and Health Probes**:
   - Add backend pool by selecting the virtual machines or availability sets to be load balanced.
   - Configure health probes to check the health of your backend VMs.

7. **Review and Create**:
   - Review the settings and click "Create" to deploy the Internal Load Balancer.

### External Load Balancer:

1. **Navigate to the Azure Portal**:
   - Go to the Azure Portal at portal.azure.com.

2. **Create a New Resource**:
   - Click on "+ Create a resource" in the left menu.

3. **Search for Load Balancer**:
   - In the search bar, type "Load Balancer" and press Enter.

4. **Select External Load Balancer**:
   - Click on "External Load Balancer" from the search results.

5. **Fill in Load Balancer Details**:
   - Follow steps 5-7 from the Internal Load Balancer section, but ensure you choose the appropriate settings for an external load balancer, including a public IP address.

6. **Configure Frontend IP Configuration**:
   - For an external load balancer, you need to configure a public IP address as the frontend IP configuration.

7. **Review and Create**:
   - Review the settings and click "Create" to deploy the External Load Balancer.

###   QUESTION- 4

### Step 1: Create Application Gateway

1. **Navigate to the Azure Portal**:
   - Go to the Azure Portal at portal.azure.com.

2. **Create a New Resource**:
   - Click on "+ Create a resource" in the left menu.

3. **Search for Application Gateway**:
   - In the search bar, type "Application Gateway" and press Enter.

4. **Select Application Gateway**:
   - Click on "Application Gateway" from the search results.

5. **Fill in Application Gateway Details**:
   - Subscription: Choose your subscription.
   - Resource Group: Select or create a resource group.
   - Name: Enter a name for your Application Gateway.
   - Region: Choose the region where you want to deploy the Application Gateway.
   - SKU: Choose the appropriate SKU based on your requirements (Standard_v2 is recommended for most scenarios).
   - Tier: Select the appropriate tier (Standard or WAF).
   - Virtual network: Select the virtual network where you want to deploy the Application Gateway.
   - Subnet: Choose a subnet within the selected virtual network.

6. **Configure Backend Pool and HTTP Settings**:
   - Add backend pool by specifying the IP addresses or DNS names of the backend servers.
   - Configure HTTP settings like port, protocol, and cookie-based affinity.

7. **Configure Frontend IP and Listener**:
   - Define the frontend IP configuration, including a public IP address (if needed).
   - Add a listener with appropriate settings (port, protocol, and SSL if required).

8. **Configure Routing Rules**:
   - Define routing rules to route incoming requests to appropriate backend pools based on the request path or host header.

9. **Review and Create**:
   - Review the settings and click "Create" to deploy the Application Gateway.

### Step 2: Test Application Gateway

1. **Access Application Gateway's Public IP**:
   - Once the Application Gateway is deployed, note down its public IP address.

2. **Access Backend Service**:
   - Ensure that your backend service (e.g., a web server) is running and accessible within your virtual network.

3. **Test Access to Backend Service**:
   - Open a web browser and navigate to the public IP address of the Application Gateway.
   - You should be able to access your backend service through the Application Gateway.

4. **Test Load Balancing and Routing**:
   - If you configured multiple backend pools and routing rules, test the load balancing and routing by accessing different paths or hostnames defined in your routing rules.

5. **Monitor Application Gateway Metrics**:
   - After testing, monitor the performance and health of your Application Gateway using Azure Monitor metrics and logs.

By following these steps, you can create and test an Azure Application Gateway to manage and optimize traffic to your web applications.
###   QUESTION - 5

To set up a domain, configure a DNS server, and direct traffic to your server using the Azure CLI, you would follow these general steps:

### Step 1: Create a Resource Group

```bash
az group create --name MyResourceGroup --location <location>
```

### Step 2: Create a Virtual Network and Subnet

```bash
az network vnet create --resource-group MyResourceGroup --name MyVnet --address-prefixes 10.0.0.0/16 --subnet-name MySubnet --subnet-prefixes 10.0.0.0/24
```

### Step 3: Create a Virtual Machine

```bash
az vm create --resource-group MyResourceGroup --name MyVM --image UbuntuLTS --admin-username azureuser --generate-ssh-keys --vnet-name MyVnet --subnet MySubnet
```

### Step 4: Assign a Public IP Address to the VM (if needed)

```bash
az network public-ip create --resource-group MyResourceGroup --name MyPublicIP
az network nic ip-config update --resource-group MyResourceGroup --name ipconfigmyvm --nic-name MyVMVMNic --public-ip-address MyPublicIP
```

### Step 5: Install and Configure DNS Server on the VM

SSH into the VM and install a DNS server software such as BIND or dnsmasq.

### Step 6: Configure DNS Records

Add DNS records for your domain pointing to the public IP address of your VM.

### Step 7: Update DNS Server Settings

Update the DNS server settings in your virtual network to point to your DNS server.

```bash
az network vnet update --resource-group MyResourceGroup --name MyVnet --dns-servers <private-ip-of-your-dns-server>
```

### Step 8: Verify and Test

Verify that your domain resolves to the correct IP address by using tools like nslookup or dig. You can also try accessing your domain in a web browser to see if it loads your server.

Ensure that your DNS server is properly configured to handle incoming traffic and direct it to the appropriate resources on your VM.

These steps should guide you through setting up a domain, configuring a DNS server, and directing traffic to your server using the Azure CLI. Remember to replace placeholders like `<location>` and `<private-ip-of-your-dns-server>` with appropriate values.
