# Assignment-on-Dockerizing-a-Plain-HTML-Page-With-Nginx

This guide provides a step-by-step walkthrough for Dockerizing a simple HTML page with Nginx and hosting the Docker image on Amazon Elastic Container Registry (ECR).

## **Prerequisites**

1. **Installed Tools:**

   - Docker
   - AWS CLI (configured with your credentials)

2. **AWS Account:**

   - Ensure you have access to Amazon ECR and proper IAM permissions.

3. **Basic Files Prepared:**

   - `index.html`: The HTML file for your application.
   - `nginx.conf`: The Nginx configuration file.
   - `Dockerfile`: Instructions to build your Docker image.

---

## **Steps**

### **Step 1: Create the HTML Page**

1. Open a text editor and create a file named `index.html`.
2. Add the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to Docker</title>
</head>
<body>
    <h1>Hello, Docker!</h1>
    <p>This is a simple HTML page served by Nginx in a Docker container.</p>
</body>
</html>
```

3. Save the file.

---

### **Step 2: Create the Nginx Configuration**

1. Create a file named `nginx.conf`.
2. Add the following content:

```nginx
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
```

3. Save the file.

---

### **Step 3: Create the Dockerfile**

1. Create a file named `Dockerfile` (no extension).
2. Add the following content:

```dockerfile
# Use the official Nginx image as the base image
FROM nginx:latest

# Copy the custom HTML and Nginx configuration files
COPY index.html /usr/share/nginx/html/index.html
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start Nginx when the container launches
CMD ["nginx", "-g", "daemon off;"]
```

3. Save the file.

---

### **Step 4: Build the Docker Image**

1. Open a terminal or command prompt.
2. Navigate to the directory containing `Dockerfile`, `index.html`, and `nginx.conf`.
3. Build the Docker image:

```bash
docker build -t simple-html-nginx .
```

---

### **Step 5: Run the Docker Container**

1. Start a container from the image:

```bash
docker run -d -p 8080:80 --name simple-html-container simple-html-nginx
```

2. Open your web browser and navigate to `http://localhost:8080`. You should see your HTML page.

---

### **Step 6: Push the Docker Image to Amazon ECR**

#### **1. Create a Repository in Amazon ECR**

1. Go to the [Amazon ECR console](https://aws.amazon.com/ecr/).
2. Navigate to **Repositories** > **Create repository**.
3. Select **Public repository**.
4. Provide a name, e.g., `simple-html-nginx`.
5. Click **Create repository**.

#### **2. Authenticate Docker to Amazon ECR**

Run the following command to authenticate Docker:

```bash
aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/<account_id>
```

#### **3. Tag the Docker Image**

Tag the image with your ECR repository details:

```bash
docker tag simple-html-nginx:latest public.ecr.aws/<account_id>/<repository_name>:latest
```

#### **4. Push the Docker Image**

Push the image to Amazon ECR:

```bash
docker push public.ecr.aws/<account_id>/<repository_name>:latest
```

---

### **Step 7: Verify the Docker Image on ECR**

1. Go to the [Amazon ECR console](https://aws.amazon.com/ecr/).
2. Navigate to your repository and verify that the image has been successfully uploaded.

---

## **Files in This Repository**

1. `index.html`: The HTML file served by Nginx.
2. `nginx.conf`: The configuration file for Nginx.
3. `Dockerfile`: Instructions to build the Docker image.

---

## **Useful Commands**

- **List Docker Images:**

  ```bash
  docker images
  ```

- **Stop a Running Container:**

  ```bash
  docker stop simple-html-container
  ```

- **Remove a Docker Container:**

  ```bash
  docker rm simple-html-container
  ```

- **Remove a Docker Image:**

  ```bash
  docker rmi simple-html-nginx
  ```

# Amazon Elastic Container Registry

public.ecr.aws/f8g8h5d4/ralph/docker
