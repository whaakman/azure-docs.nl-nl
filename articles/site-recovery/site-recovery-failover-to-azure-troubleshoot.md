---
title: Problemen met failover naar Azure fouten oplossen | Microsoft Docs
description: In dit artikel worden manieren voor het oplossen van veelvoorkomende fouten in de failover wordt uitgevoerd naar Azure beschreven.
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
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: b7b5dcd88b6e4e09dd9beb21e83ef405df148115
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443381"
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

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Kan geen verbinding maken/RDP/SSH met de mislukte grijs via virtuele machine vanwege de knop verbinding maken op de virtuele machine

Als de knop verbinden wordt grijs weergegeven en u bent niet verbonden met Azure via een Express Route of Site-naar-Site VPN-verbinding, vervolgens

1. Ga naar **virtuele machine** > **netwerken**, klik op de naam van de netwerkinterface vereist.  ![de netwerkinterface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
1. Navigeer naar **Ip-configuraties**, klikt u op het naamveld van de vereiste IP-configuratie. ![IP-configuraties](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
1. Om in te schakelen openbaar IP-adres, klikt u op **inschakelen**. ![IP inschakelen](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
1. Klik op **vereiste instellingen configureren** > **nieuw**. ![Maak een nieuwe](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
1. Voer de naam van het openbare adres, kiest u de standaardopties voor **SKU** en **toewijzing**, klikt u vervolgens op **OK**.
1. Nu, om de wijzigingen hebt opgeslagen, klikt u op **opslaan**.
1. De deelvensters sluiten en vervolgens naar **overzicht** sectie van de virtuele machine verbinding maken/RDP.

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-even-though-connect-button-is-available-not-grayed-out-on-the-virtual-machine"></a>Kan geen verbinding maken/RDP/SSH met de mislukte via virtuele machine, ook al verbinding maken met de knop wordt beschikbaar (niet lichter gekleurd) op de virtuele machine

Controleer **diagnostische gegevens over opstarten** op uw virtuele Machine en Controleer op fouten die in dit artikel worden beschreven.

1. Als de virtuele machine niet is gestart, probeert Failover-overschakeling uitvoeren naar een ouder herstelpunt.
1. Als de toepassing in de virtuele machine niet actief, probeer failover wordt uitgevoerd naar een app-consistente herstelpunt is.
1. Als de virtuele machine toegevoegd aan een domein is, zorg ervoor dat domeincontroller correct werkt. Dit kan worden gedaan door het volgende op de hieronder opgegeven stappen.
    a. een nieuwe virtuele machine in hetzelfde netwerk maken

    b.  Zorg ervoor dat deze kan worden toegevoegd aan hetzelfde domein waarop de mislukte via virtuele machine wordt verwacht.

    c. Als de domeincontroller **niet** werking nauwkeurig, probeer bent aangemeld bij de mislukte via virtuele machine met behulp van een lokale administrator-account
1. Als u van een aangepaste DNS-server gebruikmaakt en vervolgens te controleren of deze bereikbaar is. Dit kan worden gedaan door het volgende op de hieronder opgegeven stappen.
    a. Maak een nieuwe virtuele machine in hetzelfde netwerk en b. Controleer of de virtuele machine kunnen naamomzetting met behulp van de aangepaste DNS-Server

>[!Note]
>Azure VM-Agent moet worden geïnstalleerd op de virtuele machine voordat de failover wordt uitgevoerd vereist elke andere instelling dan diagnostische gegevens over opstarten

## <a name="next-steps"></a>Volgende stappen

Als u meer hulp nodig hebt, klikt u vervolgens boek uw query op [Site Recovery-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) of een opmerking toevoegen aan het einde van dit document. Er is een actieve community die zou het mogelijk om u te helpen.
