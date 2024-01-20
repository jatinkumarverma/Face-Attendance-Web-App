# Face Attendance Web App (React + Python)

This repository contains the code for a web application that utilizes face recognition for attendance tracking. The system is built using React for the frontend and Python (FastAPI) for the backend.

## Features

1. **Login and Logout:**

   - Users can log in and log out using face recognition to track attendance.

2. **User Registration:**

   - New users can be registered by uploading their images, which are then stored in the database along with their face embeddings.

3. **Attendance Logging:**

   - Attendance records are logged in CSV files with timestamps, indicating whether a user logged in or out.

4. **Webcam Access:**

   - The application allows access to the webcam for face recognition.

5. **Security:**
   - Nginx is used to proxy requests, and security group settings are configured to allow only necessary traffic.

## Technologies Used

### Backend

- **FastAPI:** A modern, fast (high-performance), web framework for building APIs with Python 3.7+.
- **OpenCV:** Open Source Computer Vision Library for image and video processing.
- **face-recognition:** A face recognition library built with dlib.
- **Starlette:** An asynchronous framework for building concurrent Python applications.

### Frontend

- **React:** A JavaScript library for building user interfaces.
- **npm:** A package manager for Node.js to manage project dependencies.
- **axios:** A promise-based HTTP client for making requests to the backend.

### Deployment

- **AWS EC2:** Cloud computing service to host the backend and frontend.
- **Nginx:** A web server used to serve the frontend and proxy requests to the backend.

## Backend Setup (FastAPI + face-recognition)

To set up the backend on an AWS EC2 instance:

1. **Launch EC2 Instance:**

   - Log into your AWS account and launch a `t2.2xlarge` EC2 instance, using the latest stable Ubuntu image.

2. **SSH into the Instance:**

   - SSH into the instance and update the software repository:

     ```bash
     sudo apt-get update
     sudo apt install -y python3-pip nginx
     ```

3. **Configure Nginx:**

   - Create an Nginx configuration file:

     ```bash
     sudo nano /etc/nginx/sites-enabled/fastapi_nginx
     ```

   - Add the following configuration (replace `<YOUR_EC2_IP>` with your EC2 instance's public IP):

     ```nginx
     server {
         listen 80;
         server_name <YOUR_EC2_IP>;
         location / {
             proxy_pass http://127.0.0.1:8000;
         }
     }
     ```

   - Restart Nginx:

     ```bash
     sudo service nginx restart
     ```

4. **Update Security Group:**

   - Update EC2 security group settings for your instance to allow HTTP traffic to port 80.

5. **Launch API Endpoints:**

   - Clone the repository:

     ```bash
     cd face-attendance-web-app-react-python
     cd backend
     ```

   - Install Python 3.8, create a virtual environment, and install requirements:

     ```bash
     sudo apt update
     sudo apt install software-properties-common
     sudo add-apt-repository ppa:deadsnakes/ppa
     sudo apt install python3.8
     sudo apt install python3.8-dev
     sudo apt install python3.8-distutils
     sudo apt install python3-virtualenv

     virtualenv venv --python=python3.8
     source venv/bin/activate

     pip install cmake==3.25.0
     pip install -r requirements.txt
     ```

   - Launch the app:

     ```bash
     python3 -m uvicorn main:app
     ```

## Frontend Setup (React)

The frontend can be hosted locally or on a server. Below are instructions for hosting it on a server.

### Setup Server

1. **Launch EC2 Instance:**

   - Launch a `t2.large` instance from AWS.

2. **SSH into the Instance:**

   - SSH into the instance and install Nginx:

     ```bash
     sudo apt-get update
     sudo apt install -y nginx
     ```

3. **Configure Nginx:**

   - Create an Nginx configuration file:

     ```bash
     sudo nano /etc/nginx/sites-enabled/frontend_nginx
     ```

   - Add the following configuration (replace `<YOUR_EC2_IP>` with your EC2 instance's public IP):

     ```nginx
     server {
         listen 80;
         server_name <YOUR_EC2_IP>;
         location / {
             proxy_pass http://127.0.0.1:3000;
         }
     }
     ```

   - Restart Nginx:

     ```bash
     sudo service nginx restart
     ```

4. **Update Security Group:**
   - Update EC2 security group settings for your instance to allow HTTP traffic to port 80.

### Launch App

1. **Clone Repository:**

   ```bash
   git clone https://github.com/computervisiondeveloper/face-attendance-web-app-react-python.git
   cd face-attendance-web-app-react-python
   cd frontend/face-attendance-web-app-front/
   ```

2. **Install Dependencies:**

   ```bash
   sudo apt-get install npm
   npm install
   ```

3. **Start the App:**

   ```bash
   npm start
   ```

4. **Adjust API URL:**

   - Edit the value of `API_BASE_URL` in `src/API.js` with the IP address of the backend server.

5. **Webcam Access:**
   - Adjust your browser settings to allow access to your webcam through an insecure connection from the EC2 IP address. In Chrome, adjust this setting at `chrome://flags/#unsafely-treat-insecure-origin-as-secure`.

## Contributing

Feel free to explore, modify, and contribute to this project!
