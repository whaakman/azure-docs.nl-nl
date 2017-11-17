---
title: Azure Service Fabric-toepassingen met Azure Service Fabric CLI beheren
description: Informatie over het implementeren en toepassingen van een Azure Service Fabric-cluster verwijderen met behulp van Azure Service Fabric CLI
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: 6c8501e8c863e1240a277bf8064118a2b113a81b
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli"></a>Een Azure Service Fabric-toepassing te beheren met behulp van Azure Service Fabric CLI

Informatie over het maken en verwijderen van toepassingen die worden uitgevoerd in een Azure Service Fabric-cluster.

## <a name="prerequisites"></a>Vereisten

* Service Fabric CLI installeren. Selecteer vervolgens uw Service Fabric-cluster. Zie voor meer informatie [aan de slag met Service Fabric CLI](service-fabric-cli.md).

* Een Service Fabric-toepassingspakket gereed om te worden geïmplementeerd hebben. Lees voor meer informatie over het maken en een toepassing pakket over de [Service Fabric-toepassingsmodel](service-fabric-application-model.md).

## <a name="overview"></a>Overzicht

Voer deze stappen voor het implementeren van een nieuwe toepassing:

1. Upload een toepassingspakket naar het archief van de Service Fabric-installatiekopie.
2. Een toepassingstype inrichten.
3. Verwijder de inhoud van de store.
4. Geef op en maak een toepassing.
5. Geef en services maken.

Een bestaande toepassing te verwijderen door deze stappen te voltooien:

1. Verwijder de toepassing.
2. Het type van de bijbehorende inrichting.

## <a name="deploy-a-new-application"></a>Een nieuwe toepassing implementeren

Voor het implementeren van een nieuwe toepassing, moet u de volgende taken uitvoeren:

### <a name="upload-a-new-application-package-to-the-image-store"></a>Een nieuw toepassingspakket uploaden naar de image store

Voordat u een toepassing maakt, moet u het toepassingspakket uploaden naar het archief van de Service Fabric-installatiekopie.

Bijvoorbeeld, als uw toepassingspakket is in de `app_package_dir` directory, de volgende opdrachten gebruiken voor het uploaden van de map:

```azurecli
sfctl application upload --path ~/app_package_dir
```

Voor grote toepassingpakketten, kunt u de `--show-progress` optie om de voortgang van het uploaden van de weer te geven.

### <a name="provision-the-application-type"></a>Het toepassingstype inrichten

Wanneer het uploaden is voltooid, richt u de toepassing. Gebruik de volgende opdracht voor het inrichten van de toepassing:

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

De waarde voor `application-type-build-path` is de naam van de map waar u uw toepassingspakket geüpload.

### <a name="delete-the-application-package"></a>Het toepassingspakket wissen

Het verdient aanbeveling dat u het toepassingspakket verwijderen nadat de toepassing is geregistreerd.  Toepassingspakketten verwijderen uit de image store vrijgemaakt systeembronnen.  Schijfopslag verbruikt en leidt tot een toepassing prestatieproblemen in ongebruikte toepassingspakketten houden. 

De om toepassingspakket te verwijderen uit het installatiekopiearchief, gebruik de volgende opdracht:

```azurecli
sfctl store delete --content-path app_package_dir
```

`content-path`moet de naam van de map die u hebt geüpload wanneer u de toepassing gemaakt.

### <a name="create-an-application-from-an-application-type"></a>Maak een toepassing van een toepassingstype

Nadat u de toepassing inricht, moet u de volgende opdracht gebruiken een naam en het maken van uw toepassing:

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name`de naam die u wilt gebruiken voor het toepassingsexemplaar is. Aanvullende parameters kunt u krijgen via het eerder ingerichte toepassingsmanifest.

De toepassingsnaam moet beginnen met het voorvoegsel `fabric:/`.

### <a name="create-services-for-the-new-application"></a>Services voor de nieuwe toepassing maken

Nadat u een toepassing hebt gemaakt, kunt u services maken van de toepassing. In het volgende voorbeeld maken wordt een nieuwe staatloze service van de toepassing. De services die u van een toepassing maken kunt worden gedefinieerd in een servicemanifest in het eerder ingerichte toepassingspakket.

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Controleer of de implementatie van de toepassing en status

Om te controleren of dat alles in orde is, gebruik de volgende health-opdrachten:

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

Om te controleren of de service in orde is, gelijksoortige opdrachten te gebruiken voor het ophalen van de status van de service en de toepassing:

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

In orde services en toepassingen hebben een `HealthState` waarde van `Ok`.

## <a name="remove-an-existing-application"></a>Een bestaande toepassing verwijderen

Voer de volgende taken een toepassing te verwijderen:

### <a name="delete-the-application"></a>De toepassing verwijderen

Gebruik de volgende opdracht voor het verwijderen van de toepassing:

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a>Het type van de inrichting

Nadat u de toepassing hebt verwijderd, kunt u het type van de inrichting als u deze niet langer nodig hebt. Als u wilt het toepassingstype inrichting, moet u de volgende opdracht gebruiken:

```azurecli
sfctl application unprovision --application-type-name TestAppType --application-type-version 1.0
```

De naam en type versie moeten overeenkomen met de naam en versie in de eerder ingerichte toepassingsmanifest.

## <a name="upgrade-application"></a>Toepassing bijwerken

U kunt dezelfde reeks stappen voor het inrichten van een tweede versie van uw toepassing herhalen na het maken van uw toepassing. Vervolgens kunt u met de upgrade van een Service Fabric-toepassing worden overgezet naar de tweede versie van de toepassing. Zie voor meer informatie de documentatie op [Service Fabric-toepassingsupgrades](service-fabric-application-upgrade.md).

Als u wilt een upgrade uitvoert, moet u eerst de volgende versie van de toepassing die gebruikmaakt van dezelfde opdrachten als voorheen inrichten:

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
sfctl store delete --content-path app_package_dir_2
```

U wordt aangeraden vervolgens naar een bewaakte Automatische upgrade uitvoeren, start de upgrade door de volgende opdracht uit te voeren:

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

Upgrades overschrijven bestaande parameters met welke set is opgegeven. Parameters voor de toepassing moeten worden doorgegeven als argumenten voor de opdracht upgraden, indien nodig. Parameters voor de toepassing moeten worden gecodeerd als een JSON-object.

Voor het ophalen van eerder opgegeven parameters, kunt u de `sfctl application info` opdracht.

Wanneer u een upgrade van de toepassing wordt uitgevoerd, de status kan worden opgehaald met de `sfctl application upgrade-status` opdracht.

Ten slotte, als een upgrade wordt uitgevoerd en moet worden geannuleerd, kunt u de `sfctl application upgrade-rollback` de upgrade terugdraaien.

## <a name="next-steps"></a>Volgende stappen

* [Basisprincipes van service Fabric CLI](service-fabric-cli.md)
* [Aan de slag met Service Fabric op Linux](service-fabric-get-started-linux.md)
* [Starten van de upgrade van een Service Fabric-toepassing](service-fabric-application-upgrade.md)
