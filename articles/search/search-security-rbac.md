---
title: Instellen van RBAC-rollen voor toegang van Azure met beheerdersrechten in de portal - Azure Search
description: Op rollen gebaseerd beheer (RBAC) in de Azure-portal voor het beheren en het overdragen van administratieve taken voor het beheer van Azure Search.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 38b8e8a0e413f367d34a4ccf5dbd87817891b8ea
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53313017"
---
# <a name="set-rbac-roles-for-administrative-access"></a>Instellen van RBAC-rollen voor een beheeractiviteit

Azure biedt een [globale op basis van rollen autorisatiemodel](../role-based-access-control/role-assignments-portal.md) voor alle services worden beheerd via de portal of de resourcemanager API's. Rollen eigenaar, Inzender en lezer bepalen het niveau van *servicebeheer* voor Active Directory-gebruikers, groepen en beveiligings-principals die zijn toegewezen aan elke rol. 

> [!Note]
> Er is geen op rollen gebaseerd toegangsbeheer voor het delen van een index of een subset van documenten te beveiligen. Voor toegang op basis van identiteit via zoekresultaten, kunt u beveiligingsfilters Trim resultaten door identiteit, documenten waarvoor de aanvrager mag geen toegang verwijderen. Zie voor meer informatie, [beveiligingsfilters](search-security-trimming-for-azure-search.md) en [beveiligen met Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="management-tasks-by-role"></a>Beheertaken op rol

Rollen zijn gekoppeld aan de machtigingsniveaus die ondersteuning bieden voor de volgende beheertaken voor Azure Search:

| Rol | Taak |
| --- | --- |
| Eigenaar |Maken of verwijderen van de service of elk gewenst object in de service, met inbegrip van de api-sleutels, indexen, indexeerfuncties, indexeerfunctie gegevensbronnen en indexeerfunctie schema's.<p>Bekijk de status van de service, met inbegrip van aantal en de opslaggrootte.<p>Toevoegen of verwijderen van lidmaatschap van de rol (alleen een eigenaar kan lidmaatschap van de rol beheren).<p>Alle abonnementsbeheerders en eigenaren van de service hebt automatisch lidmaatschap in de rol van eigenaar. |
| Inzender |Dezelfde mate van toegang als eigenaar van min beheer van RBAC-rollen. Bijvoorbeeld, een Inzender kunt maken of verwijderen van objecten, of weergeven en opnieuw genereren [api-sleutels](search-security-api-keys.md), maar rollidmaatschappen niet wijzigen. |
| [Ingebouwde rol van Search Service Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | Komt overeen met de rol Inzender. |
| Lezer |Weergave service essentials en metrische gegevens. Leden van deze rol kunnen geen index, indexeerfunctie, gegevensbron of belangrijke informatie weergeven.  |

Rollen Verleen geen toegangsrechten voor het service-eindpunt. Search-servicebewerkingen, zoals beheer van de index, populatie van de index en query's op zoekgegevens, worden beheerd via api-sleutels, geen rollen. Zie voor meer informatie, [api-sleutels beheren](search-security-api-keys.md).

## <a name="see-also"></a>Zie ook

+ [Beheren met PowerShell](search-manage-powershell.md) 
+ [Prestaties en optimalisatie in Azure Search](search-performance-optimization.md)
+ [Aan de slag met toegangsbeheer op basis van rollen in Azure portal](../role-based-access-control/overview.md).