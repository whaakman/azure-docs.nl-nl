---
title: Rapporteren en controleer de status van Azure Service Fabric | Microsoft Docs
description: Informatie over het statusrapporten verzenden vanuit uw servicecode en het controleren van de status van uw service met behulp van de health-controleprogramma die Azure Service Fabric bevat.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: mfussell
editor: 
ms.assetid: 7c712c22-d333-44bc-b837-d0b3603d9da8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: dekapur
ms.openlocfilehash: 83981d5bec14c06c509f1a8a4153dc23298f5ce0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="report-and-check-service-health"></a>Servicestatus rapporteren en controleren
Wanneer uw services problemen ondervindt, is de mogelijkheid om te reageren op en oplossen van incidenten en storingen afhankelijk van de mogelijkheid voor het detecteren van de problemen snel. Als u problemen en fouten aan de Azure Service Fabric health manager vanuit uw servicecode, kunt u standaard voor health monitoring hulpprogramma's die Service Fabric bevat om de health-status te controleren.

Er zijn drie manieren dat u de status van de service kan rapporteren:

* Gebruik [partitie](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) of [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) objecten.  
  U kunt de `Partition` en `CodePackageActivationContext` objecten voor het rapporteren van de status van de elementen die deel van de huidige context uitmaken. Code die wordt uitgevoerd als onderdeel van een replica kan bijvoorbeeld health rapporteren alleen op die replica en de partitie die hoort bij de toepassing die deel van uitmaakt.
* Gebruik `FabricClient`.   
  U kunt `FabricClient` aan het rapport de status van de code als het cluster niet [beveiligde](service-fabric-cluster-security.md) of als de service wordt uitgevoerd met beheerdersbevoegdheden. De meeste real-world scenario's geen niet-beveiligde clusters gebruiken of bieden beheerdersbevoegdheden. Met `FabricClient`, u kunt een entiteit die deel uitmaakt van het cluster health rapporteren. In het ideale geval echter moet servicecode alleen rapporten verzenden die gerelateerd zijn aan een eigen health.
* Gebruik de REST API's op het cluster, toepassing, geïmplementeerde toepassingen, service, servicepakket, partitie, replica of knooppunt niveaus. Dit kan worden gebruikt voor het rapporteren van de status van binnen een container.

Dit artikel begeleidt u bij een voorbeeld waarin de status van de code van de rapporten. Het voorbeeld ziet ook hoe de hulpprogramma's van Service Fabric kunnen worden gebruikt om de health-status te controleren. In dit artikel is bedoeld als een korte inleiding in de controlefuncties van Service Fabric. U kunt de reeks diepgaande artikelen over health die met de koppeling aan het einde van dit artikel beginnen lezen voor meer gedetailleerde informatie.

## <a name="prerequisites"></a>Vereisten
Hiervoor hebt u het volgende zijn geïnstalleerd:

* Visual Studio 2015 of Visual Studio 2017
* Service Fabric SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Een lokale veilige dev-cluster maken
* Open PowerShell met beheerdersbevoegdheden en voer de volgende opdrachten:

![Opdrachten die laten zien hoe u een beveiligde dev-cluster maken](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Een toepassing implementeren en controleren van de status
1. Open Visual Studio als beheerder.
2. Een project maken met behulp van de **Stateful Service** sjabloon.
   
    ![Maken van een Service Fabric-toepassing met Stateful Service](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
3. Druk op **F5** voor het uitvoeren van de toepassing in de foutopsporingsmodus. De toepassing wordt geïmplementeerd op het lokale cluster.
4. Nadat de toepassing wordt uitgevoerd, met de rechtermuisknop op het pictogram Lokaal Clusterbeheer in het systeemvak en selecteert u **lokaal Cluster beheren** in het snelmenu om Service Fabric Explorer te openen.
   
    ![Service Fabric Explorer openen vanuit het systeemvak](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
5. De toepassingsstatus moet worden weergegeven zoals in deze afbeelding. Op dit moment moet de toepassing zonder fouten in orde.
   
    ![In orde toepassing in Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
6. U kunt ook de status controleren met behulp van PowerShell. U kunt ```Get-ServiceFabricApplicationHealth``` om te controleren van de status van een toepassing, en u kunnen gebruiken ```Get-ServiceFabricServiceHealth``` om te controleren van de status van een service. Het statusrapport voor dezelfde toepassing in PowerShell is in deze afbeelding.
   
    ![In orde toepassing in PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Aangepaste health gebeurtenissen toevoegen aan uw servicecode
De sjablonen voor de Service Fabric-project in Visual Studio bevatten voorbeeldcode. De volgende stappen laten zien hoe u aangepaste health gebeurtenissen vanuit uw servicecode kan rapporteren. Deze rapporten weergegeven automatisch in de standaard hulpprogramma's voor statuscontrole biedt Service Fabric, zoals Service Fabric Explorer, Azure portal health weergeven en PowerShell.

1. Open de toepassing die u eerder hebt gemaakt in Visual Studio of een nieuwe toepassing maken met behulp van de **Stateful Service** Visual Studio-sjabloon.
2. Open het bestand Stateful1.cs en zoek de `myDictionary.TryGetValueAsync` -aanroep in de `RunAsync` methode. U ziet dat deze methode retourneert een `result` die bevat de huidige waarde van de teller omdat de sleutel logica in deze toepassing is een aantal actief houden. Als dit een echte toepassing zijn, en als het ontbreken van het resultaat van een fout weergegeven, wilt u dat de gebeurtenis vlag.
3. Toevoegen de volgende stappen uit om het rapport een statusgebeurtenis wanneer het ontbreken van resultaat duidt op een fout.
   
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
    We gerapporteerd replica health omdat deze wordt gerapporteerd van een stateful service. De `HealthInformation` parameter wordt opgeslagen informatie over het statusprobleem met de wordt gerapporteerd.
   
    Als u een stateless service hebt gemaakt, gebruikt u de volgende code
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
4. Als uw service wordt uitgevoerd met beheerdersbevoegdheden, of als het cluster niet [beveiligde](service-fabric-cluster-security.md), u kunt ook `FabricClient` voor de gezondheid van het rapport zoals weergegeven in de volgende stappen uit.  
   
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
5. Laten we simuleren van deze fout en wordt weergegeven in de health-controleprogramma weergegeven. De fout moet de eerste regel in de health reporting code die u eerder hebt toegevoegd commentaar simuleren. Nadat u de eerste regel uitcommentariëren, wordt de code eruit als in het volgende voorbeeld.
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   Deze code wordt het statusrapport geactiveerd telkens `RunAsync` wordt uitgevoerd. Nadat u de wijziging aanbrengt, drukt u op **F5** de toepassing uit te voeren.
6. Nadat de toepassing wordt uitgevoerd, opent u de Service Fabric Explorer om te controleren van de status van de toepassing. Deze tijd ziet Service Fabric Explorer u dat de toepassing niet in orde is. Dit is vanwege de fout die is gerapporteerd door de code dat we eerder toegevoegd.
   
    ![Slecht toepassingstype in Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
7. Als u de primaire replica in de structuurweergave van Service Fabric Explorer selecteert, ziet u dat **status** te duidt op een fout. Service Fabric Explorer geeft ook de health-rapportgegevens die zijn toegevoegd aan de `HealthInformation` parameter in de code. Hier ziet u de dezelfde statusrapporten in PowerShell en de Azure-portal.
   
    ![Status van de replica in Service Fabric Explorer](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Dit rapport blijft in de health manager totdat deze is vervangen door een ander rapport of totdat deze replica wordt verwijderd. Omdat we niet hebt ingesteld `TimeToLive` voor deze health-rapport in de `HealthInformation` object, het rapport verloopt nooit.

Het is raadzaam dat status op het meest gedetailleerd niveau, in dit geval de replica wordt moet worden gerapporteerd. U kunt ook rapporteren health `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Voor de gezondheid van het rapport op `Application`, `DeployedApplication`, en `DeployedServicePackage`, gebruik `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Volgende stappen
* [Deep dive op Service Fabric-status](service-fabric-health-introduction.md)
* [REST-API voor de status van de service reporting](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [REST-API voor het melden van toepassingsstatus](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

