---
title: Synchronisatie-inhoud uit een cloud-map in Azure App Service
description: Informatie over het implementeren van uw app in Azure App Service via inhoud synchronisatie uit een cloud-map.
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
ms.date: 06/05/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: d456ae2ffbd3745ef976ad94219a3f998838066b
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850216"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronisatie-inhoud uit een cloud-map in Azure App Service
Dit artikel laat zien hoe u wilt synchroniseren van de inhoud op [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) van Dropbox en OneDrive. 

De implementatie van inhoud op aanvraag-synchronisatie is ingeschakeld door de App Service [implementatie-engine Kudu](https://github.com/projectkudu/kudu/wiki). U kunt werken met uw app-code en de inhoud in een map voor de opgegeven cloud en vervolgens te synchroniseren naar App Service met één klik op een knop. Inhoud synchronisatie maakt gebruik van de Kudu-build-server. 

## <a name="enable-content-sync-deployment"></a>Implementatie van inhoud synchronisatie inschakelen

Om inhoud synchronisatie, gaat u naar uw App Service-app-pagina in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **OneDrive** of **Dropbox** > **autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-deploy-content-sync/choose-source.png)

U hoeft alleen te autoriseren eenmaal met OneDrive of Dropbox. Als u al bent gemachtigd, klikt u op **doorgaan**. U kunt het geautoriseerde OneDrive of Dropbox-account wijzigen door te klikken op **account wijzigen**.

![](media/app-service-deploy-content-sync/continue.png)

In de **configureren** pagina, selecteer de map die u wilt synchroniseren. Deze map wordt gemaakt onder het volgende aangewezen Inhoudspad in OneDrive of Dropbox. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

Wanneer u klaar bent, klikt u op **doorgaan**.

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

## <a name="synchronize-content"></a>Inhoud wordt gesynchroniseerd

Als u synchroniseren van inhoud in de map van de cloud met App Service wilt, gaat u terug naar de **Implementatiecentrum** pagina en klik op **Sync**.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > Vanwege onderliggende verschillen in de API's, **OneDrive voor bedrijven** wordt niet ondersteund op dit moment. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>Implementatie van inhoud sync uitschakelen

Als u wilt uitschakelen inhoud synchronisatie, gaat u naar uw App Service-app-pagina in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **OneDrive** of **Dropbox** > **Disconnect**.

![](media/app-service-deploy-content-sync/disable.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Implementeren vanaf de lokale Git-opslagplaats](app-service-deploy-local-git.md)
