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
ms.openlocfilehash: a03f2b4e8d216db3764af03dc06b5188289ffc92
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50964200"
---
Op rollen gebaseerd toegangsbeheer is een strategie voor beveiliging op basis van de overname voor het beheren van toegang, machtigingen en rollen. Onderliggende functies overnemen machtigingen van bovenliggende rollen. Machtigingen kunnen ook worden toegewezen zonder wordt overgenomen van een bovenliggende rol. Ze kunnen ook worden toegewezen aan een rol aanpassen, indien nodig.

Een beheerder van de ruimte moet mogelijk globale toegang tot alle bewerkingen voor een opgegeven ruimte worden uitgevoerd. Toegang tot bevat alle knooppunten onder of in de ruimte. Het installatieprogramma van een apparaat moet mogelijk alleen *lezen* en *bijwerken* machtigingen voor apparaten en sensoren.

In elk geval functies worden verleend *precies en niet langer zijn dan de vereiste toegang* om te voldoen aan hun taken per het principe van minimale bevoegdheden. Op basis van dit principe een identiteit die wordt verleend *alleen*:

* De mate van toegang die nodig zijn om de taak te voltooien.
* Een rol geschikt en zijn beperkt tot de taak is uitgevoerd.

>[!IMPORTANT]
> Voer altijd het principe van minimale bevoegdheden.

Twee andere belangrijke op rollen gebaseerde toegang besturingselement procedures te volgen:

> [!div class="checklist"]
> * Controleer regelmatig de roltoewijzingen om te controleren dat elke rol de juiste machtigingen heeft.
> * Opschonen van rollen en -toewijzingen wanneer personen rollen of toewijzingen wijzigen.