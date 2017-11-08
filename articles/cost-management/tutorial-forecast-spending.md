---
title: Voorspellen uitgaven met Azure kosten Management | Microsoft Docs
description: Voorspellen uitgaven met behulp van historische informatie over het gebruik en de uitgaven van gegevens.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: d8b0cd2a3e5f9829f0844783aad22d375eb9d7a8
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="forecast-future-spending"></a>Voorspellen van toekomstige uitgaven

Azure kostenbeheer door Cloudyn helpt u het voorspellen van toekomstige uitgaven met behulp van historische informatie over het gebruik en de uitgaven van gegevens. U Cloudyn rapporten gebruiken om alle projectie kostengegevens weer te geven. De voorbeelden in deze zelfstudie doorlopen kosten projecties met behulp van de rapporten bekijken. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Voorspellen van toekomstige uitgaven

## <a name="forecast-future-spending"></a>Voorspellen van toekomstige uitgaven

Cloudyn bevat kosten projectie rapporten om te voorspellen op basis van uw gebruik na verloop van tijd uitgaven. Hun primaire doel is om te zorgen dat uw kosten trends verwachtingen van uw organisatie niet overschrijden. De rapporten die u gebruikt zijn de huidige maand geschatte kosten en jaarlijkse geschatte kosten. Beide weergeven verwachte toekomstige uitgaven als uw gebruik relatief consistent blijft met de laatste 30 dagen van het gebruik van.

De huidige maand geschatte kosten-rapport geeft de kosten van uw services. Kosten vanaf het begin van de maand en de vorige maand wordt gebruikt om de geschatte kosten weer te geven. Klik op het menu rapporten boven aan de portal **kosten** > **projectie en Budget** > **huidige maand geschatte kosten**. De volgende afbeelding toont een voorbeeld.

![huidige maand geschatte kosten](./media/tutorial-forecast-spending/project-month01.png)

In het voorbeeld ziet u welke services die de meeste besteed. Azure kosten zijn lager is dan AWS-kosten. Als u wilt dat kosten projectie om details te bekijken voor Azure VM's de **Filter** selecteert **/virtuele machine van Azure**.

![Azure VM huidige maand geschatte kosten](./media/tutorial-forecast-spending/project-month02.png)

Volg dezelfde voorgaande basisstappen om te kijken naar maandelijkse kosten projecties voor andere services die u geïnteresseerd bent in.

Jaarlijkse geschatte kosten dit rapport bevat de geëxtrapoleerde kosten van uw services via de volgende twaalf maanden.

Klik op het menu rapporten boven aan de portal **kosten** > **projectie en Budget** > **jaarlijkse geschatte kosten**. De volgende afbeelding toont een voorbeeld.

![jaarlijkse geschatte kostenrapport](./media/tutorial-forecast-spending/project-annual01.png)

In het voorbeeld ziet u welke services die de meeste besteed. Azure kosten zijn als het maandelijkse voorbeeld lager is dan AWS-kosten. Als u wilt dat kosten projectie om details te bekijken voor Azure VM's de **Filter** selecteert **/virtuele machine van Azure**.

![jaarlijkse geschatte kosten van de virtuele machines](./media/tutorial-forecast-spending/project-annual02.png)

In de afbeelding hierboven wordt de jaarlijkse geschatte kosten van Azure Virtual machines $28,374.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Voorspellen van toekomstige uitgaven


Ga naar de volgende zelfstudie voor informatie over het beheren van kosten met kosten toewijzing en showback rapporten.

> [!div class="nextstepaction"]
> [Kosten met kosten toewijzing en showback rapporten beheren](tutorial-manage-costs.md)
