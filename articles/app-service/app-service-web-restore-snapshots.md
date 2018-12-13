---
title: Herstellen vanuit back-up - Azure App Service
description: Leer hoe u uw app terugzetten vanuit een momentopname.
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
ms.custom: seodec18
ms.openlocfilehash: 174fbd8f3e4c652c25a78911185ef8a98a80bb50
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53260035"
---
# <a name="restore-an-app-in-azure-from-a-snapshot"></a>Een app in Azure herstellen vanuit een momentopname
Dit artikel leest u hoe u een app in herstellen [Azure App Service](../app-service/app-service-web-overview.md) vanuit een momentopname. U kunt uw app herstellen naar een eerdere status, gebaseerd op een van de momentopnamen van uw app. U hoeft geen momentopnamen back-up inschakelen, het platform een momentopname van alle apps voor gegevenshersteldoeleinden automatisch opgeslagen.

Momentopnamen zijn incrementele schaduwkopieën en ze bieden diverse voordelen ten opzichte van de normale [back-ups](web-sites-backup.md):
- Er is geen bestand kopiëren fouten als gevolg van vergrendelingen.
- Er is geen opslag formaat beperking.
- Er is geen configuratie vereist.

Het terugzetten van momentopnamen is beschikbaar voor apps die worden uitgevoerd in **Premium** laag of hoger. Zie voor meer informatie over het omhoog schalen van uw app [opschalen van een app in Azure](web-sites-scale.md).

## <a name="limitations"></a>Beperkingen

- De functie is momenteel in preview.
- U kunt alleen herstellen op dezelfde app of op een site die behoren tot deze app.
- App Service stopt de doel-app of doelsleuf tijdens het terugzetten.
- App Service houdt u drie maanden waard is om van momentopnamen voor gegevenshersteldoeleinden platform.
- U kunt momentopnamen alleen herstellen voor de afgelopen 30 dagen.
 

## <a name="restore-an-app-from-a-snapshot"></a>Een app herstellen vanuit een momentopname

1. Op de **instellingen** pagina van uw app in de [Azure-portal](https://portal.azure.com), klikt u op **back-ups** om weer te geven de **back-ups** pagina. Klik vervolgens op **herstellen** onder de **Snapshot(Preview)** sectie.
   
    ![](./media/app-service-web-restore-snapshots/1.png)

2. In de **herstellen** pagina, selecteert u de momentopname om terug te zetten.
   
    ![](./media/app-service-web-restore-snapshots/2.png)
   
3. Geef de bestemming voor het herstellen van de app in **hersteldoel**.
   
    ![](./media/app-service-web-restore-snapshots/3.png)
   
   > [!WARNING]
   > Als u ervoor kiest **overschrijven**, worden alle bestaande gegevens in de huidige bestandssysteem van uw app worden gewist en overschreven. Voordat u klikt op **OK**, zorg ervoor dat het is wat u wilt doen.
   > 
   > 
      
   > [!Note]
   > Vanwege de huidige technische beperkingen, kunt u alleen herstellen naar apps in de dezelfde schaaleenheid. Deze beperking wordt verwijderd in een toekomstige release.
   > 
   > 
   
    U kunt selecteren **bestaande App** om een site te herstellen. Voordat u deze optie gebruikt, moet hebt u al een sleuf gemaakt in uw app.

4. U kunt kiezen om de siteconfiguratie van uw te herstellen.
   
    ![](./media/app-service-web-restore-snapshots/4.png)

5. Klik op **OK**.
