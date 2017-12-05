---
title: Versies van uw API met Azure API Management publiceren | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor informatie over het publiceren van meerdere versies in API Management.
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
ms.openlocfilehash: d63bdd3110f5c5db3e7bfec424644fdbc8d8d90c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="publish-multiple-versions-of-your-api"></a>Publiceren van meerdere versies van uw API 

Er zijn tijden wanneer is het niet praktisch alle aanroepfuncties voor uw API-gebruik hebben dezelfde versie. Soms wilt u een nieuwe of andere API-functies om bepaalde gebruikers publiceren, terwijl anderen wilt houden met de API die voor hen wordt momenteel ondersteund. Wanneer aanroepfuncties upgraden naar een nieuwere versie wilt, die ze willen kunnen doen dit met een gemakkelijk te begrijpen benadering.  We kunnen dit doen met **versies** in Azure API Management. Zie voor meer informatie [versies & revisies](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe versie toevoegen aan een bestaande API
> * Kiest u een schema versie
> * De versie op een product toevoegen
> * De portal voor ontwikkelaars om te zien van de versie bladeren

![Versie die wordt weergegeven op de portal voor ontwikkelaars](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Vereisten

+ Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Ook de volgende zelfstudie te voltooien: [importeren en publiceren van uw eerste API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-version"></a>Voeg een nieuwe versie

![Contextmenu van API - versie toevoegen](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Selecteer **conferentie API** uit de lijst van de API.
2. Selecteer het snelmenu (**...** ) ernaast.
3. Selecteer **+ versie toevoegen**.

    > [!TIP]
    > Versies kunnen ook worden ingeschakeld wanneer u eerst een nieuwe API - maakt Selecteer **versie deze API?** op de **API toevoegen** scherm.

## <a name="choose-a-versioning-scheme"></a>Kies een schema

Azure API Management kunt u kiezen op de manier waarop u aanroepfuncties kunt u opgeven welke versie van uw API ze wilt toestaan. Dit doet u door het kiezen van een **schema**. Dit schema kan zijn **pad, koptekst of query-tekenreeks**. In ons voorbeeld gebruiken we pad.

![Versiescherm toevoegen](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Laat **pad** geselecteerde als uw **schema**.
2. Voeg **v1** als uw **versie-id**.

    > [!TIP]
    > Als u selecteert **header** of **querytekenreeks** als een schema, moet u een extra waarde - Geef de naam van de header of querytekenreeksparameter.

3. Geef een beschrijving als u wenst.
4. Selecteer **maken** voor het instellen van uw nieuwe versie.
5. Onder **Big conferentie API** in de lijst API ziet u nu twee verschillende API's - **oorspronkelijke**, en **v1**.

    ![Versies vermeld in een API in de Azure portal](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Als u een versie aan een niet-samengestelde API toevoegt, maken we altijd een **oorspronkelijke** - u reageert op de standaard-URL. Dit zorgt ervoor dat eventuele bestaande aanroepfuncties niet worden onderbroken door het proces van het toevoegen van een versie. Als u een nieuwe API met versies die aan het begin is ingeschakeld maakt, wordt een oorspronkelijke niet gemaakt.

6. U kunt nu bewerken en configureren **v1** als een API die is gescheiden naar **oorspronkelijke**. Wijzigingen in één versie hebben geen invloed op een andere.

## <a name="add-the-version-to-a-product"></a>De versie op een product toevoegen

Voor aanroepers naar de nieuwe versie zien, moet deze worden toegevoegd aan een **product** (producten wordt niet overgenomen van bovenliggende versies).

1. Selecteer **producten** op de servicepagina-beheer.
2. Selecteer **onbeperkte**.
3. Selecteer **API's**.
4. Selecteer **Toevoegen**.
5. Selecteer **conferentie API, versie v1**.
6. Terug naar de servicepagina-beheer en selecteer **API's**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>De portal voor ontwikkelaars om te zien van de versie bladeren

1. Selecteer **Ontwikkelaarsportal** in het menu bovenaan.
2. Selecteer **API's**, zoals u ziet **conferentie API** toont **oorspronkelijke** en **v1** versies.
3. Selecteer **v1**.
4. U ziet de **verzoek-URL** van de eerste bewerking in de lijst. Er wordt weergegeven dat het API-URL-pad bevat **v1**.

    ![Contextmenu van API - versie toevoegen](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe versie toevoegen aan een bestaande API
> * Kiest u een schema versie 
> * De versie op een product toevoegen
> * De portal voor ontwikkelaars om te zien van de versie bladeren

Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Upgraden en schaal aanpassen](upgrade-and-scale.md)