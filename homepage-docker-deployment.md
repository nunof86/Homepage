# Homepage Docker Deployment

## Creation of Homepage Directory

1. First we need to create the directory for <mark style="color:red;">`homepage`</mark>and jump to the new directory.

```bash
mkdir /opt/homepage
cd /opt/homepage
```

2. Then we need to create a <mark style="color:red;">`.env`</mark>  file with the command <mark style="color:red;">`nano .env`</mark> with the following information:

```bash
PUID=1000
PGID=1000
```

3. This is the information of the <mark style="color:red;">`UID`</mark> and <mark style="color:red;">`GID`</mark> of your user, you can check this information by tipping <mark style="color:red;">`id username`</mark>.

## Configuration of the docker-compose file

1. We need to create a <mark style="color:red;">`docker-compose.yml`</mark> file with the following information:

```yaml
version: "3.3"
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    ports:
      - 8000:3000
    env_file: .env
    volumes:
      - ./config:/app/config
    environment:
      PUID: $PUID
      PGID: $PGID
```

## Docker Compose

1. Start the Homepage deployment using docker-compose:

```bash
docker-compose up -d
```

2. After that navigate to the dashboard with <mark style="color:red;">`http://your_ip_address:8000`</mark>.
