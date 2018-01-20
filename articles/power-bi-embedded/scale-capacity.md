---
title: De capaciteit van de Power BI Embedded schalen | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een Power BI Embedded capaciteit schalen in Microsoft Azure.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/19/2018
ms.author: asaxton
ms.openlocfilehash: 7eb64cce37f2655b72ab9b5fadedf7581fe007fb
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="scale-your-power-bi-embedded-capacity"></a>De capaciteit van de Power BI Embedded schalen

In dit artikel wordt uitgelegd hoe u een Power BI Embedded capaciteit schalen in Microsoft Azure. Schalen, kunt u het formaat van de capaciteit vergroten of verkleinen.

Dit wordt ervan uitgegaan dat u een Power BI Embedded capaciteit hebt gemaakt. Als u niet hebt, raadpleegt u [Power BI Embedded maken capaciteit in de Azure-portal](create-capacity.md) aan de slag.

> [!NOTE]
> Een bewerking voor vergroten/verkleinen kan ongeveer een minuut duren. Gedurende deze tijd zich de capaciteit niet beschikbaar. Ingesloten inhoud kan niet worden geladen.

## <a name="scale-a-capacity"></a>Een capaciteit schalen

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).

2. Selecteer **meer services** > **Power BI Embedded** om te zien van de capaciteit.

    ![Meer services in Azure portal](media/scale-capacity/azure-portal-more-services.png)

3. Selecteer de capaciteit die u wilt schalen.

    ![Power BI Embedded capaciteitslijst binnen Azure-portal](media/scale-capacity/azure-portal-capacity-list.png)

4. Selecteer **prijscategorie** onder **Scale** binnen uw capaciteit.

    ![Prijzen van laag optie onder schaal](media/scale-capacity/azure-portal-scale-pricing-tier.png)

    Uw huidige prijscategorie wordt beschreven in blauw.

    ![Huidige prijscategorie die worden beschreven in blauw](media/scale-capacity/azure-portal-current-tier.png)

5. Voor omhoog of omlaag schalen, selecteert u de nieuwe categorie om naar te verplaatsen. Als u een nieuwe laag, wordt een gestreepte blauw kader rond de selectie geplaatst. Selecteer **Selecteer** schalen aan de nieuwe laag.

    ![Nieuwe laag selecteren](media/scale-capacity/azure-portal-select-new-tier.png)

    De capaciteit van de schaal, duurt enkele minuten duren om te voltooien.

6. Bevestig uw laag door het overzichtstabblad weer te geven. De huidige prijscategorie wordt vermeld.

    ![Huidige tier bevestigen](media/scale-capacity/azure-portal-confirm-tier.png)

## <a name="next-steps"></a>Volgende stappen

Als u wilt onderbreken of starten van de capaciteit, Zie [onderbreken en de capaciteit van de Power BI Embedded te starten in de Azure-portal](pause-start.md).

Om te beginnen met het insluiten van Power BI-inhoud in uw toepassing, Zie [insluiten uw Power BI-dashboards, rapporten en tegels](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Nog vragen? [Probeer de Power BI-Community vragen](http://community.powerbi.com/)
