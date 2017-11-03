---
title: Schaal U-SQL lokaal uitvoeren en testen met Azure Data Lake U-SQL-SDK | Microsoft Docs
description: Ontdek hoe u met Azure Data Lake U-SQL-SDK scale U-SQL-taken lokaal uitvoeren en testen met de opdrachtregel en API's op uw lokale werkstation.
services: data-lake-analytics
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: yanacai
ms.openlocfilehash: 55242bcf644ca0e7f30cfe7eada2130451c36e64
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a>Schaal U-SQL lokaal uitvoeren en testen met Azure Data Lake U-SQL-SDK

Bij het ontwikkelen van U-SQL-script, is het gebruikelijk om uit te voeren en U-SQL-testscript lokaal verzenden voordat het in de cloud. Azure Data Lake biedt een Nuget-pakket Azure Data Lake U-SQL-SDK voor dit scenario aangeroepen, via die u kunt gemakkelijk de schaal van U-SQL lokaal uitvoeren en testen. Het is ook mogelijk om te integreren met deze test U-SQL met CI (continue integratie) systeem compilatie automatiseren en testen.

Als u het belangrijkst hoe handmatig lokale uitvoeren en foutopsporing van U-SQL-script met GUI tooling, kunt u Azure Data Lake Tools voor Visual Studio gebruiken voor die. U kunt meer informatie van [hier](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="install-azure-data-lake-u-sql-sdk"></a>Installeren Azure Data Lake U-SQL-SDK

U kunt de Azure Data Lake U-SQL-SDK krijgen [hier](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) op Nuget.org. En voordat u deze gebruikt, moet u om ervoor te zorgen dat u als volgt afhankelijkheden hebben.

### <a name="dependencies"></a>Afhankelijkheden

De Data Lake U-SQL-SDK vereist de volgende afhankelijkheden:

- [Microsoft .NET Framework 4.6 of nieuwere](https://www.microsoft.com/download/details.aspx?id=17851).
- Microsoft Visual C++-14 en Windows-SDK 10.0.10240.0 of hoger (die wordt CppSDK in dit artikel genoemd). Er zijn twee manieren om op te halen CppSDK:

    - Installeer [Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou). U hebt een \Windows Kits\10 map onder de map Program Files--bijvoorbeeld C:\Program Files (x86) \Windows Kits\10\. Ook vindt u de versie van Windows 10 SDK onder \Windows Kits\10\Lib. Als u deze mappen niet ziet, installeer Visual Studio en moet u de Windows 10-SDK selecteren tijdens de installatie. Als u dit met Visual Studio is geïnstalleerd hebt, wordt de U-SQL lokaal compiler deze automatisch gevonden.

    ![Data Lake Tools voor Visual Studio local-klaar Windows 10-SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - Installeer [Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs). U kunt vinden op C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK voorverpakte Visual C++- en Windows-SDK-bestanden. De afhankelijkheden niet in dit geval automatisch door de lokale U-SQL-compiler vinden. U moet het pad CppSDK opgeven voor deze. Kopieer de bestanden naar een andere locatie of gebruikt u deze zo is.

## <a name="understand-basic-concepts"></a>Basisconcepten begrijpen

### <a name="data-root"></a>Gegevenshoofdmap

De gegevens-hoofdmap is 'lokale store' voor de lokale compute-account. Dit is gelijk aan het Azure Data Lake Store-account van een Data Lake Analytics-account. Overschakelen naar een andere hoofdmap van de gegevens is net als overschakelen naar een andere store-account. Als u toegang wilt tot vaak gedeelde gegevens met verschillende gegevenshoofdmap mappen, moet u de absolute paden gebruiken in uw scripts. Of het bestand system symbolische koppelingen maken (bijvoorbeeld **mklink** van NTFS) onder de hoofdmap voor gegevens om te verwijzen naar de gedeelde gegevens.

De hoofdmap voor de gegevens wordt gebruikt voor:

- Lokale metagegevens, zoals databases, tabellen, tabelfuncties (tvf's) en assembly's opgeslagen.
- Zoek de invoer- en exportpaden die zijn gedefinieerd als relatieve paden in de U-SQL. U vergemakkelijkt het implementeren van uw U-SQL-projecten in Azure met relatieve paden.

### <a name="file-path-in-u-sql"></a>Bestandspad in U-SQL

U kunt een relatief pad en een lokaal absoluut pad in U-SQL-scripts gebruiken. Het relatieve pad is ten opzichte van het pad voor de opgegeven gegevens-hoofdmap. Het is raadzaam dat u '/' als het padscheidingsteken uw scripts om compatibel te maken met de serverzijde. Hier volgen enkele voorbeelden van relatieve paden en hun equivalente absolute paden. In deze voorbeelden is C:\LocalRunDataRoot het gegevens-hoofdmap.

|Relatief pad|Het absolute pad|
|-------------|-------------|
|/ABC/DEF/Input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/Input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/Input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Werkmap

Wanneer het U-SQL-script lokaal wordt uitgevoerd, wordt een werkmap tijdens de compilatie onder de huidige actieve directory gemaakt. Naast de compilatie-uitvoer worden de benodigde runtime-bestanden voor lokale uitvoering shadow gekopieerd naar deze werkmap. De hoofdmap van de werkende directory heet 'ScopeWorkDir' en de bestanden in de werkmap zijn als volgt:

|Map/bestand|Map/bestand|Map/bestand|Definitie|Beschrijving|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Hash-tekenreeks van de runtime-versie|Schaduwkopie van de runtimebestanden die nodig zijn voor lokale uitvoering|
| |Script_66AE4909AA0ED06C| |De naam van een script + hash-tekenreeks van scriptpad|Compilatie-uitvoer en uitvoering van stap logboekregistratie|
| | |\_script\_.abr|Compiler-uitvoer|Wiskundige bestand|
| | |\_ScopeCodeGen\_. *|Compiler-uitvoer|Beheerde code gegenereerd|
| | |\_ScopeCodeGenEngine\_. *|Compiler-uitvoer|Gegenereerde systeemeigen code|
| | |assemblies waarnaar wordt verwezen|Assembly-verwijzing|Bestanden van de assembly waarnaar wordt verwezen|
| | |deployed_resources|Resources implementeren|Bronbestanden van de implementatie|
| | |XXXXXXXX.xxx[1..n]\_\*. *|Uitvoeringslogboek|Logboek van de stappen worden uitgevoerd|


## <a name="use-the-sdk-from-the-command-line"></a>Gebruik de SDK vanaf de opdrachtregel

### <a name="command-line-interface-of-the-helper-application"></a>Opdrachtregelinterface van de helper-toepassing

SDK-directory\build\runtime is LocalRunHelper.exe de toepassing vanaf de opdrachtregel die voorziet in interfaces voor de meeste functies van de meest gebruikte lokaal uitvoeren. Houd er rekening mee dat de opdracht en de schakelopties hoofdlettergevoelig zijn. Om aan te roepen het:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

LocalRunHelper.exe worden uitgevoerd zonder argumenten of met de **help** overschakelen naar de help-informatie weergeven:

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

-  **Opdracht** krijgt de naam van de opdracht.  
-  **Een vereist Argument** een lijst met argumenten die moeten worden opgegeven.  
-  **Optioneel Argument** een lijst met argumenten die optioneel, met standaardwaarden zijn.  Optionele Booleaanse argumenten geen parameters hebben en hun uiterlijk betekenen negatieve op hun standaardwaarde.

### <a name="return-value-and-logging"></a>Retourwaarde en logboekregistratie

Retourneert de helper-toepassing **0** voor succes en **-1** van de fout. Standaard wordt met de helper alle berichten verzendt naar de huidige console. De meeste van de opdrachten ondersteunen echter het **- MessageOut pad_naar_logboekbestand** optioneel argument waarmee de uitvoer worden omgeleid naar een logboekbestand.

### <a name="environment-variable-configuring"></a>Omgeving variabele configureren

U-SQL lokaal moet een hoofdmap opgegeven gegevens run as-account met lokale opslag, evenals een opgegeven pad CppSDK voor afhankelijkheden. U kunt beide instellen het argument in opdrachtregel- of stel omgevingsvariabele voor hen.

- Stel de **SCOPE_CPP_SDK** omgevingsvariabele.

    Als u Microsoft Visual C++ en de Windows SDK door het installeren van Data Lake Tools voor Visual Studio, Controleer of u de volgende map:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Definieer een nieuwe omgevingsvariabele aangeroepen **SCOPE_CPP_SDK** om te verwijzen naar deze map. Of de map te kopiëren naar een andere locatie en geef **SCOPE_CPP_SDK** als die.

    Naast het instellen van de omgevingsvariabele, kunt u de **- CppSDK** argument wanneer u de opdrachtregel. Dit argument overschrijft uw standaard CppSDK-omgevingsvariabele.

- Stel de **LOCALRUN_DATAROOT** omgevingsvariabele.

    Definieer een nieuwe omgevingsvariabele aangeroepen **LOCALRUN_DATAROOT** die verwijst naar de gegevenshoofdmap.

    Naast het instellen van de omgevingsvariabele, kunt u de **- DataRoot** argument met het pad van hoofdmap van de gegevens wanneer u een opdrachtregel. Dit argument wordt overschreven door de omgevingsvariabele van uw standaard-hoofdmap van de gegevens. U moet dit argument toevoegen aan elke vanaf de opdrachtregel uitvoert zodat u kunt de omgevingsvariabele voor standaard hoofdmap van de gegevens voor alle bewerkingen overschrijven.

### <a name="sdk-command-line-usage-samples"></a>SDK opdrachtregel usage-voorbeelden

#### <a name="compile-and-run"></a>Compileren en uitvoeren

De **uitvoeren** opdracht wordt gebruikt voor het compileren van het script en vervolgens de resultaten wordt uitgevoerd. De opdrachtregelargumenten zijn een combinatie van die van **compileren** en **uitvoeren**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

Hieronder vindt u optionele argumenten voor **uitvoeren**:


|Argument|Standaardwaarde|Beschrijving|
|--------|-------------|-----------|
|-CodeBehind|False|Het script heeft .cs code achter|
|-CppSDK| |CppSDK Directory|
|-DataRoot| De omgevingsvariabele DataRoot|DataRoot voor lokaal uitvoeren, standaard ingesteld op 'LOCALRUN_DATAROOT'-omgevingsvariabele|
|-MessageOut| |Dump berichten op de console naar een bestand|
|-Parallel|1|Het plan uitvoeren met de opgegeven parallelle uitvoering|
|-Verwijzingen| |Lijst met paden extra verwijzings-assembly's en de gegevensbestanden van de code achter, gescheiden door ';'|
|-UdoRedirect|False|Udo assembly omleiding config genereren|
|-UseDatabase|model|Database moet worden gebruikt voor code achter tijdelijke assembly-registratie|
|-Verbose|False|Gedetailleerde uitvoer van de runtime weergeven|
|-WorkDir|Huidige map|Map voor de compiler gebruiks- en uitvoer|
|-RunScopeCEP|0|ScopeCEP modus gebruiken|
|-ScopeCEPTempPath|TEMP|TEMP-pad moet worden gebruikt voor het streamen van gegevens|
|-OptFlags| |Door komma's gescheiden lijst met optimalisatie van vlaggen|


Hier volgt een voorbeeld:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Naast combineren **compileren** en **uitvoeren**, u kunt compileren en de gecompileerde uitvoerbare bestanden afzonderlijk uitvoeren.

#### <a name="compile-a-u-sql-script"></a>Compileren van een U-SQL-script

De **compileren** opdracht wordt gebruikt voor het compileren van een U-SQL-script voor uitvoerbare bestanden.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

Hieronder vindt u optionele argumenten voor **compileren**:


|Argument|Beschrijving|
|--------|-----------|
| -CodeBehind [standaardwaarde 'False']|Het script heeft .cs code achter|
| -CppSDK [standaardwaarde '']|CppSDK Directory|
| -DataRoot [standaardwaarde 'Omgevingsvariabele DataRoot']|DataRoot voor lokaal uitvoeren, standaard ingesteld op 'LOCALRUN_DATAROOT'-omgevingsvariabele|
| -MessageOut [standaardwaarde '']|Dump berichten op de console naar een bestand|
| -Verwijst naar [standaardwaarde '']|Lijst met paden extra verwijzings-assembly's en de gegevensbestanden van de code achter, gescheiden door ';'|
| -Recente [standaardwaarde 'False']|Recente compileren|
| -UdoRedirect [standaardwaarde 'False']|Udo assembly omleiding config genereren|
| -UseDatabase [standaardwaarde 'master']|Database moet worden gebruikt voor code achter tijdelijke assembly-registratie|
| -WorkDir [standaardwaarde 'Huidige map']|Map voor de compiler gebruiks- en uitvoer|
| -RunScopeCEP [standaardwaarde '0']|ScopeCEP modus gebruiken|
| -ScopeCEPTempPath [standaardwaarde 'temp']|TEMP-pad moet worden gebruikt voor het streamen van gegevens|
| -OptFlags [standaardwaarde '']|Door komma's gescheiden lijst met optimalisatie van vlaggen|


Hier volgen enkele voorbeelden van het gebruik.

Compileren van een U-SQL-script:

    LocalRunHelper compile -Script d:\test\test1.usql

Compileren van een U-SQL-script en stelt u de gegevens-hoofdmap. Houd er rekening mee dat hiermee de omgevingsvariabele set wordt overschreven.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

Compileren van een U-SQL-script en stel een werkmap, de referentie-assembly en de database:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Gecompileerde resultaten uitvoeren

De **uitvoeren** opdracht wordt gebruikt voor het uitvoeren van de resultaten.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

Hieronder vindt u optionele argumenten voor **uitvoeren**:

|Argument|Beschrijving|
|--------|-----------|
|-DataRoot [standaardwaarde '']|De gegevenshoofdmap voor de uitvoering van de metagegevens. Wordt standaard de **LOCALRUN_DATAROOT** omgevingsvariabele.|
|-MessageOut [standaardwaarde '']|Dump berichten in de console naar een bestand.|
|-Parallel standaardwaarde '1'|De indicator de gegenereerde lokaal uitvoeren stappen uitvoeren met het niveau van de opgegeven parallelle uitvoering.|
|-Verbose [standaardwaarde 'False']|Indicator om gedetailleerde uitvoer van de runtime weer te geven.|

Hier volgt een voorbeeld van gebruik:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-the-sdk-with-programming-interfaces"></a>Gebruik de SDK met programming interfaces

De API's bevinden zich in de LocalRunHelper.exe. U kunt deze gebruiken voor het integreren van de functionaliteit van de U-SQL-SDK en de C# testframework voor het schalen van uw lokale test van U-SQL-script. In dit artikel wordt ik de standaard C#-eenheid testproject gebruiken om te laten zien hoe deze interfaces voor het testen van uw U-SQL-script gebruiken.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>Stap 1: C#-eenheid test-project en de configuratie maken

- Maak een C#-eenheid test-project via Bestand > Nieuw > Project > Visual C# > testen > Project eenheid testen.
- LocalRunHelper.exe toevoegen als een verwijzing voor het project. De LocalRunHelper.exe bevindt zich op \build\runtime\LocalRunHelper.exe in Nuget-pakket.

    ![Azure Data Lake U-SQL-SDK verwijzing toevoegen](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- U-SQL-SDK **alleen** x64 ondersteuningsomgeving, zorg ervoor dat u het doel van build-platform als x64 instellen. Dat kunt u instellen via projecteigenschap > bouwen > Platform doel.

    ![Azure Data Lake U-SQL-SDK configureren x64 Project](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Zorg ervoor dat uw testomgeving instellen als x64. In Visual Studio kunt u dit instellen via testen > instellingen testen > processorarchitectuur standaard > x64.

    ![Azure Data Lake U-SQL-SDK configureren x64 testomgeving](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- Zorg ervoor dat alle afhankelijkheidsbestanden onder NugetPackage\build\runtime\ naar project werkmap namelijk doorgaans ProjectFolder\bin\x64\Debug kopiëren.

### <a name="step-2-create-u-sql-script-test-case"></a>Stap 2: Maak Testscenario U-SQL-script

Hieronder vindt u de voorbeeldcode voor het testen van U-SQL-script. Voor het testen moet u scripts, invoerbestanden en bestanden van de verwachte uitvoer voorbereiden.

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


### <a name="programming-interfaces-in-localrunhelperexe"></a>Programming interfaces in LocalRunHelper.exe

LocalRunHelper.exe biedt de programming interfaces voor de U-SQL lokaal compileren, uitvoeren, enzovoort. De interfaces staan.

**Constructor**

openbare LocalRunHelper ([System.IO.TextWriter messageOutput = null])

|Parameter|Type|Beschrijving|
|---------|----|-----------|
|messageOutput|System.IO.TextWriter|voor Uitvoerberichten dat is ingesteld op null-Console te gebruiken|

**Eigenschappen**

|Eigenschap|Type|Beschrijving|
|--------|----|-----------|
|AlgebraPath|Tekenreeks|Het pad naar bestand wiskundige (wiskundige-bestand is een van de compilatieresultaten)|
|CodeBehindReferences|Tekenreeks|Als het script aanvullende code achter verwijzingen heeft, geeft u de paden van elkaar gescheiden door ';'|
|CppSdkDir|Tekenreeks|CppSDK directory|
|CurrentDir|Tekenreeks|Huidige map|
|DataRoot|Tekenreeks|Pad naar de hoofdmap van gegevens|
|DebuggerMailPath|Tekenreeks|Het pad naar het foutopsporingsprogramma mailslot|
|GenerateUdoRedirect|BOOL|Als we wilt genereren bij het laden van assembly omleiding config overschrijven|
|HasCodeBehind|BOOL|Als het script heeft code achter|
|InputDir|Tekenreeks|Map voor invoergegevens|
|MessagePath|Tekenreeks|Bericht dump bestandspad|
|OutputDir|Tekenreeks|Map voor de uitvoergegevens|
|Parallelle uitvoering|int|Parallelle uitvoering de wiskundige uitvoeren|
|ParentPid|int|De Pincode van de bovenliggende waarop de service bewaakt om af te sluiten, ingesteld op 0 of een negatieve te negeren|
|ResultPath|Tekenreeks|Resultaat dump bestandspad|
|RuntimeDir|Tekenreeks|Runtime-map|
|scriptPath|Tekenreeks|Waar vind ik het script|
|Recente|BOOL|Recente compileren of niet|
|TempDir|Tekenreeks|Tijdelijke map|
|UseDataBase|Tekenreeks|Geef de database moet worden gebruikt voor de code achter tijdelijke assembly-registratie, master standaard|
|WorkDir|Tekenreeks|Voorkeur werkmap|


**Methode**

|Methode|Beschrijving|retourneren|Parameter|
|------|-----------|------|---------|
|openbare bool DoCompile()|Compileren van de U-SQL-script|True indien geslaagd| |
|openbare bool DoExec()|Het gecompileerde resultaat uitvoeren|True indien geslaagd| |
|openbare bool DoRun()|Voer het U-SQL-script (compileren + uitvoeren)|True indien geslaagd| |
|openbare bool IsValidRuntimeDir (pad tekenreeks)|Controleer of het opgegeven pad geldig runtime-pad is|Geldt voor geldige|Het pad naar map runtime|


## <a name="faq-about-common-issue"></a>Veelgestelde vragen over veel voorkomende problemen

### <a name="error-1"></a>Fout 1:
E_CSC_SYSTEM_INTERNAL: Interne fout! Kan bestand of assembly 'ScopeEngineManaged.dll' of een afhankelijkheid hiervan niet laden. De opgegeven module kan niet worden gevonden.

Controleer het volgende:

- Zorg ervoor dat u hebt x64 omgeving. Het doelplatform build en de testomgeving moet worden x64, raadpleeg dan **stap 1: eenheid maken C#-project en configuratie testen** hierboven.
- Zorg ervoor dat u alle afhankelijkheidsbestanden onder NugetPackage\build\runtime\ naar project werkmap hebt gekopieerd.


## <a name="next-steps"></a>Volgende stappen

* Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
* Zie voor logboekregistratie van diagnostische gegevens [toegang tot logboeken met diagnostische gegevens voor Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).
* Zie voor een complexere query [websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Taakdetails Zie [gebruik taak Browser en weergave van de taak voor Azure Data Lake Analytics-taken](data-lake-analytics-data-lake-tools-view-jobs.md).
* Zie voor het gebruik van de weergave van de uitvoering hoekpunt [gebruik van de Vertex Execution View in Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
