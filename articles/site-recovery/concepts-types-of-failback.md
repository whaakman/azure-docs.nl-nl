---
title: Failback in de Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van verschillende typen failback en waarschuwingen te worden beschouwd bij mislukte terug naar de on-premises met de Azure Site Recovery-service.
services: site-recovery
author: rajani-janaki-ram
manager: guaravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: rajanki
ms.openlocfilehash: 372a7867b47960338d7a1bf7e646fb9fffbe72e1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
ms.locfileid: "29803549"
---
# <a name="overview-of-failback"></a>Overzicht van failback

Nadat u hebt een failover uitvoeren naar Azure, kunt u terug naar uw on-premises site mislukt. Er zijn twee verschillende soorten failback die mogelijk met Azure Site Recovery zijn: 

- Failback naar de oorspronkelijke locatie 
- Failback naar een alternatieve locatie

Als de failover van een virtuele VMware-machine kunt u failback naar dezelfde bron lokale virtuele machine als deze nog bestaat. In dit scenario worden alleen de wijzigingen terug gerepliceerd. Dit scenario wordt ook wel **herstel oorspronkelijke locatie**. Als de on-premises virtuele machine niet bestaat, wordt het scenario is een **herstel naar alternatieve locaties**.

> [!NOTE]
> U kunt alleen een failback naar de oorspronkelijke vCenter en de configuratieserver. U kunt geen implementeren van een nieuwe configuratieserver of terug met behulp van het mislukken. U kunt ook een nieuwe vCenter toevoegen aan de bestaande configuratieserver en de failback naar de nieuwe vCenter.

## <a name="original-location-recovery-olr"></a>Herstel oorspronkelijke locatie (OLR)
Als u ervoor kiest de failback naar de oorspronkelijke virtuele machine, moeten de volgende voorwaarden worden voldaan:

* Als de virtuele machine wordt beheerd door een vCenter-server, moet toegang tot het gegevensarchief van de virtuele machine hebben op het hoofddoel ESX-host.
* Als de virtuele machine zich op een ESX-host, maar wordt niet beheerd door de vCenter, moet zich de harde schijf van de virtuele machine in een gegevensopslag die toegang het hoofddoel host tot.
* Als uw virtuele machine zich op een ESX-host en maakt geen gebruik van vCenter, moet u detectie van de ESX-host van het hoofddoel uitvoeren voordat u opnieuw beveiligen. Dit geldt als u bent terug fysieke servers te mislukken.
* U kunt niet naar een virtuele storage area network (vSAN) of een schijf die op basis van niet-gecodeerd apparaat (RDM) toewijzen als de schijven bestaan al en zijn verbonden met de on-premises virtuele machine.

> [!IMPORTANT]
> Het is belangrijk disk.enableUUID= waar inschakelen zodat tijdens de failback, de Azure Site Recovery-service is kunnen identificeren van de oorspronkelijke VMDK op de virtuele machine waarop de wijzigingen in behandeling worden geschreven. Als deze waarde niet is ingesteld op TRUE zijn, klikt u vervolgens de service probeert te identificeren van de bijbehorende lokale VMDK op basis van best effort. Als de juiste VMDK niet wordt gevonden, maakt u een extra schijf en de gegevens opgehaald bij die geschreven.

## <a name="alternate-location-recovery-alr"></a>Herstellen op alternatieve locaties (ALR)
Als u de on-premises virtuele machine bestaat niet voor de virtuele machine opnieuw te beveiligen, wordt het scenario voor het herstel van een alternatieve locatie genoemd. De werkstroom opnieuw beveiligen maakt de on-premises virtuele machine opnieuw. Ook wordt hierdoor het downloaden van een volledige gegevens.

* Wanneer u een failover naar een alternatieve locatie, wordt de virtuele machine wordt hersteld naar dezelfde ESX-host waarop de hoofddoelserver is geïmplementeerd. Het gegevensarchief die wordt gebruikt voor het maken van de schijf worden het hetzelfde gegevensarchief die is geselecteerd bij de virtuele machine opnieuw te beveiligen.
* U kunt niet weer alleen aan een virtuele machine file system (VMFS) of een virtueel SAN gegevensarchief. Als u een RDM hebt, werkt beveiligt en failback niet.
* Beveiligt, moet u een grote eerste gegevensoverdracht die wordt gevolgd door de wijzigingen. Dit proces bestaat omdat de virtuele machine niet on-premises bestaat. De volledige gegevens heeft weer worden gerepliceerd. Deze opnieuw beveiligen duurt ook langer dan een herstel van de oorspronkelijke locatie.
* U afkeuren terug naar schijven op basis van RDM niet. Nieuwe virtuele machine-schijven (VMDKs) kunnen alleen worden gemaakt op een gegevensopslag VMFS/virtueel SAN.

> [!NOTE]
> Een fysieke computer als een failover naar Azure, failover back alleen als een virtuele VMware-machine. Dit komt overeen met de dezelfde werkstroom als het herstel van alternatieve locatie. Zorg ervoor dat u ten minste één hoofddoelserver en de benodigde ESX/ESXi-hosts waaraan u moet een failback uit detecteren.

## <a name="next-steps"></a>Volgende stappen

Volg de stappen voor het uitvoeren van de [failback-bewerking](vmware-azure-failback.md).

