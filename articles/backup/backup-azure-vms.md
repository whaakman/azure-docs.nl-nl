---
title: "Maak een back-up van klassieke geïmplementeerde virtuele machines van Azure naar een back-upkluis | Microsoft Docs"
description: Back-up van uw virtuele machines met deze procedures voor back-up van virtuele machine van Azure detecteren en registreren.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-up van virtuele machine. back-up van virtuele machine; back-up en herstel na noodgevallen; VM-back-up
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: e1da8bce96078a43c656f84005cefc8bbe81c9e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a>Back-up van virtuele machines in Azure (klassieke portal)
> [!div class="op_single_selector"]
> * [Back-up van virtuele machines naar een Recovery Services-kluis](backup-azure-arm-vms.md)
> * [Back-up van virtuele machines naar Backup-kluis](backup-azure-vms.md)
>
>

Dit artikel vindt de procedures voor back-ups van een klassiek geïmplementeerd Azure virtuele machine (VM) naar een Backup-kluis. Er zijn een paar taken die u behandelen moet voordat u kunt back-up van een virtuele machine van Azure. Als u dit nog niet hebt gedaan, voert u de [vereisten](backup-azure-vms-prepare.md) aan uw omgeving voorbereiden voor back-ups van uw virtuele machines.

Zie voor meer informatie de artikelen op [plannen van uw back-upinfrastructuur VM in Azure](backup-azure-vms-introduction.md) en [virtuele Azure-machines](https://azure.microsoft.com/documentation/services/virtual-machines/).

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). Een back-upkluis kan alleen klassiek geïmplementeerde VM's beveiligen. U kunt geen Resource Manager geïmplementeerde VM's met een back-upkluis. Zie [Back-up van virtuele machines naar een Recovery Services-kluis](backup-azure-arm-vms.md) voor meer informatie over het werken met Recovery Services-kluizen.
>
>

Back-ups van virtuele machines in Azure omvat drie belangrijke stappen:

![Drie stappen voor het back-up van een virtuele machine van Azure IaaS](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> Het maken van back-ups van virtuele machines is een lokaal proces. Er kan geen virtuele machines in één regio Backup naar een back-upkluis in een andere regio. Daarom moet u een back-upkluis in elke Azure-regio maken wanneer er virtuele machines die wordt een back-up.
>
> [!IMPORTANT]
> Vanaf maart 2017 is het niet meer mogelijk om de klassieke portal te gebruiken voor het maken van back-upkluizen.
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Na 15 oktober 2017 kunt u PowerShell niet meer gebruiken voor het maken van back-upkluizen. **Per 1 november 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>

## <a name="step-1---discover-azure-virtual-machines"></a>Stap 1: virtuele machines van Azure detecteren
Nieuwe virtuele machines (VM's) toegevoegd aan het abonnement worden geïdentificeerd voordat u registreert, zodat het detectieproces uitvoeren. Er wordt een query uitgevoerd om de virtuele Azure-machines in het abonnement weer te geven, samen met aanvullende informatie zoals de naam van de cloudservice en de regio.

1. Aanmelden bij de [klassieke portal](http://manage.windowsazure.com/)
2. Klik in de lijst van Azure-services op **Recovery Services** openen van de lijst met kluizen back-up en herstel van de Site.
    ![Lijst van de kluis openen](./media/backup-azure-vms/choose-vault-list.png)
3. Selecteer in de lijst van de Backup-kluizen de kluis voor back-up van een virtuele machine.

    Als dit een nieuwe kluis de portal geopend met de **Quick Start** pagina.

    ![Geregistreerde items menu openen](./media/backup-azure-vms/vault-quick-start.png)

    Als de kluis eerder is geconfigureerd, wordt het meest recent gebruikte menu van de portal geopend.
4. Klik in het kluismenu (boven aan de pagina), **geregistreerde Items**.

    ![Geregistreerde items menu openen](./media/backup-azure-vms/vault-menu.png)
5. Selecteer in het menu **Type** **Azure virtuele machine**.

    ![Workload selecteren](./media/backup-azure-vms/discovery-select-workload.png)
6. Klik op **DISCOVER** onder aan de pagina.
    ![Detectieknop](./media/backup-azure-vms/discover-button-only.png)

    Het detectieproces kan enkele minuten in beslag nemen, terwijl er een tabelindeling van de virtuele machines wordt gemaakt. Onder in het scherm verschijnt een melding dat het proces wordt uitgevoerd.

    ![VM's detecteren](./media/backup-azure-vms/discovering-vms.png)

    De melding wijzigt zodra het proces is voltooid. Als u het detectieproces is niet gevonden voor de virtuele machines, controleert u eerst dat de virtuele machines bestaan. Als de virtuele machines bestaat, controleert u dat de virtuele machines zich in dezelfde regio bevinden als de back-upkluis. Als de virtuele machines bestaat en zich in dezelfde regio, zorg ervoor dat de virtuele machines zijn niet bij een back-upkluis is geregistreerd. Als een virtuele machine is toegewezen aan een back-upkluis is het niet beschikbaar moet worden toegewezen aan andere back-upkluizen.

    ![Detectie uitgevoerd](./media/backup-azure-vms/discovery-complete.png)

    Zodra u de nieuwe items hebt gevonden, gaat u naar stap 2 en registreren van uw virtuele machines.

## <a name="step-2---register-azure-virtual-machines"></a>Stap 2: Register virtuele machines in Azure
Registreert u een virtuele machine van Azure wilt koppelen aan de Azure Backup-service. Dit is meestal een eenmalige activiteit.

1. Navigeer naar de back-upkluis onder **Recovery Services** in de Azure-portal en klik vervolgens op **geregistreerde Items**.
2. Selecteer **Virtuele machine van Azure** in de vervolgkeuzelijst.

    ![Workload selecteren](./media/backup-azure-vms/discovery-select-workload.png)
3. Klik op **REGISTER** onder aan de pagina.
    ![Registratieknop](./media/backup-azure-vms/register-button-only.png)
4. Selecteer in het snelmenu **Items registreren** de virtuele machines die u wilt registreren. Als er twee of meer virtuele machines met dezelfde naam, gebruikt u de cloudservice onderscheid maken tussen deze.

   > [!TIP]
   > U kunt in meerdere virtuele machines tegelijkertijd registreren.
   >
   >

    Voor elke virtuele machine die u hebt geselecteerd wordt een taak gemaakt.
5. Klik op **Taak weergeven** in de melding om naar de pagina **Taken** te gaan.

    ![Taak registreren](./media/backup-azure-vms/register-create-job.png)

    De virtuele machine wordt ook in de lijst met geregistreerde items weergegeven, samen met de status van de bewerking voor de registratie.

    ![Status 1 registreren](./media/backup-azure-vms/register-status01.png)

    Wanneer de bewerking is voltooid, wordt de status aangepast in *Geregistreerd*.

    ![Registratie van status 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a>Stap 3: Azure virtuele machines beveiligen
U kunt nu een back-up en retentie beleid voor de virtuele machine instellen. Meerdere virtuele machines kunnen worden beveiligd met behulp van één actie beveiligen.

Azure Backup-kluizen gemaakt nadat mei 2015 worden geleverd met een standaardbeleid ingebouwd in de kluis. Dit standaardbeleid wordt geleverd met een standaard bewaartermijn van 30 dagen en een eenmaal dagelijks back-upschema.

1. Navigeer naar de back-upkluis onder **Recovery Services** in de Azure-portal en klik vervolgens op **geregistreerde Items**.
2. Selecteer **Virtuele machine van Azure** in de vervolgkeuzelijst.

    ![Workload selecteren in de portal](./media/backup-azure-vms/select-workload.png)
3. Klik op **PROTECT** onder aan de pagina.

    De **wizard Items beveiligen** wordt weergegeven. De wizard is alleen een lijst met virtuele machines die zijn geregistreerd en niet beveiligd. Selecteer de virtuele machines die u wilt beveiligen.

    Als er twee of meer virtuele machines met dezelfde naam, gebruikt u de cloudservice onderscheid maken tussen de virtuele machines.

   > [!TIP]
   > U kunt meerdere virtuele machines tegelijk beveiligen.
   >
   >

    ![Beveiliging op schaal configureren](./media/backup-azure-vms/protect-at-scale.png)

4. Kies een **back-upschema** back-up van de virtuele machines die u hebt geselecteerd. U kunt kiezen uit een bestaande set met beleidsregels of definieer een nieuwe.

    Aan elk back-upbeleid kunnen meerdere virtuele machines zijn gekoppeld. Echter, de virtuele machine kan alleen worden gekoppeld aan een beleid op elk gewenst moment in de tijd.

    ![Beveiligen met een nieuw beleid](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Een back-upbeleid bevat een bewaarschema voor de geplande back-ups. Als u een bestaande back-upbeleid selecteert, kunt u de opties in de volgende stap niet wijzigen.
   >
   >

5. Kies een **bewaartermijn** koppelen aan de back-ups.

    ![Beveiligen met flexibele bewaren](./media/backup-azure-vms/policy-retention.png)

    Het bewaarbeleid geeft aan hoe lang een back-up wordt bewaard. U kunt verschillende bewaarbeleidsregels opgeven op basis van het moment waarop de back-up wordt gemaakt. Een back-uppunt worden dagelijks uitgevoerd (die fungeert als een operationele herstelpunt) kan bijvoorbeeld 90 dagen worden bewaard. Ter vergelijking moet een back-uppunt genomen aan het einde van elk kwartaal (voor auditdoeleinden) mogelijk veel maanden of jaren worden bewaard.

    ![Er wordt een back-up van de virtuele machine gemaakt met een herstelpunt](./media/backup-azure-vms/long-term-retention.png)

    In deze voorbeeldafbeelding:

   * **Dagelijkse bewaarbeleid**: back-ups dagelijks die zijn opgeslagen voor 30 dagen.
   * **Wekelijkse bewaarbeleid**: back-ups die elke week op zondag 104 weken blijven behouden.
   * **Maandelijkse bewaarbeleid**: back-ups die op de laatste zondag van elke maand worden bewaard gedurende 120 maanden.
   * **Jaarlijks bewaarbeleid**: back-ups die op de eerste zondag van elke januari 99 jaar worden bewaard.

     Een taak is gemaakt voor het configureren van het beveiligingsbeleid en koppel de virtuele machines met dit beleid voor elke virtuele machine die u hebt geselecteerd.
6. De lijst weergeven van **beveiliging configureren** taken in het menu kluizen, klikt u op **taken** en selecteer **beveiliging configureren** van de **bewerking**filter.

    ![Beveiligingstaak configureren ](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a>Eerste back-up
Zodra de virtuele machine is beveiligd met een beleid, wordt deze weergegeven onder de **beveiligde Items** tabblad met de status van *beveiligd - (in behandeling van eerste back-up)*. Standaard is de eerste geplande back-up de *eerste back-up*.

Voor het activeren van de eerste back-up onmiddellijk na de configuratie van beveiliging:

1. Aan de onderkant van de **beveiligde Items** pagina, klikt u op **back-up nu**.

    De Azure Backup-service maakt een back-uptaak voor de eerste back-upbewerking.
2. Klik op het tabblad **Taken** om de lijst met taken weer te geven.

    ![Back-up wordt uitgevoerd](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> Tijdens de back-upbewerking wordt met de Azure Backup-service een opdracht verstrekt aan de back-upextensie in elke virtuele machine op het leegmaken van alle taken voor schrijven en een consistente momentopname.
>
>

Wanneer de eerste back-up is voltooid, wordt de status van de virtuele machine in de **beveiligde Items** tabblad *beveiligde*.

![Er wordt een back-up van de virtuele machine gemaakt met een herstelpunt](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a>Back-status en details weer te geven
Als beveiligd, verhoogt het aantal virtuele machines ook de **Dashboard** pagina overzicht. De **Dashboard** pagina bevat ook het aantal taken van de afgelopen 24 uur die waren *geslaagde*, hebben *mislukt*, en zijn *Bezig* . Op de **taken** pagina, gebruikt u de **Status**, **bewerking**, of **van** en **naar** menu's om de taken te filteren.

![Status van de back-up in dashboardpagina](./media/backup-azure-vms/dashboard-protectedvms.png)

Waarden in het dashboard worden elke 24 uur vernieuwd.

## <a name="troubleshooting-errors"></a>Het oplossen van problemen
Als u problemen tijdens het back-ups maken van uw virtuele machine, bekijk de [VM probleemoplossingsartikel](backup-azure-vms-troubleshoot.md) voor hulp.

## <a name="next-steps"></a>Volgende stappen
* [Uw virtuele machines beheren en controleren](backup-azure-manage-vms.md)
* [Virtuele machines herstellen](backup-azure-restore-vms.md)
