---
title: Een tabellair model maken met behulp van de Azure Analysis Services-Web designer | Microsoft Docs
description: Informatie over het maken van een tabellair model van Azure Analysis Services met behulp van de Web designer in Azure portal.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 17ff6ebed615971b4157831431d9e2395ca68b48
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441672"
---
# <a name="create-a-model-in-azure-portal"></a>Een model maken in Azure portal

De functie Azure Analysis Services web designer (preview) in Azure portal biedt een snelle en eenvoudige manier om te maken en bewerken van modellen in tabelvorm en query uitvoeren op model gegevens rechtstreeks in uw browser. 

Houd er rekening mee, is de webdesigner **preview**. Functionaliteit is beperkt. Voor meer geavanceerde model ontwikkelen en testen is het raadzaam het gebruik van Visual Studio (SSDT) en SQL Server Management Studio (SSMS).

## <a name="before-you-begin"></a>Voordat u begint

- Een Azure Analysis Services-server op de laag Standard- of -ontwikkelaar. Nieuwe modellen die zijn gemaakt met behulp van de Web designer zijn DirectQuery, alleen ondersteund door deze lagen.
- Een Azure SQL Database, Azure SQL Data Warehouse of Power BI Desktop (.pbix)-bestand als een gegevensbron. Nieuwe modellen die zijn gemaakt op basis van Power BI Desktop-ondersteuning voor Azure SQL Database en Azure SQL Data Warehouse.
- Een SQL Server-account en wachtwoord om verbinding te maken met Azure SQL Database of Azure SQL Data Warehouse-gegevensbronnen.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="to-create-a-new-tabular-model"></a>Een nieuw tabellair model maken

1. In de server **overzicht** > **webdesigner**, klikt u op **Open**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. In **webdesigner** > **modellen**, klikt u op **+ toevoegen**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. In **nieuw model**, typ de modelnaam van een en selecteer vervolgens een gegevensbron.

    ![Dialoogvenster voor nieuw model in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. In **Connect**, voer de verbindingseigenschappen. Gebruikersnaam en het wachtwoord moet een SQL Server-serviceaccount.

     ![Verbinding maken met het dialoogvenster in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. In **tabellen en weergaven**, selecteer de tabellen in een model opneemt, en klik vervolgens op **maken**. Relaties worden automatisch gemaakt tussen de tabellen met een sleutelpaar.

     ![Tabellen en weergaven selecteren](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Het nieuwe model wordt weergegeven in uw browser. Hier kunt u het volgende doen:   

- Modelgegevens op te vragen door velden naar de ontwerpfunctie voor query's te slepen en filters toe te voegen.
- Maak nieuwe metingen in tabellen.
- De metagegevens van model bewerken met behulp van de json-editor.
- Open het model in Visual Studio (SSDT), Power BI Desktop of Excel.

![Tabellen en weergaven selecteren](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Wanneer u de metagegevens van model bewerken of maken van nieuwe metingen in uw browser, kunt u deze wijzigingen wilt opslaan aan uw model in Azure. Als u ook aan uw model in SSDT, Power BI Desktop of Excel werkt, is uw model krijgt niet gesynchroniseerd.


## <a name="next-steps"></a>Volgende stappen 
[Databaserollen en gebruikers beheren](analysis-services-database-users.md)  
[Verbinden met Excel](analysis-services-connect-excel.md)  


