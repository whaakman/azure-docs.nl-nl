---
title: Problemen met failover naar Azure oplossen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u algemene problemen oplossen tijdens de failover naar Azure met Azure Site Recovery.
author: ponatara
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 09/11/2018
ms.author: ponatara
ms.openlocfilehash: de0b3a51ae7c7cca91366b955c5fa74963d95d27
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50211669"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Fouten bij het uitvoeren van een failover een virtuele machine naar Azure oplossen

Mogelijk ontvangt u een van de volgende fouten tijdens de failover van een virtuele machine naar Azure. Om op te lossen, gebruikt u de stappen beschreven voor elke fout.

## <a name="failover-failed-with-error-id-28031"></a>Failover is mislukt met fout-ID 28031

Site Recovery kan niet een mislukte maken via de virtuele machine in Azure. Dit kan gebeuren vanwege een van de volgende redenen:

* Is er niet voldoende quotum beschikbaar voor het maken van de virtuele machine: U kunt het beschikbare quotum controleren door te gaan naar de abonnement -> Gebruik + quota. U kunt openen een [nieuwe ondersteuningsaanvraag](http://aka.ms/getazuresupport) om het quotum te verhogen.

* U probeert te failover-virtuele machines van de van verschillende groottefamilies in dezelfde beschikbaarheidsset. Zorg ervoor dat u dezelfde serie met grootten voor alle virtuele machines in dezelfde beschikbaarheidsset kiezen. Grootte wijzigen door naar reken- en netwerkinstellingen van de virtuele machine te gaan en voer vervolgens de failover opnieuw uit.

* Er is een beleid voor het abonnement dat wordt voorkomen het maken van een virtuele machine dat. Wijzig het beleid voor het maken van een virtuele machine toestaan en voer vervolgens de failover opnieuw uit.

## <a name="failover-failed-with-error-id-28092"></a>Failover is mislukt met fout-ID 28092

Site Recovery kan niet een netwerkinterface voor de mislukte maken via de virtuele machine. Zorg ervoor dat u beschikt over voldoende quotum beschikbaar voor het maken van netwerkinterfaces in het abonnement. U kunt het beschikbare quotum controleren door te gaan naar de abonnement -> Gebruik + quota. U kunt openen een [nieuwe ondersteuningsaanvraag](http://aka.ms/getazuresupport) om het quotum te verhogen. Als u beschikt over voldoende quotum, wordt dit wordt mogelijk een onregelmatige uitgeven, probeer het opnieuw. Als het probleem zich blijft, zelfs na nieuwe pogingen voordoen, laat u een opmerking aan het einde van dit document.  

## <a name="failover-failed-with-error-id-70038"></a>Failover is mislukt met fout-ID 70038

Site Recovery kan niet een mislukte maken via de klassieke virtuele machine in Azure. Dit kan gebeuren omdat:

* Een van de resources, zoals een virtueel netwerk dat is vereist voor de virtuele machine moet worden gemaakt, bestaat niet. Het virtuele netwerk zoals bepaald in de instellingen berekening en netwerk van de virtuele machine maken of wijzigen van de instelling voor een virtueel netwerk dat al bestaat en voer vervolgens de failover opnieuw uit.

## <a name="unable-to-connectrdpssh---vm-connect-button-grayed-out"></a>Kan geen verbinding maken/RDP/SSH - VM verbinding maken met knop uitgeschakeld

Als de **Connect** knop op de failover-VM in Azure is uitgeschakeld en u bent niet verbonden met Azure via een Express Route of Site-naar-Site VPN-verbinding, vervolgens

1. Ga naar **virtuele machine** > **netwerken**, klik op de naam van de netwerkinterface vereist.  ![de netwerkinterface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navigeer naar **Ip-configuraties**, klikt u op het naamveld van de vereiste IP-configuratie. ![IP-configuraties](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
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

Dit is normaal gesproken geen aanleiding en kan meestal worden genegeerd voor niet-geplande failovers. In het geval van een geplande failover, zorg ervoor dat de virtuele machine correct wordt afgesloten voordat u een failover en geef voldoende tijd voor in behandeling zijnde replicatie gegevens on-premises naar Azure worden verzonden. Gebruik vervolgens de **nieuwste** kiezen op de [Failover scherm](site-recovery-failover.md#run-a-failover) zodat alle in behandeling gegevens in Azure worden verwerkt in een herstelpunt, die vervolgens wordt gebruikt voor VM-failover.

## <a name="retaining-drive-letter-after-failover"></a>Stationsletter behouden na een failover
Als u wilt behouden de stationsletter op virtuele machines na een failover, kunt u instellen de **SAN-beleid** voor de virtuele machine on-premises naar **OnlineAll**. [Meer informatie](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).

## <a name="next-steps"></a>Volgende stappen
- Problemen oplossen [RDP-verbinding met Windows-VM](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Problemen oplossen [SSH-verbinding met Linux-VM](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Als u meer hulp nodig hebt, klikt u vervolgens boek uw query op [Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) of een opmerking toevoegen aan het einde van dit document. Er is een actieve community die zou het mogelijk om u te helpen.
