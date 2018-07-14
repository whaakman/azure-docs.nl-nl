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
ms.openlocfilehash: 48cb88b9815ba723d93c18caf63f33b50eea850c
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009194"
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
    * **De naam**   
        Geef de naam op voor het volume dat u maakt.  
        
        De naam moet uniek zijn binnen een resourcegroep. Het moet ten minste 3 tekens lang zijn.  Het kan alle alfanumerieke tekens gebruiken.

    * **Pad naar bestand**     
        Geef het bestandspad dat wordt gebruikt voor het maken van het pad voor exporteren voor het nieuwe volume. Het pad voor exporteren wordt gebruikt om te koppelen en toegang tot het volume.   
        
        Een koppeldoel is het eindpunt van het IP-adres van de NFS-service. Er wordt automatisch gegenereerd.   
        
        De bestandsnaam van het pad mag letters, cijfers en afbreekstreepjes ('-') alleen. Moet liggen tussen 16 en 40 tekens lang. 

    * **Quotum**  
        Geef de hoeveelheid logische opslag die is toegewezen aan het volume.  

        De **beschikbare quotum** veld bevat het bedrag van ongebruikte ruimte in de groep van de gekozen capaciteit die u gebruiken kunt voor het maken van een nieuw volume. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.

    *   **Virtueel netwerk**  
        Geef de Azure-netwerk (Vnet) van waaruit u wilt toegang tot het volume. 
        
        De Vnet die u opgeeft moet Azure NetApp bestanden die zijn geconfigureerd. De service Azure NetApp bestanden zijn alleen van een Vnet dat zich in dezelfde locatie als het volume toegankelijk.  

    ![Nieuw volume hersteld](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
    
5. Klik op **OK**.   
    Het nieuwe volume waarop de momentopname is teruggezet, wordt weergegeven in de blade Volumes.

