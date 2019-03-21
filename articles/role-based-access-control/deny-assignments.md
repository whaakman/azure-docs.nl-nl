---
title: Informatie over toewijzingen voor Azure-resources te weigeren | Microsoft Docs
description: Meer informatie over weigeren toewijzingen in op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources.
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
ms.date: 03/13/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: fff213c8d1a408bf96e385f2097a5ef30dcc05d2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57992099"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Informatie over toewijzingen voor Azure-resources te weigeren

Vergelijkbaar met een roltoewijzing een *weigeren toewijzing* wordt een reeks acties voor weigeren aan een gebruiker, groep of service-principal bij een bepaald bereik voor de toegang wordt geweigerd. Weigeren toewijzingen voorkomen dat gebruikers van het uitvoeren van acties die specifieke Azure-resource, zelfs als een roltoewijzing deze toegang wordt verleend. Sommige providers in Azure zijn nu resource weigeren toewijzingen.

In sommige opzichten weigeren toewijzingen zijn anders dan roltoewijzingen. Weigeren toewijzingen kunnen uitsluiten van beveiligings-principals en te voorkomen dat de overname van onderliggende bereiken. Weigeren toewijzingen zijn ook van toepassing op [klassiek abonnementsbeheerder](rbac-and-directory-admin-roles.md) toewijzingen.

Dit artikel wordt beschreven hoe weigeren toewijzingen zijn gedefinieerd.

> [!NOTE]
> Op dit moment de enige manier waarop u kunt toevoegen uw eigen weigeren toewijzingen met behulp van Azure blauwdrukken is. Zie voor meer informatie, [nieuwe resources beveiligen met blauwdrukken voor Azure-resourcevergrendelingen](../governance/blueprints/tutorials/protect-new-resources.md).

## <a name="deny-assignment-properties"></a>Van toewijzingseigenschappen weigeren

 De toewijzing van een weigeren heeft de volgende eigenschappen:

> [!div class="mx-tableFixed"]
> | Eigenschap | Vereist | Type | Description |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Ja | String | De weergavenaam van de toewijzing weigeren. Namen moeten uniek zijn voor een bepaald bereik. |
> | `Description` | Nee | String | De beschrijving van de toewijzing weigeren. |
> | `Permissions.Actions` | Ten minste één acties of één DataActions | String[] | Een matrix met tekenreeksen die de bewerkingen waarvoor de toewijzing weigeren toegang wordt geblokkeerd opgeeft. |
> | `Permissions.NotActions` | Nee | String[] | Een matrix met tekenreeksen die de beheerbewerkingen die moeten worden uitgesloten van de toewijzing weigeren opgeeft. |
> | `Permissions.DataActions` | Ten minste één acties of één DataActions | String[] | Een matrix met tekenreeksen die de gegevensbewerkingen waarop de toewijzing weigeren blokkeert de toegang te geven. |
> | `Permissions.NotDataActions` | Nee | String[] | Een matrix met tekenreeksen die de gegevensbewerkingen moeten worden uitgesloten van de toewijzing weigeren opgeeft. |
> | `Scope` | Nee | String | Een tekenreeks waarmee het bereik dat de weigeren-toewijzing is van toepassing op. |
> | `DoNotApplyToChildScopes` | Nee | Booleaans | Hiermee geeft u op of de toewijzing weigeren van toepassing op het onderliggende bereik is. Standaardwaarde is false. |
> | `Principals[i].Id` | Ja | String[] | Een matrix met Azure AD principal object-id's (gebruiker, groep, service-principal of beheerde identiteit) waarop de toewijzing weigeren van toepassing is. Ingesteld op een lege GUID `00000000-0000-0000-0000-000000000000` om weer te geven van alle beveiligings-principals. |
> | `Principals[i].Type` | Nee | String[] | Een matrix met de objecttypen die wordt vertegenwoordigd door Principals [i] .id. Ingesteld op `SystemDefined` om weer te geven van alle beveiligings-principals. |
> | `ExcludePrincipals[i].Id` | Nee | String[] | Een matrix met Azure AD principal object-id's (gebruiker, groep, service-principal of beheerde identiteit) waarop de toewijzing weigeren is niet van toepassing. |
> | `ExcludePrincipals[i].Type` | Nee | String[] | Een matrix met de objecttypen die wordt vertegenwoordigd door .id ExcludePrincipals [i]. |
> | `IsSystemProtected` | Nee | Booleaans | Hiermee geeft u op of dit weigeren toewijzing is gemaakt door Azure en kan niet worden bewerkt of verwijderd. Op dit moment weigeren alle toewijzingen worden door het systeem beveiligd. |

## <a name="system-defined-principal"></a>Het systeem gedefinieerde Principal

Voor ondersteuning weigeren toewijzingen, de **System-Defined Principal** is geïntroduceerd. Deze principal vertegenwoordigt alle gebruikers, groepen, service-principals en beheerde identiteiten in Azure AD-adreslijst. Als de principal-ID een nul GUID is `00000000-0000-0000-0000-000000000000` en het type principal `SystemDefined`, de principal vertegenwoordigt alle beveiligings-principals. `SystemDefined` kan worden gecombineerd met `ExcludePrincipals` voor het weigeren van alle beveiligings-principals met uitzondering van bepaalde gebruikers. `SystemDefined` heeft de volgende beperkingen:

- Alleen in kan worden gebruikt `Principals` en kan niet worden gebruikt `ExcludePrincipals`.
- `Principals[i].Type` moet worden ingesteld op `SystemDefined`.

## <a name="next-steps"></a>Volgende stappen

* [Lijst weigeren toewijzingen voor Azure-resources met behulp van de REST-API](deny-assignments-rest.md)
* [Informatie over roldefinities voor Azure-resources](role-definitions.md)
