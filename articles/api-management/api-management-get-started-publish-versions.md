---
title: Versies van uw API met Azure API Management publiceren | Microsoft Docs
description: Volg de stappen in deze zelfstudie voor informatie over het publiceren van meerdere versies in Azure API Management.
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
ms.openlocfilehash: 7d61fa25f29f1380fe58069dbc25ed9ce1fff53c
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="publish-multiple-versions-of-your-api"></a>Meerdere versies van uw API publiceren 

Er zijn tijden wanneer het niet praktisch is dat alle aanroepers voor uw API dezelfde versie gebruiken. Wanneer aanroepers willen upgraden naar een nieuwere versie willen ze dit met een gemakkelijk te begrijpen benadering kunnen doen. Het is mogelijk om dit te doen met behulp van **Versies** in Azure API Management. Zie voor meer informatie [Versies en revisies](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe versie toevoegen aan een bestaande API
> * Een versieschema kiezen
> * Voeg de versie toe aan een product
> * Blader door de portal voor ontwikkelaars om de versie te zien

![Versie die wordt weergegeven op de portal voor ontwikkelaars](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Vereisten

* Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
* Voltooi tevens de volgende zelfstudie: [Uw eerste API importeren en publiceren](import-and-publish.md).

## <a name="add-a-new-version"></a>Een nieuwe versie toevoegen

![Contextmenu van API - versie toevoegen](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Selecteer **Conferentie-API** uit de lijst van de API.
2. Selecteer het contextmenu (**...** ) ernaast.
3. Selecteer **+ Versie toevoegen**.

    > [!TIP]
    > Versies kunnen ook worden ingeschakeld wanneer u eerst een nieuwe API maakt - selecteer **Versie van deze API?** op het scherm **API toevoegen**.

## <a name="choose-a-versioning-scheme"></a>Kies een versiebeheerschema

Met Azure API Management kunt u de manier kiezen waarop u het aanroepers mogelijk maakt om op te geven welke versie van uw API ze willen. U geeft op welke API-versie moet worden gebruikt door het selecteren van een **Versiebeheerschema**. Dit schema kan **pad, koptekst of query-tekenreeks**zijn. In het volgende voorbeeld wordt pad gebruikt voor het selecteren van het versieschema.

![Versiescherm toevoegen](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Laat **pad** geselecteerd als uw **versiebeheerschema**.
2. Voeg **v1** toe als uw **versie-id**.

    > [!TIP]
    > Als u **header** of **querytekenreeks** selecteert als een versiebeheerschema, moet u een extra waarde opgeven: de naam van de header of query-tekenreeksparameter.

3. Geef een beschrijving als u wenst.
4. Selecteer **Maken** voor het instellen van uw nieuwe versie.
5. Onder **Grote conferentie API** in de lijst API ziet u nu twee verschillende API's: **Origineel** en **v1**.

    ![Versies vermeld in een API in Azure Portal](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Als u een versie aan een niet-samengestelde API toevoegt, wordt automatisch een **Origineel** gemaakt dat reageert op de standaard-URL. Dit zorgt ervoor dat eventuele bestaande aanroepfuncties niet worden onderbroken door het proces van het toevoegen van een versie. Als u een nieuwe API met versies maakt die aan het begin zijn ingeschakeld, wordt geen Origineel gemaakt.

6. U kunt nu **v1** bewerken en configureren als een API die verschilt van het **Origineel**. Wijzigingen in één versie hebben geen invloed op een andere.

## <a name="add-the-version-to-a-product"></a>Voeg de versie toe aan een product

Als aanroepers de nieuwe versie willen zien, moet deze worden toegevoegd aan een **product**.

1. Selecteer **Producten** via de klassieke implementatiemodel-pagina.
2. Selecteer **Onbeperkt**.
3. Selecteer **API's**.
4. Selecteer **Toevoegen**.
5. Selecteer **Conferentie-API, versie v1**.
6. Navigeer naar de service-beheerpagina en selecteer **API's**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Blader door de portal voor ontwikkelaars om de versie te zien

1. Selecteer **ontwikkelaarsportal** in het menu bovenaan.
2. Selecteer **API's**. Zoals u ziet toont **Conferentie-API** de versies **Origineel** en **v1**.
3. Selecteer **v1**.
4. U ziet de **Verzoek-URL** van de eerste bewerking in de lijst. Het laat zien dat het API URL-pad **v1** bevat.

    ![Contextmenu van API - versie toevoegen](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe versie toevoegen aan een bestaande API
> * Een versieschema kiezen 
> * Voeg de versie toe aan een product
> * Blader door de portal voor ontwikkelaars om de versie te zien

Ga door naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Upgraden en schaal aanpassen](upgrade-and-scale.md)