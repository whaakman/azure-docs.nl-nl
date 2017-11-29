---
title: Met behulp van het venster Azure Cloud Shell | Microsoft Docs
description: Overzicht van het gebruik van het venster Azure Cloud-Shell.
services: azure
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: juluk
ms.openlocfilehash: 4eb5680c618d78e0722e1eb4a0f551f26b4dc902
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="using-the-azure-cloud-shell-window"></a>Met behulp van het venster Azure Cloud Shell

Dit document wordt uitgelegd hoe u het venster Cloud Shell gebruiken.

## <a name="swap-between-bash-and-powershell-environments"></a>Wisselen tussen Bash en PowerShell-omgevingen
![](media/using-the-shell-window/env-selector.png)

Gebruik de selector omgeving op de werkbalk Cloud Shell wisselen tussen Bash en PowerShell-omgevingen.

## <a name="restart-cloud-shell"></a>Cloud-Shell starten
![](media/using-the-shell-window/restart.png)
> [!WARNING]
> Cloud-Shell opnieuw te starten, wordt MACHINESTATUS opnieuw instellen en alle bestanden niet door uw Azure-bestandsshare niet verloren.

* Klik op het pictogram opnieuw opstarten in de werkbalk Cloud Shell opnieuw instellen van status van de machine.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimaliseren & Cloud Shell venster maximaliseren
![](media/using-the-shell-window/minmax.png)
* Klik op het pictogram minimaliseren bovenaan rechts van het venster om deze te verbergen. Klik op het pictogram Cloud Shell opnieuw voor het zichtbaar maken.
* Klik op het pictogram om het venster ingesteld op maximumhoogte voor maximaliseren. Klik op herstellen om het venster herstellen tot vorige grootte.

## <a name="concurrent-sessions"></a>Gelijktijdige sessies
Cloud-Shell kan meerdere gelijktijdige sessies over browsertabbladen doordat elke sessie bestaan als een afzonderlijk Bash-proces.
Als een sessie wordt afgesloten, zorg er dan voor dat elke sessievenster afsluiten als elk proces wordt onafhankelijk uitgevoerd, hoewel ze op dezelfde computer worden uitgevoerd.

## <a name="copy-and-paste"></a>Kopiëren en plakken
[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Het formaat van Cloud-Shell-venster
* Klik en sleep de bovenrand van de werkbalk omhoog of omlaag naar het formaat van het Cloud-Shell-venster.

## <a name="scrolling-text-display"></a>Tekstweergave schuiven
* Schuiven met de muis of touchpad om terminal tekst te verplaatsen.

## <a name="changing-the-text-size"></a>De tekengrootte wijzigen
![](media/using-the-shell-window/text-size.png)
* Klik op het Instellingenpictogram op de bovenste links van het venster en vervolgens de muisaanwijzer op de optie ' Text ' en selecteer de gewenste tekengrootte. Uw selectie wordt over de sessies worden vastgehouden.

## <a name="exit-command"></a>Afsluitopdracht
Met `exit` de actieve sessie wordt beëindigd. Dit gebeurt standaard na 20 minuten zonder interactie.

## <a name="next-steps"></a>Volgende stappen

[In de Cloud Shell snel Bash](quickstart.md)
[PowerShell in de Cloud Shell Quick Start](quickstart-powershell.md)