---
title: Probleemoplossing | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Snelle Kubernetes ontwikkeling met containers en microservices op Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 371bb9195266f3511d115de2532e6b64f49ef26f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen

Deze handleiding bevat informatie over veelvoorkomende problemen die u wellicht bij gebruik van Azure Dev spaties.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Fout 'upstream verbindingsfout of Verbreek de verbinding met/reset voordat headers'
U kunt deze fout wordt weergegeven wanneer u probeert te krijgen tot uw service. Bijvoorbeeld, wanneer u gaat naar de URL van de service in een browser. 

### <a name="reason"></a>Reden 
De container-poort is niet beschikbaar. Dit kan komen doordat: 
* De container is nog steeds wordt gemaakt en geïmplementeerd. Dit kan bijvoorbeeld het geval als u `azds up` of het foutopsporingsprogramma start en probeer het vervolgens toegang tot de container voordat deze is geïmplementeerd.
* Poortconfiguratie is niet consistent tussen uw Dockerfile, Helm grafiek en servercode die een poort wordt geopend.

### <a name="try"></a>Probeer een van:
1. Als de container is bezig te worden gebouwd/geïmplementeerd, kunt u wacht 2-3 seconden en probeer het opnieuw openen van de service. 
1. Controleer uw poortconfiguratie. De opgegeven poortnummers moet **identieke** in de onderstaande activa:
    * **Dockerfile:** opgegeven door de `EXPOSE` instructie.
    * **[Helm grafiek](https://docs.helm.sh):** opgegeven door de `externalPort` en `internalPort` waarden voor een service (vaak zich in een `values.yml` bestand),
    * Alle poorten wordt geopend in de toepassingscode, bijvoorbeeld in Node.js: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Configuratiebestand niet gevonden
U hebt uitgevoerd `azds up` en het volgende foutbericht: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Reden
U moet uitvoeren `azds up` uit de hoofddirectory van de code die u wilt uitvoeren en moet u de map code uit te voeren met Azure Dev spaties initialiseren.

### <a name="try"></a>Probeer een van:
1. Uw huidige map naar de hoofdmap met uw servicecode wijzigen. 
1. Als u een bestand azds.yaml niet in de map code hebt, voert u `azds prep` voor het genereren van Docker, Kubernetes en Azure Dev spaties activa.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Fout: 'pipe wordt onverwacht afgesloten met code 126 azds.'
Starten van het foutopsporingsprogramma tegenover Code leiden soms tot deze fout. Dit is een bekend probleem.

### <a name="try"></a>Probeer een van:
1. Sluit en open VS-Code.
2. Druk nogmaals op F5.


## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Fout bij foutopsporing 'geconfigureerde foutopsporing type 'coreclr' wordt niet ondersteund'
Het foutopsporingsprogramma tegenover Code wordt uitgevoerd, rapporteert de fout: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Reden
U hebt niet de Code van de VS-extensie voor Azure Dev spaties geïnstalleerd op uw ontwikkelcomputer.

### <a name="try"></a>Probeer een van:
Installeer de [tegenover Code-extensie voor Azure Dev spaties](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>Het type of de naam 'MijnBibliotheek' kan niet worden gevonden.

### <a name="reason"></a>Reden 
De build-context is op het project serviceniveau standaard, won't dus een bibliotheekproject u gevonden.

### <a name="try"></a>Probeer een van:
Wat moet worden uitgevoerd:
1. Het bestand azds.yaml de build-context ingesteld op het niveau van de oplossing wijzigen.
2. Wijzig de bestanden Dockerfile en Dockerfile.develop om te verwijzen naar de bestanden csproj correct ten opzichte van de nieuwe build-context.
3. Een bestand .dockerignore plaatsen naast het .sln-bestand en wijzig indien nodig.

U vindt een voorbeeld op https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftconnectedenvironmentregisteraction-authorization-error"></a>Autorisatiefout 'Microsoft.ConnectedEnvironment/register/action'
U kunt de volgende fout tegenkomen wanneer u een Azure-Dev-ruimte beheert en u in een Azure-abonnement waarvoor er geen eigenaar of bijdrager toegang werkt.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.ConnectedEnvironment/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Reden
Het geselecteerde Azure-abonnement is niet geregistreerd voor de naamruimte Microsoft.ConnectedEnvironment.

### <a name="try"></a>Probeer een van:
Iemand met de eigenaar of bijdrager toegang tot het Azure-abonnement kunt de volgende opdracht in de Azure CLI handmatig registreren van de naamruimte Microsoft.ConnectedEnvironment uitvoeren:

```cmd
az provider register --namespace Microsoft.ConnectedEnvironment
```

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure Dev spaties lijkt niet mijn bestaande Dockerfile gebruiken voor het bouwen van een container 

### <a name="reason"></a>Reden
Azure Dev spaties kunnen worden geconfigureerd om te verwijzen naar een specifieke Dockerfile in uw project. Als het erop dat Azure Dev spaties is niet met behulp van de Dockerfile die u verwacht lijkt te bouwen van uw containers, moet u mogelijk u expliciet vertelt Azure Dev spaties waar is. 

### <a name="try"></a>Probeer een van:
Open de `azds.yaml` -bestand dat is gegenereerd door Azure Dev spaties in uw project. Gebruik de `configurations->develop->build->dockerfile` richtlijn om te verwijzen naar de Dockerfile die u wilt gebruiken:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```