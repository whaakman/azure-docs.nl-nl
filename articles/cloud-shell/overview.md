---
title: Overzicht van Azure Cloud Shell | Microsoft Docs
description: Overzicht van de Azure-Cloud-Shell.
services: 
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
ms.date: 01/17/2018
ms.author: juluk
ms.openlocfilehash: b710c324f72fa56a2ebad0d1b35052639611d30d
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="overview-of-azure-cloud-shell"></a>Overzicht van Azure-Cloud-Shell
Azure Cloud-Shell is een interactieve, browser toegankelijke shell voor het beheren van Azure-resources.
Dit biedt de flexibiliteit van het kiezen van de shell-ervaring die het beste past bij de manier waarop die u werkt.
Linux-gebruikers kunnen voor een Bash-ervaring kiezen en voor Windows-gebruikers is PowerShell beschikbaar.

Probeer van shell.azure.com met deze knop.

[![](https://shell.azure.com/images/launchcloudshell.png "Azure-Cloud-Shell starten")](https://shell.azure.com)

Probeer vanuit Azure-portal met behulp van de Cloud Shell-pictogram.

![Portal starten](media/overview/portal-launch-icon.png)

## <a name="features"></a>Functies
### <a name="browser-based-shell-experience"></a>Browser gebaseerde shell-ervaring
Cloud-Shell kan toegang tot een browser gebaseerde opdrachtregelprogramma ervaring gebouwd met Azure beheertaken in gedachten.
Hefboomwerking Cloud Shell voelt van een lokale computer werkt op een manier alleen de cloud kunnen bieden.

### <a name="choice-of-preferred-shell-experience"></a>Keuze van voorkeur shell-ervaring
Linux-gebruikers kunnen gebruiken Bash Cloud-shell, terwijl gebruikers van Windows PowerShell in de Cloud-Shell (Preview gebruiken kunnen) uit de vervolgkeuzelijst shell.

![In de Cloud-Shell Bash](media/overview/overview-bash-pic.png)

![PowerShell in de Cloud-Shell (Preview)](media/overview/overview-ps-pic.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Geverifieerde en geconfigureerde Azure werkstation
Cloud-Shell wordt beheerd door Microsoft, zodat deze wordt geleverd met populaire opdrachtregelprogramma's en ondersteuning voor talen. Cloud-Shell verifieert veilig ook automatisch voor directe toegang tot uw resources via de Azure CLI 2.0 of Azure PowerShell-cmdlets.

Overzicht van de volledige tooling voor de [Bash ervaring](features.md#tools) en [PowerShell (Preview)-ervaring.](features-powershell.md#tools)

### <a name="multiple-access-points"></a>Meerdere toegangspunten
Cloud-Shell is een flexibele hulpprogramma dat kan worden gebruikt vanuit:
* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI 2.0 'Try It'-documentatie](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)
* [Mobiele Apps van Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Uitbreiding van VS Code Azure-Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Verbinding maken met uw Microsoft Azure File storage
Cloud-Shell-machines zijn tijdelijk en vereisen een Azure-bestanden-share moet worden gekoppeld als `clouddrive` voor het persistent maken van uw bestanden.

Op de eerste keer opstarten die cloud Shell wordt gevraagd om een resource te maken delen groep en storage-account Azure-bestanden namens jou. Dit is een eenmalige stap en wordt automatisch voor alle sessies worden gekoppeld. Één bestandsshare kan worden toegewezen en wordt gebruikt door zowel Bash als PowerShell in de Cloud-Shell (Preview).

#### <a name="create-new-storage"></a>Maken van nieuwe opslag
![](media/overview/basic-storage.png)

Een lokaal redundant opslagaccount (LRS) en de Azure-bestanden delen kunnen namens jou worden gemaakt. De share Azure-bestanden wordt gebruikt voor zowel Bash en PowerShell omgevingen als u wilt gebruiken. Reguliere opslagkosten van toepassing.

Drie bronnen worden namens jou gemaakt:
1. De resourcegroep met de naam:`cloud-shell-storage-<region>`
2. Opslagaccount met de naam:`cs<uniqueGuid>`
3. De bestandsshare met de naam:`cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bash in de Cloud-Shell maakt ook een standaardinstallatiekopie van 5 GB schijfruimte om te blijven behouden `$Home`. Alle bestanden in uw directory $Home zoals SSH-sleutels worden doorgevoerd in de schijfinstallatiekopie van uw gebruiker opgeslagen in de gekoppelde Azure-bestandsshare. Aanbevolen procedures van toepassing bij het opslaan van bestanden in uw directory $Home en de gekoppelde Azure-bestandsshare.

#### <a name="use-existing-resources"></a>Bestaande bronnen gebruiken
![](media/overview/advanced-storage.png)

Een geavanceerde optie is opgegeven voor het koppelen van bestaande resources voor Cloud-Shell.
Klik op 'Weergeven geavanceerde instellingen' bij de prompt opslag setup om extra opties weer te geven.

> [!Note]
> Opgegeven waarin DropDowns worden gefilterd voor uw vooraf toegewezen Cloud Shell regio en LRS/GRS-accounts voor opslag.

[Meer informatie over Cloud Shell opslag, updaten van Azure-bestandsshares en het uploaden/downloaden van bestanden.](persisting-shell-storage.md)

## <a name="concepts"></a>Concepten
* Cloud-Shell wordt uitgevoerd op een tijdelijke host die is opgegeven op een per-sessie per gebruiker
* Time-out opgetreden voor de cloud Shell na 20 minuten zonder interactieve activiteit
* Cloud-Shell vereist een Azure-bestandsshare te koppelen
* Cloud-Shell de dezelfde Azure-bestandsshare gebruikt voor zowel Bash en PowerShell
* Cloud-Shell wordt één machine per gebruikersaccount toegewezen
* Bash zich blijft voordoen $Home met de installatiekopie van een 5 GB ondergebracht in de bestandsshare
* Machtigingen zijn ingesteld als een gewone gebruiker Linux in Bash

Meer informatie over functies in [Bash in de Cloud-Shell](features.md) en [PowerShell in de Cloud-Shell (Preview)](features-powershell.md).

## <a name="pricing"></a>Prijzen
De computer die als host fungeert voor Cloud-Shell is gratis, met een vereiste van een gekoppelde Azure Files-share. Reguliere opslagkosten van toepassing.

## <a name="next-steps"></a>Volgende stappen
[In de Cloud Shell snel Bash](quickstart.md) <br>
[PowerShell in Quick Start voor Cloud-Shell (Preview)](quickstart-powershell.md)