---
title: Gegevens van Azure Blockchain Workbench gebruiken in Microsoft Power BI
description: Leer hoe u gegevens uit een SQL-database van Azure Blockchain Workbench laadt en weergeeft in Microsoft Power BI.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/3/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 321a34589277d62290c2fde680bb461de34b4568
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075167"
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Gegevens van Azure Blockchain Workbench weergeven met Microsoft Power BI

Microsoft Power BI biedt de mogelijkheid om met Power BI Desktop eenvoudig krachtige rapporten te genereren van SQL DB-databases en deze vervolgens te publiceren naar [https://www.powerbi.com](http://www.powerbi.com).

Dit artikel bevat stapsgewijze instructies om vanuit Power BI Desktop verbinding te maken met een SQL-database van Azure Blockchain Workbench, een rapport te maken en dit rapport te implementeren op powerbi.com.

## <a name="prerequisites"></a>Vereisten

* Download [PowerBI Desktop](https://aka.ms/pbidesktopstore).

## <a name="connecting-powerbi-to-data-in-azure-blockchain-workbench"></a>Power BI verbinden met gegevens in Azure Blockchain Workbench

1.  Open Power BI Desktop.
2.  Selecteer **Gegevens ophalen**.

    ![Gegevens ophalen](media/blockchain-workbench-data-powerbi/get-data.png)
3.  Selecteer **SQL Server** in de lijst met gegevensbronnen.

4.  Geef de naam van de server en de database op in het dialoogvenster. Geef aan of u de gegevens wilt importeren of een **DirectQuery** wilt uitvoeren. Selecteer **OK**.

    ![SQL-server selecteren](media/blockchain-workbench-data-powerbi/select-sql.png)

5.  Geef de databasereferenties op voor toegang tot Azure Blockchain Workbench. Selecteer **Database** en voer uw referenties in.

    Als u de referenties gebruikt die zijn gemaakt tijdens de implementatie van Azure Blockchain Workbench, is de gebruikersnaam **dbadmin** en het wachtwoord het wachtwoord dat u hebt opgegeven tijdens de implementatie.

    ![Instellingen van SQL-database](media/blockchain-workbench-data-powerbi/db-settings.png)

6.  Als er verbinding is met de database, ziet u in het dialoogvenster **Navigator** de tabellen en weergaven die beschikbaar zijn in de database. De weergaven zijn ontworpen voor rapportagedoeleinden en herkent u aan het voorvoegsel **vw**.

    ![Navigator](media/blockchain-workbench-data-powerbi/navigator.png)

7.  Selecteer de weergaven die u wilt opnemen in het rapport. Om zo veel mogelijk functionaliteit te laten zien, nemen we ook **vwContractAction** op, om details weer te geven van alle acties die hebben plaatsgevonden voor een contract.

    ![Weergaven selecteren](media/blockchain-workbench-data-powerbi/select-views.png)

U kunt nu rapporten maken en publiceren zoals u dat gewend bent met Power BI.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Databaseweergaven in Azure Blockchain Workbench](blockchain-workbench-database-views.md)