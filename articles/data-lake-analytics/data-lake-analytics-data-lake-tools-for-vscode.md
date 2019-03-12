---
title: Azure Data Lake Tools voor Visual Studio Code gebruiken
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio Code om te maken, testen en uitvoeren van U-SQL-scripts.
services: data-lake-analytics
ms.service: data-lake-analytics
author: Jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: 5042d89f1cb5e928444e4b3c9a23db7bb1d66585
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57531603"
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Azure Data Lake Tools voor Visual Studio Code gebruiken

In dit artikel, leert u hoe u kunt Azure Data Lake Tools voor Visual Studio Code (VS-Code) gebruiken voor het maken, testen en uitvoeren van U-SQL-scripts. De informatie wordt ook beschreven in de volgende video:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Vereisten

Azure Data Lake Tools voor VS Code biedt ondersteuning voor Windows, Linux en macOS. U-SQL lokaal uitvoeren en lokale foutopsporing werkt alleen in Windows.

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)

Voor MacOS en Linux:
- [.NET Core SDK 2.0](https://www.microsoft.com/net/download/core)
- [Mono 5.2.x](https://www.mono-project.com/download/)

## <a name="install-azure-data-lake-tools"></a>Azure Data Lake Tools installeren

Nadat u de vereiste onderdelen hebt geïnstalleerd, kunt u Azure Data Lake Tools voor VS Code installeren.

**Azure Data Lake Tools installeren**

1. Open Visual Studio Code.
2. Selecteer **extensies** in het linkerdeelvenster. Voer **Azure Data Lake Tools** in het zoekvak in.
3. Selecteer **installeren** naast **Azure Data Lake Tools**. 

   ![Selecties voor het installeren van Data Lake-Tools](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

   Na enkele seconden, de **installeren** knop verandert **opnieuw laden**.
4. Selecteer **opnieuw laden** activeren de **Azure Data Lake Tools** extensie.
5. Selecteer **Reload Window** om te bevestigen. U kunt zien **Azure Data Lake Tools** in de **extensies** deelvenster.

 
## <a name="activate-azure-data-lake-tools"></a>Azure Data Lake-Tools activeren
Maak een bestand .usql of open een bestaande .usql-bestand voor het activeren van de extensie. 


## <a name="work-with-u-sql"></a>Werken met U-SQL

Om te werken met U-SQL, moet u een U-SQL-bestand of een map opent.

**Het voorbeeldscript openen**

Open het opdrachtenpalet (Ctrl + Shift + P) en voer de **ADL: Open Sample Script**. Hiermee opent u een ander exemplaar van dit voorbeeld. U kunt ook bewerken, configureren en verzenden van een script op dit exemplaar.

**Een map voor uw U-SQL-project openen**

1. Selecteer in Visual Studio Code, de **bestand** menu en selecteer vervolgens **map openen**.
2. Geef een map, en selecteer vervolgens **map selecteren**.
3. Selecteer de **bestand** menu en selecteer vervolgens **nieuw**. Een naamloos-1-bestand wordt toegevoegd aan het project.
4. Voer de volgende code in het bestand naamloos-1:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    Het script maakt een bestand departments.csv met enkele gegevens die zijn opgenomen in de map/Output.

5. Sla het bestand op als **myUSQL.usql** in de map geopend.

**Voor het compileren van een U-SQL-script**

1. Selecteer Ctrl + Shift + P het opdrachtenpalet te openen. 
2. Voer **ADL: Script compileren**. De compilatie-resultaten worden weergegeven de **uitvoer** venster. U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: Script compileren** voor het compileren van een U-SQL-taak. Het compilatieresultaat wordt weergegeven in de **uitvoer** deelvenster.
 
**Om in te dienen een U-SQL-script**

1. Selecteer Ctrl + Shift + P het opdrachtenpalet te openen. 
2. Voer **ADL: Taak verzenden**. U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: Taak verzenden**. 

Nadat u een U-SQL-taak verzenden, de logboeken verzenden weergegeven in de **uitvoer** venster in VS Code. De taakweergave wordt weergegeven in het rechter deelvenster. Als het verzenden voltooid is, wordt de taak-URL te wordt weergegeven. U kunt de taak-URL openen in een webbrowser om bij te houden van de status van de realtime. 

In de taakweergave **samenvatting** tabblad kunt u de taakdetails weergeven. Belangrijkste functies omvatten opnieuw verzenden van een script, dupliceren van een script en openen in de portal. In de taakweergave **gegevens** tabblad kunt u verwijzen naar de invoerbestanden, uitvoerbestanden en bestanden. Bestanden kunnen worden gedownload naar de lokale computer.

![Tabblad Overzicht in de taakweergave](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-summary.png)

![Tabblad gegevens op in de taakweergave](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-data.png)

**Om in te stellen de standaardcontext**

De standaardcontext deze instellingen toepassen op alle scriptbestanden als u parameters voor de bestanden afzonderlijk niet hebt ingesteld, kunt u instellen.

1. Selecteer Ctrl + Shift + P het opdrachtenpalet te openen. 
2. Voer **ADL: Stel de standaardcontext van de**. Of met de rechtermuisknop op de script-editor en selecteer **ADL: Stel de standaardcontext van de**.
3. Kies de account, een database en een schema dat u wilt. De instelling wordt opgeslagen in het configuratiebestand xxx_settings.json.

   ![Account, database en schema instellen als de standaard-context](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-sequence.png)

**Scriptparameters in te stellen**

1. Selecteer Ctrl + Shift + P het opdrachtenpalet te openen. 
2. Voer **ADL: Scriptparameters ingesteld**.
3. Het bestand xxx_settings.json is geopend met de volgende eigenschappen:

   - **account**: Een Azure Data Lake Analytics-account onder uw Azure-abonnement dat nodig is om te compileren en uitvoeren van U-SQL-taken. U moet het computeraccount configureren voordat u compileren en uitvoeren van U-SQL-taken.
   - **Database**: Een database onder uw account. De standaardwaarde is **master**.
   - **Schema**: Een schema op uw database. De standaardwaarde is **dbo**.
   - **optionalSettings**:
        - **Prioriteit**: De prioriteitsbereik is 1 tot 1000, waarbij 1 als de hoogste prioriteit. De standaardwaarde is **1000**.
        - **degreeOfParallelism**: De parallelle uitvoering bereik is 1 tot 150. De standaardwaarde is het maximum aantal parallelle uitvoering toegestaan in uw Azure Data Lake Analytics-account. 

   ![Inhoud van het JSON-bestand](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-setting.png)
      
> [!NOTE] 
> Nadat u de configuratie opslaan, de account, database, en schema-informatie weergegeven op de statusbalk in de linkerbenedenhoek van het bijbehorende .usql-bestand als u hebt een standaard-context instellen.

**Om in te stellen Git negeren**

1. Selecteer Ctrl + Shift + P het opdrachtenpalet te openen. 
2. Voer **ADL: Set Git negeren**.

   - Als u geen een **.gitIgnore** bestand in de VS Code-werkmap, een bestand met de naam **.gitIgnore** wordt gemaakt in uw map. Vier items (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) worden standaard toegevoegd in het bestand. U kunt meer updates kunt maken, indien nodig.
   - Als u al een **.gitIgnore** bestand in de werkmap van VS Code, het hulpprogramma voegt vier items (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) in uw **.gitIgnore** bestand als de vier items die niet zijn opgenomen in het bestand.

   ![Items in het bestand .gitIgnore](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-gitignore.png)


## <a name="work-with-code-behind-files-c-sharp-python-and-r"></a>Werken met code-behind-bestanden: C Sharp, Python, and R

Azure Data Lake Tools biedt ondersteuning voor meerdere aangepaste code te schrijven. Zie voor instructies [U-SQL ontwikkelen met Python, R en C Sharp voor Azure Data Lake Analytics in VS Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="work-with-assemblies"></a>Werken met assembly 's

Zie voor meer informatie over het ontwikkelen van assembly's [ontwikkelen van U-SQL-assembly's voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-assemblies.md).

Data Lake-Tools kunt u aangepaste code assembly's registreren in de Data Lake Analytics-catalogus.

**Voor het registreren van een assembly**

U kunt de assembly via registreren de **ADL: Registreren Assembly** of **ADL: Assembly (Geavanceerd) registreren** opdracht.

**Registreren via de ADL: Registreren Assembly-opdracht**
1.  Selecteer Ctrl + Shift + P het opdrachtenpalet te openen.
2.  Voer **ADL: Registreren Assembly**. 
3.  Geef het lokale assembly-pad. 
4.  Selecteer een Data Lake Analytics-account.
5.  Selecteer een database.

De portal wordt geopend in een browser en geeft het proces van de registratie van assembly.  

Een handige manier voor het activeren van de **ADL: Registreren Assembly** opdracht is met de rechtermuisknop op het DLL-bestand in Verkenner. 

**Registreren via de ADL: Registreren van de opdracht Assembly (Geavanceerd)**
1.  Selecteer Ctrl + Shift + P het opdrachtenpalet te openen.
2.  Voer **ADL: Registreren van Assembly (Geavanceerd)**. 
3.  Geef het lokale assembly-pad. 
4.  Het JSON-bestand wordt weergegeven. Bekijken en bewerken van de assembly-afhankelijkheden en de resourceparameters, indien nodig. Instructies worden weergegeven in de **uitvoer** venster. Opslaan (Ctrl + S) het JSON-bestand om door te gaan naar de assembly-registratie.

    ![JSON-bestand met de assembly-afhankelijkheden en Resourceparameters](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
    
>[!NOTE]
>- Een voor Azure Data Lake-hulpprogramma's of het DLL-bestand heeft de assembly-afhankelijkheden. De afhankelijkheden worden weergegeven in het JSON-bestand nadat ze zijn gedetecteerd. 
>- U kunt uw resources dll-bestand (bijvoorbeeld, txt, .png en CSV) uploaden als onderdeel van de assembly-registratie. 

Een andere manier voor het activeren van de **ADL: Assembly (Geavanceerd) registreren** opdracht is met de rechtermuisknop op het DLL-bestand in Verkenner. 

De volgende U-SQL-code demonstreert hoe u aan te roepen een assembly. In het voorbeeld de assemblynaam is *testen*.


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


## <a name="use-u-sql-local-run-and-local-debug-for-windows-users"></a>U-SQL lokaal uitvoeren en lokaal fouten opsporen voor Windows-gebruikers gebruiken
U-SQL lokaal uitvoeren Test uw lokale gegevens en uw script lokaal wordt gevalideerd voordat uw code wordt gepubliceerd naar Data Lake Analytics. De volgende taken voltooien voordat u uw code wordt verzonden naar Data Lake Analytics kunt u de functie lokaal fouten opsporen: 
- Fouten opsporen in uw C#-code-behind. 
- De code doorlopen. 
- Uw script lokaal valideren.

De functie lokaal fouten opsporen en lokaal worden uitgevoerd alleen werkt in Windows-omgevingen, en wordt niet ondersteund in macOS en Linux-besturingssystemen.

Zie voor instructies voor het lokaal uitvoeren en lokaal fouten opsporen, [U-SQL lokaal uitvoeren en lokaal fouten opsporen met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).


## <a name="connect-to-azure"></a>Verbinding maken met Azure

Voordat u kunt compileren en uitvoeren van U-SQL-scripts in Data Lake Analytics, moet u verbinding met uw Azure-account.

<b id="sign-in-by-command">Verbinding maken met Azure met behulp van een opdracht</b>

1.  Selecteer Ctrl + Shift + P het opdrachtenpalet te openen. 
2.  Voer **ADL: Aanmelding**. De aanmeldingsgegevens wordt weergegeven in de rechterbenedenhoek.

    ![De opdracht voor aanmelden uit te voeren](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)

    ![Bericht over aanmelding en verificatie](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)

3.  Selecteer **kopiëren & openen** openen de [aanmelding webpagina](https://aka.ms/devicelogin). Plak de code in het vak en selecteer vervolgens **doorgaan**.

    ![Aanmelding webpagina](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png)  
     
4.  Volg de instructies voor het aanmelden van de webpagina. Wanneer u verbonden bent, is de naam van uw Azure-account wordt weergegeven op de statusbalk in de linkerbenedenhoek van het venster VS Code. 

> [!NOTE] 
>- Data Lake-Tools ondertekent automatisch u in de volgende keer dat als u niet afmelden.
>- Als uw account twee factoren ingeschakeld heeft, wordt u aangeraden dat u telefonische verificatie in plaats van gebruik van een PINCODE gebruiken.


Als u wilt afmelden, voer de opdracht **ADL: Logout**.

**Verbinding maken met Azure vanuit de Verkenner**

Vouw **AZURE DATALAKE**, selecteer **aanmelden bij Azure**, en volgt u stap 3 en stap 4 van [verbinding maken met Azure met behulp van een opdracht](#sign-in-by-command).

!['Aanmelden bij Azure' selectie in de Verkenner](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-sign-in-from-explorer.png )  

U kunt zich niet uit vanuit de Verkenner. Als u wilt afmelden, Zie [verbinding maken met Azure met behulp van een opdracht](#sign-in-by-command).


## <a name="create-an-extraction-script"></a>Maken van een script extractie 
U kunt een script extractie voor CSV, .tsv en txt-bestanden maken met behulp van de opdracht **ADL: Pak Script maken** of vanuit de Verkenner van Azure Data Lake.

**Het maken van een script extraheren met behulp van een opdracht**

1. Selecteer Ctrl + Shift + P om het opdrachtenpalet te openen en voer **ADL: EXTRACT-Script maken**.
2. Geef het volledige pad naar een Azure Storage-bestand en selecteert u de Enter-toets.
3. Selecteer één account.
4. Voor een txt-bestand, een scheidingsteken om op te halen van het bestand te selecteren. 

![Proces voor het maken van een script extractie](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-process.png)

De extractie-script is gegenereerd op basis van uw gegevens. De kolommen kiezen voor een script dat niet wordt gedetecteerd in de twee opties. Als dat niet het geval is, wordt slechts één script wordt gegenereerd.

![Resultaat van het maken van een script extractie](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-result.png)

**Het maken van een script extractie van de Verkenner**

Er is een andere manier om de extractie-script te maken via het menu met de rechtermuisknop op (snelkoppeling) op de CSV, .tsv of txt-bestand in Azure Data Lake Store of Azure Blob storage.

![De opdracht 'EXTRACT Script maken' in het snelmenu](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu.png)

## <a name="integrate-with-azure-data-lake-analytics-through-a-command"></a>Integreren met Azure Data Lake Analytics via een opdracht

U kunt toegang tot Azure Data Lake Analytics-resources aan de lijst van accounts, toegang tot metagegevens en analytics-taken weergeven. 

**Om de Azure Data Lake Analytics-accounts onder uw Azure-abonnement weer te geven**

1. Selecteer Ctrl + Shift + P het opdrachtenpalet te openen.
2. Voer **ADL: Lijst van Accounts**. De accounts worden weergegeven in de **uitvoer** deelvenster.

**Voor toegang tot Azure Data Lake Analytics-metagegevens**

1.  Selecteer Ctrl + Shift + P, en voer vervolgens **ADL: Lijst met tabellen**.
2.  Selecteer een van de Data Lake Analytics-accounts.
3.  Selecteer een van de Data Lake Analytics-databases.
4.  Selecteer een van de schema's. Hier ziet u de lijst met tabellen.

**Azure Data Lake Analytics-taken weergeven**
1.  Open het opdrachtenpalet (Ctrl + Shift + P) en selecteer **ADL: Taken weergeven**. 
2.  Selecteer een Data Lake Analytics of lokale account. 
3.  Wacht totdat de lijst wordt weergegeven voor het account.
4.  Selecteer een taak in de taaklijst. Data Lake-Tools wordt geopend de taakweergave in het rechter deelvenster en enkele gegevens weergegeven in de VS Code-uitvoer.

    ![Takenlijst](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="integrate-with-azure-data-lake-store-through-a-command"></a>Integreren met Azure Data Lake Store via een opdracht

U kunt Azure Data Lake Store-gerelateerde opdrachten te gebruiken:
 - [Blader door de Azure Data Lake Store-resources](#list-the-storage-path) 
 - [Voorbeeld van de Azure Data Lake Store-bestand](#preview-the-storage-file) 
 - Upload het bestand rechtstreeks naar Azure Data Lake Store in VS Code
 - Download het bestand rechtstreeks vanuit Azure Data Lake Store in VS Code

### <a name="list-the-storage-path"></a>Het opslagpad lijst 

**Om het opslagpad via het opdrachtenpalet weer te geven**

1. Met de rechtermuisknop op de script-editor en selecteer **ADL: Pad**.
2. Kies de map in de lijst of selecteer **Geef een pad** of **bladeren van het pad naar hoofdmap**. (We maken gebruik van **Geef een pad** als voorbeeld.) 
3. Selecteer uw Data Lake Analytics-account.
4. Blader naar of typ het pad van de opslag-map (bijvoorbeeld/output /).  

Het opdrachtenpalet bevat informatie over het pad op basis van uw gegevens.

![Opslag pad resultaten](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Er is een handige manier om het relatieve pad weer te geven via het snelmenu.

**Om het opslagpad via het snelmenu weer te geven**

Met de rechtermuisknop op de padtekenreeks en selecteer **lijst pad**.

!["Pad lijst' in het snelmenu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


### <a name="preview-the-storage-file"></a>Voorbeeld van de opslagbestand

1. Met de rechtermuisknop op de script-editor en selecteer **ADL: Voorbeeld van bestand**.
2. Selecteer uw Data Lake Analytics-account. 
3. Geef het pad in een Azure Storage-bestand (bijvoorbeeld /output/SearchLog.txt). 

Het bestand wordt geopend in VS Code.

![Stappen en het resultaat van het bestand storage is beschikbaar als Preview](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Een andere manier om een voorbeeld van het bestand is via het snelmenu op het volledige pad van het bestand of het relatieve pad van het bestand in de script-editor. 

### <a name="upload-a-file-or-folder"></a>Een bestand of map uploaden

1. Met de rechtermuisknop op de script-editor en selecteer **-bestand uploaden** of **map uploaden**.
2. Kies een bestand of meerdere bestanden als u hebt geselecteerd **-bestand uploaden**, of selecteert u de hele map als u hebt geselecteerd **map uploaden**. Selecteer vervolgens **uploaden**. 
3. Kies de map storage in de lijst of selecteer **Geef een pad** of **bladeren van het pad naar hoofdmap**. (We maken gebruik van **Geef een pad** als voorbeeld.) 
4. Selecteer uw Data Lake Analytics-account. 
5. Blader naar of typ het pad van de opslag-map (bijvoorbeeld/output /). 
6. Selecteer **huidige map kiezen** om op te geven van de bestemming uploaden.

![Stappen en het resultaat van een bestand of map uploaden](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    

Er is een andere manier om bestanden te uploaden naar opslag via het snelmenu op het volledige pad van het bestand of het relatieve pad van het bestand in de script-editor.

U kunt [bewaken van de uploadstatus](#check-storage-tasks-status).


### <a name="download-a-file"></a>Bestand downloaden 
U kunt een bestand downloaden met behulp van de opdracht **ADL: Bestand downloaden** of **ADL: (Geavanceerd)-bestand downloaden**.

**Een bestand te downloaden via de ADL: Downloaden van bestand (Geavanceerd)**
1. Met de rechtermuisknop op de script-editor, en selecteer vervolgens **bestand downloaden (Geavanceerd)**.
2. VS Code wordt een JSON-bestand weergegeven. U kunt bestandspaden invoeren en meerdere bestanden tegelijkertijd te downloaden. Instructies worden weergegeven in de **uitvoer** venster. Opslaan (Ctrl + S) het JSON-bestand om door te gaan voor het downloaden van het bestand of de bestanden.

    ![JSON-bestand met de paden voor downloaden van bestanden](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

De **uitvoer** venster geeft de status van het bestand downloaden.

![Uitvoervenster met de downloadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

U kunt [bewaken van de downloadstatus](#check-storage-tasks-status).

**Een bestand te downloaden via de ADL: Bestand downloaden**

1. Met de rechtermuisknop op de script-editor, selecteert u **bestand downloaden**, en selecteer vervolgens de map van de **map selecteren** in het dialoogvenster.
2. Kies de map in de lijst of selecteer **Geef een pad** of **bladeren van het pad naar hoofdmap**. (We maken gebruik van **Geef een pad** als voorbeeld.) 
3. Selecteer uw Data Lake Analytics-account. 
4. Blader naar of typ het pad van de opslag-map (bijvoorbeeld/output /) en kies vervolgens een bestand te downloaden.

![Stappen en het resultaat van een bestand wordt gedownload](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

Er is een andere manier om opslagbestanden te downloaden via het snelmenu op het volledige pad van het bestand of het relatieve pad van het bestand in de script-editor.

U kunt [bewaken van de downloadstatus](#check-storage-tasks-status).

### <a name="check-storage-tasks-status"></a>Controleer de status van de storage-taken
De status uploaden en downloaden wordt weergegeven op de statusbalk. Selecteer de statusbalk en vervolgens de status wordt weergegeven op de **uitvoer** tabblad.

![Statusbalk en de details van uitvoer](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="integrate-with-azure-data-lake-analytics-from-the-explorer"></a>Integreren met Azure Data Lake Analytics vanuit de Verkenner

Nadat u zich hebt aangemeld, alle abonnementen voor uw Azure-account worden weergegeven in het linkerdeelvenster onder **AZURE DATALAKE**. 

![Data Lake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

### <a name="data-lake-analytics-metadata-navigation"></a>Data Lake Analytics-metagegevens navigatie

Vouw uw Azure-abonnement. Onder de **U-SQL-Databases** knooppunt, kunt u via uw U-SQL-database en mappen weergeven zoals **schema's**, **referenties**, **assembly's**, **Tabellen**, en **Index**.

### <a name="data-lake-analytics-metadata-entity-management"></a>Beheer van Data Lake Analytics-metagegevens entiteit

Vouw **U-SQL-Databases**. U kunt een database, schema, tabel, tabeltype, index of statistieken maken door met de rechtermuisknop op het bijbehorende knooppunt en vervolgens de optie **Script maken** in het snelmenu. Op de pagina geopend script, bewerk het script op basis van uw behoeften. Vervolgens verzendt u de taak met de rechtermuisknop op en selecteert u **ADL: Taak verzenden**. 

Nadat u klaar bent met het maken van het item met de rechtermuisknop op het knooppunt en selecteer vervolgens **vernieuwen** om het item weer te geven. U kunt ook het artikel verwijderen met de rechtermuisknop op en selecteert u **verwijderen**.

![De opdracht 'Script maken' in het snelmenu in Data Lake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

![Script-pagina voor het nieuwe item](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

### <a name="data-lake-analytics-assembly-registration"></a>Data Lake Analytics assembly-registratie

U kunt een assembly in de bijbehorende database registreren met de rechtermuisknop op de **assembly's** knooppunt en vervolgens de optie **registreren assembly**.

![De opdracht 'Registreren assembly' in het snelmenu voor het knooppunt assembly 's](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

## <a name="integrate-with-azure-data-lake-store-from-the-explorer"></a>Integreren met Azure Data Lake Store vanuit de Verkenner

Blader naar **Data Lake Store**:

- U kunt met de rechtermuisknop op het mapknooppunt en gebruik vervolgens de **vernieuwen**, **verwijderen**, **uploaden**, **map uploaden**, **kopiëren Relatief pad**, en **volledig pad kopiëren** opdrachten in het snelmenu.

   ![In het snelmenu voor een mapknooppunt in de Data Lake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

- U kunt met de rechtermuisknop op het knooppunt van het bestand en vervolgens met de **Preview**, **downloaden**, **verwijderen**, **EXTRAHEREN Script maken** (alleen beschikbaar voor CSV TSV, en TXT-bestanden), **relatieve pad kopiëren**, en **volledig pad kopiëren** opdrachten in het snelmenu.

   ![In het snelmenu voor het knooppunt van een bestand in Data Lake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-extract.png)

## <a name="integrate-with-azure-blob-storage-from-the-explorer"></a>Integreren met Azure Blob storage vanuit de Verkenner

Blader naar de Blob-opslag:

- U kunt met de rechtermuisknop op het knooppunt van blob-container en gebruik vervolgens de **vernieuwen**, **Blobcontainer verwijderen**, en **Blob uploaden** opdrachten in het snelmenu.

   ![In het snelmenu voor een blob-container-knooppunt onder Blob-opslag](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-blob-container-node.png)

- U kunt met de rechtermuisknop op het mapknooppunt en gebruik vervolgens de **vernieuwen** en **Blob uploaden** opdrachten in het snelmenu.

   ![In het snelmenu voor een mapknooppunt onder de Blob-opslag](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-folder-node.png)

- U kunt met de rechtermuisknop op het knooppunt van het bestand en vervolgens met de **Preview/bewerken**, **downloaden**, **verwijderen**, **EXTRAHEREN Script maken** (beschikbaar alleen voor CSV, TSV en TXT-bestanden), **relatieve pad kopiëren**, en **volledig pad kopiëren** opdrachten in het snelmenu.

    ![In het snelmenu voor een bestandsknooppunt onder Blob-opslag](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu-2.png)

## <a name="open-the-data-lake-explorer-in-the-portal"></a>Open de Data Lake explorer in de portal
1. Selecteer Ctrl + Shift + P het opdrachtenpalet te openen.
2. Voer **Open Web Azure Storage Explorer** of met de rechtermuisknop op een relatief pad of het volledige pad in de script-editor, en selecteer vervolgens **Open Web Azure Storage Explorer**.
3. Selecteer een Data Lake Analytics-account.

Data Lake-Tools wordt het pad van de Azure Storage in Azure portal geopend. U kunt het pad niet vinden en voorbeeld van het bestand van het web.

## <a name="additional-features"></a>Aanvullende functies

Data Lake Tools voor VS Code ondersteunt de volgende functies:

-   **IntelliSense automatisch aanvullen**: Suggesties worden weergegeven in het pop-upvensters over items zoals trefwoorden, methoden en variabelen. Verschillende typen objecten de verschillende pictogrammen:

    - Scala-gegevenstype
    - Complex gegevenstype
    - Ingebouwde UDT 's
    - .NET-verzameling en -klassen
    - C#-expressies
    - Ingebouwde C# UDF's, UDO's en UDAAGs 
    - U-SQL-functies
    - U-SQL windowing functies
 
    ![IntelliSense objecttypen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   **IntelliSense automatisch aanvullen van Data Lake Analytics-metagegevens**: Data Lake-Tools downloadt de metagegevens van de Data Lake Analytics lokaal. De functie IntelliSense wordt automatisch gevuld voor objecten van de metagegevens van de Data Lake Analytics. Deze objecten opnemen uit de database, schema, tabel, weergave, table valued function, procedures en C#-assembly's.
 
    ![IntelliSense metagegevens](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   **IntelliSense fout markering**: Data Lake-Tools onderstreept bewerken fouten voor U-SQL en C#. 
-   **Syntaxis van de hoogtepunten**: Data Lake-hulpprogramma's maakt gebruik van kleuren te onderscheiden van items, zoals variabelen, trefwoorden, gegevenstypen en -functies. 

    ![Syntaxis met verschillende kleuren](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

> [!NOTE]
> Het is raadzaam dat u een upgrade naar Azure Data Lake Tools voor Visual Studio versie 2.3.3000.4 uitvoert of hoger. De vorige versies zijn niet meer beschikbaar om te downloaden en zijn nu afgeschaft.  
   
## <a name="next-steps"></a>Volgende stappen
- [Ontwikkelen van U-SQL met Python, R en C Sharp voor Azure Data Lake Analytics in VS Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [U-SQL lokaal uitvoeren en lokaal fouten opsporen met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Zelfstudie: Aan de slag met Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md)
- [Zelfstudie: U-SQL-scripts ontwikkelen met behulp van Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
