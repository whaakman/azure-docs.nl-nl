---
title: SSL inschakelen in Azure Container Instances
description: Een SSL of TLS-eindpunt voor een containergroep die worden uitgevoerd in Azure Container Instances maken
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/03/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 12de4ef31084d8ac8586c79ffe3d0a8e891727bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65411385"
---
# <a name="enable-an-ssl-endpoint-in-a-container-group"></a>Een SSL-eindpunt in een containergroep inschakelen

Dit artikel wordt beschreven hoe u maakt een [containergroep](container-instances-container-groups.md) met een App-container en een sidecar-container die wordt uitgevoerd een SSL-provider. Door het instellen van een containergroep met een afzonderlijk SSL-eindpunt, inschakelen u SSL-verbindingen voor uw toepassing zonder de toepassingscode van uw te wijzigen.

Instellen van een containergroep die bestaat uit twee containers:
* Een toepassingscontainer die wordt uitgevoerd een eenvoudige web-app met behulp van de openbare Microsoft [aci-helloworld](https://hub.docker.com/_/microsoft-azuredocs-aci-helloworld) installatiekopie. 
* Een sidecar-container met de openbare [Nginx](https://hub.docker.com/_/nginx) afbeelding is geconfigureerd om SSL te gebruiken. 

In dit voorbeeld wordt toont de containergroep alleen poort 443 voor Nginx met het openbare IP-adres. Nginx stuurt de HTTPS-aanvragen naar de bijbehorende web-app, die intern op poort 80 luistert. U kunt het voorbeeld voor container-apps die op andere poorten luisteren aanpassen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

U kunt de Azure Cloud Shell of een lokale installatie van de Azure CLI gebruiken om te voltooien in dit artikel. Als u wilt gebruiken op het bestand lokaal op, versie 2.0.55 of later wordt aanbevolen. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

Als u Nginx als SSL-provider instelt, moet u een SSL-certificaat. In dit artikel laat zien hoe om te maken en instellen van een zelf-ondertekend SSL-certificaat. Voor productiescenario's, moet u een certificaat verkrijgen van een certificeringsinstantie.

Gebruik voor het maken van een zelf-ondertekend SSL-certificaat de [OpenSSL](https://www.openssl.org/) hulpprogramma beschikbaar in Azure Cloud Shell en veel Linux-distributies, of een vergelijkbare clienthulpprogramma gebruiken in uw besturingssysteem.

Maak eerst een certificaataanvraag (.csr-bestand) in een lokale werkmap:

```console
openssl req -new -newkey rsa:2048 -nodes -keyout ssl.key -out ssl.csr
```

Volg de aanwijzingen voor het toevoegen van de identificatie-informatie. Voer de hostnaam die is gekoppeld aan het certificaat voor de algemene naam. Wanneer u hierom wordt gevraagd om een wachtwoord op te geven, drukt u op Enter op te geven om over te slaan met een wachtwoord toe te voegen.

Voer de volgende opdracht om te maken van de zelf-ondertekend certificaat (.crt-bestand) van de certificaataanvraag. Bijvoorbeeld:

```console
openssl x509 -req -days 365 -in ssl.csr -signkey ssl.key -out ssl.crt
```

U ziet nu de drie bestanden in de map: de certificaataanvraag (`ssl.csr`), de persoonlijke sleutel (`ssl.key`), en het zelfondertekende certificaat (`ssl.crt`). U gebruikt `ssl.key` en `ssl.crt` in latere stappen.

## <a name="configure-nginx-to-use-ssl"></a>Configureer Nginx om het gebruik van SSL

### <a name="create-nginx-configuration-file"></a>Nginx-configuratiebestand maken

In deze sectie maakt u een configuratiebestand voor Nginx om SSL te gebruiken. Begin met het kopiëren van de volgende tekst in een nieuw bestand met de naam`nginx.conf`. In Azure Cloud Shell, kunt u Visual Studio Code om te maken van het bestand in uw werkmap:

```console
code nginx.conf
```

In `location`, moet u instellen `proxy_pass` met de juiste poort voor de app. In dit voorbeeld stellen we poort 80 voor de `aci-helloworld` container.

```console
# nginx Configuration File
# https://wiki.nginx.org/Configuration

# Run as a less privileged user for security reasons.
user nginx;

worker_processes auto;

events {
  worker_connections 1024;
}

pid        /var/run/nginx.pid;

http {

    #Redirect to https, using 307 instead of 301 to preserve post data

    server {
        listen [::]:443 ssl;
        listen 443 ssl;

        server_name localhost;

        # Protect against the BEAST attack by not using SSLv3 at all. If you need to support older browsers (IE6) you may need to add
        # SSLv3 to the list of protocols below.
        ssl_protocols              TLSv1 TLSv1.1 TLSv1.2;

        # Ciphers set to best allow protection from Beast, while providing forwarding secrecy, as defined by Mozilla - https://wiki.mozilla.org/Security/Server_Side_TLS#Nginx
        ssl_ciphers                ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:ECDHE-RSA-RC4-SHA:ECDHE-ECDSA-RC4-SHA:AES128:AES256:RC4-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!3DES:!MD5:!PSK;
        ssl_prefer_server_ciphers  on;

        # Optimize SSL by caching session parameters for 10 minutes. This cuts down on the number of expensive SSL handshakes.
        # The handshake is the most CPU-intensive operation, and by default it is re-negotiated on every new/parallel connection.
        # By enabling a cache (of type "shared between all Nginx workers"), we tell the client to re-use the already negotiated state.
        # Further optimization can be achieved by raising keepalive_timeout, but that shouldn't be done unless you serve primarily HTTPS.
        ssl_session_cache    shared:SSL:10m; # a 1mb cache can hold about 4000 sessions, so we can hold 40000 sessions
        ssl_session_timeout  24h;


        # Use a higher keepalive timeout to reduce the need for repeated handshakes
        keepalive_timeout 300; # up from 75 secs default

        # remember the certificate for a year and automatically connect to HTTPS
        add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains';

        ssl_certificate      /etc/nginx/ssl.crt;
        ssl_certificate_key  /etc/nginx/ssl.key;

        location / {
            proxy_pass http://localhost:80; # TODO: replace port if app listens on port other than 80
            
            proxy_set_header Connection "";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $remote_addr;
        }
    }
}
```

### <a name="base64-encode-secrets-and-configuration-file"></a>Geheimen Base64-codering en -configuratiebestand

Base64-gecodeerd de Nginx-configuratiebestand, het SSL-certificaat en sleutel van de SSL. In de volgende sectie voert u de gecodeerde inhoud in een YAML-bestand dat is gebruikt voor het implementeren van de containergroep.

```console
cat nginx.conf | base64 -w 0 > base64-nginx.conf
cat ssl.crt | base64 -w 0 > base64-ssl.crt
cat ssl.key | base64 -w 0 > base64-ssl.key
```

## <a name="deploy-container-group"></a>Containergroep implementeren

Nu de containergroep implementeren door op te geven van de container-configuraties in een [YAML-bestand](container-instances-multi-container-yaml.md).

### <a name="create-yaml-file"></a>YAML-bestand maken

De volgende YAML kopiëren naar een nieuw bestand met de naam `deploy-aci.yaml`. In Azure Cloud Shell, kunt u Visual Studio Code om te maken van het bestand in uw werkmap:

```console
code deploy-aci.yaml
```

Voer de inhoud van de met base64 gecodeerde bestanden indien vermeld onder `secret`. Bijvoorbeeld, `cat` elk van de met base64 gecodeerde bestanden om te zien van de inhoud ervan. Tijdens de implementatie van deze bestanden worden toegevoegd aan een [geheime volume](container-instances-volume-secret.md) in de containergroep. In dit voorbeeld wordt is het geheime volume gekoppeld aan de Nginx-container.

```YAML
api-version: 2018-10-01
location: westus
name: app-with-ssl
properties:
  containers:
  - name: nginx-with-ssl
    properties:
      image: nginx
      ports:
      - port: 443
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - name: nginx-config
        mountPath: /etc/nginx
  - name: my-app
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  volumes:
  - secret:
      ssl.crt: <Enter contents of base64-ssl.crt here>
      ssl.key: <Enter contents of base64-ssl.key here>
      nginx.conf: <Enter contents of base64-nginx.conf here>
    name: nginx-config
  ipAddress:
    ports:
    - port: 443
      protocol: TCP
    type: Public
  osType: Linux
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

### <a name="deploy-the-container-group"></a>De containergroep implementeren

Maak een resourcegroep met de [az-groep maken](/cli/azure/group#az-group-create) opdracht:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implementeren van de containergroep met de [az container maken](/cli/azure/container#az-container-create) opdracht, de YAML-bestand als een argument doorgeven.

```azurecli
az container create --resource-group <myResourceGroup> --file deploy-aci.yaml
```

### <a name="view-deployment-state"></a>Status van de implementatie weergeven

Als u wilt de status van de implementatie weergeven, gebruikt u de volgende [az container show](/cli/azure/container#az-container-show) opdracht:

```azurecli
az container show --resource-group <myResourceGroup> --name app-with-ssl --output table
```

Voor een geslaagde implementatie is uitvoer vergelijkbaar met het volgende:

```console
Name          ResourceGroup    Status    Image                                                    IP:ports             Network    CPU/Memory       OsType    Location
------------  ---------------  --------  -------------------------------------------------------  -------------------  ---------  ---------------  --------  ----------
app-with-ssl  myresourcegroup  Running   mcr.microsoft.com/azuredocs/nginx, aci-helloworld        52.157.22.76:443     Public     1.0 core/1.5 gb  Linux     westus
```

## <a name="verify-ssl-connection"></a>SSL-verbinding controleren

Als u de toepassing die wordt uitgevoerd, gaat u naar het IP-adres in uw browser. Het IP-adres wordt weergegeven in dit voorbeeld is bijvoorbeeld `52.157.22.76`. U moet gebruiken `https://<IP-ADDRESS>` om te zien van de actieve toepassing vanwege de configuratie van de Nginx-server. Probeert verbinding te maken met `http://<IP-ADDRESS>` mislukken.

![Schermafbeelding van browser met toepassing die wordt uitgevoerd in een exemplaar van een Azure-container](./media/container-instances-container-group-ssl/aci-app-ssl-browser.png)

> [!NOTE]
> Omdat in dit voorbeeld een zelfondertekend certificaat en niet een van een certificeringsinstantie wordt, wordt er in de browser een beveiligingswaarschuwing weergegeven bij het verbinden met de site via HTTPS. Dit gedrag is verwacht.
>

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd u over het instellen van een Nginx-container om in te schakelen SSL-verbindingen met een web-app die wordt uitgevoerd in de containergroep. U kunt in dit voorbeeld voor apps die op poort dan poort 80 luisteren kunt aanpassen. U kunt ook de Nginx-configuratiebestand voor de server-verbindingen op poort 80 (HTTP) voor gebruik van HTTPS automatisch omleiding bijwerken.

Hoewel dit artikel wordt gebruikgemaakt van Nginx in de sidecar, kunt u een andere SSL-provider zoals [Caddy](https://caddyserver.com/).

Een andere benadering voor SSL inschakelen in een containergroep is het implementeren van de groep in een [virtueel Azure-netwerk](container-instances-vnet.md) met een [Azure-toepassingsgateway](../application-gateway/overview.md). De gateway kan worden ingesteld als een SSL-eindpunt. Bekijk een voorbeeld [implementatiesjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress-vnet) kunt u aanpassen om in te schakelen SSL-beëindiging op de gateway.
