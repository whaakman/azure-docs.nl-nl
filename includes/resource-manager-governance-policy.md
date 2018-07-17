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
ms.openlocfilehash: c04514218c7ed8dfd72b94345d2deb88e663fda1
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740539"
---
[Azure-beleid](/azure/azure-policy/) helpt u ervoor te zorgen dat alle resources in het abonnement voldoen aan de bedrijfsnormen. Gebruik beleidsregels om uw kosten te verlagen door implementatie-opties te beperken tot alleen de resourcetypen en SKU's die zijn goedgekeurd. U definieert regels en acties voor uw resources. Deze regels worden dan automatisch afgedwongen tijdens de implementatie. U kunt bijvoorbeeld bepalen welke typen resources worden geïmplementeerd. Of u kunt de goedgekeurde locaties voor bronnen beperken. Met sommige beleidsregels wordt een actie geweigerd en met andere wordt de controle van een actie ingesteld.

Azure Policy is een aanvulling op op rollen gebaseerd toegangsbeheer (RBAC). RBAC is gericht op de toegang van gebruikers en is een systeem dat is gebaseerd op standaard weigeren en expliciet toestaan. Azure Policy is gericht op resource-eigenschappen tijdens en na de implementatie. Het is een systeem dat is gebaseerd op standaard toestaan en expliciet weigeren.

Hierbij zijn er twee beleidsconcepten die u moet begrijpen: *beleidsdefinities* en *beleidstoewijzingen*. Een beleidsdefinitie beschrijft de beheervoorwaarden die u wilt afdwingen. Een beleidstoewijzing zet een beleidsdefinitie in werking voor een bepaald bereik.

![Beleid toewijzen](./media/resource-manager-governance-policy/policy-concepts.png)

Azure biedt verschillende ingebouwde beleidsdefinities die u zonder enige wijziging kunt gebruiken. U geeft parameterwaarden door om de waarden op te geven die zijn toegestaan binnen uw bereik. Als een ingebouwde beleidsdefinitie niet aan uw vereisten voldoet, kunt u [aangepaste beleidsdefinities maken](../articles/azure-policy/create-manage-policy.md).
