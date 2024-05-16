# Tuto : deploy geochestra with docker, on production

Composition offered by the geochestra comminty

##  Step 1 : clone the official repo
```bash
Création d’un dossier georchestra : mkdir -p dev/idgeo/georchestra
Ensuite se mettre dans le répertoire de travail :  cd idgeo/georchestra/
Installation de GIT: sudo apt install git
Clonage de git : git clone https://github.com/georchestra/datadir.git
Se mettre dans le dossier de clonage : cd datadir
Voir les différentes branche de travail : git branch -l
Voir les éléments des branches : git branch -la
Voir la branche de travail : git status
```
## Step 2 : Change treafik configuration file
*ressources/treafik_custom.yaml:*
```yaml
global: 
  sendAnonymousUsage: false
  checkNewVersion: false

entryPoints:
  web:
    address: ":80"
    http:
      redirections:
        entryPoint:
          to: websecure
          scheme: https

  websecure:
    address: ":443"

providers:
  docker:
    watch: true
    exposedByDefault: false
    endpoint: unix:///var/run/docker.sock

api:
  dashboard: true

log:
  level: INFO

certificatesResolvers:
  letsEncrypt:
    acme:
      # comment this line when going to production
      caServer: https://acme-staging-v02.api.letsencrypt.org/directory
      email: jean.pommier@pi-geosolutions.fr
      storage: /acme/acme.json
      httpChallenge:
        entryPoint: web

ping: {}
```

## Step 3 : update accordingly traefik config in docker-compose.overrid.yml
In docker-compose.overide.yml,
- We can comment the ` traefik_me_certificateèdowlader` block ( optimal but not useful anymore)
- add a volume named`acme`
- For`geochestra-127-0-1-1.traefik.me`service:
  - comment the`depends_on` section
  -change the `volumre` section for:
  ```
  
  ```
