# Connecting to the OpenShift Local instance
In this article we will cover the following:
- Starting the OpenShift local instance.
- Accessing OpenShift via the command line
- Accessing OpenShift via the WebUI.
- Creating the `roadshow` namespace (or project) used in modules 2 and 3.

## Start OpenShift Local
1. Open a command prompt or terminal.
2. Start the instance by running the following command:
    ```
    crc start
    ```

## Connect to OpenShift via the Terminal
1. Configure use of the `oc` command:
    - MacOS or Linux
      ```
      eval $(crc oc-env)
      ```
    - Windows
      ```
      crc oc-env | Invoke-Expression
      ```
2. Login to the OpenShift instance:
    ```
    oc login -u kubeadmin https://api.crc.testing:6443
    ```

## Connect to OpenShift via the WebGUI
1. Open the following URL in a browser:
   > https://console-openshift-console.apps-crc.testing<br/>

2.  Login as the user `kubeadmin` using the credentials provided from the `crc start` command.

## Create the **roadshow** namespace
- Comand line
  -  Run the following command:
     ```
     oc new-project roadshow
     ```
- WebGUI
  1. Navigate to `Home > Projects`
  2. Select the button **Create Project**
  3. In the name enter **roadshow**
  4. Select **Create**