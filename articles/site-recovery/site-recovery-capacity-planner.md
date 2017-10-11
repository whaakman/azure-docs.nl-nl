---
title: Schatting maken van de capaciteit van de replicatie in Azure | Microsoft Docs
description: Gebruik dit artikel om in te schatten capaciteit bij het repliceren met Azure Site Recovery
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
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: 134e17ebda3105be2b53d072fdef7aeda4a98bde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Plannen van capaciteit voor het beveiligen van virtuele machines en fysieke servers in de Azure Site Recovery

Het hulpprogramma Azure Site Recovery Capacity Planner helpt u om te achterhalen van uw capaciteitsvereisten bij het repliceren van Hyper-V-machines, virtuele VMware-machines en fysieke servers met Windows of Linux, met Azure Site Recovery.

De Site Recovery Capacity Planner gebruiken voor het analyseren van uw bronomgeving en werkbelastingen, schatting bandbreedte behoeften en bronnen van de server u voor de bronlocatie moet en de resources (virtuele machines en opslag, enzovoort), die u nodig hebt in de doellocatie.

U kunt het hulpprogramma uitvoeren in een aantal modi:

* **Snelle planning**: Voer het hulpprogramma in deze modus netwerk- en projecties ophalen op basis van een gemiddelde aantal virtuele machines, schijven, opslag en wijzigingssnelheid.
* **Gedetailleerde planning**: het hulpprogramma uitvoeren in deze modus en Geef details op van elke werkbelasting op het niveau van de virtuele machine. VM-compatibiliteit analyseren en netwerk- en projecties ophalen.

## <a name="before-you-start"></a>Voordat u begint


1. Gegevens verzamelen over uw omgeving, inclusief virtuele machines, schijven per virtuele machine, opslag per schijf.
2. De snelheid van uw dagelijkse wijzigen (verloop) voor gerepliceerde gegevens identificeren. Om dit te doen:

   * Als u Hyper-V-machines repliceert, downloaden de [Hyper-V-hulpprogramma voor capaciteitsplanning](https://www.microsoft.com/download/details.aspx?id=39057) de wijzigingssnelheid ophalen. [Meer informatie](site-recovery-capacity-planning-for-hyper-v-replication.md) over dit hulpprogramma. U wordt aangeraden dat u dit hulpprogramma uitvoeren via een week om vast te leggen gemiddelden.
   * Als u virtuele VMware-machines repliceert, gebruikt u de [Azure Site Recovery implementatie Planner](./site-recovery-deployment-planner.md) om het verloop te achterhalen.
   * Als u fysieke servers repliceert, moet u handmatig een schatting maken.

## <a name="run-the-quick-planner"></a>De snelle Planner uitvoeren
1. Download en open de [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) hulpprogramma. U moet uitvoeren van macro's, dus selecteer inschakelen bewerken en inschakelen van inhoud wanneer u wordt gevraagd.
2. In **Selecteer een type planner** Selecteer **snelle Planner** in de keuzelijst.

   ![Aan de slag](./media/site-recovery-capacity-planner/getting-started.png)
3. In de **Capacity Planner** werkblad, voer de vereiste informatie. U moet alle velden omcirkeld in rood in de onderstaande schermafbeelding invullen.

   * In **selecteert u uw scenario**, kies **Hyper-V naar Azure** of **VMware of fysieke naar Azure**.
   * In **gemiddelde dagelijkse gegevens snelheid (%) wijzigen**, plaatsen in de informatie die u verzamelen met behulp van de [Hyper-V-hulpprogramma voor capaciteitsplanning](site-recovery-capacity-planning-for-hyper-v-replication.md) of de [Azure Site Recovery implementatie Planner](./site-recovery-deployment-planner.md).  
   * **Compressie** geldt alleen voor compressie worden aangeboden wanneer virtuele VMware-machines of fysieke servers repliceren naar Azure. We verwachten 30% of meer, maar u kunt de instelling zo nodig wijzigen. Voor het Hyper-V-machines repliceren naar Azure compressie, kunt u een apparaat van derden zoals Riverbed.
   * In **bewaren invoer**, opgeven hoe lang de replica's moeten worden bewaard. Als u VMware of fysieke servers repliceert, voer dan de waarde in dagen. Als u Hyper-V repliceert, geeft u de tijd in uren.
   * In **aantal uren in waarop de initiële replicatie voor de batch van virtuele machines moet worden voltooid** en **aantal virtuele machines per batch van de initiële replicatie**, u de instellingen die zijn gebruikt voor invoer vereisten voor initiële replicatie berekenen.  Wanneer Site Recovery wordt geïmplementeerd, moet de volledige set met initiële gegevens worden geüpload.

   ![Invoer](./media/site-recovery-capacity-planner/inputs.png)
4. Nadat u de waarden voor de bronomgeving hebt geplaatst, bevat de uitvoer weergegeven:

   * **Bandbreedte vereist is voor replicatie van verschillen** (MB/sec). Netwerkbandbreedte voor replicatie van verschillen wordt berekend op de gemiddelde dagelijkse wijzigingssnelheid van gegevens.
   * **Bandbreedte die is vereist voor de initiële replicatie** (MB/sec). Netwerkbandbreedte voor de initiële replicatie wordt berekend op de initiële replicatie waarden die u in plaatsen.
   * **Opslag vereist (in GB's)** is het totaal aantal Azure-opslag vereist.
   * **Totaal aantal IOPS op standaard storage-accounts** wordt berekend op basis van de grootte van 8 kB IOPS op de totale standaard storage-accounts.  Het getal wordt berekend op basis van de schijven voor de virtuele machines bron en dagelijkse gegevens snelheid wijzigen voor de snelle Planner. Het nummer is berekend op basis van het totale aantal VM's die zijn toegewezen aan de standaard Azure VM's voor de gedetailleerde Planner en gegevens wijzigen tarief op deze virtuele machines.
   * **Aantal accounts standaardopslag** geeft het totale aantal standaard storage-accounts die nodig zijn voor het beveiligen van de virtuele machines. Standard-opslagaccount kan tot 20.000 IOP's bevatten voor alle VM's in een standard-opslag en maximaal 500 IOP's per schijf wordt ondersteund.
   * **Het aantal blob-schijven die zijn vereist** geeft het aantal schijven die worden gemaakt op Azure-opslag.
   * **Aantal premium storage-accounts vereist** geeft het totale aantal premium storage-accounts die nodig zijn voor het beveiligen van de virtuele machines. Een bron-VM met een hoge IOPS (groter dan 20000) moet een premium storage-account. Een premium storage-account kan tot 80.000 IOP's bevatten.
   * **Totaal aantal IOPS op premium-opslag** wordt berekend op basis van de grootte van 256 kB IOPS op de totale premium storage-accounts.  Het getal wordt berekend op basis van de schijven voor de virtuele machines bron en dagelijkse gegevens snelheid wijzigen voor de snelle Planner. Het getal wordt berekend op basis van het totale aantal virtuele machines die zijn toegewezen aan een premium Azure VM (Active Directory en GS-serie) voor de gedetailleerde Planner en de gegevens wijzigen tarief op deze virtuele machines.
   * **Aantal vereiste configuratieservers** geeft aan hoeveel configuratieservers vereist voor de implementatie.
   * **Aantal vereiste aanvullende processen servers** geeft aan of extra processervers vereist, naast de processerver die wordt uitgevoerd op de configuratieserver standaard.
   * **Extra opslagruimte op de bron-100%** toont of extra opslagruimte op de bronlocatie vereist is.

   ![Uitvoer](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>De gedetailleerde Planner uitvoeren

1. Download en open de [Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) hulpprogramma. U moet uitvoeren van macro's, dus selecteer inschakelen bewerken en inschakelen van inhoud wanneer u wordt gevraagd.
2. In **Selecteer een type planner**, selecteer **gedetailleerde Planner** in de keuzelijst.

   ![Aan de slag](./media/site-recovery-capacity-planner/getting-started-2.png)
3. In de **werkbelasting kwalificatie** werkblad, voer de vereiste informatie. U moet de gemarkeerde velden invullen.

   * In **Processor-cores**, het totale aantal kernen op een bronserver opgeven.
   * In **toewijzing van geheugen in MB**, de grootte van het RAM-geheugen van een bronserver opgeven.
   * De **nummer van de NIC's**, geef het aantal netwerkadapters op een bronserver.
   * In **totale opslag (in GB)**, geef de totale grootte van de VM-opslag. Bijvoorbeeld, als de bronserver 3 schijven met 500 GB heeft, is grootte van de totale opslagruimte 1500 GB.
   * In **aantal gekoppelde schijven**, het totale aantal schijven van een bronserver opgeven.
   * In **schijf gebruik van capaciteit**, geef het gemiddelde gebruik.
   * In **dagelijks snelheid (%) wijzigen**, geef de dagelijkse hoeveelheden frequentie van een bronserver wijzigen.
   * In **toewijzing Azure grootte**, geef de Azure VM-grootte die u wilt toewijzen. Als u niet dat dit handmatig doen wilt, klikt u op **Compute IaaS VM's**. Als u een handmatige instelling invoer en klik vervolgens op Compute IaaS VM's, kan de handmatige instelling worden overschreven omdat het proces compute automatisch de beste overeenkomst op Azure VM-grootte identificeert.

   ![Kwalificatie werkbelasting](./media/site-recovery-capacity-planner/workload-qualification.png)
4. Als u op **Compute IaaS VM's** Hier ziet u hoe het werkt:

   * Valideert de verplichte invoeren.
   * Berekent de IOPS en wordt aangeraden de beste Azure VM aize overeenkomst voor elke virtuele machines die in aanmerking komen voor replicatie naar Azure. Als een correcte grootte van virtuele machine van Azure kan worden gedetecteerd met dat een fout is uitgegeven. Bijvoorbeeld als het aantal schijven gekoppeld 65, een fout gegenereerd omdat de hoogste Azure VM-grootte 64 is.
   * Stelt een opslagaccount die kan worden gebruikt voor een virtuele machine in Azure.
   * Berekent het totale aantal standaard storage-accounts en premium-opslagaccounts die zijn vereist voor de werkbelasting. Schuif omlaag naar de weergave van het type Azure-opslag en het opslagaccount die kan worden gebruikt voor een bronserver.
   * Is voltooid en de rest van de tabel op basis van het vereiste opslagtype (standard of premium) toegewezen voor een virtuele machine en het aantal gekoppelde schijven gesorteerd. Voor alle virtuele machines die voldoen aan de vereisten voor Azure, de kolom **Is VM gekwalificeerd?** toont **Ja**. Als een virtuele machine kan niet een back-up naar Azure, wordt er een fout weergegeven.

AA AE worden uitgevoerd, en kolommen bevatten informatie voor elke virtuele machine.

![Kwalificatie werkbelasting](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Voorbeeld
Als u bijvoorbeeld voor zes VM's met de waarden in de tabel het hulpprogramma wordt berekend en het meest geschikt voor de virtuele machine van Azure en de vereisten voor Azure-opslag moet worden toegewezen.

![Kwalificatie werkbelasting](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* Let op het volgende in het voorbeeld van uitvoer:

  * De eerste kolom is een kolom validatie voor de virtuele machines, schijven en verloop.
  * Twee standaard storage-accounts en een premium-opslagaccount nodig zijn voor vijf virtuele machines.
  * VM3 niet in aanmerking voor beveiliging, omdat een of meer schijven meer dan 1 TB zijn.
  * VM1 en VM2 van kunt het eerste standard-opslag-account gebruiken
  * VM4 kunt de tweede standard-opslagaccount gebruiken.
  * VM5 en VM6 een premium-opslagaccount nodig en kunnen beide één account gebruiken.

    > [!NOTE]
    > IOPS op standard en premium storage worden berekend op het niveau van de virtuele machine en niet op het niveau van de schijf. Een standaard virtuele machine kan maximaal 500 IOP's per schijf verwerken. Als IOPS aan voor een schijf groter dan 500 zijn, moet u premium-opslag. Echter als IOPS aan voor een schijf meer dan 500 zijn, maar IOP's voor het totale aantal VM-schijven zijn binnen de grenzen voor ondersteuning voor virtuele machine van Azure standard (VM-grootte, het aantal schijven, het aantal adapters, CPU, geheugen), hervat klikt u vervolgens de planner een standaard virtuele machine en niet in de Active Directory of GS-serie. U moet handmatig bijwerken van de cel van de Azure-grootte toewijzing juiste DS- of GS-serie VM.


Nadat alle gegevens gemaakt zijn, klikt u op **gegevens verzenden naar het hulpprogramma planner** openen de **Capacity Planner** werkbelastingen om weer te geven of ze in aanmerking komen voor beveiliging of niet zijn worden gemarkeerd.

### <a name="submit-data-in-the-capacity-planner"></a>Verzenden van gegevens in de Capacity Planner
1. Wanneer u opent de **Capacity Planner** werkblad dit ingevuld op basis van de instellingen die u hebt opgegeven. Het woord 'Werkbelasting' wordt weergegeven in de **Infra invoer bron** cel aangeven dat de invoer is de **werkbelasting kwalificatie** werkblad.
2. Als u wijzigingen aanbrengen wilt, moet u wijzigt de **werkbelasting kwalificatie** werkblad en klik op **gegevens verzenden naar het hulpprogramma planner** opnieuw.  

   ![Capaciteit Planner](./media/site-recovery-capacity-planner/capacity-planner.png)
