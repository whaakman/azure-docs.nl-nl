---
title: Beheren van toegang voor externe gebruikers met RBAC in Azure | Microsoft Docs
description: Informatie over het beheren van toegang voor gebruikers buiten een organisatie met op rollen gebaseerde toegangsbeheer (RBAC) in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/20/2018
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: 98eb104981051bd5e7440954470960977b38286d
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296216"
---
# <a name="manage-access-for-external-users-using-rbac"></a>Beheren van toegang voor externe gebruikers met RBAC

Op rollen gebaseerde toegangsbeheer (RBAC) kunt beter beveiligingsbeheer voor grote organisaties en voor midden-en kleinbedrijf werkt met externe deelnemers, leveranciers of freelancers die toegang tot specifieke bronnen in uw omgeving, maar niet per se op de gehele nodig infrastructuur of alle scopes facturering gerelateerd. RBAC kunt de flexibiliteit van die eigenaar is van één Azure-abonnement beheerd door de administrator-account (service-beheerdersrol op abonnementsniveau) en hebben meerdere gebruikers uitgenodigd onder hetzelfde abonnement, maar zonder Administrator-rechten voor het werken.

> [!NOTE]
> Abonnementen voor Office 365 of Azure Active Directory-licenties (bijvoorbeeld: toegang tot Azure Active Directory) vanaf de Office 365 Admin center niet in aanmerking komen voor het gebruik van RBAC wordt ingericht.

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>RBAC-rollen op de scope abonnement toewijzen

Er zijn twee algemene voorbeelden wanneer RBAC is gebruikt (maar niet tot beperkt):

* Externe gebruikers van de organisaties hebben (die geen deel uit van de gebruiker admin Azure Active Directory-tenant) uitgenodigd om bepaalde bronnen of het hele abonnement te beheren
* Werken met gebruikers binnen de organisatie (ze zijn onderdeel van de gebruiker Azure Active Directory-tenant), maar deel uitmaken van verschillende teams of groepen die gedetailleerde toegang tot het hele abonnement of bepaalde resourcegroepen of resource-scopes in de omgeving nodig hebben

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Toegang verlenen op het abonnementsniveau van een voor een gebruiker buiten Azure Active Directory

RBAC-rollen kunnen alleen worden toegekend **eigenaars** van het abonnement. Daarom moet de beheerder zijn aangemeld als een gebruiker die deze rol heeft een vooraf toegewezen of het Azure-abonnement is gemaakt.

Nadat u zich als beheerder aanmelden, selecteert u vanuit de Azure-portal 'Abonnementen' in en kies de gewenste versie.
![abonnementsblade in Azure-portal](./media/role-assignments-external-users/0.png) standaard als het Azure-abonnement heeft aangeschaft door de gebruiker met beheerdersrechten de gebruiker wordt weergegeven als **accountbeheerder**, dit wordt de rol van het abonnement. Zie voor meer informatie over de Azure-abonnement rollen [toevoegen of wijzigen Azure-beheerdersrollen die het abonnement of de services beheren](../billing/billing-add-change-azure-subscription-administrator.md).

In dit voorbeeld wordt de gebruiker "alflanigan@outlook.com' is de **eigenaar** van de 'gratis proefversie' abonnement in het AAD-tenant 'tenant Azure Default'. Omdat deze gebruiker de maker van het Azure-abonnement met de eerste 'Outlook' van de Microsoft-Account (Microsoft-Account = Outlook, etc. Live) naam van het standaarddomein voor andere gebruikers die zijn toegevoegd aan deze tenant zijn **'\@ alflaniganuoutlook.onmicrosoft.com'**. Door het ontwerp van de syntaxis van het nieuwe domein wordt gevormd door het samenstellen van de naam van de gebruikersnaam en het domein van de gebruiker die de tenant wordt gemaakt en het toevoegen van de extensie **'. onmicrosoft.com '**.
Bovendien kunnen gebruikers zich aanmelden met een aangepaste domeinnaam in de tenant na het toevoegen van en controle van de voor de nieuwe tenant. Zie voor meer informatie over het controleren van een aangepaste domeinnaam in een Azure Active Directory-tenant [een aangepaste domeinnaam toevoegen aan uw directory](/active-directory/active-directory-add-domain).

In dit voorbeeld bevat de "standaard Azure ' tenantmap alleen gebruikers met de naam van het domein '\@alflanigan.onmicrosoft.com '.

Na het selecteren van het abonnement, de gebruiker met beheerdersrechten moet op **Access Control (IAM)** en vervolgens **een nieuwe rol toevoegen**.

![onderdeel voor toegangsbeheer IAM-functie in Azure-portal](./media/role-assignments-external-users/1.png)

![nieuwe gebruiker toevoegen in het onderdeel voor toegangsbeheer IAM-functie in Azure-portal](./media/role-assignments-external-users/2.png)

De volgende stap is het selecteren van de functie moet worden toegewezen en de gebruiker waaraan de RBAC-rol wordt toegewezen aan. In de **rol** vervolgkeuzemenu de gebruiker met beheerdersrechten ziet alleen de ingebouwde rollen RBAC die beschikbaar in Azure zijn. Zie voor meer uitleg van elke rol en hun toewijsbare bereiken gedetailleerde, [ingebouwde rollen](built-in-roles.md).

De gebruiker met beheerdersrechten moet de e-mailadres van de externe gebruiker toevoegen. Het verwachte gedrag is voor de externe gebruiker worden niet weergegeven in de bestaande tenant. Nadat de externe gebruiker heeft uitgenodigd, hij zijn zichtbaar onder **abonnementen > Access Control (IAM)** met de huidige gebruikers die momenteel een RBAC-rol op het bereik van het abonnement zijn toegewezen.

![Voeg machtigingen toe aan het nieuwe RBAC-rol](./media/role-assignments-external-users/3.png)

![lijst met RBAC-rollen op abonnementsniveau](./media/role-assignments-external-users/4.png)

De gebruiker "chessercarlton@gmail.com' heeft uitgenodigd om deel te worden een **eigenaar** voor het abonnement 'Gratis'. De externe gebruiker ontvangt een e-mailbevestiging met een activeringskoppeling, na het verzenden van de uitnodiging.
![e-uitnodiging voor RBAC-rol](./media/role-assignments-external-users/5.png)

Alleen buiten de organisatie, heeft de nieuwe gebruiker geen bestaande kenmerken in de tenantmap 'Standaard Azure'. Ze worden gemaakt nadat de externe gebruiker toestemming heeft gegeven moeten worden vastgelegd in de map die is gekoppeld aan het abonnement hij een rol is toegewezen.

![e-mailbericht uitnodiging voor RBAC-rol](./media/role-assignments-external-users/6.png)

De externe gebruiker wordt weergegeven in de Azure Active Directory-tenant op als de externe gebruiker en deze kunnen worden weergegeven in de Azure portal.

![gebruikers blade azure active directory Azure-portal](./media/role-assignments-external-users/7.png)

In de **gebruikers** weergave voor de externe gebruikers kunnen worden herkend door het type ander pictogram in de Azure-portal.

Echter verlenen **eigenaar** of **Inzender** toegang tot een externe gebruiker op de **abonnement** bereik, staat niet toe dat de toegang tot de admin gebruikerslijst, tenzij de **globale beheerder** is toegestaan. In de eigenschappen van de gebruiker, de **gebruikerstype**, die twee algemene parameters, heeft **lid** en **Gast** kunnen worden geïdentificeerd. Een lid is van een gebruiker die is geregistreerd in de map terwijl een gast een gebruiker uitgenodigd voor de map van een externe bron is. Zie voor meer informatie [hoe Azure Active Directory-beheerders Voeg B2B-samenwerking gebruikers](../active-directory/active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Zorg ervoor dat na het invoeren van de referenties in de portal, selecteert u de juiste map aan te melden bij de externe gebruiker. Dezelfde gebruiker kan toegang hebben tot meerdere directory's en selecteer een van deze door te klikken op de gebruikersnaam in de rechterbovenhoek in de Azure portal en kies vervolgens de juiste map in de vervolgkeuzelijst.

Terwijl een gast in de directory, de externe gebruiker alle resources voor het Azure-abonnement kunt beheren, maar heeft geen toegang tot de map.

![toegang beperkt tot azure active directory-Azure-portal](./media/role-assignments-external-users/9.png)

Azure Active Directory en een Azure-abonnement niet hebben een relatie van de onderliggende structuur met bovenliggende net als andere Azure-resources (bijvoorbeeld: virtuele machines, virtuele netwerken, web-apps, opslag, enzovoort) met een Azure-abonnement hebt. Alle deze is gemaakt, beheerd en onder een Azure-abonnement in rekening gebracht terwijl een Azure-abonnement wordt gebruikt voor het beheren van de toegang tot een Azure-map. Zie voor meer informatie [hoe een Azure-abonnement is gerelateerd aan Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

Van alle ingebouwde RBAC rollen, **eigenaar** en **Inzender** bieden volledige beheertoegang tot alle bronnen in de omgeving, het verschil is dat een medewerker kan geen maken en nieuwe RBAC-rollen te verwijderen. De ingebouwde rollen, zoals **Virtual Machine Contributor** bieden volledige management alleen toegang tot de resources die zijn aangegeven door de naam, ongeacht de **resourcegroep** ze worden gemaakt in.

Toewijzen van de ingebouwde RBAC-rol van **Virtual Machine Contributor** op abonnementsniveau, betekent dit dat de gebruiker de rol toegewezen:

* Kan alle virtuele machines ongeacht hun datum van de implementatie en weergeven ze deel van uitmaken resourcegroepen
* Volledig beheertoegang heeft tot de virtuele machines in het abonnement
* Andere brontypen weergeven in het abonnement niet
* Kan niet worden uitgevoerd vanuit het oogpunt van facturering van wijzigingen

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Een ingebouwde RBAC-rol toewijzen aan een externe gebruiker

Voor een ander scenario in deze test de externe gebruiker "alflanigan@gmail.com' wordt toegevoegd als een **Virtual Machine Contributor**.

![virtuele machine ingebouwde rol van Inzender](./media/role-assignments-external-users/11.png)

Het normale gedrag voor deze externe gebruiker met deze ingebouwde functie is om te zien en beheren van alleen virtuele machines en hun aangrenzende Resource Manager alleen bronnen nodig zijn tijdens de implementatie. Deze beperkte rollen bieden standaard alleen toegang tot hun bijbehorende resources in de Azure portal gemaakt.

![overzicht van virtuele machines Inzender rol in Azure-portal](./media/role-assignments-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Toegang verlenen op het abonnementsniveau van een voor een gebruiker in dezelfde map

De processtroom identiek is aan een externe gebruiker toe te voegen, zowel vanuit het perspectief beheerder is de RBAC-rol, evenals de gebruiker verlenen hun toegang wordt verleend aan de rol. Het verschil is hier is dat uitgenodigde de gebruiker niet alle e-uitnodigingen ontvangt als de resource-scopes in het abonnement beschikbaar in het dashboard zijn na het aanmelden.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>RBAC-rollen op het groepsbereik resource toewijzen

Toewijzen van een RBAC-rol op een **resourcegroep** scope heeft een identieke proces voor het toewijzen van de rol op het abonnementsniveau voor beide typen gebruikers - externe of interne (onderdeel van dezelfde directory). De gebruikers die zijn toegewezen de RBAC-rol is om te zien in hun omgeving alleen de resourcegroep waarin ze zijn toegewezen toegang vanaf de **resourcegroepen** pictogram in de Azure-portal.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>RBAC-rollen op het bereik van de resource toe te wijzen

Toewijzen van een RBAC-rol op een scope resource in Azure heeft een identieke proces voor het toewijzen van de rol op het abonnementsniveau of op het niveau van de resourcegroep, dezelfde werkstroom voor beide scenario's te volgen. De gebruikers die de RBAC-rol is toegewezen Zie opnieuw kunnen alleen de items die ze toegang zijn toegewezen aan in de **alle Resources** tabblad of rechtstreeks in het dashboard.

Er is een belangrijk aspect voor RBAC zowel op resource groepsbereik of resource-bereik voor de gebruikers om te controleren of aan te melden bij de juiste map.

![Directory-aanmelding in Azure-portal](./media/role-assignments-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>RBAC-rollen voor een Azure Active Directory-groep toewijzen

Alle scenario's die gebruikmaken van RBAC op de drie verschillende bereiken in Azure bieden de bevoegdheid beheren, implementeren en beheren van verschillende bronnen als een toegewezen gebruiker zonder de noodzaak om een persoonlijke abonnement te beheren. Ongeacht de RBAC-rol is toegewezen voor een abonnement, resourcegroep of resource-scope, zijn onder de één Azure-abonnement waar gebruikers toegang tot hebben alle resources verder gemaakt door de toegewezen gebruikers kosten in rekening gebracht. Op deze manier de gebruikers die beschikken over beheerdersrechten voor die volledige Azure-abonnement facturering heeft een volledig overzicht over het verbruik ongeacht wie de resources wordt beheerd.

Voor grotere organisaties, kunnen de RBAC-rollen worden toegepast op dezelfde manier voor het perspectief dat de gebruiker met beheerdersrechten toegang wil verlenen de gedetailleerde voor teams of volledige afdelingen, niet afzonderlijk voor elke gebruiker, overweegt het dus als een zeer tijd- en efficiënte optie overweegt Azure Active Directory-groepen. Ter illustratie van dit voorbeeld wordt de **Inzender** rol is toegevoegd aan een van de groepen in de tenant op het abonnementsniveau.

![RBAC-rol voor AAD-groepen toevoegen](./media/role-assignments-external-users/14.png)

Deze groepen zijn beveiligingsgroepen die zijn ingericht en beheerd alleen binnen Azure Active Directory.

