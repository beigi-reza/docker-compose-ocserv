![Image Size](https://img.shields.io/docker/image-size/rezabeigi/ocserv)
![Downloads](https://img.shields.io/docker/pulls/rezabeigi/ocserv)
![Build](https://img.shields.io/docker/automated/rezabeigi/ocserv)

# OpenConnect VPN server (ocserv) on docker

OpenConnect VPN server (ocserv) is an Open Source SSL VPN server. for secure and scalable VPN infrastructure.


## Installtion

**It is launched with the following settings**

- 2 Device connections for each user (`max-same-clients=2`)
- Up to 32 clients (`max-clients=32`)
- `10.10.10.0/24` as the internal IP pool
- Listens on port `443`
- Tunnels DNS to the server



## Edit docker-compse.yml & Run
1- Download `docker-compose.yml`
   ```
   wget https://raw.githubusercontent.com/Pezhvak/docker-ocserv/develop/docker-compose.yml
   ```
2- Replace the `<IPorDNS>` variable in docker-compse.yml with appropriate value.

3- Uncommet & Edit `volume` related to certificate if use valid **SSL** 

```yml
    volumes:
      - /etc/letsencrypt/live/<EXAMPLE.com>/fullchain.pem:/etc/ocserv/server-cert.pem
      - /etc/letsencrypt/live/<EXAMPLE.com>/privkey.pem:/etc/ocserv/server-key.pem
```

3- Run `docker-compose up -d`.


# Usage

## User Managment
### Create User

```bash
docker exec -it ocserv ash -c "ocuser create <username>"
```

### Delete a User

```bash
docker exec ocserv ash -c "ocuser delete <username>"
```
### Lock a User

```bash
docker exec ocserv ash -c "ocuser lock <username>"
```
### Unlock a User

```bash
docker exec ocserv ash -c "ocuser unlock <username>"
```

### list of User 
view `ocpasswd` file

```
docker exec ocserv cat /etc/ocserv/data/ocpasswd
```


## Using Client

- [Android (Cisco AnyConnect)](https://play.google.com/store/apps/details?id=com.cisco.anyconnect.vpn.android.avf)
- [Android (OpenConnect)](https://play.google.com/store/apps/details?id=com.github.digitalsoftwaresolutions.openconnect)
- [iOS](https://apps.apple.com/us/app/cisco-anyconnect/id1135064690)
- [MacOS](https://www.cisco.com/c/en/us/support/docs/smb/routers/cisco-rv-series-small-business-routers/smb5642-install-cisco-anyconnect-secure-mobility-client-on-a-mac-com-rev1.html)
- [Windows](https://www.cisco.com/c/en/us/support/docs/smb/routers/cisco-rv-series-small-business-routers/smb5686-install-cisco-anyconnect-secure-mobility-client-on-a-windows.html)
- [Ubuntu](https://www.cisco.com/c/en/us/support/docs/smb/routers/cisco-rv-series-small-business-routers/Kmgmt-785-AnyConnect-Linux-Ubuntu.html)



## `To use or not to use` VALID SSL Certficate

you will need an SSL certificate, It's up to you how you would like to generate it or Use It


### Valid SSL

You need to have a domain pointing to your server IP address and ports 80 and 443 available to be listened to by the container for letsencrypt ACME challenge verification.

for mor info view this link ...

[Simplest HTTPS setup](https://leangaurav.medium.com/simplest-https-setup-nginx-reverse-proxy-letsencrypt-ssl-certificate-aws-cloud-docker-4b74569b3c61)


### Not Valid SSL

If you can't create one ( ports 80 and 443 are not available on your server, or you don't have a domain), a fallback script will generate a self-signed certificate for you inside the container. The only difference is a ***warning message about the certificate not being trusted*** when logging in.


# References

- [cisco-anyconnect-server-docker](https://github.com/soreana/cisco-anyconnect-server-docker)
- [TommyLau/docker-ocserv](https://github.com/TommyLau/docker-ocserv)
