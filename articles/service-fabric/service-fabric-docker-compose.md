---
title: Azure Service Fabric Docker Compose implementatie-Preview
description: Azure Service Fabric accepteert Docker Compose-indeling voor het indelen van bestaande containers met behulp van Service Fabric te vereenvoudigen. Deze ondersteuning is momenteel in preview.
services: service-fabric
documentationcenter: .net
author: TylerMSFT
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: twhitney, subramar
ms.openlocfilehash: e4eb26ab91261d1888d3c756d611db1b31801e8f
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52720223"
---
# <a name="docker-compose-deployment-support-in-azure-service-fabric-preview"></a>Ondersteuning voor de implementatie van docker Compose in Azure Service Fabric (Preview)

Docker maakt gebruik van de [docker-compose.yml](https://docs.docker.com/compose) bestand voor het definiëren van toepassingen met meerdere containers. Mogelijk te maken voor klanten bekend zijn met Docker voor het indelen van bestaande containertoepassingen op Azure Service Fabric, hebben we ondersteuning voor Preview-versie voor de implementatie van Docker Compose systeemeigen opgenomen in het platform. Service Fabric accepteert versie 3 en hoger van `docker-compose.yml` bestanden. 

Omdat deze ondersteuning in preview is, wordt alleen een subset van de Compose-instructies ondersteund. Toepassingsupgrades worden bijvoorbeeld niet ondersteund. U kunt echter altijd verwijderen en implementeren van toepassingen in plaats van de upgrade.

Voor het gebruik van deze Preview-versie, maakt u uw cluster met versie 5.7 of hoger van de Service Fabric-runtime via de Azure-portal samen met de bijbehorende SDK. 

> [!NOTE]
> Deze functie is beschikbaar als preview en wordt niet ondersteund in de productieomgeving.
> De onderstaande voorbeelden zijn gebaseerd op de runtime-versie 6.0 en SDK-versie 2.8.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Een Docker Compose-bestand in Service Fabric implementeren

De volgende opdrachten maakt u een Service Fabric-toepassing (met de naam `fabric:/TestContainerApp`), die u kunt controleren en beheren, zoals elke andere Service Fabric-toepassing. U kunt de naam van de opgegeven toepassing voor statusquery's gebruiken.
Service Fabric herkent 'DeploymentName' als de id van de implementatie voor opstellen.

### <a name="use-powershell"></a>PowerShell gebruiken

Een Service Fabric-Compose-implementatie van een docker-compose.yml-bestand maken door het uitvoeren van de volgende opdracht uit in PowerShell:

```powershell
New-ServiceFabricComposeDeployment -DeploymentName TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName` en `RegistryPassword` verwijzen naar de container registry gebruikersnaam en wachtwoord. Nadat u de implementatie hebt voltooid, kunt u de status ervan controleren met behulp van de volgende opdracht uit:

```powershell
Get-ServiceFabricComposeDeploymentStatus -DeploymentName TestContainerApp
```

Als u wilt verwijderen van de implementatie voor opstellen via PowerShell, gebruikt u de volgende opdracht uit:

```powershell
Remove-ServiceFabricComposeDeployment  -DeploymentName TestContainerApp
```

Voor het starten van een upgrade van de implementatie opstellen via PowerShell, gebruikt u de volgende opdracht uit:

```powershell
Start-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp -Compose docker-compose-v2.yml -Monitored -FailureAction Rollback
```

Ongedaan maken van de implementatie voor opstellen bijwerken via PowerShell, gebruikt u de volgende opdracht uit:

```powershell
Start-ServiceFabricComposeDeploymentRollback -DeploymentName TestContainerApp
```

Nadat de upgrade is geaccepteerd, kan voortgang van de upgrade worden bijgehouden met de volgende opdracht:

```powershell
Get-ServiceFabricComposeDeploymentUpgrade -DeploymentName TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Azure Service Fabric-CLI (sfctl) gebruiken

U kunt ook de volgende Service Fabric-CLI-opdracht gebruiken:

```azurecli
sfctl compose create --deployment-name TestContainerApp --file-path docker-compose.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [ --timeout ]
```

Nadat u de implementatie hebt gemaakt, kunt u de status ervan controleren met behulp van de volgende opdracht uit:

```azurecli
sfctl compose status --deployment-name TestContainerApp [ --timeout ]
```

Als u wilt verwijderen van de implementatie voor opstellen, gebruik de volgende opdracht:

```azurecli
sfctl compose remove  --deployment-name TestContainerApp [ --timeout ]
```

Voor het starten van een upgrade van de implementatie opstellen, gebruik de volgende opdracht:

```azurecli
sfctl compose upgrade --deployment-name TestContainerApp --file-path docker-compose-v2.yml [ [ --user --encrypted-pass ] | [ --user --has-pass ] ] [--upgrade-mode Monitored] [--failure-action Rollback] [ --timeout ]
```

Upgrade te draaien, maar de implementatie voor opstellen, gebruik de volgende opdracht:

```azurecli
sfctl compose upgrade-rollback --deployment-name TestContainerApp [ --timeout ]
```

Nadat de upgrade is geaccepteerd, kan voortgang van de upgrade worden bijgehouden met de volgende opdracht:

```azurecli
sfctl compose upgrade-status --deployment-name TestContainerApp
```

## <a name="supported-compose-directives"></a>Ondersteunde Compose-instructies

Deze Preview-versie ondersteunt een subset van de configuratieopties van de Compose versie 3-indeling, met inbegrip van de volgende primitieven:

* Services > implementeren > replica's
* Services > implementeren > plaatsing > beperkingen
* Services > implementeren > Resources > limieten
    * cpu-shares
    * -geheugen
    * -geheugen-wisselen
* Services > opdrachten
* Services > omgeving
* Services > poorten
* Services > installatiekopie
* Services > isolatie (alleen voor Windows)
* Services > logboekregistratie > stuurprogramma
* Services > logboekregistratie > stuurprogramma > Opties
* Volume & implementeren > Volume

Instellen van het cluster voor het afdwingen van resourcebeperkingen, zoals beschreven in [Service Fabric-resourcebeheer](service-fabric-resource-governance.md). Alle andere Docker Compose richtlijnen worden niet ondersteund voor deze Preview-versie.

### <a name="ports-section"></a>Sectie poorten

Geef de http- of https-protocol in de sectie poorten die worden gebruikt door de Service Fabric service-listener. Dit zorgt ervoor dat de eindpunt-protocol correct is gepubliceerd met de naming-service om toe te staan van omgekeerde proxy voor het doorsturen van aanvragen:
* Om te routeren naar niet-beveiligde Service Fabric-Compose-services, geef **/http**. Bijvoorbeeld- **"80:80 / http"**.
* Als u wilt doorsturen naar beveiligde Service Fabric-Compose-services, geef **/https**. Bijvoorbeeld- **"443:443 / https"**.

> [!NOTE]
> De syntaxis van de sectie /http en /https poorten is specifiek voor Service Fabric voor het registreren van de correcte URL van de Service Fabric-listener.  Als de Docker compose syntaxis van bestanden via een programma is gevalideerd, kan dit ertoe leiden dat een validatiefout.

## <a name="servicednsname-computation"></a>ServiceDnsName berekening

Als de naam van de service die u in een opstellen-bestand opgeeft een volledig gekwalificeerde domeinnaam is (dat wil zeggen, bevat een punt [.]), de DNS-naam die is geregistreerd door Service Fabric is `<ServiceName>` (inclusief de punt). Als dat niet het geval is, moet u elk padsegment in de naam van de toepassing wordt een domeinlabel in de naam van het DNS-service, met het eerste padsegment steeds het domeinlabel op het hoogste niveau.

Bijvoorbeeld, als de naam van de opgegeven toepassing is `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` zou de geregistreerde DNS-naam.

## <a name="compose-deployment-instance-definition-versus-service-fabric-app-model-type-definition"></a>(In het definitie exemplaar) implementatie voor opstellen ten opzichte van Service Fabric-app-model (typedefinitie)

Een docker-compose.yml-bestand beschrijft een implementeerbare set van containers, waaronder eigenschappen en configuraties.
Het bestand kan bijvoorbeeld, omgevingsvariabelen en poorten bevatten. U kunt ook implementatieparameters, zoals plaatsingsbeperkingen bronlimieten en DNS-namen, in het docker-compose.yml-bestand opgeven.

De [Service Fabric-toepassingsmodel](service-fabric-application-model.md) maakt gebruik van service-typen en de toepassingstypen, waar u veel exemplaren van hetzelfde type kunt hebben. U kunt bijvoorbeeld één toepassingsexemplaar per klant hebben. Dit model op basis van het type biedt ondersteuning voor meerdere versies van hetzelfde toepassingstype dat geregistreerd met de runtime.

Bijvoorbeeld een klant kan een toepassing gemaakt met het type 1.0 van AppTypeA hebben, en klant B kan een andere toepassing gemaakt met hetzelfde type en dezelfde versie hebben. U definieert de soorten toepassingen in de Toepassingsmanifesten en u de naam en de implementatie van een toepassingsparameters opgeven wanneer u de toepassing maakt.

Hoewel dit model biedt een flexibiliteit, zijn we ook plannen voor de ondersteuning van een eenvoudigere, op basis van een exemplaar implementatiemodel waar typen impliciete van het manifestbestand zijn. In dit model krijgt elke toepassing een eigen onafhankelijke manifest. We zijn beschikbaar als Preview inspanningen door het toevoegen van ondersteuning voor docker-compose.yml, dit is een exemplaar op basis van implementatie-indeling.

## <a name="next-steps"></a>Volgende stappen

* Lees de [Service Fabric-toepassingsmodel](service-fabric-application-model.md)
* [Aan de slag met Service Fabric-CLI](service-fabric-cli.md)
