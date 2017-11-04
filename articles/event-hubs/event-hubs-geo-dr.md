---
title: Azure Event Hubs geo-disaster recovery | Microsoft Docs
description: Het gebruik van de geografische regio's voor failover en herstel na noodgevallen uitvoeren in Azure Event Hubs
services: event-hubs
documentationcenter: 
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: sethm
ms.openlocfilehash: 94c2782b3166fbc65ae755291a82a2a14556b96f
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="azure-event-hubs-geo-disaster-recovery-preview"></a>Azure Event Hubs geo-noodherstel (preview)

Wanneer regionale gegevenscentra uitvaltijd ondervinden, is het essentieel voor gegevensverwerking blijven gebruiken in een andere regio of datacenter. Als zodanig *geo-noodherstel* en *geo-replicatie* zijn de belangrijkste functies die voor elke onderneming. Azure Event Hubs ondersteunt zowel geo-noodherstel als geo-replicatie op het niveau van de naamruimte. 

De geo-disaster recovery functie van Azure Event Hubs is een noodherstel. De concepten en werkstroom beschreven in dit artikel van toepassing op noodherstel scenario's en niet op tijdelijke of tijdelijke storingen.

Zie voor een gedetailleerde bespreking van herstel na noodgevallen in Microsoft Azure [in dit artikel](/azure/architecture/resiliency/disaster-recovery-azure-applications). 

## <a name="terminology"></a>Terminologie

**U koppelt**: de primaire naamruimte wordt aangeduid als *active* en berichten worden ontvangen. De failover-naamruimte is *passieve* en niet ontvangen van berichten. De metagegevens tussen beide is synchroon, zodat beide berichten zonder codewijzigingen toepassing naadloos kunnen accepteren. Tot stand brengen van de configuratie van het herstel na noodgevallen tussen de actieve regio en passieve regio staat bekend als *koppelen* de regio's.

**Alias**: een naam voor een noodherstelconfiguratie die u hebt ingesteld. De alias bevat een stabiele verbindingstekenreeks van de Fully Qualified Domain Name (FQDN). Toepassingen gebruiken deze verbindingsreeks alias verbinding maken met een naamruimte.

**Metagegevens**: verwijst naar een event hub-namen, consumergroepen, partities, doorvoereenheden, entiteiten en eigenschappen die gekoppeld aan de naamruimte zijn.

## <a name="enable-geo-disaster-recovery"></a>Geo-noodherstel inschakelen

U inschakelen Event Hubs-geo-noodherstel binnen 3 stappen: 

1. Maak een geo-koppeling, die een verbindingsreeks alias maakt en metagegevens van live-replicatie biedt. 
2. De bestaande client-verbindingstekenreeksen bijwerken naar de alias die in stap 1 hebt gemaakt.
3. Start de failover: de geo-koppeling is verbroken en de alias verwijst naar de secundaire naamruimte als de nieuwe primaire naamruimte.

De volgende afbeelding ziet deze werkstroom:

![Stroom geo koppelen][1] 

### <a name="step-1-create-a-geo-pairing"></a>Stap 1: Maak een geo-koppeling

Voor het maken van een koppeling tussen twee regio's, moet u een primaire naamruimte en een secundaire naamruimte. Vervolgens maakt u een alias voor de geo-paar vormen. Wanneer de naamruimten die zijn gekoppeld aan een alias, wordt de metagegevens periodiek gerepliceerd in beide naamruimten. 

De volgende code toont hoe u dit doet:

```csharp
ArmDisasterRecovery adr = await client.DisasterRecoveryConfigs.CreateOrUpdateAsync(
                                    config.PrimaryResourceGroupName,
                                    config.PrimaryNamespace,
                                    config.Alias,
                                    new ArmDisasterRecovery(){ PartnerNamespace = config.SecondaryNamespace});
```

### <a name="step-2-update-existing-client-connection-strings"></a>Stap 2: de bestaande client-verbindingstekenreeksen bijwerken

Zodra de geo-koppeling voltooid is, moeten de verbindingsreeksen die naar de primaire naamruimten verwijzen worden bijgewerkt om te verwijzen naar de alias-verbindingsreeks. De verbindingsreeksen verkrijgen zoals in het volgende voorbeeld:

```csharp
var accessKeys = await client.Namespaces.ListKeysAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, "RootManageSharedAccessKey");
var aliasPrimaryConnectionString = accessKeys.AliasPrimaryConnectionString;
var aliasSecondaryConnectionString = accessKeys.AliasSecondaryConnectionString;
```

### <a name="step-3-initiate-a-failover"></a>Stap 3: initieer een failover

Als er een ramp optreedt, of als u besluit te start een failover naar de secundaire naamruimte, start metagegevens en gegevens die binnenkomen in de secundaire naamruimte. Omdat de toepassingen de verbindingsreeksen alias gebruiken, is er geen verdere actie vereist omdat ze lezen van en schrijven naar de event hubs in de secundaire naamruimte voor het automatisch starten. 

De volgende code toont hoe u de failover activeren:

```csharp
await client.DisasterRecoveryConfigs.FailOverAsync(config.SecondaryResourceGroupName,
                                                   config.SecondaryNamespace, config.Alias);
```

Zodra de failover voltooid is en u de gegevens aanwezig is in de primaire naamruimte moet, om op te halen van de gegevens moet u een expliciete verbindingsreeks naar de event hubs in de primaire naamruimte.

### <a name="other-operations-optional"></a>Andere bewerkingen (optioneel)

U kunt ook opsplitsen geo koppelen of verwijderen van een alias, zoals wordt weergegeven in de volgende code. Houd er rekening mee dat als u wilt verwijderen een verbindingsreeks alias, u eerst de geo-paring verbreken moet:

```csharp
// Break pairing
await client.DisasterRecoveryConfigs.BreakPairingAsync(config.PrimaryResourceGroupName,
                                                       config.PrimaryNamespace, config.Alias);

// Delete alias connection string
// Before the alias is deleted, pairing must be broken
await client.DisasterRecoveryConfigs.DeleteAsync(config.PrimaryResourceGroupName,
                                                 config.PrimaryNamespace, config.Alias);
```

## <a name="considerations-for-public-preview"></a>Overwegingen voor de openbare preview

Houd rekening met de volgende overwegingen voor deze release:

1. De geo-noodherstel is alleen beschikbaar in de Noordelijk Centraal, VS en Zuid-centraal VS gebieden. 
2. De functie wordt alleen ondersteund voor gemaakte naamruimten.
3. Voor de preview-versie is alleen metagegevens replicatie ingeschakeld. Werkelijke gegevens worden niet gerepliceerd.
4. Met de preview-versie is er geen kosten voor het inschakelen van de functie. Echter, zowel de primaire als de secundaire naamruimten zal worden kosten in rekening voor de eenheden gereserveerde doorvoer.

## <a name="next-steps"></a>Volgende stappen

* De [op GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) wordt begeleid bij een eenvoudige werkstroom die u maakt een geo-koppeling en start een failover voor een noodherstelscenario.
* De [naslaginformatie over REST API](/rest/api/eventhub/disasterrecoveryconfigs) API's voor het uitvoeren van de Geo noodherstelconfiguratie beschrijft.

Voor meer informatie over Event Hubs gaat u naar de volgende koppelingen:

* Aan de slag met een [Event Hubs-zelfstudie](event-hubs-dotnet-standard-getstarted-send.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
* [Voorbeeldtoepassingen die gebruikmaken van Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/eh-geodr1.png

