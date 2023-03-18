# building-apps-for-k8s-l6-using-Helm

In this repo [Helm](https://helm.sh/) is used to package Kubernetes apps using a templating system as opposed to Kustomize which uses overlays. That explains why Helm is defined as a package manager for k8s apps.
The created file (Helm Chart) will be saved in the app code repo, it can also be stored in a chart repo separate from the app code.

## The Steps, using the CLI in the root of this directory
1. Create a chart with a directory structure:
	`helm create build4kube`

2. Clean the repo by removing uneeded files. The command above creates a lot of template files that do not apply to this specific use case which can be removed.

3. Add templates to the build4kube/templates directory. YAML templates ([deployment.yaml](https://github.com/Fabr1ce/building-apps-for-k8s-l5-using-Kustomize/blob/main/deployment.yaml) and [service.yaml](https://github.com/Fabr1ce/building-apps-for-k8s-l5-using-Kustomize/blob/main/service.yaml)) from [lab 5](https://github.com/Fabr1ce/building-apps-for-k8s-l5-using-Kustomize) can be used.
The templates need to be changed in order for Helm to use them:
- Add a **release name**: this is a **versioning** system in Helm. Add this as a **variable** and repeat for any other values you do not wish to hard code. Helm replaces these at deployment.
- Create a build4kube/values.yaml file with the removed values in previuous step.
- Create the release name by running the following cmd in the root directory:
	`helm install --generate-name ./build4kube/ --dry-run`
	This command will display a full manifest that will be deployed.
4. Manage the application
This section deals with managing the app deployment. A k8s cluster must already exist in order to deploy this app. Steps from [this lab](https://github.com/Fabr1ce/building-apps-for-k8s-l4-using-Kind) show how to create a k8s cluster using  kind. 
- **Deploy** app: 
	`helm install --generate-name ./build4kube`
  Very deployment with: 
  	`kubectl get all`
- To **updgrade** deployment too new version:
	- Make changes to the files as needed, change the version in the Chart.yaml
	- Upgrade:
	`helm upgrade build4kube-<release-version> ./build4kube`
- To **rollback** to previous release run: 
	`helm rollback build4kube-<previous-release>`
After rollback, the Helm chart will no longer match what's deployed. Another way to rollback would be to change the version in the Helm chart then generate the release again (this is recommended for version control and CICD).

`helm package <directory-for-chart>` creates a tar zip file with the version that match the chart in the provided directory. 

4. Don't forget to clean up!
	`helm delete <your-release> --purge`

To make sure all k8s pods are delete:
	`kubectl get pods`
        `kubectl delete all --all --all-namespaces`
	
That's it! This lab uses Helm to create a chart and templates that are use to package, deploy, and manage a k8s application.
For a more detailed step by step video, see the source ([KubeAcademy course](https://kube.academy/courses/building-applications-for-kubernetes/lessons/packaging-your-application)). 

