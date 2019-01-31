---
title: Geheimen beheren bij het werken met een Azure Dev Space | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
ms.openlocfilehash: 7081412172b617632073cad4e76d7c8fc4e81519
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466554"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Geheimen beheren bij het werken met een Azure Dev-adresruimte

Uw services kunnen bepaalde wachtwoorden, connectiestrings en andere geheimen vereisen, zoals voor databases of andere beveiligde Azure-services. Door de waarden van deze geheimen in configuratiebestanden in te stellen, kunt u ze als omgevingsvariabelen beschikbaar maken in uw code.  Deze moeten met zorg worden behandeld om te voorkomen dat de veiligheid van de geheimen in gevaar komt.

Azure Dev Spaces biedt twee aanbevolen opties voor het opslaan van geheimen: in het bestand values.dev.yaml en direct inline in azds.yaml. Het is niet aan te raden om geheimen op te slaan in values.yaml.
 
## <a name="method-1-valuesdevyaml"></a>Methode 1: values.dev.yaml
1. Open VS Code met uw project dat is ingeschakeld voor Azure Dev Spaces.
2. Voeg een bestand met de naam _values.dev.yaml_ toe in dezelfde map als het bestaande _values.yaml_ en definieer de geheime sleutel en de waarden, zoals in het volgende voorbeeld:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. Werk _azds.yaml_ bij om Azure Dev Spaces te vertellen dat het uw nieuwe bestand _values.dev.yaml_ moet gebruiken. Om dit te doen, moet u deze configuratie onder de sectie configurations.develop.container toevoegen:

    ```yaml
           container:
             values:
             - "charts/webfrontend/values.dev.yaml"
    ```
 
4. Wijzig de servicecode om te verwijzen naar deze geheime gegevens als omgevingsvariabelen, zoals in het volgende voorbeeld:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Werk de services die in uw cluster worden uitgevoerd bij met deze wijzigingen. Voer de volgende opdracht op de opdrachtregel uit:

    ```
    azds up
    ```
 
6. (Optioneel) Controleer of deze geheime gegevens zijn gemaakt via de opdrachtregel:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Zorg ervoor dat u _values.dev.yaml_ toevoegt aan het bestand _.gitignore_ om te voorkomen dat geheimen in broncodebeheer worden vastgelegd.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Methode 2: Inline rechtstreeks in azds.yaml
1.  In _azds.yaml_, stel geheimen in onder de yaml sectie configuraties/ontwikkeling/installatie. U kunt wel geheime waarden direct invoeren, maar het is niet aanbevolen omdat _azds.yaml_ wordt opgenomen in broncodebeheer. In plaats daarvan voegt u met de syntaxis '$PLACEHOLDER' tijdelijke aanduidingen toe.

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
     
2.  Maak een _.env_ bestand in dezelfde map als _azds.yaml_. Voer geheimen met behulp van standaard sleutel = waarde-notatie. Neem het _.env_ bestand niet op in broncodebeheer. (Om bestanden uit te sluiten vanuit broncodebeheer in git-gebaseerde versiebeheersystemen voegt u deze toe aan het bestand _.gitignore_ .) Het volgende voorbeeld toont een _.env_ bestand:

    ```
    REDIS_PORT_DEV=3333
    REDIS_HOST_DEV=myredishost
    REDIS_KEY_DEV=myrediskey
    ```
2.  Wijzig uw servicecode om te verwijzen naar deze geheimen in code, zoals in het volgende voorbeeld:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Werk de services die in uw cluster worden uitgevoerd bij met deze wijzigingen. Voer de volgende opdracht op de opdrachtregel uit:

    ```
    azds up
    ```

4.  (optioneel) Bekijk de geheimen van kubectl:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Volgende stappen

Met deze methoden kunt u nu veilig verbinding maken met een database, een Azure-Cache voor Redis, of toegang tot beveiligde Azure-services.
 
