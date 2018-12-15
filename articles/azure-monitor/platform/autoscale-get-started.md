---
title: Aan de slag met automatisch schalen in Azure
description: Meer informatie over het schalen van uw Web-App-resource, Cloudservice, virtuele Machine of virtuele-Machineschaalset is ingesteld in Azure.
author: rajram
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/07/2017
ms.author: rajram
ms.component: autoscale
ms.openlocfilehash: 640a939f6f9aabb00ad830f9ba27e396b329ce7f
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440560"
---
# <a name="get-started-with-autoscale-in-azure"></a>Aan de slag met automatisch schalen in Azure
Dit artikel wordt beschreven hoe u uw instellingen voor automatisch schalen voor uw resource in de Microsoft Azure portal kunt instellen.

Automatisch schalen van Azure Monitor is alleen bedoeld voor [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/), [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/), en [API Management-services](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

## <a name="discover-the-autoscale-settings-in-your-subscription"></a>Detecteren van de instellingen voor automatisch schalen in uw abonnement
U kunt alle resources die voor automatisch schalen van toepassing in Azure Monitor is kan detecteren. Gebruik de volgende stappen uit voor stapsgewijze instructies:

1. Open de [Azure-portal.][1]
1. Klik op het pictogram van de Azure Monitor in het linkerdeelvenster.
  ![Open Azure Monitor][2]
1. Klik op **voor automatisch schalen** om alle resources die voor automatisch schalen van toepassing, samen met de huidige status van de functie voor automatisch schalen is weer te geven.
  ![Automatisch schalen in Azure Monitor detecteren][3]

U kunt het filterdeelvenster boven aan het bereik in de lijst gebruiken om resources te selecteren in een specifieke resourcegroep, specifieke brontypen of een specifieke resource.

Voor elke resource vindt u het huidige aantal instanties en de status voor automatisch schalen. De status voor automatisch schalen kan zijn:

- **Niet geconfigureerd**: U hebt voor automatisch schalen die nog niet ingeschakeld voor deze resource.
- **Ingeschakeld**: U kunt automatisch schalen hebt ingeschakeld voor deze resource.
- **Uitgeschakelde**: U kunt automatisch schalen hebt uitgeschakeld voor deze resource.

## <a name="create-your-first-autoscale-setting"></a>Uw eerste instelling voor automatisch schalen maken

Laten we nu Ga via een eenvoudige stapsgewijze te maken van uw eerste instelling voor automatisch schalen.

1. Open de **voor automatisch schalen** blade in Azure Monitor en selecteer een resource die u wilt schalen. (Een App Service-plan dat is gekoppeld aan een web-app in de volgende stappen gebruiken. U kunt [in vijf minuten uw eerste ASP.NET-web-app maken in Azure.] [4])
1. Houd er rekening mee dat het huidige aantal instanties 1 is. Klik op **automatisch schalen inschakelen**.
  ![Instelling voor de nieuwe web-app schalen][5]
1. Geef een naam op voor de schaalinstelling van de en klik vervolgens op **toevoegen van een regel**. U ziet de schaalopties regel die als een context deelvenster aan de rechterkant geopend. Hiermee wordt de optie voor het schalen van uw aantal instanties met 1 als het CPU-percentage van de resource groter is dan 70 procent standaard ingesteld. Laat de standaardwaarden en klik op **toevoegen**.
  ![Schaalinstelling voor een web-app maken][6]
1. U hebt nu uw eerste schaalregel. Houd er rekening mee dat de UX raadt aan om aanbevolen procedures en aangegeven dat 'het wordt aanbevolen dat ten minste één schaal in regel." Dit doet u als volgt:

    a. Klik op **toevoegen van een regel**.

    b. Stel **Operator** naar **minder dan**.

    c. Stel **drempelwaarde** naar **20**.

    d. Stel **bewerking** naar **aantal verlagen met**.

   U hebt nu een schaalinstelling dat schalen out/worden geschaald op basis van CPU-gebruik.
   ![Schaal op basis van CPU][8]
1. Klik op **Opslaan**.

Gefeliciteerd! U hebt nu uw eerste schaalinstelling voor automatisch schalen die uw web-app op basis van CPU-gebruik is gemaakt.

> [!NOTE]
> Dezelfde stappen zijn van toepassing op aan de slag met een virtuele-machineschaalset of rol van de service in de cloud.

## <a name="other-considerations"></a>Andere overwegingen
### <a name="scale-based-on-a-schedule"></a>Schaal op basis van een planning
Naast de schaal op basis van CPU, kunt u de schaalset anders instellen voor bepaalde dagen van de week.

1. Klik op **een schaalvoorwaarde toevoegen**.
1. De schaalmodus en de regels in te stellen, is hetzelfde als de standaardvoorwaarde.
1. Selecteer **herhalen op specifieke dagen** voor de planning.
1. Selecteer de dagen en de begin-/ eindtijd voor wanneer de schaalvoorwaarde moet worden toegepast.

![Schaalvoorwaarde op basis van planning][9]
### <a name="scale-differently-on-specific-dates"></a>Anders schalen op specifieke datums
Naast de schaal op basis van CPU, kunt u de schaalset anders instellen voor specifieke datums.

1. Klik op **een schaalvoorwaarde toevoegen**.
1. De schaalmodus en de regels in te stellen, is hetzelfde als de standaardvoorwaarde.
1. Selecteer **begin-en einddatums opgeven** voor de planning.
1. Selecteer de begin-en einddatums en de begin-/ eindtijd voor wanneer de schaalvoorwaarde moet worden toegepast.

![Schaalvoorwaarde op basis van datums][10]

### <a name="view-the-scale-history-of-your-resource"></a>De geschiedenis van de schaal van uw resource weergeven
Wanneer uw resource omhoog of omlaag wordt geschaald, wordt een gebeurtenis wordt geregistreerd in het activiteitenlogboek. U kunt de geschiedenis van de schaal van uw resource weergeven voor de afgelopen 24 uur door het overschakelen naar de **Uitvoeringsgeschiedenis** tabblad.

![Uitvoer.gesch][11]

Als u wilt weergeven van de geschiedenis van de volledige schaal (voor maximaal 90 dagen), selecteert u **Klik hier voor meer informatie**. Het activiteitenlogboek wordt geopend, met automatisch schalen die vooraf zijn geselecteerd voor uw resource en de categorie.

### <a name="view-the-scale-definition-of-your-resource"></a>De definitie van de schaal van uw resource weergeven
Automatische schaalaanpassing is een Azure Resource Manager-resource. U kunt de definitie van de schaal in JSON weergeven door het overschakelen naar de **JSON** tabblad.

![De definitie van de schaal][12]

U kunt wijzigingen aanbrengen in JSON rechtstreeks, indien nodig. Deze wijzigingen worden weergegeven nadat u ze opslaat.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Automatisch schalen uitschakelen en handmatig opschalen uw instanties
Er is mogelijk dat u de instelling van uw huidige schalen uitschakelen en handmatig schalen van uw resource.

Klik op de **automatisch schalen uitschakelen** bovenaan op de knop.
![Automatisch schalen uitschakelen][13]

> [!NOTE]
> Deze optie wordt uitgeschakeld voor uw configuratie. Maar kunt je nu weer toe nadat u de functie voor automatisch schalen opnieuw inschakelen.

U kunt nu het aantal exemplaren dat u schalen wilt naar handmatig instellen.

![Set handmatig schalen][14]

U kunt altijd terugkeren naar de functie voor automatisch schalen door te klikken op **automatisch schalen inschakelen** en vervolgens **opslaan**.

## <a name="next-steps"></a>Volgende stappen
- [Een activiteit Log-waarschuwing voor het bewaken van alle voor automatisch schalen-engine-bewerkingen op uw abonnement maken](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Een activiteit Log-waarschuwing voor het bewaken van alle mislukte voor automatisch schalen schaal-in/scale-out-bewerkingen op uw abonnement maken](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/autoscale-get-started/azure-monitor-launch.png
[3]: ./media/autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-dotnet
[5]: ./media/autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/autoscale-get-started/scale-in-recommendation.png
[8]: ./media/autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/autoscale-get-started/scale-condition-schedule.png
[10]: ./media/autoscale-get-started/scale-condition-dates.png
[11]: ./media/autoscale-get-started/scale-history.png
[12]: ./media/autoscale-get-started/scale-definition-json.png
[13]: ./media/autoscale-get-started/disable-autoscale.png
[14]: ./media/autoscale-get-started/set-manualscale.png
