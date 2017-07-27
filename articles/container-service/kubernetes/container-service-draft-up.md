---
title: Draft gebruiken met Azure Container Service en Azure Container Registry | Microsoft Docs
description: Maak een ACS Kubernetes-cluster en een Azure Container Registry om uw eerste toepassing met Draft te maken in Azure.
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: Docker, containers, microservices, Kubernetes, Draft, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: b70d2340c0f1286fa355a78a4cd0cb1ce37cbc39
ms.contentlocale: nl-nl
ms.lasthandoff: 07/25/2017

---

# <a name="use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes"></a>Draft gebruiken met Azure Container Service en Azure Container Registry om een Kubernetes-toepassing te bouwen en implementeren

[Draft](https://aka.ms/draft) is een nieuw open-source hulpprogramma waarmee u eenvoudig containertoepassingen kunt ontwikkelen en ze kunt implementeren in Kubernetes-clusters. U hoeft hiervoor niet veel te weten over Docker en Kubernetes (u hoeft ze zelfs niet eens te installeren!). Als u een hulpprogramma als Draft gebruikt, kunnen u en uw team zich richten op het bouwen van de toepassing met Kubernetes en hoeft er minder aandacht te worden besteed aan de infrastructuur.

U kunt Draft gebruiken met alle Docker-installatiekopieregisters en alle Kubernetes-clusters, ook de lokale. In deze zelfstudie leert u hoe u ACS gebruikt met Kubernetes, ACR en Azure DNS om een live-CI/CD-ontwikkelingspijplijn maakt met Draft.


## <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken
U kunt eenvoudig [een nieuw Azure Container Registry maken](../../container-registry/container-registry-get-started-azure-cli.md). De stappen daarvoor zijn als volgt:

1. Maak een Azure-resourcegroep voor het beheren van uw ACR-register en het Kubernetes-cluster in ACS.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Een ACR-installatiekopiekopieregister maken met [az acr create](/cli/azure/acr#create)
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a>Een Azure Container Service maken met Kubernetes

U bent er nu klaar voor om [az acs create](/cli/azure/acs#create) te gebruiken voor het maken van een ACS-cluster met Kubernetes als de `--orchestrator-type`-waarde.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
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
De installatie-instructies voor Draft staan in de [Draft-opslagplaats](https://github.com/Azure/draft/blob/master/docs/install.md). De instructies zijn relatief eenvoudig, maar er is wel enige configuratie nodig. [Helm](https://aka.ms/helm) is namelijk nodig om een Helm-grafiek te maken en implementeren in het Kubernetes-cluster.

1. [Download en installeer Helm](https://aka.ms/helm#install).
2. Gebruik Helm om `stable/traefik` te zoeken en installeren en gebruik een ingangscontroller om binnenkomende verzoeken in te schakelen voor uw builds.
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    Stel nu een controle in voor de `ingress`-controller om de externe IP-waarde vast te leggen bij de implementatie. Dit IP-adres wordt [toegewezen aan het implementatiedomein](#wire-up-deployment-domain) in de volgende sectie.

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    In dit geval is het externe IP-adres voor het implementatiedomein `13.64.108.240`. U kunt uw domein nu toewijzen aan dat IP-adres.

## <a name="wire-up-deployment-domain"></a>Wire-up van het implementatiedomein

Draft maakt een release voor elke Helm-grafiek die er wordt gemaakt en elke toepassing waar u aan werkt. Elk item krijgt een gegenereerde naam die door Draft als _subdomein_ wordt gebruikt op het hoofd_implementatiedomein_ dat u beheert. (In dit voorbeeld wordt `squillace.io` gebruikt als domein voor implementatie.) Als u dit subdomeingedrag wilt inschakelen, moet u een A-record maken voor `'*'` in de DNS-vermeldingen van het implementatiedomein. Op die manier wordt elk gegenereerde subdomein gerouteerd naar de controller voor binnenkomend verkeer van het Kubernetes-cluster.

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
3. Voeg de DNS-servers die u ontvangt toe aan de domeinprovider van uw implementatiedomein. Op die manier kunt u Azure DNS gebruiken om uw domein naar wens opnieuw toe te wijzen.
4. Maak een A-recordsetvermelding voor uw implementatiedomein dat is toegewezen aan het `ingress` IP-adres uit stap 2 van de vorige sectie.
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
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
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. Configureer Draft om uw eigen register te gebruiken en om subdomeinen te maken voor elk Helm-diagram dat wordt gemaakt. U hebt het volgende nodig voor het configureren van Draft:
  - de naam van uw Azure Container Registry (in dit voorbeeld is dat `draft`)
  - de registersleutel of het wachtwoord van `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.
  - het hoofdimplementatiedomein dat u hebt geconfigureerd voor toewijzing aan het externe-IP-adres voor binnenkomend verkeer van Kubernetes (hier is dat `squillace.io`)

  Roep `draft init` aan. In het configuratieproces wordt u dan om de bovenstaande waarden gevraagd. De eerste keer dat u het proces uitvoert, ziet het er ongeveer als volgt uit.
    ```
    draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order to install Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

U kunt nu een toepassing implementeren.


## <a name="build-and-deploy-an-application"></a>Een toepassing bouwen en implementeren

In de Draft-opslagplaats staan [zes eenvoudige voorbeeldtoepassingen](https://github.com/Azure/draft/tree/master/examples). Kloon de opslagplaats en gebruik het [Python-voorbeeld](https://github.com/Azure/draft/tree/master/examples/python). Ga naar de map examples/Python en typ `draft create` om de toepassing te bouwen. De toepassing zou er als volgt moeten uitzien.
```bash
$ draft create
--> Python app detected
--> Ready to sail
```

De uitvoer bevat een Dockerfile en een Helm-diagram. Typ `draft up` om te bouwen en implementeren. De uitvoer is uitgebreid, maar begint als het volgende voorbeeld.
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

en eindigt ongeveer als in het volgende voorbeeld als het proces goed is verlopen.
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying to Kubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io to access your application

Watching local files for changes...
```

Ongeacht de naam van de grafiek is, kunt u nu naar `curl http://gangly-bronco.squillace.io` gaan voor het ontvangen van het antwoord `Hello World!`.

## <a name="next-steps"></a>Volgende stappen

Nu u beschikt over een ACS Kubernetes-cluster, kunt u onderzoek uitvoeren met [Azure Container Registry](../../container-registry/container-registry-intro.md) om meer en andere implementaties te maken voor dit scenario. U kunt bijvoorbeeld de domein-DNS-recordset draft._basedomain.toplevel_ maken om processen van specifieke ACS-implementaties vanuit een dieper gelegen subdomein te controleren.







