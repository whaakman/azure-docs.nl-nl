---
title: Selfservice voor groepsbeheer instellen - Azure Active Directory | Microsoft Docs
description: Beveiligingsgroepen of Office 365-groepen maken en beheren in Azure Active Directory en lidmaatschap van een beveiligingsgroep of Office 365-groep aanvragen
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5584e4ecc78a8805135cb3f36bd5f5cd7079129
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57767070"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Self-service groepsbeheer in Azure Active Directory instellen 

U kunt gebruikers maken en beheren hun eigen beveiligingsgroepen of Office 365-groepen in Azure Active Directory (Azure AD) inschakelen. De eigenaar van de groep kunt goedkeuren of weigeren van aanvragen voor groepslidmaatschap en kunnen delegeren van beheer van groepslidmaatschap. Groepsbeheer met Self-service-beheerfuncties zijn niet beschikbaar voor beveiligingsgroepen die e-mail of voor distributielijsten. 

## <a name="self-service-group-membership-defaults"></a>Standaardinstellingen voor groepsbeheer met Self-service lidmaatschap

Als beveiligingsgroepen worden gemaakt in Azure portal of met behulp van Azure AD PowerShell, alleen de eigenaren van de groep kunnen lidmaatschap bijwerken. Beveiligingsgroepen die zijn gemaakt in de [Toegangsvenster](https://account.activedirectory.windowsazure.com/r#/joinGroups) en alle Office 365-groepen zijn beschikbaar om toe te voegen voor alle gebruikers, of de eigenaar van goedgekeurd of automatisch goedgekeurd. In het toegangsvenster, kunt u opties voor lidmaatschap wijzigen bij het maken van de groep.

Groepen die zijn gemaakt in | Standaardgedrag voor beveiliging groep | Standaardgedrag voor Office 365-groep
------------------ | ------------------------------- | ---------------------------------
[PowerShell voor Azure AD](groups-settings-cmdlets.md) | Alleen eigenaars kunnen leden toevoegen<br>Zichtbaar, maar niet beschikbaar zijn om toe te voegen in Toegangsvenster | Open om toe te voegen voor alle gebruikers
[Azure-portal](https://portal.azure.com) | Alleen eigenaars kunnen leden toevoegen<br>Zichtbaar, maar niet beschikbaar zijn om toe te voegen in Toegangsvenster<br>De eigenaar wordt niet automatisch toegewezen tijdens het maken van groep | Open om toe te voegen voor alle gebruikers
[Toegangsvenster](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Open om toe te voegen voor alle gebruikers<br>Lidmaatschap van opties kunnen worden gewijzigd nadat de groep is gemaakt | Open om toe te voegen voor alle gebruikers<br>Lidmaatschap van opties kunnen worden gewijzigd nadat de groep is gemaakt

## <a name="self-service-group-management-scenarios"></a>Scenario's voor het beheer van selfservicegroepen

* **Gedelegeerd groepsbeheer** Een voorbeeld is een beheerder die toegang beheert tot een SaaS-toepassing die het bedrijf gebruikt. Het beheren van deze gebruiksrechten is omslachtig en daarom vraagt de beheerder de eigenaar van het bedrijf om een nieuwe groep te maken. De beheerder wijst de toegang voor de toepassing toe aan de nieuwe groep en voegt aan de groep alle personen die al toegang hebben tot de toepassing. De bedrijfseigenaar kan meer gebruikers toevoegen en deze gebruikers worden automatisch ingericht op de toepassing. De bedrijfseigenaar hoeft niet op de beheerder te wachten om de toegang voor gebruikers te beheren. Als de beheerder worden dezelfde toelating aan een beheerder in ander ebedrijfsgroep, verleent en vervolgens die persoon ook toegang voor hun eigen groepsleden beheren. Noch de bedrijfseigenaar, noch de beheerder kan bekijken of beheren van elkaars groepslidmaatschappen. De beheerder kan nog steeds alle gebruikers die toegang tot de toepassing hebben zien en zonodig de toegangsrechten blokkeren.
* **Self-service voor groepsbeheer** Een voorbeeld van dit scenario zijn twee gebruikers die allebei SharePoint Online-sites hebben die ze onafhankelijk hebben ingesteld. Ze willen elkaars teams toegang geven tot hun sites. Hiervoor kunnen ze in Azure AD een groep maken en in SharePoint selecteert elk van hen de groep waartoe ze op hun sites toegang willen verlenen. Wanneer iemand toegang wil, kan dit worden aangevraagd bij het toegangspaneel en na goedkeuring wordt automatisch toegang verleend tot beide SharePoint Online-sites. Later beslist één van hen dat alle personen die de site openen ook toegang moeten krijgen tot een specifieke SaaS-toepassing. De beheerder van de SaaS-toepassing kan toegangsrechten toevoegen voor de toepassing aan de SharePoint Online-site. Vanaf dan verlenen alle goedgekeurde verzoeken toegang tot de twee SharePoint Online-sites en ook tot deze SaaS-toepassing.

## <a name="make-a-group-available-for-user-self-service"></a>Een groep beschikbaar maken voor self-service door gebruikers
1. Meld u aan bij het [beheercentrum van Azure AD](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de directory.
2. Selecteer **Gebruikers en groepen** en selecteer vervolgens **Groepsinstellingen**.
3. Stel **Self-service voor groepsbeheer is ingeschakeld** in op **Ja**.
4. Stel **Gebruikers kunnen beveiligingsgroepen maken** of **Gebruikers kunnen Office 365-groepen maken** in op **Ja**.
  * Als deze instellingen zijn ingeschakeld, kunnen alle gebruikers in uw directory nieuwe beveiligingsgroepen maken en leden toevoegen aan deze groepen. Deze nieuwe groepen zullen ook verschijnen in het Toegangsvenster voor alle andere gebruikers. Als de beleidsinstelling van de groep dit toestaat, kunnen andere gebruikers verzoeken maken om lid te worden van deze groepen. 
  * Als deze instellingen zijn uitgeschakeld, kunnen gebruikers geen groepen maken en kunnen ze bestaande groepen waarvan ze een eigenaar zijn niet wijzigen. Ze kunnen echter nog steeds de lidmaatschappen van die groepen beheren en aanvragen van andere gebruikers om lid te worden van hun groep goedkeuren.

U kunt ook **Gebruikers die beveiligingsgroepen kunnen beheren** en **Gebruikers die Office 365-groepen kunnen beheren** gebruiken voor een meer fijnmazig toegangsbeheer over het self-servicegroepsbeheer voor uw gebruikers. Als **Gebruikers kunnen groepen maken** is ingeschakeld, kunnen alle gebruikers in uw tenant nieuwe groepen maken en leden toevoegen aan deze groepen. Door deze optie in te stellen op **Enkele**, beperkt u het groepsbeheer tot slechts een beperkte groep gebruikers. Als deze optie is ingesteld op **Enkele** moet u gebruikers toevoegen aan de groep SSGMSecurityGroupsUsers alvorens ze nieuwe groepen en leden hieraan kunnen toevoegen. Door **Gebruikers die self-service kunnen gebruiken voor beveiligingsgroepen** en **Gebruikers die Office 365-groepen kunnen beheren** in te stellen op **Alle**, kunt u alle gebruikers in uw tenant de mogelijkheid geven om nieuwe groepen te maken.

U kunt ook **Groep die beveiligingsgroepen kan beheren** of **Groep die Office 365-groepen kan beheren** gebruiken om een enkele groep op te geven waarvan de leden self-service kunnen gebruiken.

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Toegang tot resources beheren met Azure Active Directory-groepen](../fundamentals/active-directory-manage-groups.md)
* [Azure Active Directory cmdlets for configuring group settings](groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Application Management in Azure Active Directory](../manage-apps/what-is-application-management.md) (Toepassingsbeheer in Azure Active Directory)
* [Wat is Azure Active Directory?](../fundamentals/active-directory-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
