---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 4f5dbbd1da8a221fb5f8b9641e016829987db538
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
Lees voordat u items maakt, laten we het concept van het bereik. Azure biedt vier niveaus van beheer: beheergroepen, abonnement, resourcegroep en resource. [Beheergroepen](../articles/billing/billing-enterprise-mgmt-group-overview.md) zich in een preview-versie. De volgende afbeelding toont een voorbeeld van deze lagen.

![Bereik](./media/resource-manager-governance-scope/scope-levels.png)

U toepassen beheerinstellingen op elk van deze niveaus van scope. Het niveau dat u kiest, bepaalt hoe ver de instelling wordt toegepast. Lagere niveaus overnemen-instellingen van hogere niveaus. Wanneer u een instelling op het abonnement toepast, wordt deze instelling wordt toegepast op alle resourcegroepen en resources in uw abonnement. Wanneer u een instelling toepast op toegepast de resourcegroep die instelling is de resourcegroep en alle bijbehorende resources. Een andere resourcegroep heeft echter geen die instelling.

Meestal is het handig om toe te passen kritieke instellingen op een hoger niveau en vereisten voor het project op lagere niveaus. U wilt controleren of dat alle resources voor uw organisatie zijn geïmplementeerd voor bepaalde gebieden. Om te bewerkstelligen, moet u een beleid toepassen op het abonnement waarmee de toegestane locaties. Terwijl andere gebruikers in uw organisatie nieuwe resourcegroepen en resources toevoegt, worden automatisch de toegestane locaties afgedwongen. 
