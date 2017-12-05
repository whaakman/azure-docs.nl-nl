---
title: De paginastijl op de Azure API Management-portal voor ontwikkelaars aanpassen | Microsoft Docs
description: Volg de stappen van deze snelstartgids voor het aanpassen van de stijl van de elementen op de Azure API Management-portal voor ontwikkelaars.
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
ms.openlocfilehash: f427663ba1c437785c8c521925d9f733c45cb40d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="customize-the-style-of-the-developer-portal-pages"></a>De stijl van de portal-pagina's van ontwikkelaars aanpassen

Er zijn drie meest voorkomende manieren naar de ontwikkelaarsportal aanpassen in Azure API Management:
 
* [De inhoud van statische pagina's en pagina-elementen voor lay-out bewerken](api-management-modify-content-layout.md)
* Bijwerken van de stijlen die wordt gebruikt voor pagina-elementen in de portal voor ontwikkelaars (uitgelegd in deze handleiding)
* [Wijzigen van de sjablonen voor pagina's die worden gegenereerd door de portal gebruikt](api-management-developer-portal-templates.md) (bijvoorbeeld API docs, producten, gebruikersverificatie)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Aanpassen van de stijl van de elementen op de pagina's van de **Developer** portal
> * De wijziging van uw weergeven

![stijl aanpassen](./media/modify-developer-portal-style/developer_portal.png)

## <a name="prerequisites"></a>Vereisten

+ Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Ook de volgende zelfstudie te voltooien: [importeren en publiceren van uw eerste API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="customize-the-developer-portal"></a>De ontwikkelaarsportal aanpassen

1. Selecteer **overzicht**.
2. Klik op de **ontwikkelaarsportal** knop op boven van de **overzicht** venster. U kunt ook klikken op de **ontwikkelaarsportal URL** koppeling.
3. In de linkerbovenhoek van het scherm ziet u een pictogram dat bestaat uit twee Schilderkwasten. Beweeg de muisaanwijzer over dit pictogram om het menu portalaanpassing te openen.

    ![stijl aanpassen](./media/modify-developer-portal-style/modify-developer-portal-style01.png)
4. Selecteer **stijlen** in het menu openen van het deelvenster stijl aanpassen.

    Alle elementen die u kunt aanpassen met behulp van **stijlen** weergegeven op de pagina
5. Voer 'headings-color' in de **wijzigen de waarden van variabelen developer portal weergave aanpassen:** veld.

    De  **@headings-color**  element op de pagina wordt weergegeven. Deze variabele bepaalt de kleur van de tekst.

    ![stijl aanpassen](./media/modify-developer-portal-style/modify-developer-portal-style02.png)
    
6. Klik op het veld voor de  **@headings-color**  variabele. 
    
    Kleurenkiezer vervolgkeuzelijst wordt geopend.
7. Selecteer een nieuwe kleur van de kleurkiezers vervolgkeuzelijst.

    > [!TIP]
    > Realtime-voorbeeld is beschikbaar voor alle wijzigingen. Er verschijnt een voortgangsindicator aan de bovenkant van het deelvenster aanpassen. Na een paar seconden wijzigingen de koptekst in de zojuist geselecteerde in kleur.

8. Selecteer **publiceren** van linksonder in het deelvenster aanpassing menu.
9. Selecteer **aanpassingen publiceert** om de wijzigingen openbaar beschikbaar.

## <a name="view-your-change"></a>De wijziging van uw weergeven

1. Navigeer naar de portal voor ontwikkelaars.
2. Hier ziet u de wijziging die u hebt aangebracht.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Aanpassen van de stijl van de elementen op de pagina's van de **Developer** portal
> * De wijziging van uw weergeven

> [!div class="nextstepaction"]
> [De sjablonen met Azure API Management-ontwikkelaarsportal aanpassen](api-management-developer-portal-templates.md)