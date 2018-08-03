---
title: Een script toevoegen aan een plan voor herstel in Azure Site Recovery | Microsoft Docs
description: Meer informatie over de vereisten voor het toevoegen van een nieuwe System Center Virtual Machine Manager (VMM)-script aan een plan voor herstel in Azure.
services: site-recovery
documentationcenter: ''
author: rajani-janaki-ram
manager: rochakm
editor: ''
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: rajanaki
ms.openlocfilehash: 0b2bb17c85f76498e11ea3f007d55d7488f249cf
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426884"
---
# <a name="add-a-vmm-script-to-a-recovery-plan"></a>Een VMM-script toevoegen aan een plan voor herstel

In dit artikel wordt beschreven hoe u een script voor System Center Virtual Machine Manager (VMM) maken en toe te voegen aan een plan voor herstel in [Azure Site Recovery](site-recovery-overview.md).

Eventuele opmerkingen of vragen plaatsen aan de onderkant van dit artikel of op de [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Vereisten

U kunt PowerShell-scripts gebruiken in uw plannen voor herstel. Zodat ze toegankelijk zijn vanuit het herstelplan te gaan, moet u de auteur van het script en plaats het script in de VMM-bibliotheek. Houd er rekening mee met de volgende overwegingen wanneer u het script te schrijven:

* Zorg dat scripts trycatch-blokken, gebruikt zodat uitzonderingen correct worden verwerkt.
    - Als er een uitzondering in het script optreedt, het script niet meer werkt en ziet u de taak is mislukt.
    - Als er een fout optreedt, wordt de rest van het script niet uitvoeren.
    - Als wordt een fout optreedt wanneer u een niet-geplande failover uitvoert, blijft het herstelplan te gaan.
    - Als er een fout optreedt wanneer u een geplande failover uitvoert, stopt het herstelplan te gaan. Herstel het script, Controleer of deze wordt uitgevoerd zoals verwacht en voer vervolgens het herstelplan opnieuw.
        - De `Write-Host` opdracht werkt niet in een herstelplanscript. Als u de `Write-Host` opdracht in een script wordt het script is mislukt. Voor het maken van uitvoer, maakt u een proxyscript dat op zijn beurt de belangrijkste script wordt uitgevoerd. Om ervoor te zorgen dat alle uitvoer van wordt doorgegeven, gebruikt u de **\> \>** opdracht.
        - Het script een time-out optreedt als deze niet in 600 seconden retourneren.
        - Als is geschreven naar STDERR, wordt het script wordt geclassificeerd als is mislukt. Deze informatie wordt weergegeven in de details van het script kan worden uitgevoerd.

* Scripts in een herstelplan worden uitgevoerd in de context van de VMM-serviceaccount. Zorg ervoor dat dit account leesmachtigingen heeft voor de externe share waarop het script zich bevindt. Test het script uit te voeren met hetzelfde niveau van gebruikersrechten als het VMM-serviceaccount.
* VMM-cmdlets worden geleverd in een Windows PowerShell-module. De module wordt geïnstalleerd wanneer u de VMM-console installeren. Gebruik de volgende opdracht in het script voor het laden van de module in uw script: 

    `Import-Module -Name virtualmachinemanager`

    Zie voor meer informatie, [aan de slag met Windows PowerShell en VMM](https://technet.microsoft.com/library/hh875013.aspx).
* Zorg ervoor dat er ten minste één bibliotheekserver in uw VMM-implementatie. Standaard het bibliotheeksharepad voor een VMM-server zich lokaal op de VMM-server. Naam van de map is MSCVMMLibrary.

  Als uw bibliotheeksharepad is extern (of als het lokaal maar niet gedeeld met MSCVMMLibrary is), configureert u de share als volgt met behulp van \\libserver2.contoso.com\share\ als een voorbeeld:
  
  1. Open de Register-Editor en ga vervolgens naar **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.

  1. Wijzig de waarde voor **ScriptLibraryPath** naar  **\\\libserver2.contoso.com\share\\**. Geef de volledige FQDN-naam. Machtigingen opgeven voor de locatie van de bestandsshare. Dit is het hoofdknooppunt van de share. Om te controleren of het hoofdknooppunt in VMM, gaat u naar het hoofdknooppunt in de bibliotheek. Het pad dat wordt geopend, is de hoofdmap van het pad. Dit is het pad dat u in de variabele moet gebruiken.

  1. Test het script met behulp van een gebruikersaccount dat de hetzelfde niveau van gebruikersrechten als het VMM-serviceaccount is. Met behulp van deze rechten, verifieert die zelfstandige, geteste scripts die de dezelfde manier waarop ze worden uitgevoerd in plannen voor herstel worden uitgevoerd. Stel op de VMM-server het uitvoeringsbeleid om over te slaan, als volgt:

     a. Open de **64-bits Windows PowerShell** console als beheerder.
     
     b. Voer **Set-executionpolicy bypass**. Zie voor meer informatie, [met de cmdlet Set-ExecutionPolicy](https://technet.microsoft.com/library/ee176961.aspx).

     > [!IMPORTANT]
     > Stel **Set-executionpolicy bypass** alleen in de 64-bits PowerShell-console. Als u deze voor de 32-bits PowerShell-console instelt, de scripts niet uitgevoerd.

## <a name="add-the-script-to-the-vmm-library"></a>Het script toevoegen aan de VMM-bibliotheek

Als u een VMM-bronsite hebt, kunt u een script maken op de VMM-server. Voeg vervolgens het script in uw plan voor herstel.

1. Maak een nieuwe map in de bibliotheekshare. Bijvoorbeeld, \<VMM-servernaam > \MSSCVMMLibrary\RPScripts. Plaats de map op de bron en doel van de VMM-servers.
1. Het script maken. Bijvoorbeeld: naam van het script RPScript. Controleer of het script werkt zoals verwacht.
1. Plaats het script in de \<VMM-servernaam > \MSSCVMMLibrary map op de bron- en VMM-servers.

## <a name="add-the-script-to-a-recovery-plan"></a>Het script toevoegen aan een plan voor herstel

Nadat u de virtuele machines of replicatiegroepen hebt toegevoegd aan een herstelplan en de planning hebt gemaakt, u het script aan de groep toevoegen kunt.

1. Open het herstelplan te gaan.
1. In de **stap** , selecteert u een item. Selecteer vervolgens een **Script** of **handmatige actie**.
1. Geef op of het script of actie vóór of na het geselecteerde item toe te voegen. Voor het verplaatsen van de positie van het script omhoog of omlaag, selecteer de **omhoog** en **omlaag** knoppen.
1. Als u een VMM-script toevoegen, selecteert u **VMM script voor Failover naar**. In **scriptpad**, voert u het relatieve pad naar de share. Voer bijvoorbeeld **\RPScripts\RPScript.PS1**.
1. Als u een Azure Automation-runbook toevoegt, geeft u het Automation-account waarin het runbook zich bevindt. Selecteer vervolgens de Azure-runbookscript die u wilt gebruiken.
1. Om ervoor te zorgen dat het script zoals werkt verwacht, voer een failovertest uit van het herstelplan te gaan.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [uitvoeren van failovers](site-recovery-failover.md).

