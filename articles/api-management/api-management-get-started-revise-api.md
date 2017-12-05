---
title: Revisies gebruiken vaste wijzigingen aanbrengen veilig in Azure API Management | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor meer informatie over hoe u vaste wijzigingen aanbrengt met wijzigingen in API Management.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 50d7ac17faebb34f1a1f9a3259aa0196950391d9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Revisies vaste wijzigingen aanbrengen veilig gebruiken
Wanneer uw API is gereed om te gaan en moet worden gebruikt door ontwikkelaars begint, moet u doorgaans Wees voorzichtig bij het aanbrengen van wijzigingen die API en op hetzelfde moment niet te verstoren aanroepfuncties van uw API. Het is ook handig om te laten weten over de aangebrachte wijzigingen ontwikkelaars. We kunt dit doen in het gebruik van Azure API Management **revisies**. Zie voor meer informatie [versies & revisies](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) en [API-versies met Azure API Management](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe revisie toevoegen
> * Vaste wijzigingen aanbrengen in uw revisie
> * Uw revisie huidige maken en toevoegen van een logboekvermelding wijzigen
> * De portal voor ontwikkelaars om de wijzigingen bekijken en wijzigen van logboek bladeren

![Het wijzigingslogboek op de Portal voor ontwikkelaars](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Vereisten

+ Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Ook de volgende zelfstudie te voltooien: [importeren en publiceren van uw eerste API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-revision"></a>Een nieuwe revisie toevoegen

1. Selecteer **API's** pagina.
2. Selecteer **conferentie API** van de API-lijst (of andere API die u wilt toevoegen revisies).
3. Klik op de **revisies** tabblad in het menu aan de bovenkant van de pagina.
4. Selecteer **+ revisie toevoegen**

    > [!TIP]
    > U kunt ook **revisie toevoegen** in het contextmenu (**...** ) van de API.
    
    ![Revisies menu aan de bovenkant van scherm](media/api-management-getstarted-revise-api/TopMenu.PNG)

5. Geef een beschrijving voor de nieuwe revisie, om te onthouden wat het wordt gebruikt voor.
6. Selecteer **maken**
7. Uw nieuwe revisie wordt nu gemaakt.

    > [!NOTE]
    > Uw oorspronkelijke API blijft in **revisie 1**. Dit is de uw gebruikers gaan om aan te roepen, totdat u ervoor kiest om een andere revisie huidige revisie.

## <a name="make-non-breaking-changes-to-your-revision"></a>Vaste wijzigingen aanbrengen in uw revisie

1. Selecteer **conferentie API** uit de lijst van de API.
2. Selecteer de **ontwerp** tabblad boven in het scherm.
3. U ziet dat de **revisie selector** (direct boven het tabblad ontwerpen) ziet u uw huidige revisie voor zover **revisie 2**.

    > [!TIP]
    > Gebruik de selector revisie schakelen tussen de wijzigingen die u wilt werken.

4. Selecteer **+ toevoegbewerking**.
5. Uw nieuwe bewerking worden **POST**, en de naam en de weergavenaam van de bewerking als **testen**
6. **Sla** uw nieuwe bewerking.
7. Er zijn nu een wijziging aangebracht **revisie 2**. Gebruik de **revisie Selector** bovenaan op de pagina overschakelen naar **revisie 1**.
8. Merk op dat uw nieuwe bewerking wordt niet weergegeven in **revisie 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Uw revisie huidige maken en toevoegen van een logboekvermelding wijzigen
1. Selecteer de **revisies** tabblad in het menu aan de bovenkant van de pagina.

    ![Het menu revisie op het scherm revisie.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
1. Open het contextmenu (**...** ) voor **revisie 2**.
2. Selecteer **instellen als huidige**. Controleer **voor deze API naar openbare wijzigingenlogboek boeken**, als u wilt plaatsen van opmerkingen over deze wijziging.
3. Selecteer **Post naar openbare wijzigingenlogboek voor deze API**
4. Geef een beschrijving op voor de wijziging die ontwikkelaars wordt weergegeven, bijvoorbeeld **revisies testen. Toegevoegde nieuwe 'test'-bewerking.**
5. **Revisie 2** nu actueel is.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>De portal voor ontwikkelaars om de wijzigingen bekijken en wijzigen van logboek bladeren
1. Selecteer in de Azure-portal **API's**
2. Selecteer **Ontwikkelaarsportal** in het menu bovenaan.
3. Selecteer **API's**, en selecteer vervolgens **conferentie API**.
4. U ziet uw nieuwe **testen** bewerking is nu beschikbaar.
5. Selecteer **API wijzigingsoverzicht** onderstaande de API-naam.
6. U ziet dat de logboekvermelding wijziging in deze lijst wordt weergegeven.

    ![ontwikkelaarsportal](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een nieuwe revisie toevoegen
> * Vaste wijzigingen aanbrengen in uw revisie
> * Uw revisie huidige maken en toevoegen van een logboekvermelding wijzigen
> * De portal voor ontwikkelaars om de wijzigingen bekijken en wijzigen van logboek bladeren

Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Publiceren van meerdere versies van uw API](api-management-get-started-publish-versions.md)