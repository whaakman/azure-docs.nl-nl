---
title: Instellen van de bron en doel voor VMware-replicatie naar Azure met Azure Site Recovery | Microsoft Docs
description: Geeft een overzicht van de stappen voor het instellen van de bron en doel-instellingen voor de replicatie van virtuele VMware-machines naar Azure storage met Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 94b629a62c3a54eee69ee397b2f27e3f20b753d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-vmware-replication-to-azure"></a>Stap 8: Instellen van de bron en doel voor VMware-replicatie naar Azure

Dit artikel wordt beschreven hoe u de bron en doel-instellingen configureren wanneer de lokale virtuele VMware-machines repliceren naar Azure, met behulp van de [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.

Opmerkingen en vragen plaatsen onder aan dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

Instellen van de configuratieserver, registreert u dit in de kluis en VM's detecteren.

1. Klik op **Site Recovery** > **stap 1: infrastructuur voorbereiden** > **bron**.
2. Als u een configuratieserver geen hebt, klikt u op **+ configuratieserver**.
3. In **Server toevoegen**, controleert u of **configuratieserver** wordt weergegeven in **servertype**.
4. Download het installatiebestand van de Site Recovery Unified Setup.
5. Download de kluisregistratiesleutel. U moet dit wanneer u Unified Setup uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.

   ![Bron instellen](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-the-configuration-server-in-the-vault"></a>De configuratieserver in de kluis registreren

Het volgende doen voordat u start vervolgens Setup Unified om te installeren van de configuratieserver, de processerver en de hoofddoelserver.
    - Een video-overzicht

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Op de configuratieserver VM Zorg ervoor dat de systeemklok is gesynchroniseerd met een [tijdserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Deze moet overeenkomen. Als het op de voorgrond 15 minuten of achter de installatie mislukken.
    - Voer setup uit als lokale beheerder op de configuratieserver VM.
    - Zorg ervoor dat TLS 1.0 op de virtuele machine is ingeschakeld.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> De configuratieserver kan ook worden geïnstalleerd [vanaf de opdrachtregel](http://aka.ms/installconfigsrv).



## <a name="connect-to-vmware-servers"></a>Verbinding maken met VMware-servers

U moet verbinding maken met de VMware vCenter-Server of vSphere ESXi-hosts met Site Recovery zodat Azure Site Recovery voor het detecteren van virtuele machines die worden uitgevoerd in uw on-premises omgeving. Let op het volgende voordat u begint:

- Als u de vCenter-server of vSphere hosts Site Recovery met een account zonder beheerdersrechten op de server toevoegt, moet het account deze bevoegdheden ingeschakeld:
    - Datacenter, gegevensopslag, map, Host, netwerk, Resource, virtuele machine, vSphere gedistribueerde Switch.
    - De vCenter-server moet opslag weergaven machtigingen.
- Wanneer u VMware-servers aan de Site Recovery toevoegen, duurt het kan 15 minuten of langer voordat ze worden weergegeven in de portal.

### <a name="add-the-account-for-automatic-discovery"></a>Voeg het account voor automatische detectie

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a>Een verbinding instellen

Verbinding met servers als volgt:

1. Selecteer **+ vCenter** starten gebruikmaken van een VMware vCenter-server of een VMware vSphere ESXi-host.
2. Geef in **vCenter toevoegen** een beschrijvende naam op voor de vSphere-host of vCenter-server, en geef vervolgens het IP-adres of de FQDN op voor de server.
3. Behoud poort 443 tenzij de VMware-servers zijn geconfigureerd om te luisteren naar aanvragen van een andere poort. Selecteer het account dat is verbonden met de VMware vCenter-of vSphere ESXi-server. Klik op **OK**.
4. Site Recovery maakt verbinding met de VMware-servers met de opgegeven instellingen en virtuele machines worden gedetecteerd.

> [!NOTE]
> Als u een server of de host met een account dat geen administrator-bevoegdheden op de server met vCenter of host toevoegt, ervoor zorgen dat het account deze ingeschakeld heeft: Datacenter, gegevensopslag, map, Host, netwerk, Resource, virtuele machine en vSphere gedistribueerde overschakelen. De VMware vCenter-server moet bovendien de bevoegdheid opslag weergaven is ingeschakeld.


## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Voordat u de doelomgeving instellen, zorg er dan voor dat u hebt een Azure-opslagaccount en een virtueel netwerk instellen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Opgeven of de doel-implementatiemodel is Resource Manager gebaseerde of klassieke.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

   ![doel](./media/vmware-walkthrough-source-target/gs-target.png)
4. Als u een opslagaccount of een netwerk dat nog niet hebt gemaakt, klikt u op **+ opslagaccount** of **+ netwerk**, voor het maken van een Resource Manager-account of inline-netwerk.

## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 9: een replicatiebeleid instellen](vmware-walkthrough-replication.md)
