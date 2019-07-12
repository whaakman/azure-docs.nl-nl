---
title: Windows firewall gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met gegevens van Windows firewall Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: a863910ee338da5655e9f3b5610b0a8049b8b2a9
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620783"
---
# <a name="connect-windows-firewall"></a>Verbinding maken met Windows-firewall

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

De Windows firewall-connector kunt u eenvoudig verbinding maken met uw Windows Firewall-logboeken, als ze zijn verbonden met uw Azure-Sentinel-werkruimte. Deze verbinding kunt u dashboards weergeven, aangepaste waarschuwingen maken en onderzoek verbeteren. Dit biedt u meer inzicht in het netwerk van uw organisatie en verbetert uw mogelijkheden voor beveiliging bewerking. De oplossing verzamelt Windows firewall-gebeurtenissen van de Windows-machines waarop een Log Analytics-agent is geïnstalleerd. 


> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werkruimte waarop u werkt met Azure Sentinel.

## <a name="enable-the-connector"></a>De connector inschakelen 

1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors** en klik vervolgens op de **Windows firewall** tegel. 
1.  Als uw Windows-machines in Azure:
    1. Klik op **installeren van de agent op Windows Azure virtuele machine**.
    1. In de **virtuele machines** , selecteert u de Windows-machine die u wilt streamen naar Azure Sentinel. Zorg ervoor dat dit is een Windows-VM.
    1. Klik in het venster dat wordt geopend voor die VM, **Connect**.  
    1. Klik op **inschakelen** in de **Windows firewall-connector** venster. 

2. Als uw Windows-machine niet een Azure-VM is:
    1. Klik op **installeren van de agent op niet-Azure-machines**.
    1. In de **Direct agent** venster, selecteert u **downloaden Windows-agent (64 bits)** of **downloaden Windows-agent (32 bits)** .
    1. Installeer de agent op uw Windows-computer. Kopieer de **werkruimte-ID**, **primaire sleutel**, en **secundaire sleutel** en ze wanneer hierom wordt gevraagd tijdens de installatie te gebruiken.

4. Selecteer welke gegevenstypen die u wilt streamen.
5. Klik op **oplossing installeren**.
6. Zoek voor het gebruik van de relevante schema in Log Analytics voor Windows firewall, **SecurityEvent**.

## <a name="validate-connectivity"></a>Verbinding valideren

Het duurt al twintig minuten tot de logboeken in Log Analytics wordt weergegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Windows firewall Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).

