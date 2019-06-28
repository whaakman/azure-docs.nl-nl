---
title: bestand opnemen
description: bestand opnemen
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 06/20/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: 72a69359d412a7560472fbb73ec525ab5d4a4fce
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67325792"
---
1. Meld u aan bij [Azure Portal].

2. Klik op **Een resource maken**.

3. Typ in het zoekvak **Web-App**.
    
4. Selecteer in de lijst met resultaten **Web-App** vanuit de Marketplace.

5. Selecteer uw **abonnement** en **resourcegroep** (Selecteer een bestaande resourcegroep _of_ Maak een nieuwe (Gebruik dezelfde naam als uw app)).

6. Kies een unieke **naam** van uw web-app.

7. Kies de standaardwaarden voor **publiceren** als de optie **Code**.

8. In de **runtimestack**, moet u een versie onder **ASP.NET** of **knooppunt**. Als u een .NET back-end bouwt, selecteert u een versie onder ASP.NET. Als u een op basis van knooppunttoepassing zijn op, Selecteer anders een van de versie van het knooppunt.

9. Selecteer het recht **besturingssysteem**, Linux of Windows. 

10. Selecteer de **regio** waar wilt u deze app worden geïmplementeerd. 

11. Selecteer de juiste **App Service-Plan** en klik op **controleren en maken**. 

12. Selecteer onder **Resourcegroep** een bestaande resourcegroep _of_ maak een nieuwe (gebruik dezelfde naam als uw app).

13. Klik op **Create**. Wacht enkele minuten tot de service is geïmplementeerd voordat u doorgaat. Houd het meldingspictogram (bel) in de koptekst van de portal in de gaten voor statusupdates.

14. Als de implementatie is voltooid, klikt u op de **implementatiedetails** sectie en klik vervolgens op de Resource van het Type **Microsoft.Web/sites**. Het, u gaat u naar de App Service Web-App die u zojuist hebt gemaakt. 

15. Klik op de **configuratie** blade onder **instellingen** en in de **toepassingsinstellingen**, klikt u op de **nieuwe toepassingsinstelling** de knop.

16. In de **toepassingsinstelling toevoegen/bewerken** pagina **naam** als **MobileAppsManagement_EXTENSION_VERSION** en de waarde als **nieuwste** en Klik op OK.

U wordt ingesteld op deze nieuwe App Service-Web-app als een mobiele app.

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/