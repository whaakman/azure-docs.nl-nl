---
title: Power BI Embedded capaciteit maken in de Azure portal | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u een Power BI Embedded capaciteit maken in Microsoft Azure.
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
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: 1902e5c18cd7083ceeda79e6b9e779e4baaf175a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-power-bi-embedded-capacity-in-the-azure-portal"></a>Power BI Embedded capaciteit in de Azure portal maken

In dit artikel wordt uitgelegd hoe u een Power BI Embedded capaciteit maken in Microsoft Azure. Power BI Embedded vereenvoudigt de mogelijkheden van Power BI door zodat u snel geweldige visuele elementen, rapporten en dashboards toevoegen in uw apps.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

> [!VIDEO https://www.youtube.com/embed/aXrvFfg_iSk]

## <a name="before-you-begin"></a>Voordat u begint

U hebt het volgende nodig om deze snelstartgids te voltooien:

* **Azure-abonnement:** gaat u naar [gratis proefversie van Azure](https://azure.microsoft.com/free/) om een account te maken.
* **Azure Active Directory:** uw abonnement moet worden gekoppeld aan een tenant van Azure Active Directory (AAD). En, ***moet u zijn aangemeld bij Azure met een account in deze tenant***. Microsoft-accounts worden niet ondersteund. Zie voor meer informatie, verificatie en gebruikersmachtigingen.
* **Power BI-tenant:** ten minste één account in uw AAD-tenant moet zich hebben geregistreerd voor Power BI.
* **Resourcegroep:** een resourcegroep die u al hebt gebruikt of [Maak een nieuwe](../azure-resource-manager/resource-group-overview.md).

## <a name="create-a-capacity"></a>Maken van een capaciteit

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).

2. Selecteer **+ (nieuw)** > **gegevens en analyse**.

3. Zoek in het zoekvak op *Power BI Embedded*.

4. Selecteren in Power BI Embedded **maken**.

5. Vul de vereiste gegevens in en selecteer vervolgens **maken**.

    ![Velden te vullen voor het maken van nieuwe capaciteit](media/create-capacity/azure-portal-create-power-bi-embedded.png)

    |Instelling |Beschrijving |
    |---------|---------|
    |**Resourcenaam**|Een unieke naam voor de capaciteit. Naam van de resource wordt weergegeven in de Power BI-beheerportal naast de Azure portal.|
    |**Abonnement**|Het abonnement dat u wilt maken van de capaciteit op.|
    |**Resourcegroep**|De resourcegroep met deze nieuwe capaciteit. Kiezen uit een bestaande resourcegroep of maak een nieuwe. Zie voor meer informatie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).|
    |**Power BI-capaciteit beheerder**|Power BI-capaciteit beheerders kunnen de capaciteit weergeven in de Power BI-beheerportal en toewijzing machtigingen geven voor andere gebruikers. Standaard is de beheerder van de capaciteit van uw account. De beheerder van de capaciteit moet binnen uw Power BI-tenant.|
    |**Locatie**|De locatie waar de Power BI voor uw tenant wordt gehost. Deze instelling wordt automatisch opgelost dat een andere locatie kan niet worden geselecteerd.|
    |**Prijscategorie**|Selecteer de SKU (v core-telling en het geheugen grootte) die aan uw behoeften voldoet.  Zie voor meer informatie [prijzen van Power BI Embedded](https://azure.microsoft.com/pricing/details/power-bi-embedded/)|

6. Selecteer **Maken**.

Maken duurt normaal gesproken minder dan een minuut; vaak slechts enkele seconden. Als u hebt geselecteerd **vastmaken aan dashboard**, gaat u naar het dashboard om uw nieuwe capaciteit te zien. Of Ga naar **meer services** > **Power BI Embedded** om te zien of de capaciteit van de gereed is.

![Azure portal-dashboard met Power BI Embedded capaciteit](media/create-capacity/azure-portal-dashboard.png)

## <a name="next-steps"></a>Volgende stappen

Blader naar het Power BI-beheerportal werkruimten toewijzen voor het gebruik van de capaciteit van de nieuwe Power BI Embedded. Voor meer informatie raadpleegt u [Manage capacities within Power BI Premium and Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-admin-premium-manage/) (Capaciteit beheren in Power BI Premium en Power BI Embedded).

Als u niet hoeft te gebruiken deze capaciteit, onderbreken om te stoppen facturering. Zie voor meer informatie [onderbreken en de capaciteit van de Power BI Embedded te starten in de Azure-portal](pause-start.md).

Om te beginnen met het insluiten van Power BI-inhoud in uw toepassing, Zie [insluiten uw Power BI-dashboards, rapporten en tegels](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Nog vragen? [Probeer de Power BI-Community vragen](http://community.powerbi.com/)