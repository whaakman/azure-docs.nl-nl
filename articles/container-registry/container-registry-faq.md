---
title: Azure Container Registry - Veelgestelde vragen
description: Antwoorden op veelgestelde vragen met betrekking tot de Azure Container Registry-service
services: container-registry
author: sajayantony
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 07/02/2019
ms.author: sajaya
ms.openlocfilehash: c32d7342aaf1c4cce52ce14abe48ea1bc347fdb3
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551594"
---
# <a name="frequently-asked-questions-about-azure-container-registry"></a>Veelgestelde vragen over Azure Container Registry

In dit artikel komen de antwoorden op veelgestelde vragen en bekende problemen met Azure Container Registry.

## <a name="resource-management"></a>Resourcebeheer

- [Kan ik een Azure container registry met behulp van Resource Manager-sjabloon maken?](#can-i-create-an-azure-container-registry-using-a-resource-manager-template)
- [Is er beveiligingsprobleem scannen voor afbeeldingen in ACR?](#is-there-security-vulnerability-scanning-for-images-in-acr)
- [Hoe configureer ik Kubernetes met Azure Container Registry?](#how-do-i-configure-kubernetes-with-azure-container-registry)
- [Hoe krijg ik beheerdersreferenties voor een container registry?](#how-do-i-get-admin-credentials-for-a-container-registry)
- [Hoe krijg ik beheerdersreferenties in Resource Manager-sjabloon](#how-do-i-get-admin-credentials-in-a-resource-manager-template)
- [Verwijderen van de replicatie mislukt met de status van verboden, hoewel de replicatie wordt verwijderd met behulp van de Azure CLI of Azure PowerShell](#delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell)
- [Firewall-regels zijn bijgewerkt, maar ze worden niet doorgevoerd](#firewall-rules-are-updated-successfully-but-they-do-not-take-effect)

### <a name="can-i-create-an-azure-container-registry-using-a-resource-manager-template"></a>Kan ik een Azure Container Registry met behulp van Resource Manager-sjabloon maken?

Ja. Hier volgt [een sjabloon](https://github.com/Azure/azure-cli/blob/master/src/command_modules/azure-cli-acr/azure/cli/command_modules/acr/template.json) waarmee u kunt een register maken.

### <a name="is-there-security-vulnerability-scanning-for-images-in-acr"></a>Is er beveiligingsprobleem scannen voor afbeeldingen in ACR?

Ja. Raadpleeg de documentatie van [Twistlock](https://www.twistlock.com/2016/11/07/twistlock-supports-azure-container-registry/) en [Aqua](https://blog.aquasec.com/image-vulnerability-scanning-in-azure-container-registry).

### <a name="how-do-i-configure-kubernetes-with-azure-container-registry"></a>Hoe configureer ik Kubernetes met Azure Container Registry?

Zie de documentatie voor [Kubernetes](https://kubernetes.io/docs/user-guide/images/#using-azure-container-registry-acr) en stappen voor het [Azure Kubernetes Service](container-registry-auth-aks.md).

### <a name="how-do-i-get-admin-credentials-for-a-container-registry"></a>Hoe krijg ik beheerdersreferenties voor een container registry?

> [!IMPORTANT]
> Het beheeraccount is ontworpen voor één gebruiker toegang tot het register, hoofdzakelijk voor testdoeleinden. We raden niet de referenties van het Administrator-account met meerdere gebruikers delen. Individuele identiteit wordt aanbevolen voor gebruikers en service-principals voor ' headless '-scenario's. Zie [verificatieoverzicht](container-registry-authentication.md).

Voordat het ophalen van beheerdersreferenties, moet dat gebruiker met beheerdersrechten van het register is ingeschakeld.

Ophalen van referenties met behulp van de Azure CLI:

```azurecli
az acr credential show -n myRegistry
```

Azure Powershell gebruiken:

```powershell
Invoke-AzureRmResourceAction -Action listCredentials -ResourceType Microsoft.ContainerRegistry/registries -ResourceGroupName myResourceGroup -ResourceName myRegistry
```

### <a name="how-do-i-get-admin-credentials-in-a-resource-manager-template"></a>Hoe krijg ik beheerdersreferenties in Resource Manager-sjabloon

> [!IMPORTANT]
> Het beheeraccount is ontworpen voor één gebruiker toegang tot het register, hoofdzakelijk voor testdoeleinden. We raden niet de referenties van het Administrator-account met meerdere gebruikers delen. Individuele identiteit wordt aanbevolen voor gebruikers en service-principals voor ' headless '-scenario's. Zie [verificatieoverzicht](container-registry-authentication.md).

Voordat het ophalen van beheerdersreferenties, moet dat gebruiker met beheerdersrechten van het register is ingeschakeld.

Het eerste wachtwoord ophalen:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[0].value]"
}
```

Het tweede wachtwoord ophalen:

```json
{
    "password": "[listCredentials(resourceId('Microsoft.ContainerRegistry/registries', 'myRegistry'), '2017-10-01').passwords[1].value]"
}
```

### <a name="delete-of-replication-fails-with-forbidden-status-although-the-replication-gets-deleted-using-the-azure-cli-or-azure-powershell"></a>Verwijderen van de replicatie mislukt met de status van verboden, hoewel de replicatie wordt verwijderd met behulp van de Azure CLI of Azure PowerShell

De fout wordt weergegeven wanneer de gebruiker machtigingen voor een register heeft, maar geen machtigingen op entiteitsniveau lezer op het abonnement. U lost dit probleem, aan de gebruiker toewijzen Reader-machtigingen voor het abonnement:


```azurecli  
az role assignment create --role "Reader" --assignee user@contoso.com --scope /subscriptions/<subscription_id> 
```

### <a name="firewall-rules-are-updated-successfully-but-they-do-not-take-effect"></a>Firewall-regels zijn bijgewerkt, maar ze worden niet doorgevoerd

Het duurt enige tijd aan het doorvoeren van wijzigingen van de firewall-regel. Nadat u de firewall-instellingen hebt gewijzigd, wacht een paar minuten voordat deze wijziging te controleren.


## <a name="registry-operations"></a>Registerbewerkingen

- [Hoe krijg ik toegang tot Docker Registry HTTP-API V2?](#how-do-i-access-docker-registry-http-api-v2)
- [Hoe kan ik alle manifesten die niet wordt verwezen door een tag op in een opslagplaats verwijderen?](#how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository)
- [Waarom het quotagebruik register niet in mindering na het verwijderen van installatiekopieën?](#why-does-the-registry-quota-usage-not-reduce-after-deleting-images)
- [Hoe ik opslag quota wijzigingen valideren?](#how-do-i-validate-storage-quota-changes)
- [Hoe Verifieer ik met mijn register bij het uitvoeren van de CLI in een container?](#how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container)
- [Biedt Azure Container Registry TLS 1.2 alleen configuratie en het inschakelen van TLS 1.2?](#does-azure-container-registry-offer-tls-v12-only-configuration-and-how-to-enable-tls-v12)
- [Azure Container Registry ondersteunt inhoud vertrouwen?](#does-azure-container-registry-support-content-trust)
- [Hoe ik toegang verlenen tot push of pull installatiekopieën zonder toestemming om de registerresource te beheren?](#how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource)
- [Hoe kan ik afbeeldingen automatisch in quarantaine plaatsen voor een register inschakelen](#how-do-i-enable-automatic-image-quarantine-for-a-registry)

### <a name="how-do-i-access-docker-registry-http-api-v2"></a>Hoe krijg ik toegang tot Docker Registry HTTP-API V2?

ACR biedt ondersteuning voor Docker-register HTTP-API V2. De API's kunnen worden geopend op `https://<your registry login server>/v2/`. Voorbeeld: `https://mycontainerregistry.azurecr.io/v2/`

### <a name="how-do-i-delete-all-manifests-that-are-not-referenced-by-any-tag-in-a-repository"></a>Hoe kan ik alle manifesten die niet wordt verwezen door een tag op in een opslagplaats verwijderen?

Als u in bash bent:

```bash
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv  | xargs -I% az acr repository delete -n myRegistry -t myRepository@%
```

Voor Powershell:

```powershell
az acr repository show-manifests -n myRegistry --repository myRepository --query "[?tags[0]==null].digest" -o tsv | %{ az acr repository delete -n myRegistry -t myRepository@$_ }
```

Opmerking: U kunt toevoegen `-y` in de verwijderopdracht bevestiging overslaan.

Zie voor meer informatie, [verwijderen van installatiekopieën van containers in Azure Container Registry](container-registry-delete.md).

### <a name="why-does-the-registry-quota-usage-not-reduce-after-deleting-images"></a>Waarom het quotagebruik register niet in mindering na het verwijderen van installatiekopieën?

Deze situatie kan zich voordoen als de onderliggende lagen worden nog steeds wordt verwezen door andere containerinstallatiekopieën. Als u een afbeelding met geen verwijzingen verwijdert, wordt het gebruik van het register in een paar minuten bijgewerkt.

### <a name="how-do-i-validate-storage-quota-changes"></a>Hoe ik opslag quota wijzigingen valideren?

Een installatiekopie maken met een 1GB-laag met behulp van de volgende docker-bestand. Dit zorgt ervoor dat de installatiekopie heeft een laag die niet wordt gedeeld door een andere afbeelding in het register.

```dockerfile
FROM alpine
RUN dd if=/dev/urandom of=1GB.bin  bs=32M  count=32
RUN ls -lh 1GB.bin
```

Bouw en push de installatiekopie naar uw register met behulp van de docker CLI.

```bash
docker build -t myregistry.azurecr.io/1gb:latest .
docker push myregistry.azurecr.io/1gb:latest
```

U zou het mogelijk om te zien dat het opslaggebruik is toegenomen in Azure portal of u kunt een query met behulp van de CLI gebruik.

```bash
az acr show-usage -n myregistry
```

Verwijderen van de installatiekopie met behulp van de Azure CLI of de portal en controleer de bijgewerkte gebruik in een paar minuten.

```bash
az acr repository delete -n myregistry --image 1gb
```

### <a name="how-do-i-authenticate-with-my-registry-when-running-the-cli-in-a-container"></a>Hoe Verifieer ik met mijn register bij het uitvoeren van de CLI in een container?

U moet de Azure CLI-container uitvoeren door het koppelen van de Docker-socket:

```bash
docker run -it -v /var/run/docker.sock:/var/run/docker.sock azuresdk/azure-cli-python:dev
```

In de container, installeert `docker`:

```bash
apk --update add docker
```

Vervolgens kunt u verifiëren met uw register:

```azurecli
az acr login -n MyRegistry
```

### <a name="does-azure-container-registry-offer-tls-v12-only-configuration-and-how-to-enable-tls-v12"></a>Biedt Azure Container Registry TLS 1.2 alleen configuratie en het inschakelen van TLS 1.2?

Ja. TLS in te schakelen met behulp van een recente dockerclient (versie 18.03.0 en hoger). 

### <a name="does-azure-container-registry-support-content-trust"></a>Azure Container Registry ondersteunt inhoud vertrouwen?

Ja, u kunt vertrouwde installatiekopieën gebruiken in Azure Container Registry, aangezien de [Docker notaris](https://docs.docker.com/notary/getting_started/) is geïntegreerd en kan worden ingeschakeld. Zie voor meer informatie, [inhoud wordt vertrouwd in Azure Container Registry](container-registry-content-trust.md).


####  <a name="where-is-the-file-for-the-thumbprint-located"></a>Waar is het bestand voor de vingerafdruk vinden?

Onder `~/.docker/trust/tuf/myregistry.azurecr.io/myrepository/metadata`:

* Openbare sleutels en certificaten van alle rollen (met uitzondering van de delegatie-rollen) worden opgeslagen in de `root.json`.
* Openbare sleutels en certificaten van de rol van de overdracht worden opgeslagen in het JSON-bestand van de bovenliggende rol (bijvoorbeeld `targets.json` voor de `targets/releases` rol).

Het wordt aangeraden om te controleren of de openbare sleutels en certificaten na de verificatie van de algehele TUF uitgevoerd door de client voor Docker en notaris.

### <a name="how-do-i-grant-access-to-pull-or-push-images-without-permission-to-manage-the-registry-resource"></a>Hoe ik toegang verlenen tot push of pull installatiekopieën zonder toestemming om de registerresource te beheren?

Biedt ondersteuning voor ACR [aangepaste rollen](container-registry-roles.md) die verschillende niveaus van machtigingen bieden. Met name `AcrPull` en `AcrPush` rollen toestaan dat gebruikers op pull-en/of push afbeeldingen zonder de toestemming om de registerresource in Azure te beheren.

* Azure Portal: Het register Access Control (IAM) -> toevoegen -> (Selecteer `AcrPull` of `AcrPush` voor de rol).
* Azure CLI: Zoek de resource-ID van het register door het uitvoeren van de volgende opdracht uit:

  ```azurecli
  az acr show -n myRegistry
  ```
  
  Vervolgens kunt u de `AcrPull` of `AcrPush` rol aan een gebruiker (het volgende voorbeeld wordt `AcrPull`):

  ```azurecli
    az role assignment create --scope resource_id --role AcrPull --assignee user@example.com
    ```

  Of de rol toewijzen aan een service-principal die is geïdentificeerd door de toepassings-ID:

  ```
  az role assignment create --scope resource_id --role AcrPull --assignee 00000000-0000-0000-0000-000000000000
  ```

De toegewezen gebruiker kan vervolgens om te verifiëren en toegang tot afbeeldingen in het register.

* Om te verifiëren met een register:
    
  ```azurecli
  az acr login -n myRegistry 
  ```

* Aan de lijst met opslagplaatsen:

  ```azurecli
  az acr repository list -n myRegistry
  ```

 Voor het ophalen van een installatiekopie:
    
  ```azurecli
  docker pull myregistry.azurecr.io/hello-world
  ```

Met het gebruik van alleen de `AcrPull` of `AcrPush` rol, de toegewezen gebruiker beschikt niet over de machtiging om de registerresource in Azure te beheren. Bijvoorbeeld, `az acr list` of `az acr show -n myRegistry` het register niet worden weergegeven.

### <a name="how-do-i-enable-automatic-image-quarantine-for-a-registry"></a>Hoe kan ik afbeeldingen automatisch in quarantaine plaatsen voor een register inschakelen?

Afbeelding in quarantaine is momenteel een preview-functie van de ACR. U kunt de modus in quarantaine plaatsen van een register inschakelen zodat alleen de installatiekopieën die zijn security scan is geslaagd zichtbaar voor normale gebruikers zijn. Zie voor meer informatie, de [ACR GitHub-opslagplaats](https://github.com/Azure/acr/tree/master/docs/preview/quarantine).

## <a name="diagnostics-and-health-checks"></a>Diagnostische gegevens en statuscontroles

- [Controleer de status met `az acr check-health`](#check-health-with-az-acr-check-health)
- [docker pull is mislukt met fout: net/http: aanvraag geannuleerd tijdens het wachten op voor verbinding (Client.Timeout overschreden tijdens het wachten op headers)](#docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers)
- [docker push is geslaagd, maar docker pull is mislukt met fout: niet-geautoriseerde: verificatie is vereist](#docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required)
- [Inschakelen en de logboeken voor foutopsporing van de docker-daemon ophalen](#enable-and-get-the-debug-logs-of-the-docker-daemon) 
- [Nieuwe gebruikersmachtigingen kunnen niet effectief worden onmiddellijk na het bijwerken van](#new-user-permissions-may-not-be-effective-immediately-after-updating)
- [Verificatie-informatie is niet opgenomen in de juiste indeling op direct REST API-aanroepen](#authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls)
- [Waarom Azure portal niet wordt vermeld in alle opslagplaatsen of tags?](#why-does-the-azure-portal-not-list-all-my-repositories-or-tags)
- [Hoe ik http traceringen verzamelen op Windows?](#how-do-i-collect-http-traces-on-windows)

### <a name="check-health-with-az-acr-check-health"></a>Controleer de status met `az acr check-health`

Zie voor het oplossen van algemene omgeving en registerproblemen, [controleert de status van een Azure container registry](container-registry-check-health.md).

### <a name="docker-pull-fails-with-error-nethttp-request-canceled-while-waiting-for-connection-clienttimeout-exceeded-while-awaiting-headers"></a>docker pull is mislukt met fout: net/http: aanvraag geannuleerd tijdens het wachten op voor verbinding (Client.Timeout overschreden tijdens het wachten op headers)

 - Als deze fout een tijdelijk probleem probeer het opnieuw worden uitgevoerd is.
 - Als `docker pull` mislukt, kan er een probleem met de Docker-daemon. Het probleem kan in het algemeen worden verholpen door de Docker-daemon opnieuw te starten. 
 - Als u doorgaat wilt bekijken van dit probleem na de Docker-daemon opnieuw op te starten, klikt u vervolgens de probleem kan worden enkele problemen met de netwerkverbinding met de machine. Als u wilt controleren of de algemene netwerk op de computer in orde is, voer de volgende opdracht voor het testen eindpunt verbinding. De minimale `az acr` versie met deze connectiviteitsopdracht is 2.2.9. Uw Azure-CLI bijwerken als u een oudere versie.
 
   ```azurecli
    az acr check-health -n myRegistry
    ```
 - U hebt altijd een mechanisme voor opnieuw proberen van alle Docker-clientbewerkingen.

### <a name="docker-pull-is-slow"></a>Docker pull is traag
Gebruik [dit](http://www.azurespeed.com/Azure/Download) hulpprogramma voor het testen van de downloadsnelheid van uw machine-netwerk. Als de machinenetwerk traag is, kunt u overwegen virtuele Azure-machine in dezelfde regio als het register. Dit biedt u gewoonlijk sneller snelheid van het netwerk.

### <a name="docker-push-is-slow"></a>Docker push is traag
Gebruik [dit](http://www.azurespeed.com/Azure/Upload) hulpprogramma voor het testen van de uploadsnelheid van uw machine-netwerk. Als de machinenetwerk traag is, kunt u overwegen virtuele Azure-machine in dezelfde regio als het register. Dit biedt u gewoonlijk sneller snelheid van het netwerk.

### <a name="docker-push-succeeds-but-docker-pull-fails-with-error-unauthorized-authentication-required"></a>docker push is geslaagd, maar docker pull is mislukt met fout: niet-geautoriseerde: verificatie is vereist

Deze fout kan optreden met de Red Hat-versie van de Docker-daemon waar `--signature-verification` is standaard ingeschakeld. U kunt de Docker-daemon-opties voor Red Hat Enterprise Linux (RHEL) of Fedora controleren door de volgende opdracht uit:

```bash
grep OPTIONS /etc/sysconfig/docker
```

Bijvoorbeeld, heeft Fedora 28-Server de volgende docker-daemon-opties:

```
OPTIONS='--selinux-enabled --log-driver=journald --live-restore'
```

Met `--signature-verification=false` ontbreekt, `docker pull` mislukt met een fout op vergelijkbaar met:

```bash
Trying to pull repository myregistry.azurecr.io/myimage ...
unauthorized: authentication required
```

De fout kunt oplossen:
1. De optie toevoegen `--signature-verification=false` naar het configuratiebestand van de Docker-daemon `/etc/sysconfig/docker`. Bijvoorbeeld:

  ```
  OPTIONS='--selinux-enabled --log-driver=journald --live-restore --signature-verification=false'
  ```
2. De Docker-daemon-service opnieuw starten met de volgende opdracht:

  ```bash
  sudo systemctl restart docker.service
  ```

Details van `--signature-verification` kunt u vinden door te voeren `man dockerd`.

### <a name="enable-and-get-the-debug-logs-of-the-docker-daemon"></a>Inschakelen en de logboeken voor foutopsporing van de Docker-daemon ophalen  

Start `dockerd` met de `debug` optie. Maak eerst het configuratiebestand van de Docker-daemon (`/etc/docker/daemon.json`) als deze niet bestaat, of Voeg de `debug` optie:

```json
{   
    "debug": true   
}
```

De daemon opnieuw te starten. Bijvoorbeeld, met Ubuntu 14.04:

```bash
sudo service docker restart
```

Meer informatie vindt u de [Docker-documentatie](https://docs.docker.com/engine/admin/#enable-debugging). 

 * De logboeken kunnen worden gegenereerd op verschillende locaties, afhankelijk van uw systeem. Bijvoorbeeld: voor Ubuntu 14.04, heeft `/var/log/upstart/docker.log`.   
Zie [Docker-documentatie](https://docs.docker.com/engine/admin/#read-the-logs) voor meer informatie.    

 * De logboeken worden gegenereerd voor Docker voor Windows, onder % LOCALAPPDATA%/docker/. Maar deze bevat mogelijk niet alle foutopsporingsinformatie nog.   

   Voor toegang tot het logboek voor volledige-daemon, moet u mogelijk enkele extra stappen uitvoeren:

    ```console
    docker run --privileged -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /usr/local/bin/docker:/usr/local/bin/docker alpine sh

    docker run --net=host --ipc=host --uts=host --pid=host -it --security-opt=seccomp=unconfined --privileged --rm -v /:/host alpine /bin/sh
    chroot /host
    ```
    Nu u toegang tot de bestanden van de virtuele machine die wordt uitgevoerd hebt `dockerd`. Het logboek is op `/var/log/docker.log`.

### <a name="new-user-permissions-may-not-be-effective-immediately-after-updating"></a>Nieuwe gebruikersmachtigingen kunnen niet effectief worden onmiddellijk na het bijwerken van

Wanneer u nieuwe verleent machtigingen (nieuwe rollen) voor een service-principal van de wijziging mogelijk niet onmiddellijk van kracht. Er zijn twee mogelijke redenen:

* Azure Active Directory-rol toewijzing vertraging. Normaal gesproken is het snel, maar het kan duren vanwege doorgifte vertraging.
* Machtiging vertraging op token ACR-server. Dit kan maximaal tien minuten duren. Als u wilt oplossen, kunt u `docker logout` en vervolgens te verifiëren opnieuw aan dezelfde gebruiker na 1 minuut:

  ```bash
  docker logout myregistry.azurecr.io
  docker login myregistry.azurecr.io
  ```

ACR ondersteunt momenteel niet home replicatie verwijderen door de gebruikers. De tijdelijke oplossing is om op te nemen van de oorspronkelijke replicatie maken in de sjabloon, maar het is gemaakt door toe te voegen overslaan `"condition": false` zoals hieronder wordt weergegeven:

```json
{
    "name": "[concat(parameters('acrName'), '/', parameters('location'))]",
    "condition": false,
    "type": "Microsoft.ContainerRegistry/registries/replications",
    "apiVersion": "2017-10-01",
    "location": "[parameters('location')]",
    "properties": {},
    "dependsOn": [
        "[concat('Microsoft.ContainerRegistry/registries/', parameters('acrName'))]"
     ]
},
```

### <a name="authentication-information-is-not-given-in-the-correct-format-on-direct-rest-api-calls"></a>Verificatie-informatie is niet opgenomen in de juiste indeling op direct REST API-aanroepen

U kunt tegenkomen een `InvalidAuthenticationInfo` fout, met name met behulp van de `curl` hulpprogramma met de optie `-L`, `--location` (naar omleidingen volgen).
Bijvoorbeeld: ophalen van de blob via `curl` met `-L` optie en basisverificatie wordt gebruikt:

```bash
curl -L -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest
```

kan leiden tot het volgende antwoord:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Error><Code>InvalidAuthenticationInfo</Code><Message>Authentication information is not given in the correct format. Check the value of Authorization header.
RequestId:00000000-0000-0000-0000-000000000000
Time:2019-01-01T00:00:00.0000000Z</Message></Error>
```

De hoofdoorzaak is dat sommige `curl` implementaties omleidingen met koppen volgen uit de oorspronkelijke aanvraag.

U moet volgen omleidingen handmatig zonder de headers het probleem op te lossen. De antwoordheaders met afdrukken de `-D -` optie van `curl` en pak vervolgens: de `Location` header:

```bash
redirect_url=$(curl -s -D - -H "Authorization: basic $credential" https://$registry.azurecr.io/v2/$repository/blobs/$digest | grep "^Location: " | cut -d " " -f2 | tr -d '\r')
curl $redirect_url
```

### <a name="why-does-the-azure-portal-not-list-all-my-repositories-or-tags"></a>Waarom Azure portal niet wordt vermeld in alle opslagplaatsen of tags? 

Als u van de browser Microsoft Edge/IE gebruikmaakt, kunt u maximaal 100 opslaglocaties of labels zien. Als het register meer dan 100 opslaglocaties of labels heeft, wordt u aangeraden dat u de browser Firefox of Chrome gebruiken om ze allemaal weer te geven.

### <a name="how-do-i-collect-http-traces-on-windows"></a>Hoe ik http traceringen verzamelen op Windows?

#### <a name="prerequisites"></a>Vereisten

- Ontsleutelen https in fiddler inschakelen:  <https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS>
- Docker op het gebruik van een proxy via de Docker-gebruikersinterface inschakelen: <https://docs.docker.com/docker-for-windows/#proxies>
- Zorg ervoor dat herstellen als u klaar bent.  Docker werkt niet met dit is ingeschakeld en fiddler niet wordt uitgevoerd.

#### <a name="windows-containers"></a>Windows-containers

Docker-proxy voor 127.0.0.1:8888 configureren

#### <a name="linux-containers"></a>Linux-containers

De IP-adres van de Docker vm virtuele switch vinden:

```powershell
(Get-NetIPAddress -InterfaceAlias "*Docker*" -AddressFamily IPv4).IPAddress
```

Configureer de Docker-proxy voor de uitvoer van de vorige opdracht en de poort 8888 (bijvoorbeeld 10.0.75.1:8888)

## <a name="tasks"></a>Taken

- [Hoe ik batch-annuleren wordt uitgevoerd?](#how-do-i-batch-cancel-runs)
- [Hoe moet ik de map .git opnemen in de az acr build-opdracht?](#how-do-i-include-the-git-folder-in-az-acr-build-command)

### <a name="how-do-i-batch-cancel-runs"></a>Hoe ik batch-annuleren wordt uitgevoerd?

De volgende opdrachten annuleren alle actieve taken in de opgegeven registersleutel.

```azurecli
az acr task list-runs -r $myregistry --run-status Running --query '[].runId' -o tsv \
| xargs -I% az acr task cancel-run -r $myregistry --run-id %
```

### <a name="how-do-i-include-the-git-folder-in-az-acr-build-command"></a>Hoe moet ik de map .git opnemen in de az acr build-opdracht?

Als u een lokale map op de `az acr build` opdracht, de `.git` map is uitgesloten van het geüploade pakket standaard. U kunt maken een `.dockerignore` bestand met de volgende instelling. Geeft het aan de opdracht voor het herstellen van alle bestanden onder `.git` in het geüploade pakket. 

```
!.git/**
```

Deze instelling geldt ook voor de `az acr run` opdracht.

## <a name="cicd-integration"></a>CI/CD-integratie

- [CircleCI](https://github.com/Azure/acr/blob/master/docs/integration/CircleCI.md)
- [GitHub-acties](https://github.com/Azure/acr/blob/master/docs/integration/github-actions/github-actions.md)

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie](container-registry-intro.md) over Azure Container Registry.