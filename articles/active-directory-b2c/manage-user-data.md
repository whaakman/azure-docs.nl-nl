---
title: Beheren van gebruikersgegevens in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het verwijderen of exporteren van gegevens van de gebruiker in Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 05/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: dacff48be3fbf16fc719f5a0395937b1f5acc979
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712535"
---
# <a name="manage-user-data-in-azure-ad-b2c"></a>Gebruikersgegevens in Azure AD B2C beheren

 In dit artikel bevat informatie over hoe u met de gebruikersgegevens in Azure Active Directory (AD) B2C met behulp van de bewerkingen die is geleverd beheren kunt door de [Azure Active Directory Graph API](https://msdn.microsoft.com/en-us/library/azure/ad/graph/api/api-catalog). Gebruikersgegevens beheren, biedt de mogelijkheid om te verwijderen van gegevens of gegevens exporteren uit controlelogboeken.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-user-data"></a>Gebruikersgegevens verwijderen

Gebruikersgegevens zijn opgeslagen in de Azure AD B2C-directory en in de controlelogboeken. Alle gebruikersgegevens audit Gegevensretentie 30 dagen in Azure AD B2C wordt bewaard. Als u verwijderen van gebruikersgegevens binnen dertig dagen wilt, kunt u de [een gebruiker verwijdert](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#DeleteUser) bewerking. Een DELETE-bewerking is vereist voor elk van de Azure AD B2C-tenants, waar de gegevens zich kan bevinden. 

Elke gebruiker in Azure AD B2C wordt toegewezen een object-ID. De object-ID biedt een unieke id moet worden gebruikt om gebruikersgegevens in Azure AD B2C te verwijderen.  Afhankelijk van uw architectuur, zijn de object-ID een handig correlatie-id via andere services zoals financiële, marketing en customer relationship management databases.  

Er is de meest nauwkeurige manier om de object-ID ophalen voor een gebruiker op te halen als onderdeel van een reis verificatie met Azure AD B2C.  Als een geldige aanvraag voor gegevens wordt ontvangen van een gebruiker met behulp van andere methoden, een offlineproces zoals een zoeken door een customer support van service-agent, kan het nodig zijn voor de gebruiker niet vinden en noteer de id van de bijbehorende object. 

Het volgende voorbeeld toont een mogelijke gegevensstroom verwijderen:

1. De gebruiker zich aanmeldt en **mijn gegevens verwijderen**.
2. De toepassing biedt een optie om de gegevens binnen een beheersectie van de toepassing te verwijderen.
3. Een Azure AD B2C-authenticatie zorgt ervoor dat de toepassing. Azure AD B2C biedt een token met de object-ID van de gebruiker terug naar de toepassing. 
4. Het token wordt ontvangen door de toepassing en de object-ID wordt gebruikt om de gebruikersgegevens via een aanroep naar Azure AD Graph API te verwijderen. Azure AD Graph API worden verwijderd van de gebruikersgegevens en retourneert een statuscode 200 OK.
5. Zo nodig met de object-ID of een andere id, de toepassing verwijderen van gebruikersgegevens ingedeeld in de andere organisatie-systemen.
6. De toepassing de verwijdering van gegevens en biedt de volgende stappen voor de gebruiker.

## <a name="export-customer-data"></a>Exporteren van gegevens van de klant

Het proces voor het exporteren van gegevens van de klant van Azure AD B2C is vergelijkbaar met de verwijdering.

Azure AD B2C-gebruikersgegevens is beperkt tot:

- **Gegevens die zijn opgeslagen in de Azure Active Directory** -gegevens in een Azure AD B2C verificatie gebruiker reis met behulp van de object-ID of een aanmeldingsnaam zoals e-mailadres of gebruikersnaam kunnen worden opgehaald.  
- **Gebeurtenissen van gebruikersspecifieke controlerapport** -gegevens is geïndexeerd met behulp van de object-ID.

In het volgende voorbeeld van een gegevensstroom export kunnen stappen beschreven als door de toepassing wordt uitgevoerd ook worden uitgevoerd door een back-end-proces of door een gebruiker met een beheerdersrol in de map:

1. De gebruiker zich aanmeldt bij de toepassing. Azure AD B2C wordt verificatie met meervoudige autorisatie afgedwongen, indien nodig.
2. De toepassing gebruikt de referenties van de gebruiker naar een Azure AD Graph API-bewerking voor het ophalen van gebruikerskenmerken aanroepen. De Azure AD Graph API biedt de kenmerkgegevens in JSON-indeling. Afhankelijk van het schema, kunnen de inhoud van de id-tokens worden ingesteld op alle persoonlijke gegevens over een gebruiker.
3. Eindgebruikers audit activiteit, haalt de toepassing. Azure AD Graph API biedt de gebeurtenisgegevens naar de toepassing.
4. de toepassing de gegevens worden verzameld en is beschikbaar voor de gebruiker.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het beheren van hoe gebruikers toegang krijgen tot uw toepassing in [gebruikerstoegang beheren](manage-user-access.md)




