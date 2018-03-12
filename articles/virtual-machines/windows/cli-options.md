---
title: De Azure CLI gebruiken op Windows | Microsoft Docs
description: De Azure CLI gebruiken in Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: nepeters
ms.openlocfilehash: e54ea62b7b7a7adb4874866a540ed9a6a097fc42
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="using-the-azure-cli-on-windows"></a>De Azure CLI gebruiken in Windows

De Azure-opdrachtregelinterface (CLI) biedt een opdrachtregel en scriptomgeving op servers voor het maken en beheren van Azure-resources. De Azure CLI is beschikbaar voor Mac OS-, Linux- en Windows-besturingssystemen. In deze besturingssystemen zijn de CLI-opdrachten identiek, maar de specifieke scriptsyntaxis besturingssysteem kan verschillen.

In dit document worden de manieren dat de Azure CLI kan worden geïnstalleerd en uitgevoerd op Windows en details syntactische overwegingen voor elke. Zie in de gedetailleerde Azure CLI-documentatie voor [documentatie van Azure CLI]( https://docs.microsoft.com/cli/azure).

## <a name="windows-subsystem-for-linux"></a>Windows-subsysteem voor Linux

Het Windows-subsysteem voor Linux (WSL) biedt een omgeving Ubuntu Linux op Windows 10 Verjaardag en latere versies. Wanneer ingeschakeld, WSL levert een systeemeigen Bash-ervaring die kan worden gebruikt voor het maken en uitvoeren van scripts voor Azure CLI. Omdat WSL een systeemeigen Bash-ervaring biedt, kunnen Azure CLI-scripts worden gedeeld tussen Mac OS-, Linux- en Windows zonder aanpassing.

Als u de Azure CLI in WSL, voert u de volgende.

|Taak | Instructies |
|---|---|
| WSL inschakelen | [WSL documentatie installeren ](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide) |
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
