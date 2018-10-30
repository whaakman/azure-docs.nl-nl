---
title: Een Power BI Desktop-bestand importeren in Azure Analysis Services | Microsoft Docs
description: Beschrijft hoe u een Power BI Desktop-bestand (pbix) importeren met behulp van Azure portal.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3adf0c9c2e2b264904e66b82716447d634aaeee7
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209646"
---
# <a name="import-a-power-bi-desktop-file"></a>Een Power BI Desktop-bestand importeren

U kunt een gegevensmodel in Power BI Desktop-bestand (pbix) importeren in Azure Analysis Services. Model metagegevens, gegevens in de cache en gegevensbronverbindingen worden geïmporteerd. Rapporten en visualisaties zijn niet geïmporteerd. Geïmporteerde gegevens modellen vanuit Power BI Desktop op het compatibiliteitsniveau 1400 zijn.

> [!IMPORTANT]
> Deze functie is afgeschaft. Het kan worden verwijderd of aanzienlijk gewijzigd in een toekomstige update. Het verdient dat u stoppen met het gebruik van deze functie in nieuwe en bestaande projecten voor compatibiliteit met toekomstige updates. Voor meer geavanceerde model ontwikkelen en testen is het raadzaam het gebruik van Visual Studio (SSDT) en SQL Server Management Studio (SSMS).

**Beperkingen**   


- Als uw gegevensmodel wordt gemaakt in Power BI Desktop juli 2018 Update (2.60.5169.3201) of hoger, zorg ervoor dat er geen preview-functies zijn ingeschakeld. Preview-functies zijn nog niet ondersteund in Azure Analysis Services. Als u het volgende foutbericht krijgt bij het importeren, heeft het pbix-bestand preview-functies ingeschakeld die nog niet worden ondersteund in Azure Analysis Services.

    ![Compatibiliteitswaarschuwing met niveau](./media/analysis-services-import-pbix/aas-import-pbix-cl-warning.png)   
- U moet over de machtigingen van de server-beheerder om te importeren uit een pbix-bestand.
- Het pbix-model verbinding kan maken met **Azure SQL Database** en **Azure SQL Data Warehouse** worden alleen de gegevensbronnen.
- Het pbix-model kan niet live hebben of DirectQuery-verbindingen. 


## <a name="to-import-from-pbix"></a>Om te importeren uit pbix

1. In van uw server **overzicht** > **webdesigner**, klikt u op **Open**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. In **webdesigner** > **modellen**, klikt u op **+ toevoegen**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. In **nieuw model**, typ de modelnaam van een en selecteer vervolgens Power BI Desktop-bestand.

    ![Dialoogvenster voor nieuw model in Azure portal](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. In **importeren**, zoekt en selecteert u het bestand.

     ![Verbinding maken met het dialoogvenster in Azure portal](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="change-credentials"></a>Referenties wijzigen

Wanneer u een gegevensmodel uit een pbix-bestand, standaard importeert, worden de referenties waarmee verbinding wordt gemaakt met een gegevensbron zijn ingesteld op ServiceAccount. Nadat een model is geïmporteerd uit een pbix, kunt u de referenties wijzigen met behulp van de volgende methoden:

- Gebruik de juli 2018 (versie 17.8.1) of een latere versie van SSMS op referenties bewerken. Dit is de eenvoudigste manier.
- Gebruik TMSL [opdracht Alter](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl) op de [gegevensbronnen object](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/datasources-object-tmsl) te wijzigen van de eigenschap van de verbindingsreeks. 
- Het model openen in Visual Studio, de referenties voor de verbinding met gegevensbron bewerken en het model opnieuw implementeren.

Referenties wijzigen met behulp van SSMS. 

1. Vouw in SSMS, database > **verbindingen**. 
2. Met de rechtermuisknop op de verbinding met de database en klik vervolgens op **vernieuwen referenties**. 

    ![Vernieuwen van referenties](./media/analysis-services-import-pbix/aas-import-pbix-creds.png)

3. Selecteer een referentietype in het dialoogvenster referenties en referenties invoeren. Selecteer de Database voor SQL-verificatie. Selecteer de Microsoft-account voor organisatieaccount (OAuth).
    ![Referenties bewerken](./media/analysis-services-import-pbix/aas-import-pbix-edit-creds.png)

De versie juli 2018 van Power BI Desktop bevat een nieuwe functie voor het wijzigen van de machtigingen van de gegevensbron. Op de **Start** tabblad **query's bewerken**  > **instellingen voor gegevensbron**. Selecteer de verbinding van de gegevensbron en klik vervolgens op **machtigingen bewerken**.


## <a name="see-also"></a>Zie ook

[Een model maken in Azure portal](analysis-services-create-model-portal.md)   
[Verbinding maken met Azure analyseservices](analysis-services-connect.md)  
