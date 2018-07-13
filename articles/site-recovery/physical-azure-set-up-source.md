---
title: Instellen van de bronomgeving (fysieke servers naar Azure) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u uw on-premises omgeving instelt om te beginnen met het repliceren van fysieke servers waarop Windows of Linux wordt uitgevoerd in Azure.
services: site-recovery
documentationcenter: ''
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: anoopkv
ms.openlocfilehash: 00b09db97e597521de5c73eeefab77b0dfa1304d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38671019"
---
# <a name="set-up-the-source-environment-physical-server-to-azure"></a>Instellen van de bronomgeving (fysieke server naar Azure)

Dit artikel wordt beschreven hoe u uw on-premises omgeving instelt om te beginnen met het repliceren van fysieke servers waarop Windows of Linux wordt uitgevoerd in Azure.

## <a name="prerequisites"></a>Vereisten

Het artikel wordt ervan uitgegaan dat u al hebt:
1. Een Recovery Services-kluis de [Azure-portal](http://portal.azure.com "Azure-portal").
3. Een fysieke computer waarop de configuratieserver.

### <a name="configuration-server-minimum-requirements"></a>Minimale vereisten voor configuratie-server
De volgende tabel bevat de minimale hardware, software en netwerkvereisten voor een configuratieserver.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Op HTTPS gebaseerde proxy-servers worden niet ondersteund door de configuratieserver.

## <a name="choose-your-protection-goals"></a>Uw beveiligingsdoelstellingen kiezen

1. In de Azure-portal, gaat u naar de **herstelservices** blade-kluizen en selecteer uw kluis.
2. In de **Resource** menu van de kluis, klikt u op **aan de slag** > **siteherstel** > **stap 1: infrastructuur voorbereiden**   >  **Beveiligingsdoel**.

    ![Doelstellingen kiezen](./media/physical-azure-set-up-source/choose-goals.png)
3. In **beveiligingsdoel**, selecteer **naar Azure** en **niet gevirtualiseerd/Overig**, en klik vervolgens op **OK**.

    ![Doelstellingen kiezen](./media/physical-azure-set-up-source/physical-protection-goal.png)

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

1. In **bron voorbereiden**, als u een configuratieserver, klikt u op geen **+ configuratieserver** een toe te voegen.

  ![Bron instellen](./media/physical-azure-set-up-source/plus-config-srv.png)
2. In de **-Server toevoegen** blade, controleert u of **configuratieserver** wordt weergegeven in **servertype**.
4. Download het installatiebestand van de geïntegreerde Setup van Site Recovery.
5. Download de registratiesleutel voor de kluis. Wanneer u geïntegreerde Setup uitvoert moet u de registratiesleutel. De sleutel blijft vijf dagen na het genereren ervan geldig.

    ![Bron instellen](./media/physical-azure-set-up-source/set-source2.png)
6. Voer op de computer die u als de configuratieserver gebruikt, **geïntegreerde Setup van Azure Site Recovery** voor het installeren van de configuratieserver, de processerver en de hoofddoelserver.

#### <a name="run-azure-site-recovery-unified-setup"></a>Voer Azure Site Recovery van geïntegreerde Setup

> [!TIP]
> De registratie van de configuratie mislukt als de tijd op de systeemklok van de computer meer dan vijf minuten af bij de lokale tijd is. Synchroniseren van uw systeemklok met een [tijdserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) voordat u de installatie start.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> De configuratieserver kan worden geïnstalleerd via een opdrachtregel. Zie voor meer informatie, [configuratie-server installeren met behulp van opdrachtregelprogramma's](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Algemene problemen

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Volgende stappen

Volgende stap bestaat uit [instellen van uw doelomgeving](physical-azure-set-up-target.md) in Azure.
