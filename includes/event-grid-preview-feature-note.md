---
title: bestand opnemen
description: bestand opnemen
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 11/06/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 85a1579e32b4c216f234f77c76316bedeaea77b0
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285700"
---
Deze functie is beschikbaar als preview-versie. Als u wilt gebruiken, moet u een extensie van de Preview-versie of de module te installeren.

### <a name="install-extension-for-azure-cli"></a>Extensie voor Azure CLI installeren

Voor Azure CLI, moet u de [Event Grid-extensie](/cli/azure/azure-cli-extensions-list).

In [CloudShell](/azure/cloud-shell/quickstart):

* Als u de uitbreiding eerder hebt geïnstalleerd, bijgewerkt `az extension update -n eventgrid`
* Als u de uitbreiding eerder nog niet hebt geïnstalleerd, installeren `az extension add -n eventgrid`

Voor een lokale installatie:

1. Azure CLI lokaal te verwijderen.
1. Installeer de [meest recente versie](/cli/azure/install-azure-cli) van Azure CLI.
1. Start-opdrachtvenster.
1. Vorige versies van de extensie verwijderen `az extension remove -n eventgrid`
1. De extensie installeren `az extension add -n eventgrid`

### <a name="install-module-for-powershell"></a>Module voor PowerShell installeren

Voor PowerShell, moet u de [AzureRM.EventGrid module](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview).

In [CloudShell](/azure/cloud-shell/quickstart-powershell):

* De module installeren `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Voor een lokale installatie:

1. Open de PowerShell-console als beheerder
1. De module installeren `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Als de `-AllowPrerelease` parameter is niet beschikbaar, gebruikt u de volgende stappen uit:

1. Voer `Install-Module PowerShellGet -Force` uit.
1. Voer `Update-Module PowerShellGet` uit.
1. Sluit de console van PowerShell
1. Opnieuw opstarten van PowerShell als beheerder
1. De module installeren `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`
