# Automating Updates to Deployment after Source Code changes
In this module, we will utilize the Red Hat OpenShift pipelines operator to automaticly build new images and update the Legit Info deployment when changes are detected to the source code.

## Prerequisites
1.  See [Modules 2 and 3](../docs/prerequisites.md) in the _prerequisites_ article.
2.  Clone the **Legit Info** project:
    ```
    git clone https://github.com/Call-for-Code-for-Racial-Justice/Legit-Info.git
    cd Legit-Info
    ```
3.  Use these [instructions](../docs/connect_to_openshift_local.md) to connect to the OpenShift Local instance.

## Create the OpenShift deployment
1.  Run the following command to deploy the project on OpenShift.
    ```
    oc apply -n roadshow -f configurations/openshift/deployment/.
    ```
2.  Wait until both pods report a **Ready** status of `1/1`.  Use either option to monitor progress.
    - From command line
      > **Note**: Use Ctrl+X to break the command.
      - MacOS or Linux
        ```
        watch -n5 oc get pods
        ```
      - Windows
        ```
        while (1) {cls;oc get pods;sleep 5}
        ```
    - From Web GUI<br/>
      Navigate to _Workloads > Pods_

## Install the Red Hat OpenShift Pipelines operator
1.  Login to the OpenShift Web Console
2.  From the left navigation, select _Operators > OperatorHub_
3.  In the search field, enter `pipeline`
4.  Select the **Red Hat OpenShift Pipelines** operator.
5.  Select **Install**
6.  Accept the defaults and select **Install**.
7.  Once install is complete, a **Pipelines** option will appear in the left navigation.

## Deploy the pipeline configuration
From the terminal, run the following command:
```
oc apply -f configurations/openshift/pipeline/.
```

## Create and start a Pipeline Run
1.  Open the web console
2.  Navigate to _Pipelines > Pipelines_
3.  Select `build-and-deploy`
4.  In the top-right corner, select _Actions > Start_
5.  Complete the form _Start Pipeline_ as follows:
    -  deployment-name: `legitinfo`
    -  git-url: `https://github.com/Call-for-Code-for-Racial-Justice/Legit-Info.git`
    -  git-revision: `main`
    -  IMAGE: `image-registry.openshift-image-registry.svc:5000/roadshow/legitinfo:latest`
    -  shared-workspace: `PersistentVolumeClaim > source-pvc`
6.  Select **Start**

## Monitor Pipeline Run progress
1.  Under **Last Run** column select the top entry.
2.  Select the **Logs** tab.

From here you may monitor the progress of each task.

## What's next?