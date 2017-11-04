---
title: Plannen voor herstel voor failover en herstel maken in Azure Site Recovery | Microsoft Docs
description: Hierin wordt beschreven hoe u maken en aanpassen van herstelplannen in Azure Site Recovery, failover en virtuele machines en fysieke servers herstellen
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/25/2017
ms.author: raynew
ms.openlocfilehash: 202e0ac8be36e9156ec16fadc1b722f4eb3d1432
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2017
---
# <a name="create-recovery-plans"></a>Herstelplannen maken


Dit artikel bevat informatie over het maken en aanpassen in de herstelplannen [Azure Site Recovery](site-recovery-overview.md).

U kunt onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.

 Maak herstelplannen het volgende doen:

* Groepen van de machines die samen een failover en start vervolgens samen definiëren.
* Model afhankelijkheden tussen machines, door ze te groeperen samen in een recovery plan groep. Bijvoorbeeld, failover en online zetten van een bepaalde toepassing is, groeperen u alle van de virtuele machines voor de toepassing naar dezelfde recovery plan groep.
* Een failover uitvoert. U kunt een test, gepland of niet-geplande failover uitvoeren op een herstelplan.


## <a name="create-a-recovery-plan"></a>Een herstelplan maken

1. Klik op **herstelplannen** > **herstelplan maken**.
   Geef een naam voor het herstelplan en een bron en doel. De bronlocatie moet virtuele machines hebt die zijn ingeschakeld voor failover en herstel.

    - Selecteer voor VMM naar VMM replicatie **brontype** > **VMM**, en de bron en doel-VMM-servers. Klik op **Hyper-V** om te zien van clouds die zijn beveiligd.
    - Selecteer voor VMM naar Azure **brontype** > **VMM**.  Selecteer de bron-VMM-beheerserver, en **Azure** als doel.
    - Voor Hyper-V-replicatie naar Azure (zonder VMM), selecteer **brontype** > **Hyper-V-site**. Selecteer de site als de bron en **Azure** als doel.
    - Voor een VMware-VM of een fysieke on-premises server maken in Azure, selecteert u een configuratieserver als de bron en **Azure** als doel.
    - Selecteer een Azure-regio als de bron- en een secundair Azure-regio als het doel voor een herstelplan van Azure naar Azure. Secundaire Azure-regio's zijn alleen degenen die virtuele machines worden beveiligd.
2. In **virtuele machines selecteren**, selecteer de virtuele machines (of replicatiegroep) die u wilt toevoegen aan de standaardgroep (groep 1) in het herstelplan.

## <a name="customize-and-extend-recovery-plans"></a>Aanpassen en uitbreiden herstelplannen

U kunt aanpassen en uitbreiden herstelplannen:

- **Nieuwe groepen toevoegen**: aanvullende herstelpunten planningsgroepen (maximaal zeven) toevoegen aan de standaardgroep en voeg vervolgens meer machines of replicatiegroepen aan deze herstelgroepen-plan. Groepen worden genummerd in de volgorde waarin u ze toevoegt. Een virtuele machine of de replicatiegroep, kan alleen worden opgenomen in één herstelpunt plan groep.
- **Toevoegen van een handmatige actie**— u handmatige acties die worden uitgevoerd vóór of na een groep voor gegevensherstel abonnement kunt toevoegen. Wanneer het herstelplan wordt uitgevoerd, stopt hij op het punt waar u de handmatige actie ingevoegd. Een dialoogvenster vraagt u om op te geven dat de handmatige actie is voltooid.
- **Een script toevoegen**— u kunt scripts toevoegen die worden uitgevoerd vóór of na een groep voor gegevensherstel plannen. Wanneer u een script toevoegt, wordt een nieuwe reeks handelingen voor de groep toegevoegd. Bijvoorbeeld, een reeks vooraf stappen voor het groep 1 wordt gemaakt met de naam: groep 1: vooraf stappen. Stappen voor alle vooraf wordt vermeld in deze set. U kunt alleen een script toevoegen op de primaire site hebt u een VMM-server geïmplementeerd.
- **Toevoegen van Azure-runbooks**— u kunt herstelplannen met Azure runbooks uitbreiden. Bijvoorbeeld, om taken te automatiseren, of één stap herstel maken. [Meer informatie](site-recovery-runbook-automation.md).

## <a name="add-scripts"></a>Voeg scripts toe

U kunt de PowerShell-scripts gebruiken in uw plannen voor herstel.

 - Zorg ervoor dat de scripts trycatch-blokken gebruiken zodat de uitzonderingen probleemloos worden afgehandeld.
    - Er is een uitzondering opgetreden in het script, niet meer wordt uitgevoerd als de taak wordt weergegeven als mislukt.
    - Als een fout optreedt, worden eventuele resterende deel van het script niet uitgevoerd.
    - Als er een fout optreedt wanneer u een niet-geplande failover uitvoert, blijft het herstelplan.
    - Als een fout optreedt wanneer u een geplande failover uitvoert, wordt het herstelplan gestopt. U moet oplossen van het script, Controleer of deze wordt uitgevoerd zoals verwacht en het herstelplan opnieuw uit te voeren.
- De Write-Host-opdracht werkt niet in een herstelplanscript en het script mislukken. Maak een proxyscript dat op zijn beurt uw belangrijkste script wordt uitgevoerd voor het maken van de uitvoer. Zorg ervoor dat alle uitvoer wordt doorgegeven om via de >> opdracht.
  * Het script wordt geblokkeerd als er binnen 600 seconden wordt niet geretourneerd.
  * Als is geschreven in STDERR, wordt het script wordt geclassificeerd als is mislukt. Deze informatie wordt weergegeven in de details van de uitvoering van script.

Als u VMM in uw implementatie:

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
> U moet het uitvoeringsbeleid bypass instellen op alleen de 64-bits-powershell. Als u deze voor de 32-bits powershell hebt ingesteld, worden de scripts niet exeute.

## <a name="add-a-script-or-manual-action-to-a-plan"></a>Een script of handmatige actie toevoegen aan een plan

Nadat u hebt toegevoegd virtuele machines of replicatiegroepen en het plan gemaakt, kunt u een script toevoegen aan de standaardgroep voor het plannen van herstel.

1. Open het herstelplan.
2. Klik op een item in de **stap** lijst en klik vervolgens op **Script** of **handmatige actie**.
3. Geef op of het script of actie vóór of na het geselecteerde item wilt toevoegen. Gebruik de **omhoog** en **omlaag** knoppen op de positie van het script omhoog of omlaag verplaatsen.
4. Als u een VMM-script toevoegen, selecteert u **Failover naar VMM script**. In **scriptpad**, typ het relatieve pad naar de share. In het onderstaande voorbeeld VMM geeft u het pad: **\RPScripts\RPScript.PS1**.
5. Als u een Azure automation-boek uitvoeren toevoegt, geef de Azure Automation-account waarin het runbook zich bevindt en het juiste Azure runbookscript te selecteren.
6. Voer een failover van het herstelplan om ervoor te zorgen dat het script werkt zoals verwacht.


### <a name="add-a-vmm-script"></a>Een VMM-script toevoegen

Hebt u een VMM-bronsite, kunt u een script op de VMM-beheerserver maken en deze opnemen in uw plan voor herstel.

1. Maak een nieuwe map in de bibliotheekshare. Bijvoorbeeld: \<VMMServerName > \MSSCVMMLibrary\RPScripts. Plaats het op de bron en doel van de VMM-servers.
2. Het script (bijvoorbeeld RPScript) maken en controleren van de dat test werkt zoals verwacht.
3. Het script op de locatie plaatsen \<VMMServerName > \MSSCVMMLibrary op de bron en doel-VMM-servers.


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-failover.md) over het uitvoeren van failovers.
