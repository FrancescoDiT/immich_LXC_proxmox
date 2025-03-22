# Creating a container LXC with docker inside
## **REQUIREMENTS**
* an Ubuntu template (you can find it by clicking the button "Templates" in *Datacenter* **>** *your_node_name* **>** *local(your_node_name)* **>** *CT Templates*) 

(I'm using *ubuntu-24.10-standard_24.10-1_amd64.tar.zst*)

---
### 1. **Enable cgroups and necessary functionalities**
Go to your_container configuration file
```bash
nano /etc/pve/lxc/<your_container_id>.conf
```

and add theese lines

```ini
lxc.cgroup2.devices.allow = a
lxc.cap.drop =
lxc.mount.auto = proc:rw sys:rw
```
Then save, exit, restart your_container and enter in it
```bash
pct restart <your_container_id>
pct enter <your_container_id>
```
---
### 2. **Install Docker into LXC Container**

Now you need to update and upgrade your_container, then install the required dependencies for Docker

```bash
apt update && sudo apt upgrade -y
apt install -y apt-transport-https ca-certificates curl software-properties-common
```

Add the Docker GPG authentication key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Add the official Docker repository for Ubuntu

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Then update again and install Docker

```bash
apt update
apt install -y docker-ce docker-ce-cli containerd.io
```

### 3. **Verify Docker installation and run it automatically**

```bash
sudo docker --version
```

run it first manually and then automatically

```bash
systemctl start docker
systemctl enable docker
```

if this command show an output, you've done!

```bash
docker run hello-world
```