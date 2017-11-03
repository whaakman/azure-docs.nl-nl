---
title: Een model in tabelvorm maken met behulp van de ontwerpfunctie Azure Analysis Services-webserver | Microsoft Docs
description: Informatie over het maken van een model in tabelvorm Azure Analysis Services met behulp van de ontwerpfunctie voor webpagina's in Azure-portal.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 0a70ce4a106b8d9103080f050ab2317cd69348c1
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="create-a-model-in-azure-portal"></a>Een model maken in Azure portal

De functie designer (preview) Azure Analysis Services in Azure-portal biedt een snelle en gemakkelijke manier om te maken en bewerken van modellen in tabelvorm en query model direct in uw browser. 

Houd er rekening mee, de ontwerpfunctie web **preview**. Functionaliteit is beperkt, terwijl de nieuwe functionaliteit alle tijd, in preview wordt toegevoegd. Voor meer geavanceerde model ontwikkelen en testen is het aanbevolen gebruik van Visual Studio (SSDT) en SQL Server Management Studio (SSMS).

## <a name="prerequisites"></a>Vereisten

- Een Azure Analysis Services-server op de categorie Standard of ontwikkelaar. Nieuwe modellen die zijn gemaakt met behulp van de ontwerpfunctie Web zijn DirectQuery, wordt alleen ondersteund door deze lagen.
- Een Azure SQL Database, Azure SQL Data Warehouse of Power BI Desktop (pbix)-bestand als een gegevensbron. Nieuwe modellen gemaakt op basis van Power BI Desktop-ondersteuning voor Azure SQL Database, Azure SQL Data Warehouse, Oracle en Teradata-gegevensbronnen.
- Een SQL Server-account en wachtwoord voor het verbinden met Azure SQL Database- of Azure SQL Data Warehouse-gegevensbronnen.

## <a name="to-create-a-new-tabular-model"></a>Een nieuw tabellair model maken

1. In de server **overzicht** blade > **webdesigner**, klikt u op **Open**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. In **webdesigner** > **modellen**, klikt u op **+ toevoegen**.

    ![Een model maken in Azure portal](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. In **nieuw model**, typ de modelnaam van een en selecteer vervolgens een gegevensbron.

    ![Dialoogvenster voor nieuwe model in Azure-portal](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. In **Connect**, geeft u de verbindingseigenschappen. Gebruikersnaam en het wachtwoord moet een SQL Server-serviceaccount.

     ![Verbinding maken met het dialoogvenster in Azure-portal](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. In **tabellen en weergaven**, selecteer de tabellen voor opname in het model en klik vervolgens op **maken**. Relaties worden automatisch gemaakt tussen de tabellen met een sleutelpaar.

     ![Selecteer tabellen en weergaven](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Het nieuwe model wordt weergegeven in uw browser. U kunt hier:   

- De model-gegevens opvragen door velden naar de ontwerpfunctie voor query's te slepen en filters toe te voegen.
- Nieuwe maatregelen in tabellen maken.
- De metagegevens van model bewerken met behulp van de json-editor.
- Open het model in Visual Studio (SSDT), Power BI Desktop of Excel.

![Selecteer tabellen en weergaven](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Wanneer u modelmetagegevens bewerken of maken van nieuwe maatregelen in uw browser, opslaat u deze wijzigingen aan uw model in Azure. Als u op het model in SSDT, Power BI Desktop of Excel werkt, kan het model niet meer synchroon.


## <a name="next-steps"></a>Volgende stappen 
[Databaserollen en gebruikers beheren](analysis-services-database-users.md)  
[Verbinden met Excel](analysis-services-connect-excel.md)  


