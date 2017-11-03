---
title: 'Azure Data Lake Tools: Gebruik Azure Data Lake Tools voor Visual Studio Code | Microsoft Docs'
description: 'Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio Code maken, testen en U-SQL-scripts uitvoeren. '
Keywords: VScode, Azure Data Lake Tools, lokaal uitvoeren, lokale foutopsporing, lokale Debug preview opslagbestand uploaden naar opslagpad, downloaden, uploaden
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
ms.date: 10/10/2017
ms.author: jejiang
ms.openlocfilehash: e724a8db4424a1e608ae7ee5625cd4cc16f6078f
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Azure Data Lake Tools voor Visual Studio Code gebruiken

Meer informatie over het Azure Data Lake Tools voor Visual Studio Code (VS Code) voor het maken, testen en U-SQL-scripts uitvoeren. De informatie wordt ook beschreven in de volgende video:

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

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

5. Sla het bestand als **myUSQL.usql** in de map is geopend. Een configuratiebestand adltools_settings.json wordt ook toegevoegd aan het project.
4. Open en adltools_settings.json configureren met de volgende eigenschappen:

    - Account: Een Data Lake Analytics-account onder uw Azure-abonnement.
    - Database: Een database onder uw account. De standaardwaarde is **master**.
    - Schema: Een schema op uw database. De standaardwaarde is **dbo**.
    - Optionele instellingen:
        - Prioriteit: De prioriteitsbereik loopt van 1 tot en met 1000 met 1 als de hoogste prioriteit. De standaardwaarde is **1000**.
        - Parallelle uitvoering: De parallelle uitvoering bereik is 1 op 150. De standaardwaarde is de maximale parallelle uitvoering zijn toegestaan in uw Azure Data Lake Analytics-account. 
        
        > [!NOTE] 
        > Als de instellingen ongeldige zijn, worden de standaardwaarden gebruikt.

        ![Data Lake Tools voor Visual Studio Code-configuratiebestand](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

        Een compute Data Lake Analytics-account is nodig voor het compileren en U-SQL-taken uitvoeren. U moet de computeraccount configureren voordat u deze kunt compileren en U-SQL-taken uitvoeren.
    
        Nadat de configuratie is opgeslagen, is de account, database en schema-informatie wordt weergegeven op de statusbalk in de linkerbenedenhoek van het bijbehorende .usql-bestand. 
 
 
Vergeleken met het openen van een bestand, wanneer u een map die u kunt openen:

- Gebruik een code-behind-bestand. Code-behind wordt niet ondersteund in de modus met één bestand.
- Gebruik een configuratiebestand. Wanneer u een map opent, delen de scripts in de werkmap in een enkel configuratiebestand.


De U-SQL-script wordt gecompileerd op afstand via de Data Lake Analytics-service. Wanneer u de opdracht de **compileren** opdracht, de U-SQL-script wordt verzonden naar uw Data Lake Analytics-account. Visual Studio Code ontvangt later, het compilatieresultaat. Als gevolg van het externe compileren vereist Visual Studio Code weer te geven die de gegevens voor verbinding met uw Data Lake Analytics-account in het configuratiebestand.

**Voor het compileren van een U-SQL-script**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: compileren van Script**. De compilatie-resultaten verschijnen in de **uitvoer** venster. U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: compileren Script** voor het compileren van een U-SQL-taak. Het compilatieresultaat wordt weergegeven in de **uitvoer** deelvenster.
 

**Verzenden van een U-SQL-script**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen. 
2. Voer **ADL: verzenden van taak**.  U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: Submit Job**. 

Nadat u een U-SQL-taak hebt ingediend, de verzending van Logboeken worden weergegeven in de **uitvoer** venster in VS-Code. Als de verzending geslaagd is, de taak URL wordt weergegeven ook. U kunt de URL van de taak openen in een webbrowser om bij te houden van de status van de realtime-taak.

Instellen zodat de uitvoer van de taakdetails **jobInformationOutputPath** in de **tegenover code voor de u-sql_settings.json** bestand.
 
## <a name="use-a-code-behind-file"></a>Een code-behind-bestand gebruiken

Een code-behind-bestand is een C#-bestanden die zijn gekoppeld aan een enkel U-SQL-script. U kunt een speciale script definiëren UDO, UDA, UDT en UDF in het code-behind-bestand. De UDO, UDA, UDT en UDF kunnen rechtstreeks in het script zonder eerst registreren van de assembly worden gebruikt. De code-behind-bestand is in dezelfde map als de peering U-SQL-scriptbestand geplaatst. Als het script de naam xxx.usql, is de code-behind xxx.usql.cs genoemd. Als u de code-behind-bestand voor het handmatig verwijdert, wordt de code-behind-functie is uitgeschakeld voor de bijbehorende U-SQL-script. Zie voor meer informatie over het schrijven van code voor de U-SQL-script klant [schrijven en met behulp van aangepaste Code in de U-SQL: door de gebruiker gedefinieerde functies]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

Ter ondersteuning van code-behind, moet u een werkmap te openen. 

**Een code-behind-bestand genereren**

1. Open een bronbestand. 
2. Selecteer Ctrl + Shift + P om de opdracht palet te openen.
3. Voer **ADL: genereren van Code achter**. Een code-behind-bestand wordt gemaakt in dezelfde map. 

U kunt ook met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: Code genereren achter**. 

Is hetzelfde als met de zelfstandige U-SQL-scriptbestand om te compileren en verzenden van een U-SQL-script met een code-behind-bestand.

De volgende twee schermafbeeldingen weergeven een code-behind-bestand en het bijbehorende bestand van de U-SQL-script:
 
![Data Lake Tools voor Visual Studio Code code-behind](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![Data Lake Tools voor Visual Studio Code code-behind scriptbestand](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

Raadpleeg de instructies lokaal uitvoeren en lokale foutopsporing ondersteund, [U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

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
2.  Voer **ADL: aanmelding**. De aanmeldingsgegevens wordt weergegeven in de **uitvoer** deelvenster.

    ![Data Lake Tools voor Visual Studio Code opdracht palet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![Data Lake Tools voor Visual Studio Code aanmelding apparaatgegevens](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Selecteer Ctrl + klik op de aanmeldings-URL: https://aka.ms/devicelogin de webpagina van de aanmelding te openen. Voer de code **G567LX42V** in het tekstvak in en selecteer vervolgens **doorgaan**.

   ![Data Lake Tools voor Visual Studio Code aanmelding plakken code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Volg de instructies voor het aanmelden van de webpagina. Wanneer u verbonden bent, de naam van uw Azure-account wordt weergegeven op de statusbalk in de linkerbenedenhoek van het **tegenover Code** venster. 

    > [!NOTE] 
    > Als uw account twee factoren is ingeschakeld heeft, wordt u aangeraden phone verificatie in plaats van met een PINCODE te gebruiken.

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
 - Upload het bestand rechtstreeks naar Azure Data Lake Storage in VS-Code. [Bestand uploaden](#upload-file).
 - Download het bestand rechtstreeks uit Azure Data Lake Storage in VS-Code. [Bestand downloaden](#download-file).

## <a name="list-the-storage-path"></a>Het pad voor opslag weergeven 

**Voor een lijst met het pad voor opslag via het palet opdracht**

Met de rechtermuisknop op de scripteditor en selecteer **ADL: lijst opslagpad**.

Kies de map in de lijst of klik op **pad Voer** of **bladeren vanuit hoofdmap** (maakt gebruik van een pad opgeven als een voorbeeld). Selecteer -> uw **ADLA Account**. Navigeer -> of geef het pad van de opslag (bijvoorbeeld: / uitvoer /). -De opdracht palet lijsten > informatie over het pad op basis van uw gegevens.

![Data Lake Tools voor Visual Studio Code weergeven opslag pad resultaten](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Er is een handige methode voor een lijst met het relatieve pad via het snelmenu.

**Aan de lijst door met de rechtermuisknop op het pad voor opslag**

Met de rechtermuisknop op de padtekenreeks selecteren **lijst opslagpad** om door te gaan.

![Data Lake Tools voor Visual Studio Code met de rechtermuisknop op contextmenu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


## <a name="preview-the-storage-file"></a>Voorbeeld van het opslagbestand

Met de rechtermuisknop op de scripteditor en selecteer **ADL: Preview opslagbestand**.

Selecteer uw **ADLA Account**. -> Voer een Azure-opslag-bestandspad (bijvoorbeeld /output/SearchLog.txt). -> Resultaat: bestand wordt geopend in VSCode.

   ![Data Lake Tools voor Visual Studio Code voorbeeld bestandsresultaat](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Er is een andere manier naar preview-opslagbestand via het menu met de rechtermuisknop op het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor. 

## <a name="upload-file"></a>Bestand uploaden 

U kunt bestanden uploaden door te voeren van de opdrachten **ADL: uploaden** of **ADL:-bestand uploaden via configuratie**.

**Voor het uploaden van bestanden via het ADL:-bestand uploaden via configuratie-opdracht**
1.  Met de rechtermuisknop op de scripteditor en selecteer vervolgens **-bestand uploaden via configuratie**.
2.  VS-Code wordt een JSON-bestand weergegeven. U kunt bestandspaden invoeren en meerdere bestanden tegelijk uploaden. Instructies worden weergegeven in de **uitvoer** venster. Sla (Ctrl + S) het JSON-bestand om door te gaan van het bestand te uploaden.

       ![Data Lake Tools voor Visual Studio Code bestandspad](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  Resultaten: De **uitvoer** venster wordt de status van bestand uploaden.

       ![Data Lake Tools voor Visual Studio Code Uploadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

Als hetzelfde moment kunt u controleren de [Uploadstatus](#check-storage-tasks-status).

**Voor het uploaden van bestanden via het ADL: de opdracht-bestand uploaden**

Met de rechtermuisknop op de scripteditor en selecteer **-bestand uploaden**.

Voer uw **lokale bestandspad**. -> Selecteer de map die u in de lijst of klik op **pad Voer** of **bladeren vanuit hoofdmap** (maakt gebruik van een pad opgeven als een voorbeeld). Selecteer -> uw **ADLA Account**. Navigeer -> of geef het pad van de opslag (bijvoorbeeld: / uitvoer /). Klik hier -> **huidige map kiezen** om uw upload-updoelmap te specificeren.

![Data Lake Tools voor Visual Studio Code Uploadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    


Er is een andere manier om bestanden zijn geüpload naar de opslag via het menu met de rechtermuisknop op het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor.

Als hetzelfde moment kunt u controleren de [Uploadstatus](#check-storage-tasks-status).

## <a name="download-file"></a>Bestand downloaden 
U kunt bestanden downloaden door te voeren van de opdrachten **ADL: opslagbestand downloaden** of **ADL: opslag-bestand downloaden via configuratie**.

**Voor het downloaden van bestanden via het ADL: bestand downloaden via de opdracht configuratie**
1. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **opslagbestand downloaden via configuratie**.
2. VS-Code wordt een JSON-bestand weergegeven. U kunt bestandspaden invoeren en meerdere bestanden tegelijkertijd te downloaden. Instructies worden weergegeven in de **uitvoer** venster. Sla (Ctrl + S) het JSON-bestand om door te gaan naar het bestand downloaden.

    ![Data Lake Tools voor Visual Studio Code downloaden van bestanden met config](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

3.  Resultaten: De **uitvoer** venster wordt de status van bestand uploaden.

    ![Data Lake Tools voor Visual Studio Code downloaden meerdere bestanden resultaten](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Als hetzelfde moment kunt u controleren de [status downloaden](#check-storage-tasks-status).

**Om bestanden te downloaden via de ADL: opdracht opslagbestand downloaden**

Met de rechtermuisknop op de scripteditor en selecteer vervolgens **opslagbestand downloaden**.

Kies de map in de lijst of klik op **pad Voer** of **bladeren vanuit hoofdmap** (maakt gebruik van een pad opgeven als een voorbeeld). Selecteer -> uw **ADLA Account**. Navigeer -> of geef het pad van de opslag (bijvoorbeeld: / uitvoer /) -> Kies een bestand te downloaden.

   ![Data Lake Tools voor Visual Studio Code downloaden status](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

   Het bestand wordt opgeslagen in de afbeelding resultaat als de tijdelijke map. U kunt instellen dat een pad voor de parameter downloaden standaard **usql.defaultLocalFolderForDownload** door uzelf, via het menu VSCode **bestand** -> **voorkeuren**  ->  **Instelling**.

Er is een andere manier om opslagbestanden te downloaden via het menu met de rechtermuisknop op het volledige pad van het bestand of het relatieve pad van het bestand in de scripteditor.

Als hetzelfde moment kunt u controleren de [status downloaden](#check-storage-tasks-status).

## <a name="check-storage-tasks-status"></a>Controleer de status van de opslagtaken
De status wordt weergegeven aan de onderkant van de statusbalk wanneer voltooid downloaden en uploaden.
1. Klik op de statusbalk onderstaande en vervolgens het downloaden en Uploadstatus weergeven in **uitvoer** Configuratiescherm.

   ![Data Lake Tools voor Visual Studio Code controleren de opslagstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="open-azure-storage-explorer"></a>Open Azure Opslagverkenner
U kunt openen **Azure Opslagverkenner** met de opdracht **ADL: Open Web Azure Storage Explorer** of door deze te selecteren in het contextmenu van de context.

**Azure Storage Explorer openen**

1. Selecteer Ctrl + Shift + P om de opdracht palet te openen.
2. Voer **Open Web Azure Storage Explorer** of met de rechtermuisknop op een relatief pad of het volledige pad in de scripteditor en selecteer vervolgens **Open Web Azure Storage Explorer**.
3. Selecteer een Data Lake Analytics-account.

Data Lake Tools Hiermee opent u het pad van de Azure-opslag in de Azure portal. U kunt het pad niet vinden en een voorbeeld bekijken van het bestand via het web.

## <a name="local-run-and-local-debug-for-windows-users"></a>Lokaal uitvoeren en lokaal fouten opsporen voor Windows gebruikers
U-SQL lokaal uitvoeren Test uw lokale gegevens en uw script lokaal, valideert voordat uw code naar Data Lake Analytics is gepubliceerd. De functie lokale foutopsporing kunt u de volgende taken uitvoeren voordat u uw code wordt verzonden naar Data Lake Analytics: 
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

- Zie voor U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code [U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).
- Zie voor informatie over Data Lake Analytics aan de slag [zelfstudie: aan de slag met Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Zie voor meer informatie over Data Lake Tools voor Visual Studio [zelfstudie: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Zie voor meer informatie over het ontwikkelen van assembly's [ontwikkelen van U-SQL-assembly's voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-assemblies.md).



