---
title: Verzamelen van gegevens van de Windows firewall in Azure Sentinel Preview | Microsoft Docs
description: Meer informatie over het verzamelen van gegevens van de Windows firewall in Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/28/2019
ms.author: rkarlin
ms.openlocfilehash: 3839d81f70b8bc6dcb1da3c4dd77f52443294707
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574837"
---
# <a name="connect-windows-firewall"></a>Verbinding maken met Windows-firewall

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

De Windows firewall-connector kunt u eenvoudig verbinding maken met uw Windows Firewall-logboeken, als ze zijn verbonden met uw Azure-Sentinel-werkruimte. Deze verbinding kunt u dashboards weergeven, aangepaste waarschuwingen maken en onderzoek verbeteren. Dit biedt u meer inzicht in het netwerk van uw organisatie en verbetert uw mogelijkheden voor beveiliging bewerking.  


> [!NOTE]
> 
> - Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="enable-the-connector"></a>De connector inschakelen 

1. Selecteer in de portal voor Azure Sentinel **gegevensverzameling** en klik vervolgens op de **Windows firewall** tegel. 
1. Selecteer welke gegevenstypen die u wilt streamen.
1. Klik op **Install**.
6. Zoek voor het gebruik van de relevante schema in Log Analytics voor Windows firewall, **SecurityEvent**.

## <a name="validate-connectivity"></a>Verbinding valideren

Het duurt al twintig minuten tot de logboeken in Log Analytics wordt weergegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Windows firewall Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

