# Fabr1ce-building-apps-for-k8s-l6

In this repo Helm is used to package k8s apps usinga templating system as opposed to Kustomize which uses overlays.
The created file (Helm Chart) will be saved in the app code repo.

1 - Create a chart with a directory structure:

	helm create build4kube

2 - Clean repo by removing uneeded files

3 - Add templates to the build4kube/templates directory. YAML templates from lab 5 (https://github.com/Fabr1ce/building-apps-for-k8s-l5) can be used.
The templates need to be changed in order for Helm to use them:
- Add a release name: this is a versioning system in Helm. Add this as a variable and repeat for any other values you do not wish to hard code.
- Create a values.yaml files with the removed values in previuous step.
- Create the release name by running the following cmd in the root directory:
	helm install --generate-name ./build4kube/ --dry-rum
- A cluster must already exist in order to deploy tthis app. Steps from this lab (https://github.com/Fabr1ce/building-apps-for-k8s-l4) show how to create a k8s cluster using  kind. 
- Deploy app: helm install --generate-name ./build4kube
- To rollback to previous release run: helm rollback build4kube-<previous-release>
After rollback, the Helm chart will no longer match what's deployed. Another way to rollback would be to change the version in the Helm chart them generate the release again.

Helm package <directory-for-chart> creates a tar zip file withe version that match the chart. 

Don't forget to clean up!
	helm delete <your-release> --purge

To make sure all k8s pods are delete:
	kubectl get pods
        kubectl delete all --all --all-namespaces

