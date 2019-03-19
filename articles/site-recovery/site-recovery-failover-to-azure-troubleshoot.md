---
title: Problemen met failover naar Azure fouten oplossen | Microsoft Docs
description: In dit artikel worden manieren voor het oplossen van veelvoorkomende fouten in de failover wordt uitgevoerd naar Azure beschreven.
author: ponatara
manager: abhemraj
ms.service: site-recovery
services: site-recovery
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/04/2019
ms.author: mayg
ms.openlocfilehash: 75c97a7feb63a100d322610b7e6d2e5c57bebda2
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57889689"
---
# <a name="troubleshoot-errors-when-failing-over-vmware-vm-or-physical-machine-to-azure"></a>Fouten bij het uitvoeren van een failover VM met VMware of fysieke machine naar Azure oplossen

Mogelijk ontvangt u een van de volgende fouten tijdens de failover van een virtuele machine naar Azure. Om op te lossen, gebruikt u de stappen beschreven voor elke fout.

## <a name="failover-failed-with-error-id-28031"></a>Failover is mislukt met fout-ID 28031

Site Recovery kan niet een mislukte maken via de virtuele machine in Azure. Dit kan gebeuren vanwege een van de volgende redenen:

* Er is niet voldoende quotum beschikbaar voor het maken van de virtuele machine: U kunt het beschikbare quotum controleren door te gaan naar de abonnement -> Gebruik + quota. U kunt openen een [nieuwe ondersteuningsaanvraag](https://aka.ms/getazuresupport) om het quotum te verhogen.

* U probeert te failover-virtuele machines van de van verschillende groottefamilies in dezelfde beschikbaarheidsset. Zorg ervoor dat u dezelfde serie met grootten voor alle virtuele machines in dezelfde beschikbaarheidsset kiezen. Grootte wijzigen door naar reken- en netwerkinstellingen van de virtuele machine te gaan en voer vervolgens de failover opnieuw uit.

* Er is een beleid voor het abonnement dat wordt voorkomen het maken van een virtuele machine dat. Wijzig het beleid voor het maken van een virtuele machine toestaan en voer vervolgens de failover opnieuw uit.

## <a name="failover-failed-with-error-id-28092"></a>Failover is mislukt met fout-ID 28092

Site Recovery kan niet een netwerkinterface voor de mislukte maken via de virtuele machine. Zorg ervoor dat u beschikt over voldoende quotum beschikbaar voor het maken van netwerkinterfaces in het abonnement. U kunt het beschikbare quotum controleren door te gaan naar de abonnement -> Gebruik + quota. U kunt openen een [nieuwe ondersteuningsaanvraag](https://aka.ms/getazuresupport) om het quotum te verhogen. Als u beschikt over voldoende quotum, wordt dit wordt mogelijk een onregelmatige uitgeven, probeer het opnieuw. Als het probleem zich blijft, zelfs na nieuwe pogingen voordoen, laat u een opmerking aan het einde van dit document.  

## <a name="failover-failed-with-error-id-70038"></a>Failover is mislukt met fout-ID 70038

Site Recovery kan niet een mislukte maken via de klassieke virtuele machine in Azure. Dit kan gebeuren omdat:

* Een van de resources, zoals een virtueel netwerk dat is vereist voor de virtuele machine moet worden gemaakt, bestaat niet. Het virtuele netwerk zoals bepaald in de instellingen berekening en netwerk van de virtuele machine maken of wijzigen van de instelling voor een virtueel netwerk dat al bestaat en voer vervolgens de failover opnieuw uit.

## <a name="failover-failed-with-error-id-170010"></a>Failover is mislukt met fout-ID 170010

Site Recovery kan niet een mislukte maken via de virtuele machine in Azure. Dit kan gebeuren omdat een interne activiteiten van de hydration is mislukt voor de on-premises virtuele machine.

Als wilt maken van een machine in Azure, is de Azure-omgeving vereist enkele van de stuurprogramma's in opstarten start status en -services zoals DHCP moet in staat automatisch starten. Dus hydration activiteit, op het moment van failover, converteert het opstarttype van **atapi, intelide, storflt, storvsc en vmbus stuurprogramma's** opstarten Start. Ook wordt het opstarttype van enkele services, zoals DHCP geconverteerd naar automatisch starten. Deze activiteit kan mislukken vanwege problemen met de specifieke. 

Handmatig wijzigen het opstarttype van de stuurprogramma's voor **Windows Guest OS**, volgt u de onderstaande stappen te volgen:

1. [Download](https://download.microsoft.com/download/5/D/6/5D60E67C-2B4F-4C51-B291-A97732F92369/Script-no-hydration.ps1) Nee-hydratie script en voer als volgt. Met dit script controleert als virtuele machine is vereist dat hydration.

    `.\Script-no-hydration.ps1`

    Het biedt het volgende resultaat als hydration vereist is:

        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0

        This system doesn't meet no-hydration requirement.

    Als de virtuele machine voldoet aan de vereiste Nee-hydratie, krijgt het script het resultaat 'dit systeem voldoet aan niet-hydratie vereiste'. In dit geval alle stuurprogramma's en services zijn in de status zoals vereist door Azure en hydration op de virtuele machine is niet vereist.

2. Voer het script niet-hydratie-set als volgt uit als de virtuele machine voldoet niet aan de vereiste geen hydration.

    `.\Script-no-hydration.ps1 -set`
    
    Hiermee converteert het opstarttype van de stuurprogramma's en geeft het resultaat zoals hieronder:
    
        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0 

        Updating registry:  REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc   start =  0 

        This system is now no-hydration compatible. 

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Kan geen verbinding maken/RDP/SSH met de mislukte grijs via virtuele machine vanwege de knop verbinding maken op de virtuele machine

Als de **Connect** knop op de failover-VM in Azure is uitgeschakeld en u bent niet verbonden met Azure via een Express Route of Site-naar-Site VPN-verbinding, vervolgens

1. Ga naar **virtuele machine** > **netwerken**, klik op de naam van de netwerkinterface vereist.  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navigeer naar **Ip-configuraties**, klikt u op het naamveld van de vereiste IP-configuratie. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Om in te schakelen openbaar IP-adres, klikt u op **inschakelen**. ![IP inschakelen](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Klik op **vereiste instellingen configureren** > **nieuw**. ![Maak een nieuwe](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Voer de naam van het openbare adres, kiest u de standaardopties voor **SKU** en **toewijzing**, klikt u vervolgens op **OK**.
6. Nu, om de wijzigingen hebt opgeslagen, klikt u op **opslaan**.
7. De deelvensters sluiten en vervolgens naar **overzicht** sectie van de virtuele machine verbinding maken/RDP.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>Kan geen verbinding maken/RDP/SSH - VM Connect-knop beschikbaar

Als de **Connect** knop op de failover-VM in Azure beschikbaar is (niet lichter gekleurd) en schakel de **diagnostische gegevens over opstarten** op uw virtuele Machine en Controleer op fouten, zoals vermeld in [in dit artikel](../virtual-machines/windows/boot-diagnostics.md).

1. Als de virtuele machine niet is gestart, probeert Failover-overschakeling uitvoeren naar een ouder herstelpunt.
2. Als de toepassing in de virtuele machine niet actief, probeer failover wordt uitgevoerd naar een app-consistente herstelpunt is.
3. Als de virtuele machine toegevoegd aan een domein is, zorg ervoor dat domeincontroller correct werkt. Dit kan worden gedaan door het volgende op de hieronder opgegeven stappen:

    a. Maak een nieuwe virtuele machine in hetzelfde netwerk.

    b.  Zorg ervoor dat deze kan worden toegevoegd aan hetzelfde domein waarop de mislukte via virtuele machine wordt verwacht.

    c. Als de domeincontroller **niet** werking nauwkeurig, probeer bent aangemeld bij de mislukte via virtuele machine met behulp van een lokale administrator-account.
4. Als u van een aangepaste DNS-server gebruikmaakt en vervolgens te controleren of deze bereikbaar is. Dit kan worden gedaan door het volgende op de hieronder opgegeven stappen:

    a. Maak een nieuwe virtuele machine in hetzelfde netwerk en

    b. Controleer of de virtuele machine kunnen naamomzetting met behulp van de aangepaste DNS-Server

>[!Note]
>Azure VM-Agent moet worden geïnstalleerd op de virtuele machine voordat de failover wordt uitgevoerd vereist elke andere instelling dan diagnostische gegevens over opstarten

## <a name="unexpected-shutdown-message-event-id-6008"></a>Onverwachte afsluiting bericht (gebeurtenis-ID 6008)

Bij het opstarten van een Windows VM na failover, als u een bericht is onverwacht afgesloten op de herstelde virtuele machine ontvangt, betekent dit dat de status van een virtuele machine afsluiten niet is opgenomen in het herstelpunt dat wordt gebruikt voor de failover. Dit gebeurt wanneer u naar een beheerpunt herstellen wanneer de virtuele machine is niet volledig afgesloten.

Dit is normaal gesproken geen aanleiding en kan meestal worden genegeerd voor niet-geplande failovers. Als de failover is gepland, zorg ervoor dat de virtuele machine correct wordt afgesloten voordat u een failover en geef voldoende tijd voor in behandeling zijnde replicatie gegevens on-premises naar Azure worden verzonden. Gebruik vervolgens de **nieuwste** kiezen op de [Failover scherm](site-recovery-failover.md#run-a-failover) zodat alle in behandeling gegevens in Azure worden verwerkt in een herstelpunt, die vervolgens wordt gebruikt voor VM-failover.

## <a name="unable-to-select-the-datastore"></a>Kan de gegevensopslag selecteren

Dit probleem wordt aangegeven wanneer u zich niet zien de gegevensopslag in Azure wanneer wordt geprobeerd om opnieuw te beveiligen van de virtuele machine die een failover is opgetreden in de portal. Dit komt doordat de Master target wordt niet herkend als een virtuele machine onder vCenters toegevoegd aan Azure Site Recovery.

Zie voor meer informatie over het opnieuw beveiligen van een virtuele machine, [opnieuw beveiligen en mislukt de back-machines naar een on-premises site na een failover naar Azure](vmware-azure-reprotect.md).

Het probleem kunt oplossen:

Handmatig maken van de Master-doel in het vCenter waarmee de bronmachine wordt beheerd. Het gegevensarchief worden beschikbaar na de volgende vCenter-detectie en vernieuw fabric bewerkingen.

> [!Note]
> 
> De detectie en vernieuw fabric bewerkingen kunnen tot 30 minuten duren. 

## <a name="linux-master-target-registration-with-cs-fails-with-an-ssl-error-35"></a>Linux-hoofddoel registratie bij CS is mislukt met een SSL-fout 35 

De Azure Site Recovery-Masterdoel registratie bij de configuratieserver is mislukt vanwege de geverifieerde Proxy wordt ingeschakeld op de Hoofddoelserver. 
 
Deze fout wordt aangegeven door de volgende tekenreeksen in het installatielogboek: 

RegisterHostStaticInfo opgetreden uitzondering config/talwrapper.cpp(107) [post] CurlWrapper bericht is mislukt: server: 10.38.229.221, poort: 443, phpUrl: request_handler.php, beveiligde: true, ignoreCurlPartialError: false met de volgende fout: [op curlwrapperlib/curlwrapper.cpp:processCurlResponse:231] kan niet aanvraag: (35) - SSL verbinding maken met de fout. 
 
Het probleem kunt oplossen:
 
1. Open een opdrachtprompt op de configuratieserver virtuele machine en controleer of de proxy-instellingen met de volgende opdrachten:

    CAT /etc/environment echo $gebruikt $https_proxy echo 

2. Als de uitvoer van de vorige opdrachten ziet u dat de gebruikt of https_proxy instellingen zijn gedefinieerd, kunt u een van de volgende methoden om de blokkering van de Masterdoelserver-communicatie met de configuratieserver te gebruiken:
   
   - Download de [PsExec-hulpprogramma](https://aka.ms/PsExec).
   - Het hulpprogramma gebruiken voor toegang tot de systeemcontext van de gebruiker en bepalen of de proxy-adres is geconfigureerd. 
   - Als de proxy is geconfigureerd, opent u Internet Explorer in de context van een systeem-gebruiker met het PsExec-hulpprogramma.
  
     **psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"**

   - Om ervoor te zorgen dat de hoofddoelserver met de configuratieserver communiceren kan:
  
     - Wijzig de proxy-instellingen in Internet Explorer het hoofddoel IP-adres van de server via de proxy overslaan.   
     of
     - Schakel de proxy op de hoofddoelserver. 


## <a name="next-steps"></a>Volgende stappen
- Problemen oplossen [RDP-verbinding met Windows-VM](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Problemen oplossen [SSH-verbinding met Linux-VM](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Als u meer hulp nodig hebt, klikt u vervolgens boek uw query op [Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) of een opmerking toevoegen aan het einde van dit document. Er is een actieve community die zou het mogelijk om u te helpen.
