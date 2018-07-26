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
ms.date: 07/24/2018
ms.author: juluk
ms.openlocfilehash: 9588bebdc827760f0e0d3e2aadccbff5f24723f1
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258922"
---
# <a name="overview-of-azure-cloud-shell"></a>Overzicht van Azure Cloudshell
Azure Cloud Shell is een interactieve, browser toegankelijke shell voor het beheren van Azure-resources.
Het biedt de flexibiliteit van het kiezen van de shell-ervaring die het beste bij de manier waarop die u werkt.
Linux-gebruikers kunnen voor een Bash-ervaring kiezen en voor Windows-gebruikers is PowerShell beschikbaar.

Probeer uit shell.azure.com door te klikken op volgende.

[![](https://shell.azure.com/images/launchcloudshell.png "Open Azure Cloudshell")](https://shell.azure.com)

Probeer uit Azure portal met behulp van de Cloud Shell-pictogram.

![Portal starten](media/overview/portal-launch-icon.png)

## <a name="features"></a>Functies
### <a name="browser-based-shell-experience"></a>Browser gebaseerde shell-ervaring
Cloudshell kan toegang tot een browser gebaseerde opdrachtregel ervaring die zijn gebouwd met Azure-beheertaken waarmee u rekening moet worden ingeschakeld.
Maak gebruik van Cloud Shell om te werken voelt van een lokale machine op een manier alleen de cloud kunt opgeven.

### <a name="choice-of-preferred-shell-experience"></a>Keuze van de gewenste shell-ervaring
Linux-gebruikers kunt Bash in Cloud Shell, terwijl gebruikers van Windows PowerShell in Cloud Shell (Preview) kunnen gebruiken in de vervolgkeuzelijst shell.

![Bash in Cloudshell](media/overview/overview-bash-pic.png)

![PowerShell in Cloudshell (Preview)](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Geverifieerde en geconfigureerde Azure werkstation
Cloudshell wordt beheerd door Microsoft, zodat deze wordt geleverd met populaire opdrachtregelprogramma's en ondersteuning voor talen. Cloudshell wordt ook veilig automatisch geverifieerd voor directe toegang tot uw resources via de Azure CLI 2.0 of Azure PowerShell-cmdlets.

Bekijk alle [lijst met hulpprogramma's zijn geïnstalleerd in de Cloud Shell.](features.md#tools)

### <a name="integrated-cloud-shell-editor"></a>Geïntegreerde Cloud Shell-editor
Cloudshell biedt een geïntegreerde grafische teksteditor op basis van de open-source Monaco-Editor. Eenvoudig maken en bewerken van configuratiebestanden door uit te voeren `code .` voor de naadloze implementatie via Azure CLI 2.0 of Azure PowerShell.

[Meer informatie over de Cloud Shell-editor](using-cloud-shell-editor.md).

### <a name="multiple-access-points"></a>Meerdere toegangspunten
Cloudshell is een flexibele hulpprogramma dat kan worden gebruikt vanuit:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI 2.0 'Nu uitproberen'-documentatie](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)
* [Mobiele Apps van Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [VS Code Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Verbinding maken met uw Microsoft Azure File storage
Cloud Shell-machines zijn tijdelijk en vereist een Azure-bestandsshare te koppelen als `clouddrive` om vast te leggen van de bestanden.

Op de eerste keer opstarten die cloud Shell wordt gevraagd om te maken van een resource delen groeperen, storage-account en Azure Files namens. Dit is een eenmalige stap en automatisch worden toegevoegd voor alle sessies. Één bestandsshare kan worden toegewezen en wordt gebruikt door zowel Bash als PowerShell in Cloud Shell (Preview).

#### <a name="create-new-storage"></a>Maken van nieuwe opslag
![](media/overview/basic-storage.png)

Een account voor lokaal redundante opslag (LRS) en een Azure-bestandsshare kunnen namens u worden gemaakt. De Azure-bestandsshare wordt gebruikt voor omgevingen met zowel Bash als PowerShell als u wilt gebruiken. Reguliere opslagkosten van toepassing.

Drie bronnen worden namens u gemaakt:
1. De resourcegroep met de naam: `cloud-shell-storage-<region>`
2. Storage-Account met de naam: `cs<uniqueGuid>`
3. De bestandsshare met de naam: `cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bash in Cloud Shell maakt ook een standaardinstallatiekopie van 5 GB-schijf om te blijven behouden `$Home`. Alle bestanden in uw directory $Home zoals SSH-sleutels worden opgeslagen in de schijfinstallatiekopie van de gebruiker die zijn opgeslagen in uw gekoppelde Azure-bestandsshare. Aanbevolen procedures toepassen bij het opslaan van bestanden in uw directory $Home en de gekoppelde Azure-bestandsshare.

#### <a name="use-existing-resources"></a>Gebruik bestaande bronnen
![](media/overview/advanced-storage.png)

Een geavanceerde optie is opgegeven om te koppelen van bestaande resources naar Cloud Shell.
Klik op 'Geavanceerde instellingen weergeven' bij de prompt opslag instellen om extra opties weer te geven.

> [!Note]
> Vervolgkeuzemenu's worden voor uw vooraf toegewezen Cloud Shell-regio en LRS/GRS/ZRS-opslagaccounts gefilterd.

[Meer informatie over Cloud Shell-opslag, het bijwerken van de Azure-bestandsshares en uploadt/downloadt bestanden.](persisting-shell-storage.md)

## <a name="concepts"></a>Concepten
* Cloudshell wordt uitgevoerd op een tijdelijke host die is opgegeven op een per-sessie, per gebruiker
* Cloudshell een time-out na 20 minuten zonder interactieve activiteit
* Cloudshell is vereist voor een Azure-bestandsshare te koppelen
* Cloudshell gebruikmaakt van dezelfde Azure-bestandsshare voor zowel Bash als PowerShell
* Cloudshell is één machine per gebruikersaccount toegewezen
* Cloudshell zich blijft voordoen $Home met behulp van de installatiekopie van een 5 GB die zijn ondergebracht in de bestandsshare
* Machtigingen zijn ingesteld als een gewone gebruiker Linux in Bash

Meer informatie over functies in [Bash in Cloud Shell](features.md) en [PowerShell in Cloud Shell (Preview)](features-powershell.md).

## <a name="pricing"></a>Prijzen
De computer die als host fungeert voor Cloud Shell is gratis, met een vereiste is van een gekoppelde Azure Files-share. Reguliere opslagkosten van toepassing.

## <a name="next-steps"></a>Volgende stappen
[Bash in Cloud Shell-snelstartgids](quickstart.md) <br>
[PowerShell in Cloud Shell (Preview)-snelstartgids](quickstart-powershell.md)