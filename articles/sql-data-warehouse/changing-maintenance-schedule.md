---
title: Azure onderhoudsschema's (preview) | Microsoft Docs
description: Onderhoud plannen kan klanten van plan bent om de benodigde gepland onderhoud-gebeurtenissen die de Azure SQL Data Warehouse-service gebruikt om nieuwe functies, upgrades en patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/07/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 428b9970471c9365812639e251810c571698a574
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49425952"
---
# <a name="change-a-maintenance-schedule"></a>Een onderhoudsplanning wijzigen 

## <a name="portal"></a>Portal
Een onderhoudsplanning kan worden bijgewerkt of op elk gewenst moment worden gewijzigd. Als het geselecteerde exemplaar een onderhoudscyclus van een actieve doorlopen is, kunt u de instellingen worden opgeslagen. Ze gaat worden actief tijdens de volgende geïdentificeerde onderhoudsperiode. [Meer informatie](https://docs.microsoft.com/azure/service-health/resource-health-overview) over het controleren van uw datawarehouse tijdens een actieve onderhoud. 

Terwijl Azure onderhoudsschema's beschikbaar als preview zijn, kunt u twee onderhoudsvensters selecteren tijdens een periode van zeven dagen. Elk onderhoudsvenster mag drie tot acht uur. Onderhoud kan plaatsvinden op elk gewenst moment binnen het onderhoudsvenster, maar vindt niet plaats buiten tijdvensters zonder voorafgaande kennisgeving. U moet ook een korte verlies van verbinding optreden als de service implementeert u nieuwe code naar uw datawarehouse. 

## <a name="identifying-the-primary-and-secondary-windows"></a>Identificeren van de primaire en secundaire windows

De primaire en secundaire windows moeten afzonderlijke dagreeksen hebben. Een voorbeeld is een primaire venster van dinsdag: donderdag en een secundaire van venster van zaterdag, zondag.

Als u wilt de onderhoudsplanning voor uw datawarehouse wijzigen, voert u de volgende stappen uit:
1.  Meld u aan bij [Azure Portal](https://portal.azure.com/).
2.  Selecteer het datawarehouse dat u wilt bijwerken. De pagina wordt geopend op de overzichtsblade. 
3.  Open de pagina voor onderhoud schema-instellingen door het selecteren van de **onderhoudsplanning (preview) samenvatting** koppeling op de overzichtsblade. Of Selecteer de **onderhoudsplanning** optie in het resourcemenu aan de linkerkant.  

    ![Overzicht van blade opties](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Het bereik van de dag voor uw primaire onderhoudsvenster identificeren met behulp van de opties aan de bovenkant van de pagina. Deze selectie bepaalt als uw primaire venster wordt uitgevoerd op een doordeweekse dag of tijdens het weekend. Uw selectie worden de waarden van de vervolgkeuzelijst bijgewerkt. Tijdens de preview, in sommige regio's mogelijk nog geen ondersteuning voor de volledige set van beschikbare **dag** opties.

   ![De blade instellingen onderhoud](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Kies uw favoriete primaire en secundaire onderhoudsvensters met behulp van de vakken vervolgkeuzelijst:
   - **Dag**: voorkeur dag om onderhoud uit te voeren tijdens de geselecteerde periode.
   - **Begintijd**: voorkeur begintijd voor het onderhoudsvenster.
   - **Tijdvenster**: voorkeur duur van het tijdvenster.

   De **Planningsoverzicht** gebied aan de onderkant van de blade wordt bijgewerkt op basis van de waarden die u hebt geselecteerd. 
  
6. Selecteer **Opslaan**. Een bericht wordt weergegeven, waaruit blijkt dat het nieuwe schema is nu actief. 

   Als u een schema in een regio die biedt geen ondersteuning voor onderhoud plannen opslaat, wordt het volgende bericht weergegeven. Uw instellingen zijn opgeslagen en worden actief wanneer de functie beschikbaar in de geselecteerde regio.    

   ![Bericht over beschikbaarheid in regio 's](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over webhookacties voor waarschuwingsregels.
- [Meer informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure Service Health.


