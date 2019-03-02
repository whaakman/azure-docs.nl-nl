---
title: Met behulp van de Azure Cloud Shell-venster | Microsoft Docs
description: Overzicht van het gebruik van de Azure Cloud Shell-venster.
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: damaerte
ms.openlocfilehash: a02642540e6eb39f35b9cc0d38d187a7afa36b7a
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57243445"
---
# <a name="using-the-azure-cloud-shell-window"></a>Met behulp van de Azure Cloud Shell-venster

Dit document wordt uitgelegd hoe u met behulp van de Cloud Shell-venster.

## <a name="swap-between-bash-and-powershell-environments"></a>Wisselen tussen omgevingen met Bash en PowerShell
![](media/using-the-shell-window/env-selector.png)

De omgevingsselectie in de Cloud Shell-werkbalk gebruiken om te schakelen tussen omgevingen met Bash en PowerShell.

## <a name="restart-cloud-shell"></a>Cloud Shell opnieuw starten
![](media/using-the-shell-window/restart.png)
> [!WARNING]
> Cloud Shell opnieuw start, wordt de status van machine opnieuw ingesteld en de bestanden niet-persistent door uw Azure-bestandsshare, gaan verloren.

* Klik op het pictogram opnieuw opstarten in de Cloud Shell-werkbalk MACHINESTATUS opnieuw instellen.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimaliseren & Cloud Shell-venster maximaliseren
![](media/using-the-shell-window/minmax.png)
* Klik op het pictogram minimaliseren in de rechterbovenhoek van het venster om deze te verbergen. Klik op het pictogram van de Cloud Shell opnieuw te zichtbaar maken.
* Klik op het pictogram maximaliseren om het venster maximale hoogte instellen. Als u wilt herstellen venster vorige formaat, klik op herstellen.

## <a name="concurrent-sessions"></a>Gelijktijdige sessies
Cloudshell biedt meerdere gelijktijdige sessies voor browsertabbladen doordat elke sessie bestaat als een afzonderlijk Bash-proces.
Als u een sessie afsluit, zorg er dan voor dat elke sessievenster afsluiten terwijl elk proces wordt onafhankelijk uitgevoerd, hoewel ze op dezelfde computer worden uitgevoerd.

## <a name="copy-and-paste"></a>Kopiëren en plakken
[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Het formaat van Cloud Shell-venster
* Klik en sleep de bovenrand van de werkbalk omhoog of omlaag naar de Cloud Shell-venster formaat.

## <a name="scrolling-text-display"></a>Schuiven tekst weer te geven
* Schuiven met de muis of het touchpad om terminal tekst te verplaatsen.

## <a name="changing-the-text-size"></a>De tekengrootte wijzigen
![](media/using-the-shell-window/text-size.png)
* Klik op het Instellingenpictogram in de rechterbovenhoek van het venster links en vervolgens Beweeg de muisaanwijzer over de optie ' Text ' en selecteer de gewenste tekengrootte. Uw selectie wordt in verschillende sessies worden vastgehouden.

## <a name="exit-command"></a>Afsluitopdracht
Met `exit` de actieve sessie wordt beëindigd. Dit gedrag is echter standaard na 20 minuten zonder tussenkomst van de.

## <a name="next-steps"></a>Volgende stappen

[Bash in Cloud Shell snelstartgids](quickstart.md)
[PowerShell in Cloud Shell-snelstartgids](quickstart-powershell.md)