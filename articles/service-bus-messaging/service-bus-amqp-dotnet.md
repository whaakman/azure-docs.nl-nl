---
title: Azure Servicebus met .NET en AMQP 1.0 | Microsoft Docs
description: Met behulp van Azure Servicebus van .NET met AMQP
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 96d84b2328ffb83d5c1ea7218b17314a537fa45b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57904555"
---
# <a name="use-service-bus-from-net-with-amqp-10"></a>Gebruik Servicebus van .NET met AMQP 1.0

AMQP 1.0-ondersteuning is beschikbaar in de Service Bus-Pakketversie 2.1 of hoger. U kunt controleren of u de meest recente versie hebt met het downloaden van de Service Bus-bits vanaf [NuGet][NuGet].

## <a name="configure-net-applications-to-use-amqp-10"></a>.NET-toepassingen maken gebruik van AMQP 1.0 configureren

Standaard wordt de Service Bus .NET-clientbibliotheek communiceert met de Service Bus-service met behulp van een toegewezen op basis van SOAP-protocol. Protocol vereist voor het gebruik van AMQP 1.0 in plaats van de standaard expliciete configuratie op de Service Bus-verbindingsreeks, zoals beschreven in de volgende sectie. Dan deze wijziging ongewijzigd toepassingscode blijft wanneer met behulp van AMQP 1.0.

In de huidige release zijn er enkele API-functies worden niet ondersteund wanneer met behulp van AMQP. Deze niet-ondersteunde functies vindt u in de sectie [Gedragsalgoritmen verschillen](#behavioral-differences). Enkele van de geavanceerde configuratie-instellingen ook hebben een andere betekenis wanneer u AMQP.

### <a name="configuration-using-appconfig"></a>Configuratie met behulp van App.config

Het is raadzaam voor toepassingen kunnen gebruikmaken van het App.config-configuratiebestand voor het opslaan van instellingen. U kunt voor Service Bus-toepassingen, App.config gebruiken voor het opslaan van de Service Bus-verbindingsreeks. Een voorbeeld App.config-bestand is als volgt:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

De waarde van de `Microsoft.ServiceBus.ConnectionString` instelling is de Service Bus-verbindingsreeks die wordt gebruikt voor het configureren van de verbinding met Service Bus. De indeling is als volgt:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

Waar `namespace` en `SAS key` , zijn afkomstig van de [Azure-portal] [ Azure portal] wanneer u een Service Bus-naamruimte maakt. Zie voor meer informatie, [een Service Bus-naamruimte maken met de Azure-portal][Create a Service Bus namespace using the Azure portal].

Wanneer u AMQP, de verbindingsreeks met append `;TransportType=Amqp`. Deze notatie Hiermee geeft u de clientbibliotheek zijn om verbinding te maken met Service Bus met behulp van AMQP 1.0.

## <a name="message-serialization"></a>Serialisatie van bericht

Wanneer u het standaard-protocol gebruikt, wordt het standaardgedrag van de serialisatie van de .NET-clientbibliotheek is met de [DataContractSerializer] [ DataContractSerializer] type het serialiseren van een [BrokeredMessage] [ BrokeredMessage] instantie voor het vervoer tussen de clientbibliotheek en de Service Bus-service. Wanneer u het AMQP-transportmodus, de clientbibliotheek maakt gebruik van het AMQP-systeem voor het serialiseren van de [brokered berichten] [ BrokeredMessage] in een AMQP-bericht. Deze serialisatie kan het bericht wordt ontvangen en geïnterpreteerd door een ontvangende toepassing die mogelijk wordt uitgevoerd op een ander platform, bijvoorbeeld: een Java-toepassing die de JMS-API gebruikt voor toegang tot Service Bus.

Wanneer u samenstelt een [BrokeredMessage] [ BrokeredMessage] exemplaar, kunt u een .NET-object opgeven als een parameter aan de constructor om te fungeren als de hoofdtekst van het bericht. Voor objecten die kunnen worden toegewezen aan de AMQP primitieve typen, wordt de instantie geserialiseerd in AMQP-gegevenstypen. Als het object rechtstreeks in een primitief type AMQP; kan niet worden toegewezen dat wil zeggen, een aangepaste type gedefinieerd door de toepassing, wordt het object is geserialiseerd met behulp van de [DataContractSerializer][DataContractSerializer], en de geserialiseerde bytes worden verzonden in een AMQP-gegevens-bericht.

Interoperabiliteit met niet-.NET-clients in het kader, alleen .NET-typen die kunnen worden geserialiseerd rechtstreeks in het AMQP-typen voor de hoofdtekst van het bericht te gebruiken. De volgende tabel worden deze typen en de bijbehorende toewijzen aan de AMQP-systeem.

| Objecttype van .NET-instantie | Toegewezen AMQP-Type | AMQP hoofdtekst sectietype |
| --- | --- | --- |
| bool |booleaans |AMQP-waarde |
| byte |ubyte |AMQP-waarde |
| USHORT |USHORT |AMQP-waarde |
| uint |uint |AMQP-waarde |
| ulong |ulong |AMQP-waarde |
| sbyte |byte |AMQP-waarde |
| korte |korte |AMQP-waarde |
| int |int |AMQP-waarde |
| lang |lang |AMQP-waarde |
| float |float |AMQP-waarde |
| double |double |AMQP-waarde |
| decimal |decimal128 |AMQP-waarde |
| CHAR |CHAR |AMQP-waarde |
| DateTime |tijdstempel |AMQP-waarde |
| Guid |uuid |AMQP-waarde |
| byte[] |binaire bestanden |AMQP-waarde |
| string |string |AMQP-waarde |
| System.Collections.IList |list |AMQP-waarde: opgenomen in de verzameling items kunnen alleen worden die zijn gedefinieerd in deze tabel. |
| System.Array |array |AMQP-waarde: opgenomen in de verzameling items kunnen alleen worden die zijn gedefinieerd in deze tabel. |
| System.Collections.IDictionary |map |AMQP-waarde: opgenomen in de verzameling items kunnen alleen worden die zijn gedefinieerd in deze tabel. Opmerking: alleen tekenreekssleutels worden ondersteund. |
| URI |Tekenreeks die wordt beschreven (Zie de volgende tabel) |AMQP-waarde |
| DateTimeOffset |Lange beschreven (Zie de volgende tabel) |AMQP-waarde |
| TimeSpan |Lange beschreven (Zie hieronder) |AMQP-waarde |
| Streamen |binaire bestanden |AMQP-gegevens (mogelijk meerdere). De gegevenssecties bevatten de onbewerkte bytes lezen uit de Stream-object. |
| Andere Object |binaire bestanden |AMQP-gegevens (mogelijk meerdere). Het geserialiseerde binaire bestand van het object dat gebruikmaakt van de DataContractSerializer of een serializer geleverd door de toepassing bevat. |

| .NET-type | Toegewezen AMQP beschreven Type | Opmerkingen |
| --- | --- | --- |
| URI |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="behavioral-differences"></a>Gedragsalgoritmen verschillen

Er zijn enkele kleine verschillen in het gedrag van de Service Bus .NET API bij met behulp van AMQP, vergeleken met het standaard-protocol:

* De [OperationTimeout] [ OperationTimeout] eigenschap wordt genegeerd.
* `MessageReceiver.Receive(TimeSpan.Zero)` wordt geïmplementeerd als `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* Berichten voltooien door het lock-tokens kan alleen worden uitgevoerd door de ontvangers die in eerste instantie de berichten ontvangen van berichten.

## <a name="control-amqp-protocol-settings"></a>Instellingen voor beheer AMQP-protocol

De [.NET-API's](/dotnet/api/) verschillende instellingen voor het beheren van het gedrag van het AMQP-protocol doorgeven:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: Hiermee bepaalt u het eerste tegoed toegepast op een koppeling. De standaardwaarde is 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: Besturingselementen voor de maximale grootte van de AMQP-frame tijdens de onderhandelingen bij de verbinding aangeboden opnieuw wordt geopend. De standaardwaarde is 65.536 bytes.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: Als overdrachten batchable, bepaalt deze waarde de maximale vertraging voor het verzenden van dispositions. Overgenomen door afzenders/ontvangers standaard. Afzonderlijke afzender/ontvanger kan de standaardwaarde 20 milliseconden is overschrijven.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: Hiermee bepaalt u of AMQP-verbindingen tot stand via een SSL-verbinding gebracht worden. De standaardwaarde is **waar**.

## <a name="next-steps"></a>Volgende stappen

Bent u klaar om meer te weten? Ga naar de volgende koppelingen:

* [Service Bus AMQP-overzicht]
* [AMQP 1.0-protocolhandleiding]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: https://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Service Bus AMQP-overzicht]: service-bus-amqp-overview.md
[AMQP 1.0-protocolhandleiding]: service-bus-amqp-protocol-guide.md

