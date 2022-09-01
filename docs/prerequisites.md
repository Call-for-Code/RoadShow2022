# Prerequisites
Here we provide guidance on the hardware and software requirements to participate in this technical workshop.<br />
Furthermore, since each module does not require completion of the previous, sofware requirements are separted out by module(s).

## Hardware
> These requirements are based on the recommendations for **Red Hat OpenShift Local** which is utilized in modules 2 and 3.  For more details, see the following [article](https://access.redhat.com/documentation/en-us/red_hat_openshift_local/2.5/html/getting_started_guide/installation_gsg#minimum-system-requirements_gsg).
-  4 physical CPU cores
-  9 GB of free memory
-  35 GB of storage space

## Software
### Module 1
#### Operating Systems
-  Linux
   -  RHEL 9.0 or 8.6
      -  Install **docker-ce** following [these](https://docs.docker.com/engine/install/rhel/) instructions.
      -  Install git
         ```
         dnf install git
         ```

    -  Ubuntu 22.04
       -  Install **docker-ce** following [these](https://docs.docker.com/engine/install/ubuntu/) instructions.
       -  Install packages **git** using the following command:
          ```
          apt install git
          ```
-  Windows (versions 10 or 11)
   -  Download and install [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/).
   -  Install **Git** following [these](https://github.com/git-guides/install-git#install-git-on-windows) instructions.

-  Mac
   -  Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/).
   -  Install **Git** via Homebrew using the following command:
      ```
      brew install git
      ```

### Modules 2 and 3
-  Create a Red Hat developer account
   >  Goto https://developers.redhat.com/about and click **Join now**
-  Download and install **OpenShift developer** on your laptop.
   1.  Goto https://developers.redhat.com/products/openshift/download
   2.  Click on _Install Red Hat OpenShift on your laptop_
   3.  Login with your _Red Hat developer_ account.
   4.  Under _section 1_, select your platform and download.
   5.  Follow instructions under _section 2_ to install and run OpenShift.