---
title: LinkedIn-verbindingen voor Microsoft-apps en services in Azure Active Directory inschakelen | Microsoft Docs
description: Wordt uitgelegd hoe u in- of uitschakelen van LinkedIn-accountverbindingen voor Microsoft-apps in Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/28/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 4b3ff0b2481b42f516d28ac17f2616685730b7d5
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37871967"
---
# <a name="linkedin-account-connections-for-microsoft-apps-and-services"></a>LinkedIn-accountverbindingen voor Microsoft-apps en services
In dit artikel leert u hoe u voor het beheren van LinkedIn-accountverbindingen voor uw tenant in het beheercentrum van Azure Active Directory (Azure AD). 

> [!IMPORTANT]
> LinkedIn-account verbindingen functionaliteit is op dit moment wordt geïmplementeerd voor Azure AD-tenants. Wanneer deze is geïmplementeerd voor uw tenant, is standaard ingeschakeld. Het is niet beschikbaar voor klanten uit de Amerikaanse overheid en organisaties met Exchange Online postvakken die worden gehost in Australië, Canada, China, Frankrijk, Duitsland, India, Zuid-Korea, Verenigd Koninkrijk, Japan en Zuid-Afrika. Ondersteuning voor deze locaties postvak is binnenkort beschikbaar.  Zie voor een actuele weergave van informatie over de rollout, de [Office 365-Roadmap](https://products.office.com/business/office-365-roadmap?filters=%26freeformsearch=linkedin#abc) pagina.

## <a name="benefit-to-users"></a>Voordeel voor gebruikers
Nadat gebruikers verbinding maken met hun LinkedIn-account, worden LinkedIn-gegevens worden gebruikt om weer te geven van persoonlijke gegevens en functies in verschillende Microsoft-apps of services. Gebruikers kunnen inzichten over de mensen die ze werken met in de kaart voor het profiel van Microsoft zien, zelfs als die voor buiten uw organisatie mensen. Na verloop van tijd, hun ervaring LinkedIn ook worden meer relevante en op maat gemaakte met hun werk. Bijvoorbeeld, LinkedIn kunt voorstellen van nieuwe verbindingen op basis van die gebruikers met werken, of inzichten te kunnen verkrijgen over mensen op de agenda op die dag.

## <a name="how-linkedin-account-connections-appear-to-the-user"></a>Hoe LinkedIn-accountverbindingen worden weergegeven aan de gebruiker
LinkedIn-accountverbindingen toestaan dat gebruikers om te zien van openbare LinkedIn-profielgegevens in enkele van hun Microsoft-apps. Gebruikers in uw tenant kunt verbinding maken met hun werk LinkedIn en Microsoft- of schoolaccount om extra LinkedIn-profielinformatie te bekijken. Zie voor meer informatie, [LinkedIn informatie en functies in Microsoft-apps en services](https://go.microsoft.com/fwlink/?linkid=850740).

Wanneer gebruikers in uw organisatie verbinding maken met hun Microsoft- en LinkedIn werk of school-accounts, zijn deze twee opties: 
* Geef toestemming voor het delen van gegevens tussen beide accounts. Dit betekent dat ze toestemming voor geven hun LinkedIn-account voor het delen van gegevens met hun Microsoft-werk of school-account, evenals hun Microsoft werk- of schoolaccount om gegevens te delen met hun LinkedIn-account. Gegevens die wordt gedeeld met LinkedIn verlaat de online services. 
* Geef toestemming voor het delen van alleen de gegevens van hun LinkedIn-account met hun Microsoft-werk en school-account

Voor meer informatie over de gegevens die wordt gedeeld tussen gebruikers LinkedIn en Microsoft werk of school-accounts, Zie [LinkedIn in Microsoft-toepassingen op uw werk of school](https://www.linkedin.com/help/linkedin/answer/84077). 
* [Gebruikers kunnen de verbinding verbreken accounts](https://www.linkedin.com/help/linkedin/answer/85097) en machtigingen voor delen op elk gewenst moment verwijderen. 
* [Gebruikers kunnen beheren hoe hun eigen LinkedIn-profiel wordt weergegeven](https://www.linkedin.com/help/linkedin/answer/83), met inbegrip van of het profiel kan worden weergegeven in de Microsoft-apps.

## <a name="privacy-considerations"></a>Privacy-overwegingen
Inschakelen van LinkedIn-accountverbindingen kunt Microsoft-apps en services voor toegang tot bepaalde gegevens van uw gebruikers LinkedIn. Lees de [privacyverklaring van Microsoft](https://privacy.microsoft.com/privacystatement/) voor meer informatie over de privacyoverwegingen bij het inschakelen van LinkedIn-accountverbindingen in Azure AD. 

## <a name="manage-linkedin-account-connections"></a>LinkedIn-accountverbindingen beheren
Functionaliteit voor verbindingen van LinkedIn-account is standaard ingeschakeld voor uw hele tenant. U kunt LinkedIn-accountverbindingen voor uw hele tenant, in- of uitschakelen LinkedIn-accountverbindingen voor geselecteerde gebruikers in uw tenant. 

### <a name="enable-or-disable-linkedin-account-connection-for-your-tenant-in-the-azure-portal"></a>In- of uitschakelen van LinkedIn-accountverbinding voor uw tenant in Azure portal

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) met een account dat een globale beheerder voor de Azure AD-tenant.
2. Selecteer **gebruikers**.
3. Op de **gebruikers** Selecteer **gebruikersinstellingen**.
4. Onder **LinkedIn-accountverbindingen**:
  * Selecteer **Ja** LinkedIn-accountverbindingen voor alle gebruikers in uw tenant inschakelen
  * Selecteer **geselecteerde** om in te schakelen LinkedIn-account-verbindingen voor alleen geselecteerde tenant gebruikers
  * Selecteer **Nee** LinkedIn-accountverbindingen voor alle gebruikers uitschakelen ![inschakelen van LinkedIn-accountverbindingen](./media/linkedin-integration/linkedin-integration.png)
5. Uw instellingen opslaan wanneer u klaar bent met het selecteren van **opslaan**.

### <a name="enable-or-disable-linkedin-account-connections-for-your-organizations-office-2016-apps-using-group-policy"></a>In- of uitschakelen van LinkedIn-accountverbindingen voor Office 2016-apps van uw organisatie met behulp van Groepsbeleid

1. Download de [beheersjabloon van Office 2016-bestanden (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Pak de **ADMX** bestanden en kopieer deze naar uw centrale store.
3. Open Groepsbeleidsbeheer.
4. Een Group Policy Object maken met de volgende instellingen: **Gebruikersconfiguratie** > **Beheersjablonen** > **Microsoft Office 2016**  >  **Diversen** > **weergeven LinkedIn-functies in Office-toepassingen**.
5. Selecteer **ingeschakeld** of **uitgeschakelde**.
  * Wanneer het beleid is **ingeschakeld**, wordt de **weergeven LinkedIn-functies in Office-toepassingen** instelling gevonden in het dialoogvenster Opties voor Office 2016 is ingeschakeld. Dit betekent ook dat gebruikers in uw organisatie LinkedIn-functies in hun Office-toepassingen kunnen gebruiken.
  * Wanneer het beleid is **uitgeschakeld**, wordt de **weergeven LinkedIn-functies in Office-toepassingen** instellen gevonden in de opties voor Office 2016 dialoogvenster is ingesteld op uitgeschakeld en eindgebruikers deze instelling niet wijzigen. Gebruikers in uw organisatie kunnen LinkedIn-functies gebruiken in hun Office 2016-toepassingen.

Dit groepsbeleid is van invloed op alleen apps van Office 2016 voor een lokale computer. Gebruikers kunnen LinkedIn-functies in profiel kaarten in het hele Office 365 zien, zelfs als ze LinkedIn in hun Office 2016-apps uitschakelen. 

### <a name="learn-more"></a>Meer informatie 
* [Gegevens van LinkedIn en functies in uw Microsoft-apps](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn help en ondersteuning](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende koppeling om te zien van uw huidige LinkedIn-accountverbindingen instellen in Azure portal:

[LinkedIn-accountverbindingen configureren](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 