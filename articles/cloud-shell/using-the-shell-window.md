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
ms.date: 04/15/2019
ms.author: damaerte
ms.openlocfilehash: 2511f2c8fb706e232cde9ee4c02c7f8114bd3a2b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60200609"
---
# <a name="using-the-azure-cloud-shell-window"></a>Met behulp van de Azure Cloud Shell-venster

Dit document wordt uitgelegd hoe u met behulp van de Cloud Shell-venster.

## <a name="swap-between-bash-and-powershell-environments"></a>Wisselen tussen omgevingen met Bash en PowerShell

De omgevingsselectie in de Cloud Shell-werkbalk gebruiken om te schakelen tussen omgevingen met Bash en PowerShell.  
![Omgeving selecteren](media/using-the-shell-window/env-selector.png)

## <a name="restart-cloud-shell"></a>Cloudshell opnieuw starten
Klik op het pictogram opnieuw opstarten in de Cloud Shell-werkbalk MACHINESTATUS opnieuw instellen.  
![Cloudshell opnieuw starten](media/using-the-shell-window/restart.png)
> [!WARNING]
> Cloud Shell opnieuw start, wordt de status van machine opnieuw ingesteld en de bestanden niet-persistent door uw Azure-bestandsshare, gaan verloren.

## <a name="change-the-text-size"></a>De tekengrootte wijzigen
Klik op het Instellingenpictogram in de rechterbovenhoek van het venster links en vervolgens Beweeg de muisaanwijzer over de optie ' Text ' en selecteer de gewenste tekengrootte. Uw selectie wordt in verschillende sessies worden vastgehouden.
![Tekengrootte](media/using-the-shell-window/text-size.png)

## <a name="change-the-font"></a>Het lettertype wijzigen
Klik op het Instellingenpictogram in de rechterbovenhoek van het venster links en vervolgens Beweeg de muisaanwijzer over de optie 'Lettertype' en selecteer de gewenste lettertype.  Uw selectie wordt in verschillende sessies worden vastgehouden.
![Lettertype](media/using-the-shell-window/text-font.png)

## <a name="upload-and-download-files"></a>Bestanden uploaden en downloaden
Klik op het pictogram uploaden/downloaden van bestanden in de rechterbovenhoek van het venster links, selecteer vervolgens uploaden of downloaden.  
![Bestanden uploaden/downloaden](media/using-the-shell-window/uploaddownload.png)
* Gebruik voor het uploaden van bestanden, het pop-upvenster bladeren naar het bestand op uw lokale computer, selecteert u het gewenste bestand en klik op de knop 'Geopend'.  Het bestand wordt geüpload naar de `/home/user` directory.
* Voor het downloaden van bestanden, de volledig gekwalificeerde pad naar aangaan met het pop-upvenster en selecteer de knop 'Downloaden'.  
> [!NOTE] 
> Bestanden en paden zijn hoofdlettergevoelig in Cloud Shell. Controleer uw hoofdlettergebruik in het bestandspad.

## <a name="open-another-cloud-shell-window"></a>Een andere Cloud Shell-venster openen
Cloudshell biedt meerdere gelijktijdige sessies voor browsertabbladen doordat elke sessie bestaat als een afzonderlijk proces.
Als u een sessie afsluit, zorg er dan voor dat elke sessievenster afsluiten terwijl elk proces wordt onafhankelijk uitgevoerd, hoewel ze op dezelfde computer worden uitgevoerd.  
Klik op het pictogram van de nieuwe sessie openen in de rechterbovenhoek van het venster links. Een nieuw tabblad geopend met een andere sessie verbinding met de bestaande container.
![Nieuwe sessie openen](media/using-the-shell-window/newsession.png)

## <a name="cloud-shell-editor"></a>Cloud Shell-editor
* Raadpleeg de [met behulp van de Azure Cloud Shell-editor](using-cloud-shell-editor.md) pagina.

## <a name="web-preview"></a>Web-preview
Klik op het pictogram van de Preview-versie web-bovenaan links van het venster, selecteert u 'Configureren', geef de gewenste poort te openen.  Selecteer een van beide 'Open poort' alleen de poort te openen of 'Open en blader' de poort te openen en bekijken van de poort in een nieuw tabblad.  
![Web-preview](media/using-the-shell-window/preview.png)  
<br>
![Poort configureren](media/using-the-shell-window/preview-configure.png)  
Klik op het pictogram van de Preview-versie web-bovenaan links van het venster selecteren 'Preview poort...' om een voorbeeld van een open poort in een nieuw tabblad. Klik op het pictogram van de Preview-versie web-bovenaan links van het venster selecteert u 'Sluit poort...' de open poort sluiten.  
![Preview-versie/sluiten poort](media/using-the-shell-window/preview-options.png)

## <a name="minimize--maximize-cloud-shell-window"></a>Minimaliseren & Cloud Shell-venster maximaliseren
Klik op het pictogram minimaliseren in de rechterbovenhoek van het venster om deze te verbergen. Klik op het pictogram van de Cloud Shell opnieuw te zichtbaar maken.
Klik op het pictogram maximaliseren om het venster maximale hoogte instellen. Als u wilt herstellen venster vorige formaat, klik op herstellen.  
![Minimaliseren of het venster maximaliseren](media/using-the-shell-window/minmax.png)

## <a name="copy-and-paste"></a>Kopiëren en plakken
[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

## <a name="resize-cloud-shell-window"></a>Het formaat van Cloud Shell-venster
Klik en sleep de bovenrand van de werkbalk omhoog of omlaag naar de Cloud Shell-venster formaat.

## <a name="scrolling-text-display"></a>Schuiven tekst weer te geven
Schuiven met de muis of het touchpad om terminal tekst te verplaatsen.

## <a name="exit-command"></a>Afsluitopdracht
Met `exit` de actieve sessie wordt beëindigd. Dit gedrag is echter standaard na 20 minuten zonder tussenkomst van de.

## <a name="next-steps"></a>Volgende stappen

[Bash in Cloud Shell-snelstartgids](quickstart.md) <br>
[PowerShell in Cloud Shell-snelstartgids](quickstart-powershell.md)