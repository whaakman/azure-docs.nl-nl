---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 06/05/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: ea87e51e66985f860cd6d10595a32facde1dc639
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133184"
---
Er is een maximum aantal exemplaren voor elk objecttype voor Azure Policy. De vermelding _Bereik_ slaat op het abonnement of op de [beheergroep](../articles/governance/management-groups/overview.md).

| Waar | Wat | Maximum |
|---|---|---|
| Scope | Beleidsdefinities | 500 |
| Scope | Initiatiefdefinities | 100 |
| Tenant | Initiatiefdefinities | 1000 |
| Scope | Toewijzingen van beleid of initiatief | 100 |
| Beleidsdefinitie | Parameters | 20 |
| Initiatiefdefinitie | Beleidsregels | 100 |
| Initiatiefdefinitie | Parameters | 100 |
| Toewijzingen van beleid of initiatief | Uitzonderingen (geen bereiken) | 400 |
| Beleidsregel | Geneste voorwaarden | 512 |
