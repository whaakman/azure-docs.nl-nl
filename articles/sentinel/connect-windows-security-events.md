---
title: Verzamelen van gebeurtenisgegevens van de Windows-beveiliging in Azure Sentinel Preview | Microsoft Docs
description: Meer informatie over het verzamelen van gebeurtenisgegevens van de Windows-beveiliging in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 3c79747bf33e1769af5f8d3589904ba15105f216
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58087598"
---
# <a name="connect-windows-security-events"></a>Verbinding maken met Windows-beveiligingsgebeurtenissen 

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt alle beveiligingsgebeurtenissen van de Windows-Servers die zijn verbonden met uw werkruimte Azure Sentinel streamen. Deze verbinding kunt u dashboards weergeven, aangepaste waarschuwingen maken en onderzoek verbeteren. Dit biedt u meer inzicht in het netwerk van uw organisatie en verbetert uw mogelijkheden voor beveiliging bewerking.  U kunt selecteren welke gebeurtenissen om te streamen:

- **Alle gebeurtenissen** -alle Windows-beveiligings- en AppLocker-gebeurtenissen.
- **Algemene** -een standaardset gebeurtenissen voor controledoeleinden.
- **Minimale** -een klein aantal gebeurtenissen die op mogelijke bedreigingen wijzen kunnen. Als deze optie inschakelt, kunt u zich niet aan een volledige audittrail hebben.
- **Geen** -geen beveiligings- of AppLocker-gebeurtenissen.

> [!NOTE]
> 
> - Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="set-up-the-windows-security-events-connector"></a>De Windows security gebeurtenissen connector instellen

Om uw Windows-beveiligingsgebeurtenissen volledig te integreren met Azure Sentinel:

1. Selecteer in de portal voor Azure Sentinel **gegevensverzameling** en klik vervolgens op de **Windows-beveiligingsgebeurtenissen** tegel. 
1. Selecteer welke gegevenstypen die u wilt streamen.
1. Klik op **Update**.


## <a name="validate-connectivity"></a>Verbinding valideren

Het duurt ongeveer 20 minuten totdat de logboeken in Log Analytics wordt weergegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Windows-beveiligingsgebeurtenissen Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

