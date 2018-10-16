---
title: bestand opnemen
description: bestand opnemen
services: azure-digital-twins
author: adamgerard
ms.service: azure-digital-twins
ms.topic: include
ms.date: 09/27/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 075587df445b46c8b03c5448cebf8cd8b12f1179
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324075"
---
Toegangsbeheer op basis van rollen is een strategie voor beveiliging op basis van de overname voor het beheren van toegang, machtigingen en rollen. Onderliggende functies overnemen machtigingen van bovenliggende rollen. Machtigingen kunnen ook worden toegewezen zonder wordt overgenomen van een bovenliggende rol. Ze kunnen ook worden toegewezen aan een rol aanpassen naar behoefte.

Bijvoorbeeld, een **ruimte beheerder** moet mogelijk de wereldwijde toegang tot alle bewerkingen voor een opgegeven (met inbegrip van alle knooppunten onder of binnen het) worden uitgevoerd terwijl een **apparaat installatieprogramma** hoeft alleen *lezen* en *bijwerken* machtigingen voor apparaten en sensoren.

In elk geval rollen moeten worden verleend **precies en niet langer zijn dan de vereiste toegang** om te voldoen aan hun taken per de **principe van minimale bevoegdheden**, die een identiteit verleent *alleen*:

* De mate van toegang die nodig zijn om de taak te voltooien.
* Een rol geschikt en zijn beperkt tot de taak is uitgevoerd.

>[!IMPORTANT]
> **Voer altijd het principe van minimale bevoegdheden**.

Twee andere belangrijke Role-Based Access Control procedures te volgen:

> [!div class="checklist"]
> * Controleer regelmatig de roltoewijzingen om te controleren dat elke rol de juiste machtigingen heeft.
> * Rollen en -toewijzingen moeten worden gereinigd-up zoals personen rollen of toewijzingen wijzigen.