# Prerequisites
Follow these instructions to participate in the desired module of the presentation.

## Module 1
### Operating Systems
#### Linux
- RHEL 8.6
  - Install **docker-ce** following [these](https://docs.docker.com/engine/install/rhel/) instructions.
  - Install git and postgresql client
    ```
    dnf install git postgresql
    ```

- Ubuntu 22.04
  - Install **docker-ce** following [these](https://docs.docker.com/engine/install/ubuntu/) instructions.
  - Install packages **git, python3 and postgresql client** using the following command:
    ```
    apt install git python3 postgresql-client
    ```
  - Install **pipenv**
    ```
    pip3 install --user pipenv
    ```
### Windows 10
  - Download and install [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/).
  - Install **Git** following [these](https://github.com/git-guides/install-git#install-git-on-windows) instructions.
  - Install **Python 3** following [these](https://www.python.org/downloads/windows/) instructions.
    > Ensure the python 3 version is 3.6.1 and higher by running the command `python3 --version`
  - Setup **SQLite**
    1. Download files `sqlite-dll-win32-x86*.zip` and `sqlite-tools-win32-x86*.zip` from https://www.sqlite.org/download.html
    2. Unpack and add dictory to _PATH_ environment variable.

### Mac
  - Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/).
  - Install **Python** following [these](https://github.com/git-guides/install-git#install-git-on-mac) instructions.
    > Ensure that python3 is installed. You need at least 3.6.x or later.
  - Install **Git** via Homebrew using the following command:
    ```
    brew install git
    ```
  - Setup **SQLite 3**
    1. Download binary from [here](https://www.sqlite.org/download.html).
    2. Unpack and add directory to _PATH_ environment variable.
  - Install *PostgreSQL** using [these](https://postgresapp.com/) instructions.


## Modules 2 and 3
- Create a Red Hat developer account
  > Goto https://developers.redhat.com/about and click **Join now**
- Download and install **OpenShift developer** on your laptop.
  > Before proceeding, recommend review of the [minimum](https://access.redhat.com/documentation/en-us/red_hat_openshift_local/2.5/html/getting_started_guide/installation_gsg#minimum-system-requirements_gsg) requirements.
  1. Goto https://developers.redhat.com/products/openshift/download
  2. Click on _Install Red Hat OpenShift on your laptop_
  3. Login with your _Red Hat developer_ account.
  4. Under _section 1_, select your platform and download.
  5. Follow instructions under _section 2_ to install and run OpenShift.