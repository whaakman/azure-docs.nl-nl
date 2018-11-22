---
title: Beheren van toegang voor externe gebruikers die gebruikmaken van RBAC in Azure | Microsoft Docs
description: Informatie over het beheren van toegang voor gebruikers van buiten een organisatie met behulp van op rollen gebaseerd toegangsbeheer (RBAC) in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/20/2018
ms.author: rolyon
ms.reviewer: skwan
ms.custom: it-pro
ms.openlocfilehash: aec12e6dc7d331b2610546d0b0c92fa6ce0789ee
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284520"
---
# <a name="manage-access-for-external-users-using-rbac"></a>Toegang voor externe gebruikers met RBAC beheren

Op rollen gebaseerd toegangsbeheer (RBAC) kunt beter beveiligingsbeheer voor grote organisaties en voor het midden-en kleinbedrijf werkt met externe deelnemers, leveranciers of freelancers die toegang tot bepaalde bronnen in uw omgeving, maar niet per se op de gehele nodig infrastructuur of alle scopes met betrekking tot facturering. Met RBAC kan de flexibiliteit van die eigenaar is van één Azure-abonnement beheerd door de administrator-account (service administrator-rol op een abonnementsniveau) en hebt u meerdere gebruikers uitgenodigd om te werken onder hetzelfde abonnement, maar zonder beheerdersrechten heeft voor het .

> [!NOTE]
> Office 365-abonnementen of licenties voor Azure Active Directory (bijvoorbeeld: toegang tot Azure Active Directory) ingericht vanuit het Office 365-beheercentrum center komen niet in aanmerking voor het gebruik van RBAC.

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>RBAC-rollen op het abonnementsbereik toewijzen

Er zijn twee algemene voorbeelden bij RBAC is gebruikt (maar niet tot beperkt):

* Met externe gebruikers van de organisaties (die geen deel uitmaakt van Azure Active Directory-tenant van de gebruiker met beheerdersrechten) uitgenodigd om bepaalde resources of het hele abonnement te beheren
* Werken met gebruikers binnen de organisatie (ze zijn onderdeel van Azure Active Directory-tenant van de gebruiker), maar deel uitmaken van verschillende teams of groepen die gedetailleerde toegang tot het hele abonnement of bepaalde resourcegroepen of resource-scopes in de omgeving nodig

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Toegang verlenen op het abonnementsniveau van een voor een gebruiker buiten de Azure Active Directory

RBAC-rollen kunnen alleen worden verleend **eigenaren** van het abonnement. Daarom moet de beheerder zijn aangemeld als een gebruiker die deze rol heeft een vooraf toegewezen of het Azure-abonnement is gemaakt.

Selecteer 'Abonnementen' in en kies het gewenste abonnement in Azure portal, nadat u zich hebt aangemeld als beheerder.
![abonnementsblade in Azure portal](./media/role-assignments-external-users/0.png) standaard, als de gebruiker met beheerdersrechten heeft aangeschaft het Azure-abonnement, de gebruiker wordt weergegeven als **accountbeheerder**, dit wordt het abonnement de rol. Zie voor meer informatie over de functies van de Azure-abonnement, [toevoegen of wijzigen Azure-abonnementbeheerders](../billing/billing-add-change-azure-subscription-administrator.md).

In dit voorbeeld wordt de gebruiker "alflanigan@outlook.com" is de **eigenaar** van de 'gratis' abonnement in de AAD-tenant 'tenant Azure Default'. Omdat deze gebruiker de maker van het Azure-abonnement met de eerste 'Outlook' van de Microsoft-Account is (Microsoft-Account = Outlook, etc. Live) de standaarddomeinnaam voor alle andere gebruikers die zijn toegevoegd in deze tenant worden **"\@ alflaniganuoutlook.onmicrosoft.com"**. Standaard de syntaxis van het nieuwe domein wordt gevormd door het samenstellen van de naam van de gebruikersnaam en het domein van de gebruiker die de tenant gemaakt en het toevoegen van de extensie **'. onmicrosoft.com "**.
Daarnaast wordt kunnen gebruikers zich aanmelden met een aangepaste domeinnaam in de tenant na het toevoegen van en controle van de voor de nieuwe tenant. Zie voor meer informatie over het controleren van een aangepaste domeinnaam in een Azure Active Directory-tenant [een aangepaste domeinnaam toevoegen aan uw directory](../active-directory/fundamentals/add-custom-domain.md).

In dit voorbeeld bevat de map 'Standaardtenant Azure' alleen gebruikers met de naam van het domein '\@alflanigan.onmicrosoft.com '.

Na het selecteren van het abonnement, de gebruiker met beheerdersrechten moet klikken op **Access Control (IAM)** en vervolgens **toevoegen van een nieuwe rol**.

![functie voor IAM toegang in Azure portal](./media/role-assignments-external-users/1.png)

![nieuwe gebruiker toevoegen in de functie voor IAM toegang in Azure portal](./media/role-assignments-external-users/2.png)

De volgende stap is het selecteren van de functie moet worden toegewezen en de gebruiker waaraan de RBAC-rol wordt toegewezen aan. In de **rol** in het vervolgkeuzemenu, de gebruiker met beheerdersrechten ziet alleen de ingebouwde RBAC-rollen die beschikbaar in Azure zijn. Zie voor meer uitleg van elke rol en hun toewijsbare bereiken gedetailleerde, [ingebouwde rollen](built-in-roles.md).

De gebruiker met beheerdersrechten moet vervolgens de e-mailadres van de externe gebruiker toevoegen. Het verwachte gedrag is voor de externe gebruiker worden niet weergegeven in de bestaande tenant. Nadat de externe gebruiker is uitgenodigd, hij is zichtbaar onder **abonnementen > Access Control (IAM)** met de huidige gebruikers die momenteel een RBAC-rol op het abonnementsbereik zijn toegewezen.

![Voeg machtigingen toe aan het nieuwe RBAC-rol](./media/role-assignments-external-users/3.png)

![lijst met RBAC-rollen op abonnementsniveau](./media/role-assignments-external-users/4.png)

De gebruiker "chessercarlton@gmail.com" is uitgenodigd moet een **eigenaar** voor het abonnement 'Gratis'. Nadat de uitnodiging is verzonden, ontvangt de externe gebruiker een e-mailbevestiging met een activeringskoppeling.
![e-mailuitnodiging voor RBAC-rol](./media/role-assignments-external-users/5.png)

Wordt buiten de organisatie, heeft de nieuwe gebruiker geen bestaande kenmerken in de map 'Standaardtenant Azure'. Ze worden gemaakt nadat de externe gebruiker toestemming heeft gegeven moeten worden vastgelegd in de map die is gekoppeld aan het abonnement hij een rol is toegewezen.

![e-mailbericht met de uitnodiging voor RBAC-rol](./media/role-assignments-external-users/6.png)

De externe gebruiker wordt weergegeven in de Azure Active Directory-tenant van nu af als externe gebruiker en deze kunnen worden weergegeven in de Azure-portal.

![gebruikers blade azure active directory Azure portal](./media/role-assignments-external-users/7.png)

In de **gebruikers** weergeven, de externe gebruikers kunnen worden herkend door het type ander pictogram in de Azure-portal.

Echter verleent **eigenaar** of **Inzender** toegang tot een externe gebruiker op de **abonnement** scope, staat niet toe dat de toegang tot de map van de gebruiker met beheerdersrechten, tenzij de **Globale beheerder** is toegestaan. In de eigenschappen van de gebruiker, de **gebruikerstype**, heeft twee algemene parameters, **lid** en **Gast** kunnen worden geïdentificeerd. Een lid is van een gebruiker die is geregistreerd in de map terwijl een gast een gebruiker is uitgenodigd voor de directory vanaf een externe bron is. Zie voor meer informatie, [hoe door Azure Active Directory-beheerders de gebruikers van B2B-samenwerking toevoegen](../active-directory/active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Zorg ervoor dat na het invoeren van de referenties in de portal, selecteert u de juiste map zich aanmeldt bij de externe gebruiker. Dezelfde gebruiker kan toegang hebben tot meerdere mappen en selecteer een van deze door te klikken op de gebruikersnaam in de rechterbovenhoek in Azure portal en kies vervolgens de juiste map in de vervolgkeuzelijst.

Terwijl u een gast in de map, de externe gebruiker alle resources voor het Azure-abonnement kunt beheren, maar geen toegang tot de map.

![toegang beperkt tot azure active directory met Azure portal](./media/role-assignments-external-users/9.png)

Azure Active Directory en een Azure-abonnement geen een onderliggende bovenliggende relatie net als andere Azure-resources (bijvoorbeeld: virtuele machines, virtuele netwerken, web-apps, opslag enz.) met een Azure-abonnement hebt. Alle deze is gemaakt, beheerd en onder een Azure-abonnement gefactureerd, terwijl een Azure-abonnement wordt gebruikt voor het beheren van de toegang tot een Azure-adreslijst. Zie voor meer informatie, [hoe een Azure-abonnement is gerelateerd aan Azure AD](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

Van alle de ingebouwde RBAC-rollen, **eigenaar** en **Inzender** bieden volledige beheertoegang tot alle resources in de omgeving is het verschil is dat een inzender kan niet maken en verwijderen van nieuwe RBAC-rollen . De ingebouwde rollen, zoals **Inzender voor virtuele machines** bieden volledige beheertoegang tot de resources aangegeven door de naam, ongeacht de **resourcegroep** ze worden gemaakt in.

Toewijzen van de ingebouwde RBAC-rol van **Inzender voor virtuele machines** op het abonnementsniveau van een, betekent dat de gebruiker de rol is toegewezen:

* Vindt alle virtuele machines ongeacht de datum waarop de implementatie en de resourcegroepen die ze deel van uitmaken
* Volledig beheertoegang heeft tot de virtuele machines in het abonnement
* Andere resourcetypen weergeven in het abonnement niet
* Wijzigingen die betrekking tot facturering kan niet worden uitgevoerd.

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Een ingebouwde RBAC-rol toewijzen aan een externe gebruiker

Voor een ander scenario in deze test de externe gebruiker "alflanigan@gmail.com' wordt toegevoegd als een **Inzender voor virtuele machines**.

![virtuele machine ingebouwde rol van Inzender](./media/role-assignments-external-users/11.png)

Het normale gedrag voor deze externe gebruiker met deze ingebouwde rol is het bekijken en beheren van alleen virtuele machines en hun aangrenzende Resource Manager alleen resources nodig tijdens de implementatie. Deze beperkte rollen bieden standaard alleen toegang tot de bijbehorende resources gemaakt in Azure portal.

![overzicht van virtuele machines Inzender rol in Azure portal](./media/role-assignments-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Toegang verlenen op het abonnementsniveau van een voor een gebruiker in dezelfde map

Het proces dat plaatsvindt identiek is aan een externe gebruiker toe te voegen, wordt zowel vanuit het perspectief beheerder is voor het verlenen van zowel de RBAC-rol als de gebruiker krijgt toegang tot de rol. Het verschil is hier is dat de uitgenodigde gebruiker geen uitnodigingen voor een e-mailbericht ontvangt als de resource-bereiken binnen het abonnement beschikbaar in het dashboard zijn na de aanmelding.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>RBAC-rollen in het groepsbereik resource toewijzen

Toewijzen van een RBAC-rol op een **resourcegroep** bereik heeft een identieke proces voor het toewijzen van de rol op het abonnementsniveau, voor beide typen gebruikers: externe of interne (onderdeel van dezelfde map). De gebruikers die zijn toegewezen de RBAC-rol is om te zien in hun omgeving alleen de resourcegroep waarin ze zijn toegewezen toegang vanaf de **resourcegroepen** pictogram in de Azure-portal.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Toewijzen van RBAC-rollen in het bereik van de resource

Toewijzen van een RBAC-rol op een bereik van de resource in Azure heeft een identieke proces voor het toewijzen van de rol op abonnementsniveau of op niveau van de resourcegroep, de dezelfde werkstroom voor beide scenario's te volgen. Nogmaals, de gebruikers die zijn toegewezen de RBAC-rol ziet alleen de items die ze hebben toegang tot is toegewezen, in de **alle Resources** tabblad of rechtstreeks in het dashboard.

Er is een belangrijk aspect voor RBAC zowel op het groepsbereik van de resource of resource-bereik voor de gebruikers om ervoor te zorgen voor aanmelding bij de juiste map.

![Directory-aanmelding in Azure portal](./media/role-assignments-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Toewijzen van RBAC-rollen voor een Azure Active Directory-groep

Alle scenario's die gebruikmaken van RBAC op de drie verschillende niveaus in Azure bieden de bevoegdheid beheren, implementeren en beheren van verschillende bronnen als toegewezen gebruiker zonder de noodzaak van het beheer van een persoonlijke abonnement. Ongeacht de RBAC-rol is toegewezen voor een abonnement, resourcegroep of resource-bereik, de resources die zijn gemaakt verder door de toegewezen gebruikers worden in rekening gebracht wanneer de gebruikers toegang tot hebben één Azure-abonnement. Op deze manier de gebruikers die beschikken over beheerdersmachtigingen voor die hele Azure-abonnement facturering heeft een volledig overzicht van het verbruik ongeacht wie de resources worden beheerd.

Voor grote organisaties kunnen RBAC-rollen worden toegepast op dezelfde manier voor Azure Active Directory-beveiligingsgroepen die u overweegt het perspectief dat de gebruiker met beheerdersrechten toegang wil verlenen de gedetailleerde voor teams of afdelingen gehele, niet afzonderlijk voor elke gebruiker, dus u overweegt deze als een zeer tijd en beheer van efficiënte optie. Ter illustratie van dit voorbeeld wordt de **Inzender** rol is toegevoegd aan een van de groepen in de tenant op het abonnementsniveau.

![RBAC-rol voor AAD-groepen toevoegen](./media/role-assignments-external-users/14.png)

Deze groepen zijn netwerkbeveiligingsgroepen die zijn ingericht en beheerd alleen binnen een Azure Active Directory.

