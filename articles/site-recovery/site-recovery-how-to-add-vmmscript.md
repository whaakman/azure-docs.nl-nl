---
title: Scripts toevoegen aan het herstelplan in Azure Site Recovery | Microsoft Docs
description: Beschrijft de vereisten in een nieuwe script toevoegen aan een plan voor herstel in VMM naar Azure
services: site-recovery
documentationcenter: 
author: ruturaj
manager: shons
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/13/2017
ms.author: ruturaj
ms.openlocfilehash: 60c6eebd9323028c63f42bd8a0996e3c0a2a1cf1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="add-vmm-scripts-to-a-recovery-plan"></a>VMM scripts toevoegen aan een herstelplan


Dit artikel bevat informatie over het maken en toevoegen van scripts voor het plannen voor herstel in VMM [Azure Site Recovery](site-recovery-overview.md).

Eventuele opmerkingen of vragen kunt u onder aan dit artikel plaatsen of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites-of-adding-a-script-to-recovery-plan"></a>Vereisten voor het toevoegen van een script aan het herstelplan

U kunt de PowerShell-scripts gebruiken in uw plannen voor herstel. U moet ze ontwerpen en plaats deze in de VMM-bibliotheek voor toegang vanuit het herstelplan. Hieronder volgen enkele overwegingen terwijl u het script schrijft.

* Zorg ervoor dat de scripts trycatch-blokken gebruiken zodat de uitzonderingen probleemloos worden afgehandeld.
    - Er is een uitzondering opgetreden in het script, niet meer wordt uitgevoerd als de taak wordt weergegeven als mislukt.
    - Als een fout optreedt, worden eventuele resterende deel van het script niet uitgevoerd.
    - Als er een fout optreedt wanneer u een niet-geplande failover uitvoert, blijft het herstelplan.
    - Als een fout optreedt wanneer u een geplande failover uitvoert, wordt het herstelplan gestopt. U moet oplossen van het script, Controleer of deze wordt uitgevoerd zoals verwacht en het herstelplan opnieuw uit te voeren.
        - De Write-Host-opdracht werkt niet in een herstelplanscript en het script mislukken. Maak een proxyscript dat op zijn beurt uw belangrijkste script wordt uitgevoerd voor het maken van de uitvoer. Zorg ervoor dat alle uitvoer wordt doorgegeven om via de >> opdracht.
        - Het script wordt geblokkeerd als er binnen 600 seconden wordt niet geretourneerd.
        - Als is geschreven in STDERR, wordt het script wordt geclassificeerd als is mislukt. Deze informatie wordt weergegeven in de details van de uitvoering van script.

* Scripts in een herstelplan worden uitgevoerd in de context van de VMM-serviceaccount. Zorg ervoor dat dit account heeft leesrechten nodig voor de externe share waarop het script zich bevindt. Het script wilt uitvoeren op de VMM-service-account machtigingsniveau testen.
* VMM-cmdlets worden in Windows PowerShell-module geleverd. De module wordt geïnstalleerd wanneer u de VMM-console installeren. Het kan in uw script met de volgende opdracht in het script worden geladen:
   - Import-Module-naam beheer voor virtuele machines. [Meer informatie](https://technet.microsoft.com/library/hh875013.aspx).
* Zorg ervoor dat er ten minste één bibliotheekserver in uw VMM-implementatie. Standaard het bibliotheeksharepad voor een VMM-server zich lokaal op de VMM-server met de naam van de map MSCVMMLibrary.
    * Als uw bibliotheeksharepad externe (of lokale maar niet gedeeld met MSCVMMLibrary), de share als volgt configureren (met behulp van \\libserver2.contoso.com\share\ als voorbeeld):
      * Open de Register-Editor en navigeer naar **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.
      * Bewerk de waarde **ScriptLibraryPath** en plaats deze als \\libserver2.contoso.com\share\. Geef de volledige FQDN. Machtigingen opgeven voor de bestandsshare-locatie. Houd er rekening mee dat dit het hoofdknooppunt van de share is. **Als u wilt dit selectievakje inschakelt, kunt u de bibliotheek op het hoofdknooppunt in VMM bladeren. Het pad dat wordt geopend moet de hoofdmap van het pad - de website die u moet gebruiken in de variabele**.
      * Zorg ervoor dat u een gebruikersaccount met dezelfde machtigingen als de VMM-serviceaccount voor het script test. Controleert dit of zelfstandige scripts uitvoeren op dezelfde manier uit als op de herstelplannen getest. Op de VMM-beheerserver, stel het uitvoerbeleid om over te slaan als volgt:
        * Open de **64-bits Windows PowerShell** console met verhoogde bevoegdheden.
        * Type: **Set-executionpolicy bypass**. [Meer informatie](https://technet.microsoft.com/library/ee176961.aspx).

> [!IMPORTANT]
> U moet uitvoeringsbeleid bypass op de 64-bits PowerShell alleen instellen. Als u deze voor de 32-bits PowerShell hebt ingesteld, worden de scripts niet exeute.

## <a name="add-the-script-to-the-vmm-library"></a>Het script toevoegen aan de VMM-bibliotheek

Hebt u een VMM-bronsite, kunt u een script op de VMM-beheerserver maken en deze opnemen in uw plan voor herstel.

1. Maak een nieuwe map in de bibliotheekshare. Bijvoorbeeld: \<VMMServerName > \MSSCVMMLibrary\RPScripts. Plaats het op de bron en doel van de VMM-servers.
2. Het script (bijvoorbeeld RPScript) maken en controleren van de dat test werkt zoals verwacht.
3. Het script op de locatie plaatsen \<VMMServerName > \MSSCVMMLibrary op de bron en doel-VMM-servers.

## <a name="add-the-script-to-a-recovery-plan"></a>Het script toevoegen aan een plan voor herstel

Nadat u hebt toegevoegd virtuele machines of replicatiegroepen en het plan gemaakt, kunt u het script toevoegen aan de groep.

1. Open het herstelplan.
2. Klik op een item in de **stap** lijst en klik vervolgens op **Script** of **handmatige actie**.
3. Geef op of het script of actie vóór of na het geselecteerde item wilt toevoegen. Gebruik de **omhoog** en **omlaag** knoppen op de positie van het script omhoog of omlaag verplaatsen.
4. Als u een VMM-script toevoegen, selecteert u **Failover naar VMM script**. In **scriptpad**, typ het relatieve pad naar de share. In het onderstaande voorbeeld VMM geeft u het pad: **\RPScripts\RPScript.PS1**.
5. Als u een Azure automation-boek uitvoeren toevoegt, geef de Azure Automation-account waarin het runbook zich bevindt en het juiste Azure runbookscript te selecteren.
6. Voer een testfailover uitgevoerd van het herstelplan om ervoor te zorgen dat het script werkt zoals verwacht.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-failover.md) over het uitvoeren van failovers.
