---
title: Back-up van Azure Linux VM's | Microsoft Docs
description: Uw virtuele Linux-machines beveiligen door deze back-ups van maken met Azure Backup.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2eb0958169b175813b0dca775e9250da1cb364d4
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="back-up-linux--virtual-machines-in-azure"></a>Back-up van Linux virtuele machines in Azure

U kunt uw gegevens beschermen door regelmatig back-ups te maken. Azure Backup maakt herstelpunten die zijn opgeslagen in een geografisch redundante recovery kluizen. Wanneer u vanaf een herstelpunt herstelt, kunt u de hele virtuele machine of alleen specifieke bestanden kunt herstellen. In dit artikel wordt uitgelegd hoe een enkel bestand terugzetten naar een Linux-VM met nginx. Als u een VM voor gebruik van nog geen hebt, kunt u één die gebruikmaakt van de [Linux Quick Start](quick-create-cli.md). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een back-up van een virtuele machine
> * Een dagelijkse back-up plannen
> * Een bestand te herstellen vanaf een back-up



## <a name="backup-overview"></a>Overzicht van Backup

Wanneer de Azure Backup-service een back-up begint, wordt de Backup-extensie om een momentopname punt in tijd geactiveerd. De Azure Backup-service wordt gebruikt de _VMSnapshotLinux_ extensie in Linux. De uitbreiding wordt geïnstalleerd tijdens de eerste VM back-up als de virtuele machine wordt uitgevoerd. Als de virtuele machine niet wordt uitgevoerd, maakt de Backup-service een momentopname van de onderliggende opslag (omdat er geen schrijfbewerkingen toepassing optreden terwijl de virtuele machine is gestopt).

Standaard Azure Backup duurt een bestand system consistente back-up voor Linux VM maar kan worden geconfigureerd om te [toepassing consistente back-up met script voor vóór en na-framework](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent). Zodra de Azure Backup-service de momentopname maakt, worden de gegevens worden overgedragen naar de kluis. Als u wilt optimaliseren, wordt de service identificeert en draagt alleen de blokken met gegevens die zijn gewijzigd sinds de vorige back-up.

Wanneer de overdracht van gegevens voltooid is, wordt de momentopname is verwijderd en een herstelpunt wordt gemaakt.


## <a name="create-a-backup"></a>Maak een back-up
U plant als volgt een eenvoudige dagelijkse back-up naar een Recovery Services-kluis. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **Virtuele machines** in het menu aan de linkerkant. 
3. Selecteer in de lijst de virtuele machine waarvan u een back-up wilt maken.
4. Op de blade VM in de **instellingen** sectie, klikt u op **back-up**. De **back-up** blade wordt geopend.
5. In **Recovery Services-kluis**, klikt u op **nieuw** en geef de naam op voor de nieuwe kluis. Een nieuwe kluis wordt gemaakt in dezelfde resourcegroep en locatie als de virtuele machine.
6. Klik op **back-up maken van beleid**. In dit voorbeeld behoud de standaardinstellingen en klik op **OK**.
7. Op de **back-up** blade, klikt u op **back-up inschakelen**. Hiermee maakt u een dagelijkse back-up op basis van het standaardschema.
10. Een eerste herstelpunt maken op de **back-up** Klik op de blade **back-up nu**.
11. Op de **back-up nu** blade, klikt u op het pictogram Agenda, gebruikt u het kalenderbesturingselement selecteren van de laatste dag van dit herstelpunt wordt bewaard en op **back-up**.
12. In de **back-up** blade voor uw virtuele machine, ziet u het aantal herstelpunten die voltooid zijn.

    ![Herstelpunten](./media/tutorial-backup-vms/backup-complete.png)

De eerste back-up duurt ongeveer 20 minuten. Ga naar het volgende gedeelte van deze zelfstudie nadat de back-up is voltooid.

## <a name="restore-a-file"></a>Een bestand te herstellen

Als u per ongeluk verwijdert of wijzigingen in een bestand aanbrengen, kunt u bestandsherstel het bestand te herstellen vanuit uw back-upkluis. Bestandsherstel maakt gebruik van een script dat wordt uitgevoerd op de virtuele machine, het herstelpunt dat als een lokaal station koppelen. Deze stations blijft gekoppelde 12 uur zodat u kunt bestanden van het herstelpunt kopiëren en deze naar de virtuele machine terugzetten.  

In dit voorbeeld laten we zien hoe de standaard nginx webpagina /var/www/html/index.nginx-debian.html herstellen. Het openbare IP-adres van de virtuele machine in dit voorbeeld is *13.69.75.209*. U vindt het IP-adres van het gebruik van uw vm:

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. Open een browser en typ in het openbare IP-adres van uw virtuele machine om te zien van de nginx-standaardwebpagina op uw lokale computer.

    ![Standaard nginx-webpagina](./media/tutorial-backup-vms/nginx-working.png)

1. SSH in uw virtuele machine.

    ```bash
    ssh 13.69.75.209
    ```
2. /Var/www/html/index.nginx-debian.html verwijderen.

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. Vernieuwt de browser op uw lokale computer, kunt u door op CTRL + F5 om te zien dat die standaard nginx-pagina is verwijderd.

    ![Standaard nginx-webpagina](./media/tutorial-backup-vms/nginx-broken.png)
    
1. Op de lokale computer, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com/).
6. Selecteer **Virtuele machines** in het menu aan de linkerkant. 
7. Selecteer de virtuele machine uit de lijst.
8. Op de blade VM in de **instellingen** sectie, klikt u op **back-up**. De **back-up** blade wordt geopend. 
9. Selecteer in het menu bovenaan de blade **bestandsherstel**. De **bestandsherstel** blade wordt geopend.
10. In **stap 1: Selecteer herstelpunt**, selecteer een herstelpunt in de vervolgkeuzelijst.
11. In **stap 2: downloaden script om te zoeken en herstellen van bestanden**, klikt u op de **uitvoerbaar bestand downloaden** knop. Sla het gedownloade bestand op uw lokale computer.
7. Klik op **script downloaden** voor het downloaden van het scriptbestand lokaal.
8. Open een Bash-opdrachtprompt en typ het volgende, vervangen *Linux_myVM_05-05-2017.sh* met het juiste pad en bestandsnaam op van het script dat u hebt gedownload, *azureuser* met de gebruikersnaam voor de virtuele machine en *13.69.75.209* met het openbare IP-adres voor uw virtuele machine.
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. Open op uw lokale computer, een SSH-verbinding naar de virtuele machine.

    ```bash
    ssh 13.69.75.209
    ```
    
10. Voeg op de virtuele machine, schrijfrechten hebben voor het scriptbestand.

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. Op de virtuele machine, voer het script voor het koppelen van het herstelpunt dat als een bestandssysteem.

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. De uitvoer van het script hebt u het pad voor het koppelpunt wordt gehost. De uitvoer ziet er ongeveer als volgt uit:

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
                         
    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

12. Op de virtuele machine, kopieert u de nginx-standaardwebpagina van het koppelpunt terug naar de plaats waar u het bestand verwijderd.

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. Open het browsertabblad waar u bent verbonden met het IP-adres van de virtuele machine met de standaardpagina nginx op uw lokale computer. Druk op CTRL + F5 om de browserpagina te vernieuwen. U ziet nu de standaardpagina weer werkt.

    ![Standaard nginx-webpagina](./media/tutorial-backup-vms/nginx-working.png)

18. Op de lokale computer terug naar het browsertabblad voor de Azure-portal en in **stap 3: Ontkoppel de schijven na het herstel** klikt u op de **schijven ontkoppelen** knop. Als u vergeet Voer deze stap, wordt de verbinding met het koppelpunt automatisch gesloten na 12 uur. Na deze 12 uur moet u een nieuw script voor het maken van een nieuwe koppelpunt downloaden.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maak een back-up van een virtuele machine
> * Een dagelijkse back-up plannen
> * Een bestand te herstellen vanaf een back-up

Ga naar de volgende zelfstudie voor meer informatie over het bewaken van virtuele machines.

> [!div class="nextstepaction"]
> [Virtuele machines bewaken](tutorial-monitoring.md)

