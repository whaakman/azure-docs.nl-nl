---
title: Over de Azure Site Recovery-configuratie, proces en hoofddoelservers | Microsoft Docs
description: In dit artikel biedt een overzicht van de configuratie, proces en hoofddoelservers gebruiken bij het instellen van herstel na noodgevallen van on-premises VMware-machines naar Azure met Azure Site Recovery
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 8b3815fc9dc44484779a70b51ebff4802265d53a
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417739"
---
# <a name="about-site-recovery-components-configuration-process-master-target"></a>Over Site Recovery-onderdelen (configuratie, proces, hoofddoel)

In dit artikel beschrijft de configuratie, proces en hoofddoelservers gebruikt bij het repliceren van virtuele VMware-machines en fysieke servers naar Azure met de [Site Recovery](site-recovery-overview.md) service.

## <a name="configuration-server"></a>Configuratieserver

Voor herstel na noodgevallen van on-premises VMware-machines en fysieke servers, moet u een Site Recovery on-premises configuratieserver geïmplementeerd.

**Instelling** | **Details** | **Koppelingen**
--- | --- | ---
**Onderdelen**  | De configuratie van server-machine voert alle on-premises Site Recovery-onderdelen, waaronder de configuratieserver, processerver en hoofddoelserver.<br/><br/> Bij het instellen van de configuratieserver, worden alle onderdelen automatisch geïnstalleerd. | [Lezen](vmware-azure-common-questions.md#configuration-server) de configuratieserver Veelgestelde vragen over.
**Rol** | De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie. | Meer informatie over de architectuur voor [VMware](vmware-azure-architecture.md) en [fysieke server](physical-azure-architecture.md) herstel na noodgevallen naar Azure.
**Vereisten voor VMware** | U moet installeren en uitvoeren van de configuratieserver als een on-premises, maximaal beschikbare VMware-VM voor herstel na noodgevallen van on-premises VMware-VM's. | [Meer informatie over](vmware-azure-deploy-configuration-server.md#prerequisites) de vereisten.
**VMware-implementatie** | Het is raadzaam dat u de configuratieserver met behulp van een gedownloade OVA-sjabloon implementeert. Deze methode biedt een eenvoudig manier voor het instellen van een configuratieserver die aan alle vereisten en voorwaarden voldoet.<br/><br/> Als voor een of andere reden lukt een VMware-VM met behulp van een OVA-sjabloon implementeren, kunt u de configuratie van server-machines handmatig, zoals hieronder beschreven voor noodherstel van fysieke machine instellen. | [Implementeer](vmware-azure-deploy-configuration-server.md#deployment-of-configuration-server-through-ova-template) met behulp van een OVA-sjabloon.
**Vereisten voor fysieke servers** | Voor herstel na noodgevallen voor on-premises fysieke servers implementeert u de configuratieserver handmatig. | [Meer informatie over](physical-azure-set-up-source.md#prerequisites) de vereisten.
**Fysieke server-implementatie** | Als deze kan niet worden geïnstalleerd als een VMware-VM, kunt u deze installeren op een fysieke server. | [Implementeer](physical-azure-set-up-source.md#set-up-the-source-environment) de configuratieserver handmatig.


## <a name="process-server"></a>Processerver

**Instelling** | **Details** | **Koppelingen**
--- | --- | ---
**Implementatie**  | Voor herstel na noodgevallen en replicatie van on-premises VMware-machines en fysieke servers, moet u een processerver on-premises. De processerver is standaard geïnstalleerd op de configuratieserver bij het implementeren. | [Meer informatie](vmware-azure-architecture.md?#architectural-components).
**Rol (on-premises** | -Ontvangt gegevens van replicatie van machines is ingeschakeld voor replicatie.<br/> -Optimaliseert replicatiegegevens met caching, compressie en codering, en verzendt ze naar Azure Storage.<br/> -Voert een push-installatie van de Site Recovery Mobility Service op de on-premises VMware-machines en fysieke servers die u wilt repliceren.<br/> -Voert automatische detectie van on-premises computers. | [Meer informatie](vmware-physical-azure-config-process-server-overview.md#process-server). 
**Rol (failback van Azure)** | Na de failover van uw on-premises site instellen u een processerver in Azure, als een Azure-VM voor het afhandelen van failback naar uw on-premises locatie.<br/><br/> De processerver in Azure is tijdelijk. De Azure-VM kan worden verwijderd nadat de failback is voltooid. | [Meer informatie](vmware-azure-set-up-process-server-azure.md).
**Schalen** | Voor grotere implementaties, on-premises kunt u aanvullende, scale-out processervers instellen. Extra servers scale-out-capaciteit, doordat ze de grotere aantallen repliceren van machines en grotere hoeveelheden replicatieverkeer.<br/><br/> U kunt machines verplaatsen tussen twee processervers, als u wilt laden saldo replicatieverkeer. | [Meer informatie](vmware-azure-set-up-process-server-scale.md),


## <a name="master-target-server"></a>Hoofddoelserver

Op de hoofddoelserver worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.

- Deze wordt standaard geïnstalleerd op de configuratieserver.
- U kunt een afzonderlijke hoofddoelserver voor failback toevoegen voor grote implementaties.


## <a name="next-steps"></a>Volgende stappen
- Controleer de [architectuur](vmware-azure-architecture.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers.
- Controleer de [vereisten en voorwaarden](vmware-physical-azure-support-matrix.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. 
