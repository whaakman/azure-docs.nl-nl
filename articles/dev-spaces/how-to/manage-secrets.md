---
title: Het beheren van geheimen bij het werken met een Azure-Dev-ruimte | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Snelle Kubernetes ontwikkeling met containers en microservices op Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
manager: douge
ms.openlocfilehash: b77d862f578ddc374dbb58117b4ea58eb973e5fe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198310"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Het beheren van geheimen bij het werken met een Azure-Dev-ruimte

Uw services mogelijk bepaalde wachtwoorden, verbindingsreeksen en andere geheime informatie, zoals voor databases of andere beveiligde Azure-services. De waarden van deze geheime gegevens in configuratiebestanden instelt, kunt u ze beschikbaar zijn in uw code als omgevingsvariabelen.  Dit moeten zorgvuldig om te voorkomen dat de beveiliging van de geheimen worden verwerkt.

Azure Dev spaties biedt twee opties voor het opslaan van geheimen aanbevolen: in het bestand values.dev.yaml en inline rechtstreeks in azds.yaml. Het is niet aanbevolen voor het opslaan van geheimen in values.yaml.
 
## <a name="method-1-valuesdevyaml"></a>Methode 1: values.dev.yaml
1. Open tegenover Code met uw project dat is ingeschakeld voor Azure Dev spaties.
2. Toevoegen van een bestand met de naam _values.dev.yaml_ in dezelfde map als het bestaande _values.yaml_ en definiëren van uw geheime sleutel en waarden, zoals in het volgende voorbeeld:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. Update _azds.yaml_ u vertelt Azure Dev spaties aan het gebruik van uw nieuwe _values.dev.yaml_ bestand. Om dit te doen, moet u deze configuratie onder de sectie configurations.develop.container toevoegen:

    ```yaml
           container:
             values:
             - "charts/webfrontend/values.dev.yaml"
    ```
 
4. Wijzig uw servicecode om te verwijzen naar deze geheime gegevens als omgevingsvariabelen, zoals in het volgende voorbeeld:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Bijwerken van de services die in het cluster met deze wijzigingen zijn uitgevoerd. Voer de opdracht op de opdrachtregel:

    ```
    azds up
    ```
 
6. (Optioneel) Controleer of deze geheime gegevens zijn gemaakt vanaf de opdrachtregel:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Zorg ervoor dat u toevoegt _values.dev.yaml_ naar de _.gitignore_ bestand om te voorkomen dat het vastleggen van geheimen in broncodebeheer.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Methode 2: Inline rechtstreeks in azds.yaml
1.  In _azds.yaml_, geheimen onder de yaml sectie configuraties/ontwikkelen/installatie ingesteld. U kunt wel invoeren geheim waarden rechtstreeks, deze niet aanbevolen wordt omdat _azds.yaml_ wordt ingecheckt in broncodebeheer. In plaats daarvan toevoegen met de syntaxis '$PLACEHOLDER' tijdelijke aanduidingen.

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT_DEV"
                host: "$REDIS_HOST_DEV"
                key: "$REDIS_KEY_DEV"
    ```
     
2.  Maak een _.env_ bestand in dezelfde map als _azds.yaml_. Voer geheimen met behulp van standaard sleutel = waarde-notatie. Niet doorvoeren de _.env_ bestand met resourcebeheer. (Als u wilt weglaten van broncodebeheer in git-versie besturingselement systemen, toe te voegen aan de _.gitignore_ bestand.) Het volgende voorbeeld wordt een _.env_ bestand:

    ```
    REDIS_PORT_DEV=3333
    REDIS_HOST_DEV=myredishost
    REDIS_KEY_DEV=myrediskey
    ```
2.  Wijzig de broncode van uw service om te verwijzen naar deze geheime gegevens in de code, zoals in het volgende voorbeeld:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Bijwerken van de services die in het cluster met deze wijzigingen zijn uitgevoerd. Voer de opdracht op de opdrachtregel:

    ```
    azds up
    ```

4.  (optioneel) Weergave geheimen van kubectl:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Volgende stappen

Met deze methoden kunt u nu veilig verbinding met een database, een Redis-cache of toegang tot beveiligde Azure-services.
 