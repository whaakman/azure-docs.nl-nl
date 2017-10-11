---
title: Het beheren van Azure-resources met Cloud Explorer | Microsoft Docs
description: Informatie over het gebruik van Cloud Explorer om te zoeken en beheren van Azure-resources in Visual Studio.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/25/2017
ms.author: kraigb
ms.openlocfilehash: 6e6d8d559f0251b71bfa6d529ead82a246cb3235
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="manage-the-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a>De bronnen die zijn gekoppeld aan uw Azure-accounts in Visual Studio Cloud Explorer beheren
Cloud Explorer kunt u uw Azure-resources en resourcegroepen bekijken, controleren van de eigenschappen en belangrijke ontwikkelaar diagnostics acties uitvoeren vanuit in Visual Studio. 

Als de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer is gebaseerd op de Azure Resource Manager-stack. Daarom Cloud Explorer begrijpt resources, zoals Azure-resourcegroepen en Azure-services zoals Logic apps en API apps en ondersteunt de [toegangsbeheer op basis van rollen](active-directory/role-based-access-control-configure.md) (RBAC). 

## <a name="prerequisites"></a>Vereisten
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **Azure-workload** geselecteerd, of een eerdere versie van Visual Studio met de [Microsoft Azure SDK voor .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).
- Microsoft Azure-account: als u geen account hebt, kunt u [aanmelden voor een gratis proefversie](http://go.microsoft.com/fwlink/?LinkId=623901) of [uw voordelen als Visual Studio-abonnee activeren](http://go.microsoft.com/fwlink/?LinkId=623901).

> [!NOTE]
> Als u wilt weergeven Cloud Explorer, selecteer **weergave** > **Cloud Explorer** op de menubalk.   
> 
> 

## <a name="add-an-azure-account-to-cloud-explorer"></a>Toevoegen van een Azure account toe aan de Cloud Explorer
Als u wilt weergeven van de resources die zijn gekoppeld aan een Azure-account, moet u eerst de account toevoegen aan Cloud Explorer. 

1. In **Cloud Explorer**, selecteer **Azure Accountinstellingen**.

    ![Pictogram instellingen cloud Explorer Azure-account](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Selecteer **nieuwe account toevoegt**. 

    ![Koppeling voor cloud Explorer account toevoegen](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. Meld u aan bij de Azure-account waarvan bronnen die u wilt zoeken. 

1. Zodra u aangemeld bij een Azure-account, wordt de abonnementen die zijn gekoppeld aan dit account weergegeven. Schakel de selectievakjes voor de account-abonnementen die u wilt bladeren en selecteer vervolgens **toepassen**. 
 
    ![Cloud Explorer: Selecteer een Azure-abonnementen om weer te geven](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. Na het selecteren van de abonnementen waarvan u wilt bladeren bronnen weergegeven die abonnementen en bronnen in de Cloud Explorer.

    ![Cloud Explorer resource weergeven voor een Azure-account](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a>Een Azure-account verwijderen uit Cloud Explorer 

1. In **Cloud Explorer**, selecteer **Azure Accountinstellingen**.

    ![Pictogram instellingen cloud Explorer Azure-account](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Naast het account dat u wilt verwijderen, selecteert u **verwijderen**.

    ![Pictogram instellingen cloud Explorer Azure-account](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a>Brontypen of resourcegroepen weergeven
Als u wilt weergeven van uw Azure-resources, kunt u ofwel **brontypen** of **resourcegroepen** weergeven.

1. In **Cloud Explorer**, selecteert u de vervolgkeuzelijsten resource.

    ![Cloud Explorer de vervolgkeuzelijst de gewenste bronnen weergave selecteren](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. Selecteer de gewenste weergave in het contextmenu: 

    - **Brontypen** view - de algemene weergave gebruikt op de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), ziet u uw Azure-resources ingedeeld volgens hun type, zoals web-apps, storage-accounts en virtuele machines. 
    - **Resourcegroepen** view - categoriseert Azure-bronnen door de Azure-resourcegroep waaraan ze gekoppeld zijn. Een resourcegroep is een bundel van Azure-resources, meestal worden gebruikt door een specifieke toepassing. Zie voor meer informatie over Azure-resourcegroepen, [overzicht van Azure Resource Manager](./azure-resource-manager/resource-group-overview.md).

    De volgende afbeelding toont een vergelijking van de twee weergaven:

    ![Cloud Explorer resource weergaven vergelijking](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a>Weergeven en bronnen in de Cloud Explorer navigeert
Om te navigeren naar een Azure-resource en de informatie in de Cloud Explorer bekijken, vouwt u het type of de bijbehorende brongroep van het item en selecteer vervolgens de resource. Wanneer u een resource selecteert, wordt informatie weergegeven in de twee tabbladen - **acties** en **eigenschappen** - aan de onderkant van de Cloud Explorer. 

- **Acties** tabblad - vindt u de acties die u kunt nu in de Cloud Explorer voor de geselecteerde bron. U kunt ook deze opties weergeven met de rechtermuisknop op de bron om de contextmenu weer te geven.

- **Eigenschappen** tabblad - geeft de eigenschappen van de resource, zoals de type Landinstellingen en resource-groep waaraan deze gekoppeld is.

De volgende afbeelding toont een voorbeeld van de vergelijking van wat u op elk tabblad voor een App Service ziet:

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

Elke resource heeft de actie **openen in de portal**. Als u deze actie kiest, Cloud Explorer worden weergegeven voor de geselecteerde resource in de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). De **openen in de portal** functie is handig voor het navigeren naar diep geneste resources.

Aanvullende acties en eigenschapswaarden mogelijk ook weergegeven op basis van de Azure-resource. Bijvoorbeeld, web-apps en logic apps hebben de acties **openen in browser** en **foutopsporingsprogramma koppelen** naast **openen in de portal**. Bewerkingen voor het openen van editors worden weergegeven wanneer u een account-opslag-blob, wachtrij of tabel. Apps van Azure hebben **URL** en **Status** eigenschappen, terwijl de opslagbronnen eigenschappen van sleutel en verbinding-tekenreeks zijn.

## <a name="find-resources-in-cloud-explorer"></a>Resources zoeken in de Cloud Explorer
Voer de naam in om te zoeken resources met een specifieke naam in de abonnementen voor uw Azure-account, de **Search** vak in Cloud Explorer.

![Resources zoeken in de Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

Als u tekens in de **Search** vak alleen bronnen die overeenkomen met die tekens worden weergegeven in de resourcestructuur.
