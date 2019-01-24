---
title: Azure Service Bus-verificatie en autorisatie | Microsoft Docs
description: Apps met Service Bus met Shared Access Signature (SAS)-verificatie worden geverifieerd.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 7c5a45504b7c44d97ff2250663ef9c47ef6e3595
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54851552"
---
# <a name="service-bus-authentication-and-authorization"></a>Vereenvoudigde Service Bus-verificatie en -autorisatie

Toepassingen toegang krijgen tot de Shared Access Signature (SAS)-tokenverificatie van Azure Service Bus-resources. Met SAS, toepassingen een token voor Service Bus dat is ondertekend met een symmetrische sleutel bekend is zowel voor de uitgever van het token en Service Bus (dus ' gedeeld') en deze sleutel wordt direct die zijn gekoppeld aan een regel voor specifieke rechten, zoals de machtiging te verlenen ontvangen/luisteren of berichten te verzenden. SAS-regels zijn dat beide geconfigureerd op de naamruimte of rechtstreeks op entiteiten, zoals een wachtrij of onderwerp, waardoor het fijnmazige toegangscontrole.

SAS-tokens kunnen worden gegenereerd door een Service Bus-client rechtstreeks of ze kunnen worden gegenereerd door een tussenliggende token uitgifte van eindpunt waarmee de client communiceert. Een systeem kan bijvoorbeeld vereisen dat de client voor het aanroepen van een Active Directory autorisatie beveiligd web service-eindpunt om te bewijzen de identiteit en systeemtoegangsrechten, en de webservice en retourneert de juiste Service Bus-token. Deze SAS-token kan eenvoudig worden gegenereerd met behulp van de Service Bus-tokenprovider opgenomen in de Azure SDK. 

> [!IMPORTANT]
> Als u van Azure Active Directory Access Control (ook wel bekend als Access Control Service of ACS) met Service Bus gebruikmaakt, houd er rekening mee dat de ondersteuning voor deze methode is nu beperkt en u moet uw toepassing voor het gebruik van SAS migreren. Zie voor meer informatie, [dit blogbericht](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/) en [in dit artikel](service-bus-migrate-acs-sas.md).

## <a name="shared-access-signature-authentication"></a>Shared Access Signature-verificatie

[SAS-verificatie](service-bus-sas.md) kunt u een gebruiker toegang tot Service Bus-bronnen, met specifieke rechten. SAS-verificatie in Service Bus omvat de configuratie van een cryptografische sleutel met de bijbehorende rechten op een Service Bus-resource. Clients kunnen vervolgens toegang krijgen tot deze resource door middel van een SAS-token dat bestaat uit de bron-URI die wordt benaderd en een vervaldatum die is ondertekend met de geconfigureerde sleutel.

U kunt sleutels voor SA's configureren voor een Service Bus-naamruimte. De sleutel is van toepassing op alle berichtentiteiten binnen deze naamruimte. U kunt ook sleutels over Service Bus-wachtrijen en onderwerpen. SAS wordt ook ondersteund op [Azure Relay](../service-bus-relay/relay-authentication-and-authorization.md).

U kunt configureren voor het gebruik van SAS, een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) object in een naamruimte, wachtrij of onderwerp. Deze regel bestaat uit de volgende elementen:

* *KeyName*: identificeert de regel.
* *PrimaryKey*: een cryptografische sleutel die wordt gebruikt om u te ondertekenen of te valideren SAS-tokens.
* *Secundaire sleutel*: een cryptografische sleutel die wordt gebruikt om u te ondertekenen of te valideren SAS-tokens.
* *Rechten*: bestaat uit een verzameling van **luisteren**, **verzenden**, of **beheren** rechten verleend.

Autorisatieregels geconfigureerd op het niveau van de naamruimte kunnen toegang verlenen tot alle entiteiten in een naamruimte voor clients met behulp van tokens die zijn ondertekend met behulp van de bijbehorende sleutel. U kunt maximaal 12 dergelijke autorisatieregels configureren op een Service Bus-naamruimte, wachtrij of onderwerp. Standaard een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) met alle rechten die is geconfigureerd voor elke naamruimte wanneer deze eerst is ingericht.

Voor toegang tot een entiteit, de client vereist dat een SAS-token dat is gegenereerd met behulp van een specifieke [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). De SAS-token is gegenereerd met behulp van de HMAC-SHA256 van een resourcetekenreeks die bestaat uit de resource-URI waarvoor toegang is geclaimd en een vervaldatum met een cryptografische sleutel die is gekoppeld aan de autorisatieregel.

Ondersteuning voor SAS-verificatie voor Service Bus is opgenomen in de Azure .NET SDK-versies 2.0 en hoger. SAS biedt ondersteuning voor een [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Ondersteuning voor SAS-tekenreeksen voor databaseverbindingen deel uit van alle API's die een verbindingsreeks als parameter te accepteren.

## <a name="next-steps"></a>Volgende stappen

- Lees verder [Service Bus-verificatie met Shared Access Signatures](service-bus-sas.md) voor meer informatie over SAS.
- Hoe u [van Azure Active Directory Access Control (ACS) migreren naar Shared Access Signature autorisatie](service-bus-migrate-acs-sas.md).
- [Wijzigingen aan ingeschakeld ACS-naamruimten](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).
- Zie voor de bijbehorende informatie over Azure Relay-verificatie en autorisatie, [Azure Relay-verificatie en autorisatie](../service-bus-relay/relay-authentication-and-authorization.md). 

