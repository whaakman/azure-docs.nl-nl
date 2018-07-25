---
title: Een NetApp-account maken voor toegang tot Azure NetApp Files | Microsoft Docs
description: Beschrijft hoe u toegang tot Azure NetApp Files verkrijgt en een NetApp-account maakt zodat u een capaciteitspool kunt instellen en een volume kunt maken.
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
ms.openlocfilehash: ad8cc550ce69e4dc4c19a569718fa873a65b3620
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010341"
---
# <a name="create-a-netapp-account"></a>Een NetApp-account maken
Als u een NetApp-account maakt, kunt u een capaciteitspool instellen en vervolgens een volume maken. U gebruikt de Azure NetApp Files-blade om een nieuw NetApp-account te maken.

## <a name="before-you-begin"></a>Voordat u begint
U moet zijn goedgekeurd voor toegang tot de Microsoft.NetApp Azure Resource Provider en zodanig zijn geconfigureerd dat u de Azure NetApp Files-service kunt gebruiken.  

[Registratiepagina Openbare Preview Azure NetApp Files](https://aka.ms/nfspublicpreview). 

## <a name="steps"></a>Stappen 

1. Zoek in uw preview-uitnodiging de URL voor de preview van de Azure-portal en meld u aan bij de portal. 
2.  Verkrijg toegang tot de Azure NetApp Files-blade via een van de volgende methoden:  
  * Zoek naar **Azure NetApp Files** in het zoekvak van de Azure-portal.  
  * Klik in de navigatie op **Alle services** en filter vervolgens op Azure NetApp Files.  

  U kunt de Azure NetApp Files-blade als favoriet opslaan door op het sterpictogram ernaast te klikken. 

3. Klik op **+ Toevoegen** om een nieuw NetApp-account te maken.  
  Het venster Nieuw NetApp-account verschijnt.  

4. Geef de volgende informatie voor uw NetApp-account op: 
  * **Accountnaam**  
    Geef een unieke naam voor het abonnement op.
  *  **Abonnement**  
    Kies een abonnement uit uw bestaande abonnementen.
  * **Resourcegroep**   
    Gebruik een bestaande resourcegroep of maak een nieuwe.
  * **Locatie**  
    Selecteer de regio waarin u wilt dat het account en zijn onderliggende resources zich bevinden.  
    De Azure NetApp Files-service wordt momenteel alleen ondersteund in de regio VS - oost.  

    ![Nieuw NetApp-account](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Klik op **Create**.     
  Het NetApp-account dat u hebt gemaakt, verschijnt nu in de Azure NetApp Files-blade. 

## <a name="next-steps"></a>Volgende stappen  

1. [Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)
2. [Een volume maken voor Azure NetApp Files](azure-netapp-files-create-volumes.md)
3. [Exportbeleid voor een volume configureren (optioneel)](azure-netapp-files-configure-export-policy.md)
