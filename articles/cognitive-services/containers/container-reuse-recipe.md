---
title: Recepten voor Docker-containers
titleSuffix: Azure Cognitive Services
description: Gebruik deze recepten container te maken van Cognitive Services-Containers die kunnen worden hergebruikt. Containers kunnen worden gebouwd met enkele of alle configuratie-instellingen, zodat ze niet nodig zijn wanneer de container wordt gestart. Wanneer u deze nieuwe laag van de container (met instellingen hebt) en u deze lokaal hebt getest, kunt u de container kunt opslaan in een container registry. Wanneer de container wordt gestart, moet deze alleen de instellingen die op dit moment niet worden opgeslagen in de container.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: 8a9d0d9f35c30636f4b9150c63b64098071e12e9
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67271487"
---
# <a name="create-containers-for-reuse"></a>Containers voor hergebruik maken

Gebruik deze recepten container te maken van Cognitive Services-Containers die kunnen worden hergebruikt. Containers kunnen worden gebouwd met sommige of alle configuratie-instellingen, zodat deze _niet_ die nodig zijn wanneer de container wordt gestart.

Wanneer u deze nieuwe laag van de container (met instellingen hebt) en u deze lokaal hebt getest, kunt u de container kunt opslaan in een container registry. Wanneer de container wordt gestart, moet deze alleen de instellingen die op dit moment niet worden opgeslagen in de container. De container privéregister hier configuratie voor u om deze instellingen in te geven.

## <a name="docker-run-syntax"></a>Docker syntaxis uitvoeren

Alle `docker run` voorbeelden in dit document wordt ervan uitgegaan dat een Windows-console met een `^` regel voortzetting teken. Overweeg het volgende voor eigen gebruik:

* Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met docker-containers.
* Als u een besturingssysteem dan Windows of een console dan Windows-console gebruikt, gebruikt u de juiste console/terminal, de syntaxis van de map voor koppelingen en regel voortzetting teken voor de console en het systeem.  Omdat de Cognitive Services-container een Linux-besturingssysteem is, gebruikt het koppelpunt van het doel een Linux-stijl map-syntaxis.
* `docker run` voorbeelden gebruiken de map uit de `c:` station om te voorkomen van machtigingsconflicten op Windows. Als u gebruiken van een specifieke map als de invoermap wilt, moet u mogelijk de docker verlenen machtiging-service.

## <a name="store-no-configuration-settings-in-image"></a>Er is geen configuratie-instellingen in afbeelding Store

Het voorbeeld `docker run` opdrachten voor elke service Sla de configuratie-instellingen niet in de container. Wanneer u de container met een service-console of het register begint, moeten deze configuratie-instellingen om door te geven. De container privéregister hier configuratie voor u om deze instellingen in te geven.

## <a name="reuse-recipe-store-all-configuration-settings-with-container"></a>Hergebruik van recept: alle configuratie-instellingen met container opslaan

Als u wilt alle configuratie-instellingen opslaan, maakt u een `Dockerfile` met deze instellingen.

Problemen met deze methode:

* De nieuwe container is een afzonderlijke naam en het label van de oorspronkelijke container.
* Als u wilt deze instellingen wijzigt, moet u de waarden van het bestand Dockerfile wijzigen, de installatiekopie herbouwen en opnieuw publiceren naar het register.
* Als iemand anders toegang tot uw container registry of uw lokale host, kunnen ze voert u de container en gebruikmaken van de Cognitive Services-eindpunten.
* Als uw Cognitive Service geen invoer koppelingen vereist, niet toevoegen de `COPY` regels die moeten worden uw docker-bestand.

Docker-bestand, vanuit de bestaande Cognitive Services-container die u gebruiken wilt, maakt vervolgens gebruik van docker-opdrachten in het Dockerfile in te stellen of het ophalen van gegevens in de container moet.

In dit voorbeeld:

* Hiermee stelt u het eindpunt van de facturering `{BILLING_ENDPOINT}` vanaf de host de sleutel van de omgeving met behulp van `ENV`.
* Stelt de facturering API-sleutel, `{ENDPOINT_KEY}` vanaf de host de sleutel van de omgeving met behulp van ' ENV.

### <a name="reuse-recipe-store-billing-settings-with-container"></a>Hergebruik van recept: sla de instellingen van de facturering met container

In dit voorbeeld laat zien hoe de Text Analytics sentiment container van een docker-bestand maken.

```Dockerfile
FROM mcr.microsoft.com/azure-cognitive-services/sentiment:latest
ENV billing={BILLING_ENDPOINT}
ENV apikey={ENDPOINT_KEY}
ENV EULA=accept
```

Bouwen en uitvoeren van de container [lokaal](#how-to-use-container-on-your-local-host) of van uw [privéregister container](#how-to-add-container-to-private-registry) indien nodig.

### <a name="reuse-recipe-store-billing-and-mount-settings-with-container"></a>Hergebruik van recept: opslaan van facturering en instellingen met de container voor koppelen

In dit voorbeeld laat zien hoe u Language Understanding, facturering en modellen uit het bestand Dockerfile opslaan.

* Het model Language Understanding (LUIS)-bestand uit van de host van het bestand gekopieerd met behulp van system `COPY`.
* De container LUIS ondersteunt meer dan één model. Als alle modellen worden opgeslagen in dezelfde map, moet u alle een `COPY` instructie.
* Voer het docker-bestand van het relatieve bovenliggende item van de invoermap model. Voer voor het volgende voorbeeld wordt de `docker build` en `docker run` opdrachten van het relatieve bovenliggende `/input`. De eerste `/input` op de `COPY` opdracht is van de hostcomputer directory. De tweede `/input` is de map van de container.

```Dockerfile
FROM <container-registry>/<cognitive-service-container-name>:<tag>
ENV billing={BILLING_ENDPOINT}
ENV apikey={ENDPOINT_KEY}
ENV EULA=accept
COPY /input /input
```

Bouwen en uitvoeren van de container [lokaal](#how-to-use-container-on-your-local-host) of van uw [privéregister container](#how-to-add-container-to-private-registry) indien nodig.

## <a name="how-to-use-container-on-your-local-host"></a>Het gebruik van de container op uw lokale host

Voor het bouwen van de Docker-bestand, Vervang `<your-image-name>` met de nieuwe naam van de afbeelding vervolgens gebruiken:

```console
docker build -t <your-image-name> .
```

Aan de installatiekopie uitvoert, en deze te verwijderen wanneer de container wordt gestopt (`--rm`):

```console
docker run --rm <your-image-name>
```

## <a name="how-to-add-container-to-private-registry"></a>Container toevoegen aan persoonlijke register

Volg deze stappen voor het gebruik van de docker-bestand en de nieuwe installatiekopie in uw persoonlijke containerregister plaatsen.  

1. Maak een `Dockerfile` met de tekst van recept voor hergebruik. Een `Dockerfile` beschikt niet over een uitbreiding.

1. Alle waarden in de punthaken vervangen door uw eigen waarden.

1. Bouw het bestand in een installatiekopie op de opdrachtregel of de terminal, met de volgende opdracht. Vervang de waarden in de punthaken `<>`, met uw eigen containernaam en het label.  

    De optie tag `-t`, is een manier om informatie over wat u hebt gewijzigd voor de container toevoegen. Bijvoorbeeld, een containernaam zijn van `modified-LUIS` geeft aan dat de oorspronkelijke container heeft is gelaagd. De naam van een tag van `with-billing-and-model` geeft aan hoe de container Language Understanding (LUIS) is gewijzigd.

    ```Bash
    docker build -t <your-new-container-name>:<your-new-tag-name> .
    ```

1. Meld u op een console bij Azure CLI. Met deze opdracht opent een browservenster en verificatie is vereist. Eenmaal is geverifieerd, kunt u de browser sluit en doorgaan met werken in de console.

    ```azure-cli
    az login
    ```

1. Meld u aan uw persoonlijke register met Azure CLI op een console.

    Vervang de waarden in de punthaken `<my-registry>`, met de registernaam van uw eigen.  

    ```azure-cli
    az acr login --name <my-registry>
    ```

    U kunt zich ook aanmelden met dockeraanmelding als u een service-principal zijn toegewezen.

    ```Bash
    docker login <my-registry>.azurecr.io
    ```

1. De container met de persoonlijke registerlocatie-code. Vervang de waarden in de punthaken `<my-registry>`, met de registernaam van uw eigen. 

    ```Bash
    docker tag <your-new-container-name>:<your-new-tag-name> <my-registry>.azurecr.io/<your-new-container-name-in-registry>:<your-new-tag-name>
    ```

    Als u niet de naam van een tag `latest` wordt geïmpliceerd.

1. De nieuwe installatiekopie naar uw persoonlijke containerregister pushen. Wanneer u uw persoonlijke containerregister bekijkt, is de containernaam die wordt gebruikt in de volgende CLI-opdracht is de naam van de opslagplaats.

    ```Bash
    docker push <my-registry>.azurecr.io/<your-new-container-name-in-registry>:<your-new-tag-name>
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Maken en gebruiken van Azure Container Instances](azure-container-instance-recipe.md)

<!--
## Store input and output configuration settings

Bake in input params only

FROM containerpreview.azurecr.io/microsoft/cognitive-services-luis:<tag>
COPY luisModel1 /input/
COPY luisModel2 /input/

## Store all configuration settings

If you are a single manager of the container, you may want to store all settings in the container. The new, resulting container will not need any variables passed in to run. 

Issues with this approach:

* In order to change these settings, you will have to change the values of the Dockerfile and rebuild the file. 
* If someone gets access to your container registry or your local host, they can run the container and use the Cognitive Services endpoints. 

The following _partial_ Dockerfile shows how to statically set the values for billing and model. This example uses the 

```Dockerfile
FROM <container-registry>/<cognitive-service-container-name>:<tag>
ENV billing=<billing value>
ENV apikey=<apikey value>
COPY luisModel1 /input/
COPY luisModel2 /input/
```

->