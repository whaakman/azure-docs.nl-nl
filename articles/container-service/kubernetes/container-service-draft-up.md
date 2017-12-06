---
title: Concept gebruiken met Azure Containerservice en Azure Container register
description: Maak een ACS Kubernetes-cluster en een Azure Container Registry om uw eerste toepassing met Draft te maken in Azure.
services: container-service
author: squillace
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 09/14/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: 9cf5b1227e69ada46bc0b1e5ff01cc12e73598f7
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes"></a>Draft gebruiken met Azure Container Service en Azure Container Registry om een Kubernetes-toepassing te bouwen en implementeren

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

[Draft](https://aka.ms/draft) is een nieuw open-source hulpprogramma waarmee u eenvoudig containertoepassingen kunt ontwikkelen en ze kunt implementeren in Kubernetes-clusters. U hoeft hiervoor niet veel te weten over Docker en Kubernetes (u hoeft ze zelfs niet eens te installeren!). Als u een hulpprogramma als Draft gebruikt, kunnen u en uw team zich richten op het bouwen van de toepassing met Kubernetes en hoeft er minder aandacht te worden besteed aan de infrastructuur.

U kunt Draft gebruiken met alle Docker-installatiekopieregisters en alle Kubernetes-clusters, ook de lokale. Deze zelfstudie laat zien hoe u ACS met Kubernetes en ACR maken van een live, maar veilig developer-pijplijn in Kubernetes concept met en het gebruik van Azure DNS te kunnen stellen dat ontwikkelaars pijplijn voor anderen kunnen zien in een ander domein.


## <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken
U kunt eenvoudig [een nieuw Azure Container Registry maken](../../container-registry/container-registry-get-started-azure-cli.md). De stappen daarvoor zijn als volgt:

1. Maak een Azure-resourcegroep voor het beheren van uw ACR-register en het Kubernetes-cluster in ACS.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Maak een ACR installatiekopie register met [az acr maken](/cli/azure/acr#create) en zorg ervoor dat de `--admin-enabled` optie is ingesteld op `true`.
      ```azurecli
      az acr create --resource-group draft --name draftacs --sku Basic
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a>Een Azure Container Service maken met Kubernetes

U bent er nu klaar voor om [az acs create](/cli/azure/acs#create) te gebruiken voor het maken van een ACS-cluster met Kubernetes als de `--orchestrator-type`-waarde.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes --generate-ssh-keys
```

> [!NOTE]
> Kubernetes is niet het standaardorchestratortype, dus gebruik de `--orchestrator-type kubernetes`-switch.

Wanneer dit is gelukt, ziet de uitvoer er als volgt uit.

```json
waiting for AAD role to propagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

Wanneer u een cluster hebt, kunt u de referenties importeren met de opdracht [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials). U hebt nu een lokaal configuratiebestand voor uw cluster. Dit bestand hebben Helm en Draft nodig om werk te verrichten.

## <a name="install-and-configure-draft"></a>Draft installeren en configureren


1. Concept voor uw omgeving op https://github.com/Azure/draft/releases downloaden en installeren in het pad, zodat de opdracht kan worden gebruikt.
2. Helm voor uw omgeving op https://github.com/kubernetes/helm/releases downloaden en [installeren in het pad, zodat de opdracht kan worden gebruikt](https://github.com/kubernetes/helm/blob/master/docs/install.md#installing-the-helm-client).
3. Configureer Draft om uw eigen register te gebruiken en om subdomeinen te maken voor elk Helm-diagram dat wordt gemaakt. U hebt het volgende nodig voor het configureren van Draft:
  - de naam van uw Azure Container Registry (in dit voorbeeld is dat `draftacsdemo`)
  - de registersleutel of het wachtwoord van `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.

  Roep `draft init` en het configuratieproces wordt u gevraagd om de bovenstaande waarden; dat de URL-indeling voor de Register-URL is de naam van routeringsregister (in dit voorbeeld `draftacsdemo`) plus `.azurecr.io`. De gebruikersnaam is de registernaam van het op een eigen. De eerste keer dat u het proces uitvoert, ziet het er ongeveer als volgt uit.
 ```bash
    $ draft init
    Creating /home/ralph/.draft 
    Creating /home/ralph/.draft/plugins 
    Creating /home/ralph/.draft/packs 
    Creating pack go...
    Creating pack python...
    Creating pack ruby...
    Creating pack javascript...
    Creating pack gradle...
    Creating pack java...
    Creating pack php...
    Creating pack csharp...
    $DRAFT_HOME has been configured at /home/ralph/.draft.

    In order to configure Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io/myuser, quay.io/myuser, myregistry.azurecr.io): draftacsdemo.azurecr.io
    2. Enter your username: draftacsdemo
    3. Enter your password: 
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
```

U kunt nu een toepassing implementeren.


## <a name="build-and-deploy-an-application"></a>Een toepassing bouwen en implementeren

In de Draft-opslagplaats staan [zes eenvoudige voorbeeldtoepassingen](https://github.com/Azure/draft/tree/master/examples). Kloon de opslagplaats en we gebruiken de [Java-voorbeeld](https://github.com/Azure/draft/tree/master/examples/java). Wijzigen in de voorbeelden/java-directory, en het type `draft create` de toepassing te bouwen. De toepassing zou er als volgt moeten uitzien.
```bash
$ draft create
--> Draft detected the primary language as Java with 91.228814% certainty.
--> Ready to sail
```

De uitvoer bevat een Dockerfile en een Helm-diagram. Typ `draft up` om te bouwen en implementeren. De uitvoer is uitgebreid, maar moet zich op het volgende voorbeeld.
```bash
$ draft up
Draft Up Started: 'handy-labradoodle'
handy-labradoodle: Building Docker Image: SUCCESS ⚓  (35.0232s)
handy-labradoodle: Pushing Docker Image: SUCCESS ⚓  (17.0062s)
handy-labradoodle: Releasing Application: SUCCESS ⚓  (3.8903s)
handy-labradoodle: Build ID: 01BT0ZJ87NWCD7BBPK4Y3BTTPB
```

## <a name="securely-view-your-application"></a>Uw toepassing veilig weergeven

De container wordt nu uitgevoerd in ACS. Gebruiken om het te bekijken, de `draft connect` opdracht maakt een beveiligde verbinding aan IP-adres van het cluster met een specifieke poort voor uw toepassing zodat u deze lokaal kunt bekijken. Als dit lukt, zoekt u naar de URL die u wilt verbinding maken met uw app op de eerste regel na de **geslaagd** indicator.

> [!NOTE]
> Als er een bericht weergegeven dat geen gehele product gereed zijn, wacht een ogenblik en probeer het opnieuw of u kunt het gehele product gereed met bekijken `kubectl get pods -w` en probeer het opnieuw wanneer ze doen.

```bash
draft connect
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

In het voorgaande voorbeeld, typt u `curl -s http://localhost:46143` voor het ontvangen van het antwoord `Hello World, I'm Java!`. Wanneer u CTRL + of CMD + C (afhankelijk van uw omgeving OS), beveiligde tunnel gegevenskanaal en u kunt doorgaan doorlopen.

## <a name="sharing-your-application-by-configuring-a-deployment-domain-with-azure-dns"></a>Uw toepassing delen door het configureren van een domein implementatie met Azure DNS

U hebt al de ontwikkelaar herhaling lus die concept in de voorgaande stappen maakt hebt uitgevoerd. U kunt echter uw toepassing delen via internet door:
1. Een inkomend installeren in uw cluster ACS (voor een openbaar IP-adres waarop de app weergeven)
2. Uw aangepaste domein naar Azure DNS overdragen en toewijzing van uw domein met de IP-adres ACS wordt toegewezen aan uw ingress-controller

### <a name="use-helm-to-install-the-ingress-controller"></a>Helm gebruiken om de ingress-controller te installeren.
Gebruik **helm** zoeken naar en installeren `stable/traefik`, een domeincontroller inkomend inkomende aanvragen voor uw builds inschakelen.
```bash
$ helm search traefik
NAME            VERSION DESCRIPTION
stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

$ helm install stable/traefik --name ingress
```
Stel nu een controle in voor de `ingress`-controller om de externe IP-waarde vast te leggen bij de implementatie. Dit IP-adres wordt [toegewezen aan het implementatiedomein](#wire-up-deployment-domain) in de volgende sectie.

```bash
$ kubectl get svc -w
NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
kubernetes                    10.0.0.1       <none>          443/TCP                      7h
```

In dit geval is het externe IP-adres voor het implementatiedomein `13.64.108.240`. U kunt uw domein nu toewijzen aan dat IP-adres.

### <a name="map-the-ingress-ip-to-a-custom-subdomain"></a>Het inkomend IP-adres toewijzen aan een aangepast subdomein

Draft maakt een release voor elke Helm-grafiek die er wordt gemaakt en elke toepassing waar u aan werkt. Elk ervan een gegenereerde naam die wordt gebruikt door opgehaald **concept** als een _subdomein_ boven op de hoofdmap _implementatie domein_ dat u beheert. (In dit voorbeeld wordt `squillace.io` gebruikt als domein voor implementatie.) Als u dit subdomeingedrag wilt inschakelen, moet u een A-record maken voor `'*.draft'` in de DNS-vermeldingen van het implementatiedomein. Op die manier wordt elk gegenereerde subdomein gerouteerd naar de controller voor binnenkomend verkeer van het Kubernetes-cluster. 

Elke eigen domeinprovider wijst op zijn eigen manier DNS-servers toe. Als u uw [domeinnaamservers wilt overdragen aan Azure DNS](../../dns/dns-delegate-domain-azure-dns.md), voert u de volgende stappen uit:

1. Maak een resourcegroep voor uw zone.
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. Maak een DNS-zone voor uw domein.
Gebruik de opdracht [az network dns zone create](/cli/azure/network/dns/zone#create) om de naamservers te verkrijgen voor het delegeren van DNS-controle over uw domein aan Azure.
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. Voeg de DNS-servers die u ontvangt toe aan de domeinprovider van uw implementatiedomein. Op die manier kunt u Azure DNS gebruiken om uw domein naar wens opnieuw toe te wijzen. De manier waarop u dit doet, is afhankelijk van het domein opgeven; [uw domein nameservers naar Azure DNS overdragen](../../dns/dns-delegate-domain-azure-dns.md) bevat enkele van de details die u moet weten. 
4. Als uw domein is overgedragen naar Azure DNS, maakt u een A-Recordset-vermelding voor uw implementatie domein wordt toegewezen aan de `ingress` IP-adres uit stap 2 van de vorige sectie.
  ```azurecli
  az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*.draft' -g squillace.io -z squillace.io
  ```
De uitvoer ziet er ongeveer zo uit:
  ```json
  {
    "arecords": [
      {
        "ipv4Address": "13.64.108.240"
      }
    ],
    "etag": "<guid>",
    "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
    "metadata": null,
    "name": "*.draft",
    "resourceGroup": "squillace.io",
    "ttl": 3600,
    "type": "Microsoft.Network/dnszones/A"
  }
  ```
5. Installeer **concept**

   1. Verwijder **draftd** uit het cluster door te typen `helm delete --purge draft`. 
   2. Installeer **concept** met behulp van dezelfde `draft-init` opdracht, maar met de `--ingress-enabled` optie:
    ```bash
    draft init --ingress-enabled
    ```
   Reageren op de prompts net als de eerste keer hierboven. Maar hebt u een vraag te beantwoorden, met behulp van de volledige domeinnaam pad die u hebt geconfigureerd met de Azure DNS-server.

6. Voer uw domein op het hoogste niveau voor inkomende gegevens (bijvoorbeeld draft.example.com): draft.squillace.io
7. Als u aanroept `draft up` dit moment kunt u zich kunt zien van uw toepassing (of `curl` het) op de URL van het formulier `<appname>.draft.<domain>.<top-level-domain>`. In dit voorbeeld `http://handy-labradoodle.draft.squillace.io`. 
```bash
curl -s http://handy-labradoodle.draft.squillace.io
Hello World, I'm Java!
```


## <a name="next-steps"></a>Volgende stappen

Nu u beschikt over een ACS Kubernetes-cluster, kunt u onderzoek uitvoeren met [Azure Container Registry](../../container-registry/container-registry-intro.md) om meer en andere implementaties te maken voor dit scenario. U kunt bijvoorbeeld de domein-DNS-recordset draft._basedomain.toplevel_ maken om processen van specifieke ACS-implementaties vanuit een dieper gelegen subdomein te controleren.






