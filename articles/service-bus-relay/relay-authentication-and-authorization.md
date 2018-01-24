---
title: Azure Relay-verificatie en autorisatie | Microsoft Docs
description: Overzicht van Shared Access Signature (SAS)-verificatie in Azure Relay
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: sethm
ms.openlocfilehash: 86a9cf2c1106180ba5c8c65849042784bfd2afcd
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-relay-authentication-and-authorization"></a>Azure Relay-verificatie en autorisatie

Toepassingen kunnen worden geverifieerd bij Azure Relay met Shared Access Signature (SAS)-verificatie. SAS-verificatie kan toepassingen te verifiëren bij de Azure-Relay-service met behulp van een toegangssleutel die is geconfigureerd op de Relay-naamruimte. U kunt deze sleutel vervolgens gebruiken voor het genereren van een Shared Access Signature-token die clients gebruiken kunnen om de relay-service te verifiëren.

## <a name="shared-access-signature-authentication"></a>Shared Access Signature-verificatie

[SAS verificatie](../service-bus-messaging/service-bus-sas.md) kunt u een gebruikerstoegang tot Azure Relay-resources met specifieke rechten verlenen. SAS-verificatie omvat de configuratie van een cryptografische sleutel met de gekoppelde rechten van een resource. Clients vervolgens toegang kunnen krijgen tot deze resource in de vorm van een SAS-token, die bestaat uit de bron-URI wordt geopend en een verloopdatum die zijn ondertekend met de geconfigureerde sleutel.

U kunt de sleutels voor SA's configureren voor een Relay-naamruimte. In tegenstelling tot de Service Bus-berichtenservice [Relay hybride verbindingen](relay-hybrid-connections-protocol.md) onbevoegde of anonieme afzenders ondersteunt. U kunt anonieme toegang inschakelen voor de entiteit bij het maken, zoals wordt weergegeven in de volgende schermopname van de portal:

![][0]

U kunt configureren voor het gebruik van SAS, een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) object op een Relay-naamruimte die uit het volgende bestaat:

* *KeyName* waarin de regel.
* *PrimaryKey* is een cryptografische sleutel gebruikt aanmelding/SAS om tokens te valideren.
* *Secundaire sleutel* is een cryptografische sleutel gebruikt aanmelding/SAS om tokens te valideren.
* *Rechten* die vertegenwoordigt de verzameling van luisteren, verzenden of beheren van rechten verleend.

Autorisatieregels geconfigureerd op het niveau van de naamruimte kunnen toegang verlenen tot alle relay-verbindingen in een naamruimte voor clients met tokens die zijn ondertekend met de bijbehorende sleutel. Maximaal 12 toestemming kunnen regels worden geconfigureerd voor een Relay-naamruimte. Standaard een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) met alle rechten is geconfigureerd voor elke naamruimte als het eerst is ingericht.

Voor toegang tot een entiteit moet de client vereist dat een SAS-token dat is gegenereerd met een specifieke [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). De SAS-token wordt gegenereerd op basis van het HMAC-SHA256 van een resourcetekenreeks die bestaat uit de resource-URI waartoe toegang is aangevraagd en een verloopbewerking met een cryptografische sleutel die is gekoppeld aan de autorisatieregel.

Ondersteuning van de SAS-verificatie voor Azure Relay is opgenomen in de Azure .NET SDK-versies 2.0 en hoger. SAS biedt ondersteuning voor een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Alle API's die een verbindingsreeks als een parameter accepteren onder meer ondersteuning voor SAS-verbindingsreeksen.

## <a name="next-steps"></a>Volgende stappen

- Blijven lezen [Service Bus-verificatie met Shared Access Signatures](../service-bus-messaging/service-bus-sas.md) voor meer informatie over SAS.
- Zie de [Azure Relay hybride verbindingen protocol handleiding](relay-hybrid-connections-protocol.md) voor gedetailleerde informatie over de mogelijkheden voor hybride verbindingen.
- Zie voor de bijbehorende informatie over Service Bus-berichtenservice verificatie en autorisatie, [Service Bus-verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md). 

[0]: ./media/relay-authentication-and-authorization/hcanon.png