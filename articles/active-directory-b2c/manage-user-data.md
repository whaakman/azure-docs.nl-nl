---
title: Beheren van gebruikersgegevens in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het verwijderen of exporteren van gebruikersgegevens in Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 49e9efa537ad1f2a1d7f06dd7f8a68a409c7d4e0
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054513"
---
# <a name="manage-user-data-in-azure-active-directory-b2c"></a>Beheren van gebruikersgegevens in Azure Active Directory B2C

 Dit artikel wordt beschreven hoe u de gegevens van de gebruiker in Azure Active Directory (Azure AD) B2C kunt beheren met behulp van de bewerkingen die worden geleverd door de [Azure Active Directory Graph API](https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog). Gebruikersgegevens beheren omvat het verwijderen van of het exporteren van gegevens uit auditlogboeken.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-user-data"></a>Gebruikersgegevens verwijderen

Gebruikersgegevens zijn opgeslagen in de Azure AD B2C-directory en in de auditlogboeken. Alle gebruiker audit-gegevens worden bewaard gedurende 30 dagen in Azure AD B2C. Als u verwijderen van gebruikersgegevens in die periode van 30 dagen wilt, kunt u de [verwijderen van een gebruiker](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#DeleteUser) bewerking. Een DELETE-bewerking is vereist voor elk van de Azure AD B2C-tenants waar de gegevens zich kan bevinden. 

Elke gebruiker in Azure AD B2C wordt toegewezen een object-ID. De object-ID biedt een unieke id voor u kunt gebruiken om gebruikersgegevens in Azure AD B2C te verwijderen. Afhankelijk van uw architectuur, de object-ID mag een handig correlatie-id voor andere services, zoals financiële, marketing, en klant relatie management databases. 

Er is de meest nauwkeurige manier om de object-ID voor een gebruiker op te halen als onderdeel van een reis verificatie met Azure AD B2C. Als u een geldige aanvraag voor gegevens ontvangt van een gebruiker met behulp van andere methoden, een offlineproces, zoals een zoekopdracht met de klantenondersteuning van een agent, kan het nodig zijn om te zoeken van de gebruiker en noteer het bijbehorende object-ID. 

Het volgende voorbeeld ziet u een stroom mogelijk verwijderen van gegevens:

1. De gebruiker zich aanmeldt en **verwijderen van mijn gegevens**.
2. De toepassing biedt een optie om de gegevens in een beheersectie van de toepassing te verwijderen.
3. De toepassing zorgt ervoor dat een verificatie met Azure AD B2C. Azure AD B2C biedt een token met de object-ID van de gebruiker terug naar de toepassing. 
4. Het token wordt ontvangen door de toepassing en de object-ID wordt gebruikt om de gebruikersgegevens via een aanroep naar de Azure AD Graph-API te verwijderen. De Azure AD Graph API Hiermee verwijdert u de gebruikersgegevens en retourneert een statuscode 200 OK.
5. De toepassing coördineert de verwijdering van gebruikersgegevens in de andere organisatie-systemen naar behoefte via de object-ID of andere id.
6. De toepassing wordt bevestigd dat de verwijdering van gegevens en biedt de volgende stappen aan de gebruiker.

## <a name="export-customer-data"></a>Exporteren van gegevens van de klant

Het proces van het exporteren van gegevens van de klant van Azure AD B2C is vergelijkbaar met de verwijdering.

Azure AD B2C-gebruikersgegevens is beperkt tot:

- **Gegevens die zijn opgeslagen in de Azure Active Directory**: U kunt gegevens in een Azure AD B2C-verificatie de gebruikersbeleving ophalen via de object-ID of een aanmelding naam, zoals een e-mailadres of gebruikersnaam. 
- **Gebeurtenissen voor specifieke gebruikers controlerapport**: kunt u gegevens indexeren via de object-ID.

In het volgende voorbeeld van een gegevensstroom exporteren, kunnen de stappen die worden beschreven als die worden uitgevoerd door de toepassing ook worden uitgevoerd met een back-end-proces of een gebruiker met een beheerdersrol in de map:

1. De gebruiker zich aanmeldt bij de toepassing. Azure AD B2C dwingt verificatie met Azure multi-factor Authentication af, indien nodig.
2. Referenties van de gebruiker de toepassing gebruikt voor het aanroepen van een Azure AD Graph API-bewerking voor het ophalen van kenmerken van de gebruiker. De Azure AD Graph-API biedt de kenmerkgegevens in JSON-indeling. Afhankelijk van het schema kunt u de id-token inhoud om op te nemen van alle persoonlijke gegevens over een gebruiker instellen.
3. De gebruiker audit-activiteit, haalt de toepassing. De Azure AD Graph-API biedt de gebeurtenisgegevens naar de toepassing.
4. De toepassing de gegevens verzamelt en is beschikbaar voor de gebruiker.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheren van hoe gebruikers toegang tot uw toepassing, [gebruikerstoegang beheren](manage-user-access.md).




