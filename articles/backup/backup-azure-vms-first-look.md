---
title: 'Eerste blik: een back-up maken van virtuele machines van Azure met een back-upkluis | Microsoft Docs'
description: 'Gebruik de klassieke portal om een back-up van virtuele Azure-machines te maken naar een back-upkluis. In deze zelfstudie worden alle fasen uitgelegd: het maken van de Backup-kluis, het registreren van de virtuele machines, het maken van een back-upbeleid en het uitvoeren van de eerste back-uptaak.'
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 3/10/2017
ms.author: markgal;
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 8883ff1601c521d05068452b1b58cadaee1a941f
ms.lasthandoff: 03/14/2017


---
# <a name="first-look-backing-up-azure-virtual-machines"></a>Eerste blik: een back-up maken van virtuele machines van Azure
> [!div class="op_single_selector"]
> * [Virtuele machines beveiligen met een Recovery Services-kluis](backup-azure-vms-first-look-arm.md)
> * [Virtuele machines van Azure beveiligen met een back-upkluis](backup-azure-vms-first-look.md)
>
>

In deze zelfstudie leert u hoe u een back-up maakt van een virtuele machine (VM) van Azure naar een back-upkluis in Azure. In dit artikel wordt het klassieke of Service Manager-implementatiemodel voor back-ups van virtuele machines beschreven. De volgende stappen gelden alleen voor back-upkluizen die zijn gemaakt in de klassieke portal. Voor nieuwe implementaties wordt u geadviseerd om het Resource Manager-model te gebruiken.

Zie [Eerste blik: virtuele machines beveiligen met een Recovery Services-kluis](backup-azure-vms-first-look-arm.md) voor meer informatie over back-ups van een virtuele machine naar een Recovery Services-kluis die deel uitmaakt van een resourcegroep.

Voor de volgende zelfstudie gelden de volgende vereisten:

* U hebt een VM gemaakt in uw Azure-abonnement.
* De VM heeft verbinding met openbare IP-adressen in Azure. Zie [Netwerkverbinding](backup-azure-vms-prepare.md#network-connectivity) voor meer informatie.


> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). Deze zelfstudie is van toepassing op virtuele machines die in de klassieke portal zijn gemaakt.
>
>

## <a name="create-a-backup-vault"></a>Een back-upkluis maken
Een back-upkluis is een entiteit waarmee alle back-ups en herstelpunten worden opgeslagen die in de loop van de tijd zijn gemaakt. De back-upkluis bevat ook het back-upbeleid dat wordt toegepast op de virtuele machines waarvan een back-up wordt gemaakt.

> [!IMPORTANT]
> Vanaf maart 2017 is het niet meer mogelijk om de klassieke portal te gebruiken voor het maken van back-upkluizen. Bestaande back-upkluizen worden nog wel ondersteund en het is mogelijk om [Azure PowerShell te gebruiken voor het maken van back-upkluizen](./backup-client-automation-classic.md#create-a-backup-vault). U wordt echter geadviseerd om voor alle implementaties Recovery Services-kluizen te maken, aangezien toekomstige verbeteringen alleen van toepassing zullen zijn op Recovery Services-kluizen.



## <a name="discover-and-register-azure-virtual-machines"></a>Virtuele machines van Azure detecteren en registreren
Voordat u de VM met een kluis registreert, moet u het detectieproces uitvoeren om nieuwe VM's weer te geven. Er wordt een lijst met virtuele machines in het abonnement weergegeven, samen met aanvullende informatie zoals de naam van de cloudservice en de regio.

1. Meld u aan bij de [klassieke Azure Portal](http://manage.windowsazure.com/)
2. Klik in de klassieke Azure Portal op **Recovery Services** om een lijst met Recovery Services-kluizen weer te geven.
    ![Workload selecteren](./media/backup-azure-vms-first-look/recovery-services-icon.png)
3. Selecteer in de lijst met kluizen de kluis voor de back-up van een VM.

    Wanneer u uw kluis selecteert, wordt de pagina **Quick Start** geopend
4. Klik in het kluismenu op **Geregistreerde items**.

    ![Workload selecteren](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. Selecteer in het menu **Type** **Azure virtuele machine**.

    ![Workload selecteren](./media/backup-azure-vms/discovery-select-workload.png)
6. Klik op **DISCOVER** onder aan de pagina.
    ![Detectieknop](./media/backup-azure-vms/discover-button-only.png)

    Het detectieproces kan enkele minuten in beslag nemen, terwijl er een tabelindeling van de virtuele machines wordt gemaakt. Onder in het scherm verschijnt een melding dat het proces wordt uitgevoerd.

    ![VM's detecteren](./media/backup-azure-vms/discovering-vms.png)

    De melding wijzigt zodra het proces is voltooid.

    ![Detectie uitgevoerd](./media/backup-azure-vms-first-look/discovery-complete.png)
7. Klik op **REGISTER** onder aan de pagina.
    ![Registratieknop](./media/backup-azure-vms-first-look/register-icon.png)
8. Selecteer in het snelmenu **Items registreren** de virtuele machines die u wilt registreren.

   > [!TIP]
   > U kunt in meerdere virtuele machines tegelijkertijd registreren.
   >
   >

    Voor elke virtuele machine die u hebt geselecteerd wordt een taak gemaakt.
9. Klik op **Taak weergeven** in de melding om naar de pagina **Taken** te gaan.

    ![Taak registreren](./media/backup-azure-vms/register-create-job.png)

    De virtuele machine wordt ook in de lijst met geregistreerde items weergegeven, samen met de status van de bewerking voor de registratie.

    ![Status 1 registreren](./media/backup-azure-vms/register-status01.png)

    Wanneer de bewerking is voltooid, wordt de status aangepast in *Geregistreerd*.

    ![Registratie van status 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>De VM-agent op de virtuele machine installeren
De Azure VM-agent moet worden geïnstalleerd op de virtuele Azure-machine om de Backup-extensie te kunnen gebruiken. Als uw VM is gemaakt vanuit de Azure-galerie, is de VM-agent al aanwezig op de VM. U kunt verder gaan met [Uw VM's beschermen](backup-azure-vms-first-look.md#create-the-backup-policy).

Als uw VM is gemigreerd van een on-premises datacenter, is de VM-agent waarschijnlijk niet op de VM geïnstalleerd. U moet de VM-agent op de virtuele machine installeren voordat u doorgaat met het beveiligen van de VM. Zie de [sectie VM-agent van het artikel Back-ups van VM's maken](backup-azure-vms-prepare.md#vm-agent) voor gedetailleerde stappen voor het installeren van de VM-agent.

## <a name="create-the-backup-policy"></a>Het back-upbeleid maken
Voordat u de eerste back-uptaak activeert, stelt u een schema in voor het maken van momentopnamen van de back-up. Het schema voor het maken van momentopnamen en de duur dat deze momentopnamen worden bewaard, vormen samen het back-upbeleid. De informatie over deze bewaarperiode is gebaseerd op het zogenaamde grootvader-vader-zoon-rotatieschema.

1. Navigeer naar de back-upkluis in **Recovery Services** in de klassieke Azure Portal en klik op **Geregistreerde items**.
2. Selecteer **Virtuele machine van Azure** in de vervolgkeuzelijst.

    ![Workload selecteren in de portal](./media/backup-azure-vms/select-workload.png)
3. Klik op **PROTECT** onder aan de pagina.
    ![Klik op Beveiligen](./media/backup-azure-vms-first-look/protect-icon.png)

    De **wizard Items beveiligen** verschijnt. Hierin worden *alleen* virtuele machines weergegeven die zijn geregistreerd en niet zijn beveiligd.

    ![Beveiliging op schaal configureren](./media/backup-azure-vms/protect-at-scale.png)
4. Selecteer de virtuele machines die u wilt beveiligen.

    Als er twee of meer virtuele machines met dezelfde naam zijn, gebruikt u de cloudservice om onderscheid te maken tussen de virtuele machines.
5. Selecteer in het menu **Beveiliging configureren** een bestaand beleid of maak een nieuw beleid om de virtuele machines die u hebt geïdentificeerd, te beveiligen.

    Nieuwe back-upkluizen hebben een standaardbeleid dat is gekoppeld aan de kluis. Dit beleid maakt dagelijks elke avond een momentopname en de momentopnamen worden dertig dagen bewaard. Aan elk back-upbeleid kunnen meerdere virtuele machines zijn gekoppeld. Er kan echter maar één beleid tegelijk aan een virtuele machine zijn gekoppeld.

    ![Beveiligen met een nieuw beleid](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Een back-upbeleid bevat een bewaarschema voor de geplande back-ups. Als u een bestaand back-upbeleid selecteert, kunt u de opties in de volgende stap niet wijzigen.
   >
   >
6. Geef bij **Bewaartermijn** het dagelijkse, wekelijkse, maandelijkse en jaarlijkse bereik voor de specifieke back-uppunten op.

    ![Er wordt een back-up van de virtuele machine gemaakt met een herstelpunt](./media/backup-azure-vms/long-term-retention.png)

    Het bewaarbeleid geeft aan hoe lang een back-up wordt bewaard. U kunt verschillende bewaarbeleidsregels opgeven op basis van het moment waarop de back-up wordt gemaakt.
7. Klik op **Taken** om de lijst met taken voor **Beveiliging configureren** weer te geven.

    ![Beveiligingstaak configureren ](./media/backup-azure-vms/protect-configureprotection.png)

    Nu dat u het beleid hebt ingesteld, gaat u naar de volgende stap en voert u de eerste back-up uit.

## <a name="initial-backup"></a>Eerste back-up
Als een virtuele machine is beveiligd met een beleid, kunt u deze relatie bekijken op het tabblad **Beveiligde items**. Totdat de eerste back-up wordt uitgevoerd, staat de **beveiligingsstatus** op **Beveiligd - (eerste back-up in behandeling)**. Standaard is de eerste geplande back-up de *eerste back-up*.

![Back-up in behandeling](./media/backup-azure-vms-first-look/protection-pending-border.png)

Als u de eerste back-up nu wilt starten, doet u het volgende:

1. Klik op de pagina **Beveiligde items** op pagina **Backup Now (Nu back-up maken)** onder aan de pagina.
    ![Pictogram van Backup Now (Nu back-up maken)](./media/backup-azure-vms-first-look/backup-now-icon.png)

    De Azure Backup-service maakt een back-uptaak voor de eerste back-upbewerking.
2. Klik op het tabblad **Taken** om de lijst met taken weer te geven.

    ![Back-up wordt uitgevoerd](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Wanneer de eerste back-up is voltooid, wordt de status van de virtuele machine in het tabblad **Beveiligde items** gewijzigd in *Beveiligd*.

    ![Er wordt een back-up van de virtuele machine gemaakt met een herstelpunt](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > Het maken van back-ups van virtuele machines is een lokaal proces. U kunt geen back-ups maken van virtuele machines als de back-upkluis zich niet in dezelfde regio bevindt. Voor elke Azure-regio met VM's waarvan een back-up moet worden gemaakt, moet ten minste één back-upkluis in deze regio worden gemaakt.
   >
   >

## <a name="next-steps"></a>Volgende stappen
Nu u een back-up hebt gemaakt van een VM, zijn er verschillende volgende stappen die van belang kunnen zijn. De meeste logische stap is om uzelf vertrouwd te maken met het herstellen van gegevens op een VM. Er zijn ook beheertaken waarmee u leert hoe u uw gegevens veilig kunt houden en de kosten kunt minimaliseren.

* [Uw virtuele machines beheren en controleren](backup-azure-manage-vms.md)
* [Virtuele machines herstellen](backup-azure-restore-vms.md)
* [Hulp bij het oplossen van problemen](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a>Vragen?
Als u vragen hebt of als er een functie is die u graag opgenomen zag worden, [stuurt u ons feedback](http://aka.ms/azurebackup_feedback).

