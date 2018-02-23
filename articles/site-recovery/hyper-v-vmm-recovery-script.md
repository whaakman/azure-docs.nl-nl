---
title: Een script toevoegen aan een plan voor herstel in de Azure Site Recovery | Microsoft Docs
description: Meer informatie over de vereisten voor het nieuwe System Center Virtual Machine Manager (VMM)-scripts toevoegen aan een plan voor herstel in Azure.
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
ms.openlocfilehash: 2e00f812fb35ac9a0cb390fc6a3ba40a8678f8dd
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="add-a-vmm-script-to-a-recovery-plan"></a>Een VMM-script toevoegen aan een plan voor herstel

Dit artikel wordt beschreven hoe u een script voor System Center Virtual Machine Manager (VMM) maken en toe te voegen aan een plan voor herstel in [Azure Site Recovery](site-recovery-overview.md).

Eventuele opmerkingen of vragen plaatsen aan de onderkant van dit artikel of op de [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Vereisten

U kunt de PowerShell-scripts gebruiken in uw plannen voor herstel. Toegankelijk te zijn uit het herstelplan, moet u de auteur van het script en het script plaatsen in de VMM-bibliotheek. Houd rekening met de volgende overwegingen wanneer u het script schrijven:

* Zorg ervoor dat scripts trycatch-blokken gebruiken zodat uitzonderingen probleemloos worden afgehandeld.
    - Als er een uitzondering optreedt in het script, wordt het script wordt beëindigd en de taak wordt weergegeven als mislukt.
    - Als een fout optreedt, wordt de rest van het script niet uitgevoerd.
    - Als er een fout optreedt wanneer u een niet-geplande failover uitvoert, blijft het herstelplan.
    - Als een fout optreedt wanneer u een geplande failover uitvoert, wordt het herstelplan gestopt. Herstel het script, Controleer of deze wordt uitgevoerd zoals verwacht en het herstelplan opnieuw uit te voeren.
        - De `Write-Host` opdracht werkt niet in een herstelplanscript. Als u de `Write-Host` opdracht in een script wordt het script is mislukt. Maak een proxyscript dat op zijn beurt uw belangrijkste script wordt uitgevoerd voor het maken van de uitvoer. Om ervoor te zorgen dat alle uitvoer uit wordt doorgegeven, gebruiken de  **\> \>**  opdracht.
        - Het script wordt geblokkeerd als er binnen 600 seconden wordt niet geretourneerd.
        - Als is geschreven in STDERR, wordt het script wordt geclassificeerd als is mislukt. Deze informatie wordt weergegeven in de details van de uitvoering van script.

* Scripts in een herstelplan worden uitgevoerd in de context van de VMM-serviceaccount. Zorg ervoor dat dit account leesmachtigingen heeft voor de externe share waarop het script zich bevindt. Test het script uit te voeren met hetzelfde niveau van gebruikersrechten als de VMM-serviceaccount.
* VMM-cmdlets worden in Windows PowerShell-module geleverd. De module wordt geïnstalleerd wanneer u de VMM-console installeren. Als u wilt laden van de module in uw script, gebruik de volgende opdracht in het script: 

    `Import-Module -Name virtualmachinemanager`

    Zie voor meer informatie [aan de slag met Windows PowerShell en VMM](https://technet.microsoft.com/library/hh875013.aspx).
* Zorg ervoor dat er ten minste één bibliotheekserver in uw VMM-implementatie. Standaard het bibliotheeksharepad voor een VMM-server zich lokaal op de VMM-server. De mapnaam is MSCVMMLibrary.

  Als uw bibliotheeksharepad extern (of als dit lokaal maar niet gedeeld met MSCVMMLibrary), de share als volgt configureren, met behulp van \\libserver2.contoso.com\share\ als een voorbeeld:
  
  1. Open de Register-Editor en Ga naar **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.

  2. Wijzig de waarde voor **ScriptLibraryPath** naar  **\\\libserver2.contoso.com\share\\**. Geef de volledige FQDN. Machtigingen opgeven voor de bestandsshare-locatie. Dit is het hoofdknooppunt van de share. Ga naar het hoofdknooppunt in de bibliotheek om te controleren of het hoofdknooppunt in VMM. Het pad dat wordt geopend, is de hoofdmap van het pad. Dit is het pad dat u in de variabele moet gebruiken.

  3. Het script testen met behulp van een gebruikersaccount met hetzelfde niveau van gebruikersrechten als de VMM-serviceaccount. Met behulp van deze gebruikersrechten, controleert die zelfstandige geteste scripts uitgevoerd dezelfde manier als ze worden uitgevoerd in de herstelplannen. Op de VMM-beheerserver, stel het uitvoerbeleid om over te slaan als volgt:

     a. Open de **64-bits Windows PowerShell** console als beheerder.
     
     b. Voer **Set-executionpolicy bypass**. Zie voor meer informatie [met de cmdlet Set-ExecutionPolicy](https://technet.microsoft.com/library/ee176961.aspx).

     > [!IMPORTANT]
     > Stel **Set-executionpolicy bypass** alleen in de 64-bits PowerShell-console. Als u deze voor de 32-bits PowerShell-console instelt, de scripts niet uitgevoerd.

## <a name="add-the-script-to-the-vmm-library"></a>Het script toevoegen aan de VMM-bibliotheek

Als u een bronsite VMM hebt, kunt u een script op de VMM-server. Voeg vervolgens het script in uw plan voor herstel.

1. Maak een nieuwe map in de bibliotheekshare. Bijvoorbeeld: \<VMM-servernaam > \MSSCVMMLibrary\RPScripts. Plaats de map op de bron en doel van de VMM-servers.
2. Maak het script. Het script RPScript bijvoorbeeld de naam. Controleer of het script werkt zoals verwacht.
3. Plaats het script in de \<VMM-servernaam > \MSSCVMMLibrary-map op de bron en doel-VMM-servers.

## <a name="add-the-script-to-a-recovery-plan"></a>Het script toevoegen aan een plan voor herstel

Nadat u virtuele machines of replicatiegroepen aan een plan voor herstel hebt toegevoegd en het plan gemaakt, u het script aan de groep toevoegen kunt.

1. Open het herstelplan.
2. In de **stap** , selecteert u een item. Selecteer een **Script** of **handmatige actie**.
3. Geef op of het script of actie vóór of na het geselecteerde item toe te voegen. Als u wilt de positie van het script omhoog of omlaag verplaatsen, selecteert u de **omhoog** en **omlaag** knoppen.
4. Als u een VMM-script toevoegen, selecteert u **Failover naar VMM script**. In **scriptpad**, voert u het relatieve pad naar de share. Voer bijvoorbeeld **\RPScripts\RPScript.PS1**.
5. Als u een Azure Automation-runbook toevoegt, geeft u het Automation-account waarin het runbook zich bevindt. Selecteer vervolgens het runbookscript in Azure dat u wilt gebruiken.
6. Om ervoor te zorgen dat het script werkt zoals verwacht, doet u een testfailover uitgevoerd van het herstelplan.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [failovers met](site-recovery-failover.md).

