# Project overview

## Learning objectives
At the end of this lab, you will have deployed the Legit-Info solution in your local environment and started with the first step of modernization: containers. 

The solution uses the following technologies and frameworks:

* Python: The programming language, along with pip and pipenv supporting tools
* Django: A framework for web applications written in Python
* Bootstrap: A popular user interface toolkit, providing "responsive UI" to handle different screen sizes across smartphones, tablets, and desktops
* PDFminer: Extracts text from PDF
* SQLite: A simple, local database
* PostgreSQL: A robust, relational database
* Gunicorn: A WSGI HTTP webserver for Python
* NLTK: Natural Language Toolkit
* Watson Studio Natural Language Understanding

The following repositories are used to automate the collection of legislation:
* Legiscan.com: API for all 50 USA states
* AZLeg.gov: Arizona State Legislature website
* Ohio.gov: Ohio State website

# Prerequisites
This lab requires the following software to be installed on your machine. 
1. Git
2. Python (3.6 or above)
3. SQLite
4. A way to create and run Docker containers. For Windows and macOS, you might use Docker Desktop. For some Linux distributions, Docker Desktop is also offered, but you can also just run Docker Engine.

You can check if the prerequisites are installed using the following commands:

```bash
git --version
```

```bash
python3 --version && pip3 --version
```

```bash
sqlite3 --version
```

```bash
docker --version
```

# Install the prerequisites
## pipenv
**Pipenv** is a package management tool for Python. The lab uses it to automatically create and manage a virtual environment for your local project. 

```bash
pip3 install --user pipenv
```

You can check if `pipenv` was installed by checking the version:
```bash
pipenv --version
```

## SQLite3
The lab uses a local SQLite database to store tables such as:
* user
* user_groups
* user_profiles
* locations
* impacts

![](images/sqlite-dbs.png)


[SQLite](https://sqlite.org/index.html) is a small, fast, self-contained, high-reliability, full-featured, embedded SQL database engine. A complete SQL database with multiple tables, indices, triggers, and views is contained in a single disk file. Python comes with the sqlite3 module, so you don't have to install anything in the lab environment for you application to interact with the database.

# Clone the repository
The source code for Legit-Info lives on Github. You can either clone the source directory or `fork` the repository to create your own copy and then clone that.

1. Clone the Legit-Info repository:
```bash
git clone https://github.com/Call-for-Code-for-Racial-Justice/Legit-Info.git
```

2. Change into the project directory for next steps:
```bash
cd Legit-Info
```

# Set up Django environment
**NOTE**: this section will contain instructions for running on both Windows and macOS/Linux and will be indicated as needed.

## Create virtual environment
Create a Python virtual environment using pipenv:
```bash
pipenv shell
``` 

If the environment was created successfully, you should see output similar to this:
```bash
✔ Successfully created virtual environment! 
Virtualenv location: /home/theia/.local/share/virtualenvs/Legit-Info-_tFjoHw6
Launching subshell in virtual environment...
theia@theiadocker-upkar:/home/project/Legit-Info$  . /home/theia/.local/share/virtualenvs/Legit-Info-_tFjoHw6/bin/activate
```
You should also notice that the prompt changes to reflect the new virtual environment. You can type "exit" to leave the virtual environment.
```bash
(Legit-Info) theia@theiadocker-upkar:/home/project/Legit-Info$ 
``` 

## Install Python dependencies
To run the project, you first need to install the dependencies in the virtual environment. These dependencies are defined in `Pipfile` file. You can use `pipenv` as follows:

```bash
pipenv install
```

## Migrate database
Migrations are Django’s way of propagating changes you make to your models (adding a field, deleting a model, etc.) into your database schema. You should think of migrations as a version control system for your database schema. `makemigrations` will package up your model changes into individual migration files - analogous to commits - and migrate will apply those to your database.

First, use the `makemigrations` command to create the schema for tables used by the Django models:

**Linux and macOS**:
```bash
./stage1 makemigrations
```

**Windows**:
```
stage1.bat makemigrations
```

Second, use the `migrate` command to create these tables in the sqlite database:

**Linux and macOS**:
```bash
./stage1 migrate
```

**Windows**:
```
stage1.bat migrate
```

The SQLite database is stored in `db.sqlite3` in the root folder. This is a binary file and you will need an SQLite client to read it.

## Seed data
You will now seed some initial data so that some locations, impacts, and legislation show up when you visit the site.

First, seed the initial data from the `sources/cfc-seed.json` file:

**Linux and macOS**:
```bash
./stage1 syncdata sources/cfc-seed.json
```

**Windows**:
```
stage1.bat syncdata sources\cfc-seed.json
```

You should see output as follows:
```bash
(Legit-Info) theia@theia-upkar:/home/project/Legit-Info$ ./stage1 syncdata sources/cfc-seed.json
**Using SQLite3**
Installing json fixture 'sources/cfc-seed' from absolute path.
Installed 25 objects from 1 fixture
```

Second, add the legislation data from the `sources/cfc-law30.json` file:

**Linux and macOS**:
```bash
./stage1 syncdata sources/cfc-law30.json
```

**Windows**:
```
stage1.bat syncdata sources\cfc-law30.json
```

You should see output as follows:
```bash
(Legit-Info) theia@theia-upkar:/home/project/Legit-Info$ ./stage1 syncdata sources/cfc-law30.json 
**Using SQLite3**
Installing json fixture 'sources/cfc-law30' from absolute path.
Installed 30 objects from 1 fixture
```

Let's look at the legislation data we just added:
![](images/sqlite-laws.png)

## Create superuser
We will create a superuser user in this step. The superuser has access to the admin pages and is able to:
* Manage users
* Create new locations
* Edit existing locations
* Create new impacts
* Edit existing impacts

Use this command to start the process:

**Linux and macOS**:
```bash
./stage1 createsuperuser
```

**Windows**:
```
stage1.bat createsuperuser
```

You will be asked to fill in the username, name and password. The `cfcadmin` username is already taken. You can use any other username.
```bash
(Legit-Info) theia@theiadocker-upkar:/home/project/Legit-Info$ ./stage1 createsuperuser
**Using SQLite3**
Username (leave blank to use 'root'): cfcadmin007
Email address: example@gmail.com
Password: 
Password (again): 
Superuser created successfully.
```

# Run the server locally
**NOTE**: this section will contain instructions for running on both Windows and macOS/Linux and will be indicated as needed.

To run the server locally, you can again use the `stage` utility as follows:

**Linux and macOS**:
```bash
./stage1
```

**Windows**:
```
stage1.bat
```

You should see the following output:
```bash
**Using SQLite3**
**Using SQLite3**
Watching for file changes with StatReloader
05-04 18:57:09 autoreload:run_with_reloader [INFO] Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
May 04, 2021 - 18:57:09
Django version 3.1.8, using settings 'cfc_project.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

## Run and launch the application
Open a browser and navigate to `http://localhost:3000`. You should see the application running.

![](images/legit-info-tab.png)

Now that we have the application running, let's explore it. 

# Explore the application
The application has a top menu:
* Search
* Locations
* Impacts
* Sign out
  
## Explore locations
Click on the `Locations` link on the top menu bar. You should see the following locations:
```
United States
└─ Arizona
    └─ Maricopa County
        └─ Glendale, Arizona
        └─ Phoenix, Arizona
        └─ Scottsdale, Arizona
    └─ Pima County
        └─ Tucson, Arizona
└─ Ohio
    └─ Cuyahoga County
        └─ Cleveland, Ohio
    └─ Franklin County
        └─ Columbus, Ohio
    └─ Hamilton County
        └─ Cincinnati, Ohio
```

## Explore impacts
Click on the `Impacts` link on the top menu bar. You should see the following impacts:
```
* Healthcare
* Safety
* Environment
* Transportation
* Jobs
```

## Register yourself as a new user
Click on the `Register` link on the top menu bar to create a new account for yourself.

![](images/user-register-1.png)

## Admin interface
Django has a built-in administrator interface that you can access from the top URL menu. It reads metadata from your models to provide a quick, model-centric interface where trusted users can manage content on your site. Click on the `Sign in` link to launch the admin interface and log in with the `superuser` username and password you created earlier.

![](images/admin-sign-in.png)

Once logged in, click on the `Admin` link to launch the administrative interface.

![](images/admin-link.png)

You can search for a law by clicking on `Laws`

![](images/laws-search-1.png)

Try searching for laws that have the word `health` in them.
![](images/laws-search-2.png)

Explore the different filters on the left side to filter laws by impact or location. You will deploy the application to IBM Cloud using the Command Line Interface (CLI) from the terminal in the next step. Press "Ctrl-C" to terminate the running application. 

# Containerize the application
We will now build and run the application from a container using Docker. 

## What are containers and why use them?
Containers are an abstraction that bundles your code files and runs them in an isolated environment that you are certain will have the correct code, framework, and library versions your application requires. Containers also, in most cases, require less resources to execute and are smaller compared to virtual machines or baremetal, allowing you to run more applications. This allows you to, for example, run several containers that use different versions of Python or Django all next to each other without any complications.

Containers are the building blocks for microservices and multi-cloud deployments as they are standardized and very portable. You can create, replicate, and destroy containers very quickly, allowing for faster development cycles and continuous deployments. The sky is the limit!

## Inspect the Dockerfile
Open the Dockerfile in the root of the project directory. Let's look at each line and discuss it's purpose.

`FROM registry.access.redhat.com/ubi8/python-38:1-71.1634036286` - Specifies the base container image to utilize. In this case, we are using the Python 3.8 variant of the RedHat Universal Base Image. This image is also locked in at a particular release version, which is preferable for stability in production environments.

`WORKDIR /opt/app-root/src` - This sets the working directory for the container. This directory is created if it doesn't already exist.

`COPY --chown=1001:0 . .` - This copies the files from the project directory into the container and changes ownership to the non-root user (1001).

`RUN chmod -R g=u .` - This changes the group ownership of all the copied files to the non-root user (1001).

`USER 1001` - Changes the container to run as the non-root user.

```
ENV LC_ALL=C.UTF-8 \
    LANG=C.UTF-8 \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONFAULTHANDLER=1
```
Sets various environment variables:

* `LC_ALL=C.UTF-8` - Overrides the localization settings
* `LANG=C.UTF-8` - Sets the language of the container
* `PYTHONDONTWRITEBYTECODE=1` - Prevents Python from writing `.pyc` files on the import of source modules
* `PYTHONFAULTHANDLER=1` - Enables the fault handler in order to dump the Python traceback

`RUN pip install --no-cache-dir --upgrade pip==21.3.1 && pip install --no-cache-dir pipenv==2018.11.26 && pipenv install --system --dev` - Installs theh packages using specific versions of `pip` and `pipenv`

`EXPOSE 8080` - Exposes port 8080 of the container to the host

`ENTRYPOINT ["sh", "entrypoint.sh"]` - The default entrypoint script that starts the container

`CMD ["gunicorn", "-b", "0.0.0.0:8080",  "--env", "DJANGO_SETTINGS_MODULE=cfc_project.settings", "cfc_project.wsgi", "--timeout 120"]` - This is the command that will run our code using `gunicorn`

## Build the container image
First, let's ensure that the `docker` command is available in your terminal window. Type `docker --version` and you should get output that should be similar to `Docker version 20.10.7, build 20.10.7-0ubuntu5~18.04.3`.

To build the Dockerfile into a new container image, run `docker build . -t legit-info`. This command tells Docker to build in the current directory with a tag of `legit-info`. 
You will see some output that downloads the base image and layers each subsequent command in the Dockerfile on top of it. 

After the build finishes, you can run `docker images` to see the list of container images. You should see a line for the `legit-info` image we just built that looks similar to:
```
REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
legit-info       latest    6c76b5885c97   15 seconds ago  1.21GB
```

## Run the containerized application
Now, you can run the container with the following command: `docker run -p 8080:8080 legit-info`. This command will start the container image `legit-info` that we built before and connects the container's 8080 port to the host's 8080 port.

To see the application running from the container, navigate to `http://localhost:8080` in your browser. 

After you have verified the application is running, go back to your terminal and press `Ctrl-C` to stop the container from running.

## Persisting the data between container starts
Now that we finally have our application running in a container, we need to address the database. 

In the previous steps, we worked with a SQLite database file and our Dockerfile ultimately copies the SQLite database file into the container. Containers are ephemeral and stateless, so this means any changes to the information in the SQLite database file will be lost when the container stops. 

This can be addressed in a couple different ways:
* We can mount the SQLite database file from the host system using a volume. This would allow the container to use the same SQLite database file every time.
* We can configure the application to use a more robust database system, such as PostgreSQL and run that either in another container, or via a managed service like IBM Cloud Databases for PostgreSQL.

For now, we will focus on the first point: creating a Docker volume for the database file to persist between sessions.

We can create a Docker named volume with the command `docker volume create legit-info-db`. To use this volume, we run a similar `docker run` command from above and add a new flag (`-v`) to tell the container to use the named volume we created. 

The command is `docker run -p 8080:8080 -v legit-info-db:/opt/app-root/src legit-info`. The parameter for the `-v` flag contains the name of the volume (`legit-info-db`) and the directory inside the container where the file is located (`/opt/app-root/src`). If you remember, our Dockerfile contains a line that sets the working directory, and that is the directory we need in this command.

You can verify the application is running again by navigating to `http://localhost:8080`.

### (OPTIONAL) Verify our database information persists
We can perform a validation step by logging in as our adminstrator account we created earlier and adding a new location.

To do this:
* Click the `Sign In` link at the top right of the navigation bar.
* Enter the credentials you created for your user.
* On successful login, click the Admin link on the navigation bar. You will be taken to the Django Administration backend.
* Click on Locations.
* On the right of the screen, click the `Add Location` button.
* We will enter a new state here. Use the following information:
  * Longname: `Texas`
  * Shortname: `TX`
  * Govlevel: `state`
  * Legiscan Id: `0`
  * Hierarchy: `world.usa.texas`
  * Parent: Select `United States`
  * Click the SAVE button
* Verify the information shows up on the Search page.
  * At the top right of the Django Administration page, click `VIEW SITE`. This will take you back to the home page of Legit Info.
  * Click the `Search` button. 
  * Open the `Locations` dropdown and verify `Texas` shows in the list.
  
Now, we can stop the container from running and restart it to see if our changes persisted like we expect. Back in the terminal, press `Ctrl-C` to stop the container. After the container stops, you can re-enter the same run command (`docker run -p 8080:8080 -v legit-info-db:/opt/app-root/src legit-info`), navigate to `http:localhost:8080`. When you click on the `Search` button, you should still see `Texas` in the Locations list. 

# Next steps
Congratulations on finishing the lab.

In this hands-on lab, you: 
* Installed pipenv to manage python dependencies
* Cloned the Legit-Info repository from Github
* Ran Django migrations to set up the initial SQLite database
* Seeded the database with legislation from a subset of states 
* Ran the application in the development environment
* Containerized the application and ran it with Docker

Where do you go from here?

## Join the Call for Code for Racial Justice community
Talk to the Legit-Info project team by joining the [Call for Code for Racial Justice community](https://developer.ibm.com/callforcode/racial-justice/get-started?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10008917&utm_id=NA-SkillsNetwork-Gitlab-legitinfocfc2021rj-2021-05-01).

## Contribute to the open source project
Now that you know the different pieces that make up the Legit-Info application, you can contribute back to the open source project on Github. The [issues section](https://github.com/Call-for-Code-for-Racial-Justice/Legit-Info/issues) is a great place to start.

LINK TO PART 2

**Authors:**
* Tony Pearson
* Upkar Lidder
* Charlie Evans
* Gaurav Ramakrishna
* Charles Johnson

**Other contributors:**
* Jennifer Judge Clark
