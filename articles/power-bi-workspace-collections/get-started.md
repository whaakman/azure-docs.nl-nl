---
title: Aan de slag met werkruimteverzamelingen van Microsoft Power BI
description: Power BI Workspace Collections is een Azure-service waarmee ontwikkelaars van toepassingen interactieve Power BI-rapporten aan hun eigen toepassingen kunnen toevoegen.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ROBOTS: NOINDEX
ms.assetid: 4787cf44-5d1c-4bc3-b3fd-bf396e5c1176
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/25/2017
ms.author: asaxton
ms.openlocfilehash: 4ee113ed25142507f381d8c9d49b25ee6553c525
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-microsoft-power-bi-workspace-collections"></a>Aan de slag met werkruimteverzamelingen van Microsoft Power BI

**Power BI Workspace Collections** is een Azure-service waarmee ontwikkelaars van toepassingen interactieve Power BI-rapporten aan hun eigen toepassingen kunnen toevoegen. **Power BI-werkruimteverzamelingen** werken met bestaande toepassingen zonder dat deze opnieuw hoeven te worden ontworpen en zonder dat de manier van aanmelden door gebruikers hoeft te worden gewijzigd.

> [!IMPORTANT]
> Power BI Workspace Collections is afgeschaft en is beschikbaar tot juni 2018 of tot de datum die in uw contract wordt aangegeven. Om onderbreking van uw toepassing te voorkomen, wordt u geadviseerd om een migratie naar Power BI Embedded te plannen. Voor meer informatie over het migreren van gegevens naar Power BI Embedded raadpleegt u [How to migrate Power BI Workspace Collections content to Power BI Embedded](https://powerbi.microsoft.com/documentation/powerbi-developer-migrate-from-powerbi-embedded/) (Inhoud van Power BI-werkruimteverzamelingen migreren naar Power BI Embedded).

Resources voor **Microsoft Power BI-werkruimteverzamelingen** worden ingericht via de [Azure Resource Manager-API's](https://msdn.microsoft.com/library/mt712306.aspx). In dit geval is de ingerichte resource een **Power BI-werkruimteverzameling**.

![Algemene stroom van Power BI-werkruimteverzamelingen](media/get-started/introduction.png)

## <a name="create-a-workspace-collection"></a>Een werkruimteverzameling maken

Een **werkruimteverzameling** is een Azure-resource op het hoogste niveau en een container voor de inhoud die wordt ingesloten in uw toepassing. U kunt op twee manieren een **werkruimteverzameling** maken:

* Handmatig met Azure Portal
* Programmatisch met behulp van de Azure Resource Manager-API's

Hier volgen de stappen voor het bouwen van een **werkruimteverzameling** met behulp van Azure Portal.

1. Open **Azure Portal**: [http://portal.azure.com](http://portal.azure.com) en meld u aan.
2. Selecteer **+ Nieuw** linksboven in het venster.
   
   ![+ Nieuw in Azure Portal](media/get-started/create-workspace-1.png)
3. Selecteer **Data + Analytics** en **Power BI-werkruimteverzameling**.
4. Als u al een abonnement hebt op Power BI Workspace Collections, selecteert u onder aan de pagina de optie **Een werkruimteverzameling maken**.

5. Voer op de pagina **Werkruimteverzameling** de vereiste gegevens in.
   
   ![Werkruimteverzameling maken](media/get-started/create-workspace-2.png)
1. Selecteer **Maken**.

Het duurt even voordat de **werkruimteverzameling** is ingericht. U ziet de **werkruimteverzameling** als de bewerking is voltooid.

   ![Werkruimteverzameling in Azure Portal](media/get-started/create-workspace-3.png)

De **werkruimteverzameling** bevat de informatie die u nodig hebt om de API's aan te roepen die werkruimten maken en om hierin inhoud te implementeren.

<a name="view-access-keys"/>

## <a name="view-power-bi-api-access-keys"></a>Toegangssleutels voor Power BI API

Een van de belangrijkste stukjes informatie die nodig zijn om Power BI REST API's aan te roepen, zijn de **toegangssleutels**. Deze worden gebruikt voor het genereren van de **app-tokens** die worden gebruikt om uw API-aanvragen te verifiëren. U geeft uw **toegangssleutels** weer door op de pagina **Instellingen** te klikken op **Toegangssleutels**. Meer informatie over **app-tokens** vindt u in [Authenticating and authorizing with Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren met Power BI Workspace Collections).

   ![Toegangssleutels in instellingen voor werkruimteverzamelingen in Azure Portal](media/get-started/access-keys.png)

U ziet dat u twee sleutels hebt.

   ![Twee sleutels in Toegangssleutels](media/get-started/access-keys-2.png)

Kopieer deze sleutels en sla ze veilig op in uw toepassing. Het is belangrijk dat u deze sleutels net zo behandelt als wachtwoorden, omdat deze toegang bieden tot de inhoud in uw **werkruimteverzameling**.

Hoewel er twee sleutels worden vermeld, hebt u er slechts één tegelijk nodig. De tweede sleutel biedt u de mogelijk periodiek opnieuw sleutels te genereren, zonder dat toegang tot de service wordt onderbroken.

Nu u een exemplaar van Power BI voor uw toepassing en **toegangssleutels** hebt, kunt u een rapport in uw eigen app importeren. Voordat u een rapport leert importeren, kunt u in de volgende sectie lezen hoe u Power BI-gegevenssets en -rapporten maakt om deze in een app in te sluiten.

## <a name="working-with-workspaces"></a>Werken met werkruimten

Nadat u de verzameling met werkruimten hebt gemaakt, moet u een werkruimte maken voor rapporten en gegevenssets. U moet de [Post Workspace REST-API](https://msdn.microsoft.com/library/azure/mt711503.aspx) gebruiken om een werkruimte te maken.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app-using-power-bi-desktop"></a>Power BI-gegevenssets en -rapporten maken om in een app in te sluiten met behulp van Power BI Desktop

Nu u een exemplaar van Power BI voor uw toepassing hebt gemaakt en **toegangssleutels** hebt, moet u de Power BI-gegevenssets en -rapporten maken die u wilt insluiten. Gegevenssets en rapporten kunnen worden gemaakt met behulp van **Power BI Desktop**. U kunt [Power BI Desktop gratis](https://go.microsoft.com/fwlink/?LinkId=521662) downloaden. Of u downloadt de [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547) als u snel aan de slag wilt.

> [!NOTE]
> Meer informatie over het gebruik van **Power BI Desktop** vindt u in [Aan de slag met Power BI Desktop](https://powerbi.microsoft.com/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

Met **Power BI Desktop** maakt u verbinding met uw gegevensbron door een kopie van de gegevens te importeren in **Power BI Desktop** of door een rechtstreekse verbinding te maken met de gegevensbron via **DirectQuery**.

Dit zijn de verschillen tussen **Importeren** en **DirectQuery**.

| Importeren | DirectQuery |
| --- | --- |
| Tabellen, kolommen *en gegevens* worden geïmporteerd of gekopieerd naar **Power BI Desktop**. Als u met visualisaties werkt, vraagt **Power BI Desktop** een kopie van de gegevens op. Als u wilt zien welke wijzigingen zijn doorgevoerd in de onderliggende gegevens, moet u deze vernieuwen of een volledige, actuele gegevensset opnieuw importeren. |Alleen *tabellen en kolommen* worden geïmporteerd of gekopieerd naar **Power BI Desktop**. Wanneer u met visualisaties werkt, voert **Power BI Desktop** query’s uit op de onderliggende gegevensbron. Dit betekent dat u altijd actuele gegevens bekijkt. |

Meer informatie over het maken van een verbinding met een gegevensbron vindt u in [Verbinding maken met een gegevensbron](connect-datasource.md).

Nadat u uw werk in **Power BI Desktop** hebt opgeslagen, wordt een PBIX-bestand gemaakt. Dit bestand bevat het rapport. Daarnaast bevat de PBIX ofwel de volledige gegevensset, als u gegevens importeert, ofwel, als u **DirectQuery** gebruikt, alleen een gegevensset-schema. U kunt met de [Power BI Import API](https://msdn.microsoft.com/library/mt711504.aspx) de PBIX programmatisch implementeren in uw werkruimte.

> [!NOTE]
> **Power BI Workspace Collections** heeft aanvullende API's voor het wijzigen van de server en database waarnaar uw gegevensset wijst en het instellen van accountreferenties die door de gegevensset worden gebruikt voor het maken van een verbinding met uw database. Zie [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) en [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="create-power-bi-datasets-and-reports-using-apis"></a>Power BI-gegevenssets en -rapporten maken met behulp van API's

### <a name="datasets"></a>Gegevenssets

U kunt gegevenssets in Power BI Workspace Collections maken met behulp van de REST-API. Vervolgens kunt u gegevens naar uw gegevensset pushen. Hierdoor kunt u met gegevens werken zonder Power BI Desktop te gebruiken. Zie [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx) (Gegevenssets posten) voor meer informatie.

### <a name="reports"></a>Rapporten

U kunt een rapport uit een gegevensset rechtstreeks in uw toepassing maken met de JavaScript-API. Zie [Create a new report from a dataset in Power BI Workspace Collections](create-report-from-dataset.md) (Een nieuw rapport maken uit een gegevensset in Power BI Workspace Collections) voor meer informatie.

## <a name="see-also"></a>Zie ook

[Aan de slag met het voorbeeld](get-started-sample.md)  
[Authenticating and authorizing in Power BI Workspace Collections](app-token-flow.md) (Verifiëren en autoriseren in Power BI Workspace Collections)  
[Een rapport insluiten](embed-report.md)  
[Create a new report from a dataset in Power BI Workspace Collections Een nieuw rapport maken uit een gegevensset in Power BI Workspace Collections)](create-report-from-dataset.md)
[Save reports in Power BI Workspace Collections (Rapporten opslaan in Power BI Workspace Collections)](save-reports.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Voorbeeld van ingesloten JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  

Nog vragen? [Probeer de Power BI-community](http://community.powerbi.com/)

