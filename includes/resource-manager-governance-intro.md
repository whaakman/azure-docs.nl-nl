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
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
Bij het implementeren van resources in Azure, hebben grote flexibiliteit bij het bepalen van welke typen resources te implementeren en waar ze zich bevinden en hoe deze moeten worden ingesteld. Deze flexibiliteit kan echter meer opties dan u wilt toestaan dat in uw organisatie te openen. Als u rekening houden met het implementeren van resources in Azure, wellicht u:

* Hoe ik voldoen aan wettelijke vereisten voor onafhankelijkheid van de gegevens in bepaalde landen?
* Hoe bepaal ik kosten
* Hoe kan ik ervoor zorgen dat iemand niet per ongeluk een kritieke systeem wijzigt?
* Hoe ik resourcekosten bijhouden en nauwkeurig factureren?

In dit artikel komen deze vragen. U in het bijzonder:

> [!div class="checklist"]
* Gebruikers toewijzen aan rollen en de rollen toewijzen aan een bereik, zodat gebruikers hebben een machtiging verwachte acties, maar niet meer acties uitvoeren.
* Beleidsregels die conventies voor resources in uw abonnement voorschrijven.
* Resources die essentieel voor uw systeem zijn vergrendelen.
* Resources taggen zodat u ze kunt bijhouden door waarden die geschikt zijn voor uw organisatie.

Dit artikel is gericht op de taken die u voor het implementeren van beheeracties uitvoeren. Zie voor een uitgebreidere bespreking van de concepten [Governance in Azure](../articles/security/governance-in-azure.md). 
