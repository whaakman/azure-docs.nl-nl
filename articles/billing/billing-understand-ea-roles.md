---
title: Informatie over beheerdersrollen voor Enterprise in Azure | Microsoft Docs
description: Meer informatie over Enterprise-beheerdersrollen in Azure.
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2018
ms.author: banders
ms.openlocfilehash: 98ed28af8df246549fb521a81f1968e1f5c28cc4
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54901561"
---
# <a name="understand-azure-enterprise-agreement-administrative-roles-in-azure"></a>Enterprise Agreement voor Azure-beheerdersrollen in Azure begrijpen

Om u te helpen bij het gebruik van uw organisatie te beheren en besteden, Azure kunnen klanten met een Enterprise Agreement (EA) vijf afzonderlijke beheerdersrollen toewijzen:

- Ondernemingsbeheerder
- Enterprise-beheerder (alleen-lezen)
- Afdelingsbeheerder
- Afdeling beheerder (alleen-lezen)
- Accounteigenaar
 
Deze rollen zijn specifiek voor het beheren van Azure Enterprise-overeenkomsten en naast de ingebouwde rollen die Azure heeft toegang tot resources beheren. Zie voor meer informatie, [ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md).

De volgende secties beschrijven de beperkingen en mogelijkheden van elke rol.

## <a name="user-limit-for-admin-roles"></a>Gebruikerslimiet voor beheerdersrollen

|Rol| Gebruikerslimiet|
|---|---|
|Ondernemingsbeheerder|Onbeperkt|
|Enterprise-beheerder (alleen-lezen)|Onbeperkt|
|Afdelingsbeheerder|Onbeperkt|
|Afdeling beheerder (alleen-lezen)|Onbeperkt|
|Accounteigenaar|1 per account<sup>1</sup>|

<sup>1</sup> elk account moet een unieke Microsoft-account of werk-of schoolaccount.

## <a name="organization-structure-and-permissions-by-role"></a>Machtigingen per rol en de structuur van organisatie

|Taken| Ondernemingsbeheerder|Enterprise-beheerder (alleen-lezen)|Afdelingsbeheerder|Afdeling beheerder (alleen-lezen)|Accounteigenaar|
|---|---|---|---|---|---|
|Ondernemingsadministrators weergeven|✔|✔|✘|✘|✘|
|Toevoegen of verwijderen van Ondernemingsadministrators|✔|✘|✘|✘|✘|
|Melding contactpersonen weergeven<sup>2</sup> |✔|✔|✘|✘|✘|
|Toevoegen of verwijderen van de melding contactpersonen<sup>2</sup> |✔|✘|✘|✘|✘|
|Maken en beheren van afdelingen |✔|✘|✘|✘|✘|
|Beheerders van de afdeling weergeven|✔|✔|✔|✔|✘|
|Toevoegen of verwijderen van de afdeling beheerders|✔|✔|✔|✘|✘|
|Weergave-Accounts in de inschrijving |✔|✔|✔<sup>3</sup>|✔<sup>3</sup>|✘|
|Accounts toevoegen aan de inschrijving en wijzig de eigenaar van Account|✔|✘|✔<sup>3</sup>|✘|✘|
|Abonnementen en abonnementsmachtigingen maken en beheren|✘|✘|✘|✘|✔|

- <sup>2</sup> melding contactpersonen e-mailberichten over de Azure Enterprise-overeenkomst worden verzonden.
- <sup>3</sup> taak is beperkt tot accounts in uw afdeling.


## <a name="usage-and-costs-access-by-role"></a>Gebruik en kosten toegang op basis van rollen

|Taken| Ondernemingsbeheerder|Enterprise-beheerder (alleen-lezen)|Afdelingsbeheerder|Afdeling beheerder (alleen-lezen) |Accounteigenaar|
|---|---|---|---|---|---|
|Weergave-tegoed met inbegrip van de monetaire toezegging|✔|✔|✘|✘|✘|
|Weergave afdeling uitgaven van quota|✔|✔|✘|✘|✘|
|Afdeling uitgaven quota instellen|✔|✘|✘|✘|✘|
|Organisatie EA prijslijst weergeven|✔|✔|✘|✘|✘|
|Details van gebruiks- en kostengegevens weergeven|✔|✔|✔<sup>4</sup>|✔<sup>4</sup>|✔<sup>5</sup>|
|Beheren van resources in Azure portal|✘|✘|✘|✘|✔|

- <sup>4</sup> vereist dat de Enterprise-beheerder inschakelt **DA weergave kosten** beleid in de Enterprise portal. De beheerder van de afdeling kunnen kostendetails voor de afdeling bekijken.
- <sup>5</sup> vereist dat de Enterprise-beheerder inschakelt **kosten weergeven die door de AO** beleid in de Enterprise portal. De accounteigenaar kunnen kostendetails van de voor het account bekijken.


## <a name="pricing-in-azure-portal"></a>Prijzen in Azure portal

U ziet u mogelijk verschillende prijzen in de Azure-portal, afhankelijk van uw beheerdersrol en hoe het beleid van de kosten weergeven door de Enterprise-beheerder zijn ingesteld. De twee beleidsregels in de Enterprise portal die invloed hebben op de prijzen die u in de Azure-portal ziet zijn:

- Kosten van DA weergeven
- Kosten weergeven die door de AO

Zie voor meer informatie over het instellen van deze beleidsregels, [beheren van toegang tot factureringsgegevens voor Azure](billing-manage-access.md).

De volgende tabel ziet u de relatie tussen de Enterprise Agreement-beheerdersrollen, het beleid van de kosten weergeven, de op rollen gebaseerd beheer (RBAC) functie toegang in Azure portal en de prijzen die u ziet in de Azure-portal. De Enterprise-beheerder ziet altijd informatie over het gebruik op basis van de organisatie EA-prijzen. De beheerder van de afdeling en de eigenaar van Account zien echter verschillende prijzen weergaven op basis van het beleid van de kosten in rekening gebracht weergeven en hun RBAC-rol. De beheerdersrol voor een afdeling die worden vermeld in de volgende tabel verwijst naar de beheerder van de afdeling zowel afdeling (alleen-lezen) beheerdersrollen.

|Enterprise Agreement-beheerdersrol|Beleid voor weergave kosten in rekening gebracht voor de rol|RBAC-rol|Prijzen weergeven|
|---|---|---|---|
|Accounteigenaar of beheerder van de afdeling|✔ Ingeschakeld|Eigenaar|Organisatie EA-prijzen|
|Accounteigenaar of beheerder van de afdeling|✘ Uitgeschakeld|Eigenaar|Adviesprijzen|
|Accounteigenaar of beheerder van de afdeling|✔ Ingeschakeld |geen|Er zijn geen prijzen|
|Accounteigenaar of beheerder van de afdeling|✘ Uitgeschakeld |geen|Er zijn geen prijzen|
|Geen|Niet van toepassing |Eigenaar|Adviesprijzen|

Stelt u de Enterprise-beheerdersrol en weergeven in rekening gebracht voor in de Enterprise portal. De RBAC-rol kan worden bijgewerkt in Azure portal. Zie voor meer informatie, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Volgende stappen

- [Beheer de toegang tot factureringsgegevens voor Azure](billing-manage-access.md)
- [Toegang beheren met op rollen gebaseerd toegangsbeheer en Azure Portal](../role-based-access-control/role-assignments-portal.md)
- [Ingebouwde rollen voor Azure-resources](../role-based-access-control/built-in-roles.md)
