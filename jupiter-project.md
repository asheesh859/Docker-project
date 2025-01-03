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
 width="600" background-size="cover"/>

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
