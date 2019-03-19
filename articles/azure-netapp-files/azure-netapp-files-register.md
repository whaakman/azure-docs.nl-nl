---
title: Registreren voor Azure NetApp Files | Microsoft Docs
description: Beschrijft hoe u een registratieaanvraag voor de service Azure NetApp Files kunt indienen.
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
ms.date: 01/04/2018
ms.author: b-juche
ms.openlocfilehash: 86c016a5dbcc0d78378e59bc6b3606ddf2c54f64
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58122332"
---
# <a name="register-for-azure-netapp-files"></a>Registreren voor Azure NetApp Files
Voordat u Azure NetApp Files kunt gebruiken, moet u een registratieaanvraag voor de service Azure NetApp Files indienen.  Nadat uw aanvraag is goedgekeurd, kunt u zich registreren om de service te gebruiken.

## <a name="request-to-enroll-in-the-service"></a>Registratieaanvraag voor de service indienen
Voor deze taak moet u deel uitmaken van het Openbare Preview-programma en toegang hebben gekregen tot de Microsoft.NetApp-resourceprovider. Raadpleeg voor meer informatie over deelname aan het Openbare Preview-programma de [Azure NetApp Files Public Preview signup page](https://aka.ms/nfspublicpreview) (Registratiepagina Openbare Preview Azure NetApp Files). 


## <a name="register-the-netapp-resource-provider"></a>De NetApp-resourceprovider registreren

Voor het gebruik van de service moet u de Azure-resourceprovider registreren voor Azure NetApp Files. 

1. Klik op het pictogram voor Azure Cloud Shell in de rechterbovenhoek van Azure Portal:

      ![Pictogram voor Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Als u meerdere abonnementen voor uw Azure-account hebt, selecteert u het abonnement dat is opgenomen in de whitelist voor Azure NetApp Files:
    
        az account set --subscription <subscriptionId>

3. Voer de volgende opdracht uit in de Azure Cloud Shell-console om te controleren of uw abonnement in de whitelist is opgenomen:
    
        az feature list | grep NetApp

   De uitvoer van de opdracht ziet er als volgt uit:
   
       "id": "/subscriptions/<SubID>/providers/Microsoft.Features/providers/Microsoft.NetApp/features/publicPreviewADC",  
       "name": "Microsoft.NetApp/publicPreviewADC" 
       
   `<SubID>` is uw abonnements-id.

4. Voer de volgende opdracht uit om de Azure-resourceprovider te registreren in de Azure Cloud Shell-console: 
    
        az provider register --namespace Microsoft.NetApp --wait

   Met de parameter `--wait` wordt de console geïnstrueerd te wachten totdat het registratieproces is voltooid. Dit kan enige tijd duren.

5. Voer de volgende opdracht uit in de Azure Cloud Shell-console om te controleren of de Azure-resourceprovider is geregistreerd: 
    
        az provider show --namespace Microsoft.NetApp

   De uitvoer van de opdracht ziet er als volgt uit:
   
        {
        "id": "/subscriptions/<SubID>/providers/Microsoft.NetApp",
        "namespace": "Microsoft.NetApp", 
        "registrationState": "Registered", 
        "resourceTypes": […. 

   `<SubID>` is uw abonnements-id.  De parameterwaarde `state` geeft `Registered` aan.

6. Klik in Azure Portal op de blade **Abonnementen**.
7. Klik in de blade Abonnementen op uw abonnements-id. 
8. Klik in de instellingen van het abonnement op **Resourceproviders** om te controleren of de Microsoft.NetApp-provider de status Geregistreerd heeft: 

      ![Microsoft.NetApp geregistreerd](../media/azure-netapp-files/azure-netapp-files-registered-resource-providers.png)


## <a name="next-steps"></a>Volgende stappen  

[Een NetApp-account maken](azure-netapp-files-create-netapp-account.md)