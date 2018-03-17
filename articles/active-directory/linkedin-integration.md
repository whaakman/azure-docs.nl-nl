---
title: LinkedIn-verbindingen voor Microsoft-apps en services in Azure Active Directory inschakelen | Microsoft Docs
description: Wordt uitgelegd hoe u in- of uitschakelen LinkedIn account verbindingen voor Microsoft-apps in Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 03/15/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 3bf224edea9e6da0d0eadb6fb6a409248de3d0e3
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="linkedin-account-connections-for-microsoft-apps-and-services"></a>LinkedIn account verbindingen voor Microsoft-apps en services
In dit artikel kunt u informatie over het beheren van LinkedIn account verbindingen voor uw tenant in de Azure Active Directory (Azure AD)-beheercentrum. 

> [!IMPORTANT]
> LinkedIn account verbindingen functionaliteit is binnenkort beschikbaar voor Azure AD-tenants. Wanneer deze is geïmplementeerd voor uw tenant, is standaard ingeschakeld. Het is niet beschikbaar voor de Verenigde Staten government-klanten en organisaties met Exchange Online postvakken die worden gehost in Australië, Canada, China, Frankrijk, Duitsland, India, Zuid-Korea, Verenigd Koninkrijk, Japan en Zuid-Afrika. Ondersteuning voor deze locaties postvak is binnenkort beschikbaar.  Zie voor een actuele weergave van de implementatie-informatie, de [Office 365 Roadmap](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc) pagina.

## <a name="how-linkedin-account-connections-appear-to-the-user"></a>Hoe LinkedIn account verbindingen voor de gebruiker worden weergegeven
LinkedIn account verbindingen toestaan dat gebruikers om te zien van openbare LinkedIn profielgegevens binnen enkele van hun Microsoft-apps. Gebruikers in uw tenant kunt verbinding maken met hun werk LinkedIn en Microsoft-of schoolaccounts voor een overzicht van aanvullende informatie voor LinkedIn-profiel. Zie voor meer informatie [LinkedIn informatie en functies in Microsoft-apps en services](https://go.microsoft.com/fwlink/?linkid=850740).

Wanneer gebruikers in uw organisatie verbinding maken met hun werk LinkedIn en Microsoft- of schoolaccounts, hebben twee opties: 
* Geef toestemming voor het delen van gegevens tussen beide accounts. Dit betekent dat ze machtigen voor hun account LinkedIn gegevens delen met hun Microsoft-werk of schoolaccount, evenals hun Microsoft werk- of schoolaccount gebruikt om gegevens te delen met hun LinkedIn-account. Gegevens die wordt gedeeld met LinkedIn verlaat de online services. 
* Machtigen om op te delen alleen de gegevens van hun LinkedIn-account met hun werk Microsoft en schoolaccount

Voor meer informatie over de gegevens die wordt gedeeld tussen gebruikers LinkedIn en Microsoft werken of schoolaccounts, Zie [LinkedIn in Microsoft-toepassingen op uw werk of school](https://www.linkedin.com/help/linkedin/answer/84077). 
* [Gebruikers kunnen accounts verbreken](https://www.linkedin.com/help/linkedin/answer/85097) en machtigingen voor delen op elk gewenst moment verwijderen. 
* [Gebruikers kunnen beheren hoe hun eigen LinkedIn-profiel wordt bekeken](https://www.linkedin.com/help/linkedin/answer/83), met inbegrip van of het profiel in de Microsoft-apps kan worden bekeken.

## <a name="privacy-considerations"></a>Privacyoverwegingen
LinkedIn inschakelen account verbindingen kunnen Microsoft-apps en services voor toegang tot bepaalde gebruikers LinkedIn informatie. Lees de [privacyverklaring van Microsoft](https://privacy.microsoft.com/privacystatement/) voor meer informatie over de privacyoverwegingen bij het inschakelen van LinkedIn account verbindingen in Azure AD. 

## <a name="manage-linkedin-account-connections"></a>LinkedIn account verbindingen beheren
LinkedIn account verbindingen functionaliteit is standaard ingeschakeld voor uw hele tenant. U kunt LinkedIn account verbindingen voor uw hele tenant, in- of uitschakelen LinkedIn account verbindingen voor geselecteerde gebruikers in uw tenant. 

### <a name="enable-or-disable-linkedin-account-connection-for-your-tenant-in-the-azure-portal"></a>In- of uitschakelen van LinkedIn-account verbinding voor uw tenant in de Azure portal

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) met een account met globale beheerdersrechten voor de Azure AD-tenant.
2. Selecteer **gebruikers**.
3. Op de **gebruikers** blade Selecteer **gebruikersinstellingen**.
4. Onder **LinkedIn account verbindingen**:
  * Selecteer **Ja** LinkedIn account verbindingen voor alle gebruikers in uw tenant inschakelen
  * Selecteer **geselecteerde** om in te schakelen LinkedIn accountgebruikers verbindingen voor alleen geselecteerde tenant
  * Selecteer **Nee** LinkedIn account verbindingen voor alle gebruikers uitschakelen ![LinkedIn inschakelen van account-verbindingen](./media/linkedin-integration/LinkedIn-integration.png)
5. Uw instellingen opslaan als u bent met het selecteren van klaar **opslaan**.

### <a name="enable-or-disable-linkedin-account-connections-for-your-organizations-office-2016-apps-using-group-policy"></a>In- of uitschakelen LinkedIn account verbindingen voor uw organisatie Office 2016 apps met behulp van Groepsbeleid

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
Gebruik de volgende koppeling om te zien van uw huidige LinkedIn account verbindingen instellen in de Azure-portal:

[LinkedIn account verbindingen configureren](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 