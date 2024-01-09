# Filestash-GUI-client-for-storages

Installation Guide: https://www.filestash.app/docs/install-and-upgrade/?release=agpl

# 1. Setting up a test S3 compatible Object storage [MinIO]
lenovo@ipa:~$ mkdir -p ~/minio/data
lenovo@ipa:~$ 
lenovo@ipa:~$ podman run \
>    -p 9000:9000 \
>    -p 9001:9001 \
>    -v ~/minio/data:/data \
>    -e "MINIO_ROOT_USER=ROOTNAME" \
>    -e "MINIO_ROOT_PASSWORD=CHANGEME123" \
>    quay.io/minio/minio server /data --console-address ":9001"
MinIO Object Storage Server
Copyright: 2015-2024 MinIO, Inc.
License: GNU AGPLv3 <https://www.gnu.org/licenses/agpl-3.0.html>
Version: RELEASE.2024-01-05T22-17-24Z (go1.21.5 linux/amd64)

Status:         1 Online, 0 Offline. 
S3-API: http://10.0.2.100:9000  http://127.0.0.1:9000     
Console: http://10.0.2.100:9001 http://127.0.0.1:9001

# 2. Created test buckets inside MinIO and created an access key also.
     Access key: dv6bgSP87A06r6PC2L8f
     Secret Key: 6hcdX6Q2fvAgoRalYMx64D21qBOv4Eoan7Jl10TU

![Screenshot from 2024-01-09 15-08-36](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/77ef482e-9e17-4907-8490-fa2e16f064e7)

# 3. Now setup Filestash on local using Docker-compose as prescribed in its official doc.
     mkdir filestash && cd filestash

lenovo@ipa:~/filestash$ curl -O https://downloads.filestash.app/latest/docker-compose.yml
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   563    0   563    0     0    410      0 --:--:--  0:00:01 --:--:--   410

lenovo@ipa:~/filestash$ docker-compose up -d
Pulling app (machines/filestash:)...
latest: Pulling from machines/filestash
2385a6240410: Pull complete
d61079b1d589: Pull complete
4f4fb700ef54: Pull complete
2fae6d5ccdd1: Pull complete
041eaf83de58: Pull complete
Digest: sha256:223f11a4d346711139cf8c70ea9fc9393ae5e7b932b828d0ba69d9d0b1fa13c4
Status: Downloaded newer image for machines/filestash:latest
Creating filestash      ... done
Creating filestash_oods ... done

lenovo@ipa:~/filestash$ docker ps 
CONTAINER ID   IMAGE                       COMMAND                  CREATED         STATUS         PORTS                                       NAMES
5fa99f9b9d47   onlyoffice/documentserver   "/app/ds/run-documen…"   9 minutes ago   Up 9 minutes   80/tcp, 443/tcp                             filestash_oods
ccd99805c069   machines/filestash          "/app/filestash"         9 minutes ago   Up 9 minutes   0.0.0.0:8334->8334/tcp, :::8334->8334/tcp   filestash

# 4. Access the GUI & set S3 backend  and local for testing.
![Screenshot from 2024-01-09 14-11-42](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/3c8d5d51-06e4-4578-9f5b-9a32b5a7a90f)

![Screenshot from 2024-01-09 14-54-32](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/ae9b2c83-befc-4cab-b585-a7878b81231a)

# 5. Then Without the authentication setup; I just wanted to check whether mY GUI client Filestash is connecting to MinIO or not.

For S3 backend: 

![Screenshot from 2024-01-09 15-28-26](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/0833df42-612e-47d9-a312-f94f4f242454)

For Local Storage:

![Screenshot from 2024-01-09 14-57-08](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/bfd5fd13-b415-4738-8ca5-dd1cec822067)

![Screenshot from 2024-01-09 14-57-13](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/735add9c-a49e-49f1-a2b6-1efc1f800f57)
