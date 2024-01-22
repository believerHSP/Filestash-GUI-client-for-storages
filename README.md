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


# We can a filestash using Docker CLI as well:
     You can also use the official filestash image from the docker hub and run the container using the docker CLI instead of            docker compose.
     >> docker pull machines/filestash
     >>  docker run -itd  -p 8334:8334 -v $(pwd)/filestash:/app/data/state/ --name filestash docker.io/machines/filestash

     After running the container refer the following official doc for setting up the filestash with minio s3 bucket
     https://www.filestash.app/docs/install-and-upgrade/

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

For Local Storage: [Workiing smoothly]

![Screenshot from 2024-01-09 16-24-01](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/8eec7897-fbbc-4bb7-ab30-b84f03ed155e)

![Screenshot from 2024-01-09 16-26-50](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/bb3013eb-215d-46cf-a227-3a31345ea5ba)



# Resolution: 
The above error get resolved using endpoint as http://192.168.122.1:9000 in place of 127.0.0.1:9000

Understanding still needs to be developed about the modification and resolution.

![Screenshot from 2024-01-11 12-09-01](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/0ed87215-6919-47f9-83f4-de2aa33bfd09)

![Screenshot from 2024-01-11 12-09-08](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/19a5d769-3de9-49ff-be47-bb39ef136b8d)


#                                                Part 2: LDAP Authentication in free Version

Authentication-Requirement: I need to configure an industry standard authentication for Filestash, In such a way that a user has access to only a specific bucket or sub-bucket.  
In Filestash LDAP, SAML & OPENID are available, but in enterprise release.

What can be possible solutions that we could Integrate with Filestash for above Authentication-requirement?

![Screenshot from 2024-01-11 15-07-14](https://github.com/believerHSP/Filestash-GUI-client-for-storages/assets/101576376/250ebbdc-9eeb-41e6-9fee-31d4352f326f)


**Apache** Can be used; so exploring that as of now.  **HTPASSWD**
[16:06, 11/01/2024] Devakrit Bagchi Sir: apache se BASIC authentication laga sakte ho. use ldap se authenticate karwalo.
[16:07, 11/01/2024] Devakrit Bagchi Sir: uske baad you've to configure different instances of Filestash for each user. After login the right filestash instance will be pointed to as per the Apache config.

It worked: https://www.cyberciti.biz/faq/create-update-user-authentication-files/


=============================================================================================================================



