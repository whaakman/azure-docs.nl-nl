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
ms.openlocfilehash: 3ceddb94336fc2703e6f916f05ab1ec3676cb50d
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="embed-azure-cloud-shell"></a>Azure-Cloud-Shell insluiten

Cloud-Shell insluiten kan ontwikkelaars en schrijvers van de inhoud rechtstreeks Cloud-Shell openen vanuit een specifieke URL, [shell.azure.com](https://shell.azure.com). Hiermee onmiddellijk de kracht van Cloud-Shell-verificatie, tooling, en up-to-date Azure CLI/Azure PowerShell hulpprogramma's voor uw gebruikers.

Knop voor normale grootte

[![](https://shell.azure.com/images/launchcloudshell.png "Azure-Cloud-Shell starten")](https://shell.azure.com)

Grote grootte knop

[![](https://shell.azure.com/images/launchcloudshell@2x.png "Azure-Cloud-Shell starten")](https://shell.azure.com)

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
|Onlangs gebruikte shell   |shell.azure.com           |
|Bash                       |shell.azure.com/bash       |
|PowerShell                 |shell.azure.com/powershell |

## <a name="next-steps"></a>Volgende stappen
[In de Cloud Shell snel Bash](quickstart.md)<br>
[PowerShell in de Cloud Shell Quick Start](quickstart-powershell.md)
