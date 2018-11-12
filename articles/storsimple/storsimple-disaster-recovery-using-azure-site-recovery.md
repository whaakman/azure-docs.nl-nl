---
title: StorSimple bestandsshare, herstel na Noodgeval met Azure Site Recovery automatiseren | Microsoft Docs
description: Beschrijft de stappen en aanbevolen procedures voor het maken van een oplossing voor noodherstel voor bestandsshares die worden gehost op Microsoft Azure StorSimple-opslag.
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: ''
ms.assetid: 23049a2c-055e-4d0e-b8f5-af2a87ecf53f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2017
ms.author: vidarmsft
ms.openlocfilehash: c88df7ba1a9a60ffcda9a5235197037088abca4e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249265"
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Automatisch herstel na noodgevallen oplossing met behulp van Azure Site Recovery voor bestandsshares die worden gehost op StorSimple
## <a name="overview"></a>Overzicht
Microsoft Azure StorSimple is een oplossing voor hybride cloudopslag die de complexiteit van niet-gestructureerde gegevens die betrekking hebben op bestandsshares. StorSimple maakt gebruik van cloudopslag als een uitbreiding van de on-premises oplossing en automatisch lagen gegevens in on-premises opslag en opslag in de cloud. Gegevensbescherming, geïntegreerd met lokale en cloud-momentopnamen, elimineert de noodzaak voor een veelzijdig opslaginfrastructuur.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) is een Azure-gebaseerde service die voorziet in disaster recovery (DR) functies door het coördineren van replicatie, failover en herstel van virtuele machines. Azure Site Recovery ondersteunt een aantal technologieën voor replicatie consistent repliceren, beveiligen en naadloos via failover virtuele machines en toepassingen voor persoonlijke/openbare of gehoste clouds.

Met Azure Site Recovery, replicatie voor virtuele machines en mogelijkheden van de StorSimple cloud-momentopname, kunt u het volledige bestand server-omgeving beveiligen. In geval van een onderbreking, kunt u één klik uw bestandsshares online brengen in Azure in een paar minuten.

Dit document wordt in detail uitgelegd hoe u kunt een oplossing voor noodherstel voor uw bestandsshares die worden gehost op een StorSimple-opslag maken en uitvoeren geplande, niet-geplande en testfailovers met behulp van een herstelplan met één klik. In wezen het laat zien hoe u het herstelplan kunt wijzigen in de Azure Site Recovery-kluis om te zorgen voor StorSimple failovers tijdens na noodgevallen. Bovendien beschrijft deze ondersteunde configuraties en vereisten. Dit document wordt ervan uitgegaan dat u bekend met de basisprincipes van Azure Site Recovery en de StorSimple-architecturen bent.

## <a name="supported-azure-site-recovery-deployment-options"></a>Ondersteunde Azure Site Recovery-implementatieopties
Klanten kunnen bestandsservers implementeren als fysieke servers of virtuele machines (VM's) die worden uitgevoerd op Hyper-V of VMware en maakt u bestandsshares van volumes oppervlaktevariaties buiten het StorSimple-opslag. Azure Site Recovery kan zowel fysieke als virtuele implementaties naar een secundaire site of naar Azure te beveiligen. In dit document bevat informatie over de details van een DR-oplossing met Azure als de site recovery voor een virtuele machine wordt gehost op Hyper-V server en bestandsshares op het StorSimple-opslag. Andere scenario's waarin de VM van de bestandsserver zich op een VMware-VM of een fysieke computer bevindt kunnen op dezelfde manier worden geïmplementeerd.

## <a name="prerequisites"></a>Vereisten
Implementatie van een oplossing voor noodherstel voor één muisklik die gebruikmaakt van Azure Site Recovery voor bestandsshares die worden gehost op cloudopslag met StorSimple heeft de volgende vereisten:

   - On-premises Windows Server 2012 R2-bestand-server die virtuele machine wordt gehost op Hyper-V of VMware of op een fysieke computer
   - StorSimple-apparaat on-premises opslag die is geregistreerd bij Azure StorSimple manager
   - StorSimple-Cloudapparaat hebt gemaakt in de Azure StorSimple manager. Het apparaat kan worden bewaard in een status afsluiten.
   - Bestandsshares die worden gehost op de volumes die zijn geconfigureerd op het StorSimple-opslagapparaat
   - [Azure Site Recovery services-kluis](../site-recovery/site-recovery-vmm-to-vmm.md) gemaakt in een Microsoft Azure-abonnement

Bovendien als Azure uw site recovery, voert u de [Azure Virtual Machine Readiness Assessment tool](https://azure.microsoft.com/downloads/vm-readiness-assessment/) op virtuele machines om ervoor te zorgen dat ze compatibel met Azure-VM's en Azure Site Recovery zijn-services.

Om te voorkomen dat latentie problemen (dit kunnen leiden tot hogere kosten voor het), zorg ervoor dat u uw StorSimple-Cloudapparaat, automation-account en storage maakt (s) in dezelfde regio.

## <a name="enable-dr-for-storsimple-file-shares"></a>Inschakelen van herstel na Noodgevallen voor StorSimple-bestandsshares
Elk onderdeel van de on-premises-omgeving moet worden beveiligd als u wilt inschakelen replicatie is voltooid en het herstel. Deze sectie wordt beschreven hoe u:
    
   - Instellen van Active Directory en DNS-replicatie (optioneel)
   - Gebruik Azure Site Recovery-beveiliging van de bestandsserver VM inschakelen
   - Schakel de beveiliging van de StorSimple-volumes
   - Het netwerk configureren

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Instellen van Active Directory en DNS-replicatie (optioneel)
Als u beveiligen van de computers met Active Directory en DNS wilt, zodat ze beschikbaar op de DR-site zijn, moet u expliciet beveiligen (zodat de bestandsservers na een failover met verificatie toegankelijk zijn). Er zijn twee aanbevolen opties op basis van de complexiteit van de klant on-premises omgeving.

#### <a name="option-1"></a>Optie 1
Als de klant een klein aantal toepassingen heeft, één domeincontroller voor het gehele on-premises site, en zal worden failover van de hele site, en vervolgens wordt u Azure Site Recovery-replicatie aangeraden voor het repliceren van de domain controller machine naar een secundaire de site (dit is van toepassing op zowel de site-naar-site en de site-naar-Azure).

#### <a name="option-2"></a>Optie 2
Als de klant heeft een groot aantal toepassingen, een Active Directory-forest wordt uitgevoerd en zal worden Failover-overschakeling uitvoeren enkele toepassingen op een tijdstip, wordt aangeraden het instellen van een extra domeincontroller op de DR-site (een secundaire site of in Azure).

Raadpleeg [automatisch DR-oplossing voor Active Directory en DNS met behulp van Azure Site Recovery](../site-recovery/site-recovery-active-directory.md) voor instructies wanneer het beschikbaar maken van een domeincontroller op de DR-site. Voor de rest van dit document, nemen we aan dat een domeincontroller beschikbaar is op de DR-site.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Gebruik Azure Site Recovery-beveiliging van de bestandsserver VM inschakelen
Deze stap is vereist dat u de on-premises bestand server-omgeving voorbereiden, maken en voorbereiden van een Azure Site Recovery-kluis en schakel de bestandsbeveiliging van de virtuele machine.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>De on-premises bestand server-omgeving voorbereiden
1. Stel de **User Account Control** naar **nooit een melding weergeven**. Dit is vereist zodat u Azure automation-scripts gebruiken kunt om de iSCSI-doelen na een failover door Azure Site Recovery.
   
   1. Druk op de Windows-toets + Q en zoeken naar **UAC**.  
   1. Selecteer **instellingen voor Gebruikersaccountbeheer wijzigen**.  
   1. Sleep de balk onderaan naar **nooit een melding weergeven**.  
   1. Klik op **OK** en selecteer vervolgens **Ja** wanneer hierom wordt gevraagd.  
   
      ![Instellingen voor Gebruikersaccountbeheer](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png) 

1. Installeer de VM-Agent op elk van de virtuele machines van de bestandsserver. Dit is vereist zodat u Azure automation-scripts op de mislukte over virtuele machines uitvoeren kunt.
   
   1. [Download de agent](https://aka.ms/vmagentwin) naar `C:\\Users\\<username>\\Downloads`.
   1. Open Windows PowerShell in de beheerdersmodus (als Administrator uitvoeren) en voer de volgende opdracht uit om te navigeren naar de downloadlocatie:  
         `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`
         
         > [!NOTE]
         > Naam van het bestand veranderen, afhankelijk van de versie.
      
1. Klik op **Volgende**.
1. Accepteer de **van voorwaarden** en klik vervolgens op **volgende**.
1. Klik op **Voltooien**.
1. Bestandsshares met behulp van volumes oppervlaktevariaties buiten het StorSimple-opslag maken. Zie voor meer informatie, [de StorSimple Manager-service gebruiken voor het beheren van volumes](storsimple-manage-volumes.md).
   
   1. Op uw on-premises virtuele machines, drukt u op de Windows-toets + Q en zoeken naar **iSCSI**.
   1. Selecteer **iSCSI-initiator**.
   1. Selecteer de **configuratie** tabblad en de naam van de initiator te kopiëren.
   1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
   1. Selecteer de **StorSimple** tabblad en selecteer vervolgens de StorSimple Manager-Service met het fysieke apparaat.
   1. Volume (s) maken en maak vervolgens volumes. (Deze volumes zijn voor het bestand share (s) op de bestandsserver-VM's). Kopieer de initiatornaam en geef een passende naam voor de Access Control Records bij het maken van de volumes.
   1. Selecteer de **configureren** tabblad en noteer het IP-adres van het apparaat omlaag.
   1. Op uw on-premises machines, gaat u naar de **iSCSI-initiator** opnieuw en voer het IP-adres in de sectie snel verbinding maken. Klik op **snelle verbinding** (het apparaat moet nu worden verbonden).
   1. Open de Azure portal en selecteer de **Volumes en apparaten** tabblad. Klik op **automatisch configureren**. Het volume dat u hebt gemaakt, moet worden weergegeven.
   1. Selecteer in de portal de **apparaten** tabblad en selecteer vervolgens **maken van een nieuwe virtueelapparaat.** (Dit virtuele apparaat zal worden gebruikt als een failover is uitgevoerd). Deze nieuwe virtuele apparaat kan worden bewaard in een offline status om te voorkomen dat extra kosten. Als u het virtuele apparaat offline, gaat u naar de **virtuele Machines** sectie in de Portal en sluit deze af.
   1. Ga terug naar de on-premises VM's en opent u Schijfbeheer (druk op de Windows-toets + X en selecteer **Schijfbeheer**).
   1. Hier ziet u een aantal extra schijven (afhankelijk van het aantal volumes die u hebt gemaakt). Met de rechtermuisknop op de eerste afbeelding, selecteer **schijf initialiseren**, en selecteer **OK**. Met de rechtermuisknop op de **Unallocated** sectie, selecteer **Nieuw eenvoudig Volume**, er een stationsletter aan toewijzen en de wizard hebt voltooid.
   1. Herhaal stap l voor alle schijven. U ziet nu alle schijven op **deze PC** in Windows Verkenner.
   1. De functie bestands- en opslagservices gebruiken om bestandsshares te maken op deze volumes.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Om te maken en voorbereiden van een Azure Site Recovery-kluis
Raadpleeg de [documentatie voor Azure Site Recovery](../site-recovery/site-recovery-hyper-v-site-to-azure.md) aan de slag met Azure Site Recovery voordat de virtuele machine van de bestandsserver wordt beveiligd.

#### <a name="to-enable-protection"></a>Beveiliging in te schakelen
1. De iSCSI-doelen verbreken van de on-premises-VM's die u wilt beveiligen met Azure Site Recovery:
   
   1. Druk op Windows-toets + Q en zoek naar de **iSCSI**.
   1. Selecteer **instellen van iSCSI-initiator**.
   1. Het StorSimple-apparaat dat u eerder verbinding verbreken. U kunt ook de server uitschakelen voor een paar minuten bij het inschakelen van beveiliging.
      
   > [!NOTE]
   > Dit zorgt ervoor dat de bestandsshares zijn tijdelijk niet beschikbaar.
   
1. [Schakel de beveiliging van de virtuele machine](../site-recovery/site-recovery-hyper-v-site-to-azure.md) van de bestandsserver VM vanuit de Azure Site Recovery-portal.
1. Wanneer de initiële synchronisatie wordt gestart, kunt u het doel opnieuw. Ga naar de iSCSI-initiator, selecteert u het StorSimple-apparaat en klik op **Connect**.
1. Wanneer de synchronisatie is voltooid en de status van de virtuele machine is **beveiligde**, selecteer de virtuele machine, selecteer de **configureren** tabblad en dienovereenkomstig bijwerken van het netwerk van de virtuele machine (dit is het netwerk dat de mislukte via VM (s) is een deel van). Als het netwerk wordt niet weergegeven, betekent dit dat de synchronisatie is nog steeds gebeurt.

### <a name="enable-protection-of-storsimple-volumes"></a>Schakel de beveiliging van de StorSimple-volumes
Als u niet hebt geselecteerd de **inschakelen van een standaard back-up voor dit volume** optie voor het StorSimple-volumes, gaat u naar **back-upbeleid** in StorSimple Manager-service, en maak een geschikte back-upbeleid voor alle volumes. U wordt aangeraden dat u de frequentie van back-ups ingesteld op het beoogde herstelpunt (RPO) die u graag zou willen zien voor de toepassing.

### <a name="configure-the-network"></a>Het netwerk configureren
Voor de bestandsserver VM netwerkinstellingen configureren in Azure Site Recovery zodat de VM-netwerken zijn gekoppeld aan het juiste DR-netwerk na een failover.

Kunt u de virtuele machine in de **gerepliceerde items** tab om de netwerkinstellingen configureren zoals wordt weergegeven in de volgende afbeelding.

![Berekening en netwerk](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Een herstelplan maken
U kunt een plan voor herstel in ASR voor het automatiseren van het failoverproces van de bestandsshares maken. Als er een onderbreking optreedt, kunt u de bestandsshares van brengen in een paar minuten met één klik. Als u wilt deze automatisering inschakelen, moet u een Azure automation-account.

#### <a name="to-create-an-automation-account"></a>Een Automation-account maken
1. Ga naar de Azure-portal &gt; **Automation** sectie.
1. Klik op **+ toevoegen** knop klikt, wordt geopend onder de blade.
   
   ![Automation-account toevoegen](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)
   
   - Geef de naam - Voer een nieuw automation-account
   - Abonnement - abonnement kiezen
   - Resource group - Maak nieuwe/Kies bestaande resourcegroep
   - Locatie - locatie te kiezen, bewaart u deze in de dezelfde geo/de regio waarin het StorSimple-Cloudapparaat en Storage-Accounts zijn gemaakt.
   - Azure uitvoeren als-account maken - Selecteer **Ja** optie.
   
1. Ga naar het Automation-account, klikt u op **Runbooks** &gt; **bladeren in galerie** alle vereiste runbooks in het automation-account importeren.
1. De volgende runbooks toevoegen door op te sporen **herstel na noodgevallen** tag op in de galerie:
   
   - Opruimen van de StorSimple-volumes na de Test-Failover (TFO)
   - Volumecontainers voor Failover StorSimple
   - Koppelen van volumes op StorSimple-apparaat na een failover
   - Aangepaste scriptextensie in Azure VM verwijderen
   - StorSimple Virtual Appliance starten
   
      ![Bladeren in galerie](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)
   
1. Publicatie van alle scripts door het selecteren van het runbook in het automation-account en klik op **bewerken** &gt; **publiceren** en vervolgens **Ja** in het bevestigingsbericht. Na deze stap wordt de **Runbooks** tabblad er als volgt uitzien:
   
   ![Runbooks](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)
   
1. Klik in het automation-account op **variabelen** &gt; **toevoegen van een variabele** en voeg de volgende variabelen. U kunt kiezen voor het versleutelen van deze elementen. Deze variabelen zijn specifiek plan voor herstel. Als uw herstelplan, die in de volgende stap maakt u de naam is TestPlan, en vervolgens uw variabelen moeten worden TestPlan-StorSimRegKey, TestPlan-AzureSubscriptionName, enzovoort.

   - **BaseUrl**: de Resource Manager-url voor de Azure-cloud. Met behulp van ophalen **Get-AzureRmEnvironment | De naam van de Select-Object, ResourceManagerUrl** cmdlet.
   - * RecoveryPlanName ***- ResourceGroupName**: de Resource Manager-groep met de StorSimple-resource.
   - * RecoveryPlanName ***- ManagerName**: het StorSimple-resource met de StorSimple-apparaat.
   - * RecoveryPlanName ***- DeviceName**: het StorSimple-apparaat met failover.
   - * RecoveryPlanName ***- DeviceIpAddress**: het IP-adres van het apparaat (dit kan worden gevonden in de **apparaten** tabblad onder de sectie van de StorSimple Device Manager &gt; **instellingen** &gt; **Netwerk** &gt; **DNS-instellingen** groep).
   - * RecoveryPlanName ***- VolumeContainers**: een tekenreeks met door komma's gescheiden van de volumecontainers die aanwezig zijn op het apparaat dat moet worden kan niet meer dan, bijvoorbeeld: volcon1, volcon2, volcon3.
   - * RecoveryPlanName ***- TargetDeviceName**: het StorSimple-Cloudapparaat waarop de containers zijn failover mogelijk is.
   - * RecoveryPlanName ***- TargetDeviceIpAddress**: het IP-adres van het doelapparaat (dit kan worden gevonden in de **virtuele Machine** sectie &gt; **instellingen** groep &gt; **netwerken** tabblad).
   - * RecoveryPlanName ***- StorageAccountName**: naam van het opslagaccount waarin het script (die worden gebruikt op de via virtuele machine) worden opgeslagen. Dit kan een storage-account dat de ruimte vrij voor het opslaan van het script tijdelijk is zijn.
   - * RecoveryPlanName ***- StorageAccountKey**: de toegangssleutel voor de bovenstaande storage-account.
   - * RecoveryPlanName ***- VMGUIDS**: bij het beveiligen van een virtuele machine, Azure Site Recovery wijst elke virtuele machine een unieke ID waarmee de details van de mislukte uitvoert voor virtuele machine. Als u wilt de VMGUID, selecteer de **herstelservices** tabblad en klik op **beveiligde Item** &gt; **beveiligingsgroepen** &gt;  **Machines** &gt; **eigenschappen**. Als u meerdere virtuele machines hebt, kunt u vervolgens de GUID's als een door komma's gescheiden tekenreeks op toevoegen.

    Bijvoorbeeld, als de naam van het herstelplan is fileServerpredayRP, vervolgens uw **variabelen**, **verbindingen** en **certificaten** tabblad moet als volgt worden weergegeven nadat u hebt toegevoegd alle assets.

      ![Assets](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

1. StorSimple 8000-serie runbookmodule in uw Automation-account uploaden. Gebruik de onderstaande stappen voor het toevoegen van een module:
   
   1. Open powershell, maak een nieuwe map en wijzig de map naar de map.
      
      ```
            mkdir C:\scripts\StorSimpleSDKTools
            cd C:\scripts\StorSimpleSDKTools
      ```
   1. Download nuget CLI onder de map in stap 1.
      Verschillende versies van nuget.exe zijn beschikbaar op [nuget downloads](https://www.nuget.org/downloads). Elke downloadkoppeling rechtstreeks verwijst naar een .exe-bestand, dus zorg ervoor dat u met de rechtermuisknop op en sla het bestand op uw computer in plaats uitvoeren vanuit de browser.
      
      ```
            wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
      ```
      
   1. De afhankelijke SDK downloaden
      
      ```
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
      ```
      
   1. Maak een Azure Automation-Runbook-Module voor het beheer van StorSimple 8000-serie-apparaten. Gebruik de onderstaande opdrachten voor het maken van een Automation-module-zip-bestand.
         
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
         
   1. Importeer het Azure Automation-module zip-bestand (Microsoft.Azure.Management.StorSimple8000Series.zip) in de bovenstaande stap gemaakt. Dit kan worden gedaan door het Automation-Account te selecteren, klikt u op **Modules** onder gedeelde bronnen en klik vervolgens op **toevoegen van een module**.
   
   Nadat u de StorSimple 8000-serie-module importeren de **Modules** tabblad moet er als volgt uitzien:
   
      ![Modules](./media/storsimple-disaster-recovery-using-azure-site-recovery/image12.png)

1. Ga naar de **herstelservices** sectie en selecteer de Azure Site Recovery-kluis die u eerder hebt gemaakt.
1. Selecteer de **herstelplannen (Site Recovery)** optie van **beheren** groeperen en maak een nieuw herstelplan als volgt:
   
   - Klik op **+ herstellen plan** knop klikt, wordt geopend onder de blade.
      
      ![Herstelplan maken](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)
      
   - Voer een naam in de recovery-abonnement, kiest u bron, doel & Deployment model waarden.
   
   - Selecteer de virtuele machines uit de beveiligingsgroep die u wilt opnemen in het herstelplan te gaan en op **OK** knop.
   
   - Plan voor herstel die u eerder hebt gemaakt, klik op **aanpassen** knop om de weergave Recovery plan aanpassing te openen.
   
   - Klik met de rechtermuisknop op **afsluiting van alle groepen** en klikt u op **actie vooraf toevoegen**.
   
   - Hiermee opent u actie blade invoegen, voer een naam in, selecteer **primaire zijde** in waar u de optie uitvoeren, selecteert u Automation-account (in die u hebt toegevoegd de runbooks) en selecteer vervolgens de optie de **Failover-StorSimple-Volume-Containers**  runbook.
   
   - Klik met de rechtermuisknop op **groep 1: Start** en klikt u op **toevoegen beveiligde items** optie en selecteer de virtuele machines die moeten worden beveiligd in het herstelplan te gaan en op **Ok** knop. Optioneel, als deze al virtuele machines geselecteerd.
   
   - Klik met de rechtermuisknop op **groep 1: Start** en klikt u op **boeken actie** optie en vervolgens de volgende scripts toevoegen:  
      
      - Runbook starten-StorSimple-virtueel-apparaat  
      - Via-StorSimple-volume-containers runbook mislukt  
      - Runbook koppelen volumes na failover  
      - Runbook verwijderen---extensie voor aangepaste scripts  
        
   - Toevoegen van een handmatige actie na de bovenstaande 4 scripts in dezelfde **groep 1: stappen na** sectie. Deze actie is het punt waarop u controleren kunt of alles goed werkt. Deze actie moet worden toegevoegd als onderdeel van de failovertest alleen (alleen, dus selecteer de **Testfailover** selectievakje).
    
   - Na de handmatige actie toevoegen de **opschonen** script met behulp van dezelfde procedure die u hebt gebruikt voor de andere runbooks. **Sla** het herstelplan te gaan.
    
   > [!NOTE]
   > Wanneer u een failovertest uitvoert, moet u controleren of alles bij de stap van de handmatige actie omdat de StorSimple-volumes die was op het doelapparaat is gekloond als onderdeel van opschoning worden verwijderd nadat de handmatige actie is voltooid.
       
      ![Recoery plan](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Een testfailover uitvoeren
Raadpleeg de [Active Directory-DR-oplossing](../site-recovery/site-recovery-active-directory.md) handleiding voor overwegingen met betrekking tot specifieke naar Active Directory tijdens de testfailover. Het instellen van de lokale is niet helemaal wordt verstoord wanneer de testfailover plaatsvindt. De StorSimple-volumes die zijn gekoppeld aan de on-premises VM worden gekloond naar de StorSimple-Cloudapparaat in Azure. Een virtuele machine voor testdoeleinden wordt opgeroepen in Azure en de gekloonde volumes zijn gekoppeld aan de virtuele machine.

#### <a name="to-perform-the-test-failover"></a>De testfailover uitvoeren
1. Selecteer uw Site Recovery-kluis in de Azure-portal.
1. Klik op het plan voor herstel gemaakt voor de virtuele machine van de bestandsserver.
1. Klik op **Testfailover**.
1. Selecteer het Azure-netwerk waarmee virtuele Azure-machines moeten worden verbonden nadat failover is uitgevoerd.
   
   ![Failover starten](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
   
1. Klik op **OK** om te beginnen met de failover. U kunt de voortgang volgen door te klikken op de virtuele machine om de eigenschappen te openen of op de **Test failovertaak** in kluisnaam &gt; **taken** &gt; **Site Recovery-taken**.
1. Nadat de failover is voltooid, kunt u moet ook de gerepliceerde Azure Zie machine worden weergegeven in de Azure-portal &gt; **virtuele Machines**. U kunt uw validaties uitvoeren.
1. Nadat de controles zijn uitgevoerd, klikt u op **validaties volledige**. Hiermee verwijdert u de StorSimple-Volumes en afsluiten van de StorSimple-Cloudapparaat.
1. Wanneer u klaar bent, klikt u op **failovertest** op het herstelplan te gaan. In de opmerkingen bij de vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover. Hiermee verwijdert u de virtuele machine die zijn gemaakt tijdens de testfailover.

## <a name="perform-a-planned-failover"></a>Een geplande failover uitvoeren
   De on-premises bestandsserver tijdens een geplande failover die VM zonder problemen wordt afgesloten en een cloud back-up van de momentopname van de volumes op het StorSimple-apparaat wordt gemaakt. De StorSimple-volumes zijn een failover naar het virtuele apparaat, een replica virtuele machine van Azure wordt gebracht en de volumes zijn gekoppeld aan de virtuele machine.

#### <a name="to-perform-a-planned-failover"></a>Een geplande failover uitvoeren
1. Selecteer in de Azure portal, **herstelservices** kluis &gt; **herstelplannen (Site Recovery)** &gt; **recoveryplan_name** gemaakt voor de VM-bestandsserver.
1. Klik op de blade Recovery plan **meer** &gt; **geplande failover**.

   ![Plan voor herstel](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
1. Op de **geplande Failover bevestigen** blade, kiest u de bron- en doellocaties voor- en doel selecteren, netwerk en klik op het vinkje ✓ om de failoverproces te starten.
1. Nadat de replica virtuele machines zijn gemaakt nog in behandeling staat. Klik op **doorvoeren** om door te voeren van de failover.
1. Nadat de replicatie is voltooid, starten de virtuele machines op de secundaire locatie.

## <a name="perform-a-failover"></a>Een failover uitvoeren
Tijdens een niet-geplande failover, het StorSimple-volumes zijn een failover naar het virtuele apparaat en een replica-VM op Azure worden gebracht van de volumes zijn gekoppeld aan de virtuele machine.

#### <a name="to-perform-a-failover"></a>Een failover uitvoeren
1. Selecteer in de Azure portal, **herstelservices** kluis &gt; **herstelplannen (Site Recovery)** &gt; **recoveryplan_name** gemaakt voor de VM-bestandsserver.
1. Klik op de blade Recovery plan **meer** &gt; **Failover**.
1. Op de **bevestigen Failover** blade, kiest u de bron en doel locaties.
1. Selecteer **virtuele machines afsluiten en de nieuwste gegevens synchroniseren** om op te geven dat Site Recovery proberen moet de beveiligde virtuele machine uitschakelen en de gegevens te synchroniseren, zodat de meest recente versie van de gegevens wordt geen failover worden uitgevoerd.
1. Na de failover wordt de virtuele machines in een doorvoering status in behandeling zijn. Klik op **doorvoeren** om door te voeren van de failover.


## <a name="perform-a-failback"></a>Failback uitvoeren
Tijdens een failback volumecontainers StorSimple zijn een failover naar het fysieke apparaat na een back-up is gemaakt.

#### <a name="to-perform-a-failback"></a>Een failback uitvoeren
1. Selecteer in de Azure portal, **herstelservices** kluis &gt; **herstelplannen (Site Recovery)** &gt; **recoveryplan_name** gemaakt voor de VM-bestandsserver.
1. Klik op de blade Recovery plan **meer** &gt; **geplande Failover**.
1. Kies de bron- en doellocaties, selecteer de geschikte synchronisatie van gegevens en opties voor het maken van virtuele machine.
1. Klik op **OK** knop om het failback-proces te starten.
   
   ![Starten van Failback](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>Beste praktijken
### <a name="capacity-planning-and-readiness-assessment"></a>Capaciteit plannen en readiness assessment
#### <a name="hyper-v-site"></a>Hyper-V-site
Gebruik de [gebruiker Capaciteitsplanner](https://www.microsoft.com/download/details.aspx?id=39057) ontwerpen van de server-, opslag- en netwerkinfrastructuur voor uw Hyper-V replica-omgeving.

#### <a name="azure"></a>Azure
U kunt uitvoeren de [Azure Virtual Machine Readiness Assessment tool](https://azure.microsoft.com/downloads/vm-readiness-assessment/) op virtuele machines om ervoor te zorgen dat ze compatibel met Azure VM's en Azure Site Recovery-Services zijn. De Readiness Assessment Tool controleert de configuraties van virtuele machine en u wordt gewaarschuwd wanneer configuraties niet compatibel met Azure zijn. Deze geeft bijvoorbeeld een waarschuwing als een station groter dan 127 GB is.

Capaciteitsplanning is opgebouwd uit ten minste twee belangrijke processen:

   - Toewijzing van on-premises Hyper-V-machines naar Azure VM-grootten (zoals A6, A7, A8 en A9).
   - Bepalen van de vereiste bandbreedte met Internet.

## <a name="limitations"></a>Beperkingen
   - Op dit moment slechts 1 StorSimple-apparaat kan een failover (met een één StorSimple-Cloudapparaat). Het scenario van een bestandsserver die meerdere StorSimple-apparaten omvat is nog niet ondersteund.
   - Als u een fout optreedt terwijl u beveiliging voor een virtuele machine inschakelt, zorg ervoor dat u de iSCSI-doelen niet zijn verbonden.
   - De volumecontainers die samen zijn gegroepeerd vanwege back-upbeleid verdelen over volumecontainers wordt een failover samen.
   - Alle volumes in de volumecontainers die u hebt ervoor gekozen wordt failover mogelijk is.
   - Volumes die tot meer dan 64 TB toevoegen kunnen geen failover omdat de maximale capaciteit van een enkele StorSimple-Cloudapparaat 64 TB is.
   - Als de geplande/niet-geplande failover is mislukt en de virtuele machines in Azure worden gemaakt, klikt u vervolgens verwijdert u niet de virtuele machines. In plaats daarvan een failback uitvoeren. Worden als u de virtuele machines verwijdert vervolgens de on-premises VM's kunnen niet ingeschakeld op het opnieuw.
   - Na een failover, als u niet kunnen zien van de volumes, gaat u naar de virtuele machines, opent u Schijfbeheer Scan de schijven opnieuw en ze online brengt.
   - In sommige gevallen de stationsletters in de DR-site mogelijk anders dan de letters on-premises. Als dit gebeurt, moet u handmatig het probleem te verhelpen nadat de failover is voltooid.
   - De time-out van de failover-taak: het StorSimple-script wordt een time-out als de failover van de volumecontainers langer duurt dan de Azure Site Recovery-limiet per script (momenteel 120 minuten).
   - Back-uptaak time-out: het StorSimple-script een time-out optreedt als de back-up van volumes langer dan de Azure Site Recovery-limiet per script (momenteel 120 minuten duurt).
   
   > [!IMPORTANT]
   > De back-up handmatig uitvoeren vanuit de Azure-portal en voer vervolgens het herstelplan opnieuw uit.
   
   - Klonen van de time-out van taak: het StorSimple-script een time-out optreedt als het klonen van volumes meer tijd dan de Azure Site Recovery-limiet per script (momenteel 120 minuten kost).
   - Tijd synchronisatiefout: het StorSimple-scripts fouten uit waarin wordt gemeld dat de back-ups mislukt, zijn zelfs als de back-up voltooid in de portal is. Een mogelijke oorzaak voor dit kan zijn dat de tijd van het StorSimple-apparaat mogelijk niet gesynchroniseerd met de huidige tijd in de tijdzone.
   
   > [!IMPORTANT]
   > Synchroniseer de tijd van het apparaat met de huidige tijd in de tijdzone.
   
   - Apparaat failover-fout: het StorSimple-script kan mislukken als er een failover van het apparaat wanneer het herstelplan wordt uitgevoerd.
   
   > [!IMPORTANT]
   > Nadat de failover van het apparaat voltooid is, voert u het herstelplan opnieuw uit.


## <a name="summary"></a>Samenvatting
Met Azure Site Recovery, kunt u een herstelplan volledig geautomatiseerd noodherstel voor een bestandsserver-VM maken met bestandsshares die worden gehost op een StorSimple-opslag. U kunt de failover starten binnen enkele seconden vanaf elke locatie in het geval van een onderbreking en haal de toepassing bij en worden uitgevoerd in een paar minuten.
