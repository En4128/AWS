# Workshop Registration & Stripe Integration Service

A professional, containerized Node.js and Express web application for managing workshop registrations and processing secure payments using the **Stripe Checkout API**. This application is designed for cloud-native deployment on **AWS EC2**.

---

## 🛠️ Tech Stack & Key Technologies

### **Backend**
*   **Node.js (v18+)**: High-performance, asynchronous JavaScript runtime.
*   **Express.js**: Fast, minimalist web framework for routing and middleware.
*   **Dotenv**: Environment variable configuration management.

### **Frontend**
*   **HTML5 / CSS3**: Responsive layout styling (glassmorphism/vibrant designs) and clean semantic markup.
*   **Vanilla JavaScript**: Dynamic client-side routing, form handling, and API integration.
*   **Stripe.js SDK**: Secure client-side Stripe checkout routing.

### **Infrastructure & DevOps**
*   **Docker**: Containerization to ensure environment parity between development and production.
*   **AWS EC2**: Elastic Compute Cloud instance for staging and production hosting.
*   **Git**: Distributed version control system.

---

## 🚀 Key Features

*   **Secure Stripe Payment Flow**: Fully integrated Stripe Checkout redirecting to secure payment pages.
*   **Clean Separation of Concerns**: Client-side logic handles forms, while the Express backend handles secure interaction with Stripe APIs.
*   **Containerized Builds**: Standardized environment setup via Docker container runtime.
*   **Responsive UI**: Optimized styling for various device display sizes.

---

## 📂 Project Structure

```
├── client/                     # Frontend static assets and HTML files
│   ├── assets/                 # SVGs, icons, logos, and speaker photos
│   ├── css/                    # Modular stylesheets (style, cancel, success, workshops)
│   ├── workshops/              # Workshop registration pages
│   ├── app.js                  # Main landing page interactive logic
│   ├── success.html            # Post-successful-checkout page
│   └── cancel.html             # Checkout cancellation fallback page
├── Dockerfile                  # Multi-stage container instruction file
├── .dockerignore               # Patterns to ignore during Docker image build
├── .gitignore                  # Files and directories ignored by Git (e.g. node_modules, .env)
├── package.json                # Project dependencies and script runner configurations
├── server.js                   # Node/Express server and Stripe Checkout handler
└── README.md                   # Documentation guide
```

---

## ⚙️ Prerequisites

Before starting, ensure you have the following installed/configured:
*   [Node.js (v18.x or later)](https://nodejs.org/)
*   [Stripe Account](https://dashboard.stripe.com/register) (Retrieve your Stripe Developer API Keys: `Publishable key` & `Secret key`)
*   [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Optional, for containerized runner)

---

## 💻 Local Development Setup

### **1. Clone the Repository**
```bash
git clone https://github.com/En4128/AWS.git
cd AWS-Session
```

### **2. Setup Environment Variables**
Create a `.env` file in the root directory and populate it with your environment-specific configurations:
```env
DOMAIN="http://localhost:3000"
PORT=3000
STATIC_DIR="./client"

# Stripe Developer API Keys
PUBLISHABLE_KEY="pk_test_..."
SECRET_KEY="sk_test_..."
```
> ⚠️ **Security Warning:** Never commit the `.env` file or hardcode Stripe Secret Keys in the source files. Make sure they are kept inside your `.env` (which is excluded by git via `.gitignore`).

### **3. Install Dependencies & Launch Application**
```bash
# Install NPM packages
npm install

# Start in development mode (using Nodemon)
npm run devStart

# Alternatively, start in production mode
npm start
```
The server will boot up and be accessible locally at `http://localhost:3000`.

---

## 🐳 Running with Docker

You can package and run the application as a Docker container.

### **1. Build the Docker Image**
```bash
docker build -t workshop-stripe-app .
```

### **2. Run the Container**
Inject your environment variables at runtime using the `-e` flag:
```bash
docker run -d \
  -p 3000:3000 \
  -e DOMAIN="http://localhost:3000" \
  -e PORT=3000 \
  -e STATIC_DIR="./client" \
  -e SECRET_KEY="sk_test_your_secret_key" \
  --name workshop-app \
  workshop-stripe-app
```

---

## ☁️ AWS EC2 Deployment Guide

Follow these steps to host this application on an AWS Ubuntu server instance:

### **1. Configure AWS Infrastructure**
*   **Provision an EC2 Instance**: Use an Ubuntu Server AMI (e.g., `t2.micro` under Free Tier).
*   **Allocate Elastic IP**: Allocate an Elastic IP address and associate it with your EC2 instance. This secures a static public IP that you will use for your `DOMAIN` URL.
*   **Update Inbound Security Group Rules**: Add a Custom TCP rule to allow inbound traffic on Port `3000` (or `80` if using a reverse proxy) to make the application reachable.

### **2. SSH Into EC2 Instance**
Connect to your running virtual server using your SSH key pair:
```bash
ssh -i /path/to/your-key.pem ubuntu@<EC2_PUBLIC_ELASTIC_IP>
```

### **3. Configure Ubuntu Environment**
Once logged in, update packages and install Node.js:
```bash
# Update Apt package lists
sudo apt update && sudo apt upgrade -y

# Install git, nodejs and npm
sudo apt install git -y
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

### **4. Deploy & Run the Application**
Clone your repository, create the `.env` file, and run:
```bash
git clone https://github.com/En4128/AWS.git
cd AWS
npm install
```
Create the `.env` file with `DOMAIN` set to your EC2 Elastic IP:
```env
DOMAIN="http://<EC2_PUBLIC_ELASTIC_IP>:3000"
PORT=3000
STATIC_DIR="./client"
SECRET_KEY="sk_test_..."
```
Initialise and start the project
npm install
npm run start
NOTE - We will have to edit the inbound rules in the security group of our EC2, in order to allow traffic from our particular port

Project is deployed on AWS 🎉
The app will now be online and ready for traffic at `http://<EC2_PUBLIC_ELASTIC_IP>:3000`.
