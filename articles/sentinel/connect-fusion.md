---
title: Fusion in Azure Sentinel Preview inschakelen | Microsoft Docs
description: Informatie over het inschakelen van fusion in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 82becf50-6628-47e4-b3d7-18d7d72d505f
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 60aa6ab1e6eaa4cd5f5760b3d5d9a1ff16ebfc46
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56993305"
---
# <a name="enable-fusion"></a>Fusion inschakelen

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Machine Learning in Azure Sentinel is ingebouwde rechtstreeks vanaf het begin. We hebben het systeem goed gekozen ontworpen met ML-innovaties waarmee beveiligingsanalisten, security data scientists en technici productief zijn gericht. Een dergelijke innovatie wordt dat Azure Sentinel Fusion gebouwd met name als u wilt verkleinen waarschuwing intensief gebruik.  

Fusion graph aangedreven machine learning-algoritmen gebruikt om te correleren tussen miljoenen lagere betrouwbaarheid afwijkende activiteiten van andere producten, zoals Azure AD Identity Protection en Microsoft Cloud App Security, om ze te combineren in een beheersbare aantal interessante security gevallen. 

## <a name="enable-fusion"></a>Fusion inschakelen

1. Selecteer het pictogram Cloud Shell openen in de Azure-portal.
  ![Cloud Shell](./media/connect-fusion/cloud-shell.png)
 
2.  In de **Welkom bij de Cloud Shell** windows die wordt geopend, selecteert u PowerShell.

3.  Kies het abonnement waarvoor u geïmplementeerd Azure Sentinel, en **maken van de**.
 
4. Nadat u bent geverifieerd en en uw Azure-station is gebouwd, bij de opdrachtprompt, voer de volgende opdrachten uit: 
 
            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=true --subscription "{Subscription Guid}"

## <a name="disable-fusion"></a>Fusion uitschakelen

Volg dezelfde procedure als hierboven en voer de volgende opdracht uit: 

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=false --subscription "{Subscription Guid}"

## <a name="view-the-status-of-fusion"></a>De status van fusion weergeven

            az resource show --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --subscription "{Subscription Guid}"


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u om in te schakelen Fusion in Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

