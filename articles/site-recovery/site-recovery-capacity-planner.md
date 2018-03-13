---
title: Schatting maken van de capaciteit van de replicatie in Azure | Microsoft Docs
description: Gebruik dit artikel om in te schatten capaciteit wanneer u repliceert met behulp van Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/09/2018
ms.author: nisoneji
ms.openlocfilehash: d9c2645be73c4b6e34d194d6b2444a700e3900d2
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="plan-capacity-for-protecting-hyper-v-vms-with-site-recovery"></a>Plannen van capaciteit voor het beveiligen van Hyper-V-machines met Site Recovery

Een nieuwe uitgebreide versie van [Azure Site Recovery implementatie Planner voor Hyper-V naar Azure-implementatie](site-recovery-hyper-v-deployment-planner.md) is nu beschikbaar. Het oude hulpprogramma vervangt. Het nieuwe hulpprogramma gebruiken voor het plannen van uw implementatie.
Het hulpprogramma biedt de volgende richtlijnen:

* Evaluatie van VM in aanmerking komt, op basis van het aantal schijven, schijfgrootte IOP's, verloop en enkele VM-kenmerken
* Netwerkbandbreedte moet versus RPO assessment
* Vereisten voor Azure-infrastructuur
* On-premises infrastructuurvereisten
* Initiële replicatie batchverwerking richtlijnen
* Geschatte kosten van de totale disaster recovery naar Azure


Azure Site Recovery-Capaciteitsplanner helpt u bij uw capaciteitsvereisten bepalen wanneer u Hyper-V-machines met Azure Site Recovery repliceert.

Site Recovery Capacity Planner gebruiken voor het analyseren van uw bronomgeving en werkbelastingen. Kunt u de schatting van de behoeften van de bandbreedte, de server-bronnen die u nodig hebt voor de bronlocatie, en de resources (zoals virtuele machines en opslag) moet u in de doellocatie.

U kunt het hulpprogramma uitvoeren in twee modi:

* **Snelle planning**: netwerk- en projecties op basis van een gemiddelde aantal virtuele machines, schijven, opslag en wijzigingssnelheid biedt.
* **Gedetailleerde planning**: bevat details van elke werkbelasting op het niveau van de virtuele machine. VM-compatibiliteit analyseren en netwerk- en projecties ophalen.

## <a name="before-you-start"></a>Voordat u begint

* Gegevens verzamelen over uw omgeving, inclusief virtuele machines, schijven per virtuele machine, opslag per schijf.
* De snelheid van uw dagelijkse wijzigen (verloop) voor gerepliceerde gegevens identificeren. Download de [Hyper-V-hulpprogramma voor capaciteitsplanning](https://www.microsoft.com/download/details.aspx?id=39057) de wijzigingssnelheid ophalen. [Meer informatie](site-recovery-capacity-planning-for-hyper-v-replication.md) over dit hulpprogramma. Het is raadzaam dat u dit hulpprogramma uitvoeren via een week om vast te leggen gemiddelden.


## <a name="run-the-quick-planner"></a>De snelle Planner uitvoeren
1. Download en open [Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel). U moet uitvoeren van macro's. Wanneer u wordt gevraagd, moet u selecties inschakelen bewerken en inhoud.

2. In de **Selecteer een type planner** keuzelijst met invoervak, selecteer **snelle Planner**.

   ![Aan de slag](./media/site-recovery-capacity-planner/getting-started.png)

3. Op de **Capacity Planner** werkblad, voer de vereiste informatie. Vul alle velden omcirkeld in rood in de volgende schermafbeelding:

   a. In **selecteert u uw scenario**, kies **Hyper-V naar Azure** of **VMware of fysieke naar Azure**.

   b. In **gemiddelde dagelijkse gegevens snelheid (%) wijzigen**, voert u de verzamelde informatie met behulp van de [Hyper-V-hulpprogramma voor capaciteitsplanning](site-recovery-capacity-planning-for-hyper-v-replication.md) of [Site Recovery implementatie Planner](./site-recovery-deployment-planner.md).

   c. De **compressie** instelling wordt niet gebruikt wanneer u Hyper-V-machines naar Azure repliceren. Gebruik een toestel van derden zoals Riverbed voor compressie.

   d. In **bewaartermijn in dagen**, Geef op in dagen hoe lang is voor het bewaren van replica's.

   e. In **aantal uren in waarop de initiële replicatie voor de batch van virtuele machines moet worden voltooid** en **aantal virtuele machines per batch van de initiële replicatie**, voer de instellingen die worden gebruikt om te berekenen de vereisten van de initiële replicatie. Wanneer Site Recovery wordt geïmplementeerd, wordt de hele initiële gegevensset geüpload.

   ![Invoer](./media/site-recovery-capacity-planner/inputs.png)

4. Nadat u de waarden voor de bronomgeving, bevat de uitvoer weergegeven:

   * **Bandbreedte vereist is voor replicatie van verschillen (in Megabits per seconde)**: netwerkbandbreedte voor replicatie van verschillen wordt berekend op de gemiddelde dagelijkse wijzigingssnelheid van gegevens.
   * **Bandbreedte die is vereist voor de initiële replicatie (in Megabits per seconde)**: netwerkbandbreedte voor de initiële replicatie wordt berekend op de initiële replicatie waarden die u invoert.
   * **Opslag vereist (in GB's)**: het totale aantal Azure-opslag vereist.
   * **Totaal aantal IOPS op standaardopslag**: het aantal wordt berekend op basis van de grootte van 8 kB IOPS op de totale standaard storage-accounts. Het getal wordt berekend op basis van de schijven voor de VM bron voor de snelle Planner en snelheid van de dagelijkse gegevens wijzigen. Het getal wordt berekend op basis van het totale aantal virtuele machines die zijn toegewezen aan de standaard virtuele Azure-machines en de gegevens wijzigen tarief op deze virtuele machines voor de gedetailleerde Planner.
   * **Het aantal vereiste accounts voor Standard-opslag**: het totale aantal standaard storage-accounts die nodig zijn voor het beveiligen van de virtuele machines. Standard-opslagaccount kan maximaal 20.000 IOP's bevatten over de VM's in standard-opslag. Maximaal 500 IOP's wordt per schijf ondersteund.
   * **Het aantal Blob-schijven die zijn vereist**: het aantal schijven die zijn gemaakt op Azure-opslag.
   * **Aantal premium-accounts vereist**: het totale aantal premium storage-accounts die nodig zijn voor het beveiligen van de virtuele machines. Een bron-VM met een hoge IOPS (groter dan 20.000) moet een premium storage-account. Een premium storage-account kan maximaal 80.000 IOP's bevatten.
   * **Totaal aantal IOPS op Premium-opslag**: het aantal wordt berekend op basis van de grootte van 256 kB IOPS op de totale premium storage-accounts. Het getal wordt berekend op basis van de schijven voor de VM bron voor de snelle Planner en snelheid van de dagelijkse gegevens wijzigen. Het getal wordt berekend op basis van het totale aantal virtuele machines die zijn toegewezen aan een premium Azure Virtual machines (Active Directory en GS-serie) en de gegevens wijzigen op deze virtuele machines voor de gedetailleerde Planner.
   * **Aantal vereiste configuratieservers**: geeft aan hoeveel configuratieservers vereist voor de implementatie.
   * **Aantal vereiste aanvullende processervers**: geeft aan of extra processervers vereist, naast de processerver die wordt uitgevoerd op de configuratieserver standaard.
   * **Extra opslagruimte op de bron-100%**: toont of extra opslagruimte op de bronlocatie vereist is.

      ![Uitvoer](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>De gedetailleerde Planner uitvoeren

1. Download en open [Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel). U moet uitvoeren van macro's. Wanneer u wordt gevraagd, moet u selecties inschakelen bewerken en inhoud.

2. In **Selecteer een type planner**, selecteer **gedetailleerde Planner** in de keuzelijst.

   ![Introductiehandleiding](./media/site-recovery-capacity-planner/getting-started-2.png)

3. Op de **werkbelasting kwalificatie** werkblad, voer de vereiste informatie. U moet de gemarkeerde velden invullen.

   a. In **Processor-Cores**, het totale aantal kernen op een bronserver opgeven.

   b. In **geheugentoewijzing (in MB)**, de grootte van het RAM-geheugen van een bronserver opgeven.

   c. In **nummer van de NIC's**, geef het aantal netwerkadapters op een bronserver.

   d. In **totale opslag (in GB)**, geef de totale grootte van de VM-opslag. Als de bronserver drie schijven met 500 GB heeft, is de grootte van de totale opslagruimte 1500 GB.

   e. In **aantal gekoppelde schijven**, het totale aantal schijven van een bronserver opgeven.

   f. In **schijf gebruik van capaciteit (%)**, geef het gemiddelde gebruik.

   g. In **dagelijkse gegevens wijzigen snelheid (%)**, geef de dagelijkse hoeveelheden frequentie van een bronserver wijzigen.

   h. In **toewijzing Azure VM-grootte**, geef de Azure VM-grootte die u wilt toewijzen. Als u niet dat dit handmatig doen wilt, selecteert u **Compute IaaS VM's**. Als u een handmatige instelling invoer en selecteer vervolgens **Compute IaaS VM's**, de handmatige instelling mogelijk overschreven. Het proces compute identificeert automatisch de beste overeenkomst op Azure VM-grootte.

   ![Werkbelasting kwalificatie werkblad](./media/site-recovery-capacity-planner/workload-qualification.png)

4. Als u selecteert **Compute IaaS VM's**, dit is wat het doet:

   * Valideert de verplichte invoeren.
   * Berekent de IOPS en suggestie het beste Azure VM grootte voor elke virtuele machine die in aanmerking komt voor replicatie naar Azure. Als een correcte grootte die virtuele Azure-machine kan niet worden gedetecteerd, wordt er een fout weergegeven. Als het aantal gekoppelde schijven 65 is een fout wordt weergegeven omdat de grootte van de hoogste voor een Azure VM 64 is is.
   * Stelt een opslagaccount die kan worden gebruikt voor een virtuele machine in Azure.
   * Berekent het totale aantal standaard storage-accounts en premium-opslagaccounts die zijn vereist voor de werkbelasting. Schuif omlaag om het type Azure-opslag en het opslagaccount dat kan worden gebruikt voor een bronserver te bekijken.
   * Is voltooid en de rest van de tabel op basis van het vereiste opslagtype (standard of premium) toegewezen voor een virtuele machine en het aantal gekoppelde schijven gesorteerd. Voor alle virtuele machines die voldoen aan de vereisten voor Azure, de kolom **Is VM gekwalificeerd?** toont **Ja**. Als een virtuele machine kan niet een back-up naar Azure, wordt er een fout weergegeven.

Kolommen AA AE worden uitgevoerd en informatie te verstrekken voor elke virtuele machine.

![Uitvoerkolommen AA AE](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Voorbeeld
Als u bijvoorbeeld voor zes VM's met de waarden in de tabel het hulpprogramma wordt berekend en wijst het meest geschikt voor de virtuele machine van Azure en de vereisten voor Azure-opslag.

![Werkbelasting kwalificatie toewijzingen](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Let op het volgende in het voorbeeld van uitvoer:

  * De eerste kolom is een kolom validatie voor de virtuele machines, schijven en verloop.
  * Twee standaard storage-accounts en een premium-opslagaccount nodig zijn voor vijf virtuele machines.
  * VM3 niet in aanmerking voor beveiliging, omdat een of meer schijven meer dan 1 TB zijn.
  * VM1 en VM2 van kunt het eerste standard-opslag-account gebruiken
  * VM4 kunt de tweede standard-opslagaccount gebruiken.
  * VM5 en VM6 moet een premium-opslagaccount en beide één account kunnen gebruiken.

    > [!NOTE]
    > IOPS op standard en premium storage worden berekend op het niveau van de virtuele machine en niet op het niveau van de schijf. Een standaard virtuele machine kan verwerken maximaal 500 IOP's per schijf. Als IOPS aan voor een schijf groter dan 500 zijn, moet u premium-opslag. Als IOPS aan voor een schijf meer dan 500 zijn maar IOP's voor het totale aantal VM-schijven binnen de grenzen van ondersteuning standaard Azure VM zijn, hervat de planner een standaard virtuele machine en niet de DS- of GS-serie. (De limieten van de virtuele machine in Azure zijn VM-grootte, het aantal schijven, het aantal adapters, CPU en geheugen). U moet handmatig bijwerken van de cel van de Azure-grootte toewijzing de juiste Active Directory of GS-serie VM.


Nadat alle gegevens zijn ingevoerd, selecteert u **gegevens verzenden naar het hulpprogramma planner** Capacity Planner openen. Werkbelastingen zijn om weer te geven of ze in aanmerking komen voor beveiliging zijn gemarkeerd.

### <a name="submit-data-in-capacity-planner"></a>Verzenden van gegevens in Capacity Planner
1. Wanneer u opent de **Capacity Planner** werkblad dit ingevuld op basis van de instellingen die u hebt opgegeven. Het woord 'Werkbelasting' wordt weergegeven in de **Infra invoer bron** cel aangeven dat de invoer is de **werkbelasting kwalificatie** werkblad.

2. Als u wijzigingen aanbrengen wilt, moet u wijzigt de **werkbelasting kwalificatie** werkblad. Selecteer vervolgens **gegevens verzenden naar het hulpprogramma planner** opnieuw.

   ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het uitvoeren van](site-recovery-capacity-planning-for-hyper-v-replication.md) het hulpprogramma voor capaciteitsplanning.
