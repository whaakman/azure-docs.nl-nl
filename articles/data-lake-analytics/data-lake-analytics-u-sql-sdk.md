---
title: Uitvoeren en testen van U-SQL-taken lokaal via de Azure Data Lake U-SQL-SDK
description: Informatie over het uitvoeren en testen van U-SQL-taken lokaal via de opdrachtregel en programmeerinterfaces op uw lokale werkstation.
services: data-lake-analytics
ms.service: data-lake-analytics
author: yanacai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 03/01/2017
ms.openlocfilehash: 6a73ef058a76152678099eca3f1bd15590b0b03d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51238791"
---
# <a name="run-and-test-u-sql-with-azure-data-lake-u-sql-sdk"></a>Uitvoeren en testen van U-SQL met Azure Data Lake U-SQL-SDK

Bij het ontwikkelen van U-SQL-script, is het gebruikelijk om uit te voeren en U-SQL-testscript lokaal verzenden voordat deze naar de cloud. Azure Data Lake biedt een Nuget-pakket Azure Data Lake U-SQL-SDK met de naam voor dit scenario, via die u kunt eenvoudig de schaal van U-SQL uitvoeren en testen. Het is ook mogelijk om te integreren met deze test U-SQL met CI (Continuous Integration)-systeem voor het automatiseren van het samenstellen en testen.

Als u het belangrijkst hoe handmatig lokaal uitvoeren en fouten opsporen in U-SQL-script met GUI-hulpprogramma's, kunt u Azure Data Lake Tools voor Visual Studio gebruiken voor die. U kunt meer informatie uit [hier](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="install-azure-data-lake-u-sql-sdk"></a>Installeer Azure Data Lake U-SQL-SDK

U kunt de Azure Data Lake U-SQL-SDK ophalen [hier](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) op Nuget.org. En voordat u deze gebruikt, moet u om ervoor te zorgen dat u als volgt afhankelijkheden hebben.

### <a name="dependencies"></a>Afhankelijkheden

De Data Lake U-SQL-SDK vereist de volgende afhankelijkheden:

- [Microsoft .NET Framework 4.6 of hoger](https://www.microsoft.com/download/details.aspx?id=17851).
- Microsoft Visual C++-14 en Windows SDK 10.0.10240.0 of hoger (die wordt CppSDK in dit artikel genoemd). Er zijn twee manieren om op te halen CppSDK:

    - Installeer [Community Edition van Visual Studio](https://developer.microsoft.com/downloads/vs-thankyou). Hebt u een map \Windows Kits\10 onder de map Program Files, bijvoorbeeld: C:\Program Files (x86) \Windows Kits\10\. Ook vindt u de Windows 10 SDK-versie onder \Windows Kits\10\Lib. Als u deze mappen niet ziet, opnieuw installeren van Visual Studio en zorg ervoor dat u de Windows 10 SDK selecteren tijdens de installatie. Als u dit met Visual Studio is geïnstalleerd, wordt de lokale U-SQL-compiler deze automatisch vinden.

    ![Data Lake Tools voor Visual Studio lokaal uitgevoerde Windows 10 SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - Installeer [Data Lake Tools voor Visual Studio](https://aka.ms/adltoolsvs). U vindt de voorverpakte Visual C++ en de Windows SDK-bestanden in C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK. De afhankelijkheden niet in dit geval automatisch door de U-SQL lokaal compiler vinden. U moet het pad CppSDK opgeven voor deze. U kunt de bestanden kopiëren naar een andere locatie of gebruik dit zo is.

## <a name="understand-basic-concepts"></a>Informatie over basisconcepten

### <a name="data-root"></a>Gegevenshoofdmap

De hoofdmap van de gegevens is een 'lokale opslag' voor de lokale compute-account. Dit is gelijk aan het Azure Data Lake Store-account van een Data Lake Analytics-account. Overschakelen naar een andere hoofdmap van de gegevens is net als het overschakelen naar een andere store-account. Als u toegang tot veel gedeelde gegevens met verschillende gegevens-hoofdmappen wilt, moet u de absolute paden gebruiken in uw scripts. Of bestand system symbolische koppelingen maken (bijvoorbeeld **mklink** op NTFS) onder de map van de hoofdmap van de gegevens om te verwijzen naar de gedeelde gegevens.

De hoofdmap van de gegevens wordt gebruikt om:

- Store lokale metagegevens, zoals databases, tabellen, tabelfuncties (tvf's) en assembly's.
- Zoek de invoer- en paden die zijn gedefinieerd als relatieve paden in U-SQL. Met behulp van relatieve paden gemakkelijker uw U-SQL-projecten te implementeren in Azure.

### <a name="file-path-in-u-sql"></a>Het pad naar in U-SQL

U kunt zowel een relatief pad en een lokale absolute pad in U-SQL-scripts gebruiken. Het relatieve pad is ten opzichte van het pad voor de opgegeven gegevens-hoofdmap. Het is raadzaam dat u '/' als het scheidingsteken in pad uw scripts om compatibel te maken met de serverzijde. Hier volgen enkele voorbeelden van relatieve paden en hun gelijkwaardige absolute paden. In deze voorbeelden is C:\LocalRunDataRoot de hoofdmap van de gegevens.

|Relatief pad|Het absolute pad|
|-------------|-------------|
|/abc/def/input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|abc/def/input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/abc/def/input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Werkmap

Wanneer het U-SQL-script lokaal uitvoert, wordt een werkmap gemaakt tijdens de compilatie onder de huidige actieve-map. Naast de compilatie-uitvoer worden de benodigde runtime-bestanden voor de uitvoering van de lokale schaduwkopieën naar deze werkmap. De hoofdmap van de werkende directory heet 'ScopeWorkDir' en de bestanden in de werkmap zijn als volgt:

|Active Directory/bestand|Active Directory/bestand|Active Directory/bestand|Definitie|Beschrijving|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Hash-tekenreeks van de runtime-versie|Schaduwkopie van runtimebestanden die nodig zijn voor lokale uitvoering|
| |Script_66AE4909AA0ED06C| |Scriptnaam + hash-tekenreeks van pad naar script|Uitvoer van de samenstelling en uitvoering van stap logboekregistratie|
| | |\_script\_.abr|Uitvoer van compiler|Wiskundige bestand|
| | |\_ScopeCodeGen\_.*|Uitvoer van compiler|Beheerde code gegenereerd|
| | |\_ScopeCodeGenEngine\_.*|Uitvoer van compiler|Gegenereerde systeemeigen code|
| | |waarnaar wordt verwezen, assembly 's|Assembly-verwijzing|Bestanden van de assembly waarnaar wordt verwezen|
| | |deployed_resources|Resource-implementatie|Bronbestanden voor de implementatie|
| | |xxxxxxxx.xxx[1..n]\_\*.*|Uitvoeringslogboek|Logboek van de stappen worden uitgevoerd|


## <a name="use-the-sdk-from-the-command-line"></a>Gebruik de SDK vanaf de opdrachtregel

### <a name="command-line-interface-of-the-helper-application"></a>De opdrachtregelinterface van de helper-toepassing

LocalRunHelper.exe is onder SDK directory\build\runtime, de toepassing vanaf de opdrachtregel die interfaces voor het merendeel van de meest gebruikte lokaal uitgevoerde functies biedt. Houd er rekening mee dat zowel de opdracht en de schakelopties hoofdlettergevoelig zijn. Om aan te roepen deze:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

LocalRunHelper.exe worden uitgevoerd zonder argumenten of met de **help** switch om de help-informatie weer te geven:

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile the script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

In de help-informatie:

-  **Opdracht** geeft de naam van de opdracht.  
-  **Een vereist Argument** een lijst met argumenten die moeten worden opgegeven.  
-  **Optioneel Argument** een lijst met argumenten die optioneel, met standaardwaarden zijn.  Optionele Booleaanse argumenten geen parameters hebben en hun uiterlijk betekenen negatieve op hun standaardwaarde.

### <a name="return-value-and-logging"></a>Geretourneerde waarde en logboekregistratie

Retourneert de helper-toepassing **0** voor succes en **-1** is mislukt. Standaard stuurt het hulpprogramma voor alle berichten naar de huidige console. De meeste van de opdrachten ondersteunen echter de **- MessageOut pad_naar_logboekbestand** optioneel argument waarmee wordt de uitvoer wordt omgeleid naar een logboekbestand.

### <a name="environment-variable-configuring"></a>Omgeving variabele configureren

U-SQL lokaal moet een hoofdmap opgegeven run as-account met lokale opslag, evenals een opgegeven pad CppSDK voor afhankelijkheden. U kunt beide instellen het argument in de opdrachtregel of stel omgevingsvariabele voor hen.

- Stel de **SCOPE_CPP_SDK** omgevingsvariabele.

    Als u Microsoft Visual C++ en de Windows-SDK ophalen door het installeren van Data Lake Tools voor Visual Studio, controleert u of dat u de volgende map hebt:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Een nieuwe omgevingsvariabele met de naam definiëren **SCOPE_CPP_SDK** om te verwijzen naar deze map. Kopieer de map naar een andere locatie of geef **SCOPE_CPP_SDK** als.

    Naast het instellen van de omgevingsvariabele, kunt u de **- CppSDK** argument als u vanaf de opdrachtregel. Dit argument wordt uw standaard CppSDK-omgevingsvariabele overschreven.

- Stel de **LOCALRUN_DATAROOT** omgevingsvariabele.

    Een nieuwe omgevingsvariabele met de naam definiëren **LOCALRUN_DATAROOT** die verwijst naar de gegevenshoofdmap.

    Naast het instellen van de omgevingsvariabele, kunt u de **- DataRoot** argument met het pad van de hoofdmap van de gegevens wanneer u een opdrachtregel. Dit argument wordt de omgevingsvariabele van de standaard-hoofdmap van de gegevens overschreven. U moet dit argument toevoegen aan elke vanaf de opdrachtregel uitvoert zodat u kunt de omgevingsvariabele van de standaard-hoofdmap van de gegevens voor alle bewerkingen overschrijven.

### <a name="sdk-command-line-usage-samples"></a>SDK vanaf de opdrachtregel gebruik voorbeelden

#### <a name="compile-and-run"></a>Compileren en uitvoeren

De **uitvoeren** opdracht wordt gebruikt voor het compileren van het script en voer vervolgens de resultaten. De opdrachtregelargumenten zijn een combinatie van die van **compileren** en **uitvoeren**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

Hieronder staan de optionele argumenten voor **uitvoeren**:


|Argument|Standaardwaarde|Beschrijving|
|--------|-------------|-----------|
|-CodeBehind|False|Het script heeft .cs code achter|
|-CppSDK| |CppSDK Directory|
|-DataRoot| De omgevingsvariabele DataRoot|DataRoot voor lokaal uitvoeren, standaard ingesteld op 'LOCALRUN_DATAROOT'-omgevingsvariabele|
|-MessageOut| |Berichten in de console naar een bestand dumpen|
|-Parallel|1|Het plan uitvoeren met de opgegeven parallelle uitvoering|
|-Verwijzingen| |Lijst met paden naar extra verwijzingsassembly's of gegevensbestanden van de code achter, gescheiden door ';'|
|-UdoRedirect|False|Udo assembly omleidings-config genereren|
|-UseDatabase|model|Database moet worden gebruikt voor de code achter tijdelijke assembly-registratie|
|-Verbose|False|Gedetailleerde uitvoer van de runtime weergeven|
|-WorkDir|Huidige map|Map voor de compiler gebruiks- en uitvoer|
|-RunScopeCEP|0|ScopeCEP modus waarnaar moet worden gebruikt|
|-ScopeCEPTempPath|TEMP|TEMP-pad moet worden gebruikt voor het streamen van gegevens|
|-OptFlags| |Door komma's gescheiden lijst van vlaggen optimaliseren|


Hier volgt een voorbeeld:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Naast het combineren van **compileren** en **uitvoeren**, kunt u compileren en uitvoeren van de compilatie uitvoerbare bestanden afzonderlijk.

#### <a name="compile-a-u-sql-script"></a>Compileren van een U-SQL-script

De **compileren** opdracht wordt gebruikt voor het compileren van een U-SQL-script voor het uitvoerbare bestanden.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

Hieronder staan de optionele argumenten voor **compileren**:


|Argument|Beschrijving|
|--------|-----------|
| -CodeBehind [standaardwaarde 'False']|Het script heeft .cs code achter|
| -CppSDK [standaardwaarde '']|CppSDK Directory|
| -DataRoot [standaardwaarde 'Omgevingsvariabele DataRoot']|DataRoot voor lokaal uitvoeren, standaard ingesteld op 'LOCALRUN_DATAROOT'-omgevingsvariabele|
| -MessageOut [standaardwaarde '']|Berichten in de console naar een bestand dumpen|
| -Verwijzingen [standaardwaarde '']|Lijst met paden naar extra verwijzingsassembly's of gegevensbestanden van de code achter, gescheiden door ';'|
| -Recente [standaardwaarde 'False']|Recente compileren|
| -UdoRedirect [standaardwaarde 'False']|Udo assembly omleidings-config genereren|
| -UseDatabase [standaardwaarde 'master']|Database moet worden gebruikt voor de code achter tijdelijke assembly-registratie|
| -WorkDir [standaardwaarde 'Huidige map']|Map voor de compiler gebruiks- en uitvoer|
| -RunScopeCEP [standaardwaarde '0']|ScopeCEP modus waarnaar moet worden gebruikt|
| -ScopeCEPTempPath [standaardwaarde 'temp']|TEMP-pad moet worden gebruikt voor het streamen van gegevens|
| -OptFlags [standaardwaarde '']|Door komma's gescheiden lijst van vlaggen optimaliseren|


Hier volgen enkele voorbeelden van het gebruik.

Compileren van een U-SQL-script:

    LocalRunHelper compile -Script d:\test\test1.usql

Compileren van een U-SQL-script en stel de hoofdmap van de gegevens. Houd er rekening mee dat hiermee de set-omgevingsvariabele wordt overschreven.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

Compileren van een U-SQL-script en een werkmap, verwijzing assembly en -database instellen:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Gecompileerde resultaten uitvoeren

De **uitvoeren** opdracht wordt gebruikt voor het uitvoeren van de resultaten.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

Hieronder staan de optionele argumenten voor **uitvoeren**:

|Argument|Standaardwaarde|Beschrijving|
|--------|-------------|-----------|
|-DataRoot | '' |De gegevenshoofdmap voor de uitvoering van de metagegevens. Wordt standaard de **LOCALRUN_DATAROOT** omgevingsvariabele.|
|-MessageOut | '' |Berichten in de console naar een bestand dump.|
|-Parallel | '1' |Geeft de gegenereerde lokaal uitgevoerde stappen ook uitvoeren met het niveau van de opgegeven parallelle uitvoering.|
|-Verbose | 'False' |Indicator om gedetailleerde uitvoer van de runtime weer te geven.|

Hier volgt een voorbeeld van gebruik:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a>Gebruik de SDK met programmeerinterfaces

De API's bevinden zich in de LocalRunHelper.exe. U kunt ze gebruiken voor het integreren van de functionaliteit van de U-SQL-SDK en de C# testframework voor het schalen van uw U-SQL-script lokaal testen. In dit artikel wordt ik de standaard C#-eenheid test-project gebruiken om weer te geven over het gebruik van deze interfaces voor het testen van uw U-SQL-script.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>Stap 1: C#-eenheid test-project en de configuratie maken

- Maak een C#-eenheidsproject testen via Bestand > Nieuw > Project > Visual C# > testen > Project eenheid testen.
- LocalRunHelper.exe toevoegen als een verwijzing voor het project. De LocalRunHelper.exe bevindt zich in \build\runtime\LocalRunHelper.exe in Nuget-pakket.

    ![Azure Data Lake U-SQL-SDK verwijzing toevoegen](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- U-SQL-SDK **alleen** x64 omgeving, zorg ervoor dat u het doel van build-platform als x64 instellen. U die kunt instellen via projecteigenschap > bouwen > Platform doel.

    ![Azure Data Lake U-SQL-SDK configureren x64 Project](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Zorg ervoor dat het instellen van uw testomgeving als x64. In Visual Studio, kunt u dit instellen via Test > Testinstellingen > processorarchitectuur standaard > x64.

    ![Azure Data Lake U-SQL-SDK configureren x64 testomgeving](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- Zorg ervoor dat u alle afhankelijkheden in NugetPackage\build\runtime\ bestanden kopiëren naar project directory deze bevindt zich meestal onder ProjectFolder\bin\x64\Debug werkt.

### <a name="step-2-create-u-sql-script-test-case"></a>Stap 2: U-SQL-script-Testscenario maken

Hieronder vindt u de voorbeeldcode voor testen van U-SQL-script. Voor het testen moet u scripts, invoerbestanden en de verwachte uitvoerbestanden voorbereiden.

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify the local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure the DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget to close MessageOutput to get logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a>Programmeerinterfaces in LocalRunHelper.exe

LocalRunHelper.exe biedt de API's voor de U-SQL lokaal compileren, worden uitgevoerd, enzovoort. De interfaces worden als volgt weergegeven.

**Constructor**

openbare LocalRunHelper ([System.IO.TextWriter messageOutput = null])

|Parameter|Type|Beschrijving|
|---------|----|-----------|
|messageOutput|System.IO.TextWriter|voor Uitvoerberichten, die is ingesteld op null-Console te gebruiken|

**Eigenschappen**

|Eigenschap|Type|Beschrijving|
|--------|----|-----------|
|AlgebraPath|tekenreeks|Het pad naar bestand wiskundige (wiskundige-bestand is een van de compilatieresultaten)|
|CodeBehindReferences|tekenreeks|Als het script aanvullende code achter verwijzingen heeft, geeft u de paden die zijn gescheiden door ';'|
|CppSdkDir|tekenreeks|CppSDK directory|
|CurrentDir|tekenreeks|Huidige map|
|DataRoot|tekenreeks|Pad naar de hoofdmap van gegevens|
|DebuggerMailPath|tekenreeks|Het pad naar het foutopsporingsprogramma mailslot|
|GenerateUdoRedirect|BOOL|Als we willen dat voor het genereren van het laden van assembly omleiding configuratie overschrijven|
|HasCodeBehind|BOOL|Als het script heeft de code achter|
|InputDir|tekenreeks|Map voor de invoergegevens|
|MessagePath|tekenreeks|Bericht dump bestandspad|
|OutputDir|tekenreeks|Map voor uitvoergegevens|
|Parallelle uitvoering|int|Parallelle uitvoering om uit te voeren van de wiskundige|
|ParentPid|int|PID van het bovenliggende object waarop de service bewaakt om af te sluiten, ingesteld op 0 of een negatieve te negeren|
|ResultPath|tekenreeks|Resultaat dump bestandspad|
|RuntimeDir|tekenreeks|Runtime-map|
|scriptPath|tekenreeks|Waar vind ik het script|
|Recente|BOOL|Recente compileren of niet|
|TempDir|tekenreeks|Tijdelijke map|
|UseDataBase|tekenreeks|Geef de database moet worden gebruikt voor de code achter tijdelijke assembly-registratie, hoofd standaard|
|WorkDir|tekenreeks|Gewenste werkmap|


**Methode**

|Methode|Beschrijving|Terug|Parameter|
|------|-----------|------|---------|
|openbare bool DoCompile()|Compileer de U-SQL-script|Waar bij succes| |
|openbare bool DoExec()|Het gecompileerde resultaat uitvoeren|Waar bij succes| |
|openbare bool DoRun()|Voer het U-SQL-script (compileren en uitvoeren)|Waar bij succes| |
|openbare bool IsValidRuntimeDir (tekenreeks-pad)|Controleer of het opgegeven pad geldig runtime pad|De waarde True voor geldige|Het pad van de runtime-map|


## <a name="faq-about-common-issue"></a>Veelgestelde vragen over veel voorkomende problemen

### <a name="error-1"></a>Fout: 1:
E_CSC_SYSTEM_INTERNAL: Er is een interne fout opgetreden. Kan bestand of assembly 'ScopeEngineManaged.dll' of een van de bijbehorende afhankelijkheden niet laden. De opgegeven module kan niet worden gevonden.

Controleer het volgende:

- Zorg ervoor dat u hebt x64 omgeving. Het doelplatform build en de testomgeving moet worden x64, verwijzen naar **stap 1: maken van C#-eenheid-project en de configuratie testen** hierboven.
- Zorg ervoor dat u alle afhankelijkheidsbestanden onder NugetPackage\build\runtime\ hebt gekopieerd naar de werkmap project.


## <a name="next-steps"></a>Volgende stappen

* Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
* Als u wilt logboekregistratie van diagnostische informatie, Zie [diagnostische logboeken openen voor Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).
* Zie voor een complexere query [websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Taakdetails Zie [gebruik Job Browser en Job View voor Azure Data Lake Analytics-taken](data-lake-analytics-data-lake-tools-view-jobs.md).
* Als u wilt de vertex execution view gebruiken, Zie [de Vertex Execution View gebruiken in Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
