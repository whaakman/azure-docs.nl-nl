---
title: 'Azure Data Lake Tools: Gebruik Azure Data Lake Tools voor Visual Studio Code | Microsoft Docs'
description: 'Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio Code maken, testen en U-SQL-scripts uitvoeren. '
Keywords: VScode,Azure Data Lake Tools,Local run,Local debug,Local Debug,preview file,upload to storage path,download,upload
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2017
ms.author: jejiang
ms.openlocfilehash: c70cfc309fe60f0641c89b4a341e3364af74771a
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Azure Data Lake Tools voor Visual Studio Code gebruiken

Meer informatie over het Azure Data Lake Tools voor Visual Studio Code (VS Code) voor het maken, testen en U-SQL-scripts uitvoeren. De informatie wordt ook beschreven in de volgende video:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Vereisten

Azure Data Lake Tools voor VSCode biedt ondersteuning voor Windows, Linux en Mac OS.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).

Voor Mac OS- en Linux:
- [.NET core SDK 2.0](https://www.microsoft.com/net/download/core). 
- [Mono 5.2.x](http://www.mono-project.com/download/).

## <a name="install-data-lake-tools"></a>Installeren van Data Lake Tools

Nadat u de vereiste onderdelen hebt geïnstalleerd, kunt u Data Lake Tools voor VS-Code installeren.

**Voor het installeren van Data Lake Tools**

1. Open Visual Studio Code.
2. Klik op **extensies** in het linkerdeelvenster. Voer **Azure Data Lake** in het zoekvak.
3. Klik op **installeren** naast **Azure Data Lake tools**. Na enkele seconden, de **installeren** knop wordt gewijzigd in **opnieuw laden**.
4. Klik op **opnieuw laden** voor het activeren van de **Azure Data Lake tools** extensie.
5. Klik op **opnieuw laden venster** om te bevestigen. U kunt zien **Azure Data Lake tools** in het deelvenster uitbreidingen.

    ![Data Lake Tools voor Visual Studio Code Extensions deelvenster](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

 
## <a name="activate-azure-data-lake-tools"></a>Azure Data Lake Tools activeren
Maak een nieuw .usql-bestand of open een bestaande .usql-bestand voor het activeren van de extensie. 

## <a name="open-the-sample-script"></a>Het voorbeeldscript openen
Open het palet opdracht (Ctrl + Shift + P) en voer de **ADL: Open voorbeeldscript**. Een ander exemplaar van dit voorbeeld wordt geopend. U kunt bewerken, configureren en verzenden van script op basis van dit exemplaar.

## <a name="work-with-u-sql"></a>Werken met U-SQL

U moet een U-SQL-bestand of een map voor het werken met U-SQL openen.

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

5. Sla het bestand als **myUSQL.usql** in de map is geopend. Een configuratiebestand xxx_settings.json wordt ook toegevoegd aan de map.
6. Open en xxx_settings.json configureren met de volgende eigenschappen:

    - Account: Een Data Lake Analytics-account onder uw Azure-abonnement dat nodig is voor het compileren en U-SQL-taken uitvoeren, dus u moet de computeraccount voordat compileren en uitvoeren van U-SQL-taken configureren.
    - Database: Een database onder uw account. De standaardwaarde is **master**.
    - Schema: Een schema op uw database. De standaardwaarde is **dbo**.
    - Optionele instellingen:
        - Prioriteit: De prioriteitsbereik loopt van 1 tot en met 1000 met 1 als de hoogste prioriteit. De standaardwaarde is **1000**.
        - Parallelle uitvoering: De parallelle uitvoering bereik is 1 op 150. De standaardwaarde is de maximale parallelle uitvoering zijn toegestaan in uw Azure Data Lake Analytics-account. 
        
        ![Data Lake Tools voor Visual Studio Code-configuratiebestand](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)
      
        > [!NOTE] 
        > Nadat de configuratie is opgeslagen, wordt de account, de database en de schema-informatie weergegeven op de statusbalk in de linkerbenedenhoek van het bijbehorende .usql-bestand.

**Voor het compileren van een U-SQL-script**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: compileren van Script**. De compilatie-resultaten verschijnen in de **uitvoer** venster. U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: compileren Script** voor het compileren van een U-SQL-taak. Het compilatieresultaat wordt weergegeven in de **uitvoer** deelvenster.
 

**Verzenden van een U-SQL-script**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: verzenden van taak**.  U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: Submit Job**. 

Nadat u een U-SQL-taak hebt ingediend, de verzending van Logboeken worden weergegeven in de **uitvoer** venster in VS-Code. Als de verzending geslaagd is, de taak URL wordt weergegeven ook. U kunt de URL van de taak openen in een webbrowser om bij te houden van de status van de realtime-taak.

Instellen zodat de uitvoer van de taakdetails **jobInformationOutputPath** in de **tegenover code voor de u-sql_settings.json** bestand.
 
**Set Git negeren**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: Set Git negeren**.

    - Als u geen een **.gitIgnore** bestand in uw werkmap een bestand met de naam van VSCode **.gitIgnor** in uw map wordt gemaakt. Vier items (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) worden standaard in het bestand toegevoegd. Verder kunt u updates aanbrengen, indien nodig.
    - Als u al een **.gitIgnore** bestand in uw werkmap VSCode, het hulpprogramma vier items toegevoegd (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) in uw **.gitIgnore** bestand indien de vier items niet in het bestand opgenomen zijn.

  ![Data Lake Tools voor Visual Studio Code-configuratiebestand](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-gitignore.png)

## <a name="use-python-r-and-csharp-code-behind-file"></a>Gebruik van Python, R en CSharp code-behind-bestand
Azure Data Lake-hulpprogramma ondersteunt meerdere aangepaste codes, Zie de instructies [ontwikkelen van U-SQL met Python, R en CSharp voor Azure Data Lake Analytics in VSCode](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="use-assemblies"></a>Assembly's gebruiken

Zie voor meer informatie over het ontwikkelen van assembly's [ontwikkelen van U-SQL-assembly's voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-assemblies.md).

Data Lake Tools kunt u aangepaste code-assembly's registreren in de Data Lake Analytics-catalogus.

**Registreren van een assembly**

U kunt de assembly via registreren de **ADL: registreren Assembly** of **ADL: Assembly registreren via configuratie** opdrachten.

**Registreren via de ADL: registreren Assembly-opdracht**
1.  Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2.  Voer **ADL: Assembly registreren**. 
3.  Geef het lokale assembly-pad. 
4.  Selecteer een Data Lake Analytics-account.
5.  Selecteer een database.

Resultaten: De portal wordt geopend in een browser en geeft het registratieproces assembly.  

Een andere handige manier voor het activeren van de **ADL: registreren Assembly** opdracht is met de rechtermuisknop op het DLL-bestand in Verkenner. 

**Registreren via de ADL: Assembly registreren via de opdracht configuratie**
1.  Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2.  Voer **ADL: Assembly via configuratie registreren**. 
3.  Geef het lokale assembly-pad. 
4.  Het JSON-bestand wordt weergegeven. Bekijken en bewerken van de assembly-afhankelijkheden en de resourceparameters, indien nodig. Instructies worden weergegeven in de **uitvoer** venster. Sla om door te gaan naar de assembly-registratie (Ctrl + S) het JSON-bestand.

![Data Lake Tools voor Visual Studio Code code-behind](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Assembly-afhankelijkheden: een Azure Data Lake Tools of de DLL-bestand eventuele afhankelijkheden heeft. De afhankelijkheden worden weergegeven in het JSON-bestand nadat ze zijn gedetecteerd. 
>- Resources: U kunt uw resources dll-bestand (bijvoorbeeld, txt, PNG en .csv) uploaden als onderdeel van de registratie van de assembly. 

Een andere manier voor het activeren van de **ADL: Assembly registreren via configuratie** opdracht is met de rechtermuisknop op het DLL-bestand in Verkenner. 

De volgende U-SQL-code toont het aanroepen van een assembly. In het voorbeeld de assemblynaam is *testen*.

```
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
```

## <a name="connect-to-azure"></a>Verbinding maken met Azure

Voordat u kunt compileren en U-SQL-scripts in Data Lake Analytics uitvoeren, moet u verbinding met uw Azure-account.

**Verbinding maken met Azure**

1.  Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2.  Voer **ADL: aanmelding**. De aanmeldingsgegevens verschijnt op het bovenste gedeelte.

    ![Data Lake Tools voor Visual Studio Code opdracht palet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![Data Lake Tools voor Visual Studio Code aanmelding apparaatgegevens](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3.  Klik op **Open & kopiëren** voor het openen van de webpagina van de aanmelding met URL: https://aka.ms/devicelogin. Plak de code **G567LX42V** in het tekstvak in en selecteer vervolgens **doorgaan**.

   ![Data Lake Tools voor Visual Studio Code aanmelding plakken code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Volg de instructies voor het aanmelden van de webpagina. Wanneer u verbonden bent, de naam van uw Azure-account wordt weergegeven op de statusbalk in de linkerbenedenhoek van het **tegenover Code** venster. 

    > [!NOTE] 
    >- Hulpprogramma voor de Data Lake ondertekent automatisch in de volgende keer dat als u zich hebt geregistreerd voordat, maar u bent niet afgemeld nog.
    >- Als uw account twee factoren is ingeschakeld heeft, wordt u aangeraden phone verificatie in plaats van met een PINCODE te gebruiken.


Als u wilt afmelden, voer de opdracht **ADL: afmelding**.

## <a name="list-your-data-lake-analytics-accounts"></a>Lijst van uw Data Lake Analytics-accounts

De om verbinding te testen, moet u een lijst van uw Data Lake Analytics-accounts ophalen.

**Voor een lijst met de Data Lake Analytics-accounts onder uw Azure-abonnement**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2. Voer **ADL: lijst van Accounts**. De accounts die worden weergegeven in de **uitvoer** deelvenster.


## <a name="access-the-data-lake-analytics-catalog"></a>Toegang tot de Data Lake Analytics-catalogus

Nadat u verbinding hebt gemaakt in Azure, kunt u de volgende stappen uit voor toegang tot de U-SQL-catalogus.

**Toegang tot de Azure Data Lake Analytics-metagegevens**

1.  Ctrl + Shift + P selecteren en voer vervolgens **ADL: lijst met tabellen**.
2.  Selecteer een van de Data Lake Analytics-accounts.
3.  Selecteer een van de Data Lake Analytics-databases.
4.  Selecteer een van de schema's. Hier ziet u de lijst met tabellen.

## <a name="view-data-lake-analytics-jobs"></a>Weergave Data Lake Analytics-taken

**Data Lake Analytics-taken weergeven**
1.  Open het palet opdracht (Ctrl + Shift + P) en selecteer **ADL: taak weergeven**. 
2.  Selecteer een Data Lake Analytics of lokale account. 
3.  Wacht totdat de takenlijst met voor het account dat moet worden weergegeven.
4.  Selecteer een taak in de lijst om de taak, Data Lake Tools Hiermee opent u de taakdetails van de in de Azure portal en het bestand Taakinfo wordt weergegeven in de VS-Code.

    ![Data Lake Tools voor Visual Studio Code IntelliSense objecttypen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Azure Data Lake Storage-integratie

U kunt Azure Data Lake Storage-gerelateerde opdrachten te gebruiken:
 - Blader door de Azure Data Lake Storage-resources. [Lijst van het opslagpad](#list-the-storage-path). 
 - Voorbeeld bekijken van het Azure Data Lake Storage-bestand. [Voorbeeld van het opslagbestand](#preview-the-storage-file). 
 - Upload het bestand rechtstreeks naar Azure Data Lake Storage in VS-Code. [Upload het bestand of map](#upload-file-or-folder).
 - Download het bestand rechtstreeks uit Azure Data Lake Storage in VS-Code. [Bestand downloaden](#download-file).

## <a name="list-the-storage-path"></a>Het pad voor opslag weergeven 

**Voor een lijst met het pad voor opslag via het palet opdracht**

Met de rechtermuisknop op de scripteditor en selecteer **ADL: lijst pad**.

Kies de map in de lijst of klik op **pad Voer** of **bladeren vanuit hoofdmap** (maakt gebruik van een pad opgeven als een voorbeeld). Selecteer -> uw **ADLA Account**. Navigeer -> of geef het pad van de opslag (bijvoorbeeld: / uitvoer /). -De opdracht palet lijsten > informatie over het pad op basis van uw gegevens.

![Data Lake Tools voor Visual Studio Code weergeven opslag pad resultaten](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Er is een handige methode voor een lijst met het relatieve pad via het snelmenu.

**Aan de lijst door met de rechtermuisknop op het pad voor opslag**

Met de rechtermuisknop op de padtekenreeks selecteren **lijst pad** om door te gaan.

![Data Lake Tools voor Visual Studio Code met de rechtermuisknop op contextmenu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


## <a name="preview-the-storage-file"></a>Voorbeeld van het opslagbestand

Met de rechtermuisknop op de scripteditor en selecteer **ADL: voorbeeld van het bestand**.

Selecteer uw **ADLA Account**. -> Voer een Azure-opslag-bestandspad (bijvoorbeeld /output/SearchLog.txt). -> Resultaat: bestand wordt geopend in VSCode.

   ![Data Lake Tools voor Visual Studio Code voorbeeld bestandsresultaat](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Voorbeeld van het bestand op een andere manier is via het menu met de rechtermuisknop op het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor. 

## <a name="upload-file-or-folder"></a>Upload het bestand of map

1. Met de rechtermuisknop op de scripteditor en selecteer **-bestand uploaden** of **uploaden map**.

2. Kies een bestand of meerdere bestanden als Selecteer uploadt het bestand of kies de hele map als Selecteer uploadt map vervolgens klikt u op **uploaden**. -> Selecteer de opslagmap in de lijst of klik op **pad Voer** of **bladeren vanuit hoofdmap** (maakt gebruik van een pad opgeven als een voorbeeld). Selecteer -> uw **ADLA Account**. Navigeer -> of geef het pad van de opslag (bijvoorbeeld: / uitvoer /). Klik hier -> **huidige map kiezen** om uw upload-updoelmap te specificeren.

   ![Data Lake Tools voor Visual Studio Code Uploadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    


   Er is een andere manier om bestanden zijn geüpload naar de opslag via het menu met de rechtermuisknop op het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor.

Als hetzelfde moment kunt u controleren de [Uploadstatus](#check-storage-tasks-status).


## <a name="download-file"></a>Bestand downloaden 
U kunt bestanden downloaden door te voeren van de opdrachten **ADL: bestand downloaden** of **ADL: bestand downloaden (Geavanceerd)**.

**Voor het downloaden van bestanden via het ADL: bestand downloaden (Geavanceerd)**
1. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **bestand downloaden (Geavanceerd)**.
2. VS-Code wordt een JSON-bestand weergegeven. U kunt bestandspaden invoeren en meerdere bestanden tegelijkertijd te downloaden. Instructies worden weergegeven in de **uitvoer** venster. Sla (Ctrl + S) het JSON-bestand om door te gaan naar het bestand downloaden.

    ![Data Lake Tools voor Visual Studio Code downloaden van bestanden met config](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

3.  Resultaten: De **uitvoer** venster wordt de status van bestand uploaden.

    ![Data Lake Tools voor Visual Studio Code downloaden meerdere bestanden resultaten](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Als hetzelfde moment kunt u controleren de [status downloaden](#check-storage-tasks-status).

**Om bestanden te downloaden via de ADL: bestand downloaden**

1. Met de rechtermuisknop op de scripteditor, selecteert u **bestand downloaden**, en selecteer vervolgens de doelmap uit de **map selecteren** dialoogvenster.

2. Kies de map in de lijst of klik op **pad Voer** of **bladeren vanuit hoofdmap** (maakt gebruik van een pad opgeven als een voorbeeld). Selecteer -> uw **ADLA Account**. Navigeer -> of geef het pad van de opslag (bijvoorbeeld: / uitvoer /) -> Kies een bestand te downloaden.

   ![Data Lake Tools voor Visual Studio Code downloaden status](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

   
   Er is een andere manier om opslagbestanden te downloaden via het menu met de rechtermuisknop op het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor.

Als hetzelfde moment kunt u controleren de [status downloaden](#check-storage-tasks-status).

## <a name="check-storage-tasks-status"></a>Controleer de status van de opslagtaken
De status wordt weergegeven aan de onderkant van de statusbalk wanneer voltooid downloaden en uploaden.
1. Klik op de statusbalk onderstaande en vervolgens het downloaden en Uploadstatus weergeven in **uitvoer** Configuratiescherm.

   ![Data Lake Tools voor Visual Studio Code controleren de opslagstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)

## <a name="vscode-explorer-integration-with-azure-data-lake"></a>Integratie met Azure Data Lake VSCode Explorer

**Integratie van Azure** 

- Voordat u aanmelden bij Azure, kunt u altijd uitbreiden **DATALAKE EXPLORER**, klikt u vervolgens op **aanmelden bij Azur** voor aanmelding bij Azure. Na aanmelding, ziet u alle abonnementen onder uw Azure-account worden vermeld in het linkerpaneel van de **DATALAKE EXPLORER**. 

   ![DataLake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/sign-in-datalake-explorer.png)

   ![DataLake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

**ADLA metagegevens navigatie** 

- Vouw uw Azure-abonnement, kunt u de U-SQL-database, navigeren weergave de **schema's**, **referenties**, **assembly's**, **tabel**, **Index**, enzovoort, onder het knooppunt U-SQL-Databases.

**ADLA metagegevens-Entiteitsbeheer**

- Vouw **U-SQL-Databases**, kunt u een nieuwe database, schema, tabel, tabeltypen, index, statistieken met de rechtermuisknop op de **Script maken** contextmenu onder de bijbehorende knooppunt. Op de pagina is geopend script bewerkt u het script aan uw wensen en verzenden van de taak met de rechtermuisknop op contextmenu **ADL: Submit Job**. Nadat het maken is voltooid, klikt u op contextmenu **vernieuwen** om weer te geven van het nieuwe item gemaakt. U kunt ook het item verwijderen met de rechtermuisknop op het contextmenu **verwijderen**.

   ![DataLake explorer maakt een nieuw itemmenu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

   ![DataLake explorer maakt een nieuw item script](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

**ADLA Assembly-registratie**

 - U kunt **registreren assembly** in de bijbehorende database door met de rechtermuisknop op de **assembly's** knooppunt.

    ![DataLake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

**ADLS-integratie** 

 - Navigeer naar **Opslagaccount**, kunt u **Preview**, **downloaden**, **verwijderen**, **kopie relatief pad**, **Volledig pad kopiëren** door het snelmenu dat verschijnt op het bestandsknooppunt. U kunt **vernieuwen**, **uploaden**, **map uploaden**, **verwijderen** met de rechtermuisknop op het snelmenu dat verschijnt op het mapknooppunt.

   ![DataLake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

   ![DataLake explorer](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-download-preview-file.png)

## <a name="open-adl-storage-explorer-in-portal"></a>Open Opslagverkenner van ADL in portal
1. Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2. Voer **Open Web Azure Storage Explorer** of met de rechtermuisknop op een relatief pad of het volledige pad in de scripteditor en selecteer vervolgens **Open Web Azure Storage Explorer**.
3. Selecteer een Data Lake Analytics-account.

Data Lake Tools Hiermee opent u het pad van de Azure-opslag in de Azure portal. U kunt het pad niet vinden en een voorbeeld bekijken van het bestand via het web.

## <a name="local-run-and-local-debug-for-windows-users"></a>Lokaal uitvoeren en lokaal fouten opsporen voor Windows gebruikers
U-SQL lokaal uitvoeren Test uw lokale gegevens en uw script lokaal wordt gevalideerd voordat uw code naar Data Lake Analytics is gepubliceerd. De functie lokale foutopsporing kunt u de volgende taken uitvoeren voordat u uw code wordt verzonden naar Data Lake Analytics: 
- Uw C# code-behind voor foutopsporing. 
- Analyseer de code in. 
- Het script lokaal valideren.

Zie voor instructies voor lokale uitvoering en lokale foutopsporing, [U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>Aanvullende functies

Data Lake Tools voor VS Code ondersteunt de volgende functies:

-   IntelliSense automatisch aanvullen: suggesties worden weergegeven in het pop-upvensters rond items, zoals trefwoorden, methoden en variabelen. Verschillende pictogrammen verschillende typen objecten:

    - Het gegevenstype scala
    - complex gegevenstype
    - Ingebouwde UDT 's
    - .NET-verzameling en -klassen
    - C#-expressies
    - Ingebouwde C# UDF's, UDO's en UDAAGs 
    - U-SQL-functies
    - U-SQL-windowingfunctie
 
    ![Data Lake Tools voor Visual Studio Code IntelliSense objecttypen](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   IntelliSense automatisch aanvullen van Data Lake Analytics-metagegevens: Data Lake Tools downloadt de metagegevens van de Data Lake Analytics lokaal. De functie IntelliSense wordt automatisch gevuld objecten, inclusief de database, schema, tabel, weergave, tabelwaardefunctie, procedures en C#-assembly's, uit de Data Lake Analytics-metagegevens.
 
    ![Data Lake Tools voor Visual Studio Code IntelliSense metagegevens](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   IntelliSense fout markering: Data Lake Tools onderstreept de bewerken fouten voor de U-SQL en C#. 
-   Syntaxis licht: Data Lake Tools maakt gebruik van verschillende kleuren te onderscheiden van items, zoals variabelen, trefwoorden, gegevenstype en functies. 

    ![Data Lake Tools voor Visual Studio Code syntaxis highlights](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Volgende stappen
- [U-SQL met Python, R en CSharp voor Azure Data Lake Analytics in VSCode ontwikkelen](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Zelfstudie: Aan de slag met Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md)
- [Zelfstudie: U-SQL-scripts ontwikkelen met behulp van Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Ontwikkelen van U-SQL-assembly's voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-assemblies.md)




