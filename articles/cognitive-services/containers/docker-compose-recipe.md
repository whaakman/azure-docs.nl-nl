---
title: Docker compose container recepten
titleSuffix: Azure Cognitive Services
description: Leer hoe u meerdere Cognitive Services-containers kunt implementeren. Deze procedure laat zien hoe u meerdere Docker-containerinstallatiekopieën met Docker Compose indeelt.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 8afb7e866bc2a5fefe28a71653c4a2a87fdc7a5b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445797"
---
# <a name="use-multiple-containers-in-a-private-network-with-docker-compose"></a>Meerdere containers in een particulier netwerk met Docker Compose gebruiken

Leer hoe u meerdere Cognitive Services-containers kunt implementeren. Deze procedure laat zien hoe u meerdere Docker-containerinstallatiekopieën met Docker Compose indeelt.

> [Docker Compose](https://docs.docker.com/compose/) is een hulpprogramma voor het definiëren en uitvoeren van toepassingen met meerdere containers Docker. Bij het opstellen gebruikt u een YAML-bestand voor het configureren van uw toepassing services. Vervolgens, met slechts één opdracht, u maakt en start alle services van uw configuratie.

Wanneer dat nodig is, is meerdere containerinstallatiekopieën op een enkele hostcomputer organiseren mag aantrekkelijke. In dit artikel Haal we de tekst herkennen service en formulier herkenning service samen.

## <a name="prerequisites"></a>Vereisten

Deze procedure moet diverse hulpprogramma's die moeten worden geïnstalleerd en lokaal uitvoeren.

* Gebruik een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* [Docker-engine](https://www.docker.com/products/docker-engine) en te valideren dat de Docker-CLI in een consolevenster weergegeven werkt.
* Een Azure-resource met de juiste prijscategorie. Niet alle Prijscategorieën werken met deze container:
  * **Computer Vision** resource met F0 of standaardprijzen alleen lagen.
  * **Herkenning formulier** resource met F0 of standaardprijzen alleen lagen.
  * **Cognitive Services** resource met de S0 prijscategorie.

## <a name="request-access-to-the-container-registry"></a>Aanvraag voor toegang tot de container registry

Invullen en verzenden de [Cognitive Services spraak Containers aanvraagformulier](https://aka.ms/speechcontainerspreview/) toegang vragen tot de container. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="docker-compose-file"></a>Docker compose-bestand

Het YAML-bestand definieert de services worden geïmplementeerd. Deze services zijn afhankelijk van hetzij een `DockerFile` of een bestaande containerinstallatiekopie, in dit geval gebruiken we twee preview-installatiekopieën. Kopieer en plak de volgende YAML-bestand en sla het bestand als *docker-compose.yaml*. Geef de juiste _apikey_, _facturering_, en _URI van het eindpunt_ waarden in de _docker-compose.yml_ bestand hieronder.

```yaml
version: '3.7'
services:
  forms:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    environment:
       eula: accept
       billing: # < Your form recognizer billing URL >
       apikey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionApiKey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionEndpointUri: # < Your form recognizer URI >
    volumes:
       - type: bind
         source: e:\publicpreview\output
         target: /output
       - type: bind
         source: e:\publicpreview\input
         target: /input
    ports:
      - "5010:5000"

  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    environment:
      eula: accept
      apikey: # < Your recognize text API key >
      billing: # < Your recognize text billing URL >
    ports:
      - "5021:5000"
```

> [!IMPORTANT]
> Maak de mappen op de hostmachine die zijn opgegeven onder de `volumes` knooppunt. Dit is vereist als de mappen moeten bestaan voordat u probeert te koppelen van een afbeelding met volume-bindingen.

## <a name="start-the-configured-docker-compose-services"></a>Start de geconfigureerde docker compose-services

Een docker-compose bestand maakt het beheer van alle gedefinieerde van de service-levenscyclus; weergeven van starten/stoppen en opnieuw opbouwen van services, servicestatus en logboekstreaming. Open een opdrachtregelinterface vanuit de projectmap (waar de *docker-compose.yaml* zich-bestand).

> [!NOTE]
> Zorg ervoor dat de hostcomputer schijven met correct deelt om fouten te voorkomen, de **Docker-Engine**. Bijvoorbeeld, als *e:\publicpreview* wordt gebruikt als een map in de *docker-compose.yaml* delen de *E-station* met docker.

Uitvoeren van de opdrachtregelinterface, de volgende opdracht uit om te starten (of opnieuw opstarten) op alle services die zijn gedefinieerd in de *docker-compose.yaml*:

```console
docker-compose up
```

De eerste keer wordt uitgevoerd de `docker-compose up` opdracht met deze configuratie **Docker** wordt opgehaald van de installatiekopieën die zijn geconfigureerd onder de `services` knooppunt--downloaden/koppelen ze:

```console
Pulling forms (containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:)...
latest: Pulling from microsoft/cognitive-services-form-recognizer
743f2d6c1f65: Pull complete
72befba99561: Pull complete
2a40b9192d02: Pull complete
c7715c9d5c33: Pull complete
f0b33959f1c4: Pull complete
b8ab86c6ab26: Pull complete
41940c21ed3c: Pull complete
e3d37dd258d4: Pull complete
cdb5eb761109: Pull complete
fd93b5f95865: Pull complete
ef41dcbc5857: Pull complete
4d05c86a4178: Pull complete
34e811d37201: Pull complete
Pulling ocr (containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:)...
latest: Pulling from microsoft/cognitive-services-recognize-text
f476d66f5408: Already exists
8882c27f669e: Already exists
d9af21273955: Already exists
f5029279ec12: Already exists
1a578849dcd1: Pull complete
45064b1ab0bf: Download complete
4bb846705268: Downloading [=========================================>         ]  187.1MB/222.8MB
c56511552241: Waiting
e91d2aa0f1ad: Downloading [==============================================>    ]  162.2MB/176.1MB
```

De installatiekopieën zijn gedownload, en vervolgens de installatiekopie-services zijn gestart.

```console
Starting docker_ocr_1   ... done
Starting docker_forms_1 ... doneAttaching to docker_ocr_1, docker_forms_1forms_1  | forms_1  | forms_1  | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
forms_1  | 
forms_1  | 
forms_1  | Using '/input' for reading models and other read-only data.
forms_1  | Using '/output/forms/812d811d1bcc' for writing logs and other output data.
forms_1  | Logging to console.
forms_1  | Submitting metering to 'https://westus2.api.cognitive.microsoft.com/'.
forms_1  | WARNING: No access control enabled!
forms_1  | warn: Microsoft.AspNetCore.Server.Kestrel[0]
forms_1  |       Overriding address(es) 'http://+:80'. Binding to endpoints defined in UseKestrel() instead.
forms_1  | Hosting environment: Production
forms_1  | Content root path: /app/forms
forms_1  | Now listening on: http://0.0.0.0:5000
forms_1  | Application started. Press Ctrl+C to shut down.
ocr_1    | 
ocr_1    | 
ocr_1    | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
ocr_1    |
ocr_1    | 
ocr_1    | Logging to console.
ocr_1    | Submitting metering to 'https://westcentralus.api.cognitive.microsoft.com/'.
ocr_1    | WARNING: No access control enabled!
ocr_1    | Hosting environment: Production
ocr_1    | Content root path: /
ocr_1    | Now listening on: http://0.0.0.0:5000
ocr_1    | Application started. Press Ctrl+C to shut down.
```

## <a name="verify-the-service-availability"></a>Controleer of de beschikbaarheid van services

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

Hieronder volgt een voorbeeld van uitvoer:

```
IMAGE ID            REPOSITORY                                                                 TAG
2ce533f88e80        containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer   latest
4be104c126c5        containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text    latest
```

### <a name="test-the-recognize-text-container"></a>Testen van de container van de tekst herkennen

Open een browser op de hostcomputer en gaat u naar `localhost` met de opgegeven poort van de *docker-compose.yaml*, bijvoorbeeld `http://localhost:5021/swagger/index.html`. U kunt de probeer deze functie van de API voor het testen van het eindpunt van de tekst herkennen.

![Tekst Swagger herkennen](media/recognize-text-swagger-page.png)

### <a name="test-the-form-recognizer-container"></a>Het formulier herkenning container testen

Open een browser op de hostcomputer en gaat u naar `localhost` met de opgegeven poort van de *docker-compose.yaml*, bijvoorbeeld `http://localhost:5010/swagger/index.html`. U kunt de probeer deze functie van de API voor het testen van het formulier herkenning-eindpunt.

![Formulier herkenning Swagger](media/form-recognizer-swagger-page.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Cognitive Services-Containers](../cognitive-services-container-support.md)