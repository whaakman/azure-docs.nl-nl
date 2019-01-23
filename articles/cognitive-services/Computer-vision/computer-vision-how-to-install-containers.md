---
title: Over het installeren en uitvoeren van containers - Computer Vision
titlesuffix: Azure Cognitive Services
description: Het downloaden, installeren en uitvoeren van containers voor Computer Vision in deze zelfstudie met stapsgewijze instructies.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 01/22/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: f344bb893a453a5f0b00f5cb1d87528b5943f779
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54462943"
---
# <a name="install-and-run-recognize-text-containers"></a>Installeren en te herkennen tekst containers uitvoeren

Containerstrategie is een benadering voor softwaredistributie waarin een toepassing of service wordt geleverd als een containerinstallatiekopie. De configuratie en afhankelijkheden voor de toepassing of service zijn opgenomen in de containerinstallatiekopie. De container-installatiekopie kan vervolgens worden geïmplementeerd op een containerhost met weinig of geen wijziging. Containers zijn geïsoleerd van elkaar worden verbonden en het onderliggende besturingssysteem, met een kleinere footprint dan een virtuele machine. Containers kunnen worden geïnstantieerd van containerinstallatiekopieën voor taken op korte termijn en verwijderd wanneer het niet meer nodig hebt.

Het gedeelte tekst herkennen van Computer Vision-is ook beschikbaar als een Docker-container. Hiermee kunt u om te detecteren en extraheer gedrukte tekst uit afbeeldingen van verschillende objecten met verschillende oppervlakken en achtergronden, zoals ontvangsten, posters en visitekaartjes.  
> [!IMPORTANT]
> De tekst herkennen-container wordt op dit moment werkt alleen met Engels.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="preparation"></a>Voorbereiding

U moet voldoen aan de volgende vereisten voordat u de container tekst herkennen:

**Docker-Engine**: U moet Docker Engine lokaal zijn geïnstalleerd hebben. Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), en [Windows](https://docs.docker.com/docker-for-windows/). Op Windows, moet Docker worden geconfigureerd ter ondersteuning van Linux-containers. Docker-containers kunnen ook rechtstreeks naar worden geïmplementeerd [Azure Kubernetes Service](../../aks/index.yml), [Azure Container Instances](../../container-instances/index.yml), of naar een [Kubernetes](https://kubernetes.io/) cluster geïmplementeerd op [Azure Stack](../../azure-stack/index.yml). Zie voor meer informatie over het implementeren van Kubernetes op Azure Stack [Kubernetes met Azure Stack implementeren](../../azure-stack/user/azure-stack-solution-template-kubernetes-deploy.md).

Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure.

**Vertrouwd zijn met Microsoft-Container Registry en Docker**: U moet een basiskennis hebben van zowel Microsoft Container Registry en Docker-concepten, zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van basic `docker` opdrachten.  

Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).

### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De container herkennen tekst vereist een minimum van 1 CPU-kern, ten minste 2,6 GHz (gigahertz) of sneller en 8 GB (Gigabyte) aan toegewezen geheugen, maar we raden ten minste 2 CPU-kernen en 8 GB aan toegewezen geheugen.

## <a name="request-access-to-the-private-container-registry"></a>Aanvraag voor toegang tot de privécontainerregister

U moet eerst invullen en verzenden de [Cognitive Services beeld Containers aanvraagformulier](https://aka.ms/VisionContainersPreview) toegang vragen tot de container tekst herkennen. Het formulier vraagt om informatie over u, uw bedrijf en het scenario voor gebruikers waarvoor u de container gebruikt. Wanneer verzonden, controleert het team van Azure Cognitive Services het formulier om ervoor te zorgen dat u voldoet aan de criteria voor toegang tot de persoonlijke container registry.

> [!IMPORTANT]
> U moet een e-mailadres dat is gekoppeld aan een Microsoft-Account (MSA) of Azure Active Directory (Azure AD)-account in het formulier.

Als uw aanvraag is goedgekeurd, ontvangt u vervolgens een e-mailbericht met instructies voor het verkrijgen van uw referenties en toegang tot het privé-container-register.

## <a name="create-a-computer-vision-resource-on-azure"></a>Een Computer Vision-resource in Azure maken

Als u wilt gebruiken van de container tekst herkennen, moet u een Computer Vision-resource maken in Azure. Nadat u de resource gemaakt, u vervolgens de URL van abonnement sleutel en het eindpunt van de resource gebruiken voor het instantiëren van de container. Zie voor meer informatie over het instantiëren van een container, [exemplaar maken van een container van een gedownloade containerinstallatiekopie](#instantiate-a-container-from-a-downloaded-container-image).

Voer de volgende stappen uit om te maken en gegevens ophalen uit een Azure-resource:

1. Maak een Azure-resource in Azure portal.  
   Als u gebruiken van de container tekst herkennen wilt, moet u eerst een bijbehorende Computer Vision-resource maken in Azure portal. Zie voor meer informatie [Snelstart: Een Cognitive Services-account maken in Azure portal](../cognitive-services-apis-create-account.md).

1. De eindpunt-URL en abonnement-sleutel voor de Azure-resource ophalen.  
   Nadat de Azure-resource is gemaakt, moet u de eindpunt-URL en abonnement sleutel van die resource voor het starten van de overeenkomstige tekst herkennen-container. U kunt de eindpunt-URL en abonnement sleutel kopiëren uit respectievelijk de Quick Start- en sleutels's van de Computer Vision-resource in Azure portal.

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

De installatiekopie van de container voor de container herkennen tekst is beschikbaar via een persoonlijk Docker-containerregister, met de naam `containerpreview.azurecr.io`, in Azure Container Registry. De installatiekopie van de container voor de container herkennen tekst moet worden gedownload vanuit de opslagplaats naar de container lokaal uitvoeren.

Gebruik de [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) opdracht voor het downloaden van een containerinstallatiekopie uit de opslagplaats. Bijvoorbeeld, voor het downloaden van de meest recente tekst herkennen container-installatiekopie uit de opslagplaats, gebruik de volgende opdracht:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

Zie voor een volledige beschrijving van de beschikbare labels voor de container tekst herkennen, [tekst herkennen](https://go.microsoft.com/fwlink/?linkid=2018655) op Docker Hub.

> [!TIP]
> U kunt de [docker-installatiekopieën](https://docs.docker.com/engine/reference/commandline/images/) opdracht om een lijst van uw gedownloade containerinstallatiekopieën. De volgende opdracht worden bijvoorbeeld de ID, de opslagplaats en het label van elke gedownloade containerinstallatiekopie, opgemaakt als een tabel:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Een container van een gedownloade containerinstallatiekopie maken

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht voor het starten van een container van een gedownloade containerinstallatiekopie. Bijvoorbeeld, de volgende opdracht:

* Start een container van de containerinstallatiekopie tekst herkennen
* Twee CPU-kernen en 8 GB (Gigabyte) aan geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* De container verwijderd automatisch nadat deze is afgesloten

```docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text Eula=accept Billing=https://westus.api.cognitive.microsoft.com/vision/v2.0 ApiKey=0123456789
```

Eenmaal gemaakt, kunt u bewerkingen aanroepen van de container met behulp van de container host URI. De volgende host URI vertegenwoordigt bijvoorbeeld de tekst herkennen-container die is gemaakt in het vorige voorbeeld:

```http
http://localhost:5000/
```

> [!IMPORTANT]
> U hebt toegang tot de [OpenAPI-specificatie](https://swagger.io/docs/specification/about/) (voorheen de Swagger-specificatie), met een beschrijving van de bewerkingen die worden ondersteund door een geïnstantieerde container van de `/swagger` relatieve URI voor deze container. De volgende URI geeft bijvoorbeeld de toegang tot de OpenAPI-specificatie voor de container tekst herkennen die is gemaakt in het vorige voorbeeld:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

U kunt [aanroepen van de REST-API-bewerkingen](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtocallvisionapi) beschikbaar in de container voor het synchroon of asynchroon herkennen tekst, of gebruik de [Azure Cognitive Services-Computer Vision-SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision) client -bibliotheek voor het aanroepen van deze bewerkingen.  
> [!IMPORTANT]
> Hebt u Azure Cognitive Services-Computer Vision SDK versie 3.2.0 of hoger als u wilt de-clientbibliotheek gebruiken met de container.

### <a name="asynchronous-text-recognition"></a>Asynchrone tekstherkenning

U kunt de `POST /vision/v2.0/recognizeText` en `GET /vision/v2.0/textOperations/*{id}*` bewerkingen in concert asynchroon herkennen gedrukte tekst in een afbeelding, vergelijkbaar met hoe de Computer Vision-service maakt gebruik van de bijbehorende REST-bewerkingen. De container tekst herkennen herkent alleen gedrukte tekst, niet handgeschreven tekst op dit moment, zodat de `mode` parameter normaal gesproken opgegeven voor de bewerking van de Computer Vision wordt genegeerd door de tekst herkennen-container.

### <a name="synchronous-text-recognition"></a>Synchrone tekstherkenning

U kunt de `POST /vision/v2.0/recognizeTextDirect` bewerking is synchroon gedrukte tekst in een afbeelding herkennen. Omdat deze bewerking synchroon is, de hoofdtekst van de aanvraag voor deze bewerking is hetzelfde als die voor de `POST /vision/v2.0/recognizeText` bewerking, maar het antwoord body voor deze bewerking hetzelfde als die die wordt geretourneerd is door de `GET /vision/v2.0/textOperations/*{id}*` bewerking.

### <a name="billing"></a>Billing

De container herkennen tekst verzendt factureringsgegevens naar Azure, met een bijbehorende Computer Vision-resource in uw Azure-account. De volgende opties worden gebruikt door de tekst herkennen-container voor factureringsdoeleinden:

| Optie | Description |
|--------|-------------|
| `ApiKey` | De API-sleutel van de Computer Vision-resource die is gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op een API-sleutel voor de ingerichte Computer Vision-Azure-resource in opgegeven `Billing`. |
| `Billing` | Het eindpunt van de Computer Vision-resource die is gebruikt voor het bijhouden van informatie over facturering.<br/>De waarde van deze optie moet worden ingesteld op de URI van een ingerichte Computer Vision-Azure-resource van het eindpunt.|
| `Eula` | Geeft aan dat u de licentie voor de container hebt geaccepteerd.<br/>De waarde van deze optie moet worden ingesteld op `accept`. |

> [!IMPORTANT]
> Alle drie de opties met geldige waarden moeten worden opgegeven of de container start niet.

Zie voor meer informatie over deze opties [containers configureren](computer-vision-resource-container-config.md).

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en Computer Vision-containers uitvoeren. Samenvatting:

* Computer Vision biedt een Linux-containers voor Docker, Detecteer en extraheer gedrukte tekst.
* Containerinstallatiekopieën worden gedownload van een privécontainerregister in Azure.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in Computer Vision-containers door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.

> [!IMPORTANT]
> Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens (zoals de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft.

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](computer-vision-resource-container-config.md) voor configuratie-instellingen
* Beoordeling [Computer Vision-overzicht](Home.md) voor meer informatie over het herkennen van afdrukken en handgeschreven tekst  
* Raadpleeg de [Computer Vision-API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) voor meer informatie over de methoden die wordt ondersteund door de container.
* Raadpleeg [Veelgestelde vragen (FAQ)](FAQ.md) het oplossen van problemen met betrekking tot de Computer Vision-functionaliteit.
* Meer [Cognitive Services-Containers](../cognitive-services-container-support.md)
