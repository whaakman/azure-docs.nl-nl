---
title: Service Bus met .NET- en AMQP 1.0 | Microsoft Docs
description: Met behulp van Azure Servicebus in .NET met AMQP
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 332bcb13-e287-4715-99ee-3d7d97396487
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: 0eb68c97ca26a862a79de9ffb83b1fc630ba2af4
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="use-service-bus-from-net-with-amqp-10"></a>Servicebus in .NET gebruiken met AMQP 1.0

AMQP 1.0-ondersteuning is beschikbaar in de Service Bus-Pakketversie 2.1 of hoger. U kunt controleren of u de meest recente versie hebt met het downloaden van de Service Bus-bits van [NuGet][NuGet].

## <a name="configure-net-applications-to-use-amqp-10"></a>.NET-toepassingen kunnen gebruikmaken van AMQP 1.0 configureren

Standaard wordt de Service Bus .NET-clientbibliotheek communiceert met de Service Bus-service via een toegewezen op basis van SOAP-protocol. Voor het gebruik van AMQP 1.0 in plaats van de standaard vereist protocol expliciete configuratie op de Service Bus-verbindingsreeks, zoals beschreven in de volgende sectie. Dan deze wijziging ongewijzigd toepassingscode blijft wanneer met behulp van AMQP 1.0.

In de huidige release zijn er enkele API-functies worden niet ondersteund bij gebruik van AMQP. Deze niet-ondersteunde functies later worden vermeld in de sectie [niet-ondersteunde functies, beperkingen en verschillen gebruikersgedrag](#unsupported-features-restrictions-and-behavioral-differences). Sommige van de geavanceerde configuratie-instellingen ook hebben een andere betekenis wanneer u AMQP.

### <a name="configuration-using-appconfig"></a>Configuratie met behulp van App.config

Het is raadzaam voor toepassingen kunnen gebruikmaken van het App.config-configuratiebestand voor het opslaan van instellingen. U kunt voor Service Bus-toepassingen App.config gebruiken voor het opslaan van de Service Bus-verbindingsreeks. Een voorbeeld App.config-bestand is als volgt:

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

Waar `namespace` en `SAS key` worden opgehaald uit de [Azure-portal] [ Azure portal] wanneer u een Service Bus-naamruimte maken. Zie voor meer informatie [maken van een Service Bus-naamruimte met behulp van de Azure-portal][Create a Service Bus namespace using the Azure portal].

Wanneer u AMQP, de verbindingsreeks met append `;TransportType=Amqp`. Deze notation Hiermee geeft u de clientbibliotheek zijn om verbinding te maken met Service Bus met AMQP 1.0.

## <a name="message-serialization"></a>Bericht-serialisatie

Wanneer u het standaard-protocol gebruikt, wordt het standaardgedrag voor serialisatie van de .NET-clientbibliotheek is met de [DataContractSerializer] [ DataContractSerializer] type voor het serialiseren van een [BrokeredMessage] [ BrokeredMessage] -exemplaar voor transport tussen de clientbibliotheek en de Service Bus-service. Wanneer u de transportmodus AMQP, de clientbibliotheek maakt gebruik van het systeem AMQP-type voor het serialiseren van de [brokered berichten] [ BrokeredMessage] in een AMQP-bericht. Deze serialisatie kan het bericht wordt ontvangen en geïnterpreteerd door een ontvangende toepassing die mogelijk wordt uitgevoerd op een ander platform, bijvoorbeeld een Java-toepassing die gebruikmaakt van de API JMS voor toegang tot de Service Bus.

Wanneer u samenstellen een [BrokeredMessage] [ BrokeredMessage] exemplaar, kunt u een .NET-object opgeven als een parameter voor de constructor voor het fungeren als de hoofdtekst van het bericht. Voor objecten die kunnen worden toegewezen aan de AMQP primitieve typen, wordt de hoofdtekst van het geserialiseerde in AMQP-gegevenstypen. Als het object kan niet rechtstreeks worden toegewezen aan een primitief type AMQP; dat wil zeggen, een aangepast type gedefinieerd door de toepassing, wordt het object wordt geserialiseerd met de [DataContractSerializer][DataContractSerializer], en de geserialiseerde bytes worden verzonden in een AMQP gegevens weergegeven.

Gebruik alleen .NET-typen die kunnen worden geserialiseerd rechtstreeks in het AMQP-typen voor de hoofdtekst van het bericht te bevorderen interoperabiliteit met niet-.NET-clients. De volgende tabel details over deze typen en de bijbehorende toewijzen aan het systeem AMQP-type.

| Objecttype voor .NET-instantie | Toegewezen AMQP-Type | AMQP hoofdtekst sectietype |
| --- | --- | --- |
| BOOL |booleaans |AMQP-waarde |
| byte |ubyte |AMQP-waarde |
| USHORT |USHORT |AMQP-waarde |
| uint |uint |AMQP-waarde |
| ULONG |ULONG |AMQP-waarde |
| SByte |byte |AMQP-waarde |
| korte |korte |AMQP-waarde |
| int |int |AMQP-waarde |
| lang |lang |AMQP-waarde |
| drijvend |drijvend |AMQP-waarde |
| dubbel |dubbel |AMQP-waarde |
| Decimale |decimal128 |AMQP-waarde |
| CHAR |CHAR |AMQP-waarde |
| Datum en tijd |tijdstempel |AMQP-waarde |
| GUID |UUID |AMQP-waarde |
| Byte] |Binaire |AMQP-waarde |
| tekenreeks |tekenreeks |AMQP-waarde |
| System.Collections.IList-implementatie |lijst |AMQP-waarde: de items in de verzameling kunnen alleen worden toepassingen die zijn gedefinieerd in deze tabel. |
| System.Array |matrix |AMQP-waarde: de items in de verzameling kunnen alleen worden toepassingen die zijn gedefinieerd in deze tabel. |
| System.Collections.IDictionary-implementatie |Kaart |AMQP-waarde: de items in de verzameling kunnen alleen worden toepassingen die zijn gedefinieerd in deze tabel. Opmerking: alleen tekenreekssleutels worden ondersteund. |
| URI |Tekenreeks beschreven (Zie de volgende tabel) |AMQP-waarde |
| DateTimeOffset |Lange beschreven (Zie de volgende tabel) |AMQP-waarde |
| TimeSpan |Lange beschreven (Zie hieronder) |AMQP-waarde |
| Stream |Binaire |AMQP-gegevens (mogelijk meerdere). De gegevenssecties bevatten de onbewerkte bytes lezen uit het Stream-object. |
| Andere objecten |Binaire |AMQP-gegevens (mogelijk meerdere). De geserialiseerde binaire bestanden van het object dat gebruikmaakt van de DataContractSerializer of een serialisatiefunctie opgegeven door de toepassing bevat. |

| .NET-type | Toegewezen AMQP beschreven Type | Opmerkingen |
| --- | --- | --- |
| URI |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Niet-ondersteunde functies en beperkingen gebruikersgedrag verschillen

De volgende functies van de Service Bus .NET API worden momenteel niet ondersteund bij gebruik van AMQP:

* Transacties
* Verzenden via overdracht bestemming

Er zijn ook een aantal kleine verschillen in het gedrag van de Service Bus .NET API wanneer u AMQP, vergeleken met het standaardprotocol:

* De [OperationTimeout] [ OperationTimeout] eigenschap wordt genegeerd.
* `MessageReceiver.Receive(TimeSpan.Zero)`wordt geïmplementeerd als `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.
* Berichten voltooien door vergrendeling tokens kan alleen worden uitgevoerd door de ontvangers die in eerste instantie de berichten te ontvangen van berichten.

## <a name="control-amqp-protocol-settings"></a>Beheerinstellingen van het AMQP-protocol

De [.NET API's](/dotnet/api/) verschillende instellingen voor het beheren van het gedrag van het AMQP-protocol beschikbaar:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)**: Hiermee bepaalt u het eerste krediet toegepast op een koppeling. De standaardwaarde is 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)**: besturingselementen die de maximale grootte van de AMQP-frame tijdens de onderhandeling bij de verbinding aangeboden opnieuw wordt geopend. De standaardwaarde is 65.536 bytes.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)**: als overdrachten batchable, deze waarde bepaalt de maximale wachttijd voor het verzenden van dispositions. Overgenomen door afzenders/ontvangers standaard. De standaardwaarde 20 milliseconden is kunt u afzonderlijke afzender/ontvanger overschrijven.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)**: Hiermee bepaalt u of AMQP-verbindingen tot stand via een SSL-verbinding gebracht worden. De standaardwaarde is **true**.

## <a name="next-steps"></a>Volgende stappen

Gereed voor meer informatie? Ga naar de volgende koppelingen:

* [Service Bus AMQP-overzicht]
* [AMQP 1.0-protocolhandleiding]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Service Bus AMQP-overzicht]: service-bus-amqp-overview.md
[AMQP 1.0-protocolhandleiding]: service-bus-amqp-protocol-guide.md

