---
title: Beheren van uw sleutels in LUIS | Microsoft Docs
description: Language Understanding (LUIS) gebruiken voor het beheren van uw programmeer-API, het eindpunt en de externe sleutels.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 80064584468c109d0bad49f1a22c485fa9cce846
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344515"
---
# <a name="manage-your-luis-keys"></a>Uw LUIS-sleutels beheren
Een sleutel kunt u maken en publiceren van uw LUIS-app of query uitvoeren op uw eindpunt. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
## <a name="key-concepts"></a>Belangrijkste concepten
Zie [sleutels in LUIS](luis-concept-keys.md) LUIS ontwerpen en het eindpunt belangrijkste concepten begrijpen.

<a name="create-and-use-an-endpoint-key"></a>
## <a name="assign-endpoint-key"></a>Eindpuntsleutel toewijzen
Op de **app publiceren** pagina, er is al een sleutel in de **Resources en sleutels** tabel. Dit is de sleutel van de ontwerphandleiding (starter). 

1. Maak een LUIS-sleutel op de [Azure-portal](https://portal.azure.com). Zie voor verdere instructies [maken van de eindpuntsleutel van een met behulp van Azure](luis-how-to-azure-subscription.md).
 
2. Als u wilt toevoegen de LUIS-sleutel in de vorige stap hebt gemaakt, klikt u op de **sleutel toevoegen** te openen de **een sleutel toewijzen aan uw app** dialoogvenster. 

    ![Een sleutel toewijzen aan uw app](./media/luis-manage-keys/assign-key.png)
3. Selecteer een Tenant in het dialoogvenster. 
 
    > [!Note]
    > In Azure vertegenwoordigt een tenant de Azure Active Directory-ID van de client of organisatie die zijn gekoppeld aan een service. Als u eerder hebt aangemeld voor een Azure-abonnement met uw afzonderlijke Microsoft-Account, al hebt u een tenant! Wanneer u zich aanmeldt bij Azure portal, u bent automatisch aangemeld bij [uw standaardtenant](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant). U kunt deze tenant gebruiken zijn, maar u mogelijk wilt maken van een organisatie-administrator-account.

4. Kies het Azure-abonnement dat is gekoppeld aan de Azure LUIS-sleutel die u wilt toevoegen.

5. Selecteer het Azure LUIS-account. De regio van het account dat wordt weergegeven tussen haakjes. 

    ![Kies de sleutel](./media/luis-manage-keys/assign-key-filled-out.png)

6. Nadat u deze eindpuntsleutel toewijst, kunt u dit in alle endpoint-query's gebruiken. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="publishing-regions"></a>Publicatie-regio 's
Meer informatie over publiceren [regio's](luis-reference-regions.md) inclusief publicatie in [Europa](luis-reference-regions.md#publishing-to-europe), en [Australia](luis-reference-regions.md#publishing-to-australia). Publicatie-regio's wijken af van het ontwerpen van regio's. Zorg ervoor dat u een app maken in de ontwerphandleiding regio overeenkomt met de gewenste regio voor publiceren.

## <a name="unassign-key"></a>Sleutel verwijderen

* In de **lijst met Resources en sleutels**, klikt u op het prullenbakpictogram naast de entiteit die u wilt intrekken. Klik vervolgens op **OK** in het bevestigingsbericht om verwijdering te bevestigen.
 
    ![Entiteit verwijderen](./media/luis-manage-keys/unassign-key.png)

> [!NOTE]
> Intrekken van de sleutel LUIS, worden deze niet verwijderd uit uw Azure-abonnement.

## <a name="next-steps"></a>Volgende stappen

Gebruikt u uw sleutel voor het publiceren van uw app in de **app publiceren** pagina. Zie voor instructies over het publiceren, [app publiceren](luis-how-to-publish-app.md).

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website