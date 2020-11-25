# Docker Environment for Developer Magento U Courses and Trainings

This guide will help you set up a local Magento environment for the developer courses and trainings, if you don't have one already.

The goal is to have a local environment with:
- A web server (Apache) with PHP version 7.2 or above
- A MySQL (MariaDB) database server
- Magento Open Source Code
- Magento Open Source Sample Data

## Introduction

We are providing custom docker images so once loaded, you will be ready to start with the developer courses and trainings.

At the end of this guide you will find the instructions for installing the environment with the custom docker images.
You don't need to follow those instructions, they are provided for your reference.

## Requirements

- MacOS, Linux or Windows with or without WSL (with subsystem for Linux)
- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [GIT](https://git-scm.com/downloads)
- Your Magento repository keys (see https://devdocs.magento.com/guides/v2.3/install-gde/prereq/connect-auth.html)
- (Optional) [Mutagen utility installed](https://mutagen.io/documentation/introduction/installation). Mutagen is a
utility that provides real-time file synchronization, at the moment available for MacOS and only for Windows 10 with WSL (with subsystem for Linux).
- (Optional) [Visual Studio Code](https://code.visualstudio.com/download) or [PhpStorm](https://www.jetbrains.com/phpstorm/download/) or others

## Step-by-step installation guide
**Start Docker Desktop.**

**[Clone this repo](https://github.com/mike61988/magento2-dk/) to your working directory.**

**Create a folder called _instances_ within that directory.**

_Example:_

```/magento/magento2-dk/instances```

**Copy `magento2-dev-dk/debian_config/composer/auth.json.sample` to `magento2-dev-dk/debian_config/composer/auth.json` and enter your public (username) and private (password) access keys.**

_Example:_

```
 {
     "http-basic": {
         "repo.magento.com": {
             "username": "<public_key>",
             "password": "<private_key>"
         }
     }
 }
```

**Add an entry to your local hosts file to resolve the local magento2u.loc domain:**

_Unix_
```
sudo vi /etc/hosts
127.0.0.1       magento2u.loc
```

_Windows_
```
Windows host file location is C:\Windows\System32\drivers\etc\hosts
Open file as administrator (or use visual studio code editor), add the following line:
127.0.0.1       magento2u.loc
then save the file
```

**Note:** If you are using Windows without WSL, you must update `magento2-dk/docker-compose_windows.yml` to match your installation directory like the following:
```
volumes:
- C:\PATH\TO\YOUR\magento\magento2-dk\instances:/var/www/html/shared
```

**Open a terminal in that folder and run the following command:**

_For Unix/Mac/Windows with WSL_
```
docker-compose up -d
```

_Or, for Windows without WSL_
```
docker-compose -f docker-compose_windows.yml up -d
```
This step will take a while.

**After the docker images are successfully built, run the following command to access the container:**

_Unix/Mac_
```
docker exec -ti m23 /bin/bash
```

_Windows_
```
winpty docker exec -it m23 //bin/sh
```

**Getting Access to the Magento instance**
Once inside the container, run the following command to move the Magento directory to a shared folder and exit:
```
mv /var/www/html/magento2ce /var/www/html/shared/ && exit
```

You can now access the site:
* Frontend: http://magento2u.loc:8080/
* Admin: http://magento2u.loc:8080/admin (username: admin, password: mageU123)

## Using Mutagen
If you are on Mac/Unix or Windows with WSL, you can use Mutagen. Click [here](https://mutagen.io/documentation/introduction/installation) for installation instructions.


Execute the following script in the root of your project for instructions on the most common commands.

You can also view the Mutagen [documentation](https://mutagen.io/documentation/introduction/getting-started).

_Unix/Mac_

```
sh mutagen_session.sh
```

_Windows with WSL_
```
bash mutagen_session.sh
```

The commands are also listed below.

### Create mutagen session
To create a mutagen session you need to use the `create` command with the path to your working directory on your local machine.

_Unix/Mac_
```
sh mutagen_session.sh create /Users/<YOUR_USER>/<PATH>/<TO>/<YOUR>/magento/magento2-dk/instances/
```

_Windows with WSL_
```
bash mutagen_session.sh create C:\PATH\TO\YOUR\magento\magento2-dev-dk\instances\
```

After mutagen is started and all files are synchronized, the Magento code will appear in `instances` folder of your working directory.

### To pause a mutagen session, use the `pause` command.

_Mac/Unix_
```
sh mutagen_session.sh pause
```
_Windows with WSL_
```
bash mutagen_session.sh pause
```

### To resume a mutagen session use the `resume` command.

_Mac/Unix_
```
sh mutagen_session.sh resume
```

_Windows with WSL_
```
bash mutagen_session.sh resume
```

### To terminate a mutagen session, use the `terminate` command.

_Mac/Unix_
```
sh mutagen_session.sh terminate
```

_Windows with WSL_
```
bash mutagen_session.sh terminate
```

### To list mutagen sessions, use the `list` command.

_Mac/Unix_
```
sh mutagen_session.sh list
```

_Windows with WSL_
```
bash mutagen_session.sh list
```

## Setup New Connection to the Database (via the client such as Sequel Pro, MySql Workbench, etc)

After the docker images are successfully built and running you can setup a new connection to the DB. Below we will describe steps for
MySql Workbench SQL client (You are able to use any SQL client you wish).

_MySql Workbench_
```
Open MySql Workbench client and click to "MySql Connections +" button and you will see the "Setup New Connetcion" window

- Fill "Connection Name" field with any name you want - Example: dockerDB

- "Connection Method" is "Standard (TCP/IP)"

- Hostname is 127.0.0.1

- Port is 4306

- username is magento

- password is magento

- Default Schema is m2

- Click Ok to save your new connection
```

## Other important information
After the docker images are successfully built and running you can run the following command to go inside of the container:

_Mac/Unix_
```
docker exec -ti <CONTAINER_ID> /bin/bash
```

_Windows_
```
winpty docker exec -it <CONTAINER_ID> //bin/sh
```

Example:

Unix:
```
docker exec -ti m23 /bin/bash
```

Windows:
```
winpty docker exec -it m23 //bin/sh
```

To stop the VM, use the following command:

```
docker-compose stop
```

To start the VM, use the following command (note that you will need to start it if you reboot your computer):
```
docker-compose start
```

To run the Magento CLI command in a running container:

_Mac/Unix_

```
sh m23_config/bin/console execute ANY_MAGENTO_CLI_COMMAND
```

_Windows_
```
bash m23_config/bin/console execute ANY_MAGENTO_CLI_COMMAND
```

That's it, you have your VM for learning purposes for Magento 2.2 and Magento 2.3.

## How to uninstall the VM

Open a terminal in your working directory and run the following command:
```
docker-compose down
```

Run the following command you should see something like table below:

```
docker image ls
```

```
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE

mike/m23_config       latest              167961367de5        12 days ago         3.39GB

mike/magento_config   latest              d26f7cd51da1        12 days ago         1.74GB

mike/debian_config    latest              bd3e96686fa0        12 days ago         923MB

debian                latest              67e34c1c9477        3 weeks ago         114MB
```

Now you need copy IMAGE ID and use following command:
```
docker rmi -f IMAGE ID
```
Example
```
docker rmi -f 167961367de5
```
for every single image from the table
