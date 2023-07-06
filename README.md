# Ansible-Robot-Monitoring 
This project is an extension of [this](https://github.com/sentairanger/Dual-Robot-Monitoring) project. This time we are using Ansible to automate the deployment of our application. The image below shows how Ansible works with our application.

![ansible](https://github.com/sentairanger/Ansible-Robot-Monitoring/blob/main/images/Ansible.drawio(1).png)

Ansible allows the application cluster to be deployed to multiple remote hosts and allows those hosts to access the application when needed and they can also access the dashboard to monitor the robots' progress. This allows multiple developers to make any changes based on issues they may run into. And since this uses GitHub Actions and ArgoCD, this allows the user to push any changes (CI) for final deployment (CD). 

Where is Ansible used here? It is used to build and run our Docker image, tag and push our image to DockerHub, deploy our cluster with Kubernetes or ArgoCD and to install Prometheus, Grafana, Kind, Kubectl and Helm. The `install` directory includes all the needed playbooks to install our required applications. Make sure to have Ansible installed on your own PC by following the specific instructions for your OS. For Windows it's best to use WSL. After installing Ansible you can run a playbook with `ansible-playbook <playbook-name>`. Also make sure to install Docker based on your specific OS before running some of these playbooks. Also, you can change the hosts in the playbooks to your specific hosts if you want to install to multiple hosts. Make sure to update your `/etc/ansible/hosts` file and make sure to copy the SSH key to your other hosts and specify the remote hosts that will use Ansible. The official documentation shows how to configure your hosts and how to copy SSH keys to your remote hosts.

To get this application running first run this playbook on one terminal:
`ansible-playbook deploy-cluster.yml` or `ansible-playbook deploy-argocd-cluster.yml`

The image below shows the cluster running in ArgoCD.
![argocd](https://github.com/sentairanger/Ansible-Robot-Monitoring/blob/main/images/argocd.png)

If using ArgoCD first run the playbook in the `install` directory that installs ArgoCD and then deploy the nodeport with this command:
`ansible-playbook argocd-nodeport-deploy.yml`.

Then run the application with `ansible-playbook port-forward.yml`. The image below shows the main application. 

![ansiblerobot](https://github.com/sentairanger/Ansible-Robot-Monitoring/blob/main/images/ansible.png)

After that we can move the robot around to test it. Then we can run `ansible-playbook prometheus.yml` and go to `localhost:9090` to check that our application is being scraped by Prometetheus. Press CTRL+C to exit the playbook. Then we can run Grafana with `ansible-playbook grafana.yml`. Then we go to `localhost:3000` and then we log in as admin and default password prom-operator. Please change the password for security sake. After that we can import the JSON that is provided in the `grafana` directory and the dashboard should appear as shown below.

![dashboard](https://github.com/sentairanger/Ansible-Robot-Monitoring/blob/main/images/dashboard.png)

Again, we can run this on multiple hosts to monitor our robots remotely across the network.
