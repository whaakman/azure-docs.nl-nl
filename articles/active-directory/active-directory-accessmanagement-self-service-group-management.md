---
title: Toegangsbeheer met self-service voor toepassingen instellen in Azure Active Directory | Microsoft Docs
description: Beveiligingsgroepen of Office 365-groepen maken en beheren in Azure Active Directory en lidmaatschap van een beveiligingsgroep of Office 365-groep aanvragen
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 904d5c70-c34a-46c4-a9a7-d1efecf4821c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 7a7370eb076ba8602a58a260a14bb863c55bc803
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-azure-active-directory-for-self-service-group-management"></a>Azure Active Directory instellen voor groepsbeheer met self-service
Uw gebruikers kunnen hun eigen beveiligingsgroepen of Office 365-groepen maken en beheren in Azure Active Directory (Azure AD). Gebruikers kunnen ook vragen om lidmaatschap van een beveiligingsgroep of Office 365-groep en de eigenaar van de groep kan lidmaatschap goedkeuren of ontkennen. Het dagelijkse beheer van groepslidmaatschap kan worden overgedragen aan de mensen die de bedrijfscontext voor het desbetreffende lidmaatschap begrijpen. Functies van het zelfservicegroepsbeheer zijn enkel bechikbaar voor beveiligingsgroepen en Office 365-groepen, maar niet voor beveiligingsgroepen met mail of distributielijsten.

Groepsbeheer met self-service dient momenteel twee kernscenario's: gedelegeerd groepsbeheer en groepsbeheer met self-service.

* **Gedelegeerd groepsbeheer** Een voorbeeld is een beheerder die toegang beheert tot een SaaS-toepassing die het bedrijf gebruikt. Het beheren van deze gebruiksrechten is omslachtig en daarom vraagt de beheerder de eigenaar van het bedrijf om een nieuwe groep te maken. De beheerder wijst de toegang voor de toepassing toe aan de nieuwe groep en voegt aan de groep alle personen die al toegang hebben tot de toepassing. De bedrijfseigenaar kan meer gebruikers toevoegen en deze gebruikers worden automatisch ingericht op de toepassing. De bedrijfseigenaar hoeft niet op de beheerder te wachten om de toegang voor gebruikers te beheren. Als de beheerder dezelfde toelating verleent aan een beheerder in ander ebedrijfsgroep, dan zal die persoon ook de toegang voor zijn eigen gebruikers kunnen beheren. Noch de bedrijfseigenaar, noch de beheerder kunnen elkaars gebruikers bekijken of beheren. De beheerder kan nog steeds alle gebruikers die toegang tot de toepassing hebben zien en zonodig de toegangsrechten blokkeren.
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

* [Toegang tot resources beheren met Azure Active Directory-groepen](active-directory-manage-groups.md)
* [Azure Active Directory cmdlets for configuring group settings](active-directory-accessmanagement-groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Wat is Azure Active Directory?](active-directory-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
