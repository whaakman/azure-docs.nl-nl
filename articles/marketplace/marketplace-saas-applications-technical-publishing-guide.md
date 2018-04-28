---
title: Azure Marketplace SaaS Applications Technical Guide publiceren
description: Stapsgewijze handleiding en publishing controlelijsten voor het publiceren van SaaS-toepassingen in Azure Marketplace
services: Marketplace, Compute, Storage, Networking, Blockchain, Security, SaaS
documentationcenter: ''
author: BrentL-Collabera
manager: ''
editor: BrentL-Collabera
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/28/2018
ms.author: pabutler
ms.openlocfilehash: eb6db45ca0fcb6879aeaeaaf70715691cac438b0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="saas-applications-technical-publishing-guide"></a>Technische publishing handleiding van SaaS-toepassingen

Welkom bij de Azure Marketplace SaaS-toepassingen technische handleiding publiceren. Deze handleiding is ontworpen om u te helpen candidate en bestaande uitgevers in de lijst van hun toepassingen en services in Azure Marketplace met de SaaS-toepassingen aanbieden.  
U wilt gebruiken van SaaS-toepassingen aanbieding wanneer uw oplossing worden geïmplementeerd in uw eigen Azure-abonnement en klanten moeten kunnen aanmelden via een interface die u ontwerpen en beheren van de toepassing te testen. Dit wordt uitgevoerd met behulp van [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) gebruikmaken van uw bestaande omgeving proefversie. Met andere woorden, is het een gratis proefversie klant geleid, partner gehost. Het is essentieel om uw oplossing op een manier die cloud kopers de mogelijkheid biedt om uw oplossing onafhankelijk voor geen kosten of kosten weer te geven en zodat dit aanbiedingtype een evaluatieversie biedt die overeenkomt met hoe klanten zoeken naar cloudoplossingen.  

Voor een overzicht van alle andere Marketplace-aanbiedingen, raadpleegt u de [Marketplace Publisher handleiding](https://aka.ms/sellerguide).

## <a name="saas-application-technical-guidance"></a>Technische richtlijnen voor SaaS-toepassing
De technische vereisten voor SaaS-toepassingen zijn eenvoudige. Uitgevers hoeven alleen te worden geïntegreerd met Azure AD worden gepubliceerd.  Azure AD-integratie met toepassingen goed wordt gedocumenteerd en Microsoft biedt meerdere SDK's en bronnen om dit te realiseren.  

Als u wilt starten, is het raadzaam dat u een abonnement specifiek voor uw Azure Marketplace publiceren hebt, zodat u kunt het isoleren van de andere initiatieven te werken. Bovendien, als dat niet al is geïnstalleerd, wordt aangeraden dat u de volgende hulpprogramma's als onderdeel van uw ontwikkelingsomgeving hebt: 
- [Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)  
- [Azure powerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-5.0.0)  
- [Azure ontwikkelhulpprogramma's (controleren wat er beschikbaar is)](https://azure.microsoft.com/tools/)  
- [Visual Studio Code](https://code.visualstudio.com/)  

### <a name="resources"></a>Resources
De volgende lijsten bevatten koppelingen naar de beste Azure AD-resources voor u op weg: 

**Documentatie**

- [Ontwikkelaarshandleiding Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)

- [Integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate)

- [Toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

- [Overzicht van Azure - beveiliging en identiteit](https://azure.microsoft.com/roadmap/?category=security-identity)

**Video's**

- [Azure Active Directory-verificatie met Vittorio Bertocci](https://channel9.msdn.com/Shows/XamarinShow/Episode-27-Azure-Active-Directory-Authentication-with-Vittorio-Bertocci?term=azure%20active%20directory%20integration)

- [Azure Active Directory identiteit technische gegevens meest - deel 1 van 2](https://channel9.msdn.com/Blogs/MVP-Enterprise-Mobility/Azure-Active-Directory-Identity-Technical-Briefing-Part-1-of-2?term=azure%20active%20directory%20integration)

- [Azure Active Directory identiteit technische gegevens meest - deel 2 van 2](https://channel9.msdn.com/Blogs/MVP-Azure/Azure-Active-Directory-Identity-Technical-Briefing-Part-2-of-2?term=azure%20active%20directory%20integration)

- [Apps bouwen met Microsoft Azure Active Directory](https://channel9.msdn.com/Blogs/Windows-Development-for-the-Enterprise/Building-Apps-with-Microsoft-Azure-Active-Directory?term=azure%20active%20directory%20integration)

- [Microsoft Azure-video's die zijn gericht op het Active Directory](https://azure.microsoft.com/resources/videos/index/?services=active-directory)

**Training**  
- [Microsoft Azure voor IT-professionals inhoud reeks: Azure Active Directory](https://mva.microsoft.com/en-US/training-courses/microsoft-azure-for-it-pros-content-series-azure-active-directory-16754?l=N0e23wtxC_2106218965)

**Azure Active Directory-Service-Updates**  
- [Azure AD-Service-updates](https://azure.microsoft.com/updates/?product=active-directory)

Voor ondersteuning, kunt u de volgende bronnen:
- [MSDN-forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
- [StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="the-azure-active-directory-gallery"></a>De galerie van Azure Active Directory
Naast het met uw toepassing in de Azure Marketplace/AppSource vermeld, hebt u ook de toepassing die worden vermeld in de galerie van Azure AD-toepassing die deel uitmaakt van de Azure Marketplace App Store. Klanten die gebruikmaken van Azure AD als een id-provider kan de verschillende SaaS-toepassing-connectors vinden hier gepubliceerd. IT-beheerders kunnen connectors van de app-galerie toevoegen en vervolgens configureren en gebruiken van de connectors voor eenmalige aanmelding (SSO) en de inrichting. Azure AD ondersteunt alle belangrijke federation-protocollen voor eenmalige aanmelding, met inbegrip van SAML 2.0, OpenID Connect, OAuth en WS-Fed.  

Nadat u hebt getest dat de integratie van uw toepassingen met Azure AD werkt, dient u de aanvraag voor toegang op onze toepassing netwerk-Portal. Als u een Office 365-account hebt, gebruikt u dat aan te melden bij de portal. Als u een Office 365-account niet hebt, kunt u uw Microsoft-account (zoals Outlook of Hotmail) aan te melden.

## <a name="program-benefits"></a>Programmavoordelen
- Biedt de best mogelijke eenmalige aanmelding gebruikerservaring voor klanten
- Eenvoudige en minimale configuratie van de toepassing
- Klanten kunnen zoeken naar de toepassing en deze te vinden in de galerie
- Elke klant kunt gebruiken deze integratie ongeacht welke Azure AD-SKU die ze gebruiken (vrije, Basic of Premium)
- Configuratie van stapsgewijze zelfstudie voor onze klanten wederzijdse
- Schakelt het gebruikers inrichten voor dezelfde app als u van SCIM gebruikmaakt

## <a name="prerequisites"></a>Vereisten
Als u een toepassing in de galerie van Azure AD, moet de toepassing eerst een van de federation-protocollen ondersteund door Azure AD implementeren. Lees de voorwaarden en bepalingen van de Azure AD-toepassingsgalerie zich bevindt op [juridische informatie van Microsoft Azure](https://azure.microsoft.com/support/legal/).  

De volgende tabel ziet aanvullende vereiste informatie, afhankelijk van welk protocol dat u gebruikt:

**OpenID Connect** - de multitenant-toepassing in Azure AD maken en implementeren van toestemming framework voor uw toepassing. Verzenden naar het algemene eindpunt het aanmeldingsverzoek zodat elke klant toestemming voor de toepassing kunt bieden. U kunt de klanttoegang op basis van de tenant-ID en van de gebruiker UPN in het token ontvangen en beheren.  
**SAML 2.0 of de WS-Fed** -de toepassing moet zijn een functie die de integratie SAML of WS Fed eenmalige aanmelding in de modus SP of IdP doen.
