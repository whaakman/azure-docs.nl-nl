---
title: Failback tijdens herstel na noodgeval met Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van verschillende typen failback en de beperkingen worden overwogen terwijl er een failover naar on-premises tijdens herstel na noodgeval met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: raynew
ms.openlocfilehash: 16233c9013e97a7db7ef799b79526abac4741612
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54318437"
---
# <a name="overview-of-failback"></a>Overzicht van failback

Nadat u hebt failover naar Azure als onderdeel van het herstelproces na noodgevallen, kunt u een failback naar uw on-premises site. Er zijn twee verschillende typen failback die mogelijk met Azure Site Recovery zijn: 

- Failback naar de oorspronkelijke locatie 
- Een failback uitvoeren naar een alternatieve locatie

Als de failover van een virtuele VMware-machine, kunt u een failback naar dezelfde bron on-premises virtuele machine als deze nog bestaat. In dit scenario worden alleen de wijzigingen terug gerepliceerd. In dit scenario wordt ook wel **oorspronkelijke locatie herstellen**. Als de on-premises virtuele machine niet bestaat, wordt het scenario is een **herstel naar alternatieve**.

> [!NOTE]
> U kunt alleen een failback naar de oorspronkelijke vCenter en de configuratieserver. U kunt geen implementeert een nieuwe configuratieserver en terug met behulp van het mislukken. U kan ook een nieuwe vCenter toevoegen aan de bestaande configuratie- en failback naar de nieuwe vCenter.

## <a name="original-location-recovery-olr"></a>Oorspronkelijke locatie herstellen
Als u ervoor een failback uitvoeren naar de oorspronkelijke virtuele machine kiest, moeten de volgende voorwaarden worden voldaan:

* Als de virtuele machine wordt beheerd door een vCenter-server, moet klikt u vervolgens het hoofddoel ESX-host hebben toegang tot gegevensopslag van de virtuele machine.
* Als de virtuele machine zich op een ESX-host, maar niet wordt beheerd door vCenter, moet de harde schijf van de virtuele machine zich in een gegevensopslag die toegang hebben tot de host van het hoofddoel.
* Als uw virtuele machine op een ESX-host is en geen gebruik maakt van vCenter, moet u detectie van de ESX-host van het hoofddoel voltooien voordat u opnieuw beveiligen. Dit geldt als u een te failback van fysieke servers wilt maken.
* U kunt een failback naar een virtuele storage area network (SAN) of een schijf die op basis van raw device mapping (RDM) als de schijven al bestaan en die zijn verbonden met de on-premises virtuele machine.

> [!IMPORTANT]
> Het is belangrijk om in te schakelen disk.enableUUID= waar zodat tijdens een failback wordt de Azure Site Recovery-service kan voor het identificeren van de oorspronkelijke VMDK op de virtuele machine waarop de wijzigingen in behandeling worden geschreven. Als deze waarde niet is ingesteld op TRUE zijn, klikt u vervolgens de service probeert te identificeren van de bijbehorende on-premises VMDK beste vermogen. Als de juiste VMDK niet wordt gevonden, wordt een extra schijf gemaakt en de gegevens bij die wordt geschreven.

## <a name="alternate-location-recovery-alr"></a>Herstellen op alternatieve locaties (ALR)
Als de on-premises virtuele machine niet bestaat voor de virtuele machine opnieuw beveiligen, wordt het scenario voor herstel van een alternatieve locatie genoemd. De werkstroom opnieuw beveiligen maakt de on-premises virtuele machine opnieuw. Dit wordt ook het downloaden van een volledige gegevens veroorzaken.

* Wanneer u een failback naar een alternatieve locatie, wordt de virtuele machine hersteld naar dezelfde ESX-host waarop de hoofddoelserver is geïmplementeerd. De gegevensopslag die wordt gebruikt voor het maken van de schijf worden dezelfde gegevensopslag die is geselecteerd bij de virtuele machine opnieuw beveiligen.
* U kunt niet terug alleen bij een virtuele machine-bestandssysteem (VMFS) of een virtueel SAN-gegevensopslag. Als u een RDM hebt, werkt opnieuw beveiligen en failback niet.
* Opnieuw beveiligen omvat een overdracht van grote hoeveelheden initiële gegevens dat wordt gevolgd door de wijzigingen. Dit proces bestaat omdat de virtuele machine niet on-premises bestaat. De volledige gegevens hebben terug worden gerepliceerd. Deze opnieuw beveiligen duurt ook langer dan een oorspronkelijke locatie herstellen.
* U kunt geen failback uitvoeren naar op basis van een RDM-schijven. Nieuwe virtuele machine-schijven (vmdk's) kunnen alleen worden gemaakt op een gegevensarchief VMFS/virtueel SAN.

> [!NOTE]
> Een fysieke computer, wanneer een failover naar Azure, kan geen worden back-ups maken alleen als een virtuele VMware-machine. Dit komt overeen met de dezelfde werkstroom als herstel van de alternatieve locatie. Zorg ervoor dat u ten minste één hoofddoelserver en de benodigde ESX/ESXi-hosts waaraan u moet een failback detecteren.

## <a name="next-steps"></a>Volgende stappen

Volg de stappen om uit te voeren de [failback-bewerking](vmware-azure-failback.md).

