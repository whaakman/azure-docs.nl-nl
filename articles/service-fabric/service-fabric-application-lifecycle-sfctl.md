---
title: Beheren van Azure Service Fabric-toepassingen met behulp van Azure Service Fabric-CLI (sfctl)
description: Meer informatie over het implementeren en verwijderen van toepassingen van een Azure Service Fabric-cluster met behulp van Azure Service Fabric-CLI
services: service-fabric
author: Christina-Kang
manager: timlt
ms.service: service-fabric
ms.topic: conceptual
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: 2e71996e22fee34b29139fdf19764c47616beb1d
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492746"
---
# <a name="manage-an-azure-service-fabric-application-by-using-azure-service-fabric-cli-sfctl"></a>Een Azure Service Fabric-toepassing te beheren met behulp van Azure Service Fabric-CLI (sfctl)

Informatie over het maken en verwijderen van toepassingen die worden uitgevoerd in een Azure Service Fabric-cluster.

## <a name="prerequisites"></a>Vereiste onderdelen

* Service Fabric-CLI installeren. Selecteer vervolgens uw Service Fabric-cluster. Zie voor meer informatie, [aan de slag met Service Fabric-CLI](service-fabric-cli.md).

* Hebben een Service Fabric-toepassingspakket kan worden geïmplementeerd. Voor meer informatie over hoe u de auteur en pakket met een toepassing, leest u over de [Service Fabric-toepassingsmodel](service-fabric-application-model.md).

## <a name="overview"></a>Overzicht

Voer de volgende stappen uit voor het implementeren van een nieuwe toepassing:

1. Een toepassingspakket uploaden naar de installatiekopieopslag van het Service Fabric.
2. Een toepassingstype inrichten.
3. Verwijder de inhoud van de store.
4. Geef en maken van een toepassing.
5. Geef en services maken.

Als u wilt verwijderen van een bestaande toepassing, de volgende stappen uit:

1. Verwijder de toepassing.
2. Inrichting verwijderen van het bijbehorende type.

## <a name="deploy-a-new-application"></a>Een nieuwe toepassing implementeren

Voor het implementeren van een nieuwe toepassing, moet u de volgende taken uitvoeren:

### <a name="upload-a-new-application-package-to-the-image-store"></a>Een nieuw toepassingspakket uploaden naar de installatiekopieopslag

Voordat u een toepassing maakt, moet u het toepassingspakket uploaden naar de installatiekopieopslag van het Service Fabric.

Bijvoorbeeld, als uw toepassingspakket is in de `app_package_dir` directory, de volgende opdrachten gebruiken voor het uploaden van de map:

```azurecli
sfctl application upload --path ~/app_package_dir
```

Voor grote toepassingpakketten, kunt u opgeven de `--show-progress` optie om de voortgang van het uploaden van de weer te geven.

### <a name="provision-the-application-type"></a>Inrichten van het toepassingstype

Wanneer het uploaden is voltooid, richt u de toepassing. Gebruik de volgende opdracht voor het inrichten van de toepassing:

```azurecli
sfctl application provision --application-type-build-path app_package_dir
```

De waarde voor `application-type-build-path` is de naam van de map waar u uw toepassingspakket hebt geüpload.

### <a name="delete-the-application-package"></a>Het toepassingspakket verwijderen

Het verdient aanbeveling dat u het toepassingspakket verwijderen nadat de registratie van de toepassing is geslaagd.  Toepassingspakketten verwijderen uit de installatiekopieopslag vrijgemaakt systeemresources.  Niet-gebruikte toepassingspakketten disk-opslag verbruikt en leidt tot prestatieproblemen van toepassing. 

Als het toepassingspakket verwijderen uit de installatiekopieopslag, gebruik de volgende opdracht:

```azurecli
sfctl store delete --content-path app_package_dir
```

`content-path` moet u de naam van de map die u hebt geüpload tijdens het maken van de toepassing.

### <a name="create-an-application-from-an-application-type"></a>Maak een toepassing van een toepassingstype

Nadat u de toepassing inrichten, gebruikt u de volgende opdracht een naam en het maken van uw toepassing:

```azurecli
sfctl application create --app-name fabric:/TestApp --app-type TestAppType --app-version 1.0
```

`app-name` is de naam die u wilt gebruiken voor het exemplaar van de toepassing. U kunt extra parameters ophalen uit het eerder ingerichte toepassingsmanifest.

Naam van de toepassing moet beginnen met het voorvoegsel `fabric:/`.

### <a name="create-services-for-the-new-application"></a>Services voor de nieuwe toepassing maken

Nadat u een toepassing hebt gemaakt, kunt u services maken van de toepassing. In het volgende voorbeeld maken we een nieuwe stateless service van de toepassing. De services die u van een toepassing maken kunt worden gedefinieerd in een servicemanifest in het eerder ingerichte toepassingspakket.

```azurecli
sfctl service create --app-id TestApp --name fabric:/TestApp/TestSvc --service-type TestServiceType \
--stateless --instance-count 1 --singleton-scheme
```

## <a name="verify-application-deployment-and-health"></a>Controleer of de implementatie van toepassing en status

Om te controleren of dat alles in orde is, gebruikt u de volgende status-opdrachten:

```azurecli
sfctl application list
sfctl service list --application-id TestApp
```

Om te controleren of de service in orde is, moet u vergelijkbare opdrachten gebruiken om op te halen van de status van de service en de toepassing:

```azurecli
sfctl application health --application-id TestApp
sfctl service health --service-id TestApp/TestSvc
```

In orde services en toepassingen hebben een `HealthState` waarde van `Ok`.

## <a name="remove-an-existing-application"></a>Een bestaande toepassing verwijderen

Als u wilt verwijderen van een toepassing, moet u de volgende taken uitvoeren:

### <a name="delete-the-application"></a>De toepassing verwijderen

Als u wilt verwijderen van de toepassing, gebruikt u de volgende opdracht uit:

```azurecli
sfctl application delete --application-id TestEdApp
```

### <a name="unprovision-the-application-type"></a>Het toepassingstype verwijderen

Nadat u de toepassing hebt verwijderd, kunt u het type inrichting verwijderen als u deze niet meer nodig hebt. Als u wilt het toepassingstype verwijderen, gebruik de volgende opdracht:

```azurecli
sfctl application unprovision --application-type-name TestAppType --application-type-version 1.0
```

De naam en type versie moeten overeenkomen met de naam en versie in de eerder ingerichte toepassingsmanifest.

## <a name="upgrade-application"></a>Toepassing upgraden

Na het maken van uw toepassing, kunt u de dezelfde reeks stappen voor het inrichten van een tweede versie van uw toepassing te herhalen. Vervolgens kunt u met de upgrade van een Service Fabric-toepassing ook overgaan op de tweede versie van de toepassing. Zie voor meer informatie de documentatie over [upgrades van de Service Fabric-toepassingen](service-fabric-application-upgrade.md).

Als u wilt een upgrade uitvoert, moet u eerst de volgende versie van de toepassing met behulp van dezelfde opdrachten als voorheen inrichten:

```azurecli
sfctl application upload --path ~/app_package_dir_2
sfctl application provision --application-type-build-path app_package_dir_2
sfctl store delete --content-path app_package_dir_2
```

Het verdient aanbeveling klikt u vervolgens op een bewaakte Automatische upgrade uitvoeren, start u de upgrade de volgende opdracht uit:

```azurecli
sfctl application upgrade --app-id TestApp --app-version 2.0.0 --parameters "{\"test\":\"value\"}" --mode Monitored
```

Upgrades overschrijven bestaande parameters met de gewenste set is opgegeven. Parameters voor de toepassing moeten als argumenten worden doorgegeven aan de opdracht upgrade, indien nodig. Parameters voor de toepassing moeten worden gecodeerd als een JSON-object.

Als u wilt ophalen van alle eerder opgegeven parameters, kunt u de `sfctl application info` opdracht.

Wanneer u een upgrade van de toepassing wordt uitgevoerd, de status kan worden opgehaald met behulp van de `sfctl application upgrade-status` opdracht.

Ten slotte, als een upgrade wordt uitgevoerd en moet worden geannuleerd, kunt u de `sfctl application upgrade-rollback` aan de upgrade terugdraaien.

## <a name="next-steps"></a>Volgende stappen

* [Basisprincipes van service Fabric-CLI](service-fabric-cli.md)
* [Aan de slag met Service Fabric in Linux](service-fabric-get-started-linux.md)
* [De upgrade van een Service Fabric-toepassing starten](service-fabric-application-upgrade.md)
