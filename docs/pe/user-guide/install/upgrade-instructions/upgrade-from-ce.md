---
layout: docwithnav-pe
title: ThingsBoard instructions for upgrading from Community Edition
description: Upgrading from Community Edition


---

<ul id="markdown-toc">
  <li>
    <a href="#ubuntu" id="markdown-toc-ubuntu-ce">Ubuntu</a>
  </li>
  <li>
    <a href="#centos" id="markdown-toc-centos">CentOS</a>
  </li>
  <li>
    <a href="#windows" id="markdown-toc-ubuntu-ce">Windows</a>
  </li>
  <li>
    <a href="#docker" id="markdown-toc-docker-ce">Docker</a>
  </li>
  <li>
    <a href="#docker-compose" id="markdown-toc-docker-compose-ce">Docker Compose</a>
  </li>
  <li>
    <a href="#minikube" id="markdown-toc-minikube-ce">Minikube</a>
  </li>
</ul>

## Ubuntu {#ubuntu}

{% capture difference %}
**NOTE:**
<br>
These upgrade steps are applicable for the latest ThingsBoard Community Edition version. In order to upgrade to Professional Edition you need to [**upgrade to the latest Community Edition version first**](/docs/user-guide/install/upgrade-instructions/).
{% endcapture %}
{% include templates/warn-banner.md content=difference %}

#### ThingsBoard PE package download

```bash
wget https://dist.thingsboard.io/thingsboard-{{ site.release.pe_ver }}.deb
```
{: .copy-code}

#### ThingsBoard PE service upgrade

* Stop ThingsBoard service if it is running.

```bash
sudo service thingsboard stop
```
{: .copy-code}

* Install Thingsboard Web Report component as described [here](/docs/user-guide/install/pe/ubuntu/#step-9-install-thingsboard-webreport-component).

```bash
sudo dpkg -i thingsboard-{{ site.release.pe_ver }}.deb
```
{: .copy-code}

{% capture difference %}
**NOTE:**
<br>
Package installer may ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.
{% endcapture %}
{% include templates/info-banner.md content=difference %}

* Configure Professional Edition license key as described [here](/docs/user-guide/install/pe/ubuntu/#step-3-obtain-and-configure-license-key).

Execute regular upgrade script:

```bash
sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=CE
```
{: .copy-code}

#### Start the service

```bash
sudo service thingsboard start
```
{: .copy-code}

## CentOS {#centos}

{% capture difference %}
**NOTE:**
<br>
These upgrade steps are applicable for the latest ThingsBoard Community Edition version. In order to upgrade to Professional Edition you need to [**upgrade to the latest Community Edition version first**](/docs/user-guide/install/upgrade-instructions/).
{% endcapture %}
{% include templates/warn-banner.md content=difference %}

#### ThingsBoard PE package download

```bash
wget https://dist.thingsboard.io/thingsboard-{{ site.release.pe_ver }}.rpm
```
{: .copy-code}

#### ThingsBoard PE service upgrade

* Stop ThingsBoard service if it is running.

```bash
sudo service thingsboard stop
```
{: .copy-code}

* Install Thingsboard Web Report component as described [here](/docs/user-guide/install/pe/ubuntu/#step-9-install-thingsboard-webreport-component).

```bash
sudo rpm -Uvh thingsboard-{{ site.release.pe_ver }}.rpm
```
{: .copy-code}

{% capture difference %}
**NOTE:**
<br>
Package installer may ask you to merge your thingsboard configuration. It is preferred to use **merge option** to make sure that all your previous parameters will not be overwritten.
{% endcapture %}
{% include templates/info-banner.md content=difference %}

* Configure Professional Edition license key as described [here](/docs/user-guide/install/pe/ubuntu/#step-3-obtain-and-configure-license-key).

Execute regular upgrade script:

```bash
sudo /usr/share/thingsboard/bin/install/upgrade.sh --fromVersion=CE
```
{: .copy-code}

#### Start the service

```bash
sudo service thingsboard start
```
{: .copy-code}

## Windows {#windows}

{% capture difference %}
**NOTE:**
<br>
These upgrade steps are applicable for the latest ThingsBoard Community Edition version. In order to upgrade to Professional Edition you need to [**upgrade to the latest Community Edition version first**](/docs/user-guide/install/upgrade-instructions/).
{% endcapture %}
{% include templates/warn-banner.md content=difference %}

#### ThingsBoard PE package download

Download ThingsBoard PE installation package for Windows: [thingsboard-windows-{{ site.release.pe_ver }}.zip](https://dist.thingsboard.io/thingsboard-windows-{{ site.release.pe_ver }}.zip).

#### ThingsBoard PE service upgrade

* Stop ThingsBoard service if it is running.

```text
net stop thingsboard
```
{: .copy-code}

* Make a backup of previous ThingsBoard CE configuration located in \<ThingsBoard install dir\>\conf (for ex. C:\thingsboard\conf).
* Copy content of the **thingsboard-windows-{{ site.release.pe_ver }}.zip** to the same location.
* Compare and merge your old ThingsBoard configuration files (from the backup you made in the first step) with new ones.
* Configure Professional Edition license key as described [here](/docs/user-guide/install/pe/windows/#step-3-obtain-and-configure-license-key).
* Finally, run **upgrade.bat** script to upgrade ThingsBoard to the new version.

{% capture difference %}
**NOTE:**
<br>
Scripts listed above should be executed using Administrator Role.
{% endcapture %}
{% include templates/info-banner.md content=difference %}

Execute regular upgrade script:

```text
C:\thingsboard>upgrade.bat --fromVersion=CE
```
{: .copy-code}

#### Start the service

```text
net start thingsboard
```
{: .copy-code}

## Docker {#docker}

{% capture difference %}
**NOTE:**
<br>
These upgrade steps are applicable for the latest ThingsBoard Community Edition version. In order to upgrade to Professional Edition you need to [**upgrade to the latest Community Edition version first**](/docs/user-guide/install/upgrade-instructions/).
{% endcapture %}
{% include templates/warn-banner.md content=difference %}

#### ThingsBoard PE image download

```bash
docker pull thingsboard/tb-pe-node:{{ site.release.pe_ver }}
docker pull thingsboard/tb-pe-web-report:{{ site.release.pe_ver }}
```
{: .copy-code}

#### ThingsBoard PE service upgrade

* Stop and remove the ThingsBoard CE service:

```bash
docker compose stop thingsboard-ce
docker compose rm -f thingsboard-ce
```
{: .copy-code}

* Update your `docker-compose.yml` according to the [default Docker PE manifest](/docs/user-guide/install/pe/docker/#step-2-choose-thingsboard-queue-service). Do not forget to change the image to the PE version, define the required license variables and volumes, and add the Web Report service.

* Run the upgrade and start the services:

```bash
docker compose run --rm -e UPGRADE_TB=true -e FROM_VERSION="CE" thingsboard-pe
docker compose up -d
```
{: .copy-code}

## Docker Compose {#docker-compose}

{% capture difference %}
**NOTE:**
<br>
These upgrade steps are applicable for the latest ThingsBoard Community Edition version. In order to upgrade to Professional Edition you need to [**upgrade to the latest Community Edition version first**](/docs/user-guide/install/upgrade-instructions/).
{% endcapture %}
{% include templates/warn-banner.md content=difference %}

#### ThingsBoard PE image download

```bash
docker pull thingsboard/tb-pe-node:{{ site.release.pe_ver }}
docker pull thingsboard/tb-pe-web-report:{{ site.release.pe_ver }}
```
{: .copy-code}

* Stop the CE services

```bash
./docker-stop-services.sh
```
{: .copy-code}

* Manually merge your current ThingsBoard CE cluster configuration with the [default Docker Compose cluster deployment files](https://github.com/thingsboard/thingsboard-pe-docker-compose). Ensure that you transfer all custom environment variables, volume mappings, and external service configurations to the new files.

* Configure the license key environment variables as described in the [PE Docker Compose Cluster Setup Guide](/docs/user-guide/install/pe/cluster/docker-compose-setup/#step-4-configure-your-license-key).

* Run the upgrade and start the services:

```bash
./docker-upgrade-tb.sh --fromVersion=CE
./docker-start-services.sh
```
{: .copy-code}

## Minikube {#minikube}

{% capture difference %}
**NOTE:**
<br>
These upgrade steps are applicable for the latest ThingsBoard Community Edition version. In order to upgrade to Professional Edition you need to [**upgrade to the latest Community Edition version first**](/docs/user-guide/install/upgrade-instructions/).
{% endcapture %}
{% include templates/warn-banner.md content=difference %}

#### Clone ThingsBoard PE Kubernetes scripts

```bash
git clone -b release-{{ site.release.pe_full_ver }} https://github.com/thingsboard/thingsboard-pe-k8s.git --depth 1
cd thingsboard-pe-k8s/minikube
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

{% capture minikube_upgrade_note %}
**NOTE:**
<br>
The upgrade pod requires all third-party services (Zookeeper, Kafka, Valkey, PostgreSQL, and Cassandra if using the `hybrid` database mode) to be running before executing the upgrade command.
If the pod times out, make sure all pods in the `thingsboard` namespace are in `Running` state first:

```bash
kubectl get pods -n thingsboard
```
Then re-run `./k8s-upgrade-tb.sh --fromVersion=CE`.
{% endcapture %}
{% include templates/info-banner.md content=minikube_upgrade_note %}

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

Get the Minikube IP and open it in your browser:

```bash
minikube ip
```
{: .copy-code}

Open `http://{your-minikube-ip}` and log in using your existing credentials.