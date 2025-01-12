# Docker File for Jupiter-Website and Three-Tier Architecture on AWS

## Step 1: Create GitHub Repository

1. Create a repository on GitHub named `Docker-project`.
2. Clone the GitHub repository to your local machine:
   ```bash
   git clone <repository-url>
   ```
3. Inside the cloned `Docker-project` folder, create a sub-folder named `Jupiter-website`.
4. Inside the `Jupiter-website` folder, create a file named `Dockerfile`.

---

## Step 2: Write the Dockerfile Commands

Add the following commands to your `Dockerfile`:

```dockerfile
# Use Amazon Linux as the base image
FROM amazonlinux:latest

# Install dependencies
RUN yum update -y && \
    yum install -y httpd && \
    yum search wget && \
    yum install wget -y && \
    yum install unzip -y

# Change directory
RUN cd /var/www/html

# Download web files
RUN wget https://github.com/azeezsalu/jupiter/archive/refs/heads/main.zip

# Unzip folder
RUN unzip main.zip

# Copy files into the HTML directory
RUN cp -r jupiter-main/* /var/www/html/

# Remove unwanted folders
RUN rm -rf jupiter-main main.zip

# Expose port 80 on the container
EXPOSE 80

# Set the default application to start when the container runs
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]
```

---

## Step 3: Push the Dockerfile to GitHub

Run the following commands in your terminal:

```bash
git add .
git commit -m "initial commit"
git push
```

---

## Step 4: Build and Run the Docker Image and Container

1. Build the Docker image:
   ```bash
   docker build -t jupiter .
   ```
   This command will build the Docker image. It consists of seven steps as outlined in the Dockerfile.
   <img src="https://github.com/user-attachments/assets/7c4b2fd4-045b-4d4d-bcf3-01a4d9f74604"
 width="800" background-size="cover"/>

3. Run the Docker container:
   ```bash
   docker run -dp 80:80 jupiter
   ```
   This will run your container on port 80.

4. Access the application:
   Open your browser and navigate to:
   ```
   http://localhost:80
   ```
   You should see the application output.

---

## Three-Tier Architecture for Docker Application on AWS

### Step 1: Create an IAM User

1. Open the IAM service on AWS.
2. Click "Users" and then "Create user".
3. Provide a user name (e.g., `user`).
4. Click "Next", select "Attach policies directly", and search for `AdministratorAccess`.
5. Attach the policy and create the user.

### Step 2: Generate Access Key

1. Go to the IAM dashboard and click on "Users".
2. Select the recently created user.
3. Navigate to the "Security credentials" tab, scroll down, and click "Create access key".
4. Choose the use case: `Application running on an AWS compute service`.
5. Optionally, add a description tag and then click "Create access key".
6. **Download the `.csv` file** containing the access key. Note: The access key can only be downloaded once. Ensure its status is `Active`.

### Step 3: Configure AWS CLI

1. Install the AWS CLI if it is not already installed:
   ```bash
   curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
   sudo installer -pkg AWSCLIV2.pkg -target /
   ```

2. Configure the AWS CLI with your credentials:
   ```bash
   aws configure
   ```
   - Enter the **Access Key ID** and **Secret Access Key** from the `.csv` file.
   - Specify your preferred region (e.g., `us-east-1`).
   - Set the output format (e.g., `json`).


 ### Step 3: Create a Three-Tier Architecture of VPC

#### Create VPC
1. Login to AWS and search for **VPC**.
2. Click on **Create VPC**.
   - **Name**: dev-VPC
   - **IPv4 CIDR**: `10.0.0.0/16`
   - Click **Create**.
3. Go to the **Actions** button and select **Edit DNS Hostnames**.
   - Enable **DNS Hostnames**.

---

### Step 4: Create and Attach an Internet Gateway

1. Navigate to **Internet Gateway**.
2. Click **Create Internet Gateway**.
   - **Name**: Dev Internet Gateway
3. Select the newly created gateway, click on the **Actions** button, and select **Attach to VPC**.
   - Search for **dev-VPC** and attach the internet gateway.

---

### Step 5: Create Subnets

#### Create Public Subnets
1. Navigate to **Subnets** and click **Create Subnet**.

   **First Public Subnet:**
   - **VPC**: Dev VPC
   - **Subnet Name**: Public Subnet AZ-1
   - **Availability Zone**: us-east-1a (N. Virginia)
   - **CIDR Block**: `10.0.0.0/24`
   - Click **Create Subnet**.

   **Second Public Subnet:**
   - **VPC**: Dev VPC
   - **Subnet Name**: Public Subnet AZ-2
   - **Availability Zone**: us-east-1b (N. Virginia)
   - **CIDR Block**: `10.0.1.0/24`
   - Click **Create Subnet**.

2. Enable Auto-Assign Public IP Addresses:
   - **Public Subnet AZ-1**:
     - Select the subnet and click **Actions** > **Edit Subnet Settings**.
     - Enable **Auto-assign IPv4 Public IP** and save changes.
   - **Public Subnet AZ-2**:
     - Repeat the same process.

---

### Step 6: Create and Configure a Public Route Table

1. Navigate to **Route Tables** and click **Create Route Table**.
   - **Name**: Public Route Table
   - **VPC**: Dev VPC
   - Click **Create Route Table**.

2. Allow Internet Access:
   - Select the Public Route Table and click **Edit Routes**.
   - Add a route:
     - **Destination**: `0.0.0.0/0`
     - **Target**: Internet Gateway > Dev Internet Gateway
   - Click **Save Changes**.

3. Associate the Route Table with Public Subnets:
   - Click **Edit Subnet Associations**.
   - Select both **Public Subnet AZ-1** and **Public Subnet AZ-2**.
   - Save the association.

---

### Step 7: Create Private Subnets

#### Create Application Subnets
1. Navigate to **Subnets** and click **Create Subnet**.

   **First Private App Subnet:**
   - **VPC**: Dev VPC
   - **Subnet Name**: Private App Subnet AZ-1
   - **Availability Zone**: us-east-1a (N. Virginia)
   - **CIDR Block**: `10.0.2.0/24`
   - Click **Create Subnet**.

   **Second Private App Subnet:**
   - **VPC**: Dev VPC
   - **Subnet Name**: Private App Subnet AZ-2
   - **Availability Zone**: us-east-1b (N. Virginia)
   - **CIDR Block**: `10.0.3.0/24`
   - Click **Create Subnet**.

#### Create Data Subnets

   **Third Private Data Subnet:**
   - **VPC**: Dev VPC
   - **Subnet Name**: Private Data Subnet AZ-1
   - **Availability Zone**: us-east-1a (N. Virginia)
   - **CIDR Block**: `10.0.4.0/24`
   - Click **Create Subnet**.

   **Fourth Private Data Subnet:**
   - **VPC**: Dev VPC
   - **Subnet Name**: Private Data Subnet AZ-2
   - **Availability Zone**: us-east-1b (N. Virginia)
   - **CIDR Block**: `10.0.5.0/24`
   - Click **Create Subnet**.

