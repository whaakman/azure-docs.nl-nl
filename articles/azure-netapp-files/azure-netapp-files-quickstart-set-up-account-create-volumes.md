---
title: Instellen van Azure NetApp-bestanden en het maken van een volume | Microsoft Docs
description: Beschrijft hoe u snel instellen van Azure NetApp-bestanden en het maken van een volume.
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
ms.topic: quickstarts
ms.date: 2/20/2019
ms.author: b-juche
ms.openlocfilehash: e2b9b3cdcb712fcf6c415f574dc687e80ae9ee3b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660507"
---
# <a name="set-up-azure-netapp-files-and-create-a-volume"></a>Azure NetApp Files instellen en een volume maken 

In dit artikel wordt beschreven hoe u snel Azure NetApp bestanden instellen en een volume maken. 

## <a name="before-you-begin"></a>Voordat u begint 

Voor deze taak moet u deel uitmaken van het Openbare Preview-programma en toegang hebben gekregen tot de Microsoft.NetApp-resourceprovider. Raadpleeg voor meer informatie over deelname aan het Openbare Preview-programma de [Azure NetApp Files Public Preview signup page](https://aka.ms/nfspublicpreview) (Registratiepagina Openbare Preview Azure NetApp Files). 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Registreren voor Azure NetApp bestanden en NetApp-Resourceprovider

1. Klik op het Azure Cloud Shell-pictogram in de rechterbovenhoek van de Azure-portal.

      ![Pictogram voor Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Geef het abonnement dat opgenomen in de whitelist voor Azure NetApp bestanden is:
    
        az account set --subscription <subscriptionId>

3. Registreer de Resourceprovider van Azure: 
    
        az provider register --namespace Microsoft.NetApp --wait  

    Dit kan enige tijd duren.

## <a name="create-a-netapp-account"></a>Een NetApp-account maken

1. Voer in het zoekvak van de Azure portal, **Azure NetApp Files** en selecteer vervolgens **Azure NetApp Files (preview)** in de lijst die wordt weergegeven.

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

## <a name="create-a-volume-for-azure-netapp-files"></a>Een volume maken voor Azure NetApp Files

1. Vanaf de beheerblade van de Azure NetApp bestanden van uw account NetApp, klikt u op **Volumes**.

    ![Klik op Volumes](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. Klik op **Volume toevoegen**.

    ![Klik op de volumes toevoegen](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. Geef in het maken van een venster van het Volume, informatie voor het volume: 
   1. Voer **myvol1** als naam van het volume. 
   2. Voer **myfilepath1** als het bestandspad dat wordt gebruikt voor het maken van het pad voor exporteren voor het volume.
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

4. Klik op **Controleren + maken**.

    ![Controleren en venster maken](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

5. Lees de informatie voor het volume en klik vervolgens op **maken**.  
    De gemaakte volume wordt weergegeven in de blade Volumes.

    ![Volume gemaakt](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

## <a name="next-steps"></a>Volgende stappen  

* [Informatie over de opslaghiërarchie van Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Volumes beheren met behulp van Azure NetApp bestanden](azure-netapp-files-manage-volumes.md) 
