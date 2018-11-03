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
ms.openlocfilehash: 0a54dfdb810ea578c1e7c8fcc7ca0343e72164ae
ms.sourcegitcommit: 3dcb1a3993e51963954194ba2a5e42260d0be258
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50964559"
---
Voor elk objecttype in Azure Policy bestaat er een maximum. De vermelding _Bereik_ slaat op het abonnement of op de [beheergroep](../articles/governance/management-groups/overview.md).

| Waar | Wat | Maximum |
|---|---|---|
| Bereik | Definities voor beleid | 250 |
| Bereik | Initiatiefdefinities | 100 |
| Tenant | Initiatiefdefinities | 1000 |
| Bereik | Beleids-/initiatieftoewijzingen | 100 |
| Beleidsdefinitie | Parameters | 20 |
| Initiatiefdefinitie | Beleidsregels | 100 |
| Initiatiefdefinitie | Parameters | 100 |
| Beleids-/initiatieftoewijzingen | Uitzonderingen (geen bereiken) | 250 |
| Beleidsregel | Geneste voorwaarden | 512 |
