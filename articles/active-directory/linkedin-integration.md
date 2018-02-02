---
title: In- of uitschakelen van LinkedIn-integratie voor Office-toepassingen in Azure Active Directory | Microsoft Docs
description: Wordt uitgelegd hoe u in- of uitschakelen van LinkedIn-integratie voor Microsoft-apps in Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/30/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 5ebc44d0ef6200baeacf4f1f8c4371e2d1eed9db
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="linkedin-integration-for-office-applications"></a>LinkedIn-integratie voor Office-toepassingen
In dit artikel wordt uitgelegd hoe u Beperk de gebruikers aan wie LinkedIn integratie u in Azure Active Directory (Azure AD vindt). LinkedIn-integratie is standaard ingeschakeld wanneer deze is toegevoegd aan uw tenant, waarmee gebruikers toegang krijgen tot openbare LinkedIn gegevens binnen enkele van hun Microsoft-apps. Elke gebruiker kan afzonderlijk naar hun werk of school-account koppelen aan hun account LinkedIn kiezen.

> [!IMPORTANT]
> LinkedIn-integratie wordt niet geïmplementeerd op alle Azure AD-tenants op hetzelfde moment. Nadat deze is geïmplementeerd voor uw Azure-tenant, worden LinkedIn-integratie is standaard ingeschakeld. LinkedIn-integratie is niet beschikbaar voor Ga-local, soevereine en government tenants. 

## <a name="linkedin-integration-from-the-user-perspective"></a>Integratie van LinkedIn vanuit het gebruikersperspectief
Wanneer gebruikers in uw organisatie verbinding maken met hun LinkedIn met hun werk- of schoolaccount, [ze LinkedIn om gegevens te leveren toestaan](https://www.linkedin.com/help/linkedin/answer/84077) moet worden gebruikt in de Microsoft-apps en services die voorziet in uw organisatie. [Gebruikers kunnen accounts verbreken](https://www.linkedin.com/help/linkedin/answer/85097), waarbij de machtiging voor LinkedIn gegevens delen met Microsoft worden verwijderd. LinkedIn-integratie gebruikt openbaar LinkedIn-profielinformatie. [Gebruikers kunnen beheren hoe hun eigen LinkedIn-profiel wordt bekeken](https://www.linkedin.com/help/linkedin/answer/83) LinkedIn privacy-instellingen, met inbegrip van of het profiel kan worden weergegeven in de Microsoft-apps gebruiken.

## <a name="privacy-considerations"></a>Privacyoverwegingen
Doordat LinkedIn-integratie in Azure AD, kunt u Microsoft-apps en services voor toegang tot bepaalde gebruikers LinkedIn informatie. Lees de [privacyverklaring van Microsoft](https://privacy.microsoft.com/privacystatement/) voor meer informatie over de privacyoverwegingen bij het inschakelen van LinkedIn-integratie in Azure AD. 

## <a name="manage-linkedin-integration"></a>Integratie van LinkedIn beheren
LinkedIn-integratie voor ondernemingen is standaard ingeschakeld in Azure AD. Doordat LinkedIn-integratie kan alle gebruikers in uw organisatie LinkedIn-functies binnen Microsoft-services, zoals LinkedIn-profielen weergeven in Outlook gebruiken. Uitschakelen van LinkedIn integratie LinkedIn functies verwijdert uit de Microsoft-apps en services en gegevens delen tussen LinkedIn en uw organisatie via Microsoft-services wordt gestopt.

### <a name="enable-or-disable-linkedin-integration-for-your-organization-in-the-azure-portal"></a>In- of uitschakelen van LinkedIn-integratie voor uw organisatie in de Azure portal

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) met een account met globale beheerdersrechten voor de Azure AD-tenant.
2. Selecteer **gebruikers en groepen**.
3. Op de **gebruikers en groepen** blade Selecteer **gebruikersinstellingen**.
4. Onder **LinkedIn integratie**, selecteer **Ja** of **Nee** of LinkedIn integratie uit te schakelen.
   ![LinkedIn-integratie inschakelen](./media/linkedin-integration/LinkedIn-integration.PNG)

### <a name="enable-or-disable-linkedin-integration-for-your-organizations-office-2016-apps-using-group-policy"></a>In- of uitschakelen van LinkedIn-integratie voor uw organisatie Office 2016 apps met behulp van Groepsbeleid

1. Download de [Office 2016 Beheersjablonen (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Pak de **ADMX** -bestanden en kopieer deze naar uw **centrale opslagplaats**.
3. Open Groepsbeleidsbeheer.
4. Een groepsbeleidsobject maken met de volgende instellingen: **Gebruikersconfiguratie** > **Beheersjablonen** > **Microsoft Office 2016**  >  **Diverse** > **LinkedIn integratie**.
5. Selecteer **ingeschakeld** of **uitgeschakelde**.
  * Wanneer het beleid is **ingeschakeld**, wordt de **LinkedIn weergeven functies in Office-toepassingen** gevonden in het dialoogvenster Opties voor Office 2016 instelling is ingeschakeld. Dit betekent ook dat gebruikers in uw organisatie LinkedIn-functies in hun Office-toepassingen gebruiken kunnen.
  * Wanneer het beleid is **uitgeschakeld**, wordt de **LinkedIn weergeven functies in Office-toepassingen** instellingen gevonden in de opties voor Office 2016 dialoogvenster is ingesteld op de status uitgeschakeld en eindgebruikers deze instelling niet wijzigen. Gebruikers in uw organisatie kunnen LinkedIn-functies gebruiken in hun toepassingen Office 2016. 

Dit groepsbeleid beïnvloedt alleen Office 2016 apps voor een lokale computer. Gebruikers kunnen LinkedIn-functies in de profiel-kaarten in Office 365 zien, zelfs als ze LinkedIn in hun Office 2016-apps uitschakelen. 

### <a name="learn-more"></a>Meer informatie 
* [LinkedIn informatie en functies in uw Microsoft-apps](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn help en ondersteuning](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende koppeling om te zien van uw huidige LinkedIn integratie-instelling in de Azure portal:

[LinkedIn-integratie configureren](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 