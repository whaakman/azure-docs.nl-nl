---
title: bestand opnemen
description: bestand opnemen
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2c16e82ccf259a4cc5ae8fcf35b2dd6b5d50ee2d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740545"
---
Bij het implementeren van resources naar Azure hebt u enorm veel flexibiliteit bij het bepalen welke typen resources u wilt implementeren, waar ze zich bevinden en hoe u ze instelt. Deze flexibiliteit biedt mogelijk echter meer opties dan u in uw organisatie wilt toestaan. Wanneer u de implementatie van resources naar Azure overweegt, vraagt u zich misschien het volgende af:

* Hoe kan ik voldoen aan wettelijke vereisten voor de gegevenssoevereiniteit in bepaalde landen?
* Hoe kan ik de kosten in de hand houden?
* Hoe kan ik voorkomen dat iemand per ongeluk een kritisch systeem verandert?
* Hoe kan ik resourcekosten bijhouden en deze nauwkeurig factureren?

In dit artikel worden deze vragen behandeld. U kunt met name:

> [!div class="checklist"]
* Gebruikers toewijzen aan rollen en de rollen toewijzen aan een bereik, zodat gebruikers gemachtigd zijn om verwachte acties uit te voeren, maar niet meer acties.
* Beleidsregels toepassen die conventies voor resources in uw abonnement voorschrijven.
* Resources vergrendelen die essentieel zijn voor uw systeem.
* Tags toevoegen aan resources zodat u ze kunt bijhouden op basis van waarden die zinvol zijn voor uw organisatie.

Dit artikel is gericht op de taken voor het implementeren van governance. Zie [Governance in Azure](../articles/security/governance-in-azure.md) voor een uitgebreidere bespreking van de concepten. 
