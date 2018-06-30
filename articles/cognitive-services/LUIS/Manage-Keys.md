---
title: Beheren van uw sleutels in LUIS | Microsoft Docs
description: Language Understanding (LUIS) gebruiken voor het beheren van uw programmatische API, eindpunt en externe sleutels.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 1fdf0f21697e48fd5cb9eca4212596059a9519cb
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127114"
---
# <a name="manage-your-luis-keys"></a>Uw LUIS sleutels beheren
Een sleutel kunt u ontwerpen en publiceren van uw app LUIS of uw eindpunt opvragen. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
## <a name="key-concepts"></a>Belangrijkste concepten
Zie [sleutels in LUIS](luis-concept-keys.md) LUIS ontwerp- en eindpunt belangrijke concepten te begrijpen.

<a name="create-and-use-an-endpoint-key"></a>
## <a name="assign-endpoint-key"></a>Eindpuntsleutel toewijzen
Op de **publiceren app** pagina, er is al een sleutel in de **Resources en sleutels** tabel. Dit is de authoring (starter)-sleutel. 

1. Maak een LUIS-sleutel op de [Azure-portal](https://portal.azure.com). Zie voor verdere instructies [maken van de eindpuntsleutel van een met behulp van Azure](luis-how-to-azure-subscription.md).
 
2. Om te kunnen toevoegen de LUIS-sleutel in de vorige stap hebt gemaakt, klikt u op de **sleutel toevoegen** te openen de **een toewijzen aan uw app** dialoogvenster. 

    ![Een toewijzen aan uw app](./media/luis-manage-keys/assign-key.png)
3. Selecteer een Tenant in het dialoogvenster. 
 
    > [!Note]
    > In Azure vertegenwoordigt een tenant de Azure Active Directory-ID van de client of organisatie die is gekoppeld aan een service. Als u eerder hebt aangemeld voor een Azure-abonnement met uw afzonderlijke Microsoft-Account, al hebt u een tenant! Wanneer u zich bij de Azure-portal aanmelden, u bent automatisch aangemeld bij [uw tenant standaard](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant). U kunt deze tenant gebruiken zijn, maar u mogelijk wilt maken van een organisatie-administrator-account.

4. Kies het Azure-abonnement gekoppeld aan de Azure LUIS sleutel die u wilt toevoegen.

5. Selecteer het LUIS Azure-account. De regio van het account wordt weergegeven tussen haakjes. 

    ![Kies de sleutel](./media/luis-manage-keys/assign-key-filled-out.png)

6. Nadat u deze eindpuntsleutel toewijst, worden gebruikt in alle endpoint-query's. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="publishing-regions"></a>Publishing regio 's
Meer informatie over publiceren [regio's](luis-reference-regions.md) inclusief publicatie in [Europa](luis-reference-regions.md#publishing-to-europe), en [Australië](luis-reference-regions.md#publishing-to-australia). Publishing regio's zijn anders dan de regio's ontwerpen. Zorg ervoor dat u een app in de ontwerphandleiding regio die overeenkomt met de publicatie regio die u wilt maken.

## <a name="unassign-key"></a>Toewijzing van de sleutel

* In de **lijst met bronnen en sleutels**, klikt u op het prullenbakpictogram bin naast de entiteit die u wilt intrekken. Klik vervolgens op **OK** in het bevestigingsbericht verwijderen te bevestigen.
 
    ![Toewijzing van entiteit](./media/luis-manage-keys/unassign-key.png)

> [!NOTE]
> Intrekken van de sleutel LUIS verwijderd niet van uw Azure-abonnement.

## <a name="next-steps"></a>Volgende stappen

Uw sleutel gebruiken voor het publiceren van uw app in de **publiceren app** pagina. Zie voor instructies voor het publiceren, [publiceren app](PublishApp.md).

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website