---
title: Migreren van Azure Active Directory Access Control Service naar de Shared Access Signature-verificatie | Microsoft Docs
description: Toepassingen migreren vanaf de Access Control-Service naar SAS
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2017
ms.author: sethm
ms.openlocfilehash: 7a2a55a6ad6a721a39c9f064aad817f841dd3235
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="migrate-from-azure-active-directory-access-control-service-to-shared-access-signature-authorization"></a>Migreren van Azure Active Directory Access Control Service naar de Shared Access Signature-autorisatie

Service Bus-toepassingen hebt eerder had een keuze van het gebruik van twee verschillende autorisatie modellen: de [Shared Access Signature (SAS)](service-bus-sas.md) token modellen die worden geleverd door Service Bus en een federatieve waar het beheer van autorisatieregels binnen wordt beheerd door de [Azure Active Directory](/azure/active-directory/) Access Control Service (ACS) en opgehaald uit de ACS-tokens, worden doorgegeven aan Service Bus voor het verlenen van toegang tot de gewenste functies.

Het model van de autorisatie ACS lang is vervangen door [SAS autorisatie](service-bus-authentication-and-authorization.md) als het gewenste model en alle documentatie, richtlijnen en voorbeelden exclusief gebruik SAS vandaag. Bovendien is het niet meer mogelijk te maken van nieuwe Service Bus-naamruimten die zijn gekoppeld aan ACS.

SAS heeft het voordeel dat deze niet onmiddellijk afhankelijk is van een andere service is, maar rechtstreeks van een client zonder eventuele schakels kan worden gebruikt door de clienttoegang geven tot de regel SAS-sleutel en de naam van de regel. SAS kunnen ook eenvoudig worden geïntegreerd met een methode die een client moet eerst een selectievakje autorisatie met een andere service doorgeven en vervolgens een token is uitgegeven. De laatste aanpak is vergelijkbaar met het gebruikspatroon ACS, maar kan verlenende toegangstokens op basis van toepassingsspecifieke voorwaarden die moeilijk om uit te drukken in ACS.

Voor alle bestaande toepassingen die afhankelijk van ACS zijn, wij raden ten zeerste klanten hun toepassingen kunnen vertrouwen op SAS in plaats daarvan migreren.

## <a name="migration-scenarios"></a>Migratiescenario 's

ACS en Service Bus zijn geïntegreerd via de gedeelde kennis van een *ondertekeningssleutel*. De handtekeningsleutel wordt gebruikt door een ACS-naamruimte voor het ondertekenen van tokens voor autorisatie en deze wordt gebruikt door de Service Bus om te controleren of het token dat is uitgegeven door de gekoppelde ACS-naamruimte. De ACS-naamruimte bevat service-identiteiten en autorisatieregels. De autorisatieregels definiëren welke service-identiteit opgehaald of ingesteld welk token dat is uitgegeven door een externe identiteitsprovider welk type toegang tot een deel van de Service Bus-naamruimte graph in de vorm van een overeenkomst langste voorvoegsel.

Bijvoorbeeld, een ACS-regel verleent de **verzenden** claim op het voorvoegsel van het pad `/` aan een service-identiteit, wat betekent dat een token dat is uitgegeven door ACS op basis van die regel verleent van de client rechten om te verzenden naar alle entiteiten in de naamruimte. Als het voorvoegsel pad `/abc`, de identiteit is beperkt tot verzenden naar entiteiten met de naam `abc` of onder dit voorvoegsel worden georganiseerd. Ervan wordt uitgegaan dat lezers van deze richtlijnen migratie al bekend met deze concepten bent.

De migratiescenario's worden onderverdeeld in drie hoofdcategorieën:

1.  **Standaardinstellingen ongewijzigd**. Sommige klanten gebruiken een [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) object, dat wordt doorgegeven de automatisch gegenereerde **eigenaar** service-identiteit en de geheime sleutel voor de ACS-naamruimte, gekoppeld aan de Service Bus-naamruimte en voeg geen nieuwe regels.

2.  **Aangepaste service-identiteiten met eenvoudige regels**. Sommige klanten toevoegen van nieuwe service-identiteiten en elke nieuwe service-identiteit verlenen **verzenden**, **luisteren**, en **beheren** machtigingen voor één specifieke entiteit.

3.  **Aangepaste service-identiteiten met complexe regels**. Heel weinig klanten hebben complexe regel sets in welke extern gepubliceerde tokens zijn toegewezen aan de rechten voor Relay of waaraan een één service-identiteit is toegewezen differentiated rechten op verschillende naamruimte paden via meerdere regels.

Voor hulp bij de migratie van complexe regelsets, u kunt contact opnemen met [ondersteuning van Azure](https://azure.microsoft.com/support/options/). De andere twee scenario's inschakelen eenvoudige migratie.

### <a name="unchanged-defaults"></a>Ongewijzigde standaardwaarden

Als uw toepassing ACS standaardwaarden niet is gewijzigd, kunt u alle vervangen [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider) gebruik met een [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) object en de naamruimte van de vooraf geconfigureerde gebruiken **RootManageSharedAccessKey** in plaats van de ACS **eigenaar** account. Houd er rekening mee dat zelfs met ACS **eigenaar** account, deze configuratie is (en nog steeds wordt) over het algemeen niet aanbevolen, omdat deze account/regel volledige beheerinstantie ten opzichte van de naamruimte biedt, waaronder toestemming voor het verwijderen entiteiten.

### <a name="simple-rules"></a>Eenvoudige regels

Als de toepassing gebruikmaakt van aangepaste service-identiteiten met eenvoudige regels, wordt de migratie is eenvoudig in het geval waarin de identiteit van een ACS-service voor toegangsbeheer op een specifieke wachtrij is gemaakt. Dit scenario is vaak het geval in SaaS-stijl-oplossingen waar elke wachtrij wordt gebruikt als een brug naar de tenantsite van een of filiaal en de service-identiteit is gemaakt voor die specifieke site. In dit geval wordt kan de identiteit van de betreffende service worden gemigreerd naar een regel Shared Access Signature rechtstreeks in de wachtrij. De naam van de service-identiteit kan de naam van de SAS-regel en de sleutel voor de identiteit van de SAS-sleutel regel kan worden. De rechten van de SAS-regel worden vervolgens geconfigureerd is gelijk aan de respectievelijk toepasselijke ACS-regel voor de entiteit.

U kunt deze nieuwe en aanvullende configuratie van SAS in-place maken op een bestaande naamruimte die is gefedereerd met ACS en de migratie weg ACS later wordt uitgevoerd met behulp van [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) in plaats van [SharedSecretTokenProvider](/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider). De naamruimte hoeft niet te worden ontkoppeld van ACS.

### <a name="complex-rules"></a>Complexe regels

SAS-regels zijn niet bedoeld om accounts, maar zijn benoemde handtekeningsleutels die zijn gekoppeld aan rechten. Als zodanig scenario's waarin de toepassing maakt veel service-identiteiten en hen verleent toegang tot rechten voor verschillende entiteiten of de hele naamruimte vereisen nog steeds een intermediair uitgifte van tokens. U kunt verkrijgen richtlijnen voor dergelijke tussenkomst door [contact op met ondersteuning](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie over Service Bus-verificatie:

* [Service Bus-verificatie en autorisatie](service-bus-authentication-and-authorization.md)
* [Service Bus-verificatie met handtekeningen voor gedeelde toegang](service-bus-sas.md)
* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)

