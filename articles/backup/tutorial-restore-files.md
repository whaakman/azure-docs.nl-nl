---
title: Bestanden herstellen naar een virtuele machine met Azure Backup | Microsoft Docs
description: Informatie over het bestandsniveau herstelacties uitvoeren op een Azure-VM met back-up en herstelservices.
services: backup, virtual-machines
documentationcenter: virtual-machines
author: markgalioto
manager: carmonm
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/29/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e1bbae56b70c50fcf691db47efd9dc587686e7da
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="restore-files-to-a-virtual-machine-in-azure"></a>Bestanden herstellen met een virtuele machine in Azure
Azure Backup maakt herstelpunten die zijn opgeslagen in een geografisch redundante recovery kluizen. Wanneer u vanaf een herstelpunt herstelt, kunt u de hele virtuele machine of de afzonderlijke bestanden terugzetten. Dit artikel wordt uitgelegd hoe u kunt afzonderlijke bestanden terug te zetten. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Lijst en selecteer herstelpunten
> * Verbinding maken met een herstelpunt voor een virtuele machine
> * Bestanden herstellen vanaf een herstelpunt

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.18 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Vereisten
Deze zelfstudie vereist een Linux VM die is beveiligd met Azure Backup. Om te simuleren een bestanden per ongeluk verwijderen en het herstelproces, verwijdert u een pagina uit een webserver. Als u een Linux VM die wordt uitgevoerd een webserver en is beveiligd met Azure Backup moet, Zie [Back-up van een virtuele machine in Azure met de CLI](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Overzicht van Backup
Wanneer Azure een back-up begint, maakt de Backup-extensie op de virtuele machine een momentopname punt in tijd. De Backup-extensie is geïnstalleerd op de virtuele machine wanneer de eerste back-up wordt aangevraagd. Azure Backup kan ook een momentopname van de onderliggende opslag als de virtuele machine niet wordt uitgevoerd wanneer de back-up plaatsvindt.

Standaard heeft Azure Backup een bestand system consistente back-up. Zodra de Azure Backup maakt de momentopname, worden de gegevens worden overgedragen naar de Recovery Services-kluis. Als u wilt optimaliseren, Azure Backup identificeert en alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up overdraagt.

Wanneer de overdracht van gegevens voltooid is, wordt de momentopname is verwijderd en een herstelpunt wordt gemaakt.


## <a name="delete-a-file-from-a-vm"></a>Een bestand verwijderen uit een virtuele machine
Als u per ongeluk verwijdert of wijzigingen in een bestand aanbrengen, kunt u afzonderlijke bestanden terugzetten vanaf een herstelpunt. Dit proces kunt u bladeren in de bestanden die een back-up in een herstelpunt maken en terugzetten van alleen de bestanden die u nodig hebt. In dit voorbeeld wordt een bestand verwijderen uit een webserver ter illustratie van het herstelproces op bestandsniveau.

1. Verkrijgen voor verbinding met uw virtuele machine, het IP-adres van uw virtuele machine met [az vm weergeven](/cli/azure/vm?view=azure-cli-latest#az_vm_show):

     ```azurecli-interactive
     az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
     ```

2. Om te bevestigen dat de website momenteel werkt, open een webbrowser naar het openbare IP-adres van uw virtuele machine. Laat het browservenster geopend.

    ![Standaard NGINX-webpagina](./media/tutorial-restore-files/nginx-working.png)

3. Verbinding maken met uw virtuele machine met SSH. Vervang *publicIpAddress* met het openbare IP-adres dat u hebt verkregen in een vorige opdracht:

    ```bash
    ssh publicIpAddress
    ```

4. Verwijderen van de standaardpagina van de webserver op */var/www/html/index.nginx-debian.html* als volgt:

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```

5. Vernieuw de webpagina in uw webbrowser. De website niet langer de pagina wordt geladen zoals weergegeven in het volgende voorbeeld:

    ![NGINX-website niet meer standaardpagina wordt geladen](./media/tutorial-restore-files/nginx-broken.png)

6. Sluit de SSH-sessie op de virtuele machine als volgt:

    ```bash
    exit
    ```


## <a name="generate-file-recovery-script"></a>Bestand herstel script genereren
Azure Backup biedt uw om bestanden te herstellen, een script uit te voeren op de virtuele machine die verbinding het herstelpunt als een lokale schijf maakt. U kunt deze lokaal station bladeren, bestanden terugzetten op de virtuele machine zelf en Verbreek de verbinding met het herstelpunt. Azure Backup blijft back-up van uw gegevens op basis van het toegewezen beleid voor planning en te bewaren.

1. Lijst van herstelpunten voor de virtuele machine, gebruik [az back-recoverypoint lijst](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). In dit voorbeeld wordt de meest recente herstelpunt selecteren voor de virtuele machine met de naam *myVM* die wordt beveiligd *myRecoveryServicesVault*:

    ```azurecli-interactive
    az backup recoverypoint list \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --query [0].name \
        --output tsv
    ```

2. Gebruik om het script dat verbinding maakt of koppelt, het herstelpunt dat aan uw VM [az back-up terugzetten bestanden koppelpunt rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_mount_rp). Het volgende voorbeeld wordt het script voor de virtuele machine met de naam opgehaald *myVM* die wordt beveiligd *myRecoveryServicesVault*.

    Vervang *myRecoveryPointName* met de naam van het herstelpunt dat u hebt verkregen in de voorgaande opdracht:

    ```azurecli-interactive
    az backup restore files mount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

    Het script wordt gedownload en een wachtwoord wordt weergegeven, zoals in het volgende voorbeeld:

    ```
    File downloaded: myVM_we_1571974050985163527.sh. Use password c068a041ce12465
    ```

3. Gebruiken om het script te zetten naar uw virtuele machine, Secure kopiëren (SCP). Geef de naam van het gedownloade script en vervang *publicIpAddress* met het openbare IP-adres van uw virtuele machine. Zorg ervoor dat u de afsluitende `:` aan het einde van het SCP opdracht als volgt:

    ```bash
    scp myVM_we_1571974050985163527.sh 52.174.241.110:
    ```


## <a name="restore-file-to-your-vm"></a>Bestand met uw virtuele machine herstellen
U kunt nu verbinding maken met het herstelpunt en bestanden herstellen met het herstel script gekopieerd naar uw virtuele machine.

1. Verbinding maken met uw virtuele machine met SSH. Vervang *publicIpAddress* met het openbare IP-adres van uw VM als volgt:

    ```bash
    ssh publicIpAddress
    ```

2. Als u uw script om correct wordt uitgevoerd, toevoegen uitvoeringsmachtigingen met **type chmod**. Voer de naam van uw eigen script:

    ```bash
    chmod +x myVM_we_1571974050985163527.sh
    ```

3. Voer het script voor het koppelen van het herstelpunt. Voer de naam van uw eigen script:

    ```bash
    ./myVM_we_1571974050985163527.sh
    ```

    Als het script wordt uitgevoerd, wordt u gevraagd een wachtwoord voor toegang tot het herstelpunt op te geven. Voer het wachtwoord dat wordt weergegeven in de uitvoer van de vorige [az back-up terugzetten bestanden koppelpunt rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_mount_rp) opdracht dat het script herstel gegenereerd.

    De uitvoer van het script hebt u het pad voor het herstelpunt. De volgende voorbeelduitvoer wordt weergegeven dat het herstelpunt dat is gekoppeld aan */home/azureuser/myVM-20170919213536/Volume1*:

    ```
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    Please enter the password as shown on the portal to securely connect to the recovery point. : c068a041ce12465
    
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
    
    ************ Volumes of the recovery point and their mount paths on this machine ************
    
    Sr.No.  |  Disk  |  Volume  |  MountPath
    
    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170919213536/Volume1
    
    ************ Open File Explorer to browse for files. ************
    ```

4. Gebruik **cp** kopiëren de NGINX-standaardwebpagina in het gekoppelde herstelpunt weer naar de locatie van het oorspronkelijke bestand. Vervang de */home/azureuser/myVM-20170919213536/Volume1* koppelpunt met uw eigen locatie:

    ```bash
    sudo cp /home/azureuser/myVM-20170919213536/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```

6. Vernieuw de webpagina in uw webbrowser. De website nu wordt correct geïnstalleerd, zoals wordt weergegeven in het volgende voorbeeld:

    ![NGINX-website nu wordt correct geïnstalleerd](./media/tutorial-restore-files/nginx-restored.png)

7. Sluit de SSH-sessie op de virtuele machine als volgt:

    ```bash
    exit
    ```

8. Ontkoppelen van het herstelpunt van uw virtuele machine met [az back-up terugzetten bestanden ontkoppelen rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_unmount_rp). Het volgende voorbeeld ontkoppelt van het herstelpunt van de virtuele machine met de naam *myVM* in *myRecoveryServicesVault*.

    Vervang *myRecoveryPointName* met de naam van het herstelpunt dat u hebt verkregen in de vorige opdrachten:
    
    ```azurecli-interactive
    az backup restore files unmount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een herstelpunt dat is verbonden met een VM en bestanden voor een webserver hersteld. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Lijst en selecteer herstelpunten
> * Verbinding maken met een herstelpunt voor een virtuele machine
> * Bestanden herstellen vanaf een herstelpunt

Ga naar de volgende zelfstudie voor meer informatie over hoe u back-up van Windows Server naar Azure.

> [!div class="nextstepaction"]
> [Back-up van Windows Server naar Azure](tutorial-backup-windows-server-to-azure.md)

