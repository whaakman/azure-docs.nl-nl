---
title: Verbinding maken met de DNS-gegevens in Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u DNS-gegevens in Azure Sentinel verbindt.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 77af84f9-47bc-418e-8ce2-4414d7b58c0c
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 1c79aad557efb85a8797584c33c74983ef645d07
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611320"
---
# <a name="connect-your-domain-name-server"></a>Verbinding maken met uw DNS-server

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt een Server DNS (Domain Name) die worden uitgevoerd op Windows Azure Sentinel verbinden. Dit wordt gedaan door het installeren van een agent op de DNS-machine. Met behulp van DNS registreert, krijgt u beveiliging, prestaties en bewerkingen met betrekking tot inzicht in de DNS-infrastructuur van uw organisatie door te verzamelen, analyseren, en correleren van analytische en auditlogboeken en andere gerelateerde gegevens uit de DNS-servers.

Wanneer u verbinding met het DNS-inschakelt, kunt u het volgende doen:
- Clients identificeren die op te lossen schadelijke domeinnamen
- Verlopen bronrecords identificeren
- Vaak opgevraagde domeinnamen en DNS-clients dremepelwaarde identificeren
- Weergave aanvraagbelasting op DNS-servers
- Dynamische DNS-registratie van mislukte pogingen weergeven

## <a name="connected-sources"></a>Verbonden bronnen

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing:

| **Verbonden bron** | **Ondersteuning** | **Beschrijving** |
| --- | --- | --- |
| [Windows-agents](../azure-monitor/platform/agent-windows.md) | Ja | De oplossing verzamelt DNS-gegevens van Windows-agents. |
| [Linux-agents](../azure-monitor/learn/quick-collect-linux-computer.md) | Nee | De oplossing verzamelt geen DNS-gegevens van directe Linux-agents. |
| [System Center Operations Manager-beheergroep](../azure-monitor/platform/om-agents.md) | Ja | De oplossing verzamelt DNS-gegevens van agents in een verbonden beheergroep van Operations Manager. Een directe verbinding van de Operations Manager-agent naar Azure Monitor is niet vereist. Gegevens uit de beheergroep doorgestuurd naar de Log Analytics-werkruimte. |
| [Azure Storage-account](../azure-monitor/platform/collect-azure-metrics-logs.md) | Nee | Azure storage wordt niet gebruikt door de oplossing. |

### <a name="data-collection-details"></a>Details van de verzameling gegevens

De oplossing verzamelt DNS-inventarisatie- en DNS-gebeurtenis met betrekking tot gegevens van de DNS-servers waarop een Log Analytics-agent is geïnstalleerd. Inventarisatie-gerelateerde gegevens, zoals het aantal DNS-servers, -zones en -bronrecords worden verzameld door het uitvoeren van de DNS PowerShell-cmdlets. De gegevens worden eenmaal per twee dagen bijgewerkt. De gebeurtenis-gerelateerde gegevens worden verzameld in de buurt van real-time uit de [analytische en auditlogboeken](https://technet.microsoft.com/library/dn800669.aspx#enhanc) geleverd door de verbeterde DNS-logboekregistratie en diagnostische gegevens in Windows Server 2012 R2.


## <a name="connect-your-dns-appliance"></a>Verbinding maken met uw DNS-apparaat

1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors** en kies de **DNS** tegel.
1. Als uw DNS-machines in Azure:
    1. Klik op **installeren van de agent op Windows Azure virtuele machine**.
    1. In de **virtuele machines** , selecteert u de DNS-machine die u wilt streamen naar Azure Sentinel. Zorg ervoor dat dit is een Windows-VM.
    1. Klik in het venster dat wordt geopend voor die VM, **Connect**.  
    1. Klik op **inschakelen** in de **DNS connector** venster. 

2. Als uw DNS-machine bevindt zich niet in een Azure-VM:
    1. Klik op **installeren van de agent op niet-Azure-machines**.
    1. In de **Direct agent** venster, selecteert u **downloaden Windows-agent (64 bits)** of **downloaden Windows-agent (32 bits)** .
    1. Installeer de agent op uw DNS-computer. Kopieer de **werkruimte-ID**, **primaire sleutel**, en **secundaire sleutel** en ze wanneer hierom wordt gevraagd tijdens de installatie te gebruiken.

3. Zoek voor het gebruik van de relevante schema in Log Analytics voor de DNS-Logboeken, **DnsEvents**.

## <a name="validate"></a>Valideren 

Zoek in Log Analytics, het schema **DnsEvents** en zorg ervoor dat er gebeurtenissen zijn.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u DNS-on-premises apparaten verbinden met Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
