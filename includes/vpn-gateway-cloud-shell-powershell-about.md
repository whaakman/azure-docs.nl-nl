---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/25/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b878d54f0f52768459dbfc810e47d294b9c8d996
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50097763"
---
In dit artikel maakt gebruik van PowerShell-cmdlets. Als u wilt de cmdlets uitvoert, kunt u Azure Cloud Shell, een gratis interactieve shell. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Klik op **Kopiëren** om de code te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren. U kunt Cloud Shell op verschillende manieren starten:

|  |   |
|-----------------------------------------------|---|
| Klik op **Nu uitproberen** in de rechterbovenhoek van een codeblok. | ![Cloud Shell in dit artikel](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-powershell-try-it.png) |
| Open Cloud Shell in uw browser. | [![https://shell.azure.com/powershell](./media/vpn-gateway-cloud-shell-powershell/launchcloudshell.png)](https://shell.azure.com/powershell) |
| Klik op de knop **Cloud Shell** in het menu rechtsboven in Azure Portal. | [![Cloud Shell in de portal](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-menu.png)](https://portal.azure.com) |
|  |  |

Als u niet dat Azure Cloud Shell gebruiken wilt, kunt u PowerShell lokaal in plaats daarvan. Als u wilt installeren en gebruiken van PowerShell lokaal, zorg er dan voor dat de nieuwste versie van de Azure Resource Manager PowerShell-cmdlets installeren. PowerShell-cmdlets worden regelmatig bijgewerkt en moet u meestal om bij te werken van uw PowerShell-cmdlets voor het ophalen van de meest recente functionaliteit. Als u de PowerShell-cmdlets niet bijwerkt, kunnen de opgegeven waarden mislukken. Als u wilt zien welke versie van PowerShell die u lokaal uitvoert, gebruikt u de cmdlet 'Get-Module - ListAvailable AzureRM'. Zie [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps) als u een upgrade wilt uitvoeren. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor meer informatie.