{% capture difference %}
**NOTE:**
<br>
These upgrade steps are applicable for the latest ThingsBoard Community Edition version. In order to upgrade to Professional Edition you need to [**upgrade to the latest Community Edition version first**](/docs/user-guide/install/upgrade-instructions/).
{% endcapture %}
{% include templates/warn-banner.md content=difference %}

#### Clone ThingsBoard PE Kubernetes scripts

Clone the PE K8s scripts repository and switch to the subdirectory that matches your cluster type:

| Cluster type | Subdirectory |
|---|---|
| Minikube | `minikube` |
| AWS EKS | `aws` |
| Azure AKS | `azure` |
| GCP GKE | `gcp` |

```bash
git clone -b release-{{ site.release.pe_full_ver }} https://github.com/thingsboard/thingsboard-pe-k8s.git --depth 1
cd thingsboard-pe-k8s/<subdirectory>
```
{: .copy-code}

#### Configure your license key

Open `tb-node.yml` and set the license secret:

```bash
nano tb-node.yml
```
{: .copy-code}

```yaml
- name: TB_LICENSE_SECRET
  value: "PUT_YOUR_LICENSE_SECRET_HERE"
```

See [How-to get pay-as-you-go subscription](https://www.youtube.com/watch?v=dK-QDFGxWek){:target="_blank"} or [How-to get perpetual license](https://www.youtube.com/watch?v=GPe0lHolWek){:target="_blank"} for more details.

#### Stop ThingsBoard CE resources

```bash
./k8s-delete-resources.sh
```
{: .copy-code}

#### Run the database upgrade

```bash
./k8s-upgrade-tb.sh --fromVersion=CE
```
{: .copy-code}

This command creates a temporary `tb-db-setup` pod that migrates the existing CE database schema to PE and loads PE system data.

{% capture k8s_upgrade_note %}
**NOTE:**
<br>
All third-party services (Zookeeper, Kafka, Valkey, PostgreSQL, and Cassandra if using the `hybrid` database mode) must be in `Running` state before executing the upgrade command.
Check the pod status with:

```bash
kubectl get pods -n thingsboard
```
If any pod is not `Running`, wait for it to be ready before proceeding.
{% endcapture %}
{% include templates/info-banner.md content=k8s_upgrade_note %}

#### Deploy ThingsBoard PE resources

```bash
./k8s-deploy-resources.sh
```
{: .copy-code}

#### Verify the upgrade

Wait until all pods are running:

```bash
kubectl get pods -n thingsboard
```
{: .copy-code}
