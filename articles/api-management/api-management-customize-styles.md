---
title: De paginastijl aanpassen in de ontwikkelaarsportal van Azure API Management | Microsoft Docs
description: Volg de stappen van deze snelstart voor het aanpassen van de stijl van de elementen in de ontwikkelaarsportal van Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
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
ms.openlocfilehash: d1f638c9825ea5eedf6eaee0e0ca2ccfd5a491bc
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="customize-the-style-of-the-developer-portal-pages"></a>De stijl van de pagina's van de ontwikkelaarsportal aanpassen

Er zijn drie gebruikelijke manieren waarop u de ontwikkelaarsportal in Azure API Management kunt aanpassen:
 
* [De inhoud van statische pagina's en pagina-indelingselementen bewerken](api-management-modify-content-layout.md)
* De stijlen bijwerken die worden gebruikt voor pagina-elementen in de ontwikkelaarsportal (uitgelegd in deze handleiding)
* [De sjablonen bewerken die worden gebruikt voor pagina's die worden gegenereerd door de portal](api-management-developer-portal-templates.md) (bijvoorbeeld API-documenten, producten, gebruikersverificatie, enzovoort)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Aanpassen van de stijl van elementen op pagina's van de **ontwikkelaars**portal
> * Uw wijziging bekijken

![stijl aanpassen](./media/modify-developer-portal-style/developer_portal.png)

## <a name="prerequisites"></a>Vereisten

+ Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Voltooi tevens de volgende zelfstudie: [Uw eerste API importeren en publiceren](import-and-publish.md).

## <a name="customize-the-developer-portal"></a>De ontwikkelaarsportal aanpassen

1. Selecteer **Overzicht**.
2. Klik op de knop **Ontwikkelaarsportal** bovenaan het venster **Overzicht**. U kunt ook klikken op de **Ontwikkelaarsportal URL**-link.
3. In de linkerbovenhoek van het scherm ziet u een pictogram dat bestaat uit twee schilderkwasten. Beweeg de muisaanwijzer over dit pictogram om het menu Portalaanpassing te openen.

    ![Stijl aanpassen](./media/modify-developer-portal-style/modify-developer-portal-style01.png)
4. Selecteer **Stijlen** in het menu om het deelvenster Stijl aanpassen te openen.

    Alle elementen die u kunt aanpassen met behulp van **Stijlen** worden weergegeven op de pagina
5. Voer 'headings-color' in het veld **Wijzigen van de waarden om de weergave van het ontwikkelaarsportal aan te passen:** in.

    Het element **@headings-color** wordt op de pagina weergegeven. Deze variabele bepaalt de kleur van de tekst.

    ![stijl aanpassen](./media/modify-developer-portal-style/modify-developer-portal-style02.png)
    
6. Klik op het veld voor de variabele **@headings-color**. 
    
    De vervolgkeuzelijst Kleurenkiezer wordt geopend.
7. Selecteer een nieuwe kleur uit de vervolgkeuzelijst Kleurenkiezer.

    > [!TIP]
    > Een realtime-voorbeeld is beschikbaar voor alle wijzigingen. Er verschijnt een voortgangsindicator aan de bovenkant van het deelvenster Aanpassen. Na een paar seconden verandert de koptekst naar de zojuist geselecteerde kleur.

8. Selecteer **Publiceren** linksonder in het menu van het deelvenster Aanpassen.
9. Selecteer **Aanpassingen publiceren** om de wijzigingen openbaar beschikbaar te maken.

## <a name="view-your-change"></a>Uw wijziging bekijken

1. Navigeer naar de ontwikkelaarsportal.
2. Hier ziet u de wijziging die u hebt aangebracht.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Aanpassen van de stijl van elementen op pagina's van de **ontwikkelaars**portal
> * Uw wijziging bekijken

> [!div class="nextstepaction"]
> [Met sjablonen het Azure API Management-ontwikkelaarsportal aanpassen](api-management-developer-portal-templates.md)