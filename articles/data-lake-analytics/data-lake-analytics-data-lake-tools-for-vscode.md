---
title: Azure Data Lake Tools voor Visual Studio Code gebruiken
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio Code maken, testen en U-SQL-scripts uitvoeren.
services: data-lake-analytics
author: Jejiang
ms.author: jejiang
manager: kfile
editor: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: 79cd1a04c99891e5146ad20cfd36b8bd4fe4d893
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261481"
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Azure Data Lake Tools voor Visual Studio Code gebruiken

Informatie over hoe u kunt Azure Data Lake Tools voor Visual Studio Code (VS-Code) gebruiken voor het maken, testen en U-SQL-scripts uitvoeren in dit artikel. De informatie wordt ook beschreven in de volgende video:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Vereisten

Azure Data Lake Tools voor VS Code biedt ondersteuning voor Windows, Linux en Mac OS.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)

Voor Mac OS- en Linux:
- [.NET core SDK 2.0](https://www.microsoft.com/net/download/core)
- [Mono 5.2.x](http://www.mono-project.com/download/)

## <a name="install-azure-data-lake-tools"></a>Installeren van Azure Data Lake Tools

Nadat u de vereiste onderdelen hebt geïnstalleerd, kunt u Azure Data Lake Tools voor VS-Code installeren.

**Azure Data Lake Tools installeren**

1. Open Visual Studio Code.
2. Selecteer **extensies** in het linkerdeelvenster. Voer **Azure Data Lake Tools** in het zoekvak.
3. Selecteer **installeren** naast **Azure Data Lake Tools**. 

   ![De selecties voor het installeren van Data Lake Tools](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

   Na enkele seconden, de **installeren** knop verandert **opnieuw laden**.
4. Selecteer **opnieuw laden** voor het activeren van de **Azure Data Lake Tools** extensie.
5. Selecteer **opnieuw laden venster** om te bevestigen. U kunt zien **Azure Data Lake Tools** in de **extensies** deelvenster.

 
## <a name="activate-azure-data-lake-tools"></a>Azure Data Lake Tools activeren
Maak een bestand .usql of open een bestaande .usql-bestand voor het activeren van de extensie. 


## <a name="work-with-u-sql"></a>Werken met U-SQL

Om te werken met U-SQL, moet u een U-SQL-bestand of een map te openen.

**Het voorbeeldscript openen**

Open het palet opdracht (Ctrl + Shift + P) en voer de **ADL: Open voorbeeldscript**. Een ander exemplaar van dit voorbeeld wordt geopend. U kunt bewerken, configureren en verzenden van een script op dit exemplaar.

**Een map voor uw U-SQL project openen**

1. Selecteer in Visual Studio Code, de **bestand** menu en selecteer vervolgens **map openen**.
2. Geef een map op en selecteer vervolgens **map selecteren**.
3. Selecteer de **bestand** menu en selecteer vervolgens **nieuw**. Een naamloos-1-bestand wordt toegevoegd aan het project.
4. Voer de volgende code in het bestand naamloos 1:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    Het script maakt een bestand departments.csv met enkele gegevens die zijn opgenomen in de map/Output.

5. Sla het bestand als **myUSQL.usql** in de map is geopend.

**Voor het compileren van een U-SQL-script**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: compileren van Script**. De compilatie-resultaten verschijnen in de **uitvoer** venster. U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: compileren Script** voor het compileren van een U-SQL-taak. Het compilatieresultaat wordt weergegeven in de **uitvoer** deelvenster.
 
**Verzenden van een U-SQL-script**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: verzenden van taak**. U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: Submit Job**. 

Nadat u een U-SQL-taak hebt ingediend, de verzending van Logboeken worden weergegeven in de **uitvoer** venster in VS-Code. De taakweergave wordt weergegeven in het rechterdeelvenster. Als de verzending geslaagd is, wordt de URL van de taak te wordt weergegeven. U kunt de URL van de taak openen in een webbrowser om bij te houden van de status van de realtime. 

In de Project-weergave **samenvatting** tabblad ziet u de taakdetails van de. Belangrijkste functies omvatten opnieuw indienen van een script, dupliceren van een script en openen in de portal. In de Project-weergave **gegevens** tabblad kunt u verwijzen naar de invoerbestanden, de uitvoerbestanden en de bronbestanden. Bestanden kunnen worden gedownload naar de lokale computer.

![Tabblad in de Project-weergave Samenvatting](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-summary.png)

![Tabblad gegevens in de Project-weergave](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-data.png)

**De standaardcontext instellen**

U kunt de standaardcontext deze instelling toepassen op alle scriptbestanden als u nog geen parameters zijn ingesteld voor bestanden afzonderlijk instellen.

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: Stel standaardcontext**. Of met de rechtermuisknop op de scripteditor en selecteer **ADL: Set Default Context**.
3. Kies de account, de database en het schema dat u wilt. De instelling wordt opgeslagen in het configuratiebestand xxx_settings.json.

   ![Account, database en schema instellen als de standaardcontext](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-sequence.png)

**Scriptparameters instellen**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: Stel scriptparameters**.
3. Het bestand xxx_settings.json is geopend met de volgende eigenschappen:

   - **account**: een Azure Data Lake Analytics-account onder uw Azure-abonnement dat nodig is om te compileren en U-SQL-taken uitvoeren. U moet de computeraccount configureren voordat u compileren en U-SQL-taken uitvoeren.
   - **database**: een database onder uw account. De standaardwaarde is **master**.
   - **schema**: een schema op uw database. De standaardwaarde is **dbo**.
   - **optionalSettings**:
        - **prioriteit**: het prioriteitsbereik ligt tussen 1 en 1000, waarbij 1 als de hoogste prioriteit. De standaardwaarde is **1000**.
        - **degreeOfParallelism**: de parallelle uitvoering-bereik loopt van 1 tot 150. De standaardwaarde is de maximale parallelle uitvoering zijn toegestaan in uw Azure Data Lake Analytics-account. 

   ![Inhoud van het JSON-bestand](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-setting.png)
      
> [!NOTE] 
> Nadat u de configuratie op te slaan, weergegeven de account, de database en de schema-informatie op de statusbalk in de linkerbenedenhoek van het bijbehorende .usql-bestand als er geen een standaardcontext instellen.

**Instellen van Git negeren**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: Set Git negeren**.

   - Als u hebt een **.gitIgnore** bestand in een bestand met de naam van de VS Code werkende map **.gitIgnore** in uw map wordt gemaakt. Vier items (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) worden standaard in het bestand toegevoegd. U kunt meer updates kunt maken, indien nodig.
   - Als u al een **.gitIgnore** bestand in uw werkmap tegenover Code, het hulpprogramma vier items toegevoegd (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) in uw **.gitIgnore** bestand indien de vier items niet in het bestand opgenomen zijn.

   ![Items in het bestand .gitIgnore](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-gitignore.png)


## <a name="work-with-code-behind-files-c-sharp-python-and-r"></a>Werken met bestanden van de code-behind: C-kruis, Python en R

Azure Data Lake Tools biedt ondersteuning voor meerdere aangepaste codes. Zie voor instructies [ontwikkelen van U-SQL met Python, R en C-kruis voor Azure Data Lake Analytics in VS-Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="work-with-assemblies"></a>Werken met assembly 's

Zie voor meer informatie over het ontwikkelen van assembly's [ontwikkelen van U-SQL-assembly's voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-assemblies.md).

Data Lake Tools kunt u aangepaste code-assembly's registreren in de Data Lake Analytics-catalogus.

**Registreren van een assembly**

U kunt de assembly via registreren de **ADL: registreren Assembly** of **ADL: registreren Assembly (Geavanceerd)** opdracht.

**Registreren via de ADL: registreren Assembly-opdracht**
1.  Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2.  Voer **ADL: Assembly registreren**. 
3.  Geef het lokale assembly-pad. 
4.  Selecteer een Data Lake Analytics-account.
5.  Selecteer een database.

De portal wordt geopend in een browser en geeft het registratieproces assembly.  

Een handige methode voor het activeren van de **ADL: registreren Assembly** opdracht is met de rechtermuisknop op het DLL-bestand in Verkenner. 

**Registreren via de ADL: opdracht registreren Assembly (Geavanceerd)**
1.  Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2.  Voer **ADL: Assembly (Geavanceerd) registreren**. 
3.  Geef het lokale assembly-pad. 
4.  Het JSON-bestand wordt weergegeven. Bekijken en bewerken van de assembly-afhankelijkheden en de resourceparameters, indien nodig. Instructies worden weergegeven in de **uitvoer** venster. Sla om door te gaan naar de assembly-registratie (Ctrl + S) het JSON-bestand.

    ![JSON-bestand met de assembly-afhankelijkheden en de resourceparameters](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
    
>[!NOTE]
>- Azure Data Lake Tools een Hiermee wordt aangegeven of het DLL-bestand heeft assembly-afhankelijkheden. De afhankelijkheden worden weergegeven in het JSON-bestand nadat ze zijn gedetecteerd. 
>- U kunt uw resources dll-bestand (bijvoorbeeld, txt, PNG en .csv) uploaden als onderdeel van de registratie van de assembly. 

Een andere manier voor het activeren van de **ADL: registreren Assembly (Geavanceerd)** opdracht is met de rechtermuisknop op het DLL-bestand in Verkenner. 

De volgende U-SQL-code toont het aanroepen van een assembly. In het voorbeeld de assemblynaam is *testen*.


        REFERENCE ASSEMBLY [test];

        @a = 
            EXTRACT 
                Iid int,
            Starts DateTime,
            Region string,
            Query string,
            DwellTime int,
            Results string,
            ClickedUrls string 
            FROM @"Sample/SearchLog.txt" 
            USING Extractors.Tsv();

        @d =
            SELECT DISTINCT Region 
            FROM @a;

        @d1 = 
            PROCESS @d
            PRODUCE 
                Region string,
            Mkt string
            USING new USQLApplication_codebehind.MyProcessor();

        OUTPUT @d1 
            TO @"Sample/SearchLogtest.txt" 
            USING Outputters.Tsv();


## <a name="use-u-sql-local-run-and-local-debug-for-windows-users"></a>U-SQL lokaal uitvoeren en lokale foutopsporing gebruiken voor Windows-gebruikers
U-SQL lokaal uitvoeren Test uw lokale gegevens en uw script lokaal wordt gevalideerd voordat uw code naar Data Lake Analytics is gepubliceerd. De volgende taken voltooien voordat u uw code wordt verzonden naar Data Lake Analytics kunt u de functie lokale debug: 
- Uw C# code-behind voor foutopsporing. 
- Analyseer de code in. 
- Het script lokaal valideren.

Zie voor instructies voor lokale uitvoering en lokale foutopsporing, [U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).


## <a name="connect-to-azure"></a>Verbinding maken met Azure

Voordat u kunt compileren en U-SQL-scripts in Data Lake Analytics uitvoeren, moet u verbinding met uw Azure-account.

<b id="sign-in-by-command">Verbinding maken met Azure met behulp van een opdracht</b>

1.  Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2.  Voer **ADL: aanmelding**. De aanmeldingsgegevens wordt weergegeven in de rechterbenedenhoek.

    ![De opdracht aanmelding](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)

    ![Meldingen over aanmelden en verificatie](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)

3.  Selecteer **Open & kopiëren** openen de [aanmelding webpagina](https://aka.ms/devicelogin). Plak de code in het vak en selecteer vervolgens **doorgaan**.

    ![Aanmelding webpagina](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png)  
     
4.  Volg de instructies voor het aanmelden van de webpagina. Wanneer u verbonden bent, wordt de naam van uw Azure-account wordt weergegeven op de statusbalk in de linkerbenedenhoek van het venster VS-Code. 

> [!NOTE] 
>- Data Lake Tools u automatisch afgemeld in de volgende keer dat als u niet afmelden.
>- Als uw account twee factoren is ingeschakeld heeft, wordt u aangeraden phone verificatie in plaats van met een PINCODE te gebruiken.


Als u wilt afmelden, voer de opdracht **ADL: afmelding**.

**Verbinding maken met Azure vanuit de explorer**

Vouw **AZURE DATALAKE**, selecteer **aanmelden bij Azure**, en volgt u stap 3 en stap 4 van [verbinding maken met Azure met behulp van een opdracht](#sign-in-by-command).

![Selectie 'Aanmelden bij Azure' in de Verkenner](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-sign-in-from-explorer.png )  

U kunt geen afmelden vanuit de Verkenner. Als u wilt afmelden, Zie [verbinding maken met Azure met behulp van een opdracht](#sign-in-by-command).


## <a name="create-an-extraction-script"></a>Een script extractie maken 
U kunt een script extractie voor CSV, .tsv en txt-bestanden maken met de opdracht **ADL: EXTRAHEREN Script maken** of vanuit de Verkenner Azure Data Lake.

**Een script extractie maken met behulp van een opdracht**

1. Selecteer Ctrl + Shift + P openen van het palet opdracht en voeren **ADL: EXTRAHEREN Script maken**.
2. Geef het volledige pad voor een Azure Storage-bestand en selecteer de Enter-toets.
3. Selecteer een account.
4. Selecteer een scheidingsteken om op te halen van het bestand voor een txt-bestand. 

![Proces voor het maken van een script uitpakken](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-process.png)

De extractie-script is gegenereerd op basis van uw gegevens. Voor een script dat de kolommen kan niet detecteren, moet u een van de twee opties kiezen. Als dat niet het geval is, wordt slechts één script wordt gegenereerd.

![Resultaat van het maken van een script uitpakken](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-result.png)

**Een script extractie maken vanuit de explorer**

Er is een andere manier om de extractie-script te maken via het snelmenu (snelkoppeling) op de CSV, .tsv of txt-bestand in Azure Data Lake Store of Azure Blob-opslag.

![De opdracht 'EXTRACT Script maken' in het snelmenu](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu.png)

## <a name="integrate-with-azure-data-lake-analytics-through-a-command"></a>Integreren met Azure Data Lake Analytics via een opdracht

U kunt toegang tot Azure Data Lake Analytics-bronnen voor lijst van accounts, toegang tot metagegevens en analytics-taken weergeven. 

**Voor een lijst met de Azure Data Lake Analytics-accounts onder uw Azure-abonnement**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2. Voer **ADL: lijst van Accounts**. De accounts die worden weergegeven in de **uitvoer** deelvenster.

**Voor toegang tot Azure Data Lake Analytics-metagegevens**

1.  Ctrl + Shift + P selecteren en voer vervolgens **ADL: lijst met tabellen**.
2.  Selecteer een van de Data Lake Analytics-accounts.
3.  Selecteer een van de Data Lake Analytics-databases.
4.  Selecteer een van de schema's. Hier ziet u de lijst met tabellen.

**Azure Data Lake Analytics-taken weergeven**
1.  Open het palet opdracht (Ctrl + Shift + P) en selecteer **ADL: taken weergeven**. 
2.  Selecteer een Data Lake Analytics of lokale account. 
3.  Wacht totdat de lijst worden weergegeven voor het account.
4.  Selecteer een taak in de taaklijst. Data Lake Tools Hiermee opent u de taakweergave in het rechter deelvenster en bepaalde informatie in de VS Code-uitvoer weergegeven.

    ![Takenlijst](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="integrate-with-azure-data-lake-store-through-a-command"></a>Integreren met Azure Data Lake Store via een opdracht

U kunt Azure Data Lake Store-gerelateerde opdrachten te gebruiken:
 - [Blader door de Azure Data Lake Store-bronnen](#list-the-storage-path) 
 - [Voorbeeld van de Azure Data Lake Store-bestand](#preview-the-storage-file) 
 - [Upload het bestand rechtstreeks naar Azure Data Lake Store in VS-Code](#upload-file-or-folder)
 - [Download het bestand rechtstreeks uit Azure Data Lake Store in VS-Code](#download-file)

### <a name="list-the-storage-path"></a>Het pad voor opslag weergeven 

**Voor een lijst met het pad voor opslag via het palet opdracht**

1. Met de rechtermuisknop op de scripteditor en selecteer **ADL: lijst pad**.
2. Kies de map in de lijst of selecteer **Geef een pad** of **bladeren vanuit hoofdpad**. (We maken gebruik van **Geef een pad** als voorbeeld.) 
3. Selecteer uw Data Lake Analytics-account.
4. Blader naar of Voer het pad van de opslag (bijvoorbeeld/uitvoer /).  

Het palet opdracht bevat informatie over het pad op basis van uw gegevens.

![Opslag pad resultaten](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Er is een handige methode voor een lijst met het relatieve pad via het snelmenu.

**Voor een lijst met het pad voor opslag via het snelmenu**

Met de rechtermuisknop op de padtekenreeks en selecteer **lijst pad**.

!['Lijst pad' in het snelmenu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


### <a name="preview-the-storage-file"></a>Voorbeeld van het opslagbestand

1. Met de rechtermuisknop op de scripteditor en selecteer **ADL: voorbeeld van het bestand**.
2. Selecteer uw Data Lake Analytics-account. 
3. Voer een Azure Storage-bestandspad (bijvoorbeeld /output/SearchLog.txt). 

Het bestand wordt geopend in VS-Code.

![Stappen en resultaat voor het weergeven van het opslagbestand](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Voorbeeld van het bestand op een andere manier is via het snelmenu van het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor. 

### <a name="upload-a-file-or-folder"></a>Uploaden van een bestand of map

1. Met de rechtermuisknop op de scripteditor en selecteer **-bestand uploaden** of **uploaden map**.
2. Kies een bestand of meerdere bestanden als u hebt geselecteerd **-bestand uploaden**, of selecteert u de hele map als u hebt geselecteerd **uploaden map**. Selecteer vervolgens **uploaden**. 
3. Kies de opslagmap in de lijst of selecteer **Geef een pad** of **bladeren vanuit hoofdpad**. (We maken gebruik van **Geef een pad** als voorbeeld.) 
4. Selecteer uw Data Lake Analytics-account. 
5. Blader naar of Voer het pad van de opslag (bijvoorbeeld/uitvoer /). 
6. Selecteer **huidige map kiezen** om uw upload-updoelmap te specificeren.

![Stappen en resultaat voor het uploaden van een bestand of map](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    

Bestanden uploaden naar de opslag op een andere manier is via het snelmenu van het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor.

U kunt [bewaken Uploadstatus](#check-storage-tasks-status).


### <a name="download-a-file"></a>Bestand downloaden 
U kunt een bestand downloaden via de opdracht **ADL: bestand downloaden** of **ADL: bestand downloaden (Geavanceerd)**.

**Voor het downloaden van een bestand via de ADL: opdracht Bestand downloaden (Geavanceerd)**
1. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **bestand downloaden (Geavanceerd)**.
2. VS-Code wordt een JSON-bestand weergegeven. U kunt bestandspaden invoeren en meerdere bestanden tegelijkertijd te downloaden. Instructies worden weergegeven in de **uitvoer** venster. Sla (Ctrl + S) het JSON-bestand om door te gaan naar het bestand of de bestanden downloaden.

    ![JSON-bestand voor het downloaden van bestand met paden](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

De **uitvoer** venster wordt de status van bestand downloaden.

![Het uitvoervenster met de downloadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

U kunt [bewaken van de downloadstatus](#check-storage-tasks-status).

**Voor het downloaden van een bestand via de ADL: opdracht Bestand downloaden**

1. Met de rechtermuisknop op de scripteditor, selecteert u **bestand downloaden**, en selecteer vervolgens de doelmap uit de **map selecteren** in het dialoogvenster.
2. Kies de map in de lijst of selecteer **Geef een pad** of **bladeren vanuit hoofdpad**. (We maken gebruik van **Geef een pad** als voorbeeld.) 
3. Selecteer uw Data Lake Analytics-account. 
4. Geef het pad van de opslag (bijvoorbeeld/uitvoer /) of kies een bestand te downloaden.

![Stappen en resultaat voor het downloaden van een bestand](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

Er is een andere manier om opslagbestanden te downloaden via het snelmenu van het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor.

U kunt [bewaken van de downloadstatus](#check-storage-tasks-status).

### <a name="check-storage-tasks-status"></a>Controleer de status van de opslagtaken
De status uploaden en downloaden weergegeven op de statusbalk. Selecteer de statusbalk en vervolgens de status wordt weergegeven op de **uitvoer** tabblad.

![Statusbalk en uitvoer details](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="integrate-with-azure-data-lake-analytics-from-the-explorer"></a>Integreren met Azure Data Lake Analytics vanuit de explorer

Nadat u zich hebt aangemeld, alle abonnementen voor uw Azure-account worden vermeld in het linkerdeelvenster onder **AZURE DATALAKE**. 

![Data Lake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

### <a name="data-lake-analytics-metadata-navigation"></a>Data Lake Analytics metagegevens navigatie

Vouw uw Azure-abonnement. Onder de **U-SQL-Databases** knooppunt, kunt u bladeren door de U-SQL-database en mappen weergeven, zoals **schema's**, **referenties**, **assembly's**, **Tabellen**, en **Index**.

### <a name="data-lake-analytics-metadata-entity-management"></a>Data Lake Analytics metagegevens-entiteitsbeheer

Vouw **U-SQL-Databases**. U kunt een database, schema, tabel, tabeltype, index of statistieken maken met de rechtermuisknop op het bijbehorende knooppunt en selecteert u **Script maken** in het snelmenu. Op de pagina is geopend script bewerkt u het script aan uw wensen. Vervolgens dient u de taak met de rechtermuisknop op en selecteert u **ADL: Submit Job**. 

Nadat u klaar bent met het maken van het item met de rechtermuisknop op het knooppunt en selecteer vervolgens **vernieuwen** om het item weer te geven. U kunt ook het item verwijderen met de rechtermuisknop op en selecteert u **verwijderen**.

![De opdracht 'Script maken' in het snelmenu in Data Lake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

![Script-pagina voor het nieuwe item](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

### <a name="data-lake-analytics-assembly-registration"></a>Data Lake Analytics-assembly-registratie

U kunt een assembly in de bijbehorende database registreren met de rechtermuisknop op de **assembly's** knooppunt en selecteert u **registreren assembly**.

![De opdracht "Assembly registreren" in het snelmenu voor het knooppunt assembly 's](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

## <a name="integrate-with-azure-data-lake-store-from-the-explorer"></a>Integreren met Azure Data Lake Store vanuit de explorer

Blader naar **Data Lake Store**:

- U kunt met de rechtermuisknop op het mapknooppunt en gebruik vervolgens de **vernieuwen**, **verwijderen**, **uploaden**, **map uploaden**, **kopiëren Relatief pad**, en **volledig pad kopiëren** opdrachten in het snelmenu.

   ![In het snelmenu voor een mapknooppunt in Data Lake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

- U kunt met de rechtermuisknop op het bestandsknooppunt en gebruik vervolgens de **Preview**, **downloaden**, **verwijderen**, **EXTRAHEREN Script maken** (alleen beschikbaar voor CSV TSV, en TXT-bestanden), **relatieve pad kopiëren**, en **volledig pad kopiëren** opdrachten in het snelmenu.

   ![In het snelmenu voor een bestandsknooppunt in de Data Lake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-extract.png)

## <a name="integrate-with-azure-blob-storage-from-the-explorer"></a>Integreren met Azure Blob storage vanuit de explorer

Blader naar de Blob-opslag:

- U kunt met de rechtermuisknop op het knooppunt van blob-container en gebruik vervolgens de **vernieuwen**, **Blob-Container verwijderen**, en **Blob uploaden** opdrachten in het snelmenu.

   ![In het snelmenu voor een blob-containerknooppunt onder Blob-opslag](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-blob-container-node.png)

- U kunt met de rechtermuisknop op het mapknooppunt en gebruik vervolgens de **vernieuwen** en **Blob uploaden** opdrachten in het snelmenu.

   ![In het snelmenu voor een mapknooppunt onder Blob-opslag](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-folder-node.png)

- U kunt met de rechtermuisknop op het bestandsknooppunt en gebruik vervolgens de **Preview/bewerken**, **downloaden**, **verwijderen**, **EXTRAHEREN Script maken** (beschikbaar alleen voor CSV TSV en TXT-bestanden) **relatieve pad kopiëren**, en **volledig pad kopiëren** opdrachten in het snelmenu.

    ![In het snelmenu voor een bestandsknooppunt onder Blob-opslag](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu-2.png)

## <a name="open-the-data-lake-explorer-in-the-portal"></a>De Data Lake explorer openen in de portal
1. Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2. Voer **Open Web Azure Storage Explorer** of met de rechtermuisknop op een relatief pad of het volledige pad in de scripteditor en selecteer vervolgens **Open Web Azure Storage Explorer**.
3. Selecteer een Data Lake Analytics-account.

Data Lake Tools Hiermee opent u het pad voor Azure-opslag in de Azure portal. U kunt het pad niet vinden en een voorbeeld bekijken van het bestand via het web.

## <a name="additional-features"></a>Aanvullende functies

Data Lake Tools voor VS Code ondersteunt de volgende functies:

-   **IntelliSense automatisch aanvullen**: suggesties worden weergegeven in het pop-upvensters rond items zoals trefwoorden, methoden en variabelen. Verschillende pictogrammen verschillende typen objecten:

    - Het gegevenstype scala
    - complex gegevenstype
    - Ingebouwde UDT 's
    - .NET-verzameling en -klassen
    - C#-expressies
    - Ingebouwde C# UDF's, UDO's en UDAAGs 
    - U-SQL-functies
    - U-SQL windowing functies
 
    ![IntelliSense objecttypen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   **IntelliSense automatisch aanvullen van Data Lake Analytics-metagegevens**: Data Lake Tools downloadt de metagegevens van de Data Lake Analytics lokaal. De functie IntelliSense wordt automatisch gevuld voor objecten van de metagegevens van de Data Lake Analytics. Deze objecten bevatten de database, schema, tabel, weergave, tabelwaardefunctie, procedures en C#-assembly's.
 
    ![IntelliSense metagegevens](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   **IntelliSense fout markering**: Data Lake Tools onderstreept bewerken fouten voor de U-SQL en C#. 
-   **Syntaxis licht**: Data Lake Tools kleuren te onderscheiden van items zoals variabelen, trefwoorden, gegevenstypen en functies worden gebruikt. 

    ![Syntaxis met verschillende kleuren](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

> [!NOTE]
> Het is raadzaam dat u een upgrade naar Azure Data Lake Tools voor Visual Studio versie 2.3.3000.4 uitvoert of hoger. De vorige versies zijn niet meer beschikbaar voor download en nu zijn afgeschaft.  
   
## <a name="next-steps"></a>Volgende stappen
- [Ontwikkelen van U-SQL met Python, R en C kruis voor Azure Data Lake Analytics in VS-Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Zelfstudie: Aan de slag met Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md)
- [Zelfstudie: U-SQL-scripts ontwikkelen met behulp van Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
