---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 24863e239620f84a57c631b3ecf5b08997de31c5
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
U wilt controleren of gebruikers in uw organisatie hebben het juiste niveau van toegang tot deze bronnen. U niet wilt onbeperkte toegang verlenen aan gebruikers, maar moet u ook om ervoor te zorgen dat ze hun werk te kunnen doen. Op rollen gebaseerde toegangsbeheer (RBAC) kunt u beheren welke gebruikers hebben een machtiging voor het voltooien van specifieke acties op een scope. Een rol definieert een set toegestane acties. U de rol toewijzen aan een scope en opgeven welke gebruikers deel uitmaken van die rol voor het bereik.

Verleen gebruikers de minimale bevoegdheden om u te helpen hun werk bij het plannen van uw strategie voor toegangsbeheer. De volgende afbeelding toont een voorgestelde patroon voor het toewijzen van RBAC.

![Bereik](./media/resource-manager-governance-rbac/role-examples.png)

Er zijn drie functies die van toepassing op alle resources - eigenaar, bijdrager en Reader. Alle accounts die zijn toegewezen aan de rol van eigenaar moeten strikt worden beheerd en wordt zelden gebruikt. Gebruikers die alleen de status van oplossingen moeten moeten de rol Lezer worden verleend.

De meeste gebruikers krijgen [resourcespecifieke rollen](../articles/active-directory/role-based-access-built-in-roles.md) of [aangepaste rollen](../articles/active-directory/role-based-access-control-custom-roles.md) op niveau van het abonnement of de resource-groep. Deze rollen definiëren nauw de toegestane acties. Gebruikers aan deze rollen toewijst, kunt u de vereiste toegang voor gebruikers verlenen zonder het te veel besturingselement toe te staan. U kunt een account toewijzen aan meer dan één rol en die gebruiker ontvangt de gecombineerde machtigingen van de rollen. Verlenen van toegang op het niveau van de resource is vaak te beperkend voor gebruikers, maar werkt mogelijk voor een geautomatiseerd proces ontworpen om specifieke taak.

### <a name="who-can-assign-roles"></a>Wie kan rollen toewijzen

Als u wilt maken en verwijderen van roltoewijzingen, moeten gebruikers hebben `Microsoft.Authorization/roleAssignments/*` toegang. Deze toegang wordt verleend door middel van de eigenaar of beheerder voor gebruikerstoegang rollen.