# Reflection on Hello Minikube

## 1. Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running. What do you see in the logs? Does the number of logs increase each time you open the app?

The number of log entries do increase each time I open the app. This is because after exposing the deployment as a Service using kubectl expose, I was able to access the application through the URL printed by minikube service command. Each time I open the app through this URL, a new request will be sent to the application, which will be recorded in the log entries in the application logs


## 2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`. What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

The -n or --namespace option is used to specify the namespace in which the Kubernetes resources should be listed or managed. Kubernetes uses namespaces to organize and isolate resources within a cluster. When you run kubectl get without specifying a namespace, it lists resources from the current namespace, which is usually the default namespace. However, when you run kubectl get -n kube-system, it lists resources from the kube-system namespace, which is a reserved namespace for system components like the DNS service, ingress controller, and other core services required by the cluster. The output did not list the pods/services you explicitly created because those resources were created in the default namespace, whereas the command with -n kube-system was looking for resources in the kube-system namespace.

# Reflection on Rolling Update & Kubernetes Manifest File

## 1. What is the difference between Rolling Update and Recreate deployment strategy?
The difference between Rolling Update and Recreate deployment strategy lies in how they handle the update process:

- Rolling Update: With this strategy, Kubernetes gradually replaces the existing Pods with new ones running the updated version of the application. It creates new Pods with the updated configuration and waits for them to become ready before terminating the old Pods. This process happens incrementally, ensuring that the application remains available during the update process, albeit with reduced capacity for a brief period.

- Recreate: This strategy involves terminating all existing Pods and then creating new Pods with the updated configuration. It causes downtime during the update process as the old Pods are terminated before the new ones are created. The Recreate strategy is faster but can lead to service disruption if not handled properly.

## 2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.
I've changed it from the deploymentOriginal.yaml to deployment.yaml. Included in this repository.

## 3. Prepare different manifest files for executing Recreate deployment strategy.
After applying it, it has different rollout status from rolling update to recreate.

## 4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience in deploying the app manually and compare it to your experience when deploying the same app by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.

Using Kubernetes manifest files offers several benefits:

Declarative Configuration: Manifest files allow you to define the desired state of your resources in a declarative way, making it easier to manage and track changes.

Reproducibility: By storing the manifest files in a version control system, you can easily reproduce the same deployment across different environments or clusters.

Automation: Manifest files can be easily integrated into automated deployment pipelines, enabling continuous integration and continuous deployment (CI/CD) workflows.

Consistency: Using manifest files ensures that the same configuration is applied consistently across all resources, reducing the risk of human errors.

Compared to manually deploying the app using individual kubectl commands, using manifest files provides a more streamlined and efficient approach. With manifest files, you can define the entire application stack (Deployments, Services, ConfigMaps, etc.) in a single file or a set of files, making it easier to manage and maintain the application's configuration. Additionally, applying manifest files is generally faster and less error-prone than manually executing multiple kubectl commands.
