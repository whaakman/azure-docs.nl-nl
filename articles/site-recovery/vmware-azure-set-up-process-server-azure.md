---
title: Instellen van een processerver in Azure voor VMware-VM en failback van de fysieke server met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe een processerver in Azure, voor failback virtuele Azure-machines naar VMware instellen.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 06/10/2018
ms.author: raynew
ms.openlocfilehash: 1a6a1276d1a63971fba68dc868aa56ce687a6cc2
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300919"
---
# <a name="set-up-a-process-server-in-azure-for-failback"></a>Instellen van een processerver in Azure voor failback

Nadat u een VMware-machines of fysieke servers naar Azure worden verkregen met failover [siteherstel](site-recovery-overview.md), u kunt failback ervan naar de lokale site wanneer deze opnieuw actief is. U moet een failback uit, instellen van een tijdelijke processerver in Azure, voor het afhandelen van replicatie van Azure met on-premises. Nadat de failback is voltooid, kunt u deze virtuele machine verwijderen.

## <a name="before-you-start"></a>Voordat u begint

Meer informatie over de [beveiligingspoging](vmware-azure-reprotect.md) en [failback](vmware-azure-failback.md) proces.

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-in-azure"></a>Een processerver in Azure implementeren

1. In de kluis > **Site Recovery-infrastructuur**> **beheren** > **configuratieservers**, selecteer de configuratieserver.
2. Klik op de serverpagina **+ processerver**
3. In **toevoegen processerver** pagina en selecteer de processerver in Azure implementeren.
4. De Azure-instellingen opgeven, met inbegrip van het abonnement dat u gebruikt voor failover, een resourcegroep, de Azure-regio voor failover en het virtuele netwerk waarin de Azure VM's zich bevinden. Als u meerdere Azure-netwerken gebruikt, moet u een processerver in elk criterium.

  ![Proces server galerie-item toevoegen](./media/vmware-azure-set-up-process-server-azure/add-ps-page-1.png)

4. In **servernaam**, **gebruikersnaam**, en **wachtwoord**, Geef een naam voor de processerver en de referenties die worden toegewezen beheerdersmachtigingen op de server.
5. Geef een opslagaccount moet worden gebruikt voor de VM-schijven van de server, het subnet waarin de processerver VM zich bevindt en de server IP-adres dat wordt toegewezen wanneer de virtuele machine wordt gestart.
6. Klik op **OK** knop om te beginnen met het implementeren van de processerver VM.

>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Registreren van de processerver (uitgevoerd in Azure) naar een configuratie-Server (met lokale)

Nadat de virtuele machine van de processerver actief en werkend is, moet u als volgt aan de lokale configuratie-server te registreren:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]


