---
title: Installeren en uitvoeren van containers
titleSuffix: Text Analytics - Cognitive Services - Azure
description: Het downloaden, installeren en uitvoeren van containers voor Tekstanalyse overschrijd in deze zelfstudie met stapsgewijze instructies.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 99bdb42d9a0d86d0d2acc4a6272e0c802042e6b5
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634997"
---
# <a name="install-and-run-containers"></a>Installeren en uitvoeren van containers

Containerstrategie is een benadering voor softwaredistributie waarin een toepassing of service wordt geleverd als een containerinstallatiekopie. De configuratie en afhankelijkheden voor de toepassing of service zijn opgenomen in de containerinstallatiekopie. De container-installatiekopie kan vervolgens worden geïmplementeerd op een containerhost met weinig of geen wijziging. Containers zijn geïsoleerd van elkaar worden verbonden en het onderliggende besturingssysteem, met een kleinere footprint dan een virtuele machine. Containers kunnen worden geïnstantieerd van containerinstallatiekopieën voor taken op korte termijn en verwijderd wanneer het niet meer nodig hebt.

Text Analytics biedt de volgende set Docker-containers, die elk een subset van de functionaliteit bevat:

| Container| Beschrijving |
|----------|-------------|
|Sleuteltermextractie | Extraheert sleuteltermen voor het identificeren van de belangrijkste punten. Bijvoorbeeld, voor de invoertekst 'het eten was heerlijk en de bediening fantastisch' retourneert de API de belangrijkste gespreksonderwerpen: 'eten' en 'bediening fantastisch'. |
|Taaldetectie | Voor maximaal 120 talen detecteert en rapporteert in welke taal de invoertekst wordt geschreven. De container rapporteert een enkel taalcode voor elk document dat opgenomen in de aanvraag. De taalcode is gekoppeld aan een score die de sterkte van de score aangeeft. |
|Sentimentanalyse | Onbewerkte tekst of er aanwijzingen over positief of negatief gevoel analyseert. Deze API retourneert een gevoelsscore tussen 0 en 1 voor elk document, waarbij 1 het meest positief is. De analyse-modellen zijn vooraf getrainde met behulp van een uitgebreide hoofdtekst van de tekst en natuurlijke taal technologieën van Microsoft. Voor [geselecteerde talen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages.md) kan de API elke onbewerkte tekst die u opgeeft analyseren en beoordelen en de resultaten direct doorgeven aan de aanroepende toepassing. |

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="preparation"></a>Voorbereiding

U moet voldoen aan de volgende vereisten voordat u met behulp van Text Analytics-containers:

**Docker-Engine**: U moet Docker Engine lokaal zijn geïnstalleerd. Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), en [Windows](https://docs.docker.com/docker-for-windows/). Op Windows, moet Docker worden geconfigureerd ter ondersteuning van Linux-containers. Docker-containers kunnen ook rechtstreeks naar worden geïmplementeerd [Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/), of naar een [Kubernetes](https://kubernetes.io/) cluster geïmplementeerd op [Azure Stack](/azure/azure-stack/). Zie voor meer informatie over het implementeren van Kubernetes op Azure Stack [Kubernetes met Azure Stack implementeren](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure.

**Vertrouwd zijn met Microsoft Container Registry en Docker**: U moet een basiskennis hebben van zowel Microsoft Container Registry en Docker-concepten, zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van Basic `docker` opdrachten.  

Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).

### <a name="server-requirements-and-recommendations"></a>Vereisten en aanbevelingen

De volgende tabel beschrijft de minimale en aanbevolen CPU-kernen, ten minste 2,6 GHz (gigahertz) of sneller, en het geheugen, in gigabytes (GB), om toe te wijzen voor elke container Text Analytics.

| Container | Minimum | Aanbevolen |
|-----------|---------|-------------|
|Sleuteltermextractie | 1 core, 2 GB geheugen | 1 core, 4 GB geheugen |
|Taaldetectie | 1 core, 2 GB geheugen | 1 core, 4 GB geheugen |
|Sentimentanalyse | 1 core, 8 GB geheugen | 1 core, 8 GB geheugen |

## <a name="download-container-images-from-microsoft-container-registry"></a>Containerinstallatiekopieën downloaden vanaf Microsoft Container Registry

Containerinstallatiekopieën voor Text Analytics zijn beschikbaar via Microsoft Container Registry. De volgende tabel bevat de opslagplaatsen vanuit Microsoft Container Registry met Text Analytics-containers. Elke opslagplaats bevat een containerinstallatiekopie, die moet worden gedownload naar de container lokaal uitvoeren.

| Container | Opslagplaats |
|-----------|------------|
|Sleuteltermextractie | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Taaldetectie | `mcr.microsoft.com/azure-cognitive-services/language` |
|Sentimentanalyse | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Gebruik de [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) opdracht voor het downloaden van een containerinstallatiekopie vanuit een opslagplaats. Bijvoorbeeld, voor het downloaden van de meest recente sleutel vindt er sleuteltermextractie plaats container-installatiekopie uit de opslagplaats, gebruik de volgende opdracht:

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

Zie voor een volledige beschrijving van de beschikbare labels voor de Text Analytics-containers, de volgende containers op de Docker-Hub:

* [Sleuteltermextractie](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Taaldetectie](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Sentimentanalyse](https://go.microsoft.com/fwlink/?linkid=2018654)

> [!TIP]
> U kunt de [docker-installatiekopieën](https://docs.docker.com/engine/reference/commandline/images/) opdracht om een lijst van uw gedownloade containerinstallatiekopieën. De volgende opdracht worden bijvoorbeeld de ID, de opslagplaats en het label van elke gedownloade containerinstallatiekopie, opgemaakt als een tabel:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Een container van een gedownloade containerinstallatiekopie maken

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht voor het starten van een container van een gedownloade containerinstallatiekopie. Bijvoorbeeld, de volgende opdracht:

* Start een container van de containerinstallatiekopie Sentimentanalyse
* Één CPU-kern en 8 GB (Gigabyte) aan geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* De container verwijderd automatisch nadat deze is afgesloten

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789

```

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opdrachtregelopties voor het starten van de container moeten worden opgegeven; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing).

Eenmaal gemaakt, kunt u bewerkingen aanroepen van de container met behulp van de container host URI. De volgende host URI vertegenwoordigt bijvoorbeeld de Sentimentanalyse-container die is gemaakt in het vorige voorbeeld:

```http
http://localhost:5000/
```

> [!TIP]
> U hebt toegang tot de [OpenAPI-specificatie](https://swagger.io/docs/specification/about/) (voorheen de Swagger-specificatie), met een beschrijving van de bewerkingen die worden ondersteund door een geïnstantieerde container van de `/swagger` relatieve URI voor deze container. De volgende URI geeft bijvoorbeeld de toegang tot de OpenAPI-specificatie voor de container Sentimentanalyse die is gemaakt in het vorige voorbeeld:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

U kunt [aanroepen van de REST-API-bewerkingen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-call-api) beschikbaar is via uw container of gebruik de [Azure Cognitive Services Text Analytics SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics) -clientbibliotheek om aan te roepen van deze bewerkingen.  
> [!IMPORTANT]
> Hebt u Azure Cognitive Services Text Analytics SDK versie 2.1.0 of hoger als u wilt de-clientbibliotheek gebruiken met de container.

Het enige verschil tussen een bepaalde bewerking aanroepen vanuit uw container en dat dezelfde bewerking van een bijbehorende services op Azure is die u de URI van de container-host, in plaats van de URI van een Azure-regio van de host gebruiken wilt voor het aanroepen van de bewerking aan te roepen. Als u een Text Analytics-exemplaar wordt uitgevoerd in de regio West ons Azure gebruiken wilt, zou u bijvoorbeeld de volgende REST-API-bewerking aanroepen:

```http
POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
```

Als u een sleutel vindt er sleuteltermextractie plaats-container die wordt uitgevoerd op uw lokale computer onder de standaardconfiguratie gebruikt wilt, zou u de volgende REST-API-bewerking aanroepen:

```http
POST http://localhost:5000/text/analytics/v2.0/keyPhrases
```

### <a name="billing"></a>Billing

De Text Analytics-containers facturering informatie verzenden naar Azure, met een bijbehorende Text Analytics-resource in uw Azure-account. De volgende opdrachtregelopties worden gebruikt door de Text Analytics-containers voor factureringsdoeleinden:

| Optie | Beschrijving |
|--------|-------------|
| `ApiKey` | De API-sleutel van de Text Analytics-resource die is gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op een API-sleutel voor de ingerichte Text Analytics Azure resource die is opgegeven `Billing`. |
| `Billing` | Het eindpunt van de Text Analytics-resource die is gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op de URI van een ingerichte Text Analytics-Azure-resource van het eindpunt.|
| `Eula` | Geeft aan dat u de licentie voor de container hebt geaccepteerd.<br/>De waarde van deze optie moet worden ingesteld op `accept`. |

> [!IMPORTANT]
> Alle drie de opties met geldige waarden moeten worden opgegeven of de container start niet.

Zie voor meer informatie over deze opties [containers configureren](../text-analytics-resource-container-config.md).

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en Text Analytics-containers uitvoeren. Samenvatting:

* Text Analytics biedt drie Linux-containers voor Docker, encapsulating sleuteltermextractie, taaldetectie en sentimentanalyse.
* Containerinstallatiekopieën worden gedownload van een privécontainerregister in Azure.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in Text Analytics-containers door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.
* ** Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens (zoals de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft.  

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](../text-analytics-resource-container-config.md) voor configuratie-instellingen
* Beoordeling [Text Analytics-overzicht](../overview.md) voor meer informatie over detectie van sleuteluitdrukkingen en taaldetectie sentimentanalyse  
* Raadpleeg de [Tekstanalyse-API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) voor meer informatie over de methoden die wordt ondersteund door de container.
* Raadpleeg [Veelgestelde vragen (FAQ)](../text-analytics-resource-faq.md) het oplossen van problemen met betrekking tot de Computer Vision-functionaliteit.