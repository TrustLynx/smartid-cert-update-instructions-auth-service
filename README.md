## Problem description
Starting November 27, 2024, all new Smart-ID accounts will be issued using the new intermediate certificates “SK ID Solutions EID-Q 2024E” (for qualified accounts) and “SK ID Solutions EID-NQ 2021E” (for non-qualified accounts).
This change, scheduled for November 27, 2024, affects all information systems and applications that provide digital authentication or signatures using the Smart-ID service provided by SK.
## TL authentication service adjustment instructions
### Authentication service that is already deployed via docker-compose
- inside docker-compose.yml file change authentication service image to trustlynx/dmss-authentication-service:24.0.5. Example:

```
    dmss-authentication-service:
        container_name: dmss-authentication-service
        restart: always
        ports:
            - '91:8089'
        volumes:
            - '/opt/trustlynx/dmss-authentication-service/:/confs'
        environment:
            - SPRING_CONFIG_LOCATION=/confs/
        image: 'trustlynx/dmss-authentication-service:24.0.5'
```

- take default **application.yml** file from current repository.
- fill authentication parties credentials (sections like smartid, mobileid, lvrtc e.t.c.) from your existing **application.yml** to default **application.yml** that you have taken from current repository. 
- update trustedCertificates value inside **application.yml**. Example:

```
PROD
smartId:
  trustedCertificates: classpath:trusted_certificates.jks

TEST
smartId:
  trustedCertificates: file:classpathtrusted_certificates_test.jks
```
## Switching from TEST to PROD (application.yml)
- Inside application.yml switch "digidoc4j - configuration - mode" value to PROD

```
digidoc4j:
  configuration:
    # Digidoc4j mode, PROD or TEST
    mode: PROD
```
- Navigate application.yml thrue authentication tools (smartid / mobileid / lvrtc e.t.c.) and change service URLs / credentials from DEMO / TEST to PROD. For example:

```
smartId:
  # Smart-ID Service url. PROD URL https://rp-api.smart-id.com/v2
  #hostUrl: https://sid.demo.sk.ee/smart-id-rp/v2/ - IT WAS DEMO ADDRESS BEFORE
  hostUrl:  https://rp-api.smart-id.com/v2  #NOW HOST IS SWITCHED TO PROD
```
