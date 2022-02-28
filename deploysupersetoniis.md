**How to deploy Apache Superset on Windows Server (IIS) **

**Pre-Requisites**

Install Microsoft Visual C++ 14.x standalone: Build Tools for Visual Studio 2019 (x86, x64, ARM, ARM64)

Select latest version of MSVCv142 - VS 2019 C++ x64/x86 build tools

Select Windows 10 SDK

Install Python 3.7.x (3.10 doesnt work for me but you can try and comment   

Install PIP within the installer

Add Python 3.7 to PATH

Use CMDto execute below commands (Recommended)

**Installation**

Ideally run these commands sequentially ...

:: Create directory to host the files
mkdir D:\superset
cd /d D:\superset

:: Check Versions
python --version
pip --version
systeminfo | findstr /C:"OS"

:: Upgrade Setuptools & PIP
pip install --upgrade setuptools pip

:: Create Virtual Environment named venv
python -m venv venv

:: Activate Virtual Environment
venv\Scripts\activate

:: Workaround - Upgrade Setuptools & PIP within venv
pip install --upgrade setuptools pip

:: Install Superset
pip install apache-superset

:: Install DB Drivers - Postgres & MS SQL
pip install psycopg2
pip install pymssql
pip install pymysql

:: Open Scripts folder to do superset related stuff
cd venv\Scripts

:: Create application database
python superset db upgrade

:: Create admin user
set FLASK_APP=superset
flask fab create-admin

:: Load some data to play with (optional)
python superset load_examples

:: Create default roles and permissions
python superset init

**Deploying on ISS: **
