---
title: Azure Service Bus Geo-noodherstel | Microsoft Docs
description: Het gebruik van de geografische regio's voor failover en herstel na noodgevallen in Azure Service Bus uitvoeren
services: service-bus-messaging
documentationcenter: 
author: christianwolf42
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: sethm
ms.openlocfilehash: 49f2992245d694f85b7b1f1c34339f1445c9d699
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="azure-service-bus-geo-disaster-recovery-preview"></a>Azure Service Bus Geo-noodherstel (preview)

Wanneer regionale gegevenscentra uitvaltijd ondervinden, is het essentieel voor gegevensverwerking blijven gebruiken in een andere regio of datacenter. Als zodanig *Geo-noodherstel* en *Geo-replicatie* zijn de belangrijkste functies die voor elke onderneming. Azure Service Bus ondersteunt zowel Geo-noodherstel als Geo-replicatie op het niveau van de naamruimte. 

De Geo-disaster recovery preview is momenteel alleen beschikbaar in twee gebieden (**Noordelijk Centraal, VS** en **Zuid-centraal VS)**.

## <a name="outages-and-disasters"></a>Storingen en rampen

De [aanbevolen procedures voor de isolatie van toepassingen tegen storingen van de Service Bus en noodsituaties](service-bus-outages-disasters.md) artikel maakt onderscheid tussen 'stroomstoringen' en 'noodsituaties', wat belangrijk is om aan te geven. Een *onderbreking* Azure Service Bus, tijdelijk niet beschikbaar is en een aantal onderdelen van de service, zoals een berichten-store of zelfs het gehele datacenter kunnen beïnvloeden. Echter, nadat het probleem is verholpen, Service Bus weer beschikbaar. Een storing leidt doorgaans niet tot het verlies van berichten of andere gegevens. Een voorbeeld van een dergelijke onderbreking mogelijk een stroomstoring in het datacenter.

Een *noodherstel* is gedefinieerd als de permanente of langere verlies van een Service Bus [schaaleenheid](service-bus-architecture.md#service-bus-scale-units) of datacenter. Het datacenter mogelijk niet mogelijk beschikbaar opnieuw, of voor uren of dagen is mogelijk niet actief. Voorbeelden van dergelijke rampen zijn gestart, die het overspoelen of aardbeving. Een ramp permanente wordt mogelijk het verlies van sommige berichten of andere gegevens. Echter, in de meeste gevallen moet er geen verlies van gegevens en berichten kunnen worden hersteld zodra het datacenter een back-up is.

De Geo-disaster recovery functie van Azure Service Bus is een noodherstel. De concepten en werkstroom beschreven in dit artikel van toepassing op noodherstel scenario's en niet op tijdelijke of tijdelijke storingen.  

## <a name="basic-concepts-and-terms"></a>Basic-concepten en termen

De functie van het herstel na noodgevallen metagegevens noodherstel geïmplementeerd, en is afhankelijk van de primaire en een secundaire disaster recovery naamruimten. Houd er rekening mee dat de Geo-disaster recovery-functie is beschikbaar voor [Premium-naamruimten](service-bus-premium-messaging.md) alleen. U hoeft niet te Breng eventuele wijzigingen van de tekenreeks verbinding als de verbinding is gemaakt met een alias.

De volgende termen worden gebruikt in dit artikel:

-  *Alias*: uw belangrijkste verbindingsreeks.

-  *Primaire en secundaire naamruimte*: Beschrijving van de naamruimten die met de alias overeenkomen. De primaire ' actief ' en ontvangt berichten, de secundaire ' passief ' en ontvangt geen berichten. De metagegevens tussen beide is synchroon, zodat beide berichten zonder codewijzigingen toepassing naadloos kunnen accepteren.

-  *Metagegevens*: uw weergave van objecten in Azure Service Bus. Op dit moment ondersteunen we alleen metagegevens.

-  *Failover*: het proces van het activeren van de secundaire naamruimte. U moet berichten ophalen uit de naamruimte van uw voorheen primaire zodra deze beschikbaar opnieuw en verwijder vervolgens de naamruimte. Voor het maken van een andere failover, voegt u een nieuwe secundaire naamruimte aan de koppeling. Als u gebruiken als de oude primaire naamruimte na een failover wilt, moet u eerst alle bestaande entiteiten verwijderen uit de naamruimte. Zorg ervoor dat alle berichten ontvangen voordat het doet.

## <a name="failover-workflow"></a>De werkstroom

De volgende sectie wordt een overzicht van het hele proces voor het instellen van de eerste failover en hoe ga verder vanaf dat moment.

![1][]

U eerst een primaire en secundaire-naamruimte instellen en een koppeling maken. Deze koppeling geeft u een alias die u kunt verbinding maken. Omdat u een alias gebruiken, kunt u beschikt niet over verbindingsreeksen wijzigen. Alleen nieuwe naamruimten kunnen worden toegevoegd aan uw failover-koppeling. Tot slot moet u bepaalde trigger logica (bijvoorbeeld bepaalde zakelijke logica die wordt gedetecteerd als de naamruimte niet beschikbaar is en de failover start) toevoegen. U kunt controleren voor het gebruik van de naamruimte-beschikbaarheid van de [bericht Bladeren](message-browsing.md) -functionaliteit van Service Bus.

Nadat u de bewaking en noodherstel hebt ingesteld, kunt u de failover-procedure bekijken. Als de trigger een failover initieert, of u de failover handmatig starten, zijn twee stappen vereist:

1. In geval van een andere onderbreking, die u wilt worden opnieuw failover mogelijk is. Daarom moet een tweede passieve naamruimte instellen en bijwerken van de koppeling. 
2. Pull-berichten uit de naamruimte voorheen primaire zodra de nieuwe naamruimte beschikbaar is. Daarna opnieuw gebruiken of verwijder de oude primaire naamruimte.

![2][]

## <a name="set-up-disaster-recovery"></a>Herstel na noodgeval instellen

Deze sectie beschrijft hoe u uw eigen Geo-noodherstel van Service Bus-code voor herstel. Om dit te doen, moet u twee naamruimten in onafhankelijke locaties; bijvoorbeeld, VS Zuid- en Noordelijk Centraal, VS. Het volgende voorbeeld maakt gebruik van Visual Studio 2017.

1.  Maak een nieuwe **Console App(.Net Framework)** project in Visual Studio en een naam geven, bijvoorbeeld **SBGeoDR**.

2.  De volgende NuGet-pakketten installeren:
    1.  Microsoft.IdentityModel.Clients.ActiveDirectory
    2.  Microsoft.Azure.Management.ServiceBus

3. Zorg ervoor dat de versie van het Newtonsoft.Json NuGet-pakket dat u versie 10.0.3 is.

3.  Voeg de volgende `using` instructies toe aan uw code:

    ```csharp
    using System.Threading;
    using Microsoft.Azure.Management.ServiceBus;
    using Microsoft.Azure.Management.ServiceBus.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

4. Wijzig uw `main()` om toe te voegen twee premium-naamruimten:

    ```csharp
    // 1. Create primary namespace (optional).

    var namespaceParams = new SBNamespace()
    {
        Location = "South Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    var namespace1 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, namespaceParams);

    // 2. Create secondary namespace (optional if you already have an empty namespace available).

    var namespaceParams2 = new SBNamespace()
    {
        Location = "North Central US",
        Sku = new SBSku()
        {
            Name = SkuName.Premium,
            Capacity = 1
        }

    };

    // If you re-run this program while namespaces are still paired this operation will fail with a bad request.
    // This is because we block all updates on secondary namespaces once it is paired.

    var namespace2 = client.Namespaces.CreateOrUpdate(resourceGroupName, geoDRSecondaryNS, namespaceParams2);
    ```

5. De koppeling tussen de twee naamruimten inschakelen en de alias die u later gebruiken voor verbinding met uw entiteiten ophalen:

    ```csharp
    // 3. Pair the namespaces to enable DR.

    ArmDisasterRecovery drStatus = client.DisasterRecoveryConfigs.CreateOrUpdate(
        resourceGroupName,
        geoDRPrimaryNS,
        alias,
        new ArmDisasterRecovery { PartnerNamespace = geoDRSecondaryNS });

    // A similar loop can be used to check if other operations (Failover, BreakPairing, Delete) 
    // mentioned below have been successful.
    while (drStatus.ProvisioningState != ProvisioningStateDR.Succeeded)
    {
        Console.WriteLine("Waiting for DR to be set up. Current state: " +
        drStatus.ProvisioningState);
        drStatus = client.DisasterRecoveryConfigs.Get(
        resourceGroupName,
        geoDRPrimaryNS,
        alias);

        Thread.CurrentThread.Join(TimeSpan.FromSeconds(30));
    }
    ```

U hebt twee gekoppelde naamruimten instellen. U kunt nu entiteiten om te zien van de synchronisatie van metagegevens maken. Als u een failover uitvoeren wilt onmiddellijk daarna kun je op enige tijd voor de metagegevens te synchroniseren. U kunt bijvoorbeeld een tijd korte slaapstand toevoegen:

```csharp
client.Topics.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", new SBTopic());
client.Subscriptions.CreateOrUpdate(resourceGroupName, geoDRPrimaryNS, "myTopic", "myTopic-Sub1", new SBSubscription());

// sleeping to allow metadata to sync across primary and secondary
Thread.Sleep(1000 * 60);
```

U kunt op dit moment entiteiten via de portal of via Azure Resource Manager toevoegen en Zie hoe ze synchroniseren. Tenzij uw plan handmatig failover is, moet u een app die u bewaakt de naamruimte van de primaire en failover wordt gestart als deze niet beschikbaar is. 

## <a name="initiate-a-failover"></a>Initieer een failover

De volgende code toont hoe u een failover:

```csharp
// Note that this failover operation is always run against the secondary namespace 
// (because primary might be down at time of failover).

client.DisasterRecoveryConfigs.FailOver(resourceGroupName, geoDRSecondaryNS, alias);
```

Nadat u de failover activeren, een nieuwe naamruimte passieve toevoegen en opnieuw in te stellen de koppeling. De code voor het maken van een nieuwe koppeling wordt weergegeven in de vorige sectie. Bovendien moet u de berichten verwijderen uit de oude primaire naamruimte zodra de failover voltooid is. Zie voor voorbeelden van hoe u berichten ontvangen uit een wachtrij [aan de slag met wachtrijen](service-bus-dotnet-get-started-with-queues.md).

## <a name="how-to-disable-geo-disaster-recovery"></a>Het uitschakelen van Geo-noodherstel

De volgende code toont het uitschakelen van het koppelen van een naamruimte:

```csharp
client.DisasterRecoveryConfigs.BreakPairing(resourceGroupName, geoDRPrimaryNS, alias);
```

De volgende code wordt verwijderd van de alias die u hebt gemaakt:

```csharp
// Delete the DR config (alias).
// Note that this operation must run against the namespace to which the alias is currently pointing.
// If you break the pairing and want to delete the namespaces afterwards, you must delete the alias first.

client.DisasterRecoveryConfigs.Delete(resourceGroupName, geoDRPrimaryNS, alias);
```

## <a name="steps-after-a-failover-failback"></a>Stappen na een failover (failback)

Na een failover, moet u de volgende twee stappen uitvoeren:

1.  Maak een nieuwe passieve secundaire naamruimte. De code wordt weergegeven in een eerder gedeelte.
2.  De resterende berichten verwijderen uit de wachtrij.

## <a name="alias-connection-string-and-test-code"></a>Alias connection string en test code

Als u het failoverproces van testen wilt, kunt u een voorbeeldtoepassing die berichten verstuurd naar de primaire naamruimte met de alias schrijven. Zorg dat u de verbindingsreeks alias van een actieve naamruimte verkrijgen om dit te doen. Er is geen andere interface rechtstreeks de verbindingsreeks ophalen met de huidige preview-versie. De volgende voorbeeldcode wordt verbinding gemaakt voor en na de failover.

```csharp
var accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRPrimaryNS, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;

if(aliasPrimaryConnectionString == null)
{
    accessKeys = client.Namespaces.ListKeys(resourceGroupName, geoDRSecondaryNS, "RootManageSharedAccessKey");
    aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
    aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
}
```

## <a name="next-steps"></a>Volgende stappen

- Zie de Geo-noodherstel [hier naslaginformatie over REST API](/rest/api/servicebus/disasterrecoveryconfigs).
- Voer de Geo-noodherstel [op GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Zie de Geo-noodherstel [voorbeeldtoepassing die u berichten naar een alias verzendt](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Zie voor meer informatie over Service Bus-berichtenservice, de volgende artikelen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [Rest-API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
