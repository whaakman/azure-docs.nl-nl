---
title: Verzamelen van gegevens van de Azure-activiteit in Azure Sentinel Preview | Microsoft Docs
description: Meer informatie over het verzamelen van gegevens van de Azure-activiteit in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 42ce8d06489cfacf7ba9bb3de1425224b5df95e7
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57240572"
---
# <a name="collect-data-from-azure-activity-log"></a>Gegevens verzamelen over Azure-activiteitenlogboek

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt de logboeken van streamen [Azure-activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md) in Azure Sentinel met één klik. Het activiteitenlogboek is een abonnementlogboek biedt inzicht in de abonnement gebeurtenissen die hebben plaatsgevonden in Azure. Dit omvat een scala aan gegevens van operationele gegevens van de Azure Resource Manager-updates op Service Health-gebeurtenissen. Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor een schrijfbewerking (PUT, POST, DELETE) die wordt gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. Het activiteitenlogboek bevat geen lees (GET)-bewerkingen of bewerkingen voor resources die gebruikmaken van het klassieke / 'RDFE'-model. 


## <a name="prerequisites"></a>Vereisten

- Gebruiker met de globale beheerder of beheerder beveiligingsmachtigingen


## <a name="connect-to-azure-activity-log"></a>Verbinding maken met Azure-activiteitenlogboek

1. Selecteer in Azure Sentinel, **gegevensverzameling** en klik vervolgens op de **Azure-activiteitenlogboek** tegel.

2. Selecteer de abonnementen die u wilt streamen naar Azure Sentinel in het logboekvenster Azure-activiteit. 

3. Klik op **Verbinden**.

4. Zoek voor het gebruik van de relevante schema in Log Analytics voor de activiteit Azure-waarschuwingen, **AzureActivity**.


 

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Azure-activiteitenlogboek Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
