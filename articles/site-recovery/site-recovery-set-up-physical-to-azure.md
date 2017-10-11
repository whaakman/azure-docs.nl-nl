---
title: Instellen van de bronomgeving (fysieke servers naar Azure) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u uw on-premises omgeving instelt om te repliceren van fysieke servers waarop Windows of Linux wordt uitgevoerd in Azure.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: 49b9d2e21dbcb612828a25f21ed4382327d6f64c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-the-source-environment-physical-server-to-azure"></a>Instellen van de bronomgeving (fysieke server naar Azure)
> [!div class="op_single_selector"]
> * [VMware naar Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fysieke naar Azure](./site-recovery-set-up-physical-to-azure.md)

Dit artikel wordt beschreven hoe u uw on-premises omgeving instelt om te repliceren van fysieke servers waarop Windows of Linux wordt uitgevoerd in Azure.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat u al hebt:
1. Een Recovery Services-kluis de [Azure-portal](http://portal.azure.com "Azure-portal").
3. Een fysieke computer waarop de configuratieserver te installeren.

### <a name="configuration-server-minimum-requirements"></a>Minimale vereisten voor configuratie-server
De volgende tabel bevat de minimale hardware, software en netwerkvereisten voor een configuratieserver.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> HTTPS gebaseerde proxy-servers worden niet ondersteund door de configuratieserver.

## <a name="choose-your-protection-goals"></a>Uw beveiligingsdoelstellingen kiezen

1. In de Azure portal, gaat u naar de **Recovery Services** blade-kluizen en selecteer uw kluis.
2. In de **Resource** menu van de kluis, klikt u op **aan de slag** > **siteherstel** > **stap 1: infrastructuur voorbereiden**   >  **Beveiligingsdoel**.

    ![Doelstellingen kiezen](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. In **beveiligingsdoel**, selecteer **naar Azure** en **niet gevirtualiseerde/andere**, en klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

1. In **bron voorbereiden**, als u een configuratieserver, klikt u op geen **+ configuratieserver** een toe te voegen.

  ![Bron instellen](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. In de **Server toevoegen** blade, controleert u of **configuratieserver** wordt weergegeven in **servertype**.
4. Download het installatiebestand van de Site Recovery Unified Setup.
5. Download de kluisregistratiesleutel. Wanneer u Setup Unified uitvoert moet u de registratiesleutel. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Bron instellen](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. Voer op de computer die u als de configuratieserver **Azure Site Recovery Unified Setup** voor het installeren van de configuratieserver, de processerver en de hoofddoelserver.

#### <a name="run-azure-site-recovery-unified-setup"></a>Voer Azure Site Recovery Unified Setup

> [!TIP]
> Registratie van de configuratie-server mislukt als de tijd op de systeemklok van de computer afmeldt bij de lokale tijd meer dan vijf minuten. Synchroniseren van uw systeemklok met een [tijdserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) voordat u de installatie start.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> De configuratieserver kan worden geïnstalleerd via een opdrachtregel. Zie voor meer informatie [configuratie-server installeren met behulp van de opdrachtregelprogramma's](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Algemene problemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Volgende stappen

Volgende stap omvat [instellen van uw doelomgeving](./site-recovery-prepare-target-physical-to-azure.md) in Azure.
