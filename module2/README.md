# Module 2 Instructions
In this module, we will use the deploy Legit Info paired with a PostgreSQL database to OpenShift installed on your laptop.

## Prerequisites
1. See [Modules 2 and 3](../docs/prerequisites.md) in the _prerequisites_ article.
2. Clone the **Road Show 2022** project:
   ```
   git clone https://github.com/Call-for-Code/RoadShow2022.git
   cd Roadshow2022/module2
   ```

## Start OpenShift local
1. Open a command prompt or terminal.
2. Start the instance by running the following command:
    ```
    crc start
    ```
3. Configure use of the `oc` command:
    - MacOS or Linux
      ```
      eval $(crc oc-env)
      ```
    - Windows
      ```
      crc oc-env | Invoke-Expression
      ```
4. Via the command line connect to the OpenShift instance:
    ```
    oc login -u kubeadmin https://api.crc.testing:6443
    ```

## Connect to the OpenShift WebGUI
Open the following URL in a browser:
> https://console-openshift-console.apps-crc.testing<br/>

then login as the user `kubeadmin` using the credentials provided from the `crc start` command.

## Create the **roadshow** namespace
- From command line:
  ```
  oc new-project roadshow
  ```
- From the WebGUI<br/>
  1. Navigate to `Home > Projects`
  2. Select the button **Create Project**
  3. In the name enter **roadshow**
  4. Select **Create**


## Convert the Docker compose file
We will use [kompose](https://kompose.io) to convert the Docker compose creating the following components to deploy on OpenShift:
- DeploymentConfig
- ImageStream
- Route
- Service
- Secret

Let's start by review of the file.
1. Change to the `Legit-Info` folder by using the following command:
    ```
    cd Legit-Info
    ```
2. Review the **Docker compose file** located in the following directory:
    ```
    configurations/docker/docker-compose.yaml
    ```
3. For each service **legitinfo** and **legitinfo-db** the _labels_ section is used to define functionality for the pods.<br />
    For example:
    ```
    kompose.service.expose
    ```
    indicates this service will require a route created.  Recommend review of the [kompose](https://kompose.io/user-guide/) documentation for more details.

4. Run the following command to generate the OpenShift deployment files:
    ```
    kompose --provider openshift convert --out configurations/openshift
    ```
    the resulting files will require the following adjustments:
    - The current use of `apiVersion` defined has been deprecated.  Some files will need to be updated to the supported version.<br />
      For example
      ```
      apiVersion: v1
      ```
      will need to be changed to:
      ```
      apiVersion: apps.openshift.io/v1
      ```
      And so forth.  To do so, run the following commands:
      ```
      sed -i "s|apiVersion: v1|apiVersion: apps.openshift.io/v1|g" *-deploymentconfig.yaml
      sed -i "s|apiVersion: v1|apiVersion: image.openshift.io/v1|g" *-imagestream.yaml
      sed -i "s|apiVersion: v1|apiVersion: route.openshift.io/v1|g" *-route.yaml
      ```
    - Docker only supports very basic use of secrets.  With that said, the secret should be deleted the created again.<br />
      - Delete the old secret.
        ```
        rm -f openshift/postgresql-secret.yaml
        ```
      - Create a new one.
        ```
        oc create secret generic postgresql \
          --from-literal=database-name=cfcappdb \
          --from-literal=database-user=pguser \
          --from-literal=database-password=pgpassword
        ```
      - In the `DeploymentConfig` files, we will update the environment variables to pull their values from the secret.  For example:
        - Change:
          ```
          - name: POSTGRESQL_DATABASE
            value: "cfcappdb"
        - To the following format:
          ```
          - name: POSTGRESQL_DATABASE
            valueFrom:
              secretKeyRef:
                name: postgresql
                key: database-name
          ```
        these changes will be made to the following environment variables:
        - POSTGRESQL_DATABASE
        - POSTGRESQL_PASSWORD
        - POSTGRESQL_USER

## Deploy on OpenShift
1. Make sure your scoped to the project **roadshow** by running the following command:
    ```
    oc project
    ```
    if not run the following command to switch:
    ```
    oc project roadshow
    ```

    If not already done, create the **roadshow** project using the following command:
    ```
    oc new-project roadshow
    ```
2. Run the following command from the folder:
    ```
    oc apply -n roadshow -f configurations/openshift/.
    ```

## Monitoring progress
Wait until both pods report a **Ready** status of `1/1`.
- Command line
  Use either command:
  > **Note**: Use Ctrl+X to break the command.
  - MacOS or Linux
    ```
    watch -n5 oc get pods
    ```
  - Windows
    ```
    while (1) {cls;oc get pods;sleep 5}
    ```
- Web GUI<br/>
  Navigate to `Workloads > Pods`



## Connect to the application
Navigate to the following URL in a browser:
> http://legitinfo-roadshow.apps-crc.testing

