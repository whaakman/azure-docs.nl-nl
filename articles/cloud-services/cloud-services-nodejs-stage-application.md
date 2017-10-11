---
title: Fase van een cloud service-implementatie (Node.js) | Microsoft Docs
description: Informatie over het implementeren van uw Azure-toepassing voor een testomgeving en vervolgens implementeren voor een productie-omgeving met virtuele IP-adres (VIP) voor wisselen.
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d65d26a6-b424-49cd-a88c-7ef46bb112a8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: b3000ed769e8c60eccb21e26f53ce7ccb7e68d7f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="staging-an-application-in-azure"></a>Gefaseerde installatie van een toepassing in Azure
Een verpakte toepassing kan worden toegepast op de testomgeving in Azure moet worden getest voordat u deze verplaatst naar de productie-omgeving waarin de toepassing toegankelijk via het Internet is. De testomgeving is precies zoals de productie-omgeving, behalve dat u alleen toegang tot de gefaseerde toepassing met een verborgen-URL die wordt gegenereerd door Azure. Nadat u hebt gecontroleerd of uw toepassing correct werkt, kan deze door het uitvoeren van een virtueel IP-adres (VIP) voor wisselen naar de productieomgeving worden geïmplementeerd.

> [!NOTE]
> De stappen in dit artikel zijn alleen van toepassing op het knooppunt toepassingen die worden gehost als een Azure Cloud Service.
> 
> 

## <a name="step-1-stage-an-application"></a>Stap 1: Voorbereiden van een toepassing
Deze taak wordt beschreven hoe een toepassing installeert met behulp van de **Microsoft Azure PowerShell**.

1. Bij het publiceren van een service, geeft de **-sleuf** -parameter voor de **Publish-AzureServiceProject** cmdlet.
   
   ```powershell
   Publish-AzureServiceProject -Slot staging
   ```
2. Meld u aan bij de [klassieke Azure-portal] en selecteer **Cloudservices**. Nadat de cloud service wordt gemaakt en de **fasering** kolom status is bijgewerkt naar **met**, klikt u op de naam van de service.
   
   ![weergeven van een actieve service portal][cloud-service]
3. Selecteer de **Dashboard**, en selecteer vervolgens **fasering**.
   
   ![cloud service-dashboard][cloud-service-dashboard]
4. Noteer de waarde in de **Site-URL** vermelding aan de rechterkant. De DNS-naam is een verborgen interne ID die Azure gegenereerd.
   
    ![site-url][cloud-service-staging-url]

Nu kunt u controleren of de toepassing correct in de testomgeving werkt met behulp van de staging-site-URL.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Stap 2: Een toepassing in productie door wisselen VIP's bijwerken
Nadat u de bijgewerkte versie van een toepassing in de testomgeving hebt gecontroleerd, kunt u snel maken het beschikbaar in productie door wisselen van de virtuele IP-adressen (VIP's) van de fasering en productie-omgevingen.

> [!NOTE]
> Deze stap wordt ervan uitgegaan dat u hebt al een toepassing naar productie geïmplementeerd en voorbereid de bijgewerkte versie van de toepassing.
> 
> 

1. Meld u aan bij de [klassieke Azure-portal], klikt u op **Cloudservices** en selecteer vervolgens de naam van de service.
2. Van de **Dashboard**, selecteer **fasering**, en klik vervolgens op **wisselen** aan de onderkant van de pagina. Hiermee opent u het dialoogvenster VIP's wisselen.
   
   ![VIP wisselen dialoogvenster][vip-swap-dialog]
3. Lees de informatie en klik vervolgens op **OK**. De twee implementaties beginnen met het bijwerken van de staging implementatie schakelt over naar de productie en de productie-implementatie voor fasering verandert.

U hebt voorbereid, een implementatie en een productie-implementatie door het wisselen van VIP's met de implementatie op gefaseerde wijze bijgewerkt.

## <a name="additional-resources"></a>Aanvullende resources
* [De Upgrade van een Service implementeren in productie door het wisselen van VIP's in Azure]

[klassieke Azure-portal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[De Upgrade van een Service implementeren in productie door het wisselen van VIP's in Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
