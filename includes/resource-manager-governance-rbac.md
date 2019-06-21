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
ms.openlocfilehash: f77a5d482c3f8632a3d86bd8e027fbb4418168c3
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205327"
---
U wilt er zeker van zijn dat gebruikers in uw organisatie het juiste toegangsniveau tot deze resources hebben. U wilt gebruikers geen onbeperkte toegang verlenen, maar u moet er ook voor zorgen dat ze hun werk kunnen doen. Op rollen gebaseerd toegangsbeheer (RBAC) kunt u beheren welke gebruikers gemachtigd zijn om uit te voeren van specifieke acties met een bereik. Een rol definieert een set toegestane acties. U de rol toewijzen aan een bereik en opgeven welke gebruikers deel uitmaken van die rol voor het bereik.

Geef bij de planning van een strategie voor toegangsbeheer gebruikers altijd de minimale bevoegdheden die nodig zijn om hun werk te doen. In de volgende afbeelding ziet u een voorgesteld patroon voor het toewijzen van RBAC.

![Scope](./media/resource-manager-governance-rbac/role-examples.png)

Er zijn drie rollen die betrekking hebben op alle resources - eigenaar, Inzender en lezer. Alle accounts die zijn toegewezen aan de rol van eigenaar moeten streng worden gecontroleerd en wordt zelden gebruikt. Gebruikers die alleen wilt zien van de status van oplossingen moeten worden verleend als de rol van lezer.

De meeste gebruikers krijgen [resourcespecifieke rollen](../articles/role-based-access-control/built-in-roles.md) of [aangepaste rollen](../articles/role-based-access-control/custom-roles.md) op het niveau van het abonnement of de resource. Deze rollen definiëren nauw de toegestane acties. Gebruikers aan deze rollen toewijst, kunt u de vereiste toegang voor gebruikers verlenen zonder te veel besturingselement toe te staan. U kunt een account toewijzen aan meer dan één rol en die gebruiker beschikt over de gecombineerde machtigingen van de rollen. Verlenen van toegang op het resourceniveau van de is vaak te beperkend voor gebruikers, maar mogelijk werken bij een geautomatiseerd proces dat is ontworpen voor een specifieke taak.

### <a name="who-can-assign-roles"></a>Wie kan rollen toewijzen

Om roltoewijzingen te maken en te verwijderen, moeten gebruikers `Microsoft.Authorization/roleAssignments/*`-toegang hebben. Deze toegang wordt verleend via de rol Eigenaar of Administrator voor gebruikerstoegang.