---
title: Visual Studio Connected Service for Azure Cosmos DB
description: Biedt ontwikkelaars de mogelijkheid om eenvoudig hun Azure Cosmos DB-account te koppelen en te beheren via Visual Studio Connected Services.
services: cosmos-db
documentationcenter: 
author: jejiang
manager: DJ
+tags: cosmos-db
editor: Jenny Jiang
ms.assetid: 
ms.service: cosmos-db
ms.custom: Azure Cosmos DB
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/19/2017
ms.author: jejiang
ms.openlocfilehash: f63c20831a2700f1024507bef614a5a7cc87d00e
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-visual-studio-connected-service-preview"></a>Azure Cosmos DB: Visual Studio Connected Service (Preview)

Via Visual Studio Connected Services kunnen ontwikkelaars eenvoudig hun Azure Cosmos DB-account koppelen en hun resources beheren.

U kunt ook Data Explorer in Connected Service gebruiken voor het maken van opgeslagen procedures, UDF's en triggers om bedrijfslogica aan de serverzijde uit te voeren. In Data Explorer wordt alle ingebouwde programmatische gegevenstoegang zichtbaar die beschikbaar is in de API's, maar hebt u eenvoudige toegang tot uw gegevens.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 
* Een Azure Cosmos DB-account. Als u dit nog niet hebt, volgt u de stappen in [Een Azure Cosmos DB-account maken](create-sql-api-dotnet.md) om een account te maken op de Azure Portal of raadpleegt u [Een Azure Cosmos DB-account maken in het hulpprogramma Connected Service](#Create-an-Azure-Cosmo-DB-account-in-Connected-Service-tool). 
* Als u een lokale omgeving wilt gebruiken voor ontwikkelingsdoeleinden, kunt u de [Azure Cosmos DB Emulator](local-emulator.md) gebruiken. In deze omgeving wordt de Azure Cosmos DB Service geëmuleerd.
* [Visual Studio](http://www.visualstudio.com/).
* De meest recent Azure Cosmos DB Connected Service Bits. U kunt Azure Cosmos DB Connected Service downloaden op de Visual Studio-Marketplace zoals geïllustreerd in de volgende schermopname. Open **Visual Studio** op uw computer. In het menu **Tools** selecteert u **Extensions and update...**. Kies vervolgens **Online** / **Visual Studio Marketplace**. Voer **cosmosdb** in om de bits te zoeken.

    U kunt ook Azure Cosmos DB Connected Service installeren via [Visual Studio Marketplace](https://go.microsoft.com/fwlink/?linkid=858709).

    ![Schermopname van download van Connected Service: bits.png](./media/connected-service/connected-service-downloadbits.png) 

## <a id="SetupVS"></a>Uw Visual Studio-oplossing instellen
1. Open **Visual Studio** op uw computer.
2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
3. Selecteer in het dialoogvenster **New Project** de optie **Visual C#** / **Console App (.NET Framework)** of **WPF App (.NET Framework)**, geef een naam op voor uw project en klik vervolgens op **OK**.

    ![Schermopname van het venster Nieuw project](./media/connected-service/connected-service-new-project.png)
    
## <a name="add-connected-service-and-add-account"></a>Connected Service en account toevoegen
1. Klik in Solution Explorer met de rechtermuisknop op het projectknooppunt en selecteer **Add** / **Connected Service**. Of klik op het menu **Project** en selecteer vervolgens **Add Connected Service**.

    ![Schermopname van het venster Add Connected Service](./media/connected-service/connected-service-add-connectedservice-rightclick.png)
2. Klik op de Connected Service-pagina op **Connected Services** / **Azure Cosmos DB** om de pagina **Azure Cosmos DB** te openen.

    ![Schermopname van het venster Azure Cosmos DB](./media/connected-service/connected-service-choose-azure-cosmosdb.png)
3. Klik op de pijl-omlaag om u voor de eerste keer aan te melden of een account toe te voegen. Alle Azure Cosmos DB-accounts worden na het aanmelden weergegeven in de lege ruimte. Kies één Azure Cosmos DB-account om toe te voegen aan uw project.

    ![Schermopname van de aanmelding en het venster met het DB-account](./media/connected-service/connected-service-add-db-account.png)
4. Nadat u een Azure Cosmos DB-account hebt toegevoegd, wordt een Connected Service-map voor het Azure Cosmos DB-account toegevoegd aan het project. U kunt meer dan één Azure Cosmos DB-account toevoegen door stap 1 tot en met 3 te herhalen.

    ![Schermopname van het venster met de Connected Service-map](./media/connected-service/connected-service-add-connectedservice-folder.png)

5. Nadat u een Azure Cosmos DB Connected Service hebt toegevoegd, wijzigt u het project voor toegang tot Azure Cosmos DB op een van de volgende manieren:

    * Enkele NuGet-pakketten die voor Azure Cosmos DB-client zijn vereist, worden geïnstalleerd. U kunt ze in uw configuratiebestand voor pakketten zien. 

        ![De nieuwe configuratie voor Azure Cosmos DB-pakketten](./media/connected-service/connected-service-packages-config.png)   
    
    * De URI en sleutel voor Azure Cosmos DB-verbinding worden toegevoegd aan het projectconfiguratiebestand, in dit geval App.config. 

        ![De nieuwe app.config voor Azure Cosmos DB](./media/connected-service/connected-service-app-config.png) 

## <a name="open-azure-cosmos-db-explorer"></a>Open Azure Cosmos DB Explorer openen
1. Klik met de rechtermuisknop op het projectknooppunt en selecteer **Open Cosmos DB Explorer...**.

    ![Schermopname van het venster Open Data Explorer](./media/connected-service/connected-service-right-click-open-data-exporer.png)
2. Klik op de pagina **Choose a Cosmos DB Account** op de vervolgkeuzelijst om één Azure Cosmos DB-account te selecteren.

    ![Schermopname van het venster Added Account Connected Service](./media/connected-service/connected-service-open-explorer.png)
3. Klik op **Open** wanneer het Data Explorer-venster wordt weergegeven.

## <a id="Create-an-Azure-Cosmo-DB-account-in-Connected-Service-tool"></a>Een Azure Cosmos DB-account in het hulpprogramma Connected Service maken
1. Klik op de pagina Connected Service, linksonder in het deelvenster, op **Create a New Cosmos DB Account** om de pagina **Create Cosmos DB Account** te openen.

    ![Schermopname van het geopende venster met het toegangspunt voor Create Azure Cosmos DB Account](./media/connected-service/connected-service-click-new-db-account.png)
2. Geef op de pagina **Create Cosmos DB Account** de gewenste configuratie op voor dit Azure Cosmos DB-account.

    Vul de velden in op de pagina **Create Cosmos DB Account** en gebruik hierbij de gegevens in de volgende schermopname als richtlijn. 
 
    ![De nieuwe Azure Cosmos DB-pagina](./media/connected-service/connected-service-create-new-account.png)        
3. Klik op **Maken** om het account te maken.

## <a name="use-data-explorer"></a>Data Explorer gebruiken

Na het openen van Data Explorer kunt u het volgende doen:
* Databases maken en verwijderen
* Verzamelingen maken en verwijderen
* Documenten maken, verwijderen en filteren
* Opgeslagen procedures maken en verwijderen
* Maak en verwijder triggers en door de gebruiker gedefinieerde functies om bedrijfslogica op de server uit te voeren. 

![De nieuwe Azure Cosmos DB-pagina](./media/connected-service/connected-service-dataexplorerui.png)

## <a name="demo"></a>Demo

Bekijk de volgende video voor instructies over het gebruik van Azure Cosmos DB Connected Service in Visual Studio: [Use Azure Cosmos DB Connected Service in Visual Studio (Azure Cosmos DB Connected Service in Visual Studio gebruiken)](https://go.microsoft.com/fwlink/?linkid=858711)

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u het volgende geleerd:

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account
> * Connected Service en account toevoegen
> * Azure Cosmos DB Explorer openen
> * Data Explorer gebruiken

Nu de Connected Services zijn ingeschakeld voor uw Azure Cosmos DB-account, gaat u verder met een van de zelfstudies om te beginnen met het ontwikkelen van uw oplossing:

* [Ontwikkelen met SQL API in .NET](tutorial-develop-sql-api-dotnet.md).
* [Azure Cosmos DB: zelfstudie aan de slag met SQL API](sql-api-get-started.md).
* Wilt u de schaal en prestaties testen met Azure Cosmos DB? Zie [Performance and Scale Testing with Azure Cosmos DB (Prestaties en schaal testen met Azure Cosmos DB)](performance-testing.md).
* Leer hoe u een [Azure Cosmos DB-account bewaakt](monitor-accounts.md).

