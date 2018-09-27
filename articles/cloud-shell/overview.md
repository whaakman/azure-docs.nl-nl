---
title: Overzicht van Azure Cloud Shell | Microsoft Docs
description: Overzicht van de Azure Cloudshell.
services: ''
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
ms.date: 09/04/2018
ms.author: juluk
ms.openlocfilehash: ea7393584e77b51073ca8938ec03e9d6f6ad6fc0
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166670"
---
# <a name="overview-of-azure-cloud-shell"></a>Overzicht van Azure Cloudshell
Azure Cloud Shell is een interactieve, browser toegankelijke shell voor het beheren van Azure-resources.
Het biedt de flexibiliteit van het kiezen van de shell-ervaring die het beste bij de manier waarop die u werkt.
Linux-gebruikers kunnen voor een Bash-ervaring kiezen en voor Windows-gebruikers is PowerShell beschikbaar.

Probeer uit shell.azure.com door te klikken op volgende.

[![Insluiten starten](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell openen")](https://shell.azure.com)

Probeer uit Azure portal met behulp van de Cloud Shell-pictogram.

![Portal starten](media/overview/portal-launch-icon.png)

## <a name="features"></a>Functies

### <a name="browser-based-shell-experience"></a>Browser gebaseerde shell-ervaring
Cloudshell kan toegang tot een browser gebaseerde opdrachtregel ervaring die zijn gebouwd met Azure-beheertaken waarmee u rekening moet worden ingeschakeld.
Maak gebruik van Cloud Shell om te werken voelt van een lokale machine op een manier alleen de cloud kunt opgeven.

### <a name="choice-of-preferred-shell-experience"></a>Keuze van de gewenste shell-ervaring
Gebruikers kunnen kiezen tussen Bash of PowerShell in de vervolgkeuzelijst shell.

![Bash in Cloudshell](media/overview/overview-bash-pic.png)

![PowerShell in Cloud Shell](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Geverifieerde en geconfigureerde Azure werkstation
Cloudshell wordt beheerd door Microsoft, zodat deze wordt geleverd met populaire opdrachtregelprogramma's en ondersteuning voor talen. Cloudshell wordt ook veilig automatisch geverifieerd voor directe toegang tot uw resources via de Azure CLI of Azure PowerShell-cmdlets.

Bekijk alle [lijst met hulpprogramma's zijn geïnstalleerd in de Cloud Shell.](features.md#tools)

### <a name="integrated-cloud-shell-editor"></a>Geïntegreerde Cloud Shell-editor
Cloudshell biedt een geïntegreerde grafische teksteditor op basis van de open-source Monaco-Editor. Eenvoudig maken en bewerken van configuratiebestanden door uit te voeren `code .` voor de naadloze implementatie via Azure CLI 2.0 of Azure PowerShell.

[Meer informatie over de Cloud Shell-editor](using-cloud-shell-editor.md).

### <a name="multiple-access-points"></a>Meerdere toegangspunten
Cloudshell is een flexibele hulpprogramma dat kan worden gebruikt vanuit:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI 'Nu uitproberen'-documentatie](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)
* [Mobiele Apps van Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [VS Code Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Verbinding maken met uw Microsoft Azure File storage
Cloud Shell machines zijn tijdelijk en vereisen een nieuwe of bestaande Azure-bestandsshare te koppelen als `clouddrive` om vast te leggen van de bestanden.

Op de eerste keer opstarten die cloud Shell wordt gevraagd om te maken van een resource delen groeperen, storage-account en Azure Files namens. Dit is een eenmalige stap en automatisch worden toegevoegd voor alle sessies. Één bestandsshare kan worden toegewezen en wordt gebruikt door zowel Bash als PowerShell in Cloud Shell.

Meer informatie voor meer informatie over het koppelen van een [nieuwe of bestaande opslagaccount](persisting-shell-storage.md).

## <a name="concepts"></a>Concepten
* Cloudshell wordt uitgevoerd op een tijdelijke host die is opgegeven op een per-sessie, per gebruiker
* Cloudshell een time-out na 20 minuten zonder interactieve activiteit
* Cloudshell is vereist voor een Azure-bestandsshare te koppelen
* Cloudshell gebruikmaakt van dezelfde Azure-bestandsshare voor zowel Bash als PowerShell
* Cloudshell is één machine per gebruikersaccount toegewezen
* Cloudshell zich blijft voordoen $Home met behulp van de installatiekopie van een 5 GB die zijn ondergebracht in de bestandsshare
* Machtigingen zijn ingesteld als een gewone gebruiker Linux in Bash

Meer informatie over functies in [Bash in Cloud Shell](features.md) en [PowerShell in Cloud Shell](features-powershell.md).

## <a name="pricing"></a>Prijzen
De computer die als host fungeert voor Cloud Shell is gratis, met een vereiste is van een gekoppelde Azure Files-share. Reguliere opslagkosten van toepassing.

## <a name="next-steps"></a>Volgende stappen
[Bash in Cloud Shell-snelstartgids](quickstart.md) <br>
[PowerShell in Cloud Shell-snelstartgids](quickstart-powershell.md)
