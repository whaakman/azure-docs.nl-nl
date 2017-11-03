---
title: Aan de slag met automatisch schalen in Azure | Microsoft Docs
description: Informatie over het schalen van uw resources in Azure.
author: rajram
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: rajram
ms.openlocfilehash: 84ce3af2bef1e3204ad91567d155d7cbf90e9c7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-autoscale-in-azure"></a>Aan de slag met automatisch schalen in Azure
Dit artikel wordt beschreven hoe u uw instellingen voor automatisch schalen voor uw resource in de Microsoft Azure-portal.

Monitor voor automatisch schalen die Azure geldt alleen voor virtuele-machineschaalsets, cloudservices, Azure App Service-abonnementen en App Service-omgevingen. 

## <a name="discover-the-autoscale-settings-in-your-subscription"></a>De instellingen voor automatisch schalen in uw abonnement detecteren
U kunt alle resources waarvoor automatisch schalen van toepassing in de Azure-Monitor is kan detecteren. Gebruik de volgende stappen uit voor een stapsgewijze:

1. Open de [Azure-portal.][1]
2. Klik op het pictogram van de Azure-Monitor in het linkerdeelvenster.
  ![Open Azure Monitor][2]
3. Klik op **automatisch schalen** om alle resources waarvoor automatisch schalen van toepassing, samen met de huidige status voor automatisch schalen is weer te geven.
  ![Automatisch schalen in Azure Monitor detecteren][3]

U kunt het filterdeelvenster boven aan het bereik omlaag in de lijst gebruiken om resources te selecteren in een specifieke resourcegroep, specifieke brontypen of een specifieke bron.

Voor elke resource vindt u het huidige aantal exemplaren en de status voor automatisch schalen. De status voor automatisch schalen kan zijn:

- **Niet geconfigureerd**: U hebt automatisch schalen niet nog ingeschakeld voor deze bron.
- **Ingeschakeld**: U hebt automatisch schalen ingeschakeld voor deze bron.
- **Uitgeschakeld**: U hebt automatisch schalen uitgeschakeld voor deze bron.

## <a name="create-your-first-autoscale-setting"></a>Maken van uw eerste instelling voor automatisch schalen

Laten we nu verloopt via een eenvoudige stapsgewijze voor het maken van uw eerste instelling voor automatisch schalen.

1. Open de **automatisch schalen** blade in Azure Monitor en selecteer een resource die u wilt schalen. (Een App Service-plan dat is gekoppeld aan een web-app de volgende stappen gebruiken. U kunt [uw eerste ASP.NET-web-app maken in Azure in vijf minuten.] [4])
2. Houd er rekening mee dat het huidige aantal exemplaren 1 is. Klik op **inschakelen voor automatisch schalen**.
  ![Instelling voor de nieuwe web-app schalen][5]
3. Geef een naam voor de instelling schalen en klik vervolgens op **een regel toevoegen**. Let op de regel schalingsopties die als een deelvenster context aan de rechterkant geopend. Hiermee wordt de optie voor het schalen van het aantal exemplaren op 1 als het CPU-percentage van de resource groter is dan 70 procent standaard ingesteld. Laat het veld op de standaardwaarden en klik op **toevoegen**.
  ![Scale-instelling voor een web-app maken][6]
4. U hebt nu uw eerste schaalregel gemaakt. Houd er rekening mee dat de UX raadt aan om de aanbevolen procedures en statussen dat 'het verdient aanbeveling om ten minste één scale in regel." Dit doet u als volgt:
  
    a. Klik op **een regel toevoegen**. 

    b. Stel **Operator** naar **minder dan**.

    c. Stel **drempelwaarde** naar **20**.

    d. Stel **bewerking** naar **verkleinen tellen per**.

   U hebt nu een schaalinstelling dat kan worden geschaald out/schalen op basis van CPU-gebruik.
   ![Schalen op basis van CPU][8]
5. Klik op **Opslaan**.

Gefeliciteerd. U hebt nu uw eerste scale-instelling voor automatisch schalen die uw web-app op basis van CPU-gebruik gemaakt.

> [!NOTE] 
> Dezelfde stappen van toepassing zijn op aan de slag met een virtuele-machineschaalset of cloud service-rol.

## <a name="other-considerations"></a>Andere overwegingen
### <a name="scale-based-on-a-schedule"></a>Schalen op basis van een planning
Naast de schaal aan op basis van CPU, kunt u uw scale anders instellen voor specifieke dagen van de week.

1. Klik op **een scale-voorwaarde toevoegen**.
2. Instellen van de scale-modus en de regels is hetzelfde als de standaardvoorwaarde.
3. Selecteer **specifieke dagen herhalen** voor de planning.
4. Selecteer de dagen en de tijd beginnen of eindigen voor waarop de voorwaarde van de schaal moet worden toegepast.

![Scale-voorwaarde op basis van de planning][9]
### <a name="scale-differently-on-specific-dates"></a>Anders worden geschaald op specifieke datums
Naast de schaal aan op basis van CPU, kunt u uw scale anders instellen voor specifieke datums.

1. Klik op **een scale-voorwaarde toevoegen**.
2. Instellen van de scale-modus en de regels is hetzelfde als de standaardvoorwaarde.
3. Selecteer **Geef beginnen of eindigen datums** voor de planning.
4. Selecteer de datums beginnen of eindigen en de tijd beginnen of eindigen voor waarop de voorwaarde van de schaal moet worden toegepast.

![Scale-voorwaarde op basis van datums][10]

### <a name="view-the-scale-history-of-your-resource"></a>De geschiedenis van de schaal van uw resource weergeven
Wanneer de bron wordt geschaald omhoog of omlaag, wordt een gebeurtenis vastgelegd in het gebeurtenissenlogboek. U kunt de geschiedenis van de schaal van uw resource weergeven voor de afgelopen 24 uur door het overschakelen naar de **Uitvoeringsgeschiedenis** tabblad.

![geschiedenis uitvoeren][11]

Als u wilt weergeven van de geschiedenis van de volledige schaal (voor maximaal 90 dagen), selecteert u **Klik hier voor meer informatie**. Het activiteitenlogboek wordt geopend met automatisch schalen die vooraf zijn geselecteerd voor uw resource en categorie.

### <a name="view-the-scale-definition-of-your-resource"></a>De definitie van de schaal van uw resource weergeven
Automatisch schalen is een Azure Resource Manager-bron. U kunt de definitie van de schaal in JSON weergeven door het overschakelen naar de **JSON** tabblad.

![De definitie van de schaal][12]

U kunt wijzigingen aanbrengen in JSON rechtstreeks, indien nodig. Deze wijzigingen worden weergegeven nadat u deze opslaan.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Schakel automatisch schalen en uw exemplaren handmatig schalen
Er is mogelijk dat u wilt uw huidige schaalinstelling uitschakelen en handmatig schalen uw resource.

Klik op de **uitschakelen voor automatisch schalen** bovenaan op de knop.
![Schakel automatisch schalen][13]

> [!NOTE] 
> Deze optie wordt uitgeschakeld voor uw configuratie. Echter, u kunt teruggaan naar het nadat u automatisch schalen opnieuw inschakelen. 

U kunt nu het aantal exemplaren dat u schalen wilt naar handmatig instellen.

![Set handmatig schalen][14]

U kunt altijd terugkeren naar automatisch schalen door te klikken op **inschakelen voor automatisch schalen** en vervolgens **opslaan**.

## <a name="next-steps"></a>Volgende stappen
- [Een activiteit logboek-waarschuwing voor het bewaken van alle automatisch schalen engine bewerkingen voor uw abonnement maken](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Een activiteit logboek-waarschuwing voor het bewaken van alle mislukte bewerkingen worden uitgevoerd voor automatisch schalen scale-in/scale-out op uw abonnement maken](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

