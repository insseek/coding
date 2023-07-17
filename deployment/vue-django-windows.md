# Deploy Vue and Django Project on Windows IIS

## First: Django Project

Setting up Django on Windows Server 2016

Environment：Windows Server 2016   + SQL Server 2017 + Django2.1.15 + IIS10.0

### Setup the Settings

#### Setup sqlserver for django

* Install django-pyodbc-azure    https://github.com/michiya/django-pyodbc-azure

```shell
pip install django-pyodbc-azure
```

* Install  [Micosoft ODBC Driver for SQL Server](https://docs.microsoft.com/en-us/sql/connect/odbc/microsoft-odbc-driver-for-sql-server)
* DATABASES Settings

```ini
DATABASES = {
    'default': {
        'ENGINE': 'sql_server.pyodbc',
        'NAME': 'otis_c_dev',
        'USER': 'otis_innovation',
        'PASSWORD': 'otis$gear6688',
        'HOST': '0.0.0.0',
        'PORT': '1433',
        'OPTIONS': {
            'driver': 'ODBC Driver 17 for SQL Server'
        },
    },
}
```

#### Setup ldap for django

Microsoft Active Directory

* Install django-python3-ldap  https://github.com/etianen/django-python3-ldap

```shell
pip install django-python3-ldap
```

* Working principle

    * Authenticate users with an LDAP server.
    * Sync LDAP users with a local Django database.
    * Supports custom Django user models.

### Detailed steps

#### 1、Install Python

1. Download the latest Windows x86-64 executable installer from  [https://python.org](https://python.org/)
    1. https://www.python.org/downloads/release/python-368/
    2. [Windows x86 executable installer](https://www.python.org/ftp/python/3.6.8/python-3.6.8.exe)
2. Double-click the Python installer. If you get a security warning at this point, click “Run” to continue.
3. In the Python Setup dialog that appears, click “Customize Installation”
5. On the Optional Features step, leave the default settings and click “Next”
6. On the Advanced Options step, make the following changes:
    1. Check the “Install for all users” box (note that this will also check the “Precompile standard library” box)
    2. Check “Add Python to environment variables”
    3. In the input box below “Customize install location” change the value to the following: C:\Program File (x86)\
7. ![](https://leetan.oss-cn-beijing.aliyuncs.com/code/python-setup.png)
8. Click “Install”
9. When the installation is complete, click “Close”
10. Confirm the Python installation works by typing python —version on a new command prompt (open a new command prompt so that Env variables viz. PYTHONPATH is loaded). The command will output the Python version installed on your computer.

#### 2. Create and Configure a Python Virtual Environment

`cd C:\`
`mkdir virtualenvs`
`cd virtualenvs`
Create a virtual environment named `django_iis_demo_env` inside `virtualenvs`:
`python -m venv django_iis_demo_env`
Enter the newly created virutual environment and activate it:
`cd django_iis_demo_env`
`Scripts\activate.bat` # activate the virtual env
`python -m pip install --upgrade pip` # Upgrade pip

#### 3. Setup the Django Project

* Install Django and other project dependencies:
* Place your Django code directory inside the `django_iis_demo_env` directory.
* cd into your django code directory.
* Install the project dependencies:
    * `pip install -r requirements.txt`
* Install wfastcgi
    * `pip install wfastcgi`
* run `wfastcgi-enable`
    * "c:\virtualenvs\django_iis_demo_env\scripts\python.exe|c:\virtualenvs\django_iis_demo_env\lib\site-packages\wfastcgi.py" can now be used as a FastCGI script processor

[Image: image.png]wfastcgi is a WSGI similar in purpose to Gunicorn or uwsgi. `wfastcgi` is maintained by Microsoft and you would be better of using this than trying to compile other linux based WSGI servers on windows.
If your Django App is to be used on Windows only, you can consider putting `wfastcgi` in the requirements.txt file.

Reference: https://pypi.org/project/wfastcgi/

#### 4. Setup the SqlServer database for the Django Project

Install Microsoft SQL Server 2017
Create database and database user; this database user should be db_owner.
Install django-pyodbc-azure

```shell
pip install django-pyodbc-azure
```

* Install [Micosoft ODBC Driver for SQL Server](https://docs.microsoft.com/en-us/sql/connect/odbc/microsoft-odbc-driver-for-sql-server)
* Setup Django Project DATABASES

```ini
DATABASES = {
    'default': {
        'ENGINE': 'sql_server.pyodbc',
        'NAME': 'otis_innovation_dev',
        'USER': 'otis_innovation',
        'PASSWORD': 'otis$gear6688',
        'HOST': '0.0.0.0',
        'PORT': '1433',
        'OPTIONS': {
            'driver': 'ODBC Driver 17 for SQL Server'
        },
    },
}
```

#### 5. Make sure the Django Application is functional

Start the Django Development Server:
$ `python manage.py runserver`
This should start a functional Django Application on `localhost` — if you’ve kept the defaults, you’ll can see your Django site by visiting `http://127.0.0.1:8000/`
We aren’t done yet though. This is just the Development server running locally. We need to make the install ready for production…

#### 6. Install IIS with CGI

##### Installation Steps

Open Server Manager
Add roles and features

```
Open the Control Panel
In the search box in the top right, type “windows features” (without the quotes)
In the search results under “Programs and Features” click “Turn Windows features on or off.” This launches the Add Roles and Features Wizard.
```

On the “Before you begin” step, click “Next”
On the “Select installation type” leave the “Role-based or feature-based installation” radio button selected and click “Next”
On the “Select destination server” step, leave the current server highlighted and click “Next”
On the “Select server roles” step, scroll to the bottom of the list and check “Web Server (IIS)”
In the “Add features that are required for Web Server (IIS)?” dialog that appears, leave the “Include management tools (if applicable)” checkbox checked and click “Add Features”
On the “Select server roles” step, now that “Web Server (IIS)” is checked, click “Next”
On the “Select features” step, leave the defaults and click “Next”
On the “Web Server Role (IIS)” step, click “Next”
On the “Select role services” step, scroll down to “Application Development,” expand that section, and check the “CGI” box. This will also check the “Application Development” checkbox. With “CGI” checked, click “Next.”
On the “Confirmation” step, click “Install”
Once the installation completes, click “Close”
Close Server Manager

##### Verify the IIS Installation

Open a web browser on the server
Enter [http://localhost](http://localhost/) in the address bar and press Enter. You should see the default IIS page.
If you don’t see the default IIS page:
Open Control Panel
Type “services” in the search box
Under “Administrative Tools” click “View local services”
Scroll to the bottom of the list and ensure you see “World Wide Web Publishing Service” listed, and that the status is “Running”

##### TIP: To open the IIS Manager

Click the Windows button
Click on Administrative Tools
Double-click **Internet Information Services (IIS) Manager**

#### 7. Configure IIS to serve Django Applications

##### Configure FastCGI in IIS

In the project root directory, create a new web.config configuration file and put the following:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <modules>
            <remove name="WebDAVModule" />
        </modules>
        <handlers>
            <remove name="WebDAV" />
            <add name="django-innovation" path="*" verb="*" modules="FastCgiModule" scriptProcessor="E:\otis_innovation\virtualenvs\innovation_venv\Scripts\python.exe|E:\otis_innovation\virtualenvs\innovation_venv\lib\site-packages\wfastcgi.py" resourceType="Unspecified" requireAccess="Script" />
        </handlers>
        <security>
            <requestFiltering>
                <requestLimits maxAllowedContentLength="419430400" />
            </requestFiltering>
        </security>
        <httpErrors errorMode="Detailed" />
        <asp scriptErrorSentToBrowser="true"/>
    </system.webServer>
    <appSettings>
       <add key="PYTHONPATH" value="E:\otis_innovation\backend" />
       <add key="WSGI_HANDLER" value="django.core.wsgi.get_wsgi_application()" />
       <add key="DJANGO_SETTINGS_MODULE" value="innovation.settings" />
       <add key="WSGI_LOG" value="E:\otis_innovation\logs\wfastcgi.log" />
    </appSettings>
    <system.web>
        <httpRuntime executionTimeout="6000" maxRequestLength="419430400" />
        <customErrors mode="Off"/>
        <compilation debug="true"/>
    </system.web>
</configuration>
```

Reference: https://pypi.org/project/wfastcgi/

##### Create and Configure a New IIS Web Site

1. Open IIS Manager
2. On the left-hand side under Connections, expand the tree under the server name by clicking on the arrow to the left of the server name
3. Right-click on the Sites folder and click “Add Website …”
4. For the site name enter foo
5. For the physical path, type the following: c:\virtualenvs\django_iis_demo_env\backend
6. For the purposes of this example configuration, change the Port to 8080, since the Default site is running on port 80. For a real-world application you’ll likely want to use name-based virtual hosting by adding bindings and run the site on port 80.
7. You may leave the “Host name” blank. At this point the Add Website dialog should look like this:
8. [Image: image.png]

#### 8. Firewall Settings

1. You’re most probably going to access the server from a remote browser. To do this, you’ll need to open the Firewall ports.
2. This was accessible i.e. Port 80 was open.
3. Open  [http://40.73.23.109:8080/](http://40.73.23.109:8181/)
4. If this link is accessible, you’re done setting up. Else, you need to open the server ports.
5. Open Control Panel. Click on System and Security. Then click on Windows Firewall.
6. Select Advanced settings and highlight Inbound Rules in the left pane.
7. Right click Inbound Rules and select New Rule.
8. Add the port you need to open and click Next.
9. Add the protocol (TCP or UDP) and the port number into the next window and click Next.
10. Select Allow the connection in the next window and hit Next.
11. Select the network type as you see fit and click Next.
12. Name the rule something meaningful and click Finish.
13. Visit [http://40.73.23.109:](http://40.73.23.109:8181/)8080 again in a browser from the remote server, and everything should be active.

## Second: fronend

配置项：（如有更改配置，前端代码请重新build 生成新的dist目录） [Build Frontend Project: Deploy Vue and Django Project on Windows IIS](https://quip.com/oWvOAUJpMcua#DJGACA5iBOb)

```ini
文件：.env
    VUE_APP_BASE_URL=http://10.69.144.29:8181 
配置项说明：
    VUE_APP_BASE_URL 为后端API服务的地址
```

### New IIS Web Site

* 复制新代码 admin\dist 目录复制到   服务器上   比如目录：E:\otis_innovation\admin\dist
*  Create and Configure a New IIS Web Site
    * Open IIS Manager
    * On the left-hand side under Connections, expand the tree under the server name by clicking on the arrow to the left of the server name
    * Right-click on the Sites folder and click “Add Website …”
    * For the site name enter  innovation-admin
    * For the physical path, type the following: E:\otis_innovation\admin\dist
    * For the purposes of this example configuration, change the Port to  8383 (or other available ports, like 8484), since the Default site is running on port 80. For a real-world application you’ll likely want to use name-based virtual hosting by adding bindings and run the site on port 80.
    * You may leave the “Host name” blank. At this point the Add Website dialog should look like this:
* ![](https://leetan.oss-cn-beijing.aliyuncs.com/code/add-website.png)
* Setup web.config：
    * setup URL Rewrite：https://www.iis.net/downloads/microsoft/url-rewrite
    * Choose your site => URL Rewrite ---Add Rule(s)—Blank rules=>edit your rules
    * web.config configuration file like the following:
* ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
        <system.webServer>
            <rewrite>
                <rules>
                    <rule name="vue-history">
                        <match url=".*" />
                        <conditions>
                            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
                        </conditions>
                        <action type="Rewrite" url="/index.html" />
                    </rule>
                </rules>
            </rewrite>
        </system.webServer>
    </configuration>
    ```
    

### 静态media资源访问配置

如果需要访问一些文件资源目录  比如：E:\otis_innovation\media；对**media资源**的访问需要做一下配置
1、Add Virtual Directory
      Alias:  media
      Physcal path:E:\otis_innovation\media
2、Restart website

<img src="https://leetan.oss-cn-beijing.aliyuncs.com/code/add-virt-dir-1.png" />

<img src="https://leetan.oss-cn-beijing.aliyuncs.com/code/add-virt-dir-2.png" />

## References

### build frontend project

* 1、Download and Install *Yarn Or Npm：* [*https://yarn.bootcss.com/*](https://yarn.bootcss.com/)
* 2、Install  packages and Build

Cd in the project root directory, Install  packages  and run build.
If building is successfully, you can find the folder with a name “dist”.
Tip: Before build, please ensure that the configurations in the.env file are correct.

```bash
// 设置配置项 .env
VUE_APP_BASE_URL=http://10.69.144.29:8181
//安装依赖 执行build生成dist目录
yarn install
yarn build
```
