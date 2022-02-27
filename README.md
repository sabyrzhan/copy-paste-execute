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
