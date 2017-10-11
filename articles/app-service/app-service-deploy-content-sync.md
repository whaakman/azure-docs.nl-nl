---
title: Synchronisatie-inhoud uit een cloud-map in Azure App Service
description: Informatie over het implementeren van uw app in Azure App Service via inhoud synchronisatie uit een cloud-map.
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 010e7dc492abefaa3afe814c0322af9f6fe5acd2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronisatie-inhoud uit een cloud-map in Azure App Service
Deze zelfstudie leert u hoe u implementeert in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) door het synchroniseren van uw inhoud van de opslagservices populaire cloudtoepassingen zoals Dropbox en OneDrive. 

## <a name="overview"></a>Overzicht van de implementatie van inhoud synchroniseren
De implementatie van inhoud op aanvraag-synchronisatie wordt mogelijk gemaakt door de [implementatie-engine Kudu](https://github.com/projectkudu/kudu/wiki) geïntegreerd met App Service. In de [Azure Portal](https://portal.azure.com), kunt u een map in de cloudopslag, werken met uw app-code en de inhoud in de map en synchroniseren in App Service met de op een knop te klikken. Inhoud synchronisatie maakt gebruik van de Kudu-proces voor het bouwen en uitvoeren. 

## <a name="contentsync"></a>Het inschakelen van inhoud sync-implementatie
Inschakelen van inhoud synchroniseren vanuit de [Azure Portal](https://portal.azure.com), als volgt te werk:

1. Klik in de blade van uw app in de Azure Portal op **instellingen** > **Implementatiebron**. Klik op **bron kiezen**, selecteer daarna **OneDrive** of **Dropbox** als bron voor de implementatie. 
   
    ![Inhoud synchroniseren](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > Vanwege onderliggende verschillen in de API's, **OneDrive voor bedrijven** wordt niet ondersteund op dit moment. 
   > 
   > 
2. Het voltooien van de werkstroom autorisatie om in te schakelen van App Service toegang krijgen tot een specifiek pad van de vooraf gedefinieerde aangewezen voor OneDrive of Dropbox waarin alle inhoud van de App Service wordt opgeslagen.  
    Na autorisatie App Service krijgt platform u de optie voor het maken van een map met inhoud onder het pad naar de aangewezen inhoud of een bestaande map met inhoud onder deze aangewezen Inhoudspad kiezen. De opgegeven paden naar inhoud toe onder uw cloud-opslag-accounts die worden gebruikt voor synchronisatie van App Service, zijn de volgende:  
   
   * **OneDrive**:`Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. Na de initiële synchronisatie van de inhoud kan de inhoud synchronisatie worden gestart op verzoek vanuit de Azure-portal. Geschiedenis van implementatie is beschikbaar met de **implementaties** blade.
   
    ![Implementatiegeschiedenis](./media/app-service-deploy-content-sync/onedrive_sync.png)

Meer informatie voor de implementatie van Dropbox vindt onder [implementeren van Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 

