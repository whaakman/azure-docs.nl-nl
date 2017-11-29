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
ms.date: 11/16/2017
ms.author: juluk
ms.openlocfilehash: 05c4d419f6d7b42a14b9bb13570daaa666d52db3
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="overview-of-azure-cloud-shell"></a>Overzicht van Azure-Cloud-Shell
Azure Cloud-Shell is een interactieve, browser toegankelijke shell voor het beheren van Azure-resources.
Dit biedt u de flexibiliteit van het kiezen van de shell-ervaring die het beste past bij de manier waarop die u werkt.
Linux-gebruikers kunnen voor een Bash-ervaring kiezen en voor Windows-gebruikers is PowerShell beschikbaar.

Via de Azure-portal van het pictogram Cloud Shell starten:

![Portal starten](media/overview/portal-launch-icon.png)

Gebruikmaken van Bash of PowerShell uit de vervolgkeuzelijst van de selector shell:

![In de Cloud-Shell Bash](media/overview/overview-bash-pic.png)

![PowerShell in de Cloud-Shell (Preview)](media/overview/overview-ps-pic.png)

## <a name="features"></a>Functies
### <a name="browser-based-shell-experience"></a>Browser gebaseerde shell-ervaring
Cloud-Shell kan toegang tot een browser gebaseerde opdrachtregelprogramma ervaring gebouwd met Azure beheertaken in gedachten.
Hefboomwerking Cloud Shell voelt van een lokale computer werkt op een manier alleen de cloud kunnen bieden.

### <a name="choice-of-preferred-shell-experience"></a>Keuze van voorkeur shell-ervaring
Azure Cloud Shell biedt u de flexibiliteit om de shell-ervaring te kiezen die het beste bij uw werkwijze past.
Linux-gebruikers kunnen kiezen voor Bash in de Cloud-Shell, terwijl de Windows-gebruikers kunnen kiezen voor PowerShell in de Cloud-Shell (Preview).

### <a name="authenticated-and-configured-azure-workstation"></a>Geverifieerde en geconfigureerde Azure werkstation
Cloud-Shell wordt geleverd door Microsoft beheerde zodat deze vooraf is geïnstalleerd met populaire opdrachtregelprogramma's en talen zodat u sneller kunt werken. Bovendien verifieert Cloud Shell veilig automatisch voor directe toegang tot uw resources via de Azure CLI 2.0 of Azure PowerShell-cmdlets.

Overzicht van de volledige tooling voor de [Bash ervaring](features.md#tools) en [PowerShell (Preview)-ervaring.](features-powershell.md#tools)

### <a name="multiple-access-points"></a>Meerdere toegangspunten
Naast het Cloud-Shell wordt beschikbaar vanuit de Azure-portal, kunt u deze ook geopend op:
* [Azure CLI 2.0 'Try It'-documentatie](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)
* [Mobiele Apps van Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Visual Studio Code-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Verbinding maken met uw Microsoft Azure File storage
Cloud-Shell-machines zijn tijdelijk en als gevolg hiervan vereisen een Azure-bestanden-share moet worden gekoppeld als `clouddrive` voor het persistent maken van uw directory $Home.
Op de eerste keer opstarten die cloud Shell wordt gevraagd om een resourcegroep te maken, delen storage-account en een Azure-bestand namens jou. Dit is een eenmalige stap en wordt automatisch voor alle sessies worden gekoppeld. Één Azure-bestandsshare kan worden toegewezen en wordt gebruikt door zowel Bash als PowerShell in de Cloud-Shell (Preview).

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
Opgegeven waarin DropDowns worden gefilterd voor uw toegewezen Cloud Shell regio en lokaal/globaal-redundant storage-accounts.

[Meer informatie over Cloud Shell opslag, updaten van Azure-bestandsshares en het uploaden/downloaden van bestanden.](persisting-shell-storage.md)

## <a name="concepts"></a>Concepten
* Cloud-Shell wordt uitgevoerd op een tijdelijke host die is opgegeven op een per-sessie per gebruiker
* Time-out opgetreden voor de cloud Shell na 20 minuten zonder interactieve activiteit
* Cloud-Shell vereist een Azure-bestandsshare te koppelen
* Cloud-Shell de dezelfde Azure-bestandsshare gebruikt voor zowel Bash en PowerShell
* Cloud-Shell wordt één machine per gebruikersaccount toegewezen
* Machtigingen zijn ingesteld als een gewone gebruiker Linux in Bash

Meer informatie over functies in [Bash in de Cloud-Shell](features.md) en [PowerShell in de Cloud-Shell (Preview)](features-powershell.md).

## <a name="examples"></a>Voorbeelden
* Scripts gebruiken om Azure beheertaken te automatiseren
* Azure-resources via Azure portal en Azure-opdrachtregelprogramma's tegelijk beheren
* Test-Drive Azure CLI 2.0 of Azure PowerShell-cmdlets

Deze voorbeelden in snelstartgidsen voor uitproberen [Bash in de Cloud-Shell](quickstart.md) en [PowerShell in de Cloud-Shell (Preview)](quickstart-powershell.md).

## <a name="pricing"></a>Prijzen
De computer die als host fungeert voor Cloud-Shell is gratis, met een vereiste van een gekoppelde Azure Files-share. Reguliere opslagkosten van toepassing.

## <a name="next-steps"></a>Volgende stappen
[In de Cloud Shell snel Bash](quickstart.md) <br>
[PowerShell in Quick Start voor Cloud-Shell (Preview)](quickstart-powershell.md)