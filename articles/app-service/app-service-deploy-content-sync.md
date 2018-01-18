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
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 8e132e4d4a65588d57e3cfb969e785f5a164206c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synchronisatie-inhoud uit een cloud-map in Azure App Service
Deze zelfstudie leert u hoe u wilt synchroniseren van de inhoud op [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) van populaire cloudtoepassingen storage-services zoals Dropbox en OneDrive. 

## <a name="overview"></a>Overzicht van de implementatie van inhoud synchroniseren
De implementatie van inhoud op aanvraag-synchronisatie wordt mogelijk gemaakt door de [implementatie-engine Kudu](https://github.com/projectkudu/kudu/wiki) geïntegreerd met App Service. In de [Azure-portal](https://portal.azure.com), kunt u een map in de cloudopslag, werken met uw app-code en de inhoud in de map en synchroniseren in App Service met de op een knop te klikken. Inhoud synchronisatie maakt gebruik van de Kudu-proces voor het bouwen en uitvoeren. 

## <a name="contentsync"></a>Het inschakelen van inhoud sync-implementatie
Inschakelen van inhoud synchroniseren vanuit de [Azure-portal](https://portal.azure.com), als volgt te werk:

1. Klik in uw app-pagina in de Azure-portal, op **instellingen** > **Implementatiebron**. Klik op **bron kiezen**, selecteer daarna **OneDrive** of **Dropbox** als bron voor de implementatie. 
   
    ![Inhoud synchroniseren](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > Vanwege onderliggende verschillen in de API's, **OneDrive voor bedrijven** wordt niet ondersteund op dit moment. 
   > 
   > 
2. Voltooid de werkstroom autorisatie om in te schakelen voor toegang tot een specifiek pad van de vooraf gedefinieerde aangewezen voor OneDrive of Dropbox, waarbij inhoud alle van uw App Service-App Service wordt opgeslagen. Na autorisatie biedt de App Service-platform u de optie voor het maken van een map met inhoud onder het pad naar de aangewezen inhoud of een bestaande map met inhoud onder deze aangewezen Inhoudspad kiezen. De opgegeven paden naar inhoud toe onder uw cloud-opslag-accounts die worden gebruikt voor synchronisatie van App Service, zijn de volgende:  
   
   * **OneDrive**:`Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. Na de initiële synchronisatie van de inhoud kan de inhoud synchronisatie worden gestart op verzoek vanuit de Azure-portal. Geschiedenis van implementatie is beschikbaar op de **implementaties** pagina.
   
    ![Implementatiegeschiedenis](./media/app-service-deploy-content-sync/onedrive_sync.png)

Meer informatie voor de implementatie van Dropbox vindt onder [implementeren van Dropbox] (https://azure.microsoft.com/en-in/blog/new-deploy-to-windows-azure-web-sites-from-dropbox/).

