## How to deploy Apache Superset on Windows Server (IIS)


### Pre-Requisites

Install Microsoft Visual C++ 14.x standalone: Build Tools for Visual Studio 2019 (x86, x64, ARM, ARM64)

Select latest version of MSVCv142 - VS 2019 C++ x64/x86 build tools

Select Windows 10 SDK

Install Python 3.7.x (3.10 doesnt work for me but you can try and comment   

Install PIP within the installer

Add Python 3.7 to PATH

Use CMDto execute below commands (Recommended)

### Installation

Ideally run these commands sequentially ...
# Create directory to host the files
mkdir E:\superset
cd /e E:\superset# Upgrade Setuptools & PIP
pip install --upgrade setuptools pip

# Create Virtual Environment named venv
python -m venv venv

# Activate Virtual Environment
venv\Scripts\activate

# Install Superset
pip install apache-superset

# Install DB Drivers - Postgres & MS SQL
pip install psycopg2
pip install pymssql
pip install pymysql

# Open Scripts folder to do superset related stuff
cd venv\Scripts

# Create application database
python superset db upgrade

# Create admin user
set FLASK_APP=superset
flask fab create-admin

# Load some data to play with (optional)
python superset load_examples

# Create default roles and permissions
python superset init

### Deploying on ISS:
pip install flask
pip install wfastcgi

After installing wfastcgi dont forget to enable it and place the same copy on the Application folder.
Make sure the website has the rights to all the folders including where your python is and where your application folder is, you can do is simply by going to folder settings and add "IIS AppPool\yourwebsitename" <- gives all the rights.

Follow this URL and do the exact steps as mentioned: https://medium.com/@dpralay07/deploy-a-python-flask-application-in-iis-server-and-run-on-machine-ip-address-ddb81df8edf3  

Create an "app.py" inside "C:\inetpub\wwwroot\FlaskApp" with following code
from superset.app import create_app
app = create_app()

@app.route("/")
def hello():
    return "Hello from FastCGI via IIS!"

if __name__ == "__main__":
    app.run()
Create "web.config" inside "C:\inetpub\wwwroot\FlaskApp" with following code<?xml version="1.0" encoding="utf-8"?>
<configuration>

<appSettings>
  <!-- Required settings -->
  <add key="WSGI_HANDLER" value="app.app" />
  <add key="PYTHONPATH" value="C:\inetpub\wwwroot\FlaskApp" />
</appSettings>

</configuration>
The folder should look like this now: 

![image.png](attachment:image.png)

#### Enjoy SuperSet on IIS 
