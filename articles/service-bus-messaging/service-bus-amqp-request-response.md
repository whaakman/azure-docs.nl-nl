---
title: AMQP 1.0 in Azure Service Bus-bewerkingen op basis van aanvraag-reactie | Microsoft Docs
description: Lijst met Microsoft Azure Service Bus aanvraag/antwoord-bewerkingen.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: spelluru
ms.openlocfilehash: f5d5b8064821dfb1aa6d4e99d0152e364f9a83fe
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700515"
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>AMQP 1.0 in Microsoft Azure Service Bus: aanvraag-reactie-bewerkingen

In dit artikel wordt de lijst met Microsoft Azure Service Bus-aanvraag/antwoord gebaseerde bewerkingen gedefinieerd. Deze informatie is gebaseerd op het concept van de werkende AMQP Management versie 1.0.  
  
Zie voor een gedetailleerde wire-niveau AMQP 1.0-protocol handleiding, waarin wordt uitgelegd hoe Service Bus implementeert en bouwt voort op de technische OASIS AMQP-specificatie, de [AMQP 1.0 in Azure Service Bus en Event Hubs protocol handleiding][amqp 1.0-protocolhandleiding].  
  
## <a name="concepts"></a>Concepten  
  
### <a name="entity-description"></a>Beschrijving van entiteit  

Een beschrijving van de entiteit verwijst naar een Service-Bus [QueueDescription klasse](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [TopicDescription klasse](/dotnet/api/microsoft.servicebus.messaging.topicdescription), of [SubscriptionDescription klasse](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) object.  
  
### <a name="brokered-message"></a>Brokered berichten  

Hiermee geeft u een bericht in Service Bus, die is toegewezen aan een AMQP-bericht. De toewijzing is gedefinieerd in de [Service Bus AMQP-protocolhandleiding](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-to-entity-management-node"></a>Koppelen aan het beheerknooppunt entiteit  

Alle bewerkingen die worden beschreven in dit document volgen een patroon voor aanvraag/antwoord, die zijn gericht op een entiteit en vereisen koppelen aan een knooppunt van het beheer van entiteit.  
  
### <a name="create-link-for-sending-requests"></a>Koppeling voor het verzenden van aanvragen maken  

Hiermee maakt u een koppeling naar het beheerknooppunt voor het verzenden van aanvragen.  
  
```  
requestLink = session.attach(     
role: SENDER,   
    target: { address: "<entity address>/$management" },   
    source: { address: ""<my request link unique address>" }   
)  
  
```  
  
### <a name="create-link-for-receiving-responses"></a>Koppeling voor het ontvangen van antwoorden maken  

Hiermee maakt u een koppeling voor het ontvangen van antwoorden uit het beheerknooppunt.  
  
```  
responseLink = session.attach(    
role: RECEIVER,   
    source: { address: "<entity address>/$management" }   
    target: { address: "<my response link unique address>" }   
)  
  
```  
  
### <a name="transfer-a-request-message"></a>Overdracht een aanvraagbericht  

Draagt een bericht.  
Een transactiestatus kan desgewenst worden toegevoegd voor bewerkingen die ondersteuning biedt voor transactie.

```  
requestLink.sendTransfer(  
        Message(  
                properties: {  
                        message-id: <request id>,  
                        reply-to: "<my response link unique address>"  
                },  
                application-properties: {  
                        "operation" -> "<operation>",  
                }
        ),
        [Optional] State = transactional-state: {
                txn-id: <txn-id>
        }
)
```  
  
### <a name="receive-a-response-message"></a>Ontvangen van een antwoordbericht  

Het antwoordbericht ontvangt van de antwoord-koppeling.  
  
```  
responseMessage = responseLink.receiveTransfer()  
```  
  
Het antwoordbericht heeft de volgende notatie:
  
```  
Message(  
properties: {     
        correlation-id: <request id>  
    },  
    application-properties: {  
            "statusCode" -> <status code>,  
            "statusDescription" -> <status description>,  
           },         
)  
  
```  
  
### <a name="service-bus-entity-address"></a>Service Bus-entiteit adres  

Service Bus-entiteiten moeten als volgt worden opgelost:  
  
|Entiteitstype|Adres|Voorbeeld|  
|-----------------|-------------|-------------|  
|wachtrij|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|onderwerp|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|abonnement|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>Berichtbewerkingen  
  
### <a name="message-renew-lock"></a>Bericht vergrendeling vernieuwen  

De vergrendeling van een bericht is een uitbreiding van de tijd die is opgegeven in de beschrijving van de entiteit.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
 De berichttekst van de aanvraag moet bestaan uit de sectie van een amqp-waarde met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|matrix met uuid|Ja|Bericht vergrendeling tokens te vernieuwen.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt.|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit de sectie van een amqp-waarde met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|verlopen|matrix van tijdstempel|Ja|Lock-token nieuwe verloopdatum voor het bericht overeenkomt met de aanvraag voor vergrendeling tokens.|  
  
### <a name="peek-message"></a>Bericht bekijken  

Geeft weer dat berichten zonder vergrendeling.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|lengte|Ja|Volgnummer uit waarop u wilt beginnen peek.|  
|`message-count`|int|Ja|Maximum aantal berichten te bekijken.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK – heeft meer berichten<br /><br /> 0xcc: geen inhoud – geen berichten meer|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|berichten|lijst met kaarten|Ja|Lijst met berichten waarin elke kaart een bericht vertegenwoordigt.|  
  
De kaart die een bericht moet de volgende items bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|message|matrix van bytes|Ja|AMQP 1.0 wire-bericht dat is gecodeerd.|  
  
### <a name="schedule-message"></a>Schema-bericht  

Berichten plant. Met deze bewerking podporuje transakci.
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|berichten|lijst met kaarten|Ja|Lijst met berichten waarin elke kaart een bericht vertegenwoordigt.|  
  
De kaart die een bericht moet de volgende items bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bericht-id|tekenreeks|Ja|`amqpMessage.Properties.MessageId` Als tekenreeks|  
|sessie-id|tekenreeks|Nee|`amqpMessage.Properties.GroupId as string`|  
|Partitiesleutel|tekenreeks|Nee|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|
|via de partitiesleutel|tekenreeks|Nee|`amqpMessage.MessageAnnotations."x-opt-via-partition-key"`|
|message|matrix van bytes|Ja|AMQP 1.0 wire-bericht dat is gecodeerd.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt.|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|volgnummers|matrix van lange|Ja|Volgnummer van geplande berichten. Volgnummer wordt gebruikt om te annuleren.|  
  
### <a name="cancel-scheduled-message"></a>Gepland bericht annuleren  

Annuleert de geplande berichten.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|volgnummers|matrix van lange|Ja|Volgnummers van geplande berichten om te annuleren.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt.|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|volgnummers|matrix van lange|Ja|Volgnummer van geplande berichten. Volgnummer wordt gebruikt om te annuleren.|  
  
## <a name="session-operations"></a>Sessie-bewerkingen  
  
### <a name="session-renew-lock"></a>Sessie vergrendeling vernieuwen  

De vergrendeling van een bericht is een uitbreiding van de tijd die is opgegeven in de beschrijving van de entiteit.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|sessie-id|tekenreeks|Ja|Sessie-ID.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK – heeft meer berichten<br /><br /> 0xcc: geen inhoud – geen berichten meer|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|verlooptijd|tijdstempel|Ja|Nieuwe vervaldatum.|  
  
### <a name="peek-session-message"></a>Sessie bericht bekijken  

Geeft weer dat berichten voor sessie zonder de vergrendeling.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|uit de volgnummer|lengte|Ja|Volgnummer uit waarop u wilt beginnen peek.|  
|aantal berichten|int|Ja|Maximum aantal berichten te bekijken.|  
|sessie-id|tekenreeks|Ja|Sessie-ID.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK – heeft meer berichten<br /><br /> 0xcc: geen inhoud – geen berichten meer|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|berichten|lijst met kaarten|Ja|Lijst met berichten waarin elke kaart een bericht vertegenwoordigt.|  
  
 De kaart die een bericht moet de volgende items bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|message|matrix van bytes|Ja|AMQP 1.0 wire-bericht dat is gecodeerd.|  
  
### <a name="set-session-state"></a>Set-sessiestatus  

Hiermee stelt u de status van een sessie.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|sessie-id|tekenreeks|Ja|Sessie-ID.|  
|sessiestatus|matrix van bytes|Ja|Ondoorzichtige binaire gegevens.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
### <a name="get-session-state"></a>Get-sessiestatus  

Hiermee haalt u de status van een sessie.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|sessie-id|tekenreeks|Ja|Sessie-ID.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|sessiestatus|matrix van bytes|Ja|Ondoorzichtige binaire gegevens.|  
  
### <a name="enumerate-sessions"></a>Het inventariseren van sessies  

Sessies op een Berichtentiteit inventariseren.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|Laatst bijgewerkt: tijd|tijdstempel|Ja|Gefilterd op alleen de sessies bijgewerkt na een bepaald moment.|  
|overslaan|int|Ja|Een aantal sessies overslaan.|  
|Boven|int|Ja|Maximum aantal sessies.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK – heeft meer berichten<br /><br /> 0xcc: geen inhoud – geen berichten meer|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|overslaan|int|Ja|Aantal overgeslagen sessies als statuscode 200.|  
|sessies-id 's|matrix van tekenreeksen|Ja|Matrix van sessie-id's als statuscode 200.|  
  
## <a name="rule-operations"></a>Regel-bewerkingen  
  
### <a name="add-rule"></a>Regel toevoegen  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|naam van regel|tekenreeks|Ja|Regelnaam, niet met inbegrip van namen abonnement en het onderwerp.|  
|Beschrijving van regel|Kaart|Ja|Regelbeschrijving zoals opgegeven in de volgende sectie.|  
  
De **Regelbeschrijving** kaart omvat de volgende items, waarbij **sql-filter** en **correlatiefilter** sluiten elkaar wederzijds uit:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|SQL-filter|Kaart|Ja|`sql-filter`, zoals opgegeven in de volgende sectie.|  
|correlatie-filter|Kaart|Ja|`correlation-filter`, zoals opgegeven in de volgende sectie.|  
|SQL-regelactie|Kaart|Ja|`sql-rule-action`, zoals opgegeven in de volgende sectie.|  
  
De toewijzing van de sql-filter moet de volgende items bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|expressie|tekenreeks|Ja|SQL-filter-expressie.|  
  
De **correlatiefilter** kaart moet ten minste één van de volgende items bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|correlatie-id|tekenreeks|Nee||  
|bericht-id|tekenreeks|Nee||  
|tot|tekenreeks|Nee||  
|Antwoordadres|tekenreeks|Nee||  
|label|tekenreeks|Nee||  
|sessie-id|tekenreeks|Nee||  
|antwoord-naar-sessie-id|tekenreeks|Nee||  
|inhoudstype|tekenreeks|Nee||  
|properties|Kaart|Nee|Toegewezen aan Servicebus [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
De **sql regelactie** kaart moet de volgende items bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|expressie|tekenreeks|Ja|SQL-expressie voor actie.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
### <a name="remove-rule"></a>Regel verwijderen  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|naam van regel|tekenreeks|Ja|Regelnaam, niet met inbegrip van namen abonnement en het onderwerp.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
### <a name="get-rules"></a>Regels ophalen

#### <a name="request"></a>Aanvraag

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:

|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:enumerate-rules`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  

De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|Boven|int|Ja|Het aantal regels om op te halen op de pagina.|  
|overslaan|int|Ja|Het aantal regels om over te slaan. Definieert de startIndex (+ 1) in de lijst met regels. | 

#### <a name="response"></a>Antwoord

Het antwoordbericht bevat de volgende eigenschappen:

|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt|  
|regels| matrix van kaart|Ja|Matrix van regels. Elke regel wordt vertegenwoordigd door een kaart.|

Elke kaart vermelding in de matrix bevat de volgende eigenschappen:

|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|Beschrijving van regel|matrix met objecten beschreven|Ja|`com.microsoft:rule-description:list` met AMQP beschreven code 0x0000013700000004| 

`com.microsoft.rule-description:list` is een matrix met objecten beschreven. De matrix omvat het volgende:

|Index|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
| 0 | matrix met objecten beschreven | Ja | `filter` zoals hieronder is opgegeven. |
| 1 | matrix van beschreven object | Ja | `ruleAction` zoals hieronder is opgegeven. |
| 2 | tekenreeks | Ja | naam van de regel. |

`filter` kan zijn van een van de volgende typen:

| De naam van de descriptor | Descriptor code | Waarde |
| --- | --- | ---|
| `com.microsoft:sql-filter:list` | 0x000001370000006 | SQL-filter |
| `com.microsoft:correlation-filter:list` | 0x000001370000009 | Correlatiefilter |
| `com.microsoft:true-filter:list` | 0x000001370000007 | De waarde True filter voor 1 = 1 |
| `com.microsoft:false-filter:list` | 0x000001370000008 | De waarde False filter voor 1 = 0 |

`com.microsoft:sql-filter:list` is een beschreven matrix, waaronder:

|Index|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
| 0 | tekenreeks | Ja | SQL-Filter-expressie |

`com.microsoft:correlation-filter:list` is een beschreven matrix, waaronder:

|Index (indien aanwezig)|Waardetype|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
| 0 | tekenreeks | Correlatie-id |
| 1 | tekenreeks | Bericht-id |
| 2 | tekenreeks | Handeling |
| 3 | tekenreeks | Antwoorden op |
| 4 | tekenreeks | Label |
| 5 | tekenreeks | Sessie-id |
| 6 | tekenreeks | Id antwoord op sessie|
| 7 | tekenreeks | Inhoudstype |
| 8 | Kaart | Overzicht van de toepassing gedefinieerde eigenschappen |

`ruleAction` kan bestaan uit een van de volgende typen:

| De naam van de descriptor | Descriptor code | Waarde |
| --- | --- | ---|
| `com.microsoft:empty-rule-action:list` | 0x0000013700000005 | Lege regelactie - er is geen regelactie aanwezig |
| `com.microsoft:sql-rule-action:list` | 0x0000013700000006 | SQL-regelactie |

`com.microsoft:sql-rule-action:list` is een matrix met beschreven objecten waarvan eerste vermelding is een tekenreeks die de SQL-regelactie expressie bevat.

## <a name="deferred-message-operations"></a>Een uitgesteld berichtbewerkingen  
  
### <a name="receive-by-sequence-number"></a>Ontvangen door volgnummer  

Uitgestelde berichten door volgnummer ontvangt.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|volgnummers|matrix van lange|Ja|Volgnummers.|  
|ontvanger-vereffenen-modus|ubyte|Ja|**Ontvanger vereffenen** modus zoals opgegeven in AMQP core v1.0.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|berichten|lijst met kaarten|Ja|Lijst met berichten waarin elke kaart een bericht aangeeft.|  
  
De kaart die een bericht moet de volgende items bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|LOCK-token|UUID|Ja|LOCK-token als `receiver-settle-mode` is 1.|  
|message|matrix van bytes|Ja|AMQP 1.0 wire-bericht dat is gecodeerd.|  
  
### <a name="update-disposition-status"></a>De status van de bestemming bijwerken  

De status van de toestand van uitgestelde berichten wordt bijgewerkt. Met deze bewerking biedt ondersteuning voor transacties.
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Nee|Time-out van de bewerking server in milliseconden.|  
  
De berichttekst van de aanvraag moet bestaan uit een **amqp-waarde** een sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|Disposition-status|tekenreeks|Ja|completed<br /><br /> afgebroken<br /><br /> Onderbroken|  
|LOCK-tokens|matrix met uuid|Ja|Bericht vergrendeling tokens toestand status bij te werken.|  
|onbestelbare berichten-reden|tekenreeks|Nee|Kan worden ingesteld als de status van de bestemming is ingesteld op **onderbroken**.|  
|Beschrijving van onbestelbare berichten|tekenreeks|Nee|Kan worden ingesteld als de status van de bestemming is ingesteld op **onderbroken**.|  
|Eigenschappen te wijzigen|Kaart|Nee|Lijst met Service Bus brokered-berichteigenschappen om te wijzigen.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud van waarde|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-responscode [RFC2616]<br /><br /> 200: OK-bewerking is geslaagd, anders mislukt|  
|Statusbeschrijving|tekenreeks|Nee|Beschrijving van de status.|

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het AMQP- en Service Bus, gaat u naar de volgende koppelingen:

* [Service Bus AMQP-overzicht]
* [AMQP 1.0-protocolhandleiding]
* [AMQP in WindowsServer-Servicebus]

[Service Bus AMQP-overzicht]: service-bus-amqp-overview.md
[AMQP 1.0-protocolhandleiding]: service-bus-amqp-protocol-guide.md
[AMQP in WindowsServer-Servicebus]: https://docs.microsoft.com/previous-versions/service-bus-archive/dn282144(v=azure.100)