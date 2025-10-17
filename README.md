
🧠 1. Virtual Cloud Lab: Sign-Up / Set-Up Virtual Cloud Lab

Goal: Create an account on any major cloud platform and set up your virtual lab.

Steps:

(a) Choose a Cloud Platform

You can pick any one:

Google Cloud Platform (GCP) → https://cloud.google.com/free

Amazon Web Services (AWS) → https://aws.amazon.com/free

Microsoft Azure (Students) → https://azure.microsoft.com/free/students/


(b) Sign Up

1. Click “Start Free” or “Free Tier.”


2. Create a new account using your college/student email ID.


3. Verify via phone number.


4. Add credit/debit card (no charge for free tier).


5. Accept the Terms & Conditions.



(c) Create a Project / Resource Group

GCP: Go to Manage Resources → Create Project.

AWS: Default project under “My Account.”

Azure: Create a Resource Group (like a folder for cloud resources).


(d) Provision One Resource

Example: Create a small Virtual Machine (VM) or Storage Bucket.

✅ Now your Virtual Cloud Lab is ready!


---

💻 2. Create a Cloud-Based Virtual Machine (VM)

Goal: Create and deploy a virtual server.

Using Google Cloud (example):

1. Open the GCP Console → Search Compute Engine → Click Create Instance.


2. Name your VM (e.g., vm-pravin).


3. Choose Region: asia-south1 (Mumbai).


4. Choose Machine type: e2-micro (free tier eligible).


5. Select Boot disk:

OS: Ubuntu 22.04 LTS



6. Check “Allow HTTP and HTTPS traffic.”


7. Click Create.


8. Once created, click SSH to connect to the VM terminal.


9. Test deployment:

sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2


10. Copy External IP → open in browser → you’ll see Apache default page.




---

☁️ 3. Create a Cloud Storage Bucket and Host a Static Website

Goal: Host a simple HTML website using cloud storage.

Steps:

1. In GCP Console → go to Cloud Storage → Buckets → Create Bucket.


2. Give a unique name: pravin-cloud-bucket.


3. Choose Region → asia-south1 (Mumbai).


4. Click Create.


5. Upload your files (e.g., index.html, style.css).


6. Go to Permissions → Add Principal:

Enter: allUsers

Role: Storage Object Viewer



7. Go to Website Configuration → set:

Main page: index.html

Error page: 404.html (optional)



8. Save and copy Public URL → open in browser. ✅ Your website is live!




---

🗃️ 4. Connect Google Cloud DB Service and Run SQL Queries

Goal: Create SQL database, connect, and run queries.

Steps:

1. GCP Console → SQL → Create Instance.


2. Choose MySQL.


3. Instance ID: student-db


4. Set root password.


5. Region: asia-south1


6. Click Create Instance.


7. After setup, click “Connect using Cloud Shell.”


8. Run SQL commands:

CREATE DATABASE college;
USE college;
CREATE TABLE student(id INT, name VARCHAR(30));
INSERT INTO student VALUES (1, 'Pravin'), (2, 'Amit');
SELECT * FROM student;



✅ You’ve connected, queried, and analyzed your cloud database.


---

🪣 5. Create Amazon S3 Bucket for Cloud Storage

Goal: Store and host files using Amazon S3.

Steps:

1. Go to AWS Console → S3 → Create Bucket.


2. Name: pravin-s3-bucket


3. Choose Region: Asia Pacific (Mumbai).


4. Disable “Block all public access” (for public hosting).


5. Click Create Bucket.


6. Upload files → index.html, style.css.


7. Go to Properties → Static website hosting → Enable.


8. Enter index document name: index.html.


9. Copy Bucket Website Endpoint URL and open in browser. ✅ Your website is now hosted on AWS S3.




---

⚙️ 6. Create Amazon EC2 Instances for Windows and Linux

Goal: Launch two EC2 instances — one Windows, one Linux.

Steps:

1. Go to AWS Console → EC2 → Launch Instance.


2. Linux Instance:

Name: linux-vm

AMI: Amazon Linux 2

Instance Type: t2.micro

Configure Key Pair → create or use existing.

Allow ports: 22 (SSH), 80 (HTTP).

Launch Instance.



3. Connect via SSH:

ssh -i key.pem ec2-user@<Public-IP>

Install Apache:

sudo yum install httpd -y
sudo systemctl start httpd


4. Windows Instance:

Name: windows-vm

AMI: Windows Server 2022.

Allow port 3389 (RDP).

Launch Instance → Get password → Connect via RDP.




✅ You now have Windows and Linux servers running on AWS.


---

🐳 7. Build a Container Application and Upload to Cloud

Goal: Build Docker app and upload image to cloud.

Steps:

1. Install Docker locally.


2. Create a file app.py:

from flask import Flask
app = Flask(__name__)
@app.route('/')
def home():
    return "Hello from Container!"
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)


3. Create Dockerfile:

FROM python:3.9
WORKDIR /app
COPY app.py .
RUN pip install flask
CMD ["python", "app.py"]


4. Build & run:

docker build -t myapp .
docker run -p 8080:8080 myapp


5. Push to Docker Hub:

docker tag myapp pravin/myapp
docker push pravin/myapp


6. Deploy it on cloud (Google Cloud Run / AWS ECS / Azure Container Apps).



✅ Containerized app deployed!


---

🌐 8. Deploy and Monitor Web App using Azure App Service

Goal: Deploy and monitor an app in Azure.

Steps:

1. Log in to Azure Portal.


2. Go to App Services → Create.


3. Choose:

Runtime stack: Python / Node.js

OS: Linux

Pricing plan: Free (F1)



4. Deploy code:

Option 1: Upload ZIP of your project.

Option 2: Connect GitHub repository.



5. Click Browse to open your site.


6. Enable monitoring:

Go to Application Insights → Enable.

Check performance metrics (response time, failures, logs).




✅ Your Azure web app is deployed and monitored.


---

🤖 9. Explore Azure ML Studio

Goal: Create and run a Machine Learning pipeline.

Steps:

1. Go to https://ml.azure.com/


2. Create a new workspace.


3. Open Designer → New Pipeline.


4. Add:

Dataset (e.g., Iris.csv)

Modules:

“Split Data”

“Train Model”

“Score Model”

“Evaluate Model”




5. Choose an algorithm (e.g., Decision Tree).


6. Connect modules in workflow.


7. Click Run.


8. After run completes, view:

Accuracy metrics

ROC curve

Confusion matrix




✅ You’ve successfully built and executed an ML pipeline.


---

📊 10. Visualize Data using Amazon QuickSight

Goal: Use AWS QuickSight to create visual dashboards.

Steps:

1. Go to QuickSight from AWS Console.


2. Choose Standard Edition (Free Trial).


3. Connect to Data Source:

S3, RDS, Redshift, or Upload CSV.



4. Click New Analysis → Create Dataset.


5. Import sample dataset (like sales.csv).


6. Create charts:

Bar Chart (Sales by Region)

Pie Chart (Revenue share)

Line Chart (Sales trend)



7. Add titles and publish dashboard. ✅ You’ve created a live cloud-based data visualization.




---

☁️🐳 11. Deploy Container App on Google Compute Engine

Goal: Deploy Docker container on GCP VM.

Steps:

1. Go to Compute Engine → VM Instances → Create Instance.


2. Scroll to Container Section → Check Deploy a container image.


3. Image URL: gcr.io/my-project/myapp:latest


4. Allow HTTP and HTTPS traffic.


5. Click Create.


6. Once VM is up, open External IP in browser.


7. You’ll see your containerized Flask .