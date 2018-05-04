---
title: De Azure CLI gebruiken op Windows | Microsoft Docs
description: De Azure CLI gebruiken in Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: cynthn
ms.openlocfilehash: e1d611617b72a1fd5134b83bd3a2de59ba668b11
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="using-the-azure-cli-on-windows"></a>De Azure CLI gebruiken in Windows

De Azure-opdrachtregelinterface (CLI) biedt een opdrachtregel en scriptomgeving op servers voor het maken en beheren van Azure-resources. De Azure CLI is beschikbaar voor Mac OS-, Linux- en Windows-besturingssystemen. In deze besturingssystemen zijn de CLI-opdrachten identiek, maar de specifieke scriptsyntaxis besturingssysteem kan verschillen.

In dit document worden de manieren dat de Azure CLI kan worden geïnstalleerd en uitgevoerd op Windows en details syntactische overwegingen voor elke. Zie in de gedetailleerde Azure CLI-documentatie voor [documentatie van Azure CLI]( https://docs.microsoft.com/cli/azure).

## <a name="windows-subsystem-for-linux"></a>Windows-subsysteem voor Linux

Het Windows-subsysteem voor Linux (WSL) biedt een omgeving Ubuntu Linux op Windows 10 Verjaardag en latere versies. Wanneer ingeschakeld, WSL levert een systeemeigen Bash-ervaring die kan worden gebruikt voor het maken en uitvoeren van scripts voor Azure CLI. Omdat WSL een systeemeigen Bash-ervaring biedt, kunnen Azure CLI-scripts worden gedeeld tussen Mac OS-, Linux- en Windows zonder aanpassing.

Als u de Azure CLI in WSL, voert u de volgende.

|Taak | Instructies |
|---|---|
| WSL inschakelen | [WSL documentatie installeren ](https://msdn.microsoft.com/commandline/wsl/install_guide) |
| Azure-CLI installeren |[De CLI installeren op WSL/Ubuntu 14.04](https://docs.microsoft.com/cli/azure/install-az-cli2#ubuntu)|

## <a name="powershell"></a>PowerShell

De Azure CLI kunt systeemeigen in Windows worden uitgevoerd. In deze configuratie is de Azure CLI-pakket is geïnstalleerd op de Windows-besturingssysteem en opdrachten kunnen worden uitgevoerd vanuit PowerShell. In deze configuratie, kunnen Azure CLI-opdrachten en scripts worden uitgevoerd op een ondersteunde versie van Windows, maar de specifieke scriptsyntaxis platform vereist is. Als gevolg hiervan kunnen geen scripts altijd worden gedeeld tussen Mac OS-, Linux- en Windows zonder aanpassingen.

Installeer het pakket deze instructies voor het gebruik van de Azure CLI in Windows, [CLI installeren op Windows](https://docs.microsoft.com/cli/azure/install-az-cli2#windows).

## <a name="docker-image"></a>Docker-afbeelding

Wanneer u Docker voor Windows, kan een Docker-installatiekopie worden gestart met de Azure CLI. Deze installatiekopie is gebaseerd op Linux en bevat een systeemeigen Bash-ervaring.  Als u Docker voor Windows en de installatiekopie van het Azure CLI-scripts kunnen worden gedeeld tussen Mac OS-, Linux- en Windows. 

Zorg ervoor dat Docker voor Windows wordt uitgevoerd voor het gebruik van de Azure CLI op Docker voor Windows, en voer de volgende opdracht.

```bash
docker run -it azuresdk/azure-cli-python:latest bash
```

Als voltooid, wordt een sessie wordt gestart, dat wil Bash vooraf geladen met de Azure CLI-hulpprogramma's.

## <a name="next-steps"></a>Volgende stappen

[Voorbeeld voor virtuele machines van Azure CLI](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Voorbeelden voor Web-Apps van Azure CLI](../../app-service/app-service-cli-samples.md)

[Voorbeelden voor SQL Azure CLI](../../sql-database/sql-database-cli-samples.md)
