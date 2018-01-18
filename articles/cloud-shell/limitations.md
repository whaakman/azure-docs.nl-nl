---
title: Azure Cloud-Shell-beperkingen | Microsoft Docs
description: Overzicht van de beperkingen van Azure Cloud Shell
services: azure
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
ms.openlocfilehash: 7e498582d78d2807070c943dfd838dd9efeb4ed2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="limitations-of-azure-cloud-shell"></a>Beperkingen van de Azure-Cloud-Shell

Azure Cloud-Shell heeft de volgende beperkingen:

## <a name="general-limitations"></a>Algemene beperkingen

### <a name="system-state-and-persistence"></a>Systeemstatus en persistentie

De computer waarmee u uw Cloud-Shell-sessie is tijdelijk en deze wordt gerecycled nadat uw sessie is niet actief is gedurende 20 minuten. Cloud-Shell vereist een Azure-bestandsshare te koppelen. Uw abonnement moet als gevolg hiervan kunnen storage-resources instellen voor toegang tot Cloud-Shell. Andere overwegingen zijn onder andere:

* Met gekoppelde opslag, alleen de wijzigingen in de `clouddrive` directory blijven bestaan. In Bash, uw `$Home` directory ook persistent is gemaakt.
* Azure-bestandsshares worden gekoppeld, alleen vanuit uw [regio toegewezen](persisting-shell-storage.md#mount-a-new-clouddrive).
  * Voer in Bash, `env` vinden van uw regio ingesteld als `ACC_LOCATION`.
* Azure Files ondersteunt alleen lokaal redundante opslag en geo-redundant storage-accounts.

### <a name="browser-support"></a>Browserondersteuning

Cloud-Shell biedt ondersteuning voor de nieuwste versies van Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox en Apple Safari. Safari in privé-modus wordt niet ondersteund.

### <a name="copy-and-paste"></a>Kopiëren en plakken

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Voor een bepaalde gebruiker kan slechts één shell actief zijn

Gebruikers kunnen alleen openen één type shell op een tijdstip ofwel **Bash** of **PowerShell**. U wellicht echter meerdere exemplaren van Bash of PowerShell tegelijkertijd uitvoert. Wisselen tussen Bash of PowerShell oorzaken Cloud Shell opnieuw wordt opgestart, waarmee bestaande sessies wordt beëindigd.

### <a name="usage-limits"></a>Gebruiksbeperkingen

Cloud-Shell is bedoeld voor interactieve gebruiksvoorbeelden. Als gevolg hiervan zijn geen niet-interactieve sessies langlopende beëindigd zonder waarschuwing.

## <a name="bash-limitations"></a>Bash-beperkingen

### <a name="user-permissions"></a>Gebruikersmachtigingen

Machtigingen zijn ingesteld als gewone gebruikers zonder toegang tot sudo. Elke installatie buiten uw `$Home` directory is niet persistent.

### <a name="clouddrive-smb-limited-permissions"></a>Clouddrive SMB beperkte machtigingen
Bepaalde opdrachten binnen de `clouddrive` map zoals `git clone`, beschikt niet over de juiste machtigingen voor bepaalde bestanden lezen/schrijven. Als u dit probleem raakt, opnieuw proberen van uw `$Home` directory waarvoor geen beperkingen van SMB.

### <a name="editing-bashrc"></a>Editing .bashrc

Waarschuwing nemen bij het bewerken van .bashrc, in dat geval kan leiden tot onverwachte fouten in de Cloud-Shell.

### <a name="bashhistory"></a>.bash_history

De geschiedenis van bash opdrachten mogelijk inconsistent vanwege Cloud Shell-sessie wordt onderbroken of gelijktijdige sessies.

## <a name="powershell-limitations"></a>PowerShell-beperkingen

### <a name="slow-startup-time"></a>Trage opstarten

PowerShell in Azure Cloud-Shell (Preview) kan enige tijd duren om te initialiseren tijdens de preview 60 seconden.

### <a name="no-home-directory-persistence"></a>No $Home directory persistentie

Gegevens geschreven naar `$Home` door elke toepassing (bijvoorbeeld: git, vim en andere) niet bewaard is gebleven via PowerShell-sessies. Voor een tijdelijke oplossing [Hier ziet](troubleshooting.md#powershell-resolutions).

### <a name="default-file-location-when-created-from-azure-drive"></a>Standaardbestandslocatie tijdens het maken van het Azure-station:

Met PowerShell-cmdlets, kunnen gebruikers niet maken onder het Azure-station. Wanneer gebruikers nieuwe bestanden met andere hulpprogramma's, zoals vim of nano, maken worden de bestanden naar de map C:\Users standaard opgeslagen. 

### <a name="gui-applications-are-not-supported"></a>GUI-toepassingen worden niet ondersteund

Als de gebruiker wordt uitgevoerd een opdracht die u een Windows-dialoogvenster zoals maakt `Connect-AzureAD` of `Login-AzureRMAccount`, een foutbericht zoals ziet: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

## <a name="next-steps"></a>Volgende stappen

[Het oplossen van Cloud-Shell](troubleshooting.md) <br>
[Snelstartgids voor Bash](quickstart.md) <br>
[Snelstartgids voor PowerShell](quickstart-powershell.md)
