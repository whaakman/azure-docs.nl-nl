---
title: De maandelijkse trend in de testomgeving geschatte kosten weergeven in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over de Azure DevTest Labs maandelijkse geschatte kosten trend van grafiek.
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: v-craic
ms.openlocfilehash: 660180fac4f4743bc543b1babf7dbe921bf8c26b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>De maandelijkse trend in de testomgeving geschatte kosten weergeven in Azure DevTest Labs
De beheerfunctie van kosten van DevTest Labs helpt u bij de kosten van uw testomgeving bijhouden. In dit artikel laat zien hoe u de **maandelijkse geschatte kosten Trend** grafiek voor de huidige kalendermaand geschatte kosten-to-date en de geschatte kosten van de laatste van de maand van de huidige kalendermaand weergeven. In dit artikel ziet u ook hoe beter lab kosten te beheren door in te stellen bestedingslimiet doelen en drempelwaarden die, wanneer bereikt, trigger DevTest Labs voor het rapporteren van de resultaten voor u.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>De grafiek maandelijkse geschatte kosten Trend weergeven
Als u wilt weergeven van de grafiek maandelijkse geschatte kosten Trend, de volgende stappen uit: 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer indien nodig, **alle Services**, en selecteer vervolgens **DevTest Labs** uit de lijst. (Uw lab mogelijk al worden weergegeven op het Dashboard onder **alle Resources**).
1. Selecteer de gewenste testomgeving uit de lijst van labs.  
1. Op de testomgeving **overzicht** gebied, selecteer **configuratie en het beleid**.   
1. Aan de linkerkant onder **kosten bijhouden**, selecteer **kosten trend**.

   De volgende Schermafbeelding toont een voorbeeld van een grafiek kosten. 
   
    ![Kosten grafiek](./media/devtest-lab-configure-cost-management/graph.png)

De **geschatte kosten** waarde is de huidige kalendermaand geschatte kosten-to-date. De **geschatte kosten** de geschatte kosten voor de hele huidige kalendermaand is berekend met behulp van de kosten van het testlab voor de afgelopen vijf dagen.

De kosten zijn afgerond naar het dichtstbijzijnde gehele getal. Bijvoorbeeld: 

* 5.01 Rondt maximaal 6 
* 5.50 Rondt maximaal 6
* 5.99 Rondt maximaal 6

Zoals deze boven het diagram, raken de kosten die u in de grafiek standaard ziet zijn *geschatte* via [betalen naar gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) tarieven aanbieden. U kunt ook uw eigen uitgaven doelen die worden weergegeven in de grafieken door instellen [het beheren van de kosten van doelen voor uw testomgeving.](#managing-cost-targets-for-your-lab)

Daarnaast volgen *niet* opgenomen in de kostenberekening:

* CSP en Dreamspark abonnementen worden momenteel niet ondersteund als Azure DevTest Labs gebruikt de [Azure facturering API's](../billing/billing-usage-rate-card-overview.md) voor het berekenen van de testomgeving kosten, die geen ondersteuning biedt voor CSP of Dreamspark abonnementen.
* De tarieven van de aanbieding. U kunt de tarieven van de aanbieding (die wordt weergegeven onder uw abonnement) dat u hebt onderhandeld met Microsoft of Microsoft partners op dit moment niet gebruiken. Alleen betalen naar gebruik tarieven worden gebruikt.
* Uw belastingen
* Uw kortingen
* Uw factureringsvaluta. Op dit moment wordt de kosten van het testlab alleen weergegeven in valuta USD.

## <a name="managing-cost-targets-for-your-lab"></a>Het beheer van kosten doelen voor uw lab
DevTest Labs kunt u beter de kosten in uw testomgeving door in te stellen van een bestedingslimiet doel dat u vervolgens in de grafiek maandelijkse geschatte kosten Trend weergeven kunt te beheren. DevTest Labs kunnen ook u een melding sturen wanneer de uitgaven van het opgegeven doel of de drempelwaarde is bereikt. 

1. Op uw lab **overzicht** deelvenster **configuratie en het beleid**.
1. Aan de linkerkant onder **kosten bijhouden**, selecteer **kosten trend**.

    ![Doelknop beheren](./media/devtest-lab-configure-cost-management/cost-trend.png)

1. In de **kosten trend** deelvenster **beheren doel**.

    ![Doelknop beheren](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)

1. Geef het gewenste bestedingslimiet doel en de drempelwaarden in het deelvenster van de doel-beheren. U kunt ook instellen of elke geselecteerde drempelwaarde is opgenomen in het diagram van de trend kosten of via een webhook-melding.

    ![Doel deelvenster beheren](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - Selecteer een periode gedurende welke u wilt dat kosten doelen bijgehouden.
      - **Maandelijkse**: kosten doelen worden bijgehouden per maand.
      - **Vaste**: kosten doelen worden bijgehouden voor het datumbereik dat u in de begin- en einddatumvelden opgeeft. Dit kan doorgaans komen overeen met hoe lang het project is gepland voor uitvoering.
   - Geef een **kosten als doel**. Dit is bijvoorbeeld mogelijk hoeveel u van plan bent te besteden aan dit lab in de periode die u hebt gedefinieerd.
   - Selecteer deze optie in- of uitschakelen van een drempelwaarde gewenste gerapporteerd: in stappen van 25% – tot 125% van de opgegeven **kosten als doel**.
      - **Waarschuwen**: als deze drempelwaarde wordt voldaan, bent u een melding via een webhook-URL die u opgeeft.
      - **In de grafiek getekend**: als deze drempelwaarde wordt voldaan, de resultaten worden weergegeven op de kosten trend-grafiek die u bekijken kunt, zoals beschreven in [weergeven van de grafiek maandelijkse geschatte kosten Trend](#viewing-the-monthly-estimated-cost-trend-chart).
   - Als u wilt **hoogte** als de drempelwaarde wordt voldaan, moet u een webhook-URL opgeven. Selecteer in het gebied van de integraties kosten **Klik hier om een integratie**.

      Een Webhook-URL opgeven in het meldingsvenster configureren en selecteer vervolgens **OK**.

       ![Meldingsvenster configureren](./media/devtest-lab-configure-cost-management/configure-notification.png)

      - Als u opgeeft **hoogte**, moet u een webhook-URL definiëren.
      - Op dezelfde manier als u een webhook-URL opgeven, moet u instellen **melding** naar **op** in het deelvenster van de drempelwaarde kosten.
      - U moet een webhook voordat u hier invoert.  

      Zie voor meer informatie over webhooks [maken van een webhook of API-functie voor Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 
 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwante blogberichten
* [Twee meer dingen te houden van uw kosten op bijhouden in DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Waarom kosten drempelwaarden?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele dingen om te proberen het volgende:

* [Labbeleidsregels definiëren](devtest-lab-set-lab-policy.md) -informatie over het instellen van de verschillende beleidsregels gebruikt om te bepalen hoe uw lab en bijbehorende virtuele machines worden gebruikt. 
* [Maken van aangepaste installatiekopie](devtest-lab-create-template.md) : wanneer u een virtuele machine, maakt u een basis, kan dit een aangepaste installatiekopie of een Marketplace-installatiekopie opgeven. In dit artikel laat zien hoe een aangepaste installatiekopie maken van een VHD-bestand.
* [Configureren van installatiekopieën van Marketplace](devtest-lab-configure-marketplace-images.md) - DevTest Labs ondersteunt het maken van virtuele machines op basis van Azure Marketplace-installatiekopieën. Dit artikel wordt beschreven hoe u kunt opgeven die, indien aanwezig, Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van virtuele machines in een testomgeving.
* [Een virtuele machine maken in een testomgeving](devtest-lab-add-vm.md) -ziet u hoe u een virtuele machine maken van een basisinstallatiekopie (een aangepaste of Marketplace), en het werken met artefacten in uw virtuele machine.

