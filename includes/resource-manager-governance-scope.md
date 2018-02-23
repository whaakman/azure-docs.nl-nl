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
ms.openlocfilehash: 7a0e7e19d53d8566b859004c363ec5bbc71cb733
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
Lees voordat u items maakt, laten we het concept van het bereik. Azure biedt vier niveaus van beheer: beheergroepen, abonnement, resourcegroep en resource. [Beheergroepen](../articles/billing/billing-enterprise-mgmt-group-overview.md) zich in een preview-versie. De volgende afbeelding toont een voorbeeld van deze lagen.

![Bereik](./media/resource-manager-governance-scope/scope-levels.png)

U toepassen beheerinstellingen op elk van deze niveaus van scope. Het niveau dat u kiest, bepaalt hoe ver de instelling wordt toegepast. Lagere niveaus overnemen-instellingen van hogere niveaus. Wanneer u een instelling op het abonnement toepast, wordt deze instelling wordt toegepast op alle resourcegroepen en resources in uw abonnement. Wanneer u een instelling toepast op toegepast de resourcegroep die instelling is de resourcegroep en alle bijbehorende resources. Een andere resourcegroep heeft echter geen die instelling.

Meestal is het handig om toe te passen kritieke instellingen op een hoger niveau en vereisten voor het project op lagere niveaus. U wilt controleren of dat alle resources voor uw organisatie zijn geïmplementeerd voor bepaalde gebieden. Om te bewerkstelligen, moet u een beleid toepassen op het abonnement waarmee de toegestane locaties. Terwijl andere gebruikers in uw organisatie nieuwe resourcegroepen en resources toevoegt, worden automatisch de toegestane locaties afgedwongen. Voor een bepaald project, is het raadzaam om ervoor te zorgen dat alle resources worden gemarkeerd met een code, waarmee het project. Een beleid toepassen op de resourcegroep voor het project dat dit label wordt afgedwongen. Als u nieuwe resources gebruikers toevoegen aan de resourcegroep, wordt automatisch de tag toegevoegd. Echter gebruikers kunnen de resourcegroepen die niet zijn gerelateerd aan het project toevoegen en de tag wordt niet afgedwongen.
