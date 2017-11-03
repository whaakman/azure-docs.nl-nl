---
title: Revisies gebruiken voor het bijwerken van uw API in Azure API Management | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor meer informatie over hoe u vaste wijzigingen aanbrengt met wijzigingen in API Management.
services: api-management
documentationcenter: 
author: mattfarm
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2017
ms.author: apimpm
ms.openlocfilehash: 0d67166a16ae4d640080ad83e7625e594b0dc246
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="make-non-breaking-changes-safely-using-revisions"></a>Niet-grote wijzigingen veilig gebruik revisies
Deze zelfstudie wordt beschreven hoe u veilig wijzigingen aanbrengen in uw API en de wijziging voor uw ontwikkelaars communiceren.

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie die u wilt maken van een API Management-Service en hebt een bestaande API u (in plaats van de Conferentie API wijzigen kunt).

## <a name="about-revisions"></a>Over revisies
Wanneer uw API is gereed om te gaan en moet worden gebruikt door ontwikkelaars begint, moet u doorgaans Wees voorzichtig bij het aanbrengen van wijzigingen in deze API - niet te verstoren aanroepfuncties van uw API. Het is ook handig om te laten weten over de aangebrachte wijzigingen ontwikkelaars. We kunt dit doen in het gebruik van Azure API Management **revisies**.

## <a name="walkthrough"></a>Walkthrough
In dit overzicht we een nieuwe revisie toevoegen, een bewerking toevoegen aan en die herziening huidige - maken van een logboekvermelding wijzigen wanneer we maken.

## <a name="add-a-new-revision"></a>Een nieuwe revisie toevoegen
1. Blader naar de **API's** pagina binnen uw API Management-service in de Azure portal.
2. Selecteer **conferentie API** uit de lijst API, selecteer de **revisies** tabblad in het menu aan de bovenkant van de pagina.
3. Selecteer **+ revisie toevoegen**

    > [!TIP]
    > U kunt ook **revisie toevoegen** in het contextmenu (**...** ) op de API.
![Revisies menu aan de bovenkant van scherm](media/api-management-getstarted-revise-api/TopMenu.PNG)

4. Geef een beschrijving voor de nieuwe revisie, om te onthouden wat het wordt gebruikt voor.
5. Selecteer **maken**
6. Uw nieuwe revisie wordt nu gemaakt.

    > [!NOTE]
    > Oorspronkelijke API blijft in **revisie 1**. Dit is de blijven uw gebruikers om aan te roepen, totdat u ervoor kiest om een andere revisie huidige revisie.

## <a name="make-non-breaking-changes-to-your-revision"></a>Vaste wijzigingen aanbrengen in uw revisie
1. Selecteer de **ontwerp** tabblad boven in het scherm.
2. U ziet dat de **revisie selector** (direct boven het tabblad ontwerpen) ziet u uw huidige revisie voor zover **revisie 2**.

    > [!TIP]
    > Gebruik de selector revisie schakelen tussen de wijzigingen die u wilt werken.

3. Selecteer **+ toevoegbewerking**.
4. Uw nieuwe bewerking worden **POST**, en de naam en de weergavenaam van de bewerking als **Feedback**
5. **Sla** uw nieuwe bewerking.
6. Er zijn nu een wijziging aangebracht **revisie 2**. Gebruik de **revisie Selector** bovenaan op de pagina overschakelen naar **revisie 1**.
7. Merk op dat uw nieuwe bewerking wordt niet weergegeven in **revisie 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Uw revisie huidige maken en toevoegen van een logboekvermelding wijzigen
1. Selecteer de **revisies** tabblad in het menu aan de bovenkant van de pagina.
![Het menu revisie op het scherm revisie.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
2. Open het contextmenu (**...** ) voor **revisie 2**.
3. Selecteer **instellen als huidige**.
![Revisie huidige maken en om te wijzigen van logboek plaatsen](media/api-management-getstarted-revise-api/MakeCurrent.PNG)
4. Selecteer **Post naar openbare wijzigingenlogboek voor deze API**
5. Geef een beschrijving op voor de wijziging die ontwikkelaars wordt weergegeven, bijvoorbeeld **'Nieuwe Feedback bewerking toegevoegd'.**
6. **Revisie 2** nu actueel is.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>De portal voor ontwikkelaars om de wijzigingen bekijken en wijzigen van logboek bladeren
1. Selecteer **Ontwikkelaarsportal** in het menu bovenaan.
2. Selecteer **API's**, en selecteer vervolgens **conferentie API**.
3. U ziet uw nieuwe **Feedback** bewerking is nu beschikbaar.
4. Selecteer **API wijzigingsoverzicht** onderstaande de API-naam.
5. U ziet dat de logboekvermelding wijziging in deze lijst wordt weergegeven.
![Het wijzigingslogboek op de Portal voor ontwikkelaars](media/api-management-getstarted-revise-api/ChangeLogDevPortal.PNG)

## <a name="next-steps"></a>Volgende stappen
[Publiceren van API-versies met Azure API Management](#api-management-getstarted-publish-versions.md)