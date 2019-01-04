---
title: Capaciteit plannen voor Hyper-V-noodherstel met Azure Site Recovery | Microsoft Docs
description: Gebruik dit artikel kunt u schatten capaciteit bij het instellen van herstel na noodgeval met de Azure Site Recovery-service.
author: rayne-wiselman
manager: carmonm
services: site-recovery
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/27/2018
ms.author: raynew
ms.openlocfilehash: eeadfd6a57ff8a26f3f124e2a807fcd66e77b85f
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976712"
---
# <a name="plan-capacity-for-hyper-v-vm-disaster-recovery"></a>Capaciteit plannen voor noodherstel van Hyper-V-VM 

Een nieuwe, verbeterde versie van [Azure Site Recovery Deployment Planner voor Hyper-V naar Azure-implementatie](site-recovery-hyper-v-deployment-planner.md) is nu beschikbaar. Dit vervangt de oude hulpprogramma. Het nieuwe hulpprogramma gebruiken voor het plannen van uw implementatie.
Het hulpprogramma biedt de volgende richtlijnen:

* Beoordeling van geschiktheid van virtuele machine, op basis van het aantal schijven, schijfgrootte, IOPS, verloop en een aantal VM-eigenschappen
* Netwerkbandbreedte versus RPO-evaluatie
* Vereisten voor Azure-infrastructuur
* On-premises infrastructuurvereisten
* Initiële replicatie via batchverwerking richtlijnen
* Geschatte kosten van herstel na noodgevallen naar Azure


Azure Site Recovery Capacity Planner kunt u uw capaciteitsvereisten bepalen wanneer u Hyper-V-machines met Azure Site Recovery repliceert.

Gebruik Site Recovery Capacity Planner voor het analyseren van uw bronomgeving en werkbelastingen. Hiermee kunt u schatten bandbreedtebehoeften, de server-bronnen die u nodig hebt voor de bronlocatie, en de resources (zoals virtuele machines en opslag) moet u op de doellocatie.

U kunt het hulpprogramma uitvoeren in twee modi:

* **Planning voor snelle**: Biedt de netwerk- en projecties op basis van een gemiddelde aantal virtuele machines, schijven, opslag en de wijzigingssnelheid.
* **Gedetailleerde planning**: Geeft de details van elke werkbelasting op het niveau van de virtuele machine. VM-compatibiliteit analyseren en netwerk- en projecties ophalen.

## <a name="before-you-start"></a>Voordat u begint

* Gegevens verzamelen over uw omgeving, met inbegrip van virtuele machines, schijven per virtuele machine, opslag per schijf.
* Identificeer uw dagtarief wijzigen (verloop) voor gerepliceerde gegevens. Download de [Hyper-V-hulpprogramma voor capaciteitsplanning](https://www.microsoft.com/download/details.aspx?id=39057) om op te halen van de snelheid waarmee u wijzigt. [Meer informatie](site-recovery-capacity-planning-for-hyper-v-replication.md) over dit hulpprogramma. Het is raadzaam dat u dit hulpprogramma uitvoeren in een week om vast te leggen gemiddelden.


## <a name="run-the-quick-planner"></a>De snelle Planner uitvoeren
1. Downloaden en openen [Site Recovery Capacity Planner](https://aka.ms/asr-capacity-planner-excel). U moet uitvoeren van macro's. Wanneer u wordt gevraagd, moet u selecties inschakelen bewerken en inhoud.

2. In de **een planner-type selecteren** keuzelijst, selecteer **snelle Planner**.

   ![Aan de slag](./media/site-recovery-capacity-planner/getting-started.png)

3. Op de **Capacity Planner** werkblad, voer de vereiste informatie. Vul alle velden in de volgende schermafbeelding met rood omcirkeld:

   a. In **selecteert u uw scenario**, kiest u **Hyper-V naar Azure** of **VMware/fysieke machines naar Azure**.

   b. In **gemiddelde dagelijkse gegevenswijzigingssnelheid (%)**, voert u de gegevens die u via verzamelt de [Hyper-V-hulpprogramma voor capaciteitsplanning](site-recovery-capacity-planning-for-hyper-v-replication.md) of [Site Recovery Deployment Planner](./site-recovery-deployment-planner.md).

   c. De **compressie** instelling wordt niet gebruikt wanneer u Hyper-V-machines naar Azure repliceren. Voor compressie, gebruikt u een apparaat van derden, bijvoorbeeld middels Riverbed.

   d. In **bewaartermijn in dagen**, opgeven in dagen hoelang de behouden replica's.

   e. In **aantal uren op waarvoor de initiële replicatie voor de batch virtuele machines moet worden voltooid** en **aantal virtuele machines per batch van de initiële replicatie**, voer de instellingen die worden gebruikt om te berekenen vereisten voor initiële replicatie. Wanneer Site Recovery wordt geïmplementeerd, wordt de volledige initiële gegevensset is geüpload.

   ![Invoer](./media/site-recovery-capacity-planner/inputs.png)

4. Nadat u de waarden voor de bronomgeving hebt ingevoerd, bevat de uitvoer weergegeven:

   * **Bandbreedte die is vereist voor replicatie van verschillen (in Megabits per seconde)**: Netwerkbandbreedte voor replicatie van verschillen wordt voor de gemiddelde dagelijkse veranderingssnelheid van gegevens berekend.
   * **Bandbreedte die is vereist voor de initiële replicatie (in Megabits per seconde)**: Netwerkbandbreedte voor de initiële replicatie wordt berekend op basis van de initiële replicatie waarden die u invoert.
   * **Opslag die is vereist (in GB)**: De totale Azure opslagruimte vereist.
   * **Totaal aantal IOP's op Standard-opslag**: Het getal wordt berekend op basis van de grootte van 8 kB IOPS op de totale standard storage-accounts. Het getal wordt berekend op basis van de schijven voor de virtuele machine bron en de dagelijkse gegevenswijzigingssnelheid voor de snelle Planner. Voor de gedetailleerde Planner, het getal wordt berekend op basis van het totale aantal virtuele machines die zijn toegewezen aan de standaard Azure-VM's en de gegevenswijzigingssnelheid op deze virtuele machines.
   * **Aantal vereiste accounts voor standaardopslag**: Het totale aantal standard storage-accounts nodig voor het beveiligen van de virtuele machines. Een standard storage-account kan maximaal 20.000 IOP's bevatten voor alle virtuele machines in de standard-opslag. Maximaal 500 IOP's wordt per schijf ondersteund.
   * **Aantal vereiste schijven voor Blob**: Het aantal schijven die zijn gemaakt op Azure-opslag.
   * **Het aantal vereiste accounts voor premium**: Het totale aantal premium storage-accounts nodig voor het beveiligen van de virtuele machines. Een bron-VM met hoge IOPS (groter dan 20.000) moet een premium storage-account. Premium storage-account kan maximaal 80.000 IOP's bevatten.
   * **Totaal aantal IOP's op Premium Storage**: Het getal wordt berekend op basis van de grootte van 256 kB IOPS op de totale premium storage-accounts. Het getal wordt berekend op basis van de schijven voor de virtuele machine bron en de dagelijkse gegevenswijzigingssnelheid voor de snelle Planner. Het getal wordt berekend op basis van het totale aantal virtuele machines die zijn toegewezen aan premium Azure-VM's (DS- en GS-serie) voor de gedetailleerde Planner en veranderingssnelheid van de gegevens op deze virtuele machines.
   * **Het aantal configuratieservers vereist**: Geeft het aantal configuratieservers zijn vereist voor de implementatie.
   * **Aantal extra processervers vereist**: Geeft aan of extra processervers zijn vereist, naast de processerver die standaard op de configuratieserver wordt uitgevoerd.
   * **Extra opslag van 100% op de broncomputer**: Geeft aan of extra opslag in de bronlocatie vereist is.

      ![Uitvoer](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>De gedetailleerde Planner uitvoeren

1. Downloaden en openen [Site Recovery Capacity Planner](https://aka.ms/asr-capacity-planner-excel). U moet uitvoeren van macro's. Wanneer u wordt gevraagd, moet u selecties inschakelen bewerken en inhoud.

2. In **een planner-type selecteren**, selecteer **gedetailleerde Planner** uit de lijst.

   ![Introductiehandleiding](./media/site-recovery-capacity-planner/getting-started-2.png)

3. Op de **werkbelasting kwalificatie** werkblad, voer de vereiste informatie. U moet de gemarkeerde velden invullen.

   a. In **processorkernen**, het totale aantal kernen op een bronserver opgeven.

   b. In **toewijzing van geheugen (in MB/s)**, de grootte van het RAM-geheugen van een bronserver opgeven.

   c. In **nummer van de NIC's**, geef het aantal netwerkadapters op een bronserver.

   d. In **totale opslag (in GB)**, geef de totale grootte van de VM-opslag. Bijvoorbeeld, als de bronserver drie schijven met 500 GB heeft, is de grootte van de totale opslag 1500 GB.

   e. In **aantal schijven die zijn gekoppeld**, geeft u het totale aantal schijven van een bronserver.

   f. In **schijf verbruik (%) van de opslagcapaciteit**, geef het gemiddelde gebruik.

   g. In **dagelijkse gegevenswijzigingssnelheid (%)**, veranderingssnelheid van de dagelijkse gegevens van een bronserver opgeven.

   h. In **toewijzing van Azure VM-grootte**, geef de Azure-VM-grootte die u wilt toewijzen. Als u niet dat dit handmatig doen wilt, selecteert u **IaaS-VM's Compute**. Als u een handmatige instelling invoer en selecteer vervolgens **IaaS-VM's Compute**, de instelling van de handmatige, kan worden overschreven. Het proces compute identificeert automatisch de beste overeenkomst op Azure VM-grootte.

   ![Werkblad voor werkbelasting-kwalificatie](./media/site-recovery-capacity-planner/workload-qualification.png)

4. Als u selecteert **IaaS-VM's Compute**, dit is wat het doet:

   * Valideert de verplichte invoer.
   * Berekent de IOPS en stelt het meest geschikt is Azure VM-grootte voor elke virtuele machine die in aanmerking komt voor replicatie naar Azure. Als een geschikte grootte die virtuele Azure-machine kan niet worden gedetecteerd, wordt er een fout weergegeven. Als het aantal schijven die zijn gekoppeld 65 is een fout wordt weergegeven omdat de grootte van de hoogste voor een Azure-VM 64 is is.
   * Stelt een storage-account dat kan worden gebruikt voor een Azure-VM.
   * Berekent het totale aantal standard storage-accounts en premium storage-accounts vereist voor de werkbelasting. Schuif omlaag om het type Azure-opslag en het opslagaccount dat kan worden gebruikt voor een bronserver weer te geven.
   * Is voltooid en sorteert de rest van de tabel op basis van het vereiste opslagtype (standard of premium) toegewezen voor een virtuele machine en het aantal schijven die zijn gekoppeld. Voor alle virtuele machines die voldoen aan de vereisten voor Azure, de kolom **Is VM gekwalificeerde?** bevat **Ja**. Als een virtuele machine kan niet een back-up naar Azure, wordt er een fout weergegeven.

Kolommen AA AE worden uitgevoerd en geef informatie voor elke virtuele machine.

![Uitvoerkolommen AA AE](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Voorbeeld
Als voorbeeld voor zes VM's met de waarden in de tabel, het hulpprogramma berekent en wijst het meest geschikt voor de virtuele machine van Azure en de vereisten voor Azure storage.

![Toewijzingen voor werkbelasting-kwalificatie](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Let op het volgende in de voorbeelduitvoer:

  * De eerste kolom is een kolom validatie voor de virtuele machines, schijven en gegevensverloop.
  * Twee standard storage-accounts en een premium storage-account nodig zijn voor vijf virtuele machines.
  * VM3 niet in aanmerking voor beveiliging, omdat een of meer schijven meer dan 1 TB zijn.
  * VM1 en VM2 kunt het eerste standard storage-account gebruiken
  * VM4 kunt de tweede standard storage-account gebruiken.
  * VM5 en VM6 moet een premium storage-account, en beide één account kunnen gebruiken.

    > [!NOTE]
    > IOP's op standard en premium storage worden berekend op het niveau van de virtuele machine en niet op het schijfniveau van de. Een standaard virtuele machine kan maximaal 500 IOP's per schijf worden verwerkt. Als de IOPS voor een schijf groter zijn dan 500, moet u premium-opslag. Als IOPS aan voor een schijf is meer dan 500 IOPS aan voor de totale VM-schijven zijn binnen de grenzen van ondersteuning voor standaard Azure-VM, kiest de planner een standaard virtuele machine en niet de DS- of GS-serie. (De virtuele machine van Azure-limieten zijn VM-grootte, het aantal schijven, het aantal adapters, CPU en geheugen). U moet handmatig bijwerken van de toewijzing van Azure-grootte cel met de juiste DS of GS-serie VM.


Nadat u alle informatie die is ingevoerd, selecteert u **gegevens verzenden naar de tool voor implementatieplanning** Capacity Planner openen. Workloads zijn gemarkeerd als u wilt weergeven of ze in aanmerking komen voor beveiliging.

### <a name="submit-data-in-capacity-planner"></a>Verzenden van gegevens in de Capacity Planner
1. Wanneer u opent de **Capacity Planner** werkblad, wordt dit ingevuld op basis van de instellingen die u hebt opgegeven. Het woord 'Werkbelasting' wordt weergegeven in de **Infra invoer bron** cel om weer te geven dat de invoer is de **werkbelasting kwalificatie** werkblad.

2. Als u wijzigingen aanbrengen wilt, moet u wijzigen de **werkbelasting kwalificatie** werkblad. Selecteer vervolgens **gegevens verzenden naar de tool voor implementatieplanning** opnieuw.

   ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)

## <a name="next-steps"></a>Volgende stappen
[Leer hoe u uitvoert](site-recovery-capacity-planning-for-hyper-v-replication.md) het hulpprogramma voor capaciteitsplanning.
