---
title: Informatie over toewijzingen in Azure RBAC weigeren | Microsoft Docs
description: Meer informatie over toewijzingen in op rollen gebaseerd toegangsbeheer (RBAC) weigeren voor resources in Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 8ef3a2ec44c5eff80d3a50a6c56805667e164ba8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46980165"
---
# <a name="understand-deny-assignments"></a>Informatie over toewijzingen weigeren

Vergelijkbaar met een roltoewijzing een *toewijzing weigeren* bindingen een reeks acties voor weigeren aan een gebruiker, groep of service-principal bij een bepaald bereik voor de toegang wordt geweigerd. De toewijzing van een weigeren kan ook uitsluiten van beveiligings-principals en te voorkomen dat de overname van onderliggende bereiken, die afwijkt van roltoewijzingen. Op dit moment weigeren toewijzingen worden **alleen-lezen** en kan alleen worden ingesteld door Azure. Dit artikel wordt beschreven hoe weigeren toewijzingen zijn gedefinieerd.

## <a name="deny-assignment-properties"></a>Van toewijzingseigenschappen weigeren

 De toewijzing van een weigeren heeft de volgende eigenschappen:

> [!div class="mx-tableFixed"]
> | Eigenschap | Vereist | Type | Beschrijving |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Ja | Reeks | De weergavenaam van de toewijzing weigeren. Namen moeten uniek zijn voor een bepaald bereik. |
> | `Description` | Nee | Reeks | De beschrijving van de toewijzing weigeren. |
> | `Permissions.Actions` | Ten minste één acties of één DataActions | String] | Een matrix met tekenreeksen die de bewerkingen waarvoor de toewijzing weigeren toegang wordt geblokkeerd opgeeft. |
> | `Permissions.NotActions` | Nee | String] | Een matrix met tekenreeksen die de beheerbewerkingen die moeten worden uitgesloten van de toewijzing weigeren opgeeft. |
> | `Permissions.DataActions` | Ten minste één acties of één DataActions | String] | Een matrix met tekenreeksen die de gegevensbewerkingen waarop de toewijzing weigeren blokkeert de toegang te geven. |
> | `Permissions.NotDataActions` | Nee | String] | Een matrix met tekenreeksen die de gegevensbewerkingen moeten worden uitgesloten van de toewijzing weigeren opgeeft. |
> | `Scope` | Nee | Reeks | Een tekenreeks waarmee het bereik dat de weigeren-toewijzing is van toepassing op. |
> | `DoNotApplyToChildScopes` | Nee | Booleaans | Hiermee geeft u op of de toewijzing weigeren van toepassing op het onderliggende bereik is. Standaardwaarde is false. |
> | `Principals[i].Id` | Ja | String] | Een matrix met Azure AD principal object-id's (gebruiker, groep of service-principal) waarop de toewijzing weigeren van toepassing is. Ingesteld op een lege GUID `00000000-0000-0000-0000-000000000000` voor iedereen. |
> | `Principals[i].Type` | Nee | String] | Een matrix met de objecttypen die wordt vertegenwoordigd door Principals [i] .id. Ingesteld op `Everyone` voor iedereen. |
> | `ExcludePrincipals[i].Id` | Nee | String] | Een matrix met Azure AD principal object-id's (gebruiker, groep of service-principal) waarop de toewijzing weigeren is niet van toepassing. |
> | `ExcludePrincipals[i].Type` | Nee | String] | Een matrix met de objecttypen die wordt vertegenwoordigd door .id ExcludePrincipals [i]. |
> | `IsSystemProtected` | Nee | Booleaans | Hiermee geeft u op of dit weigeren toewijzing is gemaakt door Azure en kan niet worden bewerkt of verwijderd. Op dit moment weigeren alle toewijzingen worden door het systeem beveiligd. |

## <a name="everyone-principal"></a>Iedereen principal

Voor ondersteuning weigeren toewijzingen, de principal iedereen is geïntroduceerd. De principal iedereen vertegenwoordigt alle gebruikers, groepen en service-principals in Azure AD-adreslijst. Als de principal-ID een nul GUID is `00000000-0000-0000-0000-000000000000` en het type principal `Everyone`, de principal vertegenwoordigt iedereen. De principal kan worden gecombineerd met iedereen `ExcludePrincipals` weigeren iedereen behalve bepaalde gebruikers. De principal iedereen heeft de volgende beperkingen:

- Alleen in kan worden gebruikt `Principals` en kan niet worden gebruikt `ExcludePrincipals`.
- `Principals[i].Type` moet worden ingesteld op `Everyone`.

## <a name="next-steps"></a>Volgende stappen

* [Lijst weigeren toewijzingen met RBAC en de REST-API](deny-assignments-rest.md)
* [Roldefinities begrijpen](role-definitions.md)
