---
title: Een processerver in Azure instellen voor VMware-VM en fysieke server failbacks met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een processerver in Azure, naar Azure-VM's naar VMware-failback instellen.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 20c3642bb9f9ad5089c37bb6da8562e570896cb4
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38308477"
---
# <a name="set-up-a-process-server-in-azure-for-failback"></a>Een processerver in Azure voor failback instellen

Nadat u een failover VMware-machines of fysieke servers naar Azure met [siteherstel](site-recovery-overview.md), kunt u een failover ze terug naar de on-premises site wanneer deze opnieuw te starten. Als u wilt uitvoeren van een failback, moet u het instellen van een tijdelijke processerver in Azure, voor het afhandelen van replicatie van Azure naar on-premises. Nadat de failback is voltooid, kunt u deze virtuele machine verwijderen.

## <a name="before-you-start"></a>Voordat u begint

Meer informatie over de [opnieuw beveiligen](vmware-azure-reprotect.md) en [failback](vmware-azure-failback.md) proces.

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-in-azure"></a>Een processerver in Azure implementeren

1. In de kluis > **Site Recovery-infrastructuur**> **beheren** > **configuratieservers**, selecteer de configuratieserver.
2. Klik in de pagina op **+ processerver**
3. In **toevoegen processerver** pagina en selecteer de processerver in Azure implementeren.
4. Geef de instellingen voor Azure, met inbegrip van het abonnement dat is gebruikt voor failover, een resourcegroep, de Azure-regio die wordt gebruikt voor failover en het virtuele netwerk waarin de Azure VM's zich bevinden. Als u meerdere Azure-netwerken gebruikt, moet u een processerver in elk.

  ![Proces het galerie-item toevoegen](./media/vmware-azure-set-up-process-server-azure/add-ps-page-1.png)

4. In **servernaam**, **gebruikersnaam**, en **wachtwoord**, Geef een naam voor de processerver en de referenties die beheerdersmachtigingen heeft op de server worden toegewezen.
5. Geef een opslagaccount moet worden gebruikt voor de server-VM-schijven, het subnet waarin de virtuele machine van de processerver geplaatst worden en de IP-adres dat wordt toegewezen wanneer de virtuele machine wordt gestart.
6. Klik op **OK** knop om te beginnen met het implementeren van de virtuele machine van de processerver.

>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>De processerver (die wordt uitgevoerd in Azure) registreren bij een configuratieserver (on-premises uitgevoerd)

Nadat de virtuele machine van de processerver actief is, moet u registreren bij de on-premises configuratieserver, als volgt:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]


