---
title: Synchronisatie-inhoud van een map in de cloud - Azure App Service
description: Informatie over het implementeren van uw app in Azure App Service via de synchronisatie van de inhoud van een map in de cloud.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/03/2018
ms.author: cephalin
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 60caadf58a8acdc7cecb0074e189fd1656032327
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617092"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Inhoud van de synchronisatie van een map in de cloud naar Azure App Service
Dit artikel leest u hoe u uw inhoud om te synchroniseren [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) van Dropbox en OneDrive. 

De implementatie van de synchronisatie van de inhoud op aanvraag wordt aangestuurd door het App Service [implementatie-engine Kudu](https://github.com/projectkudu/kudu/wiki). U kunt werken met uw app-code en de inhoud in een map in de opgegeven cloud en vervolgens synchroniseren met App Service met één klik op een knop. Synchronisatie van de inhoud maakt gebruik van de Kudu-build-server. 

## <a name="enable-content-sync-deployment"></a>Synchronisatie van de inhoud implementatie inschakelen

Om in te schakelen synchronisatie van de inhoud, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **OneDrive** of **Dropbox** > **Autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-deploy-content-sync/choose-source.png)

U hoeft alleen te autoriseren met OneDrive of Dropbox één keer. Als u al bent gemachtigd, klikt u op **doorgaan**. U kunt de geautoriseerde OneDrive of Dropbox-account wijzigen door te klikken op **account wijzigen**.

![](media/app-service-deploy-content-sync/continue.png)

In de **configureren** pagina, selecteert u de map die u wilt synchroniseren. Deze map wordt gemaakt onder het volgende aangewezen Inhoudspad in OneDrive of Dropbox. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

Wanneer u klaar bent, klikt u op **Doorgaan**.

In de **Samenvatting** pagina, controleer uw opties en klik op **Voltooien**.

## <a name="synchronize-content"></a>Synchroniseren van inhoud

Als u synchroniseren van inhoud in uw map in de cloud met App Service wilt, gaat u terug naar de **Implementatiecentrum** pagina en klik op **synchronisatie**.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > Vanwege onderliggende verschillen in de API's wordt **OneDrive voor bedrijven** niet ondersteund op dit moment. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>Synchronisatie van de inhoud implementatie uitschakelen

Als u wilt uitschakelen synchronisatie van de inhoud, gaat u naar de pagina met uw App Service-app in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **verbinding verbreken**.

![](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Implementeren vanuit lokale Git-opslagplaats](deploy-local-git.md)
