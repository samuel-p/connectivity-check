# [connectivity-check](https://git.sp-codes.de/samuel-p/connectivity-check)

[![Build Status](https://ci.sp-codes.de/api/badges/samuel-p/connectivity-check/status.svg)](https://ci.sp-codes.de/samuel-p/connectivity-check) [![License](https://img.shields.io/badge/license-MIT-orange)](#license) [![Docker Pulls](https://img.shields.io/docker/pulls/samuelph/connectivity-check)](https://hub.docker.com/r/samuelph/connectivity-check)

A self-hosted captive portal connectivity check.

## Usage

Just run the following command:

```bash
docker run -d -p 80:80 -p 443:443 samuelph/connectivity-check
```

Or with docker-compose:

```yaml
version: '3.4'

services:
  connectivitycheck:
    image: samuelph/connectivity-check
    restart: always
    ports:
      - 80:80
      - 443:443
```

## Run on a simple web space

Instead of running the docker image you also can just use the following `.htaccess` file on your web server:

```
RewriteEngine On

Header always set "X-NetworkManager-Status" "online"

RewriteCond "%{REQUEST_FILENAME}" "(generate)?_?204"
RewriteRule "(generate)?_?204" / [R=204,L]

RewriteCond %{HTTPS} off
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
```

## Setup your devices

### Android

To change the Captive Portal Check in Android, you need a terminal app or a connection via ADB to a computer.

To use `http` you can run the following commands with the respective method:

```
# settings put global captive_portal_use_https 0
# settings put global captive_portal_http_url "http://connectivitycheck.sp-codes.de/generate204"
```

To use `https` you can use the following two commands:

```
# settings put global captive_portal_use_https 1
# settings put global captive_portal_https_url "https://connectivitycheck.sp-codes.de/generate204"
```

Maybe you have to reboot your phone after updating the settings.

If you are using AFWall+ you need to give access to _[1000] Android-System_ and in some cases _[10040] CaptivePortalLogin_ to make it work.

For more information see [here](https://android.stackexchange.com/a/186995/288049).

### Ubuntu

In Ubuntu, the file `/etc/NetworkManager/NetworkManager.conf` must be changed. Add or change the following lines:

```
[connectivity]
uri=https://connectivitycheck.sp-codes.de/generate204
```

Restart the network-manager:

```
sudo service network-manager restart
```

For more information see [here](https://askubuntu.com/q/1167177/920103).

### Firefox

Type [about:config](about:config) in the Firefox address bar and search for `captivedetect.canonicalURL` and `network.connectivity-service`. Set the URL values to `https://connectivitycheck.sp-codes.de/generate204`, the domain values to `connectivitycheck.sp-codes.de`. That's it.


## License

connectivity-check is Free Software: It is licensed under MIT (See [LICENSE](LICENSE) for more information).
