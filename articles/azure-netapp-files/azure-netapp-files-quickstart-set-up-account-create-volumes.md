---
title: Instellen van de Quick Start - up Azure NetApp bestanden en het maken van een NFS-volume | Microsoft Docs
description: Snelstartgids - wordt beschreven hoe u snel instellen van Azure NetApp-bestanden en het maken van een volume.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 4/16/2019
ms.author: b-juche
ms.openlocfilehash: 4ea511bec75557bc6f7d37b1724b4b0db65ba9cc
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299427"
---
# <a name="quickstart-set-up-azure-netapp-files-and-create-an-nfs-volume"></a>Quickstart: Azure NetApp Files instellen en een NFS-volume maken 

In dit artikel wordt beschreven hoe u snel Azure NetApp bestanden instellen en een volume maken. 

In deze Quick Start stelt u de volgende items:

- Registratie voor Azure NetApp bestanden en NetApp-Resourceprovider
- Een NetApp-account
- Een capaciteit van toepassingen
- Een NFS-volume voor Azure NetApp-bestanden

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint 

> [!IMPORTANT] 
> U moet toegang krijgen tot de service Azure NetApp bestanden.  Zie voor het aanvragen van toegang tot de service, de [Azure NetApp Files wachtlijst verzendpagina](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u).  U moet een officiële bevestigingse-mail van het team van Azure NetApp Files wachten voordat u doorgaat. 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Registreren voor Azure NetApp bestanden en NetApp-Resourceprovider

1. Klik op het Azure Cloud Shell-pictogram in de rechterbovenhoek van de Azure-portal.

    ![Pictogram voor Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell-window.png)

2. Geef het abonnement dat opgenomen in de whitelist voor Azure NetApp bestanden is:
    
        az account set --subscription <subscriptionId>

3. Registreer de Resourceprovider van Azure: 
    
        az provider register --namespace Microsoft.NetApp --wait  

    Dit kan enige tijd duren.

## <a name="create-a-netapp-account"></a>Een NetApp-account maken

1. Voer in het zoekvak van de Azure portal, **Azure NetApp Files** en selecteer vervolgens **Azure NetApp Files** in de lijst die wordt weergegeven.

      ![Azure NetApp bestanden selecteren](../media/azure-netapp-files/azure-netapp-files-select-azure-netapp-files.png)

2. Klik op **+ Toevoegen** om een nieuw NetApp-account te maken.

     ![Nieuw NetApp-account maken](../media/azure-netapp-files/azure-netapp-files-create-new-netapp-account.png)

3. Geef de volgende informatie in het venster Nieuw NetApp-Account: 
   1. Voer **myaccount1** voor de accountnaam. 
   2. Selecteer uw abonnement.
   3. Selecteer **nieuw** om nieuwe resourcegroep te maken. Voer **myRG1** voor naam van de resourcegroep. Klik op **OK**. 
   4. Selecteer de accountlocatie van uw.  

      ![Nieuw venster van de NetApp-Account](../media/azure-netapp-files/azure-netapp-files-new-account-window.png)  

      ![Resource group-venster](../media/azure-netapp-files/azure-netapp-files-resource-group-window.png)

4. Klik op **maken** om uw nieuwe NetApp-account te maken.

## <a name="set-up-a-capacity-pool"></a>Een capaciteitspool instellen

1. Selecteer uw account NetApp vanaf de beheerblade Azure NetApp bestanden (**myaccount1**).

    ![NetApp-account selecteren](../media/azure-netapp-files/azure-netapp-files-select-netapp-account.png)  

2. Vanaf de beheerblade van de Azure NetApp bestanden van uw account NetApp, klikt u op **capaciteit pools**.

    ![Klik op de capaciteit van toepassingen](../media/azure-netapp-files/azure-netapp-files-click-capacity-pools.png)  

3. Klik op **+ groepen toevoegen**. 

    ![Klik op groepen toevoegen](../media/azure-netapp-files/azure-netapp-files-click-add-pools.png)  

4. Geef informatie voor de capaciteit van toepassingen: 
    1. Voer **mypool1** als de naam van de groep van toepassingen.
    2. Selecteer **Premium** voor het serviceniveau. 
    3. Geef **4 (TiB)** als de grootte van de groep. 

5. Klik op **OK**.

## <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Een NFS-volume maken voor Azure NetApp-bestanden

1. Vanaf de beheerblade van de Azure NetApp bestanden van uw account NetApp, klikt u op **Volumes**.

    ![Klik op Volumes](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. Klik op **Volume toevoegen**.

    ![Klik op de volumes toevoegen](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. Geef in het maken van een venster van het Volume, informatie voor het volume: 
   1. Voer **myvol1** als naam van het volume. 
   3. Selecteer de capaciteit van toepassingen (**mypool1**).
   4. Gebruik de standaardwaarde voor quotum. 
   5. Onder het virtuele netwerk, klikt u op **nieuw** te maken van een nieuwe Azure-netwerk (Vnet).  Vul vervolgens de volgende informatie:
       * Voer **myvnet1** als de Vnet-naam.
       * Een adresruimte voor uw instelling, bijvoorbeeld 10.7.0.0/16 opgeven
       * Voer **myANFsubnet** als naam van het subnet.
       * Geef het adresbereik van subnet, bijvoorbeeld 10.7.0.0/24. Houd er rekening mee dat u het toegewezen subnet niet met andere resources delen.
       * Selecteer **Microsoft.NetApp/volumes** voor subnet overdracht.
       * Klik op **OK** om het Vnet te maken.
   6. In het subnet, selecteert u het zojuist gemaakte Vnet (**myvnet1**) als het subnet gemachtigde.

      ![Maken van een volume-venster](../media/azure-netapp-files/azure-netapp-files-create-volume-window.png)  

      ![Venster virtuele netwerk maken](../media/azure-netapp-files/azure-netapp-files-create-virtual-network-window.png)  

4. Klik op **Protocol**en selecteer vervolgens **NFS** als het protocoltype voor het volume.   

    Voer **myfilepath1** als het bestandspad dat wordt gebruikt voor het maken van het pad voor exporteren voor het volume. 

    ![NFS-protocol voor snelstartgids opgeven](../media/azure-netapp-files/azure-netapp-files-quickstart-protocol-nfs.png)

5. Klik op **Controleren + maken**.

    ![Controleren en venster maken](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

5. Lees de informatie voor het volume en klik vervolgens op **maken**.  
    De gemaakte volume wordt weergegeven in de blade Volumes.

    ![Volume gemaakt](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent en u wilt, kunt u de resourcegroep verwijderen. De actie van het verwijderen van een resourcegroep is niet ongedaan worden gemaakt.  

> [!IMPORTANT]
> Alle resources binnen de resourcegroepen worden definitief verwijderd en kunnen niet ongedaan worden gemaakt. 

1. Voer in het zoekvak van de Azure portal, **Azure NetApp Files** en selecteer vervolgens **Azure NetApp Files** in de lijst die wordt weergegeven.

2. Klik in de lijst met abonnementen op de resourcegroep (myRG1) die u wilt verwijderen. 

    ![Navigeer naar resourcegroepen](../media/azure-netapp-files/azure-netapp-files-azure-navigate-to-resource-groups.png)


3. Klik in de pagina van de resourcegroep op **resourcegroep verwijderen**.

    ![Resourcegroep verwijderen](../media/azure-netapp-files/azure-netapp-files-azure-delete-resource-group.png) 

    Een venster wordt geopend en toont een waarschuwing over de resources die worden verwijderd met de resourcegroep.

4. Voer de naam van de resourcegroep (myRG1) om te bevestigen dat u wilt de resourcegroep en alle resources permanent verwijderd en klik vervolgens op **verwijderen**.

    ![Resourcegroep verwijderen](../media/azure-netapp-files/azure-netapp-files-azure-confirm-resource-group-deletion.png ) 

## <a name="next-steps"></a>Volgende stappen  

> [!div class="nextstepaction"]
> [Volumes beheren met behulp van Azure NetApp bestanden](azure-netapp-files-manage-volumes.md)  
