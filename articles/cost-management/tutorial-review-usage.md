---
title: Bekijk informatie over het gebruik en kosten in Azure kosten Management | Microsoft Docs
description: "Bekijk informatie over het gebruik en kosten voor het bijhouden van trends inefficiëntie detecteren, en waarschuwingen maken."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 363a7e8a5b5be2175cb2f6d3539878673a2b469a
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="review-usage-and-costs"></a>Bekijk informatie over het gebruik en de kosten

Azure kostenbeheer door Cloudyn leest u informatie over het gebruik en kosten zodat u trends kunt bijhouden inefficiëntie detecteren en maken van waarschuwingen. Alle informatie over het gebruik en de kosten van gegevens wordt weergegeven in Cloudyn dashboards en rapporten. De voorbeelden in deze zelfstudie helpt u stapsgewijs Hoewel gebruiks- en kosten met dashboards en rapporten bekijken. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gebruik bijhouden en de kosten van trends
> * Gebruik inefficiëntie detecteren
> * Waarschuwingen voor ongebruikelijke uitgaven of overspending maken



## <a name="open-the-cloudyn-portal"></a>De Cloudyn portal openen

U bekijken alle informatie over het gebruik en kosten in de portal Cloudyn. De portal Cloudyn openen vanuit de Azure-portal of Ga naar https://azure.cloudyn.com en aanmelden.

## <a name="track-usage-and-cost-trends"></a>Gebruik bijhouden en de kosten van trends

U bijhouden werkelijke uitgaven voor informatie over het gebruik en kosten met rapporten om trends te identificeren gedurende een periode. Als u wilt kijken trends, gebruikt u het rapport werkelijke kosten gedurende een periode. Klik op het menu rapporten boven aan de portal **kosten** > **kosten Analysis** > **werkelijke kosten gedurende een periode**. Wanneer u het rapport voor het eerst opent, zijn er zijn geen groepen of filters toegepast.

Hier volgt een voorbeeld van een rapport:

![Voorbeeld van een rapport](./media/tutorial-review-usage/actual-cost01.png)

Dit rapport bevat alle bestedingslimiet gedurende de afgelopen 30 dagen. Om weer te geven alleen uitgaven voor Azure-services, de servicegroep toepassen en filter vervolgens voor alle Azure-services. De volgende afbeelding ziet u de gefilterde services.

![gefilterde services](./media/tutorial-review-usage/actual-cost02.png)

In het voorgaande voorbeeld is minder geld besteed op 2017-08-31 dan voordat wordt gestart. Deze trend kosten blijft voor de verschillende services ongeveer negen dagen. Vervolgens extra uitgaven blijft hetzelfde als voorheen. Te veel kolommen kunnen echter een duidelijke trend worden verborgen. U kunt de rapportweergave wijzigen naar een regel of gebied grafiek zien van de gegevens die worden weergegeven in andere weergaven. De volgende afbeelding toont de trend duidelijker.

![trend van rapport](./media/tutorial-review-usage/actual-cost03.png)

In het voorbeeld duidelijk ziet u dat Azure Storage verwijderde starten op 2017-08-31 kosten terwijl uitgaven aan andere Azure-services bleef niveau. Ja, wat de oorzaak van deze verlaging in uitgaven? In dit voorbeeld sommige werknemers zijn op vakantie weg werk en de Storage-service niet gebruikt.

Een zelfstudie als video wilt bekijken over het bijhouden van gebruik en de kosten van trends, Zie [analyseren van uw cloud rekeninggegevens versus tijd met Azure kosten Management door Cloudyn](https://youtu.be/7LsVPHglM0g).

## <a name="detect-usage-inefficiencies"></a>Gebruik inefficiëntie detecteren

Optimalisatie van rapporten om efficiëntie te verbeteren, gebruik optimaliseren en manieren om op te slaan uitgaven aan uw cloudresources identificeren. Ze zijn vooral handig met rendabele sizing aanbevelingen bedoeld om te beperken van niet-actieve of dure VM's.

Een veelvoorkomend probleem dat betrekking heeft op organisaties wanneer ze in eerste instantie naar resources in de cloud is hun virtualisatiestrategie voor het. Ze vaak een benadering die overeenkomt met het abonnement dat ze gebruikt voor het maken van virtuele machines voor het virtualisatie on-premises omgeving gebruiken. En ze wordt ervan uitgegaan dat kosten worden beperkt door het verplaatsen van hun lokale virtuele machines naar de cloud-is. Deze aanpak is echter niet waarschijnlijk om kosten te verlagen.

Het probleem is dat de bestaande infrastructuur is al betaald. Gebruikers kunnen maken en houden grote virtuele machines die worden uitgevoerd als ze bevallen, niet-actieve of niet, en met weinig tot gevolg. Grote of niet-actieve virtuele machines verplaatsen naar de cloud is waarschijnlijk *verhogen* kosten. Toewijzing van de kosten voor resources is belangrijk wanneer u naar overeenkomsten met cloudserviceproviders invoert. U moet betaalt voor wat u aan of u de bron volledig of niet gebruiken doorvoeren.

Het rapport voordelige Sizing aanbevelingen identificeert jaarlijkse besparing door VM-exemplaar type capaciteit aan hun historische CPU en geheugen gebruiksgegevens vergelijken.  

Klik op het menu rapporten boven aan de portal **optimaliseren** > **prijzen optimalisatie** > **voordelige Sizing aanbevelingen**. Filteren van de provider in Azure om te kijken naar Azure-machines. Hier volgt een voorbeeld van de installatiekopie.

![Azure-VM's](./media/tutorial-review-usage/sizing01.png)

In dit voorbeeld kan $3,114 aan de hand van de aanbevelingen voor het wijzigen van de VM-exemplaar typen worden opgeslagen. Klik op het plusteken (+) onder **Details** voor de eerste aanbeveling. Hier vindt u meer informatie over de eerste aanbeveling.

![gegevens over de aanbeveling](./media/tutorial-review-usage/sizing02.png)

VM-exemplaar-id's weergeven door te klikken op het plusteken naast **lijst van kandidaten**.

![Lijst met kandidaten](./media/tutorial-review-usage/sizing03.png)

Een zelfstudie als video wilt bekijken over het detecteren van informatie over het gebruik inefficiëntie, Zie [optimaliseren van VM-grootte in Azure kosten Management Cloudyn](https://youtu.be/1xaZBNmV704).

## <a name="create-alerts-for-unusual-spending"></a>Waarschuwingen voor ongebruikelijke uitgaven maken

U kunt een waarschuwing belanghebbenden automatisch voor de bestedingslimiet afwijkingen en overspending risico's. U kunt snel en eenvoudig maken gebruik van rapporten dat ondersteuning waarschuwingen op basis van budget en de kosten van drempelwaarden voor waarschuwingen.

U maakt een waarschuwing voor elke bestedingslimiet die elk kostenrapport. In dit voorbeeld gebruikt u het rapport werkelijke kosten gedurende een periode om u te waarschuwen wanneer het totale budget Azure VM uitgaven bijna. Klik op het menu rapporten boven aan de portal **kosten** > **kosten Analysis** > **werkelijke kosten gedurende een periode**. Ingesteld **groepen** naar **Service** en stel **-Filter op de service** naar **/virtuele machine van Azure**. In de rechterbovenhoek van het rapport, klikt u op **acties** en selecteer vervolgens **rapport plant**.

Gebruik de **planning** tabblad uzelf een e-mailadres van het rapport met de frequentie waarmee u wilt verzenden. Alle tags zijn, u hebt gebruikt gefilterd en gegroepeerd zijn opgenomen in het e-mailrapport. Klik op de **drempelwaarde** Kies het tabblad en selecteer **tegenover werkelijke kosten. Drempelwaarde**. Als u had een totale budget van USD 500.000 en melding die u wilt wanneer kosten in de buurt van helft, maakt u een **rode waarschuwing** op $250.000 en een **gele waarschuwing** op $240,000. Kies vervolgens het aantal opeenvolgende waarschuwingen. Wanneer u totaal aantal waarschuwingen dat u hebt opgegeven ontvangt, worden er geen extra waarschuwingen verzonden. De geplande rapport opslaan.

![Voorbeeld van een rapport](./media/tutorial-review-usage/schedule-alert01.png)

U kunt ook de vs kosten als Percentage. Budget drempelwaarde metriek om waarschuwingen te maken. U kunt met deze metriek wordt budget percentages gebruiken in plaats van valutawaarden.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Gebruik bijhouden en de kosten van trends
> * Gebruik inefficiëntie detecteren
> * Waarschuwingen voor ongebruikelijke uitgaven of overspending maken


Ga naar de volgende zelfstudie voor meer informatie over het beheren van toegang tot gegevens.

> [!div class="nextstepaction"]
> [Toegang tot gegevens beheren](tutorial-user-access.md)
