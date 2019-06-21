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
ms.date: 06/13/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: f15d6fd81337aa4a859539e86f37a516848c9370
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165985"
---
# <a name="understand-deny-assignments-for-azure-resources"></a>Informatie over toewijzingen voor Azure-resources te weigeren

Vergelijkbaar met een roltoewijzing een *weigeren toewijzing* wordt een reeks acties voor weigeren aan een gebruiker, groep of service-principal bij een bepaald bereik voor de toegang wordt geweigerd. Weigeren toewijzingen voorkomen dat gebruikers van het uitvoeren van acties die specifieke Azure-resource, zelfs als een roltoewijzing deze toegang wordt verleend.

Dit artikel wordt beschreven hoe weigeren toewijzingen zijn gedefinieerd.

## <a name="how-deny-assignments-are-created"></a>Hoe weigeren toewijzingen worden gemaakt

Weigeren toewijzingen worden gemaakt en beheerd door Azure-resources beveiligen. Bijvoorbeeld, blauwdrukken voor Azure en Azure beheerde apps gebruik weigeren toewijzingen om systeem beheerde resources te beschermen. Zie voor meer informatie, [nieuwe resources beveiligen met blauwdrukken voor Azure-resourcevergrendelingen](../governance/blueprints/tutorials/protect-new-resources.md).

## <a name="compare-role-assignments-and-deny-assignments"></a>Roltoewijzingen vergelijken en toewijzingen weigeren

Weigeren toewijzingen volgen een vergelijkbaar patroon als roltoewijzingen, maar ook enkele verschillen.

| Mogelijkheid | Roltoewijzing | Toewijzing weigeren |
| --- | --- | --- |
| Toegang verlenen | :heavy_check_mark: |  |
| Toegang weigeren |  | :heavy_check_mark: |
| Kan rechtstreeks worden gemaakt | :heavy_check_mark: |  |
| Met een bereik toepassen | :heavy_check_mark: | :heavy_check_mark: |
| Uitsluiten van beveiligings-principals |  | :heavy_check_mark: |
| Voorkomen dat de overname van onderliggende bereiken |  | :heavy_check_mark: |
| Van toepassing op [klassiek abonnementsbeheerder](rbac-and-directory-admin-roles.md) toewijzingen |  | :heavy_check_mark: |

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
> | `DoNotApplyToChildScopes` | Nee | Boolean | Hiermee geeft u op of de toewijzing weigeren van toepassing op het onderliggende bereik is. Standaardwaarde is false. |
> | `Principals[i].Id` | Ja | String[] | Een matrix met Azure AD principal object-id's (gebruiker, groep, service-principal of beheerde identiteit) waarop de toewijzing weigeren van toepassing is. Ingesteld op een lege GUID `00000000-0000-0000-0000-000000000000` om weer te geven van alle beveiligings-principals. |
> | `Principals[i].Type` | Nee | String[] | Een matrix met de objecttypen die wordt vertegenwoordigd door Principals [i] .id. Ingesteld op `SystemDefined` om weer te geven van alle beveiligings-principals. |
> | `ExcludePrincipals[i].Id` | Nee | String[] | Een matrix met Azure AD principal object-id's (gebruiker, groep, service-principal of beheerde identiteit) waarop de toewijzing weigeren is niet van toepassing. |
> | `ExcludePrincipals[i].Type` | Nee | String[] | Een matrix met de objecttypen die wordt vertegenwoordigd door .id ExcludePrincipals [i]. |
> | `IsSystemProtected` | Nee | Boolean | Hiermee geeft u op of dit weigeren toewijzing is gemaakt door Azure en kan niet worden bewerkt of verwijderd. Op dit moment weigeren alle toewijzingen worden door het systeem beveiligd. |

## <a name="the-all-principals-principal"></a>De principal alle beveiligings-Principals

Voor ondersteuning weigeren toewijzingen, een systeem gedefinieerde principal met de naam *alle Principals* is geïntroduceerd. Deze principal vertegenwoordigt alle gebruikers, groepen, service-principals en beheerde identiteiten in Azure AD-adreslijst. Als de principal-ID een nul GUID is `00000000-0000-0000-0000-000000000000` en het type principal `SystemDefined`, de principal vertegenwoordigt alle beveiligings-principals. In de uitvoer van Azure PowerShell lijkt alle Principals op het volgende:

```azurepowershell
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
```

Alle beveiligings-Principals kunnen worden gecombineerd met `ExcludePrincipals` voor het weigeren van alle beveiligings-principals met uitzondering van bepaalde gebruikers. Alle Principals heeft de volgende beperkingen:

- Alleen in kan worden gebruikt `Principals` en kan niet worden gebruikt `ExcludePrincipals`.
- `Principals[i].Type` moet worden ingesteld op `SystemDefined`.

## <a name="next-steps"></a>Volgende stappen

* [Lijst weigeren toewijzingen voor Azure-resources met behulp van de Azure portal](deny-assignments-portal.md)
* [Informatie over roldefinities voor Azure-resources](role-definitions.md)
