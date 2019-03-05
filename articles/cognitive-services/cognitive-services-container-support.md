---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
description: Meer informatie over hoe Docker-containers dichter bij Cognitive Services toegang krijgen tot uw gegevens.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: a60013bce63ed234e15dfddb13c07fbdc33a4073
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57339620"
---
# <a name="container-support-in-azure-cognitive-services"></a>Ondersteuning voor containers in Azure Cognitive Services

Ondersteuning voor containers in Azure Cognitive Services kunnen ontwikkelaars gebruikmaken van de dezelfde uitgebreide API's die beschikbaar in Azure zijn en flexibiliteit kunt waar u wilt implementeren en te hosten van de services die worden geleverd met [Docker-containers](https://www.docker.com/what-container). Ondersteuning voor containers is momenteel beschikbaar in Preview-versie voor een subset van Azure Cognitive Services, met inbegrip van onderdelen van [Computer Vision](Computer-vision/Home.md), [Face](Face/Overview.md), [Tekstanalyse](text-analytics/overview.md), en [ Language Understanding](LUIS/luis-container-howto.md) (LUIS).

Containerstrategie is een benadering voor softwaredistributie waarin een toepassing of service, inclusief de afhankelijkheden en de configuratie wordt geleverd samen als een containerinstallatiekopie. Met weinig of geen wijziging, kan de installatiekopie van een container worden geïmplementeerd op een containerhost. Containers zijn geïsoleerd van elkaar worden verbonden en het onderliggende besturingssysteem, met een kleinere footprint dan een virtuele machine. Containers kunnen worden geïnstantieerd van containerinstallatiekopieën voor taken op korte termijn en verwijderd wanneer het niet meer nodig hebt.

De volgende video ziet u met behulp van een Cognitive Services-container.

[![Demonstratie van de container voor Cognitive Services](./media/index/containers-video-image.png)](https://azure.microsoft.com/resources/videos/containers-support-of-cognitive-services)

De [Computer Vision](Computer-vision/Home.md), [Face](Face/Overview.md), [Tekstanalyse](text-analytics/overview.md), en [Language Understanding (LUIS)](LUIS/what-is-luis.md) services zijn beschikbaar op [Microsoft Azure](https://azure.microsoft.com). Meld u aan bij de [Azure-portal](https://portal.azure.com/) maken en verken Azure-resources voor deze services.

## <a name="features-and-benefits"></a>Functies en voordelen

- **Controle over gegevens**: Kunnen klanten om te kiezen waarin deze Cognitive Services hun gegevens verwerken. Dit is essentieel voor klanten die gegevens verzenden naar de cloud, kunnen niet, maar toegang nodig tot Cognitive Services-technologie. Ondersteuning voor consistentie in hybride omgevingen: alle gegevens, beheer, identiteit en beveiliging.
- **Controle over gegevensmodellen kunnen bijwerken**: Bieden klanten flexibiliteit in versiebeheer en bijwerken van modellen die zijn geïmplementeerd in hun oplossingen.
- **Draagbare architectuur**: Het maken van een draagbare toepassingsarchitectuur die kan worden geïmplementeerd op Azure, on-premises en edge inschakelen. Containers rechtstreeks naar kunnen worden geïmplementeerd [Azure Kubernetes Service](../aks/index.yml), [Azure Container Instances](../container-instances/index.yml), of naar een [Kubernetes](https://kubernetes.io/) cluster geïmplementeerd op [Azure Stack](../azure-stack/index.yml). Zie voor meer informatie, [Kubernetes met Azure Stack implementeren](../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).
- **Hoge doorvoer / lage latentie**: Geef klanten de mogelijkheid om te schalen voor hoge doorvoer en lage latentie is vereist door het inschakelen van Cognitive Services om uit te voeren fysiek dicht bij hun logica van toepassingen en gegevens. Containers transacties per seconde (TPS) kunnen niet worden gegevenslimiet en kunnen worden gemaakt voor het schalen van zowel en uitbreiden voor het afhandelen van aanvraag als u de benodigde hardwarebronnen opgeven. 


## <a name="containers-in-azure-cognitive-services"></a>Containers in Azure Cognitive Services

Azure Cognitive Services-containers bieden de volgende set Docker-containers, die elk een subset van de functionaliteit van services in Azure Cognitive Services bevat:

| Service | Container| Description |
|---------|----------|-------------|
|[Computer Vision](Computer-vision/computer-vision-how-to-install-containers.md) |**Tekst herkennen** |Extraheert gedrukte tekst uit afbeeldingen van verschillende objecten met verschillende oppervlakken en achtergronden, zoals ontvangsten, posters en visitekaartjes.<br/><br/>**Belangrijk:** De tekst herkennen-container wordt op dit moment werkt alleen met Engels.<br>[Toegang aanvragen](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Face](Face/face-how-to-install-containers.md) |**Face** |Detecteert menselijke gezichten in afbeeldingen en -kenmerken, zoals gezichtsoriëntatiepunten (zoals hartstukken en ogen), geslacht, leeftijd en andere machine voorspeld gezichtskenmerken identificeert. Naast detectie controleren Face of twee gezichten in dezelfde afbeelding of andere installatiekopieën zijn hetzelfde met behulp van een betrouwbaarheidsscore of gezichten op een database om te zien als een gelijkende vergelijken of identieke face al bestaat. Deze kunt soortgelijke gezichten ook organiseren in groepen, met behulp van gedeelde visuele kenmerken.<br>[Toegang aanvragen](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[LUIS](LUIS/luis-container-howto.md) |**LUIS** ([installatiekopie](https://go.microsoft.com/fwlink/?linkid=2043204))|Een ervaren of gepubliceerd Language Understanding-model, ook wel bekend als een LUIS-app, worden in een docker-container en biedt toegang tot de voorspellingen van de query van de API-eindpunten van de container. U kunt het verzamelen van Logboeken voor query's uit de container en upload deze terug naar de [LUIS portal](https://www.luis.ai) voor het verbeteren van de nauwkeurigheid van de app.|
|[Tekstanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md) |**Sleutel vindt er sleuteltermextractie plaats** ([installatiekopie](https://go.microsoft.com/fwlink/?linkid=2018757)) |Extraheert sleuteltermen voor het identificeren van de belangrijkste punten. Bijvoorbeeld, voor de invoertekst 'het eten was heerlijk en de bediening fantastisch' retourneert de API de belangrijkste gespreksonderwerpen: 'eten' en 'bediening fantastisch'. |
|[Tekstanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|**Taaldetectie** ([installatiekopie](https://go.microsoft.com/fwlink/?linkid=2018759)) |Detecteert welke taal de ingevoerde tekst is geschreven en het rapport een code één taal voor elk document dat op de aanvraag ingediend voor maximaal 120 talen. De taalcode is gekoppeld aan een score die de sterkte van de score aangeeft. |
|[Tekstanalyse](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|**Sentimentanalyse** ([installatiekopie](https://go.microsoft.com/fwlink/?linkid=2018654)) |Onbewerkte tekst of er aanwijzingen over positief of negatief gevoel analyseert. Deze API retourneert een gevoelsscore tussen 0 en 1 voor elk document, waarbij 1 het meest positief is. De analyse-modellen zijn vooraf getrainde met behulp van een uitgebreide hoofdtekst van de tekst en natuurlijke taal technologieën van Microsoft. Voor [geselecteerde talen](./text-analytics/language-support.md) kan de API elke onbewerkte tekst die u opgeeft analyseren en beoordelen en de resultaten direct doorgeven aan de aanroepende toepassing. |

## <a name="container-availability-in-azure-cognitive-services"></a>Beschikbaarheid van de container in Azure Cognitive Services

Azure Cognitive Services-containers zijn openbaar beschikbaar zijn via uw Azure-abonnement en Docker-containerinstallatiekopieën kunnen worden opgehaald uit het Microsoft Container Registry of Docker Hub. U kunt de [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) opdracht voor het downloaden van de installatiekopie van een container van de juiste registersleutel.

> [!IMPORTANT]
> Op dit moment moet u een aanmeldingsproces voor toegang tot hebt voltooid de [Face](Face/face-how-to-install-containers.md) en [tekst herkennen](Computer-vision/computer-vision-how-to-install-containers.md) containers, waarin u gegevens kunt invullen en verzenden van een vragenlijst met vragen over u, uw bedrijf en de use-case waarvoor u wilt voor het implementeren van de containers. Wanneer u toegang verleend en referenties opgegeven, kunt u vervolgens de installatiekopieën van containers voor de containers gezichts- en tekst herkennen ophaalt uit een privécontainerregister wordt gehost door Azure Container Registry.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u met behulp van Azure Cognitive Services-containers:

**Docker-Engine**: U moet Docker Engine lokaal zijn geïnstalleerd hebben. Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), en [Windows](https://docs.docker.com/docker-for-windows/). Op Windows, moet Docker worden geconfigureerd ter ondersteuning van Linux-containers. Docker-containers kunnen ook rechtstreeks naar worden geïmplementeerd [Azure Kubernetes Service](../aks/index.yml) of [Azure Container Instances](../container-instances/index.yml).

Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure.

**Vertrouwd zijn met Microsoft-Container Registry en Docker**: U moet een basiskennis hebben van zowel Microsoft Container Registry en Docker-concepten, zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van basic `docker` opdrachten.

Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).

Afzonderlijke containers kunnen hun eigen vereisten, evenals, met inbegrip van de server en de toewijzing van geheugenvereisten hebben.

## <a name="developer-samples"></a>Voorbeelden voor ontwikkelaars

Voorbeelden voor ontwikkelaars zijn beschikbaar op onze [GitHub-opslagplaats](https://github.com/Azure-Samples/cognitive-services-containers-samples).

## <a name="next-steps"></a>Volgende stappen

Installeren en de functionaliteit van containers in Azure Cognitive Services verkennen:

* [Installeren en Computer Vision-containers gebruiken](Computer-vision/computer-vision-how-to-install-containers.md)
* [Installeren en gebruiken van de Face-containers](Face/face-how-to-install-containers.md)
* [Installeren en gebruiken van de Text Analytics-containers](text-analytics/how-tos/text-analytics-how-to-install-containers.md)
* [Installeren en gebruiken van containers met Language Understanding (LUIS)](LUIS/luis-container-howto.md)
