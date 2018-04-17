---
title: RBAC-rollen voor Azure Search beheerderstoegang ingesteld in de portal | Microsoft Docs
description: Op rollen gebaseerde beheer in de Azure portal.
services: search
documentationcenter: ''
author: HeidiSteen
manager: cgronlun
editor: ''
tags: azure-portal
ms.assetid: ''
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 03/20/2018
ms.author: heidist
ms.openlocfilehash: d14bf9b154c450d863d4d365c215e9712694a767
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="set-rbac-roles-for-administrative-access"></a>RBAC-rollen voor beheerderstoegang instellen

Azure biedt een [globale op rollen gebaseerde verificatie model](../role-based-access-control/role-assignments-portal.md) beheerd via de portal of de resourcemanager-API's voor alle services. Rollen eigenaar, bijdrager en lezer bepalen het niveau van *service-beheer* voor Active Directory-gebruikers, groepen en beveiligings-principals die zijn toegewezen aan elke rol. 

> [!Note]
> Er is geen op rollen gebaseerde toegangsbeheer voor het delen van een index of een subset van documenten beveiligen. Voor toegang op basis van identiteit via zoekresultaten wilt weergeven, kunt u beveiligingsfilters zodat deze resultaten door de identiteit, worden deze documenten waarvoor de aanvrager mag geen toegang verwijderen. Zie voor meer informatie [beveiligingsfilters](search-security-trimming-for-azure-search.md) en [Secure met Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="management-tasks-by-role"></a>Beheertaken per rol

Rollen zijn gekoppeld aan de machtigingsniveaus die ondersteuning bieden voor de volgende beheertaken voor Azure Search:

| Rol | Taak |
| --- | --- |
| Eigenaar |Maak of verwijder de service of een object op de service, inclusief api-sleutels, indexen, indexeerfuncties, indexeerfunctie gegevensbronnen en indexeerfunctie schema's.<p>Bekijk de servicestatus, met inbegrip van aantallen en de opslaggrootte.<p>Toevoegen of verwijderen van lidmaatschap van de rol (alleen een eigenaar kunt rollidmaatschap beheren).<p>Abonnementbeheerders en eigenaren van de service kunt u de automatische lid zijn van de rol van eigenaar. |
| Inzender |Hetzelfde niveau van toegang als eigenaar, min RBAC rollen beheren. Bijvoorbeeld, een Inzender kunt maken of verwijderen van objecten, of weergeven en opnieuw genereren [api-sleutels](search-security-api-keys.md), maar rollidmaatschappen niet wijzigen. |
| [Ingebouwde rol Search Service Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | Komt overeen met de rol Inzender. |
| Lezer |Weergave service essentials en metrische gegevens. Leden van deze rol kunnen geen index, indexeerfunctie, gegevensbron of belangrijke informatie weergeven.  |

Rollen ken geen toegangsrechten voor het service-eindpunt. Search-servicebewerkingen, zoals beheer van de index, populatie van de index en query's op zoekgegevens, worden beheerd via api-sleutels, niet rollen. Zie voor meer informatie [api-sleutels beheren](search-security-api-keys.md).

## <a name="see-also"></a>Zie ook

+ [Beheren met PowerShell](search-manage-powershell.md) 
+ [Prestaties en optimalisatie in Azure Search](search-performance-optimization.md)
+ [Aan de slag met toegangsbeheer op basis van rollen in Azure portal](../role-based-access-control/overview.md).