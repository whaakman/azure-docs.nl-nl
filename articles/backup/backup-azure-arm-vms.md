---
title: Maak een back-up van Azure Virtual machines | Microsoft Docs
description: Detecteren, registreren en back-up van virtuele machines van Azure naar een recovery services-kluis.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-up van virtuele machine. back-up van virtuele machine; back-up en herstel na noodgevallen; back-up van arm vm
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 892a88a2bc9d492f8a3afe59c05b4729f4830e6d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="back-up-azure-virtual-machines-to-a-recovery-services-vault"></a>Back-ups maken van virtuele Azure-machines naar een Recovery Services-kluis

Dit artikel wordt uitgelegd hoe u back-up van Azure Virtual machines (geïmplementeerd met Resource Manager en Classic geïmplementeerd) naar een Recovery Services-kluis. Het merendeel van het werk voor een back-ups van virtuele machines is de voorbereiding. Voordat u kunt back-up of een virtuele machine te beveiligen, moet u de [vereisten](backup-azure-arm-vms-prepare.md) voorbereiden van uw omgeving voor het beveiligen van uw virtuele machines. Nadat u de vereisten hebt voltooid, kunt u de back-upbewerking om momentopnamen van uw virtuele machine te starten.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Zie voor meer informatie de artikelen op [plannen van uw back-upinfrastructuur VM in Azure](backup-azure-vms-introduction.md) en [virtuele Azure-machines](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-the-backup-job"></a>Activering van de back-uptaak
Het back-upbeleid gekoppeld aan de Recovery Services-kluis wordt gedefinieerd hoe vaak en wanneer de back-upbewerking wordt uitgevoerd. De eerste geplande back-up is standaard de eerste back-up. Totdat de eerste back-up plaatsvindt, wordt voor de status van de laatste back-up op de blade **Back-uptaken** de tekst **Waarschuwing (eerste back-up in behandeling)** weergegeven.

![Back-up in behandeling](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Tenzij de eerste back-up binnenkort wordt gemaakt, wordt aanbevolen dat u **Nu een back-up maken** uitvoert. De volgende procedure wordt gestart vanuit het kluisdashboard. Deze procedure fungeert voor de eerste back-uptaak wordt uitgevoerd nadat u alle vereisten hebt voltooid. Deze procedure is niet beschikbaar als de eerste back-uptaak al is uitgevoerd. De gekoppelde back-upbeleid bepaalt de volgende back-uptaak.  

De eerste back-uptaak uitvoeren:

1. Klik op het kluisdashboard op het getal onder **Back-upitems** of klik op de tegel **Back-upitems**. <br/>
  ![Het pictogram Instellingen](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  De blade **Back-upitems** wordt geopend.

  ![Back-upitems](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. Selecteer het item op de blade **Back-upitems**.

  ![Het pictogram Instellingen](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  De lijst **Back-upitems** wordt geopend. <br/>

  ![Geactiveerde back-uptaak](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. Klik in de lijst **Back-upitems** op het weglatingsteken **...**  om het contextmenu te openen.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Het contextmenu wordt weergegeven.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Klik in het contextmenu op **Nu back-up maken**.

  ![Contextmenu](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  De blade Nu back-up maken wordt geopend.

  ![toont de blade Nu back-up maken](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Klik op de blade Nu back-up maken op het kalenderpictogram en selecteer in de kalender de laatste dag dat dit herstelpunt wordt bewaard. Klik vervolgens op **Back-up**.

  ![de laatste dag instellen dat het herstelpunt van Nu back-up maken wordt bewaard](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Implementatiemeldingen laten u weten dat de back-uptaak is geactiveerd en dat u de voortgang van de taak op de pagina Back-uptaken kunt controleren. Afhankelijk van de grootte van de virtuele machine kan het maken van de eerste back-up even duren.

6. Als u de status van de eerste back-up wilt bekijken of volgen, klikt u op de tegel **Back-uptaken** van het kluisdashboard op **Bezig**.

  ![Tegel Back-uptaken](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  De blade Back-uptaken wordt geopend.

  ![Tegel Back-uptaken](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  Op de blade **Back-uptaken** kunt u de status van alle taken bekijken. Controleer of de back-uptaak voor de virtuele machine nog steeds bezig is of is voltooid. Wanneer de back-uptaak is voltooid, verandert de status in *Voltooid*.

  > [!NOTE]
  > Als onderdeel van de back-upbewerking wordt met de Azure Backup-service aan de back-upextensie in elke VM opdracht gegeven om alle schrijfbewerkingen leeg te maken en een consistente momentopname te maken.
  >
  >

## <a name="troubleshooting-errors"></a>Het oplossen van problemen
Als u problemen tijdens het back-ups maken van uw virtuele machine, Zie de [VM probleemoplossingsartikel](backup-azure-vms-troubleshoot.md) voor hulp.

## <a name="next-steps"></a>Volgende stappen
Nu u uw virtuele machine hebt beveiligd, gaat u naar de volgende artikelen voor meer informatie over de VM-beheertaken en het herstellen van virtuele machines.

* [Uw virtuele machines beheren en controleren](backup-azure-manage-vms.md)
* [Virtuele machines herstellen](backup-azure-arm-restore-vms.md)
