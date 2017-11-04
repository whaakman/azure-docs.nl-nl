---
title: StorSimple fileshare DR met Azure Site Recovery automatiseren | Microsoft Docs
description: Beschrijft de stappen en best practices voor het maken van een noodherstel voor bestandsshares die worden gehost op Microsoft Azure StorSimple-opslag.
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 23049a2c-055e-4d0e-b8f5-af2a87ecf53f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2017
ms.author: vidarmsft
ms.openlocfilehash: e60cc83f49f9e0d0f878d7f49333f1be34ce54a6
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatisch herstel na noodgevallen oplossing met Azure Site Recovery voor bestandsshares die worden gehost op StorSimple
## <a name="overview"></a>Overzicht
Microsoft Azure StorSimple is een hybride cloud-opslagoplossing die zijn gericht op de complexiteit van niet-gestructureerde gegevens die betrekking hebben op bestandsshares. StorSimple maakt gebruik van cloud-opslag als een uitbreiding van de on-premises oplossing en automatisch lagen gegevens voor lokale opslag en cloud-opslag. Gegevensbescherming, geïntegreerd met lokale en cloud worden opgeslagen, hoeft u voor een weids opslaginfrastructuur.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) is een Azure-service disaster recovery (DR biedt) door te organiseren replicatie, failovers en herstel van virtuele machines. Azure Site Recovery biedt ondersteuning voor een aantal replicatietechnologieën consistent repliceren, beveiligen en naadloos via virtuele machines en toepassingen naar persoonlijke/openbare of gehoste clouds failover.

Met Azure Site Recovery, replicatie voor virtuele machines en StorSimple snapshot cloudfuncties, kunt u de server-omgeving van het volledige bestand beveiligen. In het geval van een onderbreking, kunt u een enkele klik op te zetten van uw bestandsshares online in Azure in een paar minuten.

Dit document wordt gedetailleerd uitgelegd hoe u kunt een noodherstel voor uw bestandsshares die worden gehost op een StorSimple-opslag maken en uitvoeren van geplande, niet-geplande en testfailovers met behulp van een herstelplan met één klik. In wezen zien hoe u het herstelplan kunt wijzigen in uw Azure Site Recovery-kluis StorSimple failovers inschakelen tijdens de scenario's voor noodherstel. Bovendien beschrijft ondersteunde configuraties en vereisten. Dit document wordt ervan uitgegaan dat u bekend met de basisprincipes van Azure Site Recovery en StorSimple-architecturen bent.

## <a name="supported-azure-site-recovery-deployment-options"></a>Ondersteunde opties voor Azure Site Recovery-implementatie
Klanten kunnen bestandsservers implementeren als fysieke servers of virtuele machines (VM's) op de Hyper-V- of VMware en vervolgens bestandsshares te maken van volumes oppervlaktevariaties buiten het StorSimple-opslag. Azure Site Recovery kan fysieke en virtuele implementaties naar een secundaire site of naar Azure te beveiligen. Dit document bevat informatie over de details van een oplossing voor DR met Azure als de site recovery voor een bestandsserver met de die virtuele machine wordt gehost op Hyper-V en bestandsshares op het StorSimple-opslag. Andere scenario's waarin de virtuele machine van de bestandsserver zich op een VMware-VM of een fysieke computer kunnen op dezelfde manier worden geïmplementeerd.

## <a name="prerequisites"></a>Vereisten
Implementatie van een éénkliks-noodherstel die gebruikmaakt van Azure Site Recovery voor bestandsshares die worden gehost op een StorSimple-opslag heeft de volgende vereisten:

* De lokale virtuele machine wordt gehost op Hyper-V- of VMware- of een fysieke computer bestand van Windows Server 2012 R2-server
* StorSimple opslag on-premises apparaten die zijn geregistreerd bij Azure StorSimple manager
* StorSimple Cloud toestel gemaakt in Azure StorSimple manager. Het apparaat kan worden bewaard in een status afsluiten.
* Bestandsshares die worden gehost op de volumes die zijn geconfigureerd op het StorSimple-opslagapparaat
* [Azure Site Recovery services-kluis](../site-recovery/site-recovery-vmm-to-vmm.md) gemaakt in een Microsoft Azure-abonnement

Bovendien als Azure uw site recovery is, voert u de [Azure virtuele Machine Readiness Assessment tool](http://azure.microsoft.com/downloads/vm-readiness-assessment/) services op virtuele machines om ervoor te zorgen dat ze compatibel met Azure VM's en Azure Site Recovery zijn.

Om te voorkomen dat problemen (dit kunnen leiden tot hogere kosten), zorg ervoor dat u maakt om uw StorSimple Cloud toestel, de automation-account en de opslag latentie (s) in dezelfde regio.

## <a name="enable-dr-for-storsimple-file-shares"></a>DR inschakelen voor StorSimple-bestandsshares
Elk onderdeel van de on-premises omgeving moet worden beveiligd, zodat de replicatie is voltooid en herstel. Deze sectie wordt beschreven hoe:

* Instellen van de Active Directory en DNS-replicatie (optioneel)
* Gebruik Azure Site Recovery-beveiliging van de bestandsserver VM inschakelen
* Schakel de beveiliging van StorSimple-volumes
* Het netwerk configureren

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Instellen van de Active Directory en DNS-replicatie (optioneel)
Als u beveiligen van de computers met Active Directory en DNS wilt, zodat ze beschikbaar op de DR-site zijn, moet u expliciet beveiligen (zodat de bestandsservers na een failover met verificatie toegankelijk zijn). Er zijn twee aanbevolen opties op basis van de complexiteit van de klant on-premises omgeving.

#### <a name="option-1"></a>Optie 1
Als de klant een klein aantal toepassingen heeft, één domeincontroller voor het gehele on-premises site, en zal worden failover van de hele site en vervolgens wordt u Azure Site Recovery-replicatie aangeraden voor het repliceren van de domain controller machine naar een secundaire site (dit is van toepassing voor zowel de site-naar-site en de site naar Azure).

#### <a name="option-2"></a>Optie 2
Als de klant heeft een groot aantal toepassingen, een Active Directory-forest wordt uitgevoerd en failover wordt uitgevoerd een paar toepassingen op een tijdstip, wordt aangeraden een extra domeincontroller op de DR-site instellen (een secundaire site of in Azure).

Raadpleeg [automatisch DR-oplossing voor Active Directory en DNS met Azure Site Recovery](../site-recovery/site-recovery-active-directory.md) voor instructies wanneer het beschikbaar maken van een domeincontroller op de DR-site. Voor de rest van dit document, nemen we u aan dat een domeincontroller beschikbaar is op de DR-site.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Gebruik Azure Site Recovery-beveiliging van de bestandsserver VM inschakelen
Deze stap is vereist dat u de on-premises bestand server-omgeving voorbereiden, maken en voorbereiden van een Azure Site Recovery-kluis en schakel de bestandsbeveiliging van de virtuele machine.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>De on-premises bestand server-omgeving voorbereiden
1. Stel de **Gebruikersaccountbeheer** naar **nooit waarschuwen**. Dit is vereist zodat u Azure automatiseringsscripts gebruiken kunt voor het verbinding maken met de iSCSI-doelen na een failover door Azure Site Recovery.

   1. Druk op de Windows-toets + Q en zoek naar **UAC**.
   2. Selecteer **instellingen voor Gebruikersaccountbeheer wijzigen**.
   3. Sleep de balk onderaan naar **nooit waarschuwen**.
   4. Klik op **OK** en selecteer vervolgens **Ja** wanneer u wordt gevraagd.

      ![Instellingen voor Gebruikersaccountbeheer](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png)
2. Installeer de VM-Agent op elk van de virtuele machines van de bestandsserver. Dit is vereist zodat u Azure automatiseringsscripts op de mislukte via virtuele machines uitvoeren kunt.

   1. [De agent downloaden](http://aka.ms/vmagentwin) naar `C:\\Users\\<username>\\Downloads`.
   2. Open Windows PowerShell in de beheerdersmodus (als Administrator uitvoeren) en voer vervolgens de volgende opdracht om te navigeren naar de downloadlocatie:

      `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

      > [!NOTE]
      > Naam van het bestand kan wijzigen, afhankelijk van de versie.
      >
      >
3. Klik op **Volgende**.
4. Accepteer de **voorwaarden van de overeenkomst** en klik vervolgens op **volgende**.
5. Klik op **Voltooien**.
6. Bestandsshares maken met volumes oppervlaktevariaties buiten het StorSimple-opslag. Zie voor meer informatie [de StorSimple Manager-service gebruiken voor het beheren van volumes](storsimple-manage-volumes.md).

   1. Druk op de Windows-toets + Q op uw lokale virtuele machines, en zoek naar **iSCSI**.
   2. Selecteer **iSCSI-initiator**.
   3. Selecteer de **configuratie** tabblad en de initiatornaam kopiëren.
   4. Meld u aan bij [Azure Portal](https://portal.azure.com/).
   5. Selecteer de **StorSimple** tabblad en selecteer vervolgens de StorSimple Manager-Service die het fysieke apparaat bevat.
   6. Maken van volume container (s) en maak vervolgens een of meer volumes. (Deze volumes zijn voor het bestand share (s) op de bestandsserver VM's). De initiatornaam kopiëren en geef een passende naam voor de Access Control Records bij het maken van de volumes.
   7. Selecteer de **configureren** tabblad omlaag door het IP-adres van het apparaat.
   8. Op uw lokale virtuele machines, gaat u naar de **iSCSI-initiator** opnieuw en voer het IP-adres in de sectie snel verbinding maken. Klik op **snelle verbinding** (het apparaat moet nu worden verbonden).
   9. Open de Azure portal en selecteer de **Volumes en apparaten** tabblad. Klik op **automatisch configureren**. Het volume dat u hebt gemaakt, worden weergegeven.
   10. Selecteer in de portal de **apparaten** tabblad en selecteer vervolgens **een nieuw virtueelapparaat maken.** (Dit virtuele apparaat wordt gebruikt als er een failover optreedt). Deze nieuwe virtuele apparaat kan worden bewaard in de status offline heeft om te voorkomen dat extra kosten. Als u wilt het virtuele apparaat offline, gaat u naar de **virtuele Machines** sectie in de Portal en sluit deze af.
   11. Ga terug naar de lokale virtuele machines en open Schijfbeheer (druk op de Windows-toets + X en selecteer **Schijfbeheer**).
   12. Hier ziet u een aantal extra schijven (al naar gelang het aantal volumes die u hebt gemaakt). Met de rechtermuisknop op de eerste afbeelding, selecteer **schijf initialiseren**, en selecteer **OK**. Met de rechtermuisknop op de **Unallocated** sectie **Nieuw eenvoudig Volume**, een stationsletter toewijzen en voltooi de wizard.
   13. Herhaal stap l voor alle schijven. Nu ziet u alle schijven op **deze PC** in Windows Verkenner.
   14. De functie bestands- en opslagservices gebruiken om bestandsshares te maken op deze volumes.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Maken en voorbereiden van een Azure Site Recovery-kluis
Raadpleeg de [documentatie van Azure Site Recovery](../site-recovery/site-recovery-hyper-v-site-to-azure.md) aan de slag met Azure Site Recovery voordat u de bestandsserver VM beveiligt.

#### <a name="to-enable-protection"></a>Beveiliging in te schakelen
1. De iSCSI-doel(en) verbreken van de lokale virtuele machines die u wilt beveiligen met Azure Site Recovery:

   1. Druk op Windows-toets + Q en zoek naar **iSCSI**.
   2. Selecteer **instellen van iSCSI-initiator**.
   3. Verbreek de verbinding met de StorSimple-apparaat die u eerder hebt gekoppeld. U kunt ook de server uitschakelen voor een paar minuten bij het inschakelen van beveiliging.

   > [!NOTE]
   > Hierdoor wordt de bestandsshares tijdelijk niet beschikbaar.
   >
   >
2. [Schakel de beveiliging van de virtuele machine](../site-recovery/site-recovery-hyper-v-site-to-azure.md) van de bestandsserver VM vanuit de Azure Site Recovery-portal.
3. Wanneer de initiële synchronisatie begint, kunt u het doel opnieuw. Ga naar de iSCSI-initiator Selecteer het StorSimple-apparaat en klikt u op **Connect**.
4. Wanneer de synchronisatie is voltooid en de status van de virtuele machine is **beveiligde**, selecteert u de virtuele machine, selecteert u de **configureren** tabblad en dienovereenkomstig bijwerken van het netwerk van de virtuele machine (dit is het netwerk dat failover VM('s) een deel van is). Als het netwerk niet wordt weergegeven, betekent dit dat de synchronisatie is nog steeds gebeurt.

### <a name="enable-protection-of-storsimple-volumes"></a>Schakel de beveiliging van StorSimple-volumes
Als u niet hebt geselecteerd de **een standaardback-up voor dit volume inschakelen** optie voor de StorSimple-volumes, gaat u naar **back-upbeleid** in de StorSimple Manager service en een geschikte back-upbeleid voor alle volumes maken. U wordt aangeraden dat u de frequentie van back-ups instellen op het beoogde herstelpunt (RPO) die u zou willen zien voor de toepassing.

### <a name="configure-the-network"></a>Het netwerk configureren
Voor de bestandsserver VM netwerkinstellingen configureren in Azure Site Recovery zodat de VM-netwerken zijn gekoppeld aan het juiste DR-netwerk na een failover.

Kunt u de virtuele machine in de **gerepliceerde items** tabblad de netwerkinstellingen configureren zoals wordt weergegeven in de volgende afbeelding.

![Berekening en netwerk](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Een herstelplan maken
U kunt een herstelplan maken in ASR voor het automatiseren van failover-bestandsshares. Als een onderbreking van de optreedt, kunt u de bestandsshares brengt over een paar minuten met één klik. Om deze automatisering, moet u een Azure automation-account.

#### <a name="to-create-an-automation-account"></a>Een Automation-account maken
1. Ga naar de Azure portal &gt; **Automation** sectie.
2. Klik op **+ toevoegen** knop klikt, wordt hieronder blade geopend.

   ![Automation-account toevoegen](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)

   * Geef een naam - Voer een nieuw automatiseringsaccount
   * Abonnement - abonnement kiezen
   * Resource group - nieuwe/Kies bestaande resourcegroep maken
   * Locatie - locatie kiezen, bewaart u deze in de dezelfde geo/de regio waarin het StorSimple Cloud toestel en Storage-Accounts zijn gemaakt.
   * Maken van Azure uitvoeren als-account - selecteer **Ja** optie.

3. Ga naar het Automation-account, klikt u op **Runbooks** &gt; **bladeren galerie** alle vereiste runbooks importeren in het automation-account.
4. De volgende runbooks toevoegen door te zoeken **herstel na noodgevallen** tag op in de galerie:

   * Opruimen van de StorSimple-volumes na Test Failover (TFO)
   * Failover StorSimple volumecontainers
   * Koppelen van volumes op de StorSimple-apparaat na een failover
   * Verwijderen van extensie voor aangepaste scripts in Azure VM
   * Virtueel StorSimple-apparaat starten

     ![Bladeren-galerie](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)

5. Alle scripts publiceren door het selecteren van het runbook in het automation-account en klikt u op **bewerken** &gt; **publiceren** en vervolgens **Ja** in het bevestigingsbericht. Na deze stap wordt de **Runbooks** tabblad wordt als volgt uitzien:

    ![Runbooks](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)

6. Klik in het automation-account op **variabelen** &gt; **toevoegen van een variabele** en voeg de volgende variabelen. U kunt kiezen voor het coderen van deze elementen. Deze variabelen zijn specifieke herstelplan. Als het herstel van uw plan, die in de volgende stap maakt u de naam is TestPlan, en vervolgens de variabelen moeten worden TestPlan-StorSimRegKey, TestPlan-AzureSubscriptionName, enzovoort.

   * **BaseUrl**: de Resource Manager-url voor de Azure-cloud. Met behulp van ophalen **Get-AzureRmEnvironment | Naam van de Select-Object, ResourceManagerUrl** cmdlet.
   * *RecoveryPlanName***- ResourceGroupName**: de Resource Manager-groep met de StorSimple-resource.
   * *RecoveryPlanName***- ManagerName**: het StorSimple-resource met de StorSimple-apparaat.
   * *RecoveryPlanName***- DeviceName**: het StorSimple-apparaat met failover.
   * *RecoveryPlanName***- DeviceIpAddress**: het IP-adres van het apparaat (dit vindt u in de **apparaten** tabblad onder sectie Apparaatbeheer StorSimple &gt; **Instellingen** &gt; **netwerk** &gt; **DNS-instellingen** groep).
   * *RecoveryPlanName***- VolumeContainers**: een door komma's gescheiden reeks volumecontainers aanwezig is op het apparaat dat moet worden kan niet meer dan; bijvoorbeeld: volcon1, volcon2, volcon3.
   * *RecoveryPlanName***- TargetDeviceName**: het StorSimple Cloud apparaat waarop de containers zijn failover.
   * *RecoveryPlanName***- TargetDeviceIpAddress**: het IP-adres van het doelapparaat (dit vindt u in de **virtuele Machine** sectie &gt; **instellingen**  groep &gt; **Networking** tabblad).
   * *RecoveryPlanName***- StorageAccountName**: naam van het opslagaccount waarin het script (die worden uitgevoerd op de mislukte via VM heeft) wordt opgeslagen. Dit is opslagaccount met sommige ruimte voor het tijdelijk opslaan van het script.
   * *RecoveryPlanName***- StorageAccountKey**: de toegangssleutel voor het bovenstaande storage-account.
   * *RecoveryPlanName***- VMGUIDS**: bij het beveiligen van een virtuele machine Azure Site Recovery elke VM een unieke ID toegewezen die de details van de mislukte over VM geeft. Als u de VMGUID, selecteer de **Recovery Services** tabblad en klik op **beveiligd Item** &gt; **beveiligingsgroepen** &gt; **Machines** &gt; **eigenschappen**. Als u meerdere virtuele machines hebt, moet u vervolgens de GUID's toevoegen als een door komma's gescheiden tekenreeks.

    Bijvoorbeeld, als de naam van het herstelplan is fileServerpredayRP, en vervolgens uw **variabelen**, **verbindingen** en **certificaten** tabblad moet als volgt worden weergegeven nadat u hebt toegevoegd de activa.

    ![Assets](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

7. Upload StorSimple 8000 series runbookmodule in uw Automation-account. Gebruik de onderstaande stappen voor het toevoegen van een module:

   a. Opent u powershell, maak een nieuwe map en wijzig map in de map.
    
    ```
         mkdir C:\scripts\StorSimpleSDKTools
         cd C:\scripts\StorSimpleSDKTools
    ```
   b. Download nuget CLI onder dezelfde map in stap 1.
      Verschillende versies van nuget.exe zijn beschikbaar op [nuget downloadt](https://www.nuget.org/downloads). Elke downloadkoppeling direct verwijst naar een .exe-bestand dus moet u met de rechtermuisknop op en sla het bestand op uw computer in plaats van de browser uitgevoerd.

    ```
         wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```

   c. De afhankelijke SDK downloaden

    ```
         C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
         C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
         C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```

   d. Maak een Azure Automation-Runbook-Module voor het beheer van apparaten StorSimple 8000-serie. Gebruik de onderstaande opdrachten voor het maken van een Automation-module-zip-bestand.

    ```
         # set path variables
         $downloadDir = "C:\scripts\StorSimpleSDKTools"
         $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"

         #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
         mkdir "$moduleDir"

         copy "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
         copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
         copy "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
         copy "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
         copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
         copy "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir

         #Don't change the name of the Archive
         compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip
    ```

     e. Importeer het Azure Automation-module zip-bestand (Microsoft.Azure.Management.StorSimple8000Series.zip) in de bovenstaande stap gemaakt. Dit kan worden uitgevoerd door de Automation-Account te selecteren, klikt u op **Modules** onder gedeelde bronnen en klik vervolgens op **toevoegen van een module**.

    Nadat u de StorSimple 8000 serie-module importeren de **Modules** tabblad ziet er als volgt:

    ![Modules](./media/storsimple-disaster-recovery-using-azure-site-recovery/image12.png)

8. Ga naar de **Recovery Services** sectie en selecteer de Azure Site Recovery-kluis die u eerder hebt gemaakt.
9. Selecteer de **herstelplannen (Site Recovery)** optie van **beheren** groeperen en maakt u een nieuwe herstelplan als volgt:

   a.  Klik op **+ herstellen plan** knop klikt, wordt hieronder blade geopend.

      ![Herstelplan maken](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)

   b.  Voer een naam in recovery plan, kies bron, doel & Deployment model waarden.

   c.  Selecteer de virtuele machines uit de beveiligingsgroep die u wilt opnemen in het herstelplan en klik op **OK** knop.

   d.  Plan voor herstel die u eerder hebt gemaakt, klik op **aanpassen** knop om het herstel plan aanpassing weergave te openen.

   e.  Klik met de rechtermuisknop op **alle groepen afsluiten** en klik op **pre-actie toevoegen**.

   f.  Hiermee opent u actie blade invoegen, voer een naam, selecteer **primaire kant** optie in waar u de optie uitvoert, selecteert u Automation-account (u toegevoegd waarin de runbooks) en selecteer vervolgens de **Failover StorSimple-Volume Containers** runbook.

   g.  Klik met de rechtermuisknop op **groep 1: Start** en klik op **toevoegen beveiligde items** optie en selecteer de virtuele machines die moeten worden beschermd in het herstelplan en klik vervolgens **Ok** knop. Optioneel, als deze al is geselecteerd virtuele machines.

   h.  Klik met de rechtermuisknop op **groep 1: Start** en klik op **boeken actie** optie toegevoegd en voeg vervolgens de volgende scripts:

   * Runbook starten-StorSimple-virtueel-apparaat
   * Over StorSimple-volume containers runbook mislukt
   * Runbook koppelen volumes na failover
   * Verwijderen-aangepaste--scriptextensie runbook

   ik.  Een handmatige actie na de bovenstaande 4 scripts toevoegen in hetzelfde **groep 1: na stappen** sectie. Deze actie is het punt waarop u controleren kunt of alles correct werkt. Deze actie moet worden toegevoegd als onderdeel van de testfailover alleen (dus selecteer alleen de **Testfailover** selectievakje).

   j.  Na de handmatige actie toevoegen de **opschonen** script met dezelfde procedure die u voor de andere runbooks gebruikt. **Sla** het herstelplan.

    > [!NOTE]
    > Wanneer een testfailover wordt uitgevoerd, moet u controleren of alles bij de stap van de handmatige actie omdat de StorSimple-volumes die was op het doelapparaat is gekloond wordt verwijderd als onderdeel van het opruimen van de nadat de handmatige actie is voltooid.
    >

    ![Recoery plannen](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Een testfailover uitvoeren
Raadpleeg de [Active Directory-DR-oplossing](../site-recovery/site-recovery-active-directory.md) aanvullende handleiding voor overwegingen met betrekking tot specifieke naar Active Directory tijdens de testfailover. De installatie van de lokale niet helemaal worden beïnvloed wanneer de testfailover plaatsvindt. De StorSimple-volumes die zijn gekoppeld aan de lokale virtuele machine worden op het toestel StorSimple Cloud op Azure gekloond. Een virtuele machine voor testdoeleinden wordt opgeroepen in Azure en de gekloonde volumes zijn gekoppeld aan de virtuele machine.

#### <a name="to-perform-the-test-failover"></a>De testfailover uitvoeren
1. In de Azure portal, selecteer uw Site Recovery-kluis.
2. Klik op het herstelplan gemaakt voor de virtuele machine van de bestandsserver.
3. Klik op **Testfailover**.
4. Selecteer het Azure-netwerk waarmee virtuele Azure-machines worden verbonden nadat er failover plaatsvindt.

   ![Begin Failover](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
5. Klik op **OK** om te beginnen met de failover. U kunt de voortgang volgen door te klikken op de virtuele machine om de eigenschappen te openen of op de **Test failover-taak** in kluisnaam &gt; **taken** &gt; **Site Recovery-taken**.
6. Nadat de failover is voltooid, kunt u moet ook de gerepliceerde Azure Zie machine worden weergegeven in de Azure-portal &gt; **virtuele Machines**. U kunt uw controles uitvoeren.
7. Nadat de validaties klaar bent, klikt u op **validaties voltooid**. Hiermee verwijdert u het StorSimple-Volumes en afsluiten van het toestel StorSimple-Cloud.
8. Wanneer u bent klaar, klikt u op **testfailover opschonen** op het herstelplan. In de notities vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover. Hiermee verwijdert u de virtuele machine die zijn gemaakt tijdens de testfailover.

## <a name="perform-a-planned-failover"></a>Voer een geplande failover
   Tijdens een geplande failover bestandsserver de on-premises die virtuele machine correct wordt afgesloten en een cloud back-momentopname van de volumes op de StorSimple-apparaat wordt gemaakt. De StorSimple-volumes wordt een failover uitgevoerd met het virtuele apparaat, een replica-VM wordt opgeroepen in Azure en de volumes zijn gekoppeld aan de virtuele machine.

#### <a name="to-perform-a-planned-failover"></a>Een geplande failover uitvoeren
1. Selecteer in de Azure-portal **herstelservices** kluis &gt; **herstelplannen (Site Recovery)** &gt; **recoveryplan_name** gemaakt voor de virtuele machine van de bestandsserver.
2. Klik op de blade Recovery plan **meer** &gt; **geplande failover**.

   ![Plan voor herstel](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
3. Op de **geplande Failover bevestigen** blade, kies de bron- en doellocaties en selecteer doelnetwerk en klik op het vinkje ✓ om de failoverproces te starten.
4. Nadat de replica virtuele machines worden gemaakt die ze in behandeling status zijn. Klik op **doorvoeren** doorvoeren van de failover.
5. Nadat de replicatie is voltooid, de virtuele machines gestart op de secundaire locatie.

## <a name="perform-a-failover"></a>Een failover uitvoeren
Tijdens een niet-geplande failover het StorSimple-volumes wordt een failover uitgevoerd met het virtuele apparaat, wordt een replica virtuele machine worden opgeroepen in Azure en de volumes zijn gekoppeld aan de virtuele machine.

#### <a name="to-perform-a-failover"></a>Een failover uitvoeren
1. Selecteer in de Azure-portal **herstelservices** kluis &gt; **herstelplannen (Site Recovery)** &gt; **recoveryplan_name** gemaakt voor de virtuele machine van de bestandsserver.
2. Klik op de blade Recovery plan **meer** &gt; **Failover**.
3. Op de **bevestigen Failover** blade, kies de bron en doel locaties.
4. Selecteer **virtuele machines afsluiten en de meest recente gegevens synchroniseren** om op te geven dat de Site Recovery proberen moet afsluiten van de beveiligde virtuele machine en de gegevens te synchroniseren, zodat de meest recente versie van de gegevens failover.
5. De virtuele machines zijn na de failover in behandeling status. Klik op **doorvoeren** doorvoeren van de failover.


## <a name="perform-a-failback"></a>Een failback uitvoeren
Tijdens een failback volumecontainers StorSimple wordt een failover uitgevoerd terug naar het fysieke apparaat nadat een back-up wordt genomen.

#### <a name="to-perform-a-failback"></a>Een failback uitvoeren
1. Selecteer in de Azure-portal **herstelservices** kluis &gt; **herstelplannen (Site Recovery)** &gt; **recoveryplan_name** gemaakt voor de virtuele machine van de bestandsserver.
2. Klik op de blade Recovery plan **meer** &gt; **geplande Failover**.
3. De bron- en doellocaties kiezen, selecteer de juiste gegevens zijn gesynchroniseerd en opties voor het maken van VM.
4. Klik op **OK** knop om de failback proces te starten.

   ![Failback-bewerking starten](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>Beste praktijken
### <a name="capacity-planning-and-readiness-assessment"></a>Capaciteit plannen en readiness assessment
#### <a name="hyper-v-site"></a>Hyper-V-site
Gebruik de [gebruiker Capaciteitsplanner](http://www.microsoft.com/download/details.aspx?id=39057) voor het ontwerpen van de server-, opslag- en netwerkinfrastructuur voor uw Hyper-V replica-omgeving.

#### <a name="azure"></a>Azure
U kunt uitvoeren de [Azure virtuele Machine Readiness Assessment tool](http://azure.microsoft.com/downloads/vm-readiness-assessment/) op virtuele machines om ervoor te zorgen dat ze compatibel met Azure VM's en Azure Site Recovery-Services zijn. De Readiness Assessment Tool controleert VM configuraties en waarschuwt wanneer configuraties niet compatibel met Azure zijn. Deze geeft bijvoorbeeld een waarschuwing als een station C: groter dan 127 GB is.

Capaciteitsplanning bestaat uit ten minste twee belangrijke processen:

* Toewijzing on-premises Hyper-V-machines naar Azure VM-grootten (zoals A6, A7, A8 en A9).
* Bepalen van de vereiste bandbreedte met Internet.

## <a name="limitations"></a>Beperkingen
* Op dit moment slechts 1 StorSimple-apparaat voor failover (tot een enkel StorSimple Cloud toestel). Het scenario van een bestandsserver die meerdere StorSimple-apparaten omvat is nog niet ondersteund.
* Als u een fout opgetreden krijgt tijdens het inschakelen van beveiliging voor een virtuele machine, ervoor zorgen dat u de verbinding de iSCSI-doelen verbreekt.
* De volumecontainers die samen zijn gegroepeerd vanwege back-upbeleid over volumecontainers wordt samen failover worden uitgevoerd.
* Alle volumes in de volumecontainers die u hebt ervoor gekozen wordt failover worden uitgevoerd.
* Volumes die tot meer dan 64 TB toevoegen kunnen geen failover worden uitgevoerd omdat de maximale capaciteit van een enkel StorSimple Cloud toestel 64 TB is.
* Als de geplande/niet-geplande failover is mislukt en de virtuele machines in Azure worden gemaakt, klikt u vervolgens geen clean up maakt van de virtuele machines. In plaats daarvan een failback doen. Worden als u de virtuele machines verwijdert vervolgens de lokale virtuele machines kunnen niet ingeschakeld opnieuw.
* Na een failover, als u niet kunt zien van de volumes, gaat u naar de virtuele machines, opent u Schijfbeheer, scan de schijven opnieuw en ze online brengt.
* In sommige gevallen kan de stationsletters op de site Noodherstel mogelijk anders dan de letters on-premises. Als dit het geval is, moet u handmatig het probleem te verhelpen nadat de failover is voltooid.
* De time-out van de failover-taak: het StorSimple-script wordt time-outwaarde als de failover van volumecontainers langer duurt dan de limiet voor Azure Site Recovery per script (momenteel 120 minuten).
* Back-uptaak time-out: het StorSimple-script een time-out optreedt als de back-up van volumes langer dan de limiet voor Azure Site Recovery per script (momenteel 120 minuten duurt).

  > [!IMPORTANT]
  > De back-up handmatig uitvoeren vanuit de Azure-portal en voer vervolgens het herstelplan opnieuw uit te voeren.

* Time-out van taak klonen: het StorSimple-script een time-out optreedt als het klonen van volumes langer dan de limiet voor Azure Site Recovery per script (momenteel 120 minuten duurt).
* Synchronisatiefout tijd: het StorSimple scripts fouten uit zeggen dat de back-ups mislukt, zijn ondanks dat de back-up voltooid in de portal is. Een mogelijke oorzaak hiervoor is mogelijk dat de tijd van het StorSimple-apparaat mogelijk niet gesynchroniseerd met de huidige tijd in de tijdzone.

  > [!IMPORTANT]
  > Synchroniseer de tijd van het apparaat met de huidige tijd in de tijdzone.

* Toestel failover-fout: het StorSimple-script kan mislukken als er een failover toestel is als het herstelplan wordt uitgevoerd.

  > [!IMPORTANT]
  > Nadat de failover toestel voltooid is, voert u het herstelplan opnieuw uit.


## <a name="summary"></a>Samenvatting
Met Azure Site Recovery kunt u een herstelplan volledig geautomatiseerd noodherstel voor een bestandsserver VM maken met bestandsshares die worden gehost op een StorSimple-opslag. U kunt de failover start binnen enkele seconden vanaf elke locatie in het geval van een onderbreking en ophalen van de toepassing binnen enkele minuten duren.
