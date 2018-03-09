---
title: Instellen van een processerver in Azure voor VMware-VM en failback van de fysieke server met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe een processerver in Azure, voor failback virtuele Azure-machines naar VMware instellen.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 9d9270d8c6d2ffc5e42dfc6f94818fdace89bfb5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="set-up-a-process-server-in-azure-for-failback"></a>Instellen van een processerver in Azure voor failback

Nadat u een VMware-machines of fysieke servers naar Azure worden verkregen met failover [siteherstel](site-recovery-overview.md), u kunt failback ervan naar de lokale site wanneer deze opnieuw actief is. U moet een failback uit, instellen van een tijdelijke processerver in Azure, voor het afhandelen van replicatie van Azure met on-premises. Nadat de failback is voltooid, kunt u deze virtuele machine verwijderen.

## <a name="before-you-start"></a>Voordat u begint

Meer informatie over de [beveiligingspoging](vmware-azure-reprotect.md) en [failback](vmware-azure-failback.md) proces.

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-in-azure"></a>Een processerver in Azure implementeren

1. In de kluis > **Site Recovery-infrastructuur**> **Mnaage** > **configuratieservers**, selecteer de configuratieserver.
2. Klik op de serverpagina **+ processerver**
3. In **toevoegen processerver** pagina en selecteer de processerver in Azure implementeren.
4. De Azure-instellingen opgeven, met inbegrip van het abonnement dat u gebruikt voor failover, een resourcegroep, de Azure-regio voor failover en het virtuele netwerk waarin de Azure VM's zich bevinden. Als u meerdere Azure-netwerken gebruikt, moet u een processerver in elk criterium.
5. In **servernaam**, **gebruikersnaam**, en **wachtwoord**, Geef een naam voor de processerver en de referenties die worden toegewezen beheerdersmachtigingen op de server.
6. Geef een opslagaccount moet worden gebruikt voor de VM-schijven van de server, het subnet waarin de processerver VM zich bevindt en de server IP-adres dat wordt toegewezen wanneer de virtuele machine wordt gestart.
7. Klik op **OK** knop om te beginnen met het implementeren van de processerver VM.

>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>Registreren van de processerver (uitgevoerd in Azure) naar een configuratie-Server (met lokale)

Nadat de virtuele machine van de processerver actief en werkend is, moet u als volgt aan de lokale configuratie-server te registreren:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]


