---
title: Voorwaardelijke toegang acceptatie kit - Azure Active Directory
description: Overstappen op Azure AD voor voorwaardelijke toegang voor toegang tot bronnen
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2cc4ff5fb528760be8c910f3da7d5691a6aae0d8
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2019
ms.locfileid: "67387571"
---
#  <a name="adopting-azure-ad-conditional-access"></a>Overstappen op Azure AD voorwaardelijke toegang

Gebruikers kunnen resources van uw organisatie openen vanuit een willekeurige plaats met behulp van verschillende soorten apparaten en apps in een wereld en de cloud en mobiliteit. Als gevolg hiervan is alleen richten op wie toegang heeft tot een resource niet meer genoeg. U kunt bepalen wie toegang heeft en Identificeer waar de gebruiker zich en welk apparaat wordt gebruikt en nog veel meer.

Voor dit besturingselement **voorwaardelijke toegang van Azure Active Directory (AD)** kunt u de voorwaarden die een gebruiker moet voldoen aan voor toegang tot een toepassing, zoals multi-factor Authentication (MFA) opgeven. Met behulp van beleid voor voorwaardelijke toegang bepaalt hoe gemachtigde gebruikers (gebruikers die toegang tot een cloud-app zijn verleend) toegang tot cloudapps onder bepaalde omstandigheden. Raadpleeg [wat is voorwaardelijke toegang in Azure Active Directory](overview.md#conditional-access-policies) voor meer informatie.

## <a name="key-benefits"></a>Belangrijkste voordelen

De belangrijkste voordelen van het gebruik van Azure AD voor voorwaardelijke toegang zijn:

* **De productiviteit te verhogen:** Beleid voor voorwaardelijke toegang (CA) kunnen u richten op het punt waarop gebruikers zijn gevraagd te MFA gebruiken, hebt toegang geblokkeerd of zijn vereist voor het gebruik van een vertrouwd apparaat. U kunt bijvoorbeeld beleidsregels zoals het vereisen van alleen gebruikers MFA in een toepassing als uit het bedrijfsnetwerk instellen. MFA-aanvragen verminderen blijven gebruikers productiever dan als ze MFA telkens wanneer die ze zich moeten aanmelden. Bovendien Azure AD voor voorwaardelijke toegang kunt u beleidsregels per gebruiker verkocht opgeven en maakt ook beleid voor app-specifiek.
* **Risico's beheren:** Beleid voor voorwaardelijke toegang inschakelen biedt schaalbare identity protection, mogelijkheden voor toegangsbeheer op basis van de risico's en ondersteuning voor systeemeigen meervoudige verificatie. Voorwaardelijke toegang koppelen met identity protection, kunt u definiëren wanneer toegang tot een toepassing wordt geblokkeerd of vergrendeld.
* **Naleving en beheeracties adres:** Controle aanvragen en goedkeuringen voor de toepassing en inzicht in het algehele gebruik is eenvoudiger met Azure AD omdat deze ondersteuning biedt voor native auditlogboeken voor elke toepassing toegangsaanvraag uitgevoerd. Controle omvat identiteit aanvrager, aangevraagde datum, zakelijke reden, goedkeuringsstatus en identiteit van de fiatteur. Deze gegevens zijn ook beschikbaar via een API, waarmee invoer van deze gegevens in een beveiligingsincident en gebeurtenis bewaking (SIEM)-systeem naar keuze.
* **Kosten beheren:** Toegangsbeleid verplaatsen naar Azure AD afhankelijk van de aangepaste vermindert of on-premises oplossingen zoals Active Directory Federation Services (ADFS) voor voorwaardelijke toegang, de kosten van het uitvoeren van deze infrastructuur.

## <a name="customer-case-studies"></a>Casestudy's van klanten

Ontdek hoe de meeste organisaties Azure AD voor voorwaardelijke toegang gebruiken om te definiëren en implementeren van geautomatiseerde wijze het toegangsbeheer voor toegang tot cloud-apps op basis van voorwaarden. De volgende aanbevolen verhalen laten zien hoe deze behoeften van klanten wordt voldaan.

* [**Wipro** mobiele productiviteit met Microsoft cloud beveiligingsprogramma's voor het verbeteren van de klant-engagements stations.](https://customers.microsoft.com/story/wipro-professional-services-enterprise-mobility-security) Beleid voor voorwaardelijke toegang in Azure AD is voorzien van het bedrijf voor het delen van documenten, bronnen en toepassingen met vertrouwde externe entiteiten---die hun eigen referenties---behoud van controle over een eigen bedrijfsgegevens kan gebruiken.
* [**Accenture** te kiezen voor de cloud met Microsoft Cloud App security beveiligt](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security) Accenture evalueert de [App-beheer voor voorwaardelijke toegang](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad) functie van Cloud App Security, dat gebruikmaakt van Azure Active Directory voorwaardelijke toegang tot poort toepassing toegang op basis van bepaalde voorwaarden. LePenske zegt dat deze mogelijkheid kan handig zijn, bijvoorbeeld alleen-lezen toegang inschakelt tijdens het verbieden van downloads.
* [**Aramex** levering beperkt - multinational vervoer en logistiek maakt office cloud-verbonden met de oplossing voor identiteits- en toegangsbeheer](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en). Ervoor zorgen dat beveiligde toegang is uiterst moeilijk zijn met de Aramex externe werknemers. Het bedrijf is nu toepassen van voorwaardelijke toegang zodat deze externe werknemers toegang krijgen tot hun SaaS-toepassingen buiten het netwerk. De regel voor voorwaardelijke toegang wordt beslissen of u multi-factor Authentication, zodat alleen de juiste personen de juiste toegang af te dwingen.

Bezoek voor meer informatie over ervaringen van klanten en partners op Azure AD voor voorwaardelijke toegang, - [zien welke geweldige dingen mensen doen met Azure](https://azure.microsoft.com/case-studies/?service=active-directory).

## <a name="announcements"></a>Aankondigingen

Azure AD ontvangt verbeteringen regelmatig. Als u wilt bijhouden met de meest recente ontwikkelingen, Zie [wat is er nieuw in Azure Active Directory?](../fundamentals/whats-new.md)

Recente blogs door de Tech-Community en Microsoft Identity-afdeling:

* 24 september 2018 [voorwaardelijke toegang van Azure Active Directory in Azure Databricks](https://azure.microsoft.com/updates/azure-active-directory-conditional-access-in-azure-databricks/)
* 21 september 2018 [Azure AD voor voorwaardelijke toegang aangepaste besturingselementen nu in openbare preview](https://azure.microsoft.com/updates/azure-ad-conditional-access-custom-controls-are-in-public-preview/)
* 21 september 2018 [Azure AD voor voorwaardelijke toegang ondersteuning voor beperkte toegang met Microsoft Cloud App Security is nu beschikbaar](https://azure.microsoft.com/updates/azure-ad-conditional-access-support-for-limited-access-with-microsoft-cloud-app-security-is-now-available/)
* 21 september 2018 [Azure AD voorwaardelijke toegang: Beheerde browserondersteuning voor iOS/Android-platforms nu in preview](https://azure.microsoft.com/updates/azure-ad-conditional-access-managed-browser-support-for-ios-android-platforms-now-in-preview/)
* 21 september 2018 [Azure AD voor voorwaardelijke toegang voor landcodes is in openbare preview](https://azure.microsoft.com/updates/azure-ad-conditional-access-for-country-codes-is-in-public-preview/)
* 21 september 2018 [Azure AD--gebruiksrechtovereenkomst nu beschikbaar](https://azure.microsoft.com/updates/azure-ad-terms-of-use-now-available/)

## <a name="learning-resources"></a>Learning-resources

Volg de onderstaande koppelingen voor een overzicht van hoe Azure AD voor voorwaardelijke toegang-functies.

* Meer informatie "[wat is voorwaardelijke toegang in Azure Active Directory?](overview.md)"
* Weet "[wat zijn de voorwaarden in Azure Active Directory voor voorwaardelijke toegang?](conditions.md)"
* Weet "[wat is de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang?](location-condition.md)"
* Weet "[wat toegang zijn besturingselementen in Azure Active Directory voor voorwaardelijke toegang?](controls.md)"
* Zoeken "[wat de wat is als hulpprogramma in Azure Active Directory voor voorwaardelijke toegang?"](what-if-tool.md)
* Ga als volgt [aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md)

Bovendien kunt u verwijzen naar de volgende koppelingen voor richtlijnen voor het beveiligen van toegang tot alle services die kunnen worden geïntegreerd met Azure Active Directory.

* [Wat is basislijn protection (preview)?](baseline-protection.md) Basislijn-beveiliging zorgt ervoor dat u ten minste het niveau van de basislijn van de beveiliging is ingeschakeld in uw Azure Active Directory-omgeving hebt.
* [Identiteit en Apparaattoegang toegangsconfiguraties](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations). Beschrijft hoe u veilige toegang tot cloudservices via de Enterprise Mobility + Security-producten configureren door het implementeren van een aanbevolen omgeving en de configuratie, met inbegrip van een voorgeschreven set beleidsregels voor voorwaardelijke toegang en aanverwante mogelijkheden.
* [Naslaginformatie over Azure Active Directory voor voorwaardelijke toegang](technical-reference.md). Meer informatie:
   * Welke apps gebruik van voorwaardelijke toegang?
   * Welke services zijn ingeschakeld met voorwaardelijke toegang?
* [Inschakelen van voorwaardelijke toegang van Azure Active Directory voor toegang tot beveiligde gebruiker](https://www.youtube.com/watch?v=eLAYBwjCGoA). Bekijk deze video voor informatie over hoe voorwaardelijke toegang speelt een rol in andere Enterprise en Mobility Suite van workloads.

### <a name="training-videos"></a>Trainingsvideo's

**Voorwaardelijke toegang in Enterprise Mobility + Security**
   > [!VIDEO https://www.youtube.com/embed/A7IrxAH87wc]

**Voorwaardelijke toegang op basis van apparaat**
   > [!VIDEO https://www.youtube.com/embed/AdM0zYB-3WQ]

**Azure Active Directory voor voorwaardelijke toegang inschakelen voor beveiligde toegang**
   > [!VIDEO https://www.youtube.com/embed/eLAYBwjCGoA]

### <a name="online-courses"></a>Online cursussen

Raadpleeg de volgende cursussen voor voorwaardelijke toegang en nog veel meer op [Pluralsight.com](https://www.pluralsight.com/):

* Pluralsight.com: [Ontwerp Identity Management in Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-identity-management-design)
   * "Deze cursus leidt u door de essentiële items die u weten wilt voor het ontwerpen van uw oplossing voor identiteitsbeheer met Azure AD." Voorwaardelijke toegang van Azure AD wordt beschreven in 'Werken met rollen en toegangsbeheer met Azure AD' module.

* Pluralsight.com: [Ontwerp-verificatie voor Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authentication-design)
   * "Deze cursus uitgelegd hoe u Azure AD gebruiken voor het oplossen van alle vereisten van uw cloud-verificatie." Voorwaardelijke toegang van Azure AD wordt beschreven in "Vereisten voor verschillende scenario's voor verificatie"-module.

* Pluralsight.com: [Ontwerp autorisatie voor Microsoft Azure](https://www.pluralsight.com/courses/microsoft-azure-authorization-design)
   * "In deze cursus leert autorisatie opties die beschikbaar zijn met Azure en Azure AD." Voorwaardelijke toegang van Azure AD wordt beschreven in de module 'Autorisatie met Azure Resource Manager en Azure AD'.

### <a name="books"></a>Boeken

* O'Reilly - [implementeren van oplossingen voor Azure - Second Edition.](https://www.oreilly.com/library/view/implementing-azure-solutions/9781789343045/b7ead3db-eb1c-4ace-897e-86ee25ea86be.xhtml)
   * 'Aan de slag met Azure-services en meer informatie over het implementeren hiervan in uw organisatie. Voorwaardelijke toegang van Azure AD wordt beschreven in het hoofdstuk [implementeren en Azure Active Directory synchroniseren](https://learning.oreilly.com/library/view/implementing-azure-solutions/9781789343045/02ca8bba-08cf-4691-a7d0-1b96e286e7ea.xhtml). "

* Wiley- [Mastering Microsoft Azure-infrastructuurservices](https://www.wiley.com/Mastering+Microsoft+Azure+Infrastructure+Services-p-9781119003298)
   * "Dit is alles wat die u nodig hebt om te begrijpen, evalueren, implementeren en onderhouden van omgevingen die gebruikmaken van Microsoft Azure."

## <a name="white-papers"></a>Technische documenten

* 18 December 2018 bekendgemaakt [maken van een strategie voor flexibele toegang beheren met Azure Active Directory](../authentication/concept-resilient-controls.md)
   * Dit document bevat informatie over strategieën een organisatie kan vaststellen voor de flexibiliteit om het risico van de vergrendeling van het tijdens onvoorziene onderbrekingen.

* 18 September 2018 bekendgemaakt [Resources voor het migreren van toepassingen naar Azure Active Directory](../manage-apps/migration-resources.md)
   * In dit technische document bevat een lijst met bronnen voor hulp bij het migreren van toegang tot toepassingen en -verificatie met Azure Active Directory (Azure AD).

* 12 juli 2018 bekendgemaakt [Azure-beveiliging en naleving blauwdruk: PaaS-webtoepassing die als host fungeert voor de officiële Workloads groot-Brittannië](../../security/blueprints/ukofficial-paaswa-overview.md)
   * Azure blauwdrukken bestaan uit richtlijnen documenten en automation-sjablonen die voor het implementeren van cloud-gebaseerde architecturen voor oplossingen voor scenario's waarvoor accreditatie-en nalevingsvereisten.

## <a name="guidance-for-it-administrators"></a>Richtlijnen voor IT-beheerders

Aanmelden bij de [Azure-portal](https://portal.azure.com/) als een globale beheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang. Raadpleeg [rol beheerdersmachtigingen in Azure Active Directory.](../users-groups-roles/directory-assign-admin-roles.md)

Als een IT-beheerder, gebruikt u [Azure AD voor voorwaardelijke toegang](overview.md) om te vereisen dat gebruikers verifiëren met behulp van Azure multi-factor Authentication, moet u zich aanmelden met een vertrouwd netwerk of een vertrouwd apparaat.

Hier vindt u nuttige koppelingen om u aan de slag te helpen:

* [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md)
* [Gebruik Azure AD-toegangsbeoordelingen voor het beheren van gebruikers die zijn uitgesloten van beleid voor voorwaardelijke toegang](../governance/conditional-access-exclusion.md)
* [Procedure: De implementatie van voorwaardelijke toegang in Azure Active Directory plannen](plan-conditional-access.md)
* [Snelstart: MFA vereisen voor specifieke apps met Azure Active Directory voor voorwaardelijke toegang](app-based-mfa.md)
* [Snelstart: Gebruiksrechtovereenkomst moet zijn geaccepteerd voordat u toegang tot cloud-apps vereisen](require-tou.md)
* [Snelstart: Toegang blokkeren als er een risico voor de sessie wordt gedetecteerd met Azure Active Directory voor voorwaardelijke toegang](app-sign-in-risk.md)
* [Veelgestelde vragen over de Azure AD voorwaardelijke toegang](faqs.md)
   * Als u meer vragen hebt, kunt u ook weergeven de [MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&sort=relevancedesc&brandIgnore=True&searchTerm=password+reset+azure).
   * Als u het antwoord op een probleem niet kunt vinden, zijn altijd onze ondersteuningsteams beschikbaar om te helpen u verder. Gebruik [Neem contact op met Microsoft ondersteuning](../authentication/active-directory-passwords-troubleshoot.md#contact-microsoft-support).

### <a name="tutorials"></a>Zelfstudies

* [**Snelstart: MFA vereisen voor specifieke apps met Azure Active Directory voor voorwaardelijke toegang**](app-based-mfa.md)
   * In deze Quick Start laat zien hoe het configureren van een Azure AD voor voorwaardelijke toegang-beleid dat is multi-factor authentication voor een geselecteerde cloud-app in uw omgeving vereist.

* [**Snelstart: Gebruiksrechtovereenkomst moet zijn geaccepteerd voordat u toegang tot cloud-apps vereisen**](require-tou.md)
   * In deze Quick Start laat zien hoe het configureren van een beleid voor Azure AD voor voorwaardelijke toegang waarvoor een gebruiksrechtovereenkomst moet zijn geaccepteerd voor een geselecteerde cloud-app in uw omgeving.

* [**Snelstart: Toegang blokkeren als er een risico voor de sessie wordt gedetecteerd met Azure Active Directory voor voorwaardelijke toegang**](app-sign-in-risk.md)
   * In deze Quick Start laat zien hoe het configureren van beleid voor voorwaardelijke toegang waarmee een aanmelding wordt geblokkeerd als het niveau van een geconfigureerde aanmeldingsrisico is gedetecteerd.

* [Zelfstudie: **Migreren van een klassiek beleid waarvoor multi-factor authentication in Azure portal**](policy-migration-mfa.md)
   * Deze zelfstudie laat zien hoe u migreert van een klassiek beleid waarvoor multi-factor authentication (MFA) voor een cloud-app.

## <a name="end-user-readiness-and-communication"></a>Gereedheid van de eindgebruiker en communicatie

Voorwaardelijke toegang maakt gebruik van andere Azure AD-mogelijkheden die invloed kunnen zijn op de ervaring van eindgebruikers. Bijvoorbeeld, kunt u Azure multi-factor authentication sterke verificatie inschakelen voor gebruikers. In dat geval gebruikt u de sjablonen van de eindgebruiker van Azure MFA.

## <a name="next-steps"></a>Volgende stappen

* Begint met de implementatie met de [documentatie voor voorwaardelijke toegang implementatieplanning](plan-conditional-access.md).
