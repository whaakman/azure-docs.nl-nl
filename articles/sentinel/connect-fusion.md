---
title: Fusie inschakelen in azure Sentinel preview | Microsoft Docs
description: Meer informatie over het inschakelen van Fusion in azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 82becf50-6628-47e4-b3d7-18d7d72d505f
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: f90bb86632886d8a2ca6c6e8c60d3b79f5d0b9e9
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780446"
---
# <a name="enable-fusion"></a>Fusie inschakelen

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Machine Learning in azure Sentinel is vanaf het begin ingebouwd. We hebben het systeem goed ontworpen met ML-innovaties om beveiligings analisten, beveiligings gegevens en technici productief te maken. Een van de innovaties is Azure Sentinel Fusion, met name om waarschuwings intensiefheid te verminderen.

Fusion maakt gebruik van gestroomde machine learning algoritmen van de grafiek om te correleren tussen miljoenen minder afwijkende activiteiten van verschillende producten, zoals Azure AD Identity Protection en Microsoft Cloud App Security, om ze te combi neren in een beheersbaar aantal interessante beveiligings incidenten.

## <a name="enable-fusion"></a>Fusie inschakelen

1. Selecteer in de Azure Portal het pictogram om Cloud Shell te openen.
  ![Cloud Shell](./media/connect-fusion/cloud-shell.png)

2.  Selecteer Power shell in de **Welkom bij Cloud shell** Vensters die hieronder worden weer gegeven.

3.  Kies het abonnement waarop u Azure Sentinel hebt geïmplementeerd en **Maak opslag ruimte**.

4. Nadat u bent geverifieerd en uw Azure-station is gebouwd, voert u bij de opdracht prompt de volgende opdrachten uit:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=true --subscription "{Subscription Guid}"

## <a name="disable-fusion"></a>Fusie uitschakelen

Volg dezelfde procedure als hierboven en voer de volgende opdracht uit:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=false --subscription "{Subscription Guid}"

## <a name="view-the-status-of-fusion"></a>De status van Fusion weer geven

            az resource show --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --subscription "{Subscription Guid}"


## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u Fusion in azure Sentinel inschakelt. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

