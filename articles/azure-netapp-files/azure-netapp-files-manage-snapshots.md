---
title: Momentopnamen beheren met behulp van Azure NetApp bestanden | Microsoft Docs
description: Beschrijft hoe u een momentopname op aanvraag voor een volume of terugzetten vanuit een momentopname maken naar een nieuw volume met behulp van Azure NetApp bestanden.
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
ms.topic: how-to-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: e3ae11adf84e858429cba4643802300f7915a166
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53412928"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Momentopnamen beheren met behulp van Azure NetApp bestanden
U kunt Azure NetApp bestanden gebruiken om te maken van een momentopname van een op aanvraag voor een volume of vanuit een momentopname herstellen naar een nieuw volume.

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Een momentopname op aanvraag voor een volume maken
U kunt momentopnamen maken alleen op aanvraag.  Momentopname beleid worden momenteel niet ondersteund.  
1.  Klik op de blade beheren van volumes op **momentopnamen**, klikt u vervolgens op **+ toevoegen momentopname** om een momentopname op aanvraag voor een volume te maken.

2.  Geef een naam voor de nieuwe momentopname die u maakt in het venster nieuwe momentopname.   

3. Klik op **OK**. 


## <a name="restore-a-snapshot-to-a-new-volume"></a>Een momentopname herstellen naar een nieuw volume
Op dit moment kunt u een momentopname alleen naar een nieuw volume herstellen. 
1. Ga naar de **momentopname beheren** blade van de Volume-blade om de lijst met momentopname weer te geven. 
2. Selecteer een momentopname om terug te zetten.  
3. Met de rechtermuisknop op de naam van de momentopname en selecteer **herstellen naar een nieuw volume** vanuit het menu-optie.  

    ![Nieuw volume momentopname herstellen](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Geef informatie voor het nieuwe volume in het venster NieuwVolume:  
    * **Naam**   
        Geef de naam op voor het volume dat u wilt maken.  
        
        De naam moet uniek zijn binnen de resourcegroep. Het moet ten minste drie tekens lang zijn.  U kunt hiervoor alle alfanumerieke tekens gebruiken.

    * **Pad naar bestand**     
        Geef het bestandspad op dat wordt gebruikt om het exportpad voor het nieuwe volume te maken. Het exportpad wordt gebruikt om het volume te koppelen en benaderen.   
        
        Een koppeldoel is het eindpunt van het IP-adres van de NFS-service. Deze waarde wordt automatisch gegenereerd.   
        
        Het bestandspad mag alleen letters, cijfers en afbreekstreepjes ('-') bevatten. Het bestandspad moet tussen de 16 en 40 tekens lang zijn. 

    * **Quotum**  
        Geef de hoeveelheid logische opslag op die u wilt toewijzen aan het volume.  

        Het veld **Beschikbare quotum** toont hoeveel ongebruikte ruimte er is in de gekozen capaciteitspool, die u kunt gebruiken om een nieuw volume te maken. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.

    *   **Virtueel netwerk**  
        Geef het virtuele Azure-netwerk (Vnet) op dat u wilt gebruiken om het volume te benaderen.  
        De Vnet die u opgeeft moet een subnet gedelegeerd naar Azure NetApp bestanden hebben. De service Azure NetApp bestanden zijn toegankelijk alleen uit het hetzelfde Vnet of van een Vnet dat zich in dezelfde regio als het volume via Vnet-peering. U kunt ook toegang tot het volume van uw on-premises netwerk via Expressroute. 

    * **Subnet**  
        Geef het subnet dat u wilt gebruiken voor het volume.  
        Het subnet dat u opgeeft moet worden overgedragen naar de service Azure NetApp bestanden. U kunt een nieuw subnet maken door het selecteren van **nieuw** onder het veld Subnet.  
<!--
    ![Restored new volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
-->

5. Klik op **OK**.   
    Het nieuwe volume waarop de momentopname is teruggezet, wordt weergegeven in de blade Volumes.

