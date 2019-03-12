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
ms.date: 03/07/2019
ms.author: spelluru
ms.openlocfilehash: bc81f479305f39024b8d946e1ace3fc84ecb6253
ms.sourcegitcommit: 89b5e63945d0c325c1bf9e70ba3d9be6888da681
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57588853"
---
# <a name="track-costs-associated-with-a-lab-in-azure-devtest-labs"></a>De kosten die zijn gekoppeld aan een lab in Azure DevTest Labs bijhouden
In dit artikel bevat informatie over hoe u kunt de kosten van uw lab bijhouden. Het laat zien hoe u om weer te geven de geschatte kosten Theo voor de huidige kalendermaand voor de testomgeving. Het artikel ziet u ook de kosten maand tot heden per resource weergeven in het lab.

## <a name="view-the-monthly-estimated-lab-cost-trend"></a>De maandelijkse trend voor lab geschatte kosten weergeven 
In deze sectie leert u hoe u de **maandelijkse geschatte Kostentrend** grafiek om de huidige kalendermaand geschatte kosten-tot-datum en het geplande einde van maand kosten voor de huidige kalendermaand weer te geven. U leert ook hoe u labkosten beheren door in te stellen de bestedingslimiet doelen en drempelwaarden die, na het verstrijken, trigger DevTest Labs om te rapporteren de resultaten aan u.

Als u wilt de grafiek maandelijkse Trend voor de geschatte kosten weergeven, de volgende stappen uit: 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer in de lijst met labs, uw lab.  
4. Selecteer **configuratie en het beleid** in het menu links.  
4. Selecteer **kostentrend** in de **kosten bijhouden** sectie in het menu links. De volgende Schermafbeelding toont een voorbeeld van een grafiek kosten. 
   
    ![Kosten grafiek](./media/devtest-lab-configure-cost-management/graph.png)

    De **geschatte kosten** waarde is de huidige kalendermaand geschatte kosten-to-date. De **geschatte kosten voor** is de geschatte kosten voor de hele huidige kalendermaand berekend met behulp van de kosten voor het testlab voor de afgelopen vijf dagen.

    De kosten worden naar boven afgerond op het dichtstbijzijnde gehele getal. Bijvoorbeeld: 

    * 5.01 afgerond tot 6 
    * 5.50 afgerond tot 6
    * 5.99 afgerond tot 6

    Als deze boven de grafiek staat, de kosten die u standaard in de grafiek ziet worden *geschatte* kosten met behulp van [betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) tarieven aanbieden. U kunt ook uw eigen uitgaven doelen die worden weergegeven in de grafieken door instellen [beheren van de doelen van de kosten voor uw testomgeving.](#managing-cost-targets-for-your-lab)

    De volgende kosten zijn *niet* opgenomen in de berekening van de kosten:

    * CSP- en Dreamspark-abonnementen worden momenteel niet ondersteund als Azure DevTest Labs gebruikt de [API's van Azure facturering](../billing/billing-usage-rate-card-overview.md) voor het berekenen van de testomgeving kosten, die biedt geen ondersteuning voor CSP of Dreamspark-abonnementen.
    * De tarieven voor uw aanbieding. U kunt de tarieven van de aanbieding (die wordt weergegeven onder uw abonnement) dat u hebt onderhandeld met Microsoft of Microsoft partners op dit moment niet gebruiken. Alleen betalen per gebruik-tarieven worden gebruikt.
    * Uw belastingen
    * Slevy
    * De valuta op uw factuur. Op dit moment wordt de kosten voor het testlab alleen weergegeven in USD valuta.

### <a name="managing-cost-targets-for-your-lab"></a>Doelen van de kosten voor uw lab beheren
DevTest Labs kunt u beter de kosten in uw testomgeving door in te stellen van een bestedingslimiet doel dat u vervolgens in de grafiek maandelijkse Trend voor de geschatte kosten weergeven kunt te beheren. DevTest Labs kunnen ook sturen u een melding wanneer het opgegeven doel uitgaven of de drempelwaarde is bereikt. 

1. Op de **kostentrend** weergeeft, schakelt **beheren doel**.

    ![Doelknop beheren](./media/devtest-lab-configure-cost-management/cost-trend-manage-target.png)
2. Op de **beheren doel** pagina, geeft u een bestedingslimiet doel en drempelwaarden. U kunt ook instellen of de geselecteerde drempelwaarde wordt gerapporteerd op de grafiek kosten trend of via een webhook-melding.

    ![Doel-deelvenster beheren](./media/devtest-lab-configure-cost-management/cost-trend-manage-target-pane.png)

   - Selecteer een bepaalde periode gedurende welke u wilt dat kosten doelen bijgehouden.
      - **Maandelijkse**: kosten doelen worden bijgehouden per maand.
      - **Vaste**: kosten doelen worden bijgehouden voor het datumbereik dat u in de begin- en einddatums opgeeft. Deze waarden vertegenwoordigen meestal, hoe lang het project is gepland om uit te voeren.
   - Geef een **doel kosten**. Bijvoorbeeld hoeveel u van plan bent om te besteden in voor dit lab in de periode die u hebt gedefinieerd.
   - Selecteer deze optie in- of uitschakelen van een drempelwaarde dat u wilt, gerapporteerd: in stappen van 25% – maximaal 125% van de opgegeven **doel kosten**.
      - **Op de hoogte stellen**: Als deze drempelwaarde wordt voldaan, wordt u gewaarschuwd door een webhook-URL die u opgeeft.
      - **Vykreslit v grafu**: Als deze drempelwaarde wordt voldaan, de resultaten worden weergegeven op de kosten trend grafiek die u weergeven kunt, zoals beschreven in [de grafiek maandelijkse Trend voor de geschatte kosten weergeven](#viewing-the-monthly-estimated-cost-trend-chart).
   - Als u wilt **waarschuwen** wanneer de drempelwaarde is bereikt, moet u een webhook-URL opgeven. Selecteer in het gebied van de integratie van kosten **Klik hier als u wilt toevoegen van een integratie**. Voer een **Webhook-URL** in het meldingsvenster configureren en selecteer vervolgens **OK**.

       ![Meldingsvenster configureren](./media/devtest-lab-configure-cost-management/configure-notification.png)

      - Als u opgeeft **waarschuwen**, moet u een webhook-URL opgeven.
      - Op dezelfde manier als u een webhook-URL hebt gedefinieerd, moet u instellen **melding** naar **op** in het deelvenster van de drempelwaarde kosten.
      - Een webhook voordat u hier invoert, moet u maken.  

      Zie voor meer informatie over webhooks [maken van een webhook of API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

## <a name="view-cost-by-resource"></a>Weergave-kosten per resource 
De maandelijkse kosten trend-functie in labs kunt u zien hoeveel u in de huidige kalendermaand hebt besteed. U ziet ook de projectie van de uitgaven tot het einde van de maand, op basis van uw uitgaven in de afgelopen zeven dagen. Om te begrijpen waarom de uitgaven in het lab voldoet aan de drempelwaarden vroeg ingeschakeld, kunt u de **kosten per resource** functie waarin u de kosten maand tot heden **per resource** in een tabel.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer de gewenste lab in de lijst met labs.  
4. Selecteer **configuratie en het beleid** in het menu links.
5. Selecteer **kosten per resource** in de **kosten bijhouden** sectie in het menu links. Ziet u de kosten die zijn gekoppeld aan elke resource die is gekoppeld aan een lab. 

    ![Kosten per resource](./media/devtest-lab-configure-cost-management/cost-by-resource.png)

Deze functie kunt u eenvoudig de om resources te identificeren die de meeste zodat u kunt acties te verminderen van het testlab voor de uitgavelimiet uitvoeren. Bijvoorbeeld, is de kosten van een virtuele machine gebaseerd op de grootte van de virtuele machine. Hoe groter de grootte van de VM, meer zijn de kosten. U kunt de grootte van een virtuele machine en de eigenaar van het gemakkelijk vinden zodat u met de eigenaar van de VM communiceren kunt om te begrijpen waarom deze VM-grootte is nodig en of er een kans is te verlagen, de grootte.

[Beleid voor automatisch afsluiten](devtest-lab-get-started-with-lab-policies.md#set-auto-shutdown) helpt u bij de kosten verlagen door lab VM's op een bepaald tijdstip van de dag wordt afgesloten. Een lab-gebruiker kan echter afmelden voor het beleid afsluiten, waardoor de kosten van het uitvoeren van de virtuele machine. U kunt een virtuele machine selecteren in de tabel om te zien als deze is is gekozen-out van het beleid voor automatisch afsluiten. Als dit het geval is, kunt u communiceren met de eigenaar van de VM om te zoeken waarom de virtuele machine heeft is gekozen-out van het beleid.
 
## <a name="next-steps"></a>Volgende stappen
Hier volgen enkele dingen om te proberen het volgende:

* [Beleid voor lab maken](devtest-lab-set-lab-policy.md) -informatie over het instellen van de verschillende beleidsregels gebruikt om te bepalen hoe uw lab en bijbehorende virtuele machines worden gebruikt. 
* [Aangepaste installatiekopie maken](devtest-lab-create-template.md) : wanneer u een virtuele machine, maakt u een base, dit kan een aangepaste installatiekopie of een Marketplace-installatiekopie opgeven. In dit artikel ziet u hoe u een aangepaste installatiekopie maken van een VHD-bestand.
* [Marketplace-installatiekopieën configureren](devtest-lab-configure-marketplace-images.md) - DevTest Labs ondersteunt het maken van virtuele machines gebaseerd op Azure Marketplace-installatiekopieën. In dit artikel laat zien hoe u om op te geven die, indien van toepassing, de Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van virtuele machines in een testomgeving.
* [Een virtuele machine maken in een testomgeving](devtest-lab-add-vm.md) -ziet u hoe u een virtuele machine maken vanaf een basisinstallatiekopie (een aangepaste of Marketplace), en hoe u werkt met artefacten in uw virtuele machine.

