---
title: Een app in Azure herstellen
description: Informatie over het herstellen van uw app vanuit een momentopname.
services: app-service
documentationcenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: 4164f9b5-f735-41c6-a2bb-71f15cdda417
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.topic: article
ms.date: 04/04/2018
ms.author: aelnably;nicking
ms.openlocfilehash: e1ae8fcc30323c865aa96937f43054515f293394
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="restore-an-app-in-azure-from-a-snapshot"></a>Een app in Azure herstellen vanuit een momentopname
Dit artikel laat zien hoe u een app in [Azure App Service](../app-service/app-service-web-overview.md) vanuit een momentopname. Uw app kunt u herstellen naar een eerdere status, op basis van een van de momentopnamen van uw app. U hoeft niet inschakelen van momentopnamen back-up, het platform automatisch een momentopname van alle apps voor hersteldoeleinden gegevens worden opgeslagen.

Momentopnamen zijn incrementele schaduwkopieën en ze bieden verschillende voordelen ten opzichte van de normale [back-ups](web-sites-backup.md):
- Er is geen bestand kopiëren fouten als gevolg van vergrendelingen.
- Er is geen maximale grootte van opslag.
- Er is geen configuratie vereist.

Het terugzetten van momentopnamen is beschikbaar voor apps die worden uitgevoerd **Premium** laag of hoger. Zie voor meer informatie over het schalen van uw app [een app in Azure opschalen](web-sites-scale.md).

## <a name="limitations"></a>Beperkingen

- De functie is momenteel in preview.
- U kunt alleen herstellen op dezelfde app of op een site die horen bij deze app.
- App Service stopt de doel-app of doel sleuf tijdens het terugzetten.
- App Service houdt drie maanden waard van momentopnamen voor hersteldoeleinden platform-gegevens.
- U kunt alleen momentopnamen terugzetten voor de afgelopen 30 dagen.
 

## <a name="restore-an-app-from-a-snapshot"></a>Een app herstellen vanuit een momentopname

1. Op de **instellingen** pagina van uw app in de [Azure-portal](https://portal.azure.com), klikt u op **back-ups** om weer te geven de **back-ups** pagina. Klik vervolgens op **herstellen** onder de **Snapshot(Preview)** sectie.
   
    ![](./media/app-service-web-restore-snapshots/1.png)

2. In de **herstellen** pagina, selecteert u de opname te herstellen.
   
    ![](./media/app-service-web-restore-snapshots/2.png)
   
3. Geef de bestemming voor het herstellen van de app in **terugzetdoel**.
   
    ![](./media/app-service-web-restore-snapshots/3.png)
   
   > [!WARNING]
   > Als u ervoor kiest **overschrijven**, worden alle bestaande gegevens in de huidige bestandssysteem van de app is gewist en overschreven. Voordat u op **OK**, zorg ervoor dat het is wat u wilt doen.
   > 
   > 
      
   > [!Note]
   > Vanwege de huidige technische beperkingen, kunt u alleen herstelbewerkingen naar apps in de dezelfde schaaleenheid. Deze beperking wordt verwijderd in een toekomstige release.
   > 
   > 
   
    U kunt selecteren **bestaande App** om naar een site te herstellen. Voordat u deze optie gebruikt, moet u al hebt gemaakt een site in uw app.

4. U kunt kiezen om de siteconfiguratie van uw te herstellen.
   
    ![](./media/app-service-web-restore-snapshots/4.png)

5. Klik op **OK**.
