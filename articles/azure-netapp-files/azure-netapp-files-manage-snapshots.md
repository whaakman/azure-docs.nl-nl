---
title: Momentopnamen beheren met behulp van Azure NetApp bestanden | Microsoft Docs
description: Beschrijft hoe u het maken van momentopnamen voor een volume of terugzetten van een momentopname naar een nieuw volume met behulp van Azure NetApp bestanden.
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
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: b-juche
ms.openlocfilehash: 9154add96084142ca1f93ad61c6418e3b487fb83
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58090913"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Momentopnamen beheren met behulp van Azure NetApp bestanden

U kunt Azure NetApp bestanden gebruiken om te maken van een momentopname van een op aanvraag voor een volume of vanuit een momentopname herstellen naar een nieuw volume.

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Een momentopname op aanvraag voor een volume maken

U kunt momentopnamen maken alleen op aanvraag. Momentopname beleid worden momenteel niet ondersteund.

1.  Klik op de blade Volume **momentopnamen**.

    ![Navigeer naar de momentopnamen](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Klik op **+ toevoegen momentopname** om een momentopname op aanvraag voor een volume te maken.

    ![Momentopname toevoegen](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  Geef een naam voor de nieuwe momentopname die u maakt in het venster nieuwe momentopname.   

    ![Nieuwe momentopname](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. Klik op **OK**. 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Een momentopname herstellen naar een nieuw volume

Op dit moment kunt u een momentopname alleen naar een nieuw volume herstellen. 
1. Ga naar de **momentopname beheren** blade van de Volume-blade om de lijst met momentopname weer te geven. 
2. Selecteer een momentopname om terug te zetten.  
3. Met de rechtermuisknop op de naam van de momentopname en selecteer **herstellen naar een nieuw volume** vanuit het menu-optie.  

    ![Nieuw volume momentopname herstellen](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Geef informatie voor het nieuwe volume in het venster NieuwVolume:  
    * **Naam**   
        Geef de naam op voor het volume dat u wilt maken.  
        
        De naam moet uniek zijn binnen de resourcegroep. De naam moet minstens drie tekens bevatten.  U kunt hiervoor alle alfanumerieke tekens gebruiken.

    * **Pad naar bestand**     
        Geef het bestandspad op dat wordt gebruikt om het exportpad voor het nieuwe volume te maken. Het exportpad wordt gebruikt om het volume te koppelen en benaderen.   
        
        Een koppeldoel is het eindpunt van het IP-adres van de NFS-service. Deze waarde wordt automatisch gegenereerd.   
        
        Het bestandspad mag alleen letters, cijfers en afbreekstreepjes ('-') bevatten. Het bestandspad moet tussen de 16 en 40 tekens lang zijn. 

    * **Quotum**  
        Geef de hoeveelheid logische opslag op die u wilt toewijzen aan het volume.  

        Het veld **Beschikbare quotum** toont hoeveel ongebruikte ruimte er is in de gekozen capaciteitspool, die u kunt gebruiken om een nieuw volume te maken. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.

    *   **Virtueel netwerk**  
        Geef het virtuele Azure-netwerk (Vnet) op dat u wilt gebruiken om het volume te benaderen.  
        Het opgegeven VNet moet een subnet bevatten dat is gedelegeerd aan Azure NetApp Files. U kunt toegang tot Azure NetApp bestanden alleen in hetzelfde Vnet of van een Vnet dat zich in dezelfde regio als het volume via Vnet-peering. U kunt toegang tot het volume van uw on-premises netwerk via Expressroute. 

    * **Subnet**  
        Geef het subnet op dat u wilt gebruiken voor het volume.  
        Het subnet dat u opgeeft moet worden overgedragen naar de service Azure NetApp bestanden. U kunt een nieuw subnet maken door het selecteren van **nieuw** onder het veld Subnet.  
   <!--
    ![Restored new volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
   -->

5. Klik op **OK**.   
    Het nieuwe volume waarop de momentopname is teruggezet, wordt weergegeven in de blade Volumes.

## <a name="next-steps"></a>Volgende stappen

[Informatie over de opslaghiërarchie van Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)