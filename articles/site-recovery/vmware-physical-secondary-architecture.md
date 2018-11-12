---
title: Architectuur voor noodherstel van VMware/fysieke server naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van de onderdelen en architectuur die worden gebruikt tijdens het herstel na noodgevallen van on-premises VMware-machines of fysieke Windows/Linux-servers naar een secundaire VMware-site met Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: raynew
ms.openlocfilehash: 77387b52003923dffce7519df9cca47fad8f6e6c
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007430"
---
# <a name="architecture-for-vmwarephysical-server-replication-to-a-secondary-on-premises-site"></a>Architectuur voor VMware-/ fysieke servers repliceren naar een secundaire on-premises site

Dit artikel beschrijft de architectuur en processen die worden gebruikt bij het instellen van disaster recovery-replicatie, failover en herstel van on-premises VMware-machines (VM's) of fysieke Windows/Linux-servers naar een secundaire VMware site met [Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Architectuuronderdelen

**Onderwerp** | **Onderdeel** | **Details**
--- | --- | ---
**Azure** | U implementeert dit scenario met InMage Scout. | Voor het gebruik van InMage Scout hebt u een Azure-abonnement nodig.<br/><br/> Nadat u een Recovery Services-kluis hebt gemaakt, downloadt u InMage Scout en installeert u de nieuwste updates om de implementatie in te stellen.
**Processerver** | Bevindt zich op de primaire site | U implementeert de processerver voor het afhandelen van caching, compressie en gegevensoptimalisatie.<br/><br/> Hier wordt ook de push-installatie van de Unified Agent afgehandeld naar computers die u wilt beveiligen.
**Configuratieserver** | Bevindt zich op de secundaire site | De configuratieserver dient om uw implementatie te beheren, te configureren en te bewaken met behulp van de beheerwebsite of de vContinuum-console.
**vContinuum-server** | Optioneel. Deze wordt geïnstalleerd op dezelfde locatie als de configuratieserver. | De vContinuum-server biedt een console voor het beheren en controleren van de beveiligde omgeving.
**Hoofddoelserver** | Bevindt zich op de secundaire site | De hoofddoelserver bevat gerepliceerde gegevens. De hoofddoelserver ontvangt gegevens van de processerver, maakt een replicamachine op de secundaire site en bevat de gegevensretentiepunten.<br/><br/> Het aantal hoofddoelservers dat u nodig hebt, is afhankelijk van het aantal machines dat u wilt beveiligen.<br/><br/> Als u failback wilt uitvoeren naar de primaire site, hebt u daar ook een hoofddoelserver nodig. De Unified Agent is geïnstalleerd op deze server.
**VMware ESX/ESXi- en vCenter-server** |  Virtuele machines worden op ESX-/ESXi-hosts gehost. Hosts worden beheerd via een vCenter-server | U hebt een VMware-infrastructuur nodig om virtuele VMware-machines te repliceren.
**VM's/fysieke servers** |  De Unified Agent wordt geïnstalleerd op virtuele VMware-machines of fysieke servers die u wilt repliceren. | De agent fungeert als communicatieprovider tussen alle onderdelen.

### <a name="replication-process"></a>Replicatieproces

1. U stelt de onderdeelservers in op elke site (configuratie, proces, hoofddoel) en installeert de Unified Agent op de machines die u wilt repliceren.
2. Na de initiële replicatie verzendt de agent op elke machine deltareplicatiewijzigingen naar de processerver.
3. De processerver optimaliseert de gegevens en brengt deze over naar de hoofddoelserver op de secundaire site. De configuratieserver beheert het replicatieproces.

**Afbeelding 6: replicatie van VMware naar VMware**

![VMware naar VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="next-steps"></a>Volgende stappen

[Instellen van](vmware-physical-secondary-disaster-recovery.md) herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar een secundaire site.
