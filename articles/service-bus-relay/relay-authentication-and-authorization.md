---
title: Azure Relay-verificatie en autorisatie | Microsoft Docs
description: Overzicht van de Shared Access Signature (SAS)-verificatie in Azure Relay
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: spelluru
ms.openlocfilehash: 206cca95c590a01f69d3664fb87398bc2fcb4ad9
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57445786"
---
# <a name="azure-relay-authentication-and-authorization"></a>Azure Relay-verificatie en autorisatie

Toepassingen kunnen worden geverifieerd bij Azure Relay met Shared Access Signature (SAS)-verificatie. SAS-verificatie kunnen toepassingen om te verifiëren bij de Azure Relay-service met behulp van een toegangssleutel die is geconfigureerd op de Relay-naamruimte. U kunt vervolgens deze sleutel gebruiken voor het genereren van een Shared Access Signature-token die clients gebruiken kunnen om te verifiëren bij de relay-service.

## <a name="shared-access-signature-authentication"></a>Shared Access Signature-verificatie

[SAS-verificatie](../service-bus-messaging/service-bus-sas.md) kunt u gebruikers toegang geven tot Azure Relay-resources met specifieke rechten. SAS-verificatie omvat de configuratie van een cryptografische sleutel met de bijbehorende rechten op een resource. Clients kunnen vervolgens toegang krijgen tot deze resource door middel van een SAS-token dat bestaat uit de bron-URI die wordt benaderd en een vervaldatum die is ondertekend met de geconfigureerde sleutel.

U kunt sleutels voor SA's configureren op een Relay-naamruimte. In tegenstelling tot de Service Bus-berichten, [hybride Relay-verbindingen](relay-hybrid-connections-protocol.md) biedt ondersteuning voor niet-geautoriseerde of anonieme afzenders te maken. U kunt anonieme toegang voor de entiteit inschakelen wanneer u het maken, zoals wordt weergegeven in de volgende schermafbeelding van de portal:

![][0]

U kunt configureren voor het gebruik van SAS, een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -object op een Relay-naamruimte die uit het volgende bestaat:

* *KeyName* die aangeeft dat de regel.
* *PrimaryKey* is een cryptografische sleutel gebruikt om u te ondertekenen of te valideren SAS-tokens.
* *Secundaire sleutel* is een cryptografische sleutel gebruikt om u te ondertekenen of te valideren SAS-tokens.
* *Rechten* voor het verzamelen van luisteren, verzenden of beheren van rechten verleend.

Autorisatieregels geconfigureerd op het niveau van de naamruimte kunnen toegang verlenen tot alle relay-verbindingen in een naamruimte voor clients met behulp van tokens die zijn ondertekend met behulp van de bijbehorende sleutel. Maximaal 12 dergelijke autorisatie kunnen regels worden geconfigureerd op een Relay-naamruimte. Standaard een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) met alle rechten die is geconfigureerd voor elke naamruimte wanneer deze eerst is ingericht.

Voor toegang tot een entiteit, de client vereist dat een SAS-token dat is gegenereerd met behulp van een specifieke [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). De SAS-token is gegenereerd met behulp van de HMAC-SHA256 van een resourcetekenreeks die bestaat uit de resource-URI waarvoor toegang is geclaimd en een vervaldatum met een cryptografische sleutel die is gekoppeld aan de autorisatieregel.

Ondersteuning voor SAS-verificatie voor Azure Relay is opgenomen in de Azure .NET SDK-versies 2.0 en hoger. SAS biedt ondersteuning voor een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Ondersteuning voor SAS-tekenreeksen voor databaseverbindingen deel uit van alle API's die een verbindingsreeks als parameter te accepteren.

## <a name="next-steps"></a>Volgende stappen

- Lees verder [Service Bus-verificatie met Shared Access Signatures](../service-bus-messaging/service-bus-sas.md) voor meer informatie over SAS.
- Zie de [Azure Relay Hybrid Connections-protocolhandleiding](relay-hybrid-connections-protocol.md) voor gedetailleerde informatie over de functie hybride verbindingen.
- Zie voor de bijbehorende informatie over Service Bus Messaging-verificatie en autorisatie, [Service Bus-verificatie en autorisatie](../service-bus-messaging/service-bus-authentication-and-authorization.md). 

[0]: ./media/relay-authentication-and-authorization/hcanon.png