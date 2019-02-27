---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/13/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c8f45e4bb16c05c9f322dd04d2c80f6144744e64
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884029"
---
In dit artikel maakt gebruik van PowerShell-cmdlets. Als u wilt de cmdlets uitvoert, kunt u Azure Cloud Shell. Azure Cloud Shell is een gratis interactieve shell die algemene Azure-hulpprogramma's die vooraf is geïnstalleerd en geconfigureerd voor gebruik met uw account heeft. Klik op **kopie** voor het kopiëren van de code, plak deze in de Cloud Shell en druk op enter uit te voeren. U kunt Cloud Shell op verschillende manieren starten:

|  |   |
|-----------------------------------------------|---|
| Klik op **Nu uitproberen** in de rechterbovenhoek van een codeblok. | ![Cloud Shell in dit artikel](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-powershell-try-it.png) |
| Open Cloud Shell in uw browser. | [![https://shell.azure.com/powershell](./media/vpn-gateway-cloud-shell-powershell/launchcloudshell.png)](https://shell.azure.com/powershell) |
| Klik op de knop **Cloud Shell** in het menu rechtsboven in Azure Portal. | [![Cloud Shell in de portal](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-menu.png)](https://portal.azure.com) |
|  |  |

**PowerShell lokaal uitvoert**

U kunt ook installeren en lokaal uitvoeren van de Azure PowerShell-cmdlets op uw computer. PowerShell-cmdlets worden regelmatig bijgewerkt. Als u de meest recente versie niet worden uitgevoerd, mislukken de waarden die zijn opgegeven in de volgende instructies. Als u wilt zien welke versie van PowerShell die u lokaal uitvoert, gebruikt u de `Get-Module -ListAvailable Az` cmdlet. Als u wilt installeren of bijwerken, Zie [installeren van de Azure PowerShell-module](/powershell/azure/install-az-ps).