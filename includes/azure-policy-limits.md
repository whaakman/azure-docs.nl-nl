---
title: bestand opnemen
description: bestand opnemen
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/16/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 0e5d1214a8e1af8299cb40d1a3b31ff6eafc8c5c
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40185114"
---
Voor elk objecttype in Azure Policy bestaat er een maximum. De vermelding _Bereik_ slaat op het abonnement of op de [beheergroep](../articles/azure-resource-manager/management-groups-overview.md).

| Waar | Wat | Maximum |
|---|---|---|
| Bereik | Definities voor beleid | 250 |
| Bereik | Initiatiefdefinities | 100 |
| Tenant | Initiatiefdefinities | 1000 |
| Bereik | Beleids-/initiatieftoewijzingen | 100 |
| Beleidsdefinitie | Parameters | 20 |
| Initiatiefdefinitie | Beleidsregels | 100 |
| Initiatiefdefinitie | Parameters | 100 |
| Beleids-/initiatieftoewijzingen | Uitzonderingen (geen bereiken) | 100 |
| Beleidsregel | Geneste voorwaarden | 512 |
