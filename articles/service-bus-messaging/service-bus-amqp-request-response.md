---
title: AMQP 1.0 in Azure Service Bus-bewerkingen op basis van aanvraag-antwoord | Microsoft Docs
description: Lijst met Microsoft Azure Service Bus-aanvraag/antwoord-bewerkingen.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: sethm
ms.openlocfilehash: 847fe0c08d442388cfa506042272bb358058cb4c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32194685"
---
# <a name="amqp-10-in-microsoft-azure-service-bus-request-response-based-operations"></a>AMQP 1.0 in Microsoft Azure Service Bus: aanvraag-antwoord-bewerkingen

In dit artikel definieert de lijst met Microsoft Azure Service Bus-aanvraag/antwoord-bewerkingen. Deze informatie is gebaseerd op het concept van de werkende AMQP Management versie 1.0.  
  
Zie voor een gedetailleerde wire-niveau AMQP 1.0-protocol handleiding, waarin wordt uitgelegd hoe Service Bus implementeert en bouwt voort op de technische OASIS AMQP-specificatie, de [AMQP 1.0 in Azure Service Bus en Event Hubs protocol handleiding][AMQP 1.0-protocolhandleiding].  
  
## <a name="concepts"></a>Concepten  
  
### <a name="entity-description"></a>De beschrijving van entiteit  

Een beschrijving van de entiteit verwijst naar een Service-Bus [QueueDescription klasse](/dotnet/api/microsoft.servicebus.messaging.queuedescription), [TopicDescription klasse](/dotnet/api/microsoft.servicebus.messaging.topicdescription), of [SubscriptionDescription klasse](/dotnet/api/microsoft.servicebus.messaging.subscriptiondescription) object.  
  
### <a name="brokered-message"></a>Brokered berichten  

Hiermee geeft u een bericht in Service Bus, die is toegewezen aan een AMQP-bericht. De toewijzing is gedefinieerd in de [Service Bus AMQP-protocol handleiding](service-bus-amqp-protocol-guide.md).  
  
## <a name="attach-to-entity-management-node"></a>Koppelen aan knooppunt entiteit  

Een aanvraag/antwoord-patroon volgen alle bewerkingen die in dit document beschreven, zijn gericht op een entiteit en vereisen koppelen aan een knooppunt van het beheer van entiteit.  
  
### <a name="create-link-for-sending-requests"></a>Koppeling voor het verzenden van aanvragen maken  

Maakt een koppeling naar het knooppunt voor het verzenden van aanvragen.  
  
```  
requestLink = session.attach(     
role: SENDER,   
    target: { address: "<entity address>/$management" },   
    source: { address: ""<my request link unique address>" }   
)  
  
```  
  
### <a name="create-link-for-receiving-responses"></a>Koppeling voor het ontvangen van antwoorden maken  

Maakt een koppeling voor het ontvangen van reacties van het beheerknooppunt.  
  
```  
responseLink = session.attach(    
role: RECEIVER,   
    source: { address: "<entity address>/$management" }   
    target: { address: "<my response link unique address>" }   
)  
  
```  
  
### <a name="transfer-a-request-message"></a>Een aanvraagbericht overdragen  

Een aanvraagbericht overgedragen.  
Een transactie-status kan eventueel worden toegevoegd voor bewerkingen die ondersteuning biedt voor transactie.

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
  
### <a name="receive-a-response-message"></a>Een antwoordbericht ontvangen  

Het antwoordbericht ontvangt van de koppeling antwoord.  
  
```  
responseMessage = responseLink.receiveTransfer()  
```  
  
Het antwoordbericht is de volgende notatie:
  
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
  
### <a name="service-bus-entity-address"></a>Adres van Service Bus-entiteit  

Service Bus-entiteiten moeten worden aangepakt als volgt:  
  
|Entiteitstype|Adres|Voorbeeld|  
|-----------------|-------------|-------------|  
|Wachtrij|`<queue_name>`|`“myQueue”`<br /><br /> `“site1/myQueue”`|  
|Onderwerp|`<topic_name>`|`“myTopic”`<br /><br /> `“site2/page1/myQueue”`|  
|abonnement|`<topic_name>/Subscriptions/<subscription_name>`|`“myTopic/Subscriptions/MySub”`|  
  
## <a name="message-operations"></a>Berichtbewerkingen  
  
### <a name="message-renew-lock"></a>Bericht vernieuwen vergrendelen  

Breidt de vergrendeling van een bericht op het moment dat is opgegeven in de beschrijving van de entiteit.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:renew-lock`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
 De berichttekst voor de aanvraag moet bestaan uit de sectie van een amqp-waarde met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|`lock-tokens`|matrix van uuid|Ja|Bericht vergrendeling tokens te vernieuwen.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt.|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit de sectie van een amqp-waarde met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|accountwachtwoorden|matrix van tijdstempel|Ja|Vergrendeling token nieuwe verloopdatum voor het bericht overeenkomt met het aanvragen van tokens vergrendelen.|  
  
### <a name="peek-message"></a>Bericht  

Geeft weer berichten zonder de vergrendeling.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|`from-sequence-number`|lang|Ja|Het volgnummer van waaruit peek gestart.|  
|`message-count`|int|Ja|Maximum aantal berichten te inspecteren.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – heeft geen berichten meer<br /><br /> 0xcc: Er is geen inhoud – geen berichten meer|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|berichten|lijst met maps|Ja|Lijst met berichten waarin elke kaart een bericht vertegenwoordigt.|  
  
De kaart die vertegenwoordigt een bericht moet de volgende vermeldingen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|message|matrix van byte|Ja|AMQP 1.0 wire-bericht dat is gecodeerd.|  
  
### <a name="schedule-message"></a>Schema-bericht  

Hiermee plant u berichten. Deze bewerking ondersteunt transactie.
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:schedule-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|berichten|lijst met maps|Ja|Lijst met berichten waarin elke kaart een bericht vertegenwoordigt.|  
  
De kaart die vertegenwoordigt een bericht moet de volgende vermeldingen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bericht-id|tekenreeks|Ja|`amqpMessage.Properties.MessageId` Als tekenreeks|  
|sessie-id|tekenreeks|Nee|`amqpMessage.Properties.GroupId as string`|  
|Partitiesleutel|tekenreeks|Nee|`amqpMessage.MessageAnnotations.”x-opt-partition-key"`|
|via de partitiesleutel|tekenreeks|Nee|`amqpMessage.MessageAnnotations."x-opt-via-partition-key"`|
|message|matrix van byte|Ja|AMQP 1.0 wire-bericht dat is gecodeerd.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt.|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|volgnummers|matrix van lange|Ja|Het volgnummer van de geplande berichten. Volgnummer wordt gebruikt om te annuleren.|  
  
### <a name="cancel-scheduled-message"></a>Geplande bericht annuleren  

Annuleert de geplande berichten.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:cancel-scheduled-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|volgnummers|matrix van lange|Ja|Volgnummers van geplande berichten te annuleren.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt.|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|volgnummers|matrix van lange|Ja|Het volgnummer van de geplande berichten. Volgnummer wordt gebruikt om te annuleren.|  
  
## <a name="session-operations"></a>Sessie-bewerkingen  
  
### <a name="session-renew-lock"></a>Sessie vernieuwen vergrendelen  

Breidt de vergrendeling van een bericht op het moment dat is opgegeven in de beschrijving van de entiteit.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:renew-session-lock`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|sessie-id|tekenreeks|Ja|Sessie-ID.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – heeft geen berichten meer<br /><br /> 0xcc: Er is geen inhoud – geen berichten meer|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|verlooptijd|tijdstempel|Ja|Nieuwe verlopen.|  
  
### <a name="peek-session-message"></a>Sessie-bericht  

Geeft weer berichten voor sessie zonder de vergrendeling.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|uit de volgnummer|lang|Ja|Het volgnummer van waaruit peek gestart.|  
|aantal berichten|int|Ja|Maximum aantal berichten te inspecteren.|  
|sessie-id|tekenreeks|Ja|Sessie-ID.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – heeft geen berichten meer<br /><br /> 0xcc: Er is geen inhoud – geen berichten meer|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een map met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|berichten|lijst met maps|Ja|Lijst met berichten waarin elke kaart een bericht vertegenwoordigt.|  
  
 De kaart die vertegenwoordigt een bericht moet de volgende vermeldingen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|message|matrix van byte|Ja|AMQP 1.0 wire-bericht dat is gecodeerd.|  
  
### <a name="set-session-state"></a>Set-sessiestatus  

Hiermee stelt u de status van een sessie.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:peek-message`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|sessie-id|tekenreeks|Ja|Sessie-ID.|  
|sessiestatus|bytematrix|Ja|Ondoorzichtige binaire gegevens.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
### <a name="get-session-state"></a>Get-sessiestatus  

Hiermee haalt u de status van een sessie.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:get-session-state`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|sessie-id|tekenreeks|Ja|Sessie-ID.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|sessiestatus|bytematrix|Ja|Ondoorzichtige binaire gegevens.|  
  
### <a name="enumerate-sessions"></a>Sessies opsommen  

Sessies op een Berichtentiteit inventariseren.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:get-message-sessions`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|laatste-bijgewerkt-time|tijdstempel|Ja|Alleen sessies bijgewerkt na een bepaald moment gefilterd.|  
|Overslaan|int|Ja|Een aantal sessies overslaan.|  
|Boven|int|Ja|Maximum aantal sessies.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – heeft geen berichten meer<br /><br /> 0xcc: Er is geen inhoud – geen berichten meer|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|Overslaan|int|Ja|Aantal overgeslagen sessies als statuscode 200.|  
|sessies-id 's|Matrix van tekenreeksen|Ja|Matrix van sessie-id's als statuscode 200.|  
  
## <a name="rule-operations"></a>Bewerkingen voor regel  
  
### <a name="add-rule"></a>Regel toevoegen  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:add-rule`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|naam van regel|tekenreeks|Ja|Regelnaam, met uitzondering van namen abonnement en het onderwerp.|  
|Beschrijving van regel|Kaart|Ja|Regelbeschrijving zoals opgegeven in de volgende sectie.|  
  
De **beschrijving van regel** kaart moet de volgende vermeldingen bevatten waarin **sql-filter** en **correlatie-filter** sluiten elkaar wederzijds uit:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|SQL-filter|Kaart|Ja|`sql-filter`, zoals opgegeven in de volgende sectie.|  
|correlatie-filter|Kaart|Ja|`correlation-filter`, zoals opgegeven in de volgende sectie.|  
|SQL-regelactie|Kaart|Ja|`sql-rule-action`, zoals opgegeven in de volgende sectie.|  
  
De kaart sql-filter moet de volgende vermeldingen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|expressie|tekenreeks|Ja|De filterexpressie SQL.|  
  
De **correlatie-filter** kaart moet ten minste één van de volgende vermeldingen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|correlatie-id|tekenreeks|Nee||  
|bericht-id|tekenreeks|Nee||  
|tot|tekenreeks|Nee||  
|antwoord aan|tekenreeks|Nee||  
|Label|tekenreeks|Nee||  
|sessie-id|tekenreeks|Nee||  
|antwoord-naar-sessie-id|tekenreeks|Nee||  
|type inhoud|tekenreeks|Nee||  
|properties|Kaart|Nee|Is toegewezen aan Servicebus [BrokeredMessage.Properties](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Properties).|  
  
De **sql regelactie** kaart vergezeld gaan van de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|expressie|tekenreeks|Ja|SQL-expressie in te grijpen.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
### <a name="remove-rule"></a>Regel verwijderen  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:remove-rule`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|naam van regel|tekenreeks|Ja|Regelnaam, met uitzondering van namen abonnement en het onderwerp.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
### <a name="get-rules"></a>-Regels ophalen

#### <a name="request"></a>Aanvraag

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:

|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:enumerate-rules`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  

De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|Boven|int|Ja|Het aantal regels op te halen op de pagina.|  
|Overslaan|int|Ja|Het aantal regels over te slaan. De startIndex (+ 1) definieert op de lijst met regels. | 

#### <a name="response"></a>Antwoord

Het antwoordbericht bevat de volgende eigenschappen:

|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt|  
|regels| matrix van kaart|Ja|Matrix van regels. Elke regel wordt vertegenwoordigd door een kaart.|

Elke toewijzingsvermelding in de matrix bevat de volgende eigenschappen:

|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|Beschrijving van regel|matrix van beschreven objecten|Ja|`com.microsoft:rule-description:list` met AMQP beschreven code 0x0000013700000004| 

`com.microsoft.rule-description:list` is een matrix met objecten beschreven. De matrix omvat het volgende:

|Index|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
| 0 | matrix van beschreven objecten | Ja | `filter` zoals hieronder is opgegeven. |
| 1 | matrix van beschreven object | Ja | `ruleAction` zoals hieronder is opgegeven. |
| 2 | tekenreeks | Ja | De naam van de regel. |

`filter` Er zijn een van de volgende typen:

| De naam van de descriptor | Descriptor-code | Waarde |
| --- | --- | ---|
| `com.microsoft:sql-filter:list` | 0x000001370000006 | SQL-filter |
| `com.microsoft:correlation-filter:list` | 0x000001370000009 | Correlatie-filter |
| `com.microsoft:true-filter:list` | 0x000001370000007 | De waarde True filter die 1 = 1 |
| `com.microsoft:false-filter:list` | 0x000001370000008 | ONWAAR filter die 1 = 0 vertegenwoordigt |

`com.microsoft:sql-filter:list` is een beschreven matrix, waaronder:

|Index|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
| 0 | tekenreeks | Ja | SQL-filterexpressie |

`com.microsoft:correlation-filter:list` is een beschreven matrix, waaronder:

|Index (indien aanwezig)|Waardetype|De inhoud|  
|---------|----------------|--------------|--------------------|  
| 0 | tekenreeks | Correlatie-id |
| 1 | tekenreeks | Bericht-ID |
| 2 | tekenreeks | Handeling |
| 3 | tekenreeks | Antwoorden op |
| 4 | tekenreeks | Label |
| 5 | tekenreeks | Sessie-id |
| 6 | tekenreeks | Antwoord aan de sessie-ID|
| 7 | tekenreeks | Inhoudstype |
| 8 | Kaart | Overzicht van de toepassing gedefinieerde eigenschappen |

`ruleAction` kan zijn van de volgende typen:

| De naam van de descriptor | Descriptor-code | Waarde |
| --- | --- | ---|
| `com.microsoft:empty-rule-action:list` | 0x0000013700000005 | Lege regelactie - er is geen regelactie aanwezig |
| `com.microsoft:sql-rule-action:list` | 0x0000013700000006 | SQL-regelactie |

`com.microsoft:sql-rule-action:list` is een matrix van beschreven objecten waarvan eerste vermelding is een tekenreeks die de SQL-regelactie expressie bevat.

## <a name="deferred-message-operations"></a>Uitgestelde berichtbewerkingen  
  
### <a name="receive-by-sequence-number"></a>Door volgnummer ontvangen  

Uitgestelde berichten op volgnummer ontvangt.  
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:receive-by-sequence-number`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|volgnummers|matrix van lange|Ja|Volgnummers.|  
|ontvanger vereffenen modus|ubyte|Ja|**Ontvanger vereffenen** modus zoals opgegeven in de AMQP core v1.0.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|  
  
De berichttekst van het antwoord moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|berichten|lijst met maps|Ja|Lijst met berichten waarbij elke kaart staat voor een bericht.|  
  
De kaart die vertegenwoordigt een bericht moet de volgende vermeldingen bevatten:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|LOCK-token|UUID|Ja|Vergrendeling token als `receiver-settle-mode` is 1.|  
|message|matrix van byte|Ja|AMQP 1.0 wire-bericht dat is gecodeerd.|  
  
### <a name="update-disposition-status"></a>Status van de toestand bijwerken  

De status van de toestand van uitgestelde berichten bijgewerkt. Deze bewerking ondersteunt transacties.
  
#### <a name="request"></a>Aanvraag  

Het aanvraagbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|bewerking|tekenreeks|Ja|`com.microsoft:update-disposition`|  
|`com.microsoft:server-timeout`|uint|Nee|Bewerking server time-out in milliseconden.|  
  
De berichttekst voor de aanvraag moet bestaan uit een **amqp-waarde** sectie met een **kaart** met de volgende items:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|toestand-status|tekenreeks|Ja|Voltooid<br /><br /> Afgebroken<br /><br /> Onderbroken|  
|LOCK-tokens|matrix van uuid|Ja|Bericht vergrendeling tokens disposition status bij te werken.|  
|wachtrij voor onbestelbare reden|tekenreeks|Nee|Kan worden ingesteld als bestemming status wordt ingesteld op **onderbroken**.|  
|Beschrijving van de wachtrij voor onbestelbare|tekenreeks|Nee|Kan worden ingesteld als bestemming status wordt ingesteld op **onderbroken**.|  
|Eigenschappen te wijzigen|Kaart|Nee|Lijst met Service Bus brokered-berichteigenschappen te wijzigen.|  
  
#### <a name="response"></a>Antwoord  

Het antwoordbericht moet de volgende toepassingseigenschappen zijn:  
  
|Sleutel|Waardetype|Vereist|De inhoud|  
|---------|----------------|--------------|--------------------|  
|statusCode|int|Ja|HTTP-antwoordcode [RFC2616]<br /><br /> 200: OK – geslaagd, anders mislukt|  
|StatusDescription|tekenreeks|Nee|Beschrijving van de status.|

## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende koppelingen voor meer informatie over AMQP en Service Bus:

* [Service Bus AMQP-overzicht]
* [AMQP 1.0-protocolhandleiding]
* [AMQP in WindowsServer-Servicebus]

[Service Bus AMQP-overzicht]: service-bus-amqp-overview.md
[AMQP 1.0-protocolhandleiding]: service-bus-amqp-protocol-guide.md
[AMQP in WindowsServer-Servicebus]: https://msdn.microsoft.com/library/dn574799.asp