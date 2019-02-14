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
ms.openlocfilehash: 47b9d25f8db2241bb578528780e28f43d56371e5
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963077"
---
# <a name="create-a-netapp-account"></a>Een NetApp-account maken
Als u een NetApp-account maakt, kunt u een capaciteitspool instellen en vervolgens een volume maken. U gebruikt de Azure NetApp Files-blade om een nieuw NetApp-account te maken.

## <a name="before-you-begin"></a>Voordat u begint
U moet uw abonnement hebben geregistreerd voor het gebruik van de NetApp-resourceprovider en de functie voor openbare previews.

[Registreren voor Azure NetApp Files](azure-netapp-files-register.md)

## <a name="steps"></a>Stappen 

1. Meld u aan bij Azure Portal. 
2. Verkrijg toegang tot de Azure NetApp Files-blade via een van de volgende methoden:  
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

    ![Nieuw NetApp-account](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Klik op **Create**.     
  Het NetApp-account dat u hebt gemaakt, verschijnt nu in de Azure NetApp Files-blade. 

## <a name="next-steps"></a>Volgende stappen  

[Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)

