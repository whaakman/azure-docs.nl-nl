---
title: Azure-resources beheren met Cloud Explorer | Microsoft Docs
description: Leer hoe u Cloud Explorer gebruiken om te bladeren en beheren van Azure-resources vanuit Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/25/2017
ms.author: ghogen
ms.openlocfilehash: a4be3f063c51f26e1df9ecf4cca4ae0655554750
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054308"
---
# <a name="manage-the-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a>Beheren van de resources die zijn gekoppeld aan uw Azure-accounts in Visual Studio Cloud Explorer
Cloud Explorer kunt u weergeven van uw Azure-resources en resourcegroepen, hun eigenschappen inspecteren en voert u belangrijke ontwikkelaar diagnostische gegevens over acties op basis van Visual Studio. 

Net als de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer is gebouwd op de Azure Resource Manager-stack. Daarom Cloud Explorer begrijpt resources, zoals Azure-resourcegroepen en Azure-services zoals Logic apps en API apps en ondersteunt [op rollen gebaseerd toegangsbeheer](role-based-access-control/role-assignments-portal.md) (RBAC). 

## <a name="prerequisites"></a>Vereisten
- [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de **Azure-workload** hebt geselecteerd, of een eerdere versie van Visual Studio met de [Microsoft Azure SDK voor .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).
- Microsoft Azure-account - als u geen account hebt, kunt u [zich registreren voor een gratis proefversie](http://go.microsoft.com/fwlink/?LinkId=623901) of [uw voordelen als Visual Studio-abonnee activeren](http://go.microsoft.com/fwlink/?LinkId=623901).

> [!NOTE]
> Als u Cloud Explorer, selecteer **weergave** > **Cloud Explorer** op de menubalk.   
> 
> 

## <a name="add-an-azure-account-to-cloud-explorer"></a>Toevoegen van een Azure-account aan Cloud Explorer
Als u de resources die zijn gekoppeld aan een Azure-account, moet u eerst het account toevoegen aan Cloud Explorer. 

1. In **Cloud Explorer**, selecteer **Azure-Accountinstellingen**.

    ![Pictogram instellingen cloud Explorer Azure-account](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Selecteer **nieuwe account toevoegt**. 

    ![Koppeling van cloud Explorer account toevoegen](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. Meld u aan bij de Azure-account aan waarvan bronnen die u wilt zoeken. 

1. Zodra u aangemeld bij een Azure-account, wordt de abonnementen die zijn gekoppeld aan dit account weergegeven. Schakel de selectievakjes voor de account-abonnementen die u wilt bladeren en selecteer vervolgens **toepassen**. 
 
    ![Cloud Explorer: Selecteer Azure-abonnementen om weer te geven](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. Na het selecteren van de abonnementen waarvan bronnen die u wilt bladeren, worden deze abonnementen en resources in de Cloud Explorer weergegeven.

    ![Cloud Explorer resource weergeven voor een Azure-account](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a>Een Azure-account verwijderen uit Cloud Explorer 

1. In **Cloud Explorer**, selecteer **Azure-Accountinstellingen**.

    ![Pictogram instellingen cloud Explorer Azure-account](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Naast het account dat u wilt verwijderen, selecteert u **verwijderen**.

    ![Pictogram instellingen cloud Explorer Azure-account](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a>Resourcetypen of resourcegroepen weergeven
Als u uw Azure-resources, kunt u een **resourcetypen** of **resourcegroepen** weergeven.

1. In **Cloud Explorer**, selecteer de vervolgkeuzelijst van resource weergeven.

    ![Cloud Explorer de vervolgkeuzelijst de gewenste resources weergave selecteren](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. Selecteer de gewenste weergave in het contextmenu: 

    - **Resourcetypen** view - de algemene weergave gebruikt op de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), ziet u uw Azure-resources gecategoriseerd door hun type, zoals web-apps, opslagaccounts en virtuele machines. 
    - **Resourcegroepen** view - categoriseert Azure-resources door de Azure-resourcegroep waarmee ze gekoppeld zijn. Een resourcegroep is een bundel van Azure-resources, doorgaans gebruikt door een specifieke toepassing. Zie voor meer informatie over Azure-resourcegroepen, [overzicht van Azure Resource Manager](./azure-resource-manager/resource-group-overview.md).

    De volgende afbeelding ziet u een vergelijking van de twee weergaven:

    ![Vergelijking van weergaven resource Explorer voor cloud](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a>Weergeven en door resources in de Cloud Explorer navigeren
Navigeer naar een Azure-resource en de informatie in de Cloud Explorer weergeven, vouwt u het type of de bijbehorende resourcegroep van het item en selecteer vervolgens de resource. Wanneer u een resource selecteert, informatie wordt weergegeven in de twee tabbladen - **acties** en **eigenschappen** - aan de onderkant van de Cloud Explorer. 

- **Acties** tabblad: geeft een lijst van de acties die u in de Cloud Explorer voor de geselecteerde resource uitvoeren kunt. U kunt ook deze opties weergeven met de rechtermuisknop op de resource om het contextmenu weer te geven.

- **Eigenschappen** tabblad - geeft de eigenschappen van de resource, zoals het type, de landinstelling en resource-groep waaraan deze gekoppeld is.

De volgende afbeelding toont een voorbeeld van de vergelijking van wat er op elk tabblad voor een App Service:

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

Elke resource heeft de actie **openen in portal**. Als u deze actie kiest, Cloud Explorer ziet u de geselecteerde bron in de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). De **openen in portal** functie is handig om te navigeren naar diep geneste resources.

Aanvullende acties en eigenschapswaarden mogelijk ook weergegeven op basis van de Azure-resource. Bijvoorbeeld, web-apps en logic apps ook zijn de acties **openen in browser** en **foutopsporingsprogramma koppelen** naast **openen in portal**. Bewerkingen voor het openen van editors worden weergegeven wanneer u ervoor een logboekopslagaccount-blob, wachtrij of tabel kiest. Azure-apps hebben **URL** en **Status** eigenschappen, terwijl storage-resources eigenschappen van een verbindingsreeks sleutel en -verbinding hebben.

## <a name="find-resources-in-cloud-explorer"></a>Resources zoeken in de Cloud Explorer
Als u wilt zoeken resources met een specifieke naam in uw Azure-account-abonnementen, voer de naam in de **zoeken** vak in Cloud Explorer.

![Zoeken naar bronnen in de Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

Als u tekens in de **zoeken** vak, alleen de resources die overeenkomen met die tekens worden weergegeven in de resourcestructuur van de.
