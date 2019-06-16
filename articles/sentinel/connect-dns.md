---
title: Verbinding maken met de DNS-gegevens in Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u DNS-gegevens in Azure Sentinel verbindt.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 77af84f9-47bc-418e-8ce2-4414d7b58c0c
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 6429568b33ece3ed4f26614e55e8c3069dd65d71
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65204409"
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

## <a name="how-it-works"></a>Hoe werkt het?

DNS-verbinding wordt gerealiseerd door het installeren van een agent op de DNS-machine. De agent haalt gebeurtenissen uit de DNS-server en geeft deze door aan de Log Analytics.

## <a name="connect-your-dns-appliance"></a>Verbinding maken met uw DNS-apparaat

1. Selecteer in de portal voor Azure Sentinel **gegevensconnectors** en kies de **DNS** tegel.
1. Als uw DNS-machines in Azure:
    1. Klik op **downloaden en installeren van de agent voor Windows-machines**.
    1. In de **virtuele machines** , selecteert u de DNS-machine die u wilt streamen naar Azure Sentinel. Zorg ervoor dat dit is een Windows-VM.
    1. Klik in het venster dat wordt geopend voor die VM, **Connect**.  
    1. Klik op **inschakelen** in de **DNS connector** venster. 

2. Als uw DNS-machine bevindt zich niet in een Azure-VM:
    1. Klik op **downloaden en installeren van de agent voor Windows-niet-Azure-machines**.
    1. In de **Direct agent** venster, selecteert u **downloaden Windows-agent (64 bits)** of **downloaden Windows-agent (32 bits)** .
    1. Installeer de agent op uw DNS-computer. Kopieer de **werkruimte-ID**, **primaire sleutel**, en **secundaire sleutel** en ze wanneer hierom wordt gevraagd tijdens de installatie te gebruiken.

3. Zoek voor het gebruik van de relevante schema in Log Analytics voor de DNS-Logboeken, **DnsEvents**.

## <a name="validate"></a>Valideren 

Zoek in Log Analytics, het schema **DnsEvents** en zorg ervoor dat er gebeurtenissen zijn.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u DNS-on-premises apparaten verbinden met Azure Sentinel. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
