---
title: Azure Service Fabric Docker Compose implementatie Preview
description: Azure Service Fabric accepteert Docker Compose indeling voor het indelen van bestaande containers met behulp van Service Fabric te vereenvoudigen. Deze ondersteuning is momenteel in preview.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2017
ms.author: subramar
ms.openlocfilehash: 92d1951de8c8c80f7b47033dc751cd65a63c43f6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="docker-compose-deployment-support-in-azure-service-fabric-preview"></a>Ondersteuning voor de implementatie van docker Compose in Azure Service Fabric (Preview)

Maakt gebruik van docker de [docker-compose.yml](https://docs.docker.com/compose) bestand voor het definiëren van toepassingen met meerdere container. Zodat het eenvoudig voor klanten bekend zijn met Docker u bestaande containertoepassingen op Azure Service Fabric indeelt, hebben we systeemeigen ondersteuning voor de implementatie van Docker Compose preview opgenomen in het platform. Service Fabric kan accepteren versie 3 en hoger van `docker-compose.yml` bestanden. 

Omdat deze ondersteuning in preview is, wordt alleen een subset van Compose richtlijnen ondersteund. Bijvoorbeeld: toepassingsupgrades worden niet ondersteund. U kunt echter altijd verwijderen en implementeren van toepassingen in plaats van de upgrade.

Maak het cluster wilt gebruiken deze preview, met versie 5.7 of hoger van de Service Fabric-runtime via de Azure portal samen met de bijbehorende SDK. 

> [!NOTE]
> Deze functie is Preview-versie en wordt niet ondersteund in productie.
> De volgende voorbeelden zijn gebaseerd op de runtimeversie 6.0 en SDK versie 2.8.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Een bestand Docker Compose op Service Fabric implementeren

De volgende opdrachten een Service Fabric-toepassing maken (met de naam `fabric:/TestContainerApp`), die u kunt bewaken en beheert, zoals een andere Service Fabric-toepassing. U kunt de naam van de opgegeven toepassing voor statusquery's gebruiken.
Service Fabric herkent 'DeploymentName' als de id van de implementatie opstellen.

### <a name="use-powershell"></a>PowerShell gebruiken

Een implementatie voor Service Fabric opstellen maken van een docker-compose.yml-bestand met de volgende opdracht in PowerShell:

```powershell
New-ServiceFabricComposeDeployment -DeploymentName TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName`en `RegistryPassword` verwijzen naar de container register gebruikersnaam en wachtwoord. Nadat u de implementatie hebt voltooid, kunt u de status ervan controleren met behulp van de volgende opdracht:

```powershell
Get-ServiceFabricComposeDeploymentStatus -DeploymentName TestContainerApp
```

Gebruik de volgende opdracht voor het verwijderen van de implementatie opstellen via PowerShell:

```powershell
Remove-ServiceFabricComposeDeployment  -DeploymentName TestContainerApp
```

Gebruik de volgende opdracht voor het starten van een upgrade van de implementatie opstellen via PowerShell:

```powershell
Start-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp -Compose docker-compose-v2.yml -Monitored -FailureAction Rollback
```

Nadat de upgrade is geaccepteerd, kan voortgang van de upgrade worden bijgehouden met de volgende opdracht:

```powershell
Get-ServiceFabricComposeDeploymentUpgrade -Deployment TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Gebruik van Azure Service Fabric CLI (sfctl)

U kunt ook de volgende Service Fabric CLI-opdracht gebruiken:

```azurecli
sfctl compose create --deployment-name TestContainerApp --file-path docker-compose.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [ --timeout ]
```

Als u de implementatie hebt gemaakt, kunt u de status ervan controleren met behulp van de volgende opdracht:

```azurecli
sfctl compose status --deployment-name TestContainerApp [ --timeout ]
```

Gebruik de volgende opdracht voor het verwijderen van de implementatie opstellen:

```azurecli
sfctl compose remove  --deployment-name TestContainerApp [ --timeout ]
```

Gebruik de volgende opdracht voor het starten van een upgrade van de implementatie opstellen:

```powershell
sfctl compose upgrade --deployment-name TestContainerApp --file-path docker-compose-v2.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [--upgrade-mode Monitored] [--failure-action Rollback] [ --timeout ]
```

Nadat de upgrade is geaccepteerd, kan voortgang van de upgrade worden bijgehouden met de volgende opdracht:

```powershell
sfctl compose upgrade-status --deployment-name TestContainerApp
```

## <a name="supported-compose-directives"></a>Richtlijnen voor ondersteunde opstellen

Deze preview ondersteunt een subset van de configuratieopties uit de opstellen versie 3-indeling, met inbegrip van de volgende primitieven:

* Services > implementeren > replica's
* Services > implementeren > plaatsing > beperkingen
* Services > implementeren > Resources > limieten
    * cpu-shares
    * -geheugen
    * -geheugen-swap
* Services > opdrachten
* Services > omgeving
* Services > poorten
* Services > afbeelding
* Services > isolatie (alleen voor Windows)
* Services > logboekregistratie > stuurprogramma
* Services > logboekregistratie > stuurprogramma > Opties
* Volume & implementeren > Volume

Instellen van het cluster voor het afdwingen van de resource-limieten, zoals beschreven in [Service Fabric resource governance](service-fabric-resource-governance.md). Alle andere Docker Compose richtlijnen worden niet ondersteund voor deze preview.

## <a name="servicednsname-computation"></a>ServiceDnsName berekening

Als de naam van de service die u in een bestand Compose opgeeft een volledig gekwalificeerde domeinnaam is (dat wil zeggen, bevat een punt [.]), de DNS-naam geregistreerd door het Service Fabric `<ServiceName>` (inclusief de punt). Als dat niet het geval is, moet u elk padsegment in de naam van de toepassing wordt een domeinlabel in de service DNS-naam met het eerste padsegment om het domeinlabel op het hoogste niveau.

Als de naam van de opgegeven toepassing is bijvoorbeeld `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` zou de geregistreerde DNS-naam zijn.

## <a name="compose-deployment-instance-definition-versus-service-fabric-app-model-type-definition"></a>Het opstellen van implementatie (exemplaar definitie) ten opzichte van de Service Fabric-app-model (typedefinitie)

Een docker-compose.yml-bestand wordt een implementeerbaar van containers, waaronder de eigenschappen en de configuraties beschreven.
Het bestand kan bijvoorbeeld omgevingsvariabelen en poorten bevatten. U kunt ook implementatieparameters, zoals plaatsingsbeperkingen en limieten voor DNS-namen, in de docker-compose.yml-bestand opgeven.

De [Service Fabric-toepassingsmodel](service-fabric-application-model.md) maakt gebruik van service typen en de toepassingstypen, waarbij u veel exemplaren van een toepassing van hetzelfde type kan hebben. U kunt bijvoorbeeld één exemplaar per klant hebben. Dit model op basis van het type ondersteunt meerdere versies van hetzelfde toepassingstype dat geregistreerd bij de runtime.

Bijvoorbeeld kan een klant een toepassing met het type 1.0 AppTypeA geïnstantieerd, en klant B kan een andere toepassing die zijn gemaakt met hetzelfde type en dezelfde versie hebben. Definieert u de soorten toepassingen in de Toepassingsmanifesten en u de naam en de implementatie van een toepassingsparameters opgeven wanneer u de toepassing maakt.

Dit model biedt flexibiliteit, wel we hebben ook plannen voor de ondersteuning van een eenvoudigere, op basis van het exemplaar implementatiemodel waar typen impliciete van het manifestbestand zijn. In dit model wordt elke toepassing een eigen onafhankelijk manifest. We bekijkt deze inspanningen met ondersteuning voor docker-compose.yml, dit is een implementatie op basis van een exemplaar-indeling.

## <a name="next-steps"></a>Volgende stappen

* Lees informatie over de [toepassingsmodel Service Fabric](service-fabric-application-model.md)
* [Aan de slag met Service Fabric-CLI](service-fabric-cli.md)
