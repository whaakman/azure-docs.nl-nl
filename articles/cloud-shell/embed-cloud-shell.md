---
title: Azure Cloudshell insluiten | Microsoft Docs
description: Leer hoe u Azure Cloud Shell insluiten.
services: cloud-shell
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
ms.date: 12/11/2017
ms.author: damaerte
ms.openlocfilehash: 4cd6eeb37eb441528e411377cf175cd950d50b04
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57245791"
---
# <a name="embed-azure-cloud-shell"></a>Azure Cloudshell insluiten

Cloud Shell insluiten kan ontwikkelaars en schrijvers van de inhoud rechtstreeks Cloud Shell openen vanuit een speciale URL [shell.azure.com](https://shell.azure.com). Hiermee direct de volledige kracht van Cloud Shell-verificatie, hulpprogramma's, en up-to-date Azure CLI/Azure PowerShell-hulpprogramma's voor uw gebruikers.

Knop voor normale grootte

[![Reguliere starten](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell openen")](https://shell.azure.com)

Grote grootte knop

[![Grote starten](https://shell.azure.com/images/launchcloudshell@2x.png "Azure Cloud Shell openen")](https://shell.azure.com)

## <a name="how-to"></a>Uitleg

De knop starten van Cloud Shell integreren met markdown-bestanden door het kopiëren van het volgende:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

De HTML-code in te sluiten van een pop-upvenster die cloud Shell lager dan is:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><img alt="Launch Azure Cloud Shell" src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Ervaring aanpassen

Een specifieke shell-ervaring door uit te breiden met de URL van uw ingesteld.
|Ervaring   |URL   |
|---|---|
|Meest recent gebruikte shell   |[shell.azure.com](https://shell.azure.com)           |
|Bash                       |[shell.azure.com/bash](https://shell.azure.com/bash)       |
|PowerShell                 |[shell.azure.com/powershell](https://shell.azure.com/powershell) |

## <a name="next-steps"></a>Volgende stappen
[Bash in Cloud Shell-snelstartgids](quickstart.md)<br>
[PowerShell in Cloud Shell-snelstartgids](quickstart-powershell.md)
