---
title: Installeren en uitvoeren van containers - Personalizer
titleSuffix: Azure Cognitive Services
description: Het downloaden, installeren en uitvoeren van containers voor Personalizer.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 2a62fd288f9118882e5cd9899ab572d4b247fc5a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65158537"
---
# <a name="install-and-run-personalizer-containers"></a>Installeren en Personalizer containers uitvoeren

De Personalizer-Service heeft de volgende containers: 

|Function|Functies|
|-|-|
|personalizer|Aanbevolen actie uit de huidige context van inhoud en de gebruiker bepalen.|

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u met behulp van containers Personalizer Service:

|Vereist|Doel|
|--|--|
|Docker-Engine| U moet de Docker-Engine zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> **Op Windows**, Docker moet ook worden geconfigureerd ter ondersteuning van Linux-containers.<br><br>|
|Vertrouwd zijn met Docker | U hebt een basiskennis hebt van Docker-kernconcepten zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van basic `docker` opdrachten.| 
|Personalizer Serviceresource |Als u wilt deze containers gebruiken, moet u het volgende hebben:<br><br>Een _Personalizer Service_ Azure-resource om de bijbehorende facturering sleutel en facturering URI van het eindpunt te verkrijgen. Beide waarden zijn beschikbaar op Personalizer Serviceoverzicht en sleutels van de Azure portal-pagina's en zijn vereist om te starten van de container.<br><br>**{BILLING_KEY}** : bronsleutel<br><br>**{BILLING_ENDPOINT_URI}** : voorbeeld van de eindpunt-URI is: `https://westus.api.cognitive.microsoft.com/`|

### <a name="the-host-computer"></a>De hostcomputer

De **host** is de computer met de docker-container. Het kan zijn dat een computer op uw locatie of een docker die als host fungeert de service in Azure, waaronder:

* [Azure Kubernetes Service](https://docs.microsoft.com/aks/index.yml)
* [Azure Container Instances](https://docs.microsoft.com/container-instances/index.yml)
* [Kubernetes](https://kubernetes.io/) cluster geïmplementeerd op [Azure Stack](https://docs.microsoft.com/azure-stack/index.yml). Zie voor meer informatie, [Kubernetes met Azure Stack implementeren](https://docs.microsoft.com/azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).

### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De volgende tabel beschrijft de minimale en aanbevolen CPU-kernen en geheugen toewijzen voor elke container Personalizer-Service.

| Container | Minimum | Aanbevolen |
|-----------|---------|-------------|
|personalizer | 1 core, 4 GB geheugen | 2 kernen, 8 GB geheugen |

Elke core moet ten minste 2,6 GHz (gigahertz) of sneller.

Kernen en geheugen komen overeen met de `--cpus` en `--memory` instellingen die worden gebruikt als onderdeel van de `docker run` opdracht.

De container moet geen verbinding maken met Azure Event hub in volgorde naar de relay-informatie in de positie en beloon aanroepen naar de server Personalizer in Azure, en moet geen verbinding maken met de API Personalizer om de vereiste configuratie en de meest recente machine laden Learning-modellen.

## <a name="get-the-container-image-with-docker-pull"></a>Met de installatiekopie van de container ophalen `docker pull`

Containerinstallatiekopieën voor Personalizer Service zijn beschikbaar. 

| Container | Opslagplaats |
|-----------|------------|
| personalizer | `mcr.microsoft.com/azure-cognitive-services/personalizer:latest` |

> [!TIP]
> U kunt de [docker-installatiekopieën](https://docs.docker.com/engine/reference/commandline/images/) opdracht om een lijst van uw gedownloade containerinstallatiekopieën. De volgende opdracht worden bijvoorbeeld de ID, de opslagplaats en het label van elke gedownloade containerinstallatiekopie, opgemaakt als een tabel:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>
>  IMAGE ID            REPOSITORY                                                                TAG
>  ebbee78a6baa        mcr.microsoft.com/azure-cognitive-services/personalizer                 latest
>  ``` 

### <a name="docker-pull-for-the-personalizer-service-container"></a>Docker pull voor de container Personalizer Service

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/personalizer:latest
```

## <a name="how-the-container-works"></a>De werking van de container

De container Personalizer vertegenwoordigt het clientgedeelte van de service Personalizer. Wanneer de container wordt uitgevoerd, krijgt de modellen en configuratie-instellingen van de service Personalizer in de Azure-cloud. De containerservice maakt gebruik van deze informatie om te reageren op aanvragen voor **positie** en **beloning**. De container verzendt ook deze aanvragen naar de resource Personalizer in de Azure-cloud. Deze informatie wordt vervolgens gebruikt om uw Personalizer lus-model op basis van de huidige instelling voor de updatefrequentie model te trainen. Het bijgewerkte model wordt verzonden naar de container. 

![On-premises lokale architectuur van de container Personalizer client](./media/personalizer-container-howto/personalizer-container-architecture.png)

## <a name="how-to-use-the-container"></a>Het gebruik van de container

Als de container op de [hostcomputer](#the-host-computer), de volgende procedure gebruiken om te werken met de container.

1. [Uitvoeren van de container](#run-the-container-with-docker-run), facturering met de vereiste instellingen. Meer [voorbeelden](personalizer-container-configuration.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 
1. [Query van de container voorspelling eindpunt](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>De container met uitvoeren `docker run`

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht uit te voeren op een van de drie containers. De opdracht maakt gebruik van de volgende parameters:

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | Deze sleutel wordt gebruikt voor het starten van de container en is beschikbaar op de Azure-portal op de pagina Personalizer Servicesleutels.  |
|{BILLING_ENDPOINT_URI} | Het eindpunt van de facturering URI-waarde is beschikbaar op de Azure-portal Personalizer overzichtspagina op de Service.|

Deze parameters vervangen door uw eigen waarden in het volgende voorbeeld `docker run` opdracht.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/personalizer\
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Met deze opdracht:

* Een container Personalizer Service kan worden uitgevoerd van de container-installatiekopie
* Één CPU-kern en 4 GB (Gigabyte) aan geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* De container worden automatisch verwijderd nadat deze is afgesloten. De containerinstallatiekopie is nog steeds beschikbaar op de hostcomputer. 

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing).

### <a name="run-multiple-containers-on-the-same-host"></a>Meerdere containers op dezelfde host worden uitgevoerd

Als u van plan bent om uit te voeren van meerdere containers met blootgestelde poorten, zorg ervoor dat elke container uitvoeren met een andere poort. Bijvoorbeeld: de eerste container op poort 5000- en de tweede container uitvoeren op poort 5001.

Vervang de `<container-registry>` en `<container-name>` met de waarden van de containers die u gebruikt. Deze hoeven niet te zijn van dezelfde container. U kunt de container Personalizer en de LUIS-container die samen worden uitgevoerd op de HOST hebben of u kunt meerdere Personalizer containers die worden uitgevoerd. 

De eerste container worden uitgevoerd op poort 5000. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

De tweede container op poort 5001 worden uitgevoerd.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Elke volgende container moet zich op een andere poort. 

## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het eindpunt voorspelling van de container

De container biedt eindpunt van de voorspelling query op basis van REST API's. 

Gebruikmaken van de host https://localhost:5000, voor de container met API's.

## <a name="stop-the-container"></a>De container stoppen

Als u wilt afsluiten van de container, in de opdrachtregelomgeving waarop de container wordt uitgevoerd, drukt u op **Ctrl + C**.

## <a name="troubleshoot"></a>Problemen oplossen

Als u de container wordt uitgevoerd met een uitvoer [koppelen](personalizer-container-configuration.md#mount-settings) en logboekregistratie is ingeschakeld, wordt de container genereert logboekbestanden die tot het oplossen van problemen die optreden tijdens het starten of uitvoeren van de container. 

## <a name="container-api-documentation"></a>Container API-documentatie

De container biedt een volledige set met documentatie voor de eindpunten, evenals een `Try it now` functie. Deze functie kunt u uw instellingen invoeren in een web gebaseerde HTML-formulier en de query zonder code te schrijven. Nadat de query retourneert, een voorbeeld van de CURL-opdracht om te laten zien hoe de HTTP-headers en hoofdtekst van de vereiste indeling is opgegeven. 

> [!TIP]
> Lees de [OpenAPI-specificatie](https://swagger.io/docs/specification/about/), met een beschrijving van de API-bewerkingen ondersteund door de container van de `/swagger` relatieve URI. Bijvoorbeeld:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

## <a name="billing"></a>Billing

Het verzenden van de containers Personalizer Service factuurgegevens naar Azure, met behulp van een _Personalizer Service_ resource voor uw Azure-account. 

Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens niet naar Microsoft. 

De `docker run` opdracht maakt gebruik van de volgende argumenten voor factureringsdoeleinden bepalen:

| Optie | Description |
|--------|-------------|
| `ApiKey` | De API-sleutel van de _Personalizer Service_ resource gebruikt voor het bijhouden van informatie over facturering. |
| `Billing` | Het eindpunt van de _Personalizer Service_ resource gebruikt voor het bijhouden van informatie over facturering.|
| `Eula` | Geeft aan dat u de licentie voor de container hebt geaccepteerd.<br/>De waarde van deze optie moet worden ingesteld op `accept`. |

> [!IMPORTANT]
> Alle drie de opties met geldige waarden moeten worden opgegeven of de container start niet.

Zie voor meer informatie over deze opties [containers configureren](personalizer-container-configuration.md).

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en Personalizer Service containers uitvoeren. Samenvatting:

* Personalizer Service biedt een Linux-containers voor Docker, encapsulating persoonlijke instellingen.
* Containerinstallatiekopieën worden gedownload uit het Microsoft Container Registry (MCR) in Azure.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in containers Personalizer Service door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.

> [!IMPORTANT]
> Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. De Personalizer containers wordt ook de gegevens van aanvragen verzenden naar de bijbehorende service in Azure voor de doeleinden van online training en krijgt bijgewerkt modellen periodiek van Azure.

## <a name="next-steps"></a>Volgende stappen

[Het configureren van Docker-container voor de `Docker Run` opdracht](personalizer-container-configuration.md)