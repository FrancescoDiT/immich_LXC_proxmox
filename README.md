# Creating a container LXC with docker inside
## **REQUIREMENTS**
* **OS**: an Ubuntu template (you can find it by clicking the button "Templates" in *Datacenter* **>** *your_node_name* **>** *local(your_node_name)* **>** *CT Templates*) 

(I'm using *ubuntu-24.10-standard_24.10-1_amd64.tar.zst*).

* **RAM**: Minimum 4GB, recommended 6GB.
* **CPU**: Minimum 2 cores, recommended 4 cores.
* **Storage**: Recommended Unix-compatible filesystem (EXT4, ZFS, APFS, etc.) with support for user/group ownership and permissions.
    * The generation of thumbnails and transcoded video can increase the size of the photo library by 10-20% on average.

* **Docker Engine**: to create a container LXC with docker inside follow the guide [here](https://github.com/FrancescoDiT/Docker_LXC_proxmox "Creating a container LXC with docker inside").

---

### 1. **Download the required files**

Create a directory of your choice (e.g. ./immich-app) to hold the docker-compose.yml and .env files, and then move to it.

```bash
mkdir ./immich-app
cd ./immich-app
```

Download docker-compose.yml

```bash
wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml

```

and .env configuration file.

```bash
wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
```

### 2. **Populate the .env with your values and start the containers**

In the same directory where you downloaded .env file, enter this command

```bash
nano .env
```

to modify values:

```ini
# You can find documentation for all the supported env variables at https://immich.app/docs/install/environment-variables

# The location where your uploaded files are stored
UPLOAD_LOCATION=./library #change it to your storage directory
# The location where your database files are stored
DB_DATA_LOCATION=./postgres

# To set a timezone, uncomment the next line and change Etc/UTC to a TZ identifier from this list: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List
# TZ=Etc/UTC

# The Immich version to use. You can pin this to a specific version like "v1.71.0"
IMMICH_VERSION=release

# Connection secret for postgres. You should change it to a random password
# Please use only the characters `A-Za-z0-9`, without special characters or spaces
DB_PASSWORD=postgres

# The values below this line do not need to be changed
###################################################################################
DB_USERNAME=postgres
DB_DATABASE_NAME=immich
```
you can also change other values such as *DB_USERNAME* or *DB_PASSWORD* for more security and personlization.

Then run your container

```bash
docker compose up -d
```

### 3. **Upgrading**

If you need to upgrade Immich you just need to run thoose 2 commands

```bash
docker compose pull && docker compose up -d
```

then clean up disk space from old version's obsolete container images

```bash
docker image prune
```

## **And that's it!**

All you need to do now is go through the guided setup by registering your admin account at

 http://\<machine-ip-address\>:2283.