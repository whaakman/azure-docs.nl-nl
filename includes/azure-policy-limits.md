---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 57cec39bde460c6079091490acf541761c61e003
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553630"
---
Er is een maximum aantal exemplaren voor elk objecttype voor Azure Policy. De vermelding _Bereik_ slaat op het abonnement of op de [beheergroep](../articles/governance/management-groups/overview.md).

| Waar | Wat | Maximum |
|---|---|---|
| Bereik | Beleidsdefinities | 250 |
| Bereik | Initiatiefdefinities | 100 |
| Tenant | Initiatiefdefinities | 1000 |
| Bereik | Toewijzingen van beleid of initiatief | 100 |
| Beleidsdefinitie | Parameters | 20 |
| Initiatiefdefinitie | Beleidsregels | 100 |
| Initiatiefdefinitie | Parameters | 100 |
| Toewijzingen van beleid of initiatief | Uitzonderingen (geen bereiken) | 250 |
| Beleidsregel | Geneste voorwaarden | 512 |
