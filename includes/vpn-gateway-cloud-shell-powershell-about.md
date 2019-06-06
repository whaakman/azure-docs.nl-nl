---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f6af5e5e42a16548b1997845ea5076ddd4dd3be5
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735926"
---
In dit artikel maakt gebruik van PowerShell-cmdlets. Als u wilt de cmdlets uitvoert, kunt u Azure Cloud Shell, een interactieve shell-omgeving die wordt gehost in Azure en via de browser gebruikt. Azure Cloud Shell wordt geleverd met de Azure PowerShell-cmdlets vooraf zijn geïnstalleerd.

Als u wilt uitvoeren van code die is opgenomen in dit artikel over Azure Cloud Shell, opent u een Cloud Shell-sessie, gebruikt u de **kopie** knop in een codeblok kopieert u de code en plak deze in de Cloud Shell-sessie met __Ctrl + Shift + V__ op Windows en Linux, of __Cmd + Shift + V__ in macOS. Geplakte tekst niet automatisch wordt uitgevoerd, dus druk op **Enter** code uit te voeren.

U kunt Azure Cloud Shell met starten:

|  |   |
|-----------------------------------------------|---|
| Selecteer **Nu proberen** in de rechterbovenhoek van een codeblok. Dit __niet__ tekst automatisch kopiëren naar Cloud Shell. | ![Voorbeeld van proberen voor Azure Cloudshell](./media/cloud-shell-try-it/cli-try-it.png) |
| Open [shell.azure.com](https://shell.azure.com) in uw browser. | [![Knop voor Azure Cloud Shell starten](./media/cloud-shell-try-it/launchcloudshell.png)](https://shell.azure.com) |
| Klik op de knop **Cloud Shell** in het menu in de hoek rechtsboven in de [Azure Portal](https://portal.azure.com). | ![Knop Cloud Shell in de Azure Portal](./media/cloud-shell-try-it/cloud-shell-menu.png) |

**PowerShell lokaal uitvoert**

U kunt ook installeren en lokaal uitvoeren van de Azure PowerShell-cmdlets op uw computer. PowerShell-cmdlets worden regelmatig bijgewerkt. Als u de meest recente versie niet worden uitgevoerd, mislukken de waarden die zijn opgegeven in de volgende instructies. Als u wilt zien welke versie van PowerShell die u lokaal uitvoert, gebruikt u de `Get-Module -ListAvailable Az` cmdlet. Als u wilt installeren of bijwerken, Zie [installeren van de Azure PowerShell-module](/powershell/azure/install-az-ps).
