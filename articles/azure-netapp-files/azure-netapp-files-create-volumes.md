---
title: Een volume maken voor Azure NetApp Files | Microsoft Docs
description: Dit artikel beschrijft hoe u een volume kunt maken voor Azure NetApp Files.
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
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 42e475f4da2102bb8b010daec6e6451ba7f13848
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011089"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Een volume maken voor Azure NetApp Files

Capaciteitsgebruik van een volume wordt in mindering gebracht op de ingerichte capaciteit van de pool.  U kunt meerdere volumes maken in een capaciteitspool, maar het totale capaciteitsverbruik van deze volumes mag niet groter zijn dan de grootte van de pool. 

## <a name="before-you-begin"></a>Voordat u begint 
U dient al een capaciteitspool te hebben ingesteld.  

[Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)

## <a name="steps"></a>Stappen 
1.  Klik in de blade Capaciteitspools beheren op de blade **Volumes**. 
2.  Klik op **+ Volume toevoegen** om een volume te maken.  
    Het venster Nieuw volume wordt nu weergegeven.

3.  Klik in het venster Nieuw volume op **Maken** en vul de volgende velden in:   
    * **Naam**      
        Geef de naam op voor het volume dat u wilt maken.   
        De naam moet uniek zijn binnen de resourcegroep. De naam moet ten minste 3 tekens bevatten.  U kunt hiervoor alle alfanumerieke tekens gebruiken.

    * **Bestandspad**  
        Geef het bestandspad op dat wordt gebruikt om het exportpad voor het nieuwe volume te maken. Het exportpad wordt gebruikt om het volume te koppelen en benaderen.   
        Een koppeldoel is het eindpunt van het IP-adres van de NFS-service. Deze waarde wordt automatisch gegenereerd.    
        Het bestandspad mag alleen letters, cijfers en afbreekstreepjes ('-') bevatten. Het bestandspad moet tussen de 16 en 40 tekens lang zijn.  

    * **Quotum**  
        Geef de hoeveelheid logische opslag op die u wilt toewijzen aan het volume.  
        Het veld **Beschikbare quotum** toont hoeveel ongebruikte ruimte er is in de gekozen capaciteitspool, die u kunt gebruiken om een nieuw volume te maken. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.  

    * **Virtueel netwerk**  
        Geef het virtuele Azure-netwerk (Vnet) op dat u wilt gebruiken om het volume te benaderen. Azure NetApp Files moet zijn geconfigureerd op het Vnet dat u opgeeft. De service Azure NetApp Files is alleen bereikbaar vanaf een Vnet dat zich op dezelfde locatie bevindt als het volume.   

    ![Nieuw volume](../media/azure-netapp-files/azure-netapp-files-new-volume.png)

4.  Klik op **OK**. 
 
Een volume neemt het abonnement, de resourcegroep en de locatiekenmerken over van de bijbehorende capaciteitspool. U kunt de implementatiestatus van het volume controleren vanuit het tabblad Meldingen.

## <a name="next-steps"></a>Volgende stappen  
[Exportbeleid voor een volume configureren (optioneel)](azure-netapp-files-configure-export-policy.md)

