---
title: Azure Active Directory instellen voor toegangsbeheer met selfservice voor toepassingen | Microsoft Docs
description: Met groepsbeheer met self-service kunnen gebruikers beveiligingsgroepen of Office 365-groepen maken en beheren in Azure Active Directory. Bovendien biedt het gebruikers de mogelijkheid om het lidmaatschap van een beveiligingsgroep of Office 365-groep aan te vragen.
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
ms.date: 02/3/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 4bab9f44d1c91f05618ea510b83beb06540429f2
ms.openlocfilehash: be84686351255585c3484f5ab8dad37b92462e2b


---
# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Azure Active Directory instellen voor groepsbeheer met self-service
Zelfservicegroepsbeheer zorgt ervoor dat gebruikers beveiligingsgroepen of Office 365-groepen kan maken en beheren in de Azure Active Directory (Azure AD). Gebruikers kunnen ook vragen om lidmaatschap van een beveiligingsgroep of Office 365-groep en de eigenaar van de groep kan lidmaatschap goedkeuren of ontkennen. Op deze manier kan dagelijkse beheer van groepslidmaatschap worden overgedragen aan de mensen die de bedrijfscontext voor het desbetreffende lidmaatschap begrijpen. Functies van het zelfservicegroepsbeheer zijn enkel bechikbaar voor beveiligingsgroepen en Office 365-groepen, maar niet voor beveiligingsgroepen met mail of distributielijsten.

Groepsbeheer met self-service bestaat momenteel uit twee kernscenario's: gedelegeerd groepsbeheer en groepsbeheer met self-service.

* **Gedelegeerd groepsbeheer**
    Een voorbeeld is een beheerder die toegang beheert tot een SaaS-toepassing die het bedrijf gebruikt. Het beheren van deze gebruiksrechten is omslachtig en daarom vraagt de beheerder de eigenaar van het bedrijf om een nieuwe groep te maken. De beheerder wijst de toegang voor de toepassing toe aan de nieuwe groep en voegt aan de groep alle personen die al toegang hebben tot de toepassing. De bedrijfseigenaar kan meer gebruikers toevoegen en deze gebruikers worden automatisch ingericht op de toepassing. De bedrijfseigenaar hoeft niet op de beheerder te wachten om de toegang voor gebruikers te beheren. Als de beheerder dezelfde toelating verleent aan een beheerder in ander ebedrijfsgroep, dan zal die persoon ook de toegang voor zijn eigen gebruikers kunnen beheren. Noch de bedrijfseigenaar, noch de beheerder kunnen elkaars gebruikers bekijken of beheren. De beheerder kan nog steeds alle gebruikers die toegang tot de toepassing hebben zien en zonodig de toegangsrechten blokkeren.
* **Zelfservicegroepsmanagement**
    Een voorbeeld van dit scenario zijn twee gebruikers die allebei SharePoint Online-sites hebben die ze onafhankelijk hebben ingesteld. Ze willen elkaars teams toegang geven tot hun sites. Hiervoor kunnen ze in Azure AD een groep maken en in SharePoint selecteert elk van hen de groep waartoe ze op hun sites toegang willen verlenen. Wanneer iemand toegang wil, kan dit worden aangevraagd bij het toegangspaneel en na goedkeuring wordt automatisch toegang verleend tot beide SharePoint Online-sites. Later beslist één van hen dat alle personen die de site openen ook toegang moeten krijgen tot een specifieke SaaS-toepassing. De beheerder van de SaaS-toepassing kan toegangsrechten toevoegen voor de toepassing aan de SharePoint Online-site. Vanaf dan verlenen alle goedgekeurde verzoeken toegang tot de twee SharePoint Online-sites en ook tot deze SaaS-toepassing.

## <a name="making-a-group-available-for-end-user-self-service"></a>Een groep beschikbaar maken voor self-service door eindgebruikers
1. Open in het [Klassieke Azure-portal](https://manage.windowsazure.com) uw Azure AD-directory.
2. Schakel in het tabblad **Configureren** het **Overgedragen groepsbeheer** in.
3. Schakel **Gebruikers kunnen beveiligingsgroepen maken** of **Gebruikers kunnen Office-groepen maken** in.

Als **Gebruikers kunnen beveiligingsgroepen maken** is ingeschakeld, kunnen alle gebruikers in uw directory nieuwe beveiligingsgroepen maken en leden toevoegen aan deze groepen. Deze nieuwe groepen zullen ook verschijnen in het Toegangsvenster voor alle andere gebruikers. Als de beleidsinstelling van de groep dit toestaat, kunnen andere gebruikers verzoeken maken om lid te worden van deze groepen. Als **Gebruikers kunnen beveiligingsgroepen maken** is uitgeschakeld, kunnen gebruikers geen groepen maken en kunnen ze bestaande groepen waarvan ze een eigenaar zijn niet wijzigen. Ze kunnen echter nog steeds de lidmaatschappen van die groepen beheren en aanvragen van andere gebruikers om lid te worden van hun groep goedkeuren.

U kunt ook **Gebruikers die zelfservice kunnen gebruiken voor beveiligingsgroepen** gebruiken voor een meer fijnmazige toegangscontrole over het zelfservicegroepsbeheer voor uw gebruikers. Als **Gebruikers kunnen groepen maken** is ingeschakeld, kunnen alle gebruikers in uw directory nieuwe groepen maken en leden toevoegen aan deze groepen. Als u daarnaast de optie **Gebruikers die self-service kunnen gebruiken voor beveiligingsgroepen** instelt op Sommige, beperkt u het groepsbeheer tot een bepaalde groep gebruikers. Als deze optie is ingesteld op Enkele moet u gebruikers toevoegen aan de groep SSGMSecurityGroupsUsers alvorens ze nieuwe groepen en leden hieraan kunnen toevoegen. Door **Gebruikers die self-service kunnen gebruiken voor beveiligingsgroepen** in te stellen op Alle, kunt u alle gebruikers in uw directory de mogelijkheid geven om nieuwe groepen te maken.

U kunt ook het vak **Groep die zelfservice voor beveiligingsgroepen** gebruiken om een eigen naam op te geven voor een groep waarvan de leden zelfservice kunnen gebruiken.

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure Active Directory.

* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Azure Active Directory cmdlets for configuring group settings](active-directory-accessmanagement-groups-settings-cmdlets.md) (Azure Active Directory-cmdlets voor het configureren van groepsinstellingen)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [What is Azure Active Directory?](active-directory-whatis.md) (Wat is Azure Active Directory?)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)



<!--HONumber=Feb17_HO2-->


