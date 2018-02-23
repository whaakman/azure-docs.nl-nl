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
ms.openlocfilehash: c1e490aacdc9f712c6e186e56fe35fd5872fb575
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
[Azure beleid](/azure/azure-policy/) kunt u ervoor dat alle bronnen in het abonnement voldoen aan bedrijfsnormen. Beleid gebruiken om uw kosten te verlagen door de implementatie-opties beperken tot alleen de resourcetypen en SKU's die zijn goedgekeurd. Definieert u regels en acties voor uw resources en deze regels automatisch worden afgedwongen tijdens de implementatie. U kunt bijvoorbeeld bepalen welke typen resources die zijn geïmplementeerd. Of u kunt de goedgekeurde locaties voor bronnen beperken. Een actie voor weigeren van sommige beleidsregels en sommige beleidsregels instellen van de controle van een actie.

Beleid is een aanvulling op rollen gebaseerd toegangsbeheer (RBAC). RBAC is gericht op toegang voor gebruikers en is een standaard weigeren en expliciete dat systeem. Beleid is gericht op broneigenschappen tijdens en na de implementatie. Het toestaan van een standaard en expliciete system weigeren.

Er zijn twee concepten met beleid - beleidsdefinities en beleidstoewijzingen begrijpen. Een beleidsdefinitie beschrijft de management-voorwaarden die u wilt afdwingen. Een beleidstoewijzing plaatst de beleidsdefinitie van een in de actie voor een bepaald bereik.

![Beleid toe te wijzen](./media/resource-manager-governance-policy/policy-concepts.png)

Azure biedt diverse ingebouwde beleidsdefinities die u zonder dat aanpassingen gebruiken kunt. U doorgeven parameterwaarden de waarden die zijn toegestaan in uw bereik wilt opgeven. Als de ingebouwde beleidsdefinitie niet te voldoen aan uw behoeften, kunt u [definities van aangepast beleid maken](../articles/azure-policy/create-manage-policy.md).

### <a name="who-can-create-and-assign-policies"></a>Wie kan maken en toewijzen van beleid

Als u beleid wilt gebruiken, moet u eerst worden geverifieerd via RBAC. Uw account heeft specifiek het volgende nodig:

* `Microsoft.Authorization/policydefinitions/write`-machtiging voor het definiëren van beleid.
* `Microsoft.Authorization/policyassignments/write`-machtiging voor het toepassen van beleid.

Deze machtigingen niet zijn opgenomen in de **Inzender** rol.
