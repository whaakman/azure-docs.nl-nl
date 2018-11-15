---
title: Over het installeren en uitvoeren van containers
titlesuffix: Face - Cognitive Services - Azure
description: Het downloaden, installeren en uitvoeren van containers voor gezicht in deze zelfstudie met stapsgewijze instructies.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 27a4bccfbac73c7c8c902a59fdd4cafe0c420c31
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634994"
---
# <a name="install-and-run-containers"></a>Installeren en uitvoeren van containers

Containerstrategie is een benadering voor softwaredistributie waarin een toepassing of service wordt geleverd als een containerinstallatiekopie. De configuratie en afhankelijkheden voor de toepassing of service zijn opgenomen in de containerinstallatiekopie. De container-installatiekopie kan vervolgens worden geïmplementeerd op een containerhost met weinig of geen wijziging. Containers zijn geïsoleerd van elkaar worden verbonden en het onderliggende besturingssysteem, met een kleinere footprint dan een virtuele machine. Containers kunnen worden geïnstantieerd van containerinstallatiekopieën voor taken op korte termijn en verwijderd wanneer het niet meer nodig hebt.

Face biedt een gestandaardiseerde Linux-container voor Docker, met de naam gezicht, detecteert menselijke gezichten in afbeeldingen en-kenmerken, zoals gezichtsoriëntatiepunten (zoals hartstukken en ogen), geslacht, leeftijd en andere machine voorspeld gezichtskenmerken identificeert. Naast detectie controleren Face of twee gezichten in dezelfde afbeelding of andere installatiekopieën zijn hetzelfde met behulp van een betrouwbaarheidsscore of gezichten op een database om te zien als een gelijkende vergelijken of identieke face al bestaat. Deze kunt soortgelijke gezichten ook organiseren in groepen, met behulp van gedeelde visuele kenmerken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="preparation"></a>Voorbereiding

U moet voldoen aan de volgende vereisten voordat u met de Face-container:

**Docker-Engine**: U moet Docker Engine lokaal zijn geïnstalleerd. Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), en [Windows](https://docs.docker.com/docker-for-windows/). Op Windows, moet Docker worden geconfigureerd ter ondersteuning van Linux-containers. Docker-containers kunnen ook rechtstreeks naar worden geïmplementeerd [Azure Kubernetes Service](/azure/aks/), [Azure Container Instances](/azure/container-instances/), of naar een [Kubernetes](https://kubernetes.io/) cluster geïmplementeerd op [Azure Stack](/azure/azure-stack/). Zie voor meer informatie over het implementeren van Kubernetes op Azure Stack [Kubernetes met Azure Stack implementeren](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure.

**Vertrouwd zijn met Microsoft Container Registry en Docker**: U moet een basiskennis hebben van zowel Microsoft Container Registry en Docker-concepten, zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van Basic `docker` opdrachten.  

Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).

### <a name="server-requirements-and-recommendations"></a>Vereisten en aanbevelingen

De Face-container vereist een minimum van 1 CPU-kern, ten minste 2,6 GHz (gigahertz) of sneller en 4 GB (Gigabyte) geheugen toegewezen, maar we raden ten minste 2 CPU-kernen en 6 GB aan toegewezen geheugen.

## <a name="request-access-to-the-private-container-registry"></a>Aanvraag voor toegang tot de privécontainerregister

U moet eerst invullen en verzenden de [Cognitive Services beeld Containers aanvraagformulier](https://aka.ms/VisionContainersPreview) toegang vragen tot de Face-container. Het formulier vraagt om informatie over u, uw bedrijf en het scenario voor gebruikers waarvoor u de container gebruikt. Wanneer verzonden, controleert het team van Azure Cognitive Services het formulier om ervoor te zorgen dat u voldoet aan de criteria voor toegang tot de persoonlijke container registry.

> [!IMPORTANT]
> U moet een e-mailadres dat is gekoppeld aan een Microsoft-Account (MSA) of Azure Active Directory (Azure AD)-account in het formulier.

Als uw aanvraag is goedgekeurd, ontvangt u vervolgens een e-mailbericht met instructies voor het verkrijgen van uw referenties en toegang tot het privé-container-register.

## <a name="create-a-face-resource-on-azure"></a>De resource van een gezicht in Azure maken

Als u wilt de Face-container gebruiken, moet u een resource Face maken op Azure. Nadat u de resource gemaakt, u vervolgens de URL van abonnement sleutel en het eindpunt van de resource gebruiken voor het instantiëren van de container. Zie voor meer informatie over het instantiëren van een container, [exemplaar maken van een container van een gedownloade containerinstallatiekopie](#instantiate-a-container-from-a-downloaded-container-image).

Voer de volgende stappen uit om te maken en gegevens ophalen uit een bron voor Face:

1. Maak een Face-resource in Azure portal.  
   Als u de Face-container gebruiken wilt, moet u eerst een bijbehorende Face-resource maken in Azure portal. Zie voor meer informatie, [Snelstartgids: een Cognitive Services-account maken in Azure portal](../cognitive-services-apis-create-account.md).

   > [!IMPORTANT]
   > De Face-bron moet de prijscategorie F0 gebruiken.

1. De eindpunt-URL en abonnement-sleutel voor de Azure-resource ophalen.  
   Nadat de Azure-resource is gemaakt, moet u de eindpunt-URL en abonnement sleutel van die resource voor het starten van de bijbehorende Face-container. U kunt de eindpunt-URL en abonnement sleutel kopiëren uit respectievelijk de Quick Start- en sleutels's van de Face-resource in Azure portal.

## <a name="log-in-to-the-private-container-registry"></a>Meld u aan bij de privé containerregister

Er zijn verschillende manieren om te verifiëren met de privécontainerregister voor Cognitive Services-Containers, maar wordt aanbevolen om vanaf de opdrachtregel met behulp van de [Docker-Opdrachtregelinterface](https://docs.docker.com/engine/reference/commandline/cli/).

Gebruik de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdracht, zoals wordt weergegeven in het volgende voorbeeld wordt aan te melden bij `containerpreview.azurecr.io`, de privécontainerregister voor Cognitive Services-Containers. Vervang *\<gebruikersnaam\>* met de naam van de gebruiker en *\<wachtwoord\>* met het wachtwoord dat is opgegeven in de referenties die u hebt ontvangen van de Azure Cognitive Services-team.

```docker
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Als u uw referenties in een tekstbestand hebt beveiligd, kunt u de inhoud van deze tekst samenvoegen-bestand, met behulp van de `cat` opdracht naar de `docker login` opdracht zoals wordt weergegeven in het volgende voorbeeld. Vervang *\<passwordFile\>* met het pad en de naam van het tekstbestand met het wachtwoord en *\<gebruikersnaam\>* met de naam van de gebruiker opgegeven in uw referenties.

```docker
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

## <a name="download-container-images-from-the-private-container-registry"></a>Containerinstallatiekopieën van de privécontainerregister downloaden

De installatiekopie van de container voor de Face-container is beschikbaar via een persoonlijk Docker-containerregister, met de naam `containerpreview.azurecr.io`, in Azure Container Registry. De installatiekopie van de container voor de Face-container moet worden gedownload vanuit de opslagplaats naar de container lokaal uitvoeren.

Gebruik de [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) opdracht voor het downloaden van een containerinstallatiekopie uit de opslagplaats. Bijvoorbeeld, voor het downloaden van de meest recente Face container-installatiekopie uit de opslagplaats, gebruik de volgende opdracht:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

Zie voor een volledige beschrijving van de beschikbare labels voor de Face-container, [tekst herkennen](https://go.microsoft.com/fwlink/?linkid=2018655) op Docker Hub.

> [!TIP]
> U kunt de [docker-installatiekopieën](https://docs.docker.com/engine/reference/commandline/images/) opdracht om een lijst van uw gedownloade containerinstallatiekopieën. De volgende opdracht worden bijvoorbeeld de ID, de opslagplaats en het label van elke gedownloade containerinstallatiekopie, opgemaakt als een tabel:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Een container van een gedownloade containerinstallatiekopie maken

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht voor het starten van een container van een gedownloade containerinstallatiekopie. Bijvoorbeeld, de volgende opdracht:

* Start een container van de installatiekopie van de Face-container
* Twee CPU-kernen en 6 GB (Gigabyte) aan geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* De container verwijderd automatisch nadat deze is afgesloten

```Docker
docker run --rm -it -p 5000:5000 --memory 6g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789
```

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor de instantie van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing).

Eenmaal gemaakt, kunt u bewerkingen aanroepen van de container met behulp van de container host URI. De volgende host URI vertegenwoordigt bijvoorbeeld de Face-container die is gemaakt in het vorige voorbeeld:

```http
http://localhost:5000/
```

> [!TIP]
> U hebt toegang tot de [OpenAPI-specificatie](https://swagger.io/docs/specification/about/) (voorheen de Swagger-specificatie), met een beschrijving van de bewerkingen die worden ondersteund door een geïnstantieerde container van de `/swagger` relatieve URI voor deze container. De volgende URI geeft bijvoorbeeld de toegang tot de OpenAPI-specificatie voor de Face-container die is gemaakt in het vorige voorbeeld:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Kunt u ofwel [aanroepen van de REST-API-bewerkingen](https://docs.microsoft.com/azure/cognitive-services/face/face-api-how-to-topics/howtodetectfacesinimage) beschikbaar is via uw container of gebruik de [clientbibliotheek van Azure Cognitive Services Face](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/) -clientbibliotheek om aan te roepen van deze bewerkingen.  
> [!IMPORTANT]
> Als u wilt de clientbibliotheek met uw container gebruiken, moet u Azure Cognitive Services Face-clientbibliotheek versie 2.0 of hoger hebben.

Het enige verschil tussen een bepaalde bewerking aanroepen vanuit uw container en dat dezelfde bewerking van een bijbehorende services op Azure is die u de URI van de container-host, in plaats van de URI van een Azure-regio van de host gebruiken wilt voor het aanroepen van de bewerking aan te roepen. Als u een Face-exemplaar wordt uitgevoerd in de regio West ons Azure gebruiken voor het detecteren van gezichten wilt, zou u bijvoorbeeld de volgende REST-API-bewerking aanroepen:

```http
POST https://westus.api.cognitive.microsoft.com/face/v1.0/detect
```

Als u een Face-container die wordt uitgevoerd op uw lokale computer onder de standaardconfiguratie gebruiken voor het detecteren van gezichten wilt, zou u de volgende REST-API-bewerking aanroepen:

```http
POST http://localhost:5000/face/v1.0/detect
```

### <a name="billing"></a>Billing

De Face-container verzendt factureringsgegevens naar Azure, met een bijbehorende Face-resource in uw Azure-account. De volgende opdrachtregelopties worden gebruikt door de Face-container voor factureringsdoeleinden bepalen:

| Optie | Beschrijving |
|--------|-------------|
| `ApiKey` | De API-sleutel van de Face-resource die is gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op een API-sleutel voor de ingerichte Face Azure resource die is opgegeven `Billing`. |
| `Billing` | Het eindpunt van de Face-resource die is gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op de URI van een ingerichte Face Azure-resource van het eindpunt.|
| `Eula` | Geeft aan dat u de licentie voor de container hebt geaccepteerd.<br/>De waarde van deze optie moet worden ingesteld op `accept`. |

> [!IMPORTANT]
> Alle drie de opties met geldige waarden moeten worden opgegeven of de container start niet.

Zie voor meer informatie over deze opties [containers configureren](face-resource-container-config.md).

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en Face containers uitvoeren. Samenvatting:

* Face biedt een Linux-container voor Docker, gezicht, gezichten detecteren of identificeren met behulp van een database mensen gezichten met de naam.
* Containerinstallatiekopieën worden gedownload van een privécontainerregister in Azure.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in Face containers door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.
* ** Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens (zoals de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft.  

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](face-resource-container-config.md) voor configuratie-instellingen
* Beoordeling [Face overzicht](Overview.md) voor meer informatie over het opsporen en identificeren van gezichten  
* Raadpleeg de [Face-API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) voor meer informatie over de methoden die wordt ondersteund door de container.
* Raadpleeg [Veelgestelde vragen (FAQ)](FAQ.md) het oplossen van problemen met betrekking tot Face-functionaliteit.
