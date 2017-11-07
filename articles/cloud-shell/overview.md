---
title: Overzicht van Azure Cloud-Shell (Preview) | Microsoft Docs
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
ms.date: 11/02/2017
ms.author: juluk
ms.openlocfilehash: 3acea56ea414f0c43333a02274e91226db29d454
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a>Overzicht van Azure-Cloud-Shell (Preview)
Azure Cloud-Shell is een interactieve, browser toegankelijke shell voor het beheren van Azure-resources.
Dit biedt u de flexibiliteit van het kiezen van de shell-ervaring die het beste past bij de manier waarop die u werkt.
Linux-gebruikers kunnen voor een Bash-ervaring kiezen en voor Windows-gebruikers is PowerShell beschikbaar.

Via de Azure-portal van het pictogram Cloud Shell starten:

![Portal starten](media/overview/portal-launch-icon.png)

Gebruikmaken van Bash of PowerShell uit de vervolgkeuzelijst van de selector shell:

![In de Cloud-Shell Bash](media/overview/overview-bash-pic.png)

![PowerShell in de Cloud-Shell](media/overview/overview-ps-pic.png)

## <a name="features"></a>Functies
### <a name="browser-based-shell-experience"></a>Browser gebaseerde shell-ervaring
Cloud-Shell kan toegang tot een browser gebaseerde opdrachtregelprogramma ervaring gebouwd met Azure beheertaken in gedachten.
Hefboomwerking Cloud Shell voelt van een lokale computer werkt op een manier alleen de cloud kunnen bieden.

### <a name="choice-of-preferred-shell-experience"></a>Keuze van voorkeur shell-ervaring
Azure Cloud Shell biedt u de flexibiliteit om de shell-ervaring te kiezen die het beste bij uw werkwijze past.
Linux-gebruikers kunnen voor een Bash-ervaring kiezen en voor Windows-gebruikers is PowerShell beschikbaar.

### <a name="pre-configured-azure-workstation"></a>Vooraf geconfigureerde Azure werkstation
Cloud-Shell is voorgeïnstalleerd met populaire opdrachtregelprogramma's en talen zodat u sneller kunt werken.

Overzicht van de volledige tooling voor [Bash ervaring](features.md#tools) en [PowerShell-ervaring.](features-powershell.md#tools)

### <a name="automatic-authentication"></a>Automatische verificatie
Cloud-Shell verifieert veilig automatisch op elke sessie voor directe toegang tot uw resources via de Azure CLI 2.0 of Azure PowerShell-cmdlets.

### <a name="connect-your-azure-file-storage"></a>Verbinding maken met uw Azure File storage
Cloud-Shell-machines zijn tijdelijk en als gevolg hiervan vereisen een Azure-bestanden-share moet worden gekoppeld als `clouddrive` voor het persistent maken van uw directory $Home.
Op de eerste keer opstarten die cloud Shell wordt gevraagd om een resource te maken delen groep, storage-account en -bestand namens jou. Dit is een eenmalige stap en wordt automatisch voor alle sessies worden gekoppeld. Één bestandsshare kan worden toegewezen en wordt gebruikt door zowel Bash als PowerShell in de Cloud-Shell.

#### <a name="create-new-storage"></a>Maken van nieuwe opslag
![](media/overview/basic-storage.png)

Een lokaal redundant opslagaccount (LRS) en de Azure-bestanden delen kunnen namens jou worden gemaakt. De share Azure-bestanden wordt gebruikt voor zowel Bash en PowerShell omgevingen als u wilt gebruiken. Reguliere opslagkosten van toepassing.

Drie bronnen worden namens jou gemaakt:
1. De resourcegroep met de naam:`cloud-shell-storage-<region>`
2. Opslagaccount met de naam:`cs<uniqueGuid>`
3. De bestandsshare met de naam:`cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bash in de Cloud-Shell maakt ook een standaardinstallatiekopie van 5 GB schijfruimte om te blijven behouden `$Home`. Alle bestanden in uw directory $Home zoals SSH-sleutels worden doorgevoerd in de schijfinstallatiekopie van uw gebruiker opgeslagen in de gekoppelde bestandsshare. Aanbevolen procedures van toepassing bij het opslaan van bestanden in uw directory $Home en gekoppelde bestandsshare.

#### <a name="use-existing-resources"></a>Bestaande bronnen gebruiken
![](media/overview/advanced-storage.png)

Een geavanceerde optie is opgegeven voor het koppelen van bestaande resources voor Cloud-Shell.
Klik op 'Weergeven geavanceerde instellingen' bij de prompt opslag setup om extra opties weer te geven.
Opgegeven waarin DropDowns worden gefilterd voor uw toegewezen Cloud Shell regio en lokaal/globaal-redundant storage-accounts.

[Meer informatie over opslag voor Cloud-Shell, gedeelde bestanden bijwerken en het uploaden/downloaden van bestanden.](persisting-shell-storage.md)

## <a name="concepts"></a>Concepten
* Cloud-Shell wordt uitgevoerd op een tijdelijke machine die is opgegeven op een per-sessie per gebruiker
* Time-out opgetreden voor de cloud Shell na 20 minuten zonder interactieve activiteit
* Cloud-Shell zijn alleen toegankelijk met een bestandsshare die is gekoppeld
* Cloud Shell gebruikt een dezelfde bestandsshare voor zowel Bash en PowerShell
* Cloud-Shell wordt één machine per gebruikersaccount toegewezen
* Machtigingen zijn ingesteld als een gewone gebruiker voor Linux (Bash)

Meer informatie over functies in [Bash in de Cloud-Shell](features.md) en [PowerShell in de Cloud-Shell](features-powershell.md).

## <a name="examples"></a>Voorbeelden
* Scripts gebruiken om Azure beheertaken te automatiseren
* Azure-resources via Azure portal en Azure-opdrachtregelprogramma's tegelijk beheren
* Test-Drive Azure CLI 2.0 of Azure PowerShell-cmdlets

Deze voorbeelden in snelstartgidsen voor uitproberen [Bash in de Cloud-Shell](quickstart.md) en [PowerShell in de Cloud-Shell](quickstart-powershell.md).

## <a name="pricing"></a>Prijzen
De computer die als host fungeert voor Cloud-Shell is gratis, met een vereiste van een gekoppelde Azure Files-share. Reguliere opslagkosten van toepassing.

## <a name="supported-browsers"></a>Ondersteunde browsers
Cloud-Shell wordt aanbevolen voor Chrome, rand en Safari.
Cloud-Shell wordt ondersteund voor Chrome, Firefox Safari, Internet Explorer en Edge, is Cloud Shell onderworpen aan specifieke browserinstellingen.
