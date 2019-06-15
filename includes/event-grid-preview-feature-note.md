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
ms.openlocfilehash: d32beb2d799a60cb9c5be061c39e4ec834da8dcf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814736"
---
Deze functie is beschikbaar als preview-versie. Als u wilt gebruiken, moet u een extensie van de Preview-versie of de module te installeren.

### <a name="install-extension-for-azure-cli"></a>Extensie voor Azure CLI installeren

Voor Azure CLI, moet u de [Event Grid-extensie](/cli/azure/azure-cli-extensions-list).

In [CloudShell](/azure/cloud-shell/quickstart):

* Als u de uitbreiding eerder hebt geïnstalleerd, bijgewerkt `az extension update -n eventgrid`
* Als u de uitbreiding eerder nog niet hebt geïnstalleerd, installeren `az extension add -n eventgrid`

Voor een lokale installatie:

1. [De Azure CLI installeren](/cli/azure/install-azure-cli). Zorg ervoor dat u de meest recente versie hebt door te controleren met `az --version`.
1. Vorige versies van de extensie verwijderen `az extension remove -n eventgrid`
1. Installeer de `eventgrid` uitbreiding met `az extension add -n eventgrid`

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
