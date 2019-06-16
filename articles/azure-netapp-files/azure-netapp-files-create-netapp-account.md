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
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 25cae58663f6fa7ef27995c10509eb33e49dd4c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65522817"
---
# <a name="create-a-netapp-account"></a>Een NetApp-account maken
Als u een NetApp-account maakt, kunt u een capaciteitspool instellen en vervolgens een volume maken. U gebruikt de Azure NetApp Files-blade om een nieuw NetApp-account te maken.

## <a name="before-you-begin"></a>Voordat u begint
Moet dat u een e-mailbericht hebt ontvangen van het team van Azure NetApp bestanden waaruit blijkt dat u toegang tot de service hebt gekregen. Zie [een wachtlijst indienen voor toegang tot de service](azure-netapp-files-register.md#waitlist).

U moet hebt uw abonnement voor het gebruik van de Resourceprovider NetApp ook geregistreerd. Zie [Registreer de Resourceprovider van NetApp](azure-netapp-files-register.md#resource-provider).

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
   * **Abonnement**  
     Kies een abonnement uit uw bestaande abonnementen.
   * **Resourcegroep**   
     Gebruik een bestaande resourcegroep of maak een nieuwe.
   * **Location**  
     Selecteer de regio waarin u wilt dat het account en zijn onderliggende resources zich bevinden.  

     ![Nieuw NetApp-account](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Klik op **Create**.     
   Het NetApp-account dat u hebt gemaakt, verschijnt nu in de Azure NetApp Files-blade. 

> [!NOTE] 
> Als u hebt geen toegang verleend aan de service Azure NetApp bestanden, ontvangt u de volgende fout wanneer u probeert te maken van het eerste NetApp-account:  
>
> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"NotFound","message":"{\r\n \"error\": {\r\n \"code\": \"InvalidResourceType\",\r\n \"message\": \"The resource type could not be found in the namespace 'Microsoft.NetApp' for api version '2017-08-15'.\"\r\n }\r\n}"}]}`

## <a name="next-steps"></a>Volgende stappen  

[Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)

