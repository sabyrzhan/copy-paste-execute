# 📝 👨‍💻 ▶️ copy-paste-execute in shell

## Install Docker on Ubuntu (run as `root`)
```bash
apt-get update && \
apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release && \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && \
apt-get update && \
apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose && \
docker run hello-world && \
usermod -aG docker postgres && \
usermod -aG docker vagrant
```
## Run `postgres_exporter` on Postgres host with Docker
```bash
docker run \
  --net=host \
  -e DATA_SOURCE_NAME="postgresql://postgres:postgres@localhost:5432/postgres?sslmode=disable" \
  quay.io/prometheuscommunity/postgres-exporter
```

## Install and enable any IP connections to `postgresql` on Ubuntu (PostgreSQL 12 as example)
1. `sudo apt-get update && sudo apt-get install -y postgresql-12`
2. Listen all connection:
    1. `sudo su`
    2. `nano /etc/postgresql/12/main/postgresql.conf`
    3. Set: `listen_addresses = '*'`
3. Enable connections from any host:
    1. `sudo su`
    2. `nano /etc/postgresql/12/main/pg_hba.conf`
    3. Add one of the following:
        1. `host    all             all             0.0.0.0/0               md5` - to auth by password
        2. `host    all             all             0.0.0.0/0               ident` - to auth by os user password
4. To update `postgres` user password:
    1. `sudo su`
    2. `su postgres`
    3. `psql`
    4. `ALTER USER postgres SET password 'YOUR PASSWORD'`
    5. `commit`
6. `sudo systemctl restart postgresql`

## PostgreSQL
### Quickly generate data
For example, to generate 1 000 000 records in table `temp(value int)`:
```
insert into temp(value) select random() * 100 from generate_series(0, 1000000);
```

## Install Windows11 on Intel based Macs
Source is here: https://www.youtube.com/watch?v=ISlalQsrWfk&t=358s

1. Download Windows 11 and Windows 10 ISO files from Microsoft website
2. Instal `homebrew` and install `wimlib` with homebrew
3. Create Win11 folder
4. Mount Windows 10 ISO and copy all files to Win11 folder
5. Goto `sources` folder inside Win11 folder and delete file `install.wim`
6. Mount Windows 11 ISO
7. Copy `install.wim` from sources folder to somewhere. For example, Desktop
8. In terminal goto Desktop and execute: `wimlib-imagex split install.wim install.swm 3500`
9. Delete original `install.wim` from Desktop. Move `install.swm` and `install2.swm` files to Win11/sources folder
10. Create disk image from Win11 folder
    1. Open DiskUtility
    2. Goto `File -> New image -> Image from Folder...`
    3. Select Win11 folder
    4. Specify `Image format` as `DVD/CD master` and destination folder (for example Desktop) with filename Win11.
    5. This will create `Win11.cdr` image file which we will convert to `.iso` file
    6. Open Terminal and goto Desktop. Execute: `hdiutil makehybrid -iso -joliet -o Win11.iso Win11.cdr`
    7. Now you can delete original Win11.cdr and Win11 folder
8. Install Windows using Bootcamp Assistant by specifiying Win11.iso disk image.
