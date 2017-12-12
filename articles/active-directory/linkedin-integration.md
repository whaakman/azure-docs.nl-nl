---
title: LinkedIn-integratie in Azure AD configureren | Microsoft Docs
description: Legt uit hoe in- of uitschakelen van LinkedIn-integratie voor Microsoft-apps in Azure Active Directory.
services: active-directory
author: jeffgilb
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 15c161542d6826e6aeb5f708a0d9c3fa1f1885e3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="enabling-linkedin-integration-in-azure-active-directory"></a>LinkedIn-integratie in Azure Active Directory inschakelen
Doordat LinkedIn-integratie kan gebruikers toegang krijgen tot de gegevens van beide openbare LinkedIn en, als ze dat hun persoonlijke LinkedIn-netwerk in de Microsoft-apps. Elke gebruiker kan afzonderlijk kiezen voor een werkaccount verbinding met hun LinkedIn-account.

### <a name="linkedin-integration-from-your-end-users-perspective"></a>LinkedIn integratie van uw eindgebruikers perspectief
Wanneer eindgebruikers in uw organisatie wordt hun account LinkedIn verbinding met hun werkaccount, kunnen ze de personen die ze met binnen en buiten de organisatie werken te achterhalen. Verbinding maken met de twee accounts kan LinkedIn verbindingen en profielgegevens worden gebruikt in de Microsoft-apps van uw organisatie van de gebruiker, maar ze kunnen altijd opt-out door het verwijderen van de machtiging voor LinkedIn te delen die gegevens. De integratie gebruikt ook openbaar profielgegevens en gebruikers kunnen kiezen of hun openbare profiel en het netwerk informatie delen met Microsoft-toepassingen via LinkedIn privacy-instellingen.

>[!NOTE]
> Doordat LinkedIn-integratie in Azure AD, kunt apps en services voor toegang tot bepaalde gegevens van uw eindgebruikers. Lees de [privacyverklaring van Microsoft](https://privacy.microsoft.com/privacystatement/) voor meer informatie over de privacyoverwegingen bij het inschakelen van LinkedIn-integratie in Azure AD. 

## <a name="enable-linkedin-integration"></a>LinkedIn-integratie inschakelen
LinkedIn-integratie voor ondernemingen is standaard ingeschakeld in Azure AD. Dus wanneer deze functie beschikbaar voor uw tenant gemaakt is, alle gebruikers in uw organisatie kunnen zien LinkedIn functies en -profielen. Doordat LinkedIn-integratie kan gebruikers in uw organisatie LinkedIn-functies binnen Microsoft-services, zoals profielen weer te geven wanneer de ontvangst van een e-mailbericht in Outlook gebruiken. Uitschakelen van deze functie wordt voorkomen dat toegang tot LinkedIn functies en stopt account gebruikersverbindingen en gegevens delen tussen LinkedIn en uw organisatie via Microsoft-services.

> [!IMPORTANT]
> Deze functie is niet beschikbaar voor Ga-local, soevereine en government tenants. Bovendien Azure AD-service updates, zoals LinkedIn integratiemogelijkheden, worden niet geïmplementeerd voor alle tenants van Azure op hetzelfde moment. U kunt LinkedIn integratie met Azure AD totdat deze mogelijkheid in uw Azure-tenant uitgerold niet inschakelen.

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) met een account met globale beheerdersrechten voor de map.
2. Selecteer **gebruikers en groepen**.
3. Op de **gebruikers en groepen** blade Selecteer **gebruikersinstellingen**.
4. Onder **LinkedIn integratie**, selecteer Ja of Nee om het in- of uitschakelen van LinkedIn-integratie.
   ![LinkedIn-integratie inschakelen](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="learn-more"></a>Meer informatie 
* [LinkedIn informatie en functies in uw Microsoft-apps](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn help en ondersteuning](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Volgende stappen
De volgende koppeling kunt u in- of uitschakelen LinkedIn integratie met Azure AD via de Azure-portal:

[LinkedIn-integratie configureren](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 