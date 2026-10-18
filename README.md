# Python Flask Application Deployment Using Jenkins and GitHub Webhooks

## 💡 Introduction

The **Python Flask Application** is a simple web application developed using Python and the Flask framework. It contains two routes that display messages in a web browser.

The application is deployed on a Linux server using Gunicorn as the WSGI server. GitHub is used to manage the source code, while Jenkins automates the build and deployment process. GitHub Webhooks can trigger Jenkins whenever changes are pushed to the repository.

This project demonstrates the basic implementation of a Python web application and its deployment using CI/CD concepts.

### Objectives

- Develop a web application using Python Flask.
- Create multiple routes to display different messages.
- Manage application code using Git and GitHub.
- Install Python dependencies using `pip`.
- Run the application using Gunicorn.
- Deploy the application on a Linux server.
- Integrate Jenkins with GitHub Webhooks to automate builds and deployment.

## 🏗️ Architectural Diagram

The following diagram illustrates the application's deployment architecture:

```text
                 Developer
                     |
                     | Push Code
                     v
                  GitHub
                     |
                     | GitHub Webhook
                     v
                  Jenkins
                     |
                     | Build & Deployment
                     v
                Linux Server
                     |
                     v
                Gunicorn Server
                     |
                     v
                Flask Application
                     |
             +-------+-------+
             |               |
             v               v
          Home Page        /hi Route
             |               |
             v               v
        Success Message   Flask & Docker
                            Message
```

### Architecture Components

- **Application – Python Flask:** Handles HTTP requests and returns responses to users.
- **Source Code Management – GitHub:** Stores the application source code and tracks changes.
- **CI/CD – Jenkins:** Automates the build and deployment workflow when configured with the required pipeline.
- **Webhook – GitHub Webhooks:** Notifies Jenkins about repository events, such as code pushes.
- **Server – Linux:** Provides the environment required to run the application.
- **WSGI Server – Gunicorn:** Runs the Flask application and handles incoming HTTP requests.
- **Virtual Environment – Python venv:** Provides an isolated environment for the application's Python dependencies.

## 📥 Input

The application receives HTTP requests through a web browser.

Users can access the application using the public IP address of the server and the appropriate port.

### Application Routes

The application routes.

| Route | URL | Description |
|---|---|---|
| Home Page | `http://<SERVER-PUBLIC-IP>:5000/` | Displays the deployment success message. |


### Input Process

1. The user opens a web browser.
2. The user enters the server's public IP address and port `5000`.
3. The browser sends an HTTP request to the Flask application.
4. Flask identifies the requested route.
5. The application returns the appropriate response.
6. The browser displays the response to the user.

### Application Code

The `app.py` file contains the Flask application.

```python
from flask import Flask
import os

app = Flask(__name__)

@app.route('/')
def hello_geek():
    return 'successfully deployed python application through jenkins!!!!!!!!!, added webhook'

@app.route('/hi')
def hell():
    return '<h1>Hiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiii from Flask & Docker</h1>'

if __name__ == "__main__":
    port = int(os.environ.get('PORT', 5000))
    app.run(debug=True, host='0.0.0.0', port=port)
```

### Code Explanation

- `Flask(__name__)` creates the Flask application.
- `@app.route('/')` defines the home page.
- `@app.route('/hi')` defines the second route.
- `os.environ.get('PORT', 5000)` reads the port from the environment or uses port `5000` by default.
- `app.run()` starts the Flask development server when the file is executed directly.

**Note:** The code above enables debug mode. For production deployment, disable debug mode and run the application using Gunicorn.

## 📤 Output

The application returns a different response depending on the route accessed by the user.

### 1. Successful Output — Home Page

**URL:**

```text
http://<SERVER-PUBLIC-IP>:5000/
```

The browser displays:

```text
successfully deployed python application through jenkins!!!!!!!!!, added webhook
```

This response indicates that the home route is responding to HTTP requests.

### 2. Successful Output — Flask & Docker Page

**URL:**

```text
http://<SERVER-PUBLIC-IP>:5000/hi
```

The browser displays the following heading:

**Hiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiiii from Flask & Docker**

This route demonstrates how Flask can return HTML content to the browser.

### Output Verification

To verify the deployment:

1. Confirm that the application process is running.
2. Open the server's public IP address in a browser.
3. Access port `5000` using the home page URL.
4. Verify the deployment success message.
5. Open the `/hi` route.
6. Verify that the Flask and Docker heading appears.

**Important:** The server's security group or firewall must allow inbound traffic on port `5000` for direct browser access. For production deployments, using Nginx as a reverse proxy with HTTPS is generally preferable.

## 🔄 CI/CD Workflow

This project demonstrates how Jenkins and GitHub Webhooks can be used together for automated deployment.

### Workflow

1. The developer modifies the Python application.
2. The updated code is pushed to the GitHub repository.
3. GitHub sends a webhook notification to Jenkins.
4. Jenkins receives the notification and starts the configured job or pipeline.
5. Jenkins retrieves the latest source code.
6. The pipeline installs dependencies and performs the configured build or verification steps.
7. The deployment stage updates or restarts the application on the Linux server, according to the pipeline configuration.
8. The user accesses the updated application through a web browser.

**Note:** The actual automation depends on the Jenkins job or pipeline configuration. A webhook alone does not perform deployment; Jenkins must be configured to execute the required steps.

## 🚀 Deployment Guide

### 1. Launch a Linux EC2 Instance on AWS

Launch an EC2 instance with a compatible Linux distribution.

Configure the security group to allow SSH access. If you intend to access the application directly, also allow inbound TCP traffic on port `5000` from an appropriate source.

### 2. Install the Required Packages

Run the following command on a compatible Amazon Linux instance:

```bash
sudo yum install python3 git python3-pip -y
```

These packages provide Python, Git, and pip.

### 3. Clone the GitHub Repository

```bash
sudo git clone https://github.com/Shubham-Dahake/python-project.git
```

Navigate to the project directory:

```bash
cd python-project/
```

### 4. Create a Python Virtual Environment

```bash
python3 -m venv myenv
```

Activate the virtual environment:

```bash
source myenv/bin/activate
```

The virtual environment isolates the application's Python packages from the system Python environment.

### 5. Install the Python Dependencies

Check the project files:

```bash
ls -la
```

View the dependency file:

```bash
cat requirements.txt
```

Install the dependencies:

```bash
python -m pip install -r requirements.txt
```

The dependency file should be named `requirements.txt` to match the installation command.

### 6. Run the Flask Application

For a basic development test, run:

```bash
python app.py
```

The application listens on port `5000` by default.

Open the following URLs in your browser:

```text
http://<SERVER-PUBLIC-IP>:5000/
http://<SERVER-PUBLIC-IP>:5000/hi
```

Stop the development server with `Ctrl+C` when you finish testing.

### 7. Run the Application Using Gunicorn

Make sure the virtual environment is activated and Gunicorn is installed.

Start Gunicorn with:

```bash
gunicorn --bind 0.0.0.0:5000 app:app
```

**Command explanation:**

- `gunicorn` starts the Gunicorn server.
- `--bind 0.0.0.0:5000` listens on port `5000` on all network interfaces.
- `app:app` refers to the Flask application object named `app` inside `app.py`.

Keep the process running while testing the application.

### 8. Configure Jenkins and GitHub Webhooks

To automate the deployment process:

1. Configure a Jenkins job or pipeline for the GitHub repository.
2. Configure the repository webhook to notify Jenkins when code changes are pushed.
3. Ensure Jenkins can access the repository and the deployment server.
4. Configure the pipeline to retrieve the latest code.
5. Install dependencies and perform the required verification steps.
6. Configure the deployment stage to update and restart the application.
7. Push a code change to GitHub and verify that Jenkins starts the configured job.
8. Check the application to confirm that the deployed version has been updated.

**Note:** The Jenkins pipeline configuration and webhook endpoint must be set up separately. The commands above describe manual application deployment and do not, by themselves, configure automated CI/CD.

## 🐧 Linux Commands Used

The following commands were used during the project setup and deployment.

| Command | Purpose |
|---|---|
| `sudo yum install python3 git python3-pip -y` | Installs Python, Git, and pip. |
| `sudo git clone <repository-url>` | Clones the GitHub repository. |
| `ls` | Lists files and directories. |
| `cd python-project/` | Navigates to the project directory. |
| `python3 -m venv myenv` | Creates a virtual environment. |
| `source myenv/bin/activate` | Activates the virtual environment. |
| `cat requirements.txt` | Displays the dependency file. |
| `python -m pip install -r requirements.txt` | Installs Python dependencies. |
| `vim requirements.txt` | Opens the dependency file for editing. |
| `python app.py` | Runs the Flask development server. |
| `gunicorn --bind 0.0.0.0:5000 app:app` | Runs the application using Gunicorn. |

**Note:** The `yum` command is appropriate for compatible Amazon Linux releases. Other Linux distributions may use a different package manager.

## 🏁 Conclusion

The Python Flask Application project demonstrates the development and deployment of a Python web application on a Linux server.

It provides practical experience with Python, Flask, Git, GitHub, Linux commands, virtual environments, dependency management, and Gunicorn.

By integrating Jenkins with GitHub Webhooks, the project also demonstrates the fundamentals of CI/CD automation, where repository changes can trigger a configured build and deployment workflow.

This project helped develop an understanding of how application code moves from a source code repository to a running server and how users access the deployed application through a web browser.

### Technologies Used

- **Programming Language:** Python
- **Web Framework:** Flask
- **Version Control:** Git
- **Code Repository:** GitHub
- **CI/CD Tool:** Jenkins
- **Automation Trigger:** GitHub Webhooks
- **Server Environment:** Linux / AWS EC2
- **WSGI Server:** Gunicorn
- **Dependency Management:** pip and Python Virtual Environment

## 👤 Author

**Shubham Dahake**

GitHub: [Shubham-Dahake](https://github.com/Shubham-Dahake)

Project Repository: [python-project](https://github.com/Shubham-Dahake/python-project)
