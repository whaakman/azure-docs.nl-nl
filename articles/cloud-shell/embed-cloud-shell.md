---
title: Azure-Cloud-Shell insluiten | Microsoft Docs
description: Informatie voor het insluiten van Azure Cloud-Shell.
services: cloud-shell
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
ms.date: 12/11/2017
ms.author: juluk
ms.openlocfilehash: 78b539136971aa282e5447d7882ecb02f73f346b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="embed-azure-cloud-shell"></a>Azure-Cloud-Shell insluiten

Cloud-Shell insluiten kan ontwikkelaars en schrijvers van de inhoud rechtstreeks Cloud-Shell openen vanuit een specifieke URL, [shell.azure.com](https://shell.azure.com). Hiermee onmiddellijk de kracht van Cloud-Shell-verificatie, tooling, en up-to-date Azure CLI/Azure PowerShell hulpprogramma's voor uw gebruikers.

[![](https://shell.azure.com/images/launchcloudshell.png "Azure-Cloud-Shell starten")](https://shell.azure.com)

## <a name="how-to"></a>Uitleg

Knop voor Cloud-Shell starten integreren in markdown-bestanden te kopiëren van de volgende:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

De HTML-code voor het insluiten van een pop-Cloud-Shell lager is dan:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Aanpassen

Stel een specifieke shell-ervaring door het uitbreiden van de URL van uw.
|Ervaring   |URL   |
|---|---|
|Onlangs gebruikte shell   |Shell.Azure.com           |
|Bash                       |Shell.Azure.com/Bash       |
|PowerShell                 |Shell.Azure.com/PowerShell |

## <a name="next-steps"></a>Volgende stappen
[In de Cloud Shell snel Bash](quickstart.md)<br>
[PowerShell in de Cloud Shell Quick Start](quickstart-powershell.md)
