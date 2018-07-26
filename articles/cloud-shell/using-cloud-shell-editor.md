---
title: Met behulp van de Azure Cloud Shell-editor | Microsoft Docs
description: Overzicht van het gebruik van de Azure Cloud Shell-editor.
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: juluk
ms.openlocfilehash: caf6e18a9a30654710f5445ed6ab957a5253d62e
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259837"
---
# <a name="using-the-azure-cloud-shell-editor"></a>Met behulp van de Azure Cloud Shell-editor

Azure Cloud Shell omvat een geïntegreerde bestandseditor gebouwd op basis van de open-source [Monaco Editor](https://github.com/Microsoft/monaco-editor). De Cloud Shell-editor biedt ondersteuning voor functies, zoals taalmarkeringen, het opdrachtenpalet en een bestand in bestandenverkenner.

![Cloud Shell-editor](media/using-cloud-shell-editor/open-editor.png)

## <a name="opening-the-editor"></a>De editor te openen

Voor het eenvoudig bestand maken en bewerken, start u de editor door te voeren `code .` in de Cloud Shell-terminal. Deze actie opent de editor met uw actieve werkmap instellen in de terminal.

Voor het rechtstreeks openen van een bestand voor het snel bewerken, uitvoeren `code <filename>` om de editor zonder de Verkenner te openen.

U opent de editor via de knop van de gebruikersinterface, klikt u op de `{}` editor-pictogram in de werkbalk. Hiermee opent u de editor en standaard van de Verkenner op de `/home/<user>` directory.

## <a name="closing-the-editor"></a>Sluit de editor

Sluit de editor, opent u de `...` actie deelvenster in de rechterbovenhoek van de editor en selecteer `Close editor`.

![Editor sluiten](media/using-cloud-shell-editor/close-editor.png)

## <a name="command-palette"></a>Opdrachtenpalet

Als u wilt het opdrachtenpalet starten, gebruikt u de `F1` wanneer de focus is ingesteld op de editor. Openen van het opdrachtenpalet kan ook worden gedaan via het deelvenster actie aan.

![Cmd palet](media/using-cloud-shell-editor/cmd-palette.png)

## <a name="contributing-to-the-monaco-editor"></a>Een bijdrage levert aan de Monaco-Editor

Ondersteuning voor talen markeren in de Cloud Shell-editor wordt ondersteund door de upstream-functionaliteit in de [Monaco Editor](https://github.com/Microsoft/monaco-editor)Monarch syntaxis van de definities van het gebruik. Lees meer informatie over om bijdragen te leveren, de [gids voor inzenders Monaco](https://github.com/Microsoft/monaco-editor/blob/master/CONTRIBUTING.md).

## <a name="next-steps"></a>Volgende stappen
[Probeer de Snelstartgids voor Bash in Cloud Shell](quickstart.md)
[weergeven van de volledige lijst met geïntegreerde hulpprogramma's voor Cloud Shell](features.md)