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
ms.date: 12/03/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: 10a33163b2bfe6a1c9c24b9de58f83813e7534a6
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863921"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronisatie-inhoud uit een cloud-map in Azure App Service
Dit artikel laat zien hoe u inhoud kan synchroniseren naar [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) van Dropbox en OneDrive.  

De implementatie van inhoud op aanvraag-synchronisatie wordt mogelijk gemaakt door de App Service [implementatie-engine Kudu](https://github.com/projectkudu/kudu/wiki). U kunt werken met uw app-code en inhoud in een opgegeven cloudmap en vervolgens synchroniseren naar App Service met één klik op een knop. Inhoud synchronisatie maakt gebruik van de Kudu build server. 

## <a name="enable-content-sync-deployment"></a>Implementatie van inhoud synchronisatie inschakelen

Om inhoud synchronisatie in the schakelen, gaat u naar uw App Service-app-pagina in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **OneDrive** of **Dropbox** > **Autoriseren**. Volg de aanwijzingen voor autorisatie. 

![](media/app-service-deploy-content-sync/choose-source.png)

U hoeft eenmalig te autoriseren met OneDrive of Dropbox. Als u al bent gemachtigd, klikt u op **Doorgaan**. U kunt het geautoriseerde OneDrive of Dropbox-account wijzigen door te klikken op **Account wijzigen**.

![](media/app-service-deploy-content-sync/continue.png)

In de **Configureren** pagina, selecteer de map die u wilt synchroniseren. Deze map wordt gemaakt onder het volgende aangewezen Inhoudspad in OneDrive of Dropbox. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

Wanneer u klaar bent, klikt u op **Doorgaan**.

In de **Samenvatting** pagina, controleer uw opties en klik op **Voltooien**.

## <a name="synchronize-content"></a>Inhoud synchroniseren

Als u inhoud in de map van de cloud met App Service wil synchroniseren, gaat u terug naar de **Implementatiecentrum** pagina en klik op **Sync**.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > Vanwege onderliggende verschillen in de API's wordt **OneDrive voor bedrijven** niet ondersteund op dit moment. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>Implementatie van inhoud sync uitschakelen

Als u inhoud synchronisatie wilt uitschakelen, gaat u naar uw App Service-app-pagina in de [Azure-portal](https://portal.azure.com).

Klik in het menu links op **Implementatiecentrum** > **verbinding verbreken**.

![](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Implementeren vanaf de lokale Git-opslagplaats](app-service-deploy-local-git.md)
