---
title: Azure onderhoudsschema's (preview) | Microsoft Docs
description: Onderhoud plannen kan klanten van plan bent om de benodigde gepland onderhoudsgebeurtenissen maakt gebruik van de Azure SQL Data warehouse-service naar de nieuwe functies, upgrades en patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/07/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: a6eedc0bac7aab69a9138f4f63d0d9d802e74dfc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228334"
---
# <a name="change-a-maintenance-schedule"></a>Een onderhoudsplanning wijzigen 

## <a name="portal"></a>Portal
Een onderhoudsplanning kan worden bijgewerkt of op elk gewenst moment worden gewijzigd. Echter, als het geselecteerde exemplaar momenteel is actieve onderhoudscyclus doorlopen met de instellingen wordt worden opgeslagen en worden alleen actief tijdens de volgende vastgestelde onderhoudsperiode. [Meer informatie](https://docs.microsoft.com/azure/service-health/resource-health-overview) over het controleren van uw datawarehouse tijdens een actieve onderhoud. 

Preview-versie wordt worden vragen we u twee onderhoudsvensters selecteren tijdens een periode van 7 dagen. Elk onderhoudsvenster kan liggen tussen 3 en 8 uur, met 3 uur op dit moment wordt de kortste beschikbare optie. Onderhoud kan plaatsvinden op elk gewenst moment binnen een geïdentificeerde onderhoudsvenster maar wordt niet uitgevoerd buiten de tijdvensters geïdentificeerd met de voorafgaande kennisgeving, wordt u ook ervaring worden korte verlies van verbinding als de service implementeert u nieuwe code naar uw gegevens datawarehouse. 

## <a name="identifying-the-primary-and-secondary-windows"></a>Identificeren van de primaire en secundaire windows

De primaire en secundaire windows moeten worden aangeduid in afzonderlijke dagreeksen (dat wil zeggen, venster (Tuesday, Thursday) van de primaire, secundaire venster (zaterdag, zondag)

De volgende stappen als u wilt wijzigen van de onderhoudsplanning die is toegepast op uw datawarehouse in de portal.
1.  Meld u aan bij [Azure Portal](https://portal.azure.com/).
2.  Selecteer het datawarehouse dat u wilt bijwerken. De pagina wordt geopend op de overzichtsblade. 
3.  De pagina Onderhoud schema-instellingen zijn toegankelijk door te klikken op de onderhoud plannen (preview) Samenvatting koppeling in de overzichtsblade of via de optie voor het plannen van onderhoud in het linkermenu van de Resource.  

    ![Overzicht van blade opties](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. U kunt de gewenste dagenbereik identificeren voor uw primaire onderhoudsvenster met behulp van de keuzerondjes aan de bovenkant van de pagina. Deze selectie bepaalt als uw primaire venster wordt uitgevoerd op een doordeweekse dag of tijdens het weekend. Uw selectie wordt de onderstaande vervolgkeuzelijst waarden dienovereenkomstig bijgewerkt. Tijdens de preview in sommige regio's ondersteunen mogelijk niet nog de volledige set dag welke opties beschikbaar. Deze waarden wordt in de komende maanden bijgewerkt.

   ![De blade instellingen onderhoud](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Kies uw favoriete primaire en secundaire onderhoudsvensters met behulp van de dag, begintijd en -tijd venster vervolgkeuzelijsten hieronder. De planning overzicht aan de onderkant van de blade wordt bijgewerkt op basis van de vervolgkeuzelijst waarden geselecteerd.

#### <a name="dropdown-options"></a>Opties voor vervolgkeuzelijst
- : Voorkeur dag onderhoud uitvoeren tijdens de geselecteerde periode.
- Begintijd: begintijd onderhoud de voorkeur.
- Tijdvenster: voorkeur duur van het tijdvenster.

  Klik op opslaan nadat u uw voorkeur onderhoudsvensters hebt geselecteerd. Een bevestigingsbericht wordt weergegeven waaruit blijkt dat het nieuwe schema is niet actief. Als u een planning in een regio die onderhoudsplanning nog niet wordt ondersteund opslaat, wordt wordt het volgende bericht weergegeven. De instellingen worden opgeslagen en worden actief wanneer de functie beschikbaar in de geselecteerde regio.    

    ![Niet actief is in de regio toast](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) over Webhook-acties voor waarschuwingsregels.
- [Meer informatie](https://docs.microsoft.com/azure/service-health/service-health-overview) over Azure Service Health


