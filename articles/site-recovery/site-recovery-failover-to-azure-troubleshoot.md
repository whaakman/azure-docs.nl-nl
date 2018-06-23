---
title: Failover naar Azure fouten oplossen | Microsoft Docs
description: In dit artikel wordt beschreven hoe het oplossen van veelvoorkomende fouten in Azure worden niet via
services: site-recovery
documentationcenter: ''
author: ponatara
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/09/2018
ms.author: ponatara
ms.openlocfilehash: 838eac510fc17d56f808f541f4e205a279f63c56
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318888"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Fouten bij failover van een virtuele machine in Azure oplossen

U wordt een van de volgende fouten tijdens de failover van een virtuele machine in Azure. Voor het oplossen, kunt u de stappen beschreven voor elke fout.

## <a name="failover-failed-with-error-id-28031"></a>Failover is mislukt met fout-ID 28031

Site Recovery is niet een mislukte maken via de virtuele machine in Azure. Dit kan gebeuren vanwege een van de volgende redenen:

* Is er niet voldoende quotum beschikbaar voor het maken van de virtuele machine: U kunt de beschikbare quota controleren door te gaan naar de abonnement -> Gebruik + quota's. U kunt openen een [nieuw ondersteuningsverzoek](http://aka.ms/getazuresupport) het quotum te verhogen.

* U probeert te failover virtuele machines van verschillende grootte families in dezelfde beschikbaarheidsset. Zorg ervoor dat u dezelfde familie van de grootte voor alle virtuele machines in dezelfde beschikbaarheidsset kiezen. Grootte wijzigen door te gaan naar berekenings- en instellingen van de virtuele machine en probeer vervolgens de failover.

* Er is een beleid voor het abonnement dat voorkomt het maken van een virtuele machine. Wijzig het beleid voor het toestaan van het maken van een virtuele machine en probeer de failover.

## <a name="failover-failed-with-error-id-28092"></a>Failover is mislukt met fout-ID 28092

Site Recovery is niet een netwerkinterface voor de mislukte maken via de virtuele machine. Zorg ervoor dat er voldoende quotum beschikbaar voor het maken van netwerkinterfaces in het abonnement. U kunt de beschikbare quota controleren door te gaan naar de abonnement -> Gebruik + quota's. U kunt openen een [nieuw ondersteuningsverzoek](http://aka.ms/getazuresupport) het quotum te verhogen. Als er voldoende quotum, wordt dit wordt mogelijk een onregelmatige uitgeven, probeer het opnieuw. Als het probleem zich blijft zelfs na verschillende pogingen voordoen, laat u een opmerking aan het einde van dit document.  

## <a name="failover-failed-with-error-id-70038"></a>Failover is mislukt met fout-ID 70038

Site Recovery is niet een mislukte maken via de klassieke virtuele machine in Azure. Deze fout kan optreden omdat:

* Een van de resources, zoals een virtueel netwerk dat is vereist voor de virtuele machine moet worden gemaakt bestaat niet. Maken van het virtuele netwerk, zoals bepaald in de berekenings- en instellingen van de virtuele machine of wijzig de instelling voor een virtueel netwerk dat al bestaat en voer vervolgens de failover opnieuw uit.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Kan geen verbinding maken/RDP/SSH kunt uitvoeren naar de virtuele machine vanwege lichter gekleurd weergegeven de knop verbinding maken op de virtuele machine

Als de knop verbinden lichter gekleurd weergegeven en u niet verbonden bent met Azure via een Express Route of Site-naar-Site VPN-verbinding, klikt u vervolgens

1. Ga naar **virtuele machine** > **Networking**, klik op de naam van de vereiste netwerkinterface.  ![netwerk-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navigeer naar **IP-configuraties**, klikt u op het naamveld van de vereiste IP-configuratie. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Als u wilt inschakelen in openbaar IP-adres, klikt u op **inschakelen**. ![IP inschakelen](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Klik op **vereiste instellingen configureren** > **nieuw**. ![Nieuwe maken](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Voer de naam van het openbare adres, kiest u de standaardopties voor **SKU** en **toewijzing**, klikt u vervolgens op **OK**.
6. Nu u de wijzigingen opslaan, klikt u op **opslaan**.
7. Sluit de panelen en navigeer naar **overzicht** sectie van de virtuele machine verbinding/RDP.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-even-though-connect-button-is-available-not-grayed-out-on-the-virtual-machine"></a>Kan geen verbinding maken/RDP/SSH kunt uitvoeren naar de mislukte via de virtuele machine ook al verbinding maken met de knop beschikbaar is (geen lichter gekleurd weergegeven) op de virtuele machine

Controleer **opstarten diagnostics** op uw virtuele Machine en Controleer op fouten, zoals vermeld in dit artikel.

1. Als de virtuele machine niet gestart is, probeer failover wordt uitgevoerd naar een oudere herstelpunt.
2. Als de toepassing in de virtuele machine niet van probeer worden niet via een app-consistente herstelpunt is.
3. Als de virtuele machine verbonden met het domein is, Controleer dat die domeincontroller correct werkt. Dit kan worden gedaan door de opgegeven stappen hieronder.
    a. een nieuwe virtuele machine in hetzelfde netwerk maken

    b.  Zorg ervoor dat deze toevoegen aan hetzelfde domein waarop de mislukte via de virtuele machine actief wordt verwacht.

    c. Als de domeincontroller **niet** werkt goed, probeer de aanmelding bij de mislukte via de virtuele machine met behulp van een lokale administrator-account
4. Als u een aangepaste DNS-server gebruikt, zorg ervoor dat deze bereikbaar. Dit kan worden gedaan door de opgegeven stappen hieronder.
    a. Maak een nieuwe virtuele machine in hetzelfde netwerk en b. Controleer of de virtuele machine kunnen naamomzetting met de aangepaste DNS-Server

>[!Note]
>Inschakelen van een andere instelling dan diagnostische gegevens over opstarten vereist een Azure VM-Agent moet worden geïnstalleerd op de virtuele machine voordat de failover

## <a name="next-steps"></a>Volgende stappen

Als u meer hulp nodig hebt, moet u vervolgens uw query boeken op [Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) of laat een opmerking aan het einde van dit document. We hebben een actieve community dat moet kunnen om u te helpen.
