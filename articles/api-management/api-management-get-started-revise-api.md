---
title: Revisies gebruiken om vaste wijzigingen veilig in Azure API Management aan te brengen | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor meer informatie over hoe u vaste wijzigingen aanbrengt met revisies in API Management.
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: b4812ea8d93e4bfb784370e3a3196a5d20e47519
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Revisies gebruiken om vaste wijzigingen veilig in Azure API Management aan te brengen
Wanneer uw API gereed is voor de start en moet worden gebruikt door ontwikkelaars, moet u doorgaans voorzichtig zijn bij het aanbrengen van wijzigingen aan die API en tegelijkertijd niet aanroepfuncties van uw API verstoren. Het is ook handig om ontwikkelaars te informeren over de aangebrachte wijzigingen. We kunnen dit doen in Azure API Management met **revisies**. Zie voor meer informatie [Versies en revisies](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) en [API-versies met Azure API Management](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe revisie toevoegen
> * Vaste wijzigingen aanbrengen in uw revisie
> * Uw revisie actualiseren en een logboekvermelding over de wijziging toevoegen
> * Door de portal voor ontwikkelaars browsen om de wijzigingen en het logboek in te zien

![Wijzigingenlogboek voor de ontwikkelaarsportal](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Vereisten

+ Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Voltooi tevens de volgende zelfstudie: [Uw eerste API importeren en publiceren](import-and-publish.md).

## <a name="add-a-new-revision"></a>Een nieuwe revisie toevoegen

1. Selecteer de pagina **API's**.
2. Selecteer **Conferentie-API** uit de API-lijst (of andere API waaraan u revisies wilt toevoegen).
3. Klik op het tabblad **Revisies** in het menu aan de bovenkant van de pagina.
4. Selecteer **+ Revisie toevoegen**

    > [!TIP]
    > U kunt ook **Revisie toevoegen** kiezen uit het contextmenu (**...** ) van de API.
    
    ![Revisies-menu aan de bovenkant van scherm](media/api-management-getstarted-revise-api/TopMenu.PNG)

5. Geef een beschrijving voor de nieuwe revisie, om te onthouden waar deze voor wordt gebruikt.
6. Selecteer **Maken**
7. Uw nieuwe revisie wordt nu gemaakt.

    > [!NOTE]
    > Uw oorspronkelijke API blijft in **Revisie 1**. Dit is de revisie die uw gebruikers blijven aanroepen, totdat u ervoor kiest om een andere revisie de huidige te maken.

## <a name="make-non-breaking-changes-to-your-revision"></a>Vaste wijzigingen aanbrengen in uw revisie

1. Selecteer **Conferentie-API** uit de lijst van de API.
2. Selecteer boven in het scherm het tabblad **Ontwerp**.
3. U ziet dat de **Revisieselector** (direct boven het tabblad Ontwerpen) uw huidige revisie toont als **Revisie 2**.

    > [!TIP]
    > Gebruik de revisieselector om te schakelen tussen de wijzigingen waaraan u wilt werken.

4. Klik op **+ Bewerking toevoegen**.
5. Stel uw nieuwe bewerking in als **POST** en de naam en weergavenaam van de bewerking als **Test**
6. **Sla** uw nieuwe bewerking op.
7. We hebben nu een wijziging aangebracht in **Revisie 2**. Gebruik de **Revisieselector** bovenaan op de pagina om over te schakelen naar **Revisie 1**.
8. Merk op dat uw nieuwe bewerking niet wordt weergegeven in **Revisie 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Uw revisie actualiseren en een logboekvermelding over de wijziging toevoegen
1. Klik op het tabblad **Revisies** in het menu aan de bovenkant van de pagina.

    ![Het menu Revisie op het scherm Revisie.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
1. Open het contextmenu (**...** ) voor **Revisie 2**.
2. Selecteer **Instellen als huidige**. Controleer **Op het openbare wijzigingenlogboek voor deze API plaatsen**, als u opmerkingen over deze wijziging wilt plaatsen.
3. Selecteer **Op het openbare wijzigingenlogboek voor deze API plaatsen**
4. Geef een beschrijving op voor de wijziging die ontwikkelaars wordt weergegeven, bijvoorbeeld **Revisies testen. Nieuwe 'test'-bewerking toegevoegd.**
5. **Revisie 2** is nu actueel.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Door de portal voor ontwikkelaars browsen om de wijzigingen en het logboek in te zien
1. Selecteer **API's** in Azure Portal
2. Selecteer **ontwikkelaarsportal** in het menu bovenaan.
3. Selecteer **API's** en selecteer vervolgens **Conferentie API**.
4. U ziet dat uw nieuwe **test**bewerking nu beschikbaar is.
5. Selecteer **API-wijzigingsoverzicht** onder de API-naam.
6. U ziet dat de vermelding van het wijzigingslogboek in deze lijst wordt weergegeven.

    ![ontwikkelaarsportal](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een nieuwe revisie toevoegen
> * Vaste wijzigingen aanbrengen in uw revisie
> * Uw revisie actualiseren en een logboekvermelding over de wijziging toevoegen
> * Door de portal voor ontwikkelaars browsen om de wijzigingen en het logboek in te zien

Ga door naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Meerdere versies van uw API publiceren](api-management-get-started-publish-versions.md)