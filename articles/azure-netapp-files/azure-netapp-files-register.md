---
title: Registreren voor Azure NetApp Files | Microsoft Docs
description: Beschrijft hoe u zich registreren voor het gebruik van Azure NetApp bestanden.
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
ms.date: 05/06/2019
ms.author: b-juche
ms.openlocfilehash: 6f5d84dea2e835fd12a062b628181354295ed9f6
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299214"
---
# <a name="register-for-azure-netapp-files"></a>Registreren voor Azure NetApp Files

> [!IMPORTANT] 
> Voordat u de resourceprovider Azure NetApp Files registreert, moet een e-mailbericht hebt ontvangen van het team van Azure NetApp bestanden waaruit blijkt dat u toegang tot de service hebt gekregen. 

In dit artikel leert u hoe u te registreren voor Azure NetApp bestanden, zodat u beginnen kunt met het gebruik van de service.

## <a name="waitlist"></a>Een wachtlijst indienen voor toegang tot de service

1. Een wachtlijst indienen voor toegang tot de service Azure NetApp bestanden via de [Azure NetApp Files wachtlijst verzendpagina](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u). 

    Wachtlijst aanmelding is geen garantie voor directe toegang tot. 

2. Wachten op een officiële bevestigingse-mail van het team van Azure NetApp bestanden voordat u doorgaat met de andere taken. 

## <a name="resource-provider"></a>Registreer de Resourceprovider van NetApp

Voor het gebruik van de service moet u de Azure-resourceprovider registreren voor Azure NetApp Files.

> [!NOTE] 
> U kunt zich registreren is de Resourceprovider NetApp zelfs als u geen toegang voor de service wordt verleend. Echter, zonder toestemming toegang, een Azure-portal of API-aanvraag voor het maken van een NetApp-account of een andere Azure-bestanden voor NetApp-bron wordt geweigerd met de volgende fout:  
>
> `{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"NotFound","message":"{\r\n \"error\": {\r\n \"code\": \"InvalidResourceType\",\r\n \"message\": \"The resource type could not be found in the namespace 'Microsoft.NetApp' for api version '2017-08-15'.\"\r\n }\r\n}"}]}`


1. Klik op het pictogram voor Azure Cloud Shell in de rechterbovenhoek van Azure Portal:

      ![Pictogram voor Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Als u meerdere abonnementen voor uw Azure-account hebt, selecteert u het abonnement dat is opgenomen in de whitelist voor Azure NetApp Files:
    
        az account set --subscription <subscriptionId>

3. Voer de volgende opdracht uit in de Azure Cloud Shell-console om te controleren of uw abonnement in de whitelist is opgenomen:
    
        az feature list | grep NetApp

   De uitvoer van de opdracht ziet er als volgt uit:
   
       "id": "/subscriptions/<SubID>/providers/Microsoft.Features/providers/Microsoft.NetApp/features/ANFGA",  
       "name": "Microsoft.NetApp/ANFGA" 
       
   `<SubID>` is uw abonnements-id.

    Als u niet de functienaam van de ziet `Microsoft.NetApp/ANFGA`, u hebt geen toegang tot de service. Stoppen bij deze stap. Volg de instructies in [een wachtlijst indienen voor toegang tot de service](#waitlist) service om toegang te vragen voordat u doorgaat. 

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