---
title: De maandelijkse trend voor lab geschatte kosten weergeven in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over de Azure DevTest Labs maandelijkse geschatte kosten trendgrafiek.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 1f46fdc5-d917-46e3-a1ea-f6dd41212ba4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: d3a02a850059dc96e4e4ba306fed7cded2f7dd7d
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57454184"
---
# <a name="view-the-monthly-estimated-lab-cost-trend-in-azure-devtest-labs"></a>De maandelijkse trend voor lab geschatte kosten weergeven in Azure DevTest Labs
De functie Cost Management van DevTest Labs kunt u de kosten van uw lab bijhouden. In dit artikel laat zien hoe u de **maandelijkse geschatte Kostentrend** grafiek om de huidige kalendermaand geschatte kosten-tot-datum en het geplande einde van maand kosten voor de huidige kalendermaand weer te geven. In dit artikel leest u ook hoe labkosten beter te beheren door in te stellen de bestedingslimiet doelen en drempelwaarden die, na het verstrijken, trigger DevTest Labs om te rapporteren de resultaten aan u.

## <a name="viewing-the-monthly-estimated-cost-trend-chart"></a>De grafiek maandelijkse Trend voor de geschatte kosten weergeven
Als u wilt de grafiek maandelijkse Trend voor de geschatte kosten weergeven, de volgende stappen uit: 

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer indien nodig, **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst. (Uw lab mogelijk al worden weergegeven op het Dashboard onder **alle Resources**).
1. Selecteer de gewenste lab in de lijst met labs.  
1. Op van het lab **overzicht** gedeelte **configuratie en het beleid**.   
1. Aan de linkerkant onder **kosten bijhouden**, selecteer **kostentrend**.

   De volgende Schermafbeelding toont een voorbeeld van een grafiek kosten. 
   
    ![Kosten grafiek](./media/devtest-lab-configure-cost-management/graph.png)

De **geschatte kosten** waarde is de huidige kalendermaand geschatte kosten-to-date. De **geschatte kosten voor** is de geschatte kosten voor de hele huidige kalendermaand berekend met behulp van de kosten voor het testlab voor de afgelopen vijf dagen.

De kosten worden naar boven afgerond op het dichtstbijzijnde gehele getal. Bijvoorbeeld: 

* 5.01 afgerond tot 6 
* 5.50 afgerond tot 6
* 5.99 afgerond tot 6

Als deze boven de grafiek staat, de kosten die u standaard in de grafiek ziet worden *geschatte* kosten met behulp van [betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) tarieven aanbieden. U kunt ook uw eigen uitgaven doelen die worden weergegeven in de grafieken door instellen [beheren van de doelen van de kosten voor uw testomgeving.](#managing-cost-targets-for-your-lab)

Bovendien is het volgende, *niet* opgenomen in de berekening van de kosten:

* CSP- en Dreamspark-abonnementen worden momenteel niet ondersteund als Azure DevTest Labs gebruikt de [API's van Azure facturering](../billing/billing-usage-rate-card-overview.md) voor het berekenen van de testomgeving kosten, die biedt geen ondersteuning voor CSP of Dreamspark-abonnementen.
* De tarieven voor uw aanbieding. U kunt de tarieven van de aanbieding (die wordt weergegeven onder uw abonnement) dat u hebt onderhandeld met Microsoft of Microsoft partners op dit moment niet gebruiken. Alleen betalen per gebruik-tarieven worden gebruikt.
* Uw belastingen
* Slevy
* De valuta op uw factuur. Op dit moment wordt de kosten voor het testlab alleen weergegeven in USD valuta.

## <a name="managing-cost-targets-for-your-lab"></a>Doelen van de kosten voor uw lab beheren
DevTest Labs kunt u beter de kosten in uw testomgeving door in te stellen van een bestedingslimiet doel dat u vervolgens in de grafiek maandelijkse Trend voor de geschatte kosten weergeven kunt te beheren. DevTest Labs kunnen ook sturen u een melding wanneer het opgegeven doel uitgaven of de drempelwaarde is bereikt. 

1. Op van uw lab **overzicht** venster **configuratie en het beleid**.
1. Aan de linkerkant onder **kosten bijhouden**, selecteer **kostentrend**.

    ![Doelknop beheren](./media/devtest-lab-configure-cost-management/cost-trend.png)

1. In de **kostentrend** venster **beheren doel**.

    ![Doelknop beheren](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)

1. Geef het gewenste bestedingslimiet doel en de drempelwaarden in het deelvenster van de doel-beheren. U kunt ook instellen of de geselecteerde drempelwaarde wordt gerapporteerd op de grafiek kosten trend of via een webhook-melding.

    ![Doel-deelvenster beheren](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - Selecteer een bepaalde periode gedurende welke u wilt dat kosten doelen bijgehouden.
      - **Maandelijkse**: kosten doelen worden bijgehouden per maand.
      - **Vaste**: kosten doelen worden bijgehouden voor het datumbereik dat u in de begin- en einddatumvelden opgeeft. Dit kan normaal gesproken komen overeen met hoe lang het project is gepland om uit te voeren.
   - Geef een **doel kosten**. Dit kan bijvoorbeeld zijn hoeveel u van plan bent om te besteden in voor dit lab in de periode die u hebt gedefinieerd.
   - Selecteer deze optie in- of uitschakelen van een drempelwaarde dat u wilt, gerapporteerd: in stappen van 25% – maximaal 125% van de opgegeven **doel kosten**.
      - **Op de hoogte stellen**: Als deze drempelwaarde wordt voldaan, wordt u gewaarschuwd door een webhook-URL die u opgeeft.
      - **Vykreslit v grafu**: Als deze drempelwaarde wordt voldaan, de resultaten worden weergegeven op de kosten trend grafiek die u weergeven kunt, zoals beschreven in [de grafiek maandelijkse Trend voor de geschatte kosten weergeven](#viewing-the-monthly-estimated-cost-trend-chart).
   - Als u wilt **waarschuwen** wanneer de drempelwaarde is bereikt, moet u een webhook-URL opgeven. Selecteer in het gebied van de integratie van kosten **Klik hier als u wilt toevoegen van een integratie**.

      Een Webhook-URL opgeven in het deelvenster van de melding configureren en selecteer vervolgens **OK**.

       ![Meldingsvenster configureren](./media/devtest-lab-configure-cost-management/configure-notification.png)

      - Als u opgeeft **waarschuwen**, moet u een webhook-URL opgeven.
      - Op dezelfde manier als u een webhook-URL hebt gedefinieerd, moet u instellen **melding** naar **op** in het deelvenster van de drempelwaarde kosten.
      - Een webhook voordat u hier invoert, moet u maken.  

      Zie voor meer informatie over webhooks [maken van een webhook of API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md). 
 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Gerelateerde blogberichten
* [Twee meer zaken houden van uw kosten op schema in DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/06/21/keep-your-cost-on-track/)
* [Waarom Kostendrempels?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/11/why-cost-thresholds/)

## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele dingen om te proberen het volgende:

* [Beleid voor lab maken](devtest-lab-set-lab-policy.md) -informatie over het instellen van de verschillende beleidsregels gebruikt om te bepalen hoe uw lab en bijbehorende virtuele machines worden gebruikt. 
* [Aangepaste installatiekopie maken](devtest-lab-create-template.md) : wanneer u een virtuele machine, maakt u een base, dit kan een aangepaste installatiekopie of een Marketplace-installatiekopie opgeven. In dit artikel ziet u hoe u een aangepaste installatiekopie maken van een VHD-bestand.
* [Marketplace-installatiekopieën configureren](devtest-lab-configure-marketplace-images.md) - DevTest Labs ondersteunt het maken van virtuele machines gebaseerd op Azure Marketplace-installatiekopieën. In dit artikel laat zien hoe u om op te geven die, indien van toepassing, de Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van virtuele machines in een testomgeving.
* [Een virtuele machine maken in een testomgeving](devtest-lab-add-vm.md) -ziet u hoe u een virtuele machine maken vanaf een basisinstallatiekopie (een aangepaste of Marketplace), en hoe u werkt met artefacten in uw virtuele machine.

