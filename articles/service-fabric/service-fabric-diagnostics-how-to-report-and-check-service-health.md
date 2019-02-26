---
title: Rapporteren en controleren van health gebruiken met Azure Service Fabric | Microsoft Docs
description: Informatie over health om rapporten te verzenden vanuit uw servicecode en hoe u controleert de status van uw service met behulp van de health-controleprogramma's die Azure Service Fabric biedt.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: mfussell
editor: ''
ms.assetid: 7c712c22-d333-44bc-b837-d0b3603d9da8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 2126157f49bd978d2218986601245cae2e4157b6
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821675"
---
# <a name="report-and-check-service-health"></a>Servicestatus rapporteren en controleren
Wanneer uw services problemen optreden, afhankelijk van de mogelijkheid om te reageren op incidenten en storingen oplossen kunt gebruiken om de problemen snel te detecteren. Als u problemen en fouten aan de Azure Service Fabric health manager vanuit uw servicecode rapporteren, kunt u standaard voor health monitoring hulpprogramma's die Service Fabric biedt om te controleren of de status.

Er zijn drie manieren dat u de status van de service kan rapporteren:

* Gebruik [partitie](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) of [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objecten.  
  U kunt de `Partition` en `CodePackageActivationContext` objecten voor het rapporteren van de status van de elementen die deel van de huidige context uitmaken. Code die wordt uitgevoerd als onderdeel van een replica kan bijvoorbeeld status rapporteren alleen op die replica en de partitie die hoort bij de toepassing die het is een onderdeel van.
* Gebruik `FabricClient`.   
  U kunt `FabricClient` aan het rapport wordt de status van de servicecode als het cluster niet [beveiligde](service-fabric-cluster-security.md) of als de service wordt uitgevoerd met beheerdersbevoegdheden. De meeste real-world scenario's geen niet-beveiligde clusters gebruiken, of bieden beheerdersbevoegdheden. Met `FabricClient`, kunt u de status rapporteren op elke entiteit die deel uitmaakt van het cluster. In het ideale geval echter moet servicecode alleen rapporten verzenden die gerelateerd zijn aan de eigen status.
* Gebruik de REST API's op het cluster, toepassing, geïmplementeerde toepassing, service, service-pakket, partitie, replica of knooppunt niveaus. Dit kan worden gebruikt voor het rapporteren van de status van binnen een container.

Dit artikel begeleidt u bij een voorbeeld waarin wordt de status van de code van de rapporten. Het voorbeeld ziet ook hoe de hulpprogramma's van Service Fabric kunnen worden gebruikt om de status van de status te controleren. In dit artikel is bedoeld als een korte inleiding over de mogelijkheden van Service Fabric voor statuscontrole. U kunt de reeks uitgebreide artikelen over de status van die met de koppeling aan het einde van dit artikel beginnen lezen voor meer gedetailleerde informatie.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende zijn geïnstalleerd:

* Visual Studio 2015 of Visual Studio 2017
* Service Fabric SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Een lokale veilige dev-cluster maken
* Open PowerShell met beheerdersbevoegdheden en voer de volgende opdrachten uit:

![Opdrachten die laten zien hoe u een veilige dev-cluster maken](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Een toepassing implementeren en de status controleren
1. Open Visual Studio als beheerder.
1. Maak een project met behulp van de **Stateful Service** sjabloon.
   
    ![Een Service Fabric-toepassing maken met de Stateful Service](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
1. Druk op **F5** voor het uitvoeren van de toepassing in de foutopsporingsmodus. De toepassing wordt geïmplementeerd op het lokale cluster.
1. Nadat de toepassing wordt uitgevoerd, met de rechtermuisknop op het pictogram Local Cluster Manager in het systeemvak te plaatsen en selecteert u **lokaal Cluster beheren** in het snelmenu om Service Fabric Explorer te openen.
   
    ![Service Fabric Explorer openen vanuit het systeemvak](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
1. De status van de toepassing moet worden weergegeven zoals in deze afbeelding. Op dit moment moet de toepassing in orde zonder fouten.
   
    ![In orde toepassing in Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
1. U kunt ook de status controleren met behulp van PowerShell. U kunt ```Get-ServiceFabricApplicationHealth``` om te controleren of de status van een toepassing en u kunnen gebruiken ```Get-ServiceFabricServiceHealth``` om te controleren of de status van een service. Het statusrapport voor dezelfde toepassing in PowerShell is in deze afbeelding.
   
    ![In orde toepassing in PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Aangepaste statusgebeurtenissen toevoegen aan uw servicecode
De sjablonen van de Service Fabric-project in Visual Studio bevatten voorbeelden van code. De volgende stappen laten zien hoe kunt u aangepaste statusgebeurtenissen rapporteren vanuit uw servicecode. Deze rapporten wordt automatisch weergegeven in de standard-hulpprogramma's voor de statuscontrole biedt Service Fabric, zoals Service Fabric Explorer, de weergave van de gezondheid van Azure portal en PowerShell.

1. Open de toepassing die u eerder hebt gemaakt in Visual Studio opnieuw of maak een nieuwe toepassing met behulp van de **Stateful Service** Visual Studio-sjabloon.
1. Open het bestand Stateful1.cs en zoek de `myDictionary.TryGetValueAsync` aanroepen in de `RunAsync` methode. U kunt zien dat deze methode retourneert een `result` dat de huidige waarde van de teller bevat omdat de sleutel logica in deze toepassing te houden van een aantal die wordt uitgevoerd. Als dit een echte toepassing zijn, en als het ontbreken van het resultaat van een fout weergegeven, zou u markering waarmee wordt aangegeven dat de gebeurtenis.
1. Als een statusgebeurtenis wanneer het ontbreken van resultaat staat voor een fout wilt melden, voeg de volgende stappen uit.
   
    a. Voeg de `System.Fabric.Health` naamruimte naar het bestand Stateful1.cs.
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    b. Voeg de volgende code na de `myDictionary.TryGetValueAsync` aanroepen
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    We melden replica health omdat deze wordt gerapporteerd door een stateful service. De `HealthInformation` parameter bevat informatie over het probleem met de status die wordt gerapporteerd.
   
    Als u een stateless service hebt gemaakt, gebruikt u de volgende code
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
1. Als uw service wordt uitgevoerd met beheerdersbevoegdheden, of als het cluster niet [beveiligde](service-fabric-cluster-security.md), u kunt ook `FabricClient` tot de status van het rapport zoals wordt weergegeven in de volgende stappen uit.  
   
    a. Maak de `FabricClient` exemplaar na de `var myDictionary` declaratie.
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    b. Voeg de volgende code na de `myDictionary.TryGetValueAsync` aanroepen.
   
    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.Context.PartitionId,
            this.Context.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```
1. Laten we deze fout simuleren en wordt in de status controleprogramma's weergeven. Als u wilt de fout te simuleren, opmerkingen bij de eerste regel in de status voor het melden van code die u eerder hebt toegevoegd. Nadat u bij de eerste regel opmerkingen, wordt de code lijken op het volgende voorbeeld.
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   Deze code wordt het statusrapport geactiveerd telkens wanneer `RunAsync` wordt uitgevoerd. Nadat u de wijziging hebt aangebracht, druk op **F5** de toepassing uit te voeren.
1. Nadat de toepassing wordt uitgevoerd, opent u de Service Fabric Explorer om te controleren of de status van de toepassing. Deze keer ziet Service Fabric Explorer u dat de toepassing niet in orde is. Dit is vanwege de fout die is gerapporteerd vanuit de code die we eerder toegevoegd.
   
    ![Beschadigde toepassing in Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
1. Als u de primaire replica in de structuurweergave van Service Fabric Explorer selecteert, ziet u dat **status** te geeft aan dat een fout. Service Fabric Explorer geeft ook de details van het health-rapport die zijn toegevoegd aan de `HealthInformation` parameter in de code. Hier ziet u de dezelfde statusrapporten in PowerShell en Azure portal.
   
    ![Status van de replica in Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Dit rapport blijft in de health manager totdat deze is vervangen door een ander rapport, of totdat deze replica wordt verwijderd. Omdat we niet ingesteld `TimeToLive` voor deze health-rapport in de `HealthInformation` -object, het rapport verloopt nooit.

Het is raadzaam dat status op de meest gedetailleerde niveau, die in dit geval de replica wordt moet worden gerapporteerd. U kunt ook status rapporteren op `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Tot de status van het rapport op `Application`, `DeployedApplication`, en `DeployedServicePackage`, gebruikt u `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Volgende stappen
* [Gedetailleerde informatie over Service Fabric-status](service-fabric-health-introduction.md)
* [REST-API voor het melden van servicestatus](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [REST-API voor het melden van toepassingsstatus](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

