---
title: Azure Cloudshell insluiten | Microsoft Docs
description: Leer hoe u Azure Cloud Shell insluiten.
services: cloud-shell
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
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 37ad0adf01172fadc6d9845576045a4542bdae19
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497787"
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
|Bash                       |[Shell.Azure.com/Bash](https://shell.azure.com/bash)       |
|PowerShell                 |[Shell.Azure.com/PowerShell](https://shell.azure.com/powershell) |

## <a name="next-steps"></a>Volgende stappen
[Bash in Cloud Shell-snelstartgids](quickstart.md)<br>
[PowerShell in Cloud Shell-snelstartgids](quickstart-powershell.md)
