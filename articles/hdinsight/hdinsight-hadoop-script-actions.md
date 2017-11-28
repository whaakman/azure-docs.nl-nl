---
title: Scriptactieontwikkeling met HDInsight - Azure | Microsoft Docs
description: "Informatie over het aanpassen van Hadoop-clusters met scriptactie. Scriptactie kan worden gebruikt voor het installeren van extra software op een Hadoop-cluster of de configuratie van toepassingen zijn geïnstalleerd op een cluster te wijzigen."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 836d68a8-8b21-4d69-8b61-281a7fe67f21
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: d0e95014f6ebfc4e0286d3a12999c918f831b489
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Scripts voor HDInsight Windows gebaseerde clusters scriptactie ontwikkelen
Informatie over het scriptactie om scripts te schrijven voor HDInsight. Zie voor meer informatie over het gebruik van de scriptactie scripts [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster.md). Zie voor hetzelfde artikel geschreven voor Linux gebaseerde HDInsight-clusters, [scriptactie ontwikkelen scripts voor HDInsight](hdinsight-hadoop-script-actions-linux.md).



> [!IMPORTANT]
> De stappen in dit document werkt alleen voor Windows gebaseerde HDInsight-clusters. HDInsight is alleen beschikbaar in Windows voor versies lager is dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor meer informatie over het gebruik van scriptacties met Linux gebaseerde clusters [Scriptactieontwikkeling met HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).
>
>



Scriptactie kan worden gebruikt voor het installeren van extra software op een Hadoop-cluster of de configuratie van toepassingen zijn geïnstalleerd op een cluster te wijzigen. Scriptacties zijn scripts die worden uitgevoerd op de clusterknooppunten bij HDInsight-clusters worden geïmplementeerd en ze worden uitgevoerd zodra de knooppunten in het cluster HDInsight-configuratie voltooien. Een scriptactie wordt uitgevoerd onder de systeem-account beheerdersbevoegdheden en biedt volledige toegangsrechten voor de clusterknooppunten. Elk cluster kan worden opgegeven met een lijst met scriptacties om te worden uitgevoerd in de volgorde waarin ze worden opgegeven.

> [!NOTE]
> Als u zich voor het volgende foutbericht weergegeven:
>
> System.Management.Automation.CommandNotFoundException; ExceptionMessage: De term 'Opslaan HDIFile' wordt niet herkend als de naam van een cmdlet, functie, scriptbestand of programma. Controleer de spelling van de naam of als een pad opgenomen is, Controleer of het pad juist is en probeer het opnieuw.
> Het is omdat u niet de methoden hebt opgenomen.  Zie [Help-methoden voor aangepaste scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).
>
>

## <a name="sample-scripts"></a>Voorbeeldscripts
De scriptactie is voor het maken van HDInsight-clusters op Windows-besturingssysteem, Azure PowerShell-script. Het volgende script is een voorbeeld voor het configureren van de bestanden van de site-configuratie:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Het script neemt vier parameters, de naam van het configuratiebestand, de eigenschap die u wijzigen wilt, de waarde die u instellen wilt, en een beschrijving. Bijvoorbeeld:

    hive-site.xml hive.metastore.client.socket.timeout 90

Deze parameters wordt de waarde hive.metastore.client.socket.timeout ingesteld op 90 in het bestand hive-site.xml.  De standaardwaarde is 60 seconden.

Dit voorbeeldscript kan ook worden gevonden op [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight biedt verschillende scripts voor het installeren van extra onderdelen op HDInsight-clusters:

| Naam | Script |
| --- | --- |
| **Spark installeren** |https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Zie [installeert en gebruikt Spark in HDInsight-clusters][hdinsight-install-spark]. |
| **R installeren** |https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Zie [installeert en gebruikt R op HDInsight-clusters][hdinsight-r-scripts]. |
| **Solr installeren** |https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Zie [installeert en gebruikt Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md). |
| - **Giraph installeren** |https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Zie [installeert en gebruikt Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install.md). |

Scriptactie kan worden geïmplementeerd via de Azure portal, Azure PowerShell of met behulp van de HDInsight .NET SDK.  Zie voor meer informatie [aanpassen HDInsight-clusters met behulp van de scriptactie][hdinsight-cluster-customize].

> [!NOTE]
> De voorbeeldscripts werkt alleen met HDInsight-cluster versie 3.1 of hoger. Zie voor meer informatie over de versies van HDInsight-cluster, [versies van HDInsight-cluster](hdinsight-component-versioning.md).
>
>

## <a name="helper-methods-for-custom-scripts"></a>Help-methoden voor aangepaste scripts
Script actie Help-methoden zijn hulpprogramma's die u gebruiken kunt bij het schrijven van aangepaste scripts. Deze methoden zijn gedefinieerd in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), en kan worden opgenomen in uw scripts met het volgende voorbeeld:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Hier volgen de methoden die worden geleverd door dit script:

| Help-methode | Beschrijving |
| --- | --- |
| **Opslaan HDIFile** |Downloaden van een bestand van de opgegeven id URI (Uniform Resource) naar een locatie op de lokale schijf die is gekoppeld aan de virtuele machine van Azure-knooppunt dat is toegewezen aan het cluster. |
| **Vouw HDIZippedFile** |Een ZIP-bestand uitpakken. |
| **Aanroepen HDICmdScript** |Een script uitvoeren vanaf cmd.exe. |
| **Schrijven HDILog** |Uitvoer van het aangepaste script gebruikt voor de scriptactie van een schrijven. |
| **Get-Services** |Ophalen van een lijst met services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-Service** |Met de naam van de specifieke service als invoer, krijgt u gedetailleerde informatie voor een specifieke service (servicenaam, proces-ID, status, enz.) op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIServices** |Haal een lijst van HDInsight-services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIService** |Met de specifieke HDInsight-servicenaam als invoer, krijgt u gedetailleerde informatie voor een specifieke service (servicenaam, proces-ID, status, enz.) op de computer waarop het script wordt uitgevoerd. |
| **Get-ServicesRunning** |Ophalen van een lijst met services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-ServiceRunning** |Controleer of een specifieke service (met de naam) wordt uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIServicesRunning** |Haal een lijst van HDInsight-services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIServiceRunning** |Controleer of een specifieke HDInsight-service (met de naam) wordt uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIHadoopVersion** |Ophalen van Hadoop geïnstalleerd op de computer waarop het script wordt uitgevoerd. |
| **Test IsHDIHeadNode** |Controleer of de computer waarop het script wordt uitgevoerd een hoofdknooppunt. |
| **Test IsActiveHDIHeadNode** |Controleer of de computer waarop het script wordt uitgevoerd een actieve hoofdknooppunt is. |
| **Test IsHDIDataNode** |Controleer of de computer waarop het script wordt uitgevoerd een gegevensknooppunt is. |
| **Bewerken HDIConfigFile** |Bewerk de config-bestanden hive-site.xml, core site.xml, hdfs-site.xml, mapred site.xml of yarn-site.xml. |

## <a name="best-practices-for-script-development"></a>Aanbevolen procedures voor het ontwikkelen van scripts
Wanneer u een aangepast script voor een HDInsight-cluster ontwikkelt, zijn er enkele aanbevolen procedures rekening moet houden:

* Controleer op de Hadoop-versie

    Alleen HDInsight versie 3.1 (Hadoop 2.4) en hoger scriptactie aangepaste onderdelen installeren op een cluster met ondersteuning. In een script, moet u de **Get-HDIHadoopVersion** Help-methode om te controleren van de Hadoop-versie voordat u doorgaat met de andere taken uitvoeren in het script.
* Stabiele koppelingen naar scriptbronnen die bevatten

    Gebruikers moeten ervoor zorgen dat alle scripts en andere artefacten die in het aanpassen van een cluster worden gebruikt tijdens de levensduur van het cluster beschikbaar blijven en dat de versies van deze bestanden niet voor de duur wijzigen. Deze resources zijn vereist als de installatiekopie van de knooppunten in het cluster vereist is. De aanbevolen procedure is om te downloaden en te archiveren alles in een opslagaccount die bepaalt van de gebruiker. Dit is het standaardopslagaccount of een van de extra opslagaccounts die is opgegeven op het moment van implementatie voor een aangepaste cluster.
    In de Spark en R aangepast cluster voorbeelden opgegeven in de documentatie, bijvoorbeeld: we hebben aangebracht een lokale kopie van de resources in dit opslagaccount: https://hdiconfigactions.blob.core.windows.net/.
* Zorg ervoor dat het cluster aanpassing script idempotent

    U moet verwachten dat de knooppunten van een HDInsight-cluster wordt teruggezet tijdens de levensduur van het cluster. Het cluster aanpassing-script wordt uitgevoerd wanneer de installatiekopie van een cluster wordt hersteld. Dit script moet worden ontworpen om de idempotent in de zin dat op de installatiekopie, het script ervoor zorgen moet dat het cluster wordt geretourneerd naar dezelfde aangepast staat waarin deze zich bevond vlak nadat het script is uitgevoerd voor de eerste keer wanneer het cluster in eerste instantie is gemaakt. Bijvoorbeeld, als een toepassing op D:\AppLocation een aangepast script geïnstalleerd op de eerste keer uitvoert, vervolgens bij elke volgende uitvoeren, op de installatiekopie, het script moet controleren of de toepassing op de locatie D:\AppLocation bestaat voordat u doorgaat met de overige stappen in de script.
* Aangepaste onderdelen installeren op de optimale locatie

    Wanneer de clusterknooppunten worden teruggezet, kunnen de resource-station C:\ en D:\ systeemstation worden geformatteerd, wat resulteert in het verlies van gegevens en toepassingen die was geïnstalleerd op deze schijven. Dit kan ook gebeuren als een knooppunt Azure virtuele machine (VM) die deel uitmaakt van het cluster uitvalt en wordt vervangen door een nieuw knooppunt. U kunt onderdelen installeren op station D:\ of op de locatie C:\apps op het cluster. Alle andere locaties op het station C:\ zijn gereserveerd. Geef de locatie waar toepassingen of -bibliotheken in het cluster aanpassing script moet worden geïnstalleerd.
* Zorg ervoor dat hoge beschikbaarheid van de cluster-architectuur

    HDInsight heeft een actief / passief-architectuur voor hoge beschikbaarheid, waarin een hoofdknooppunt in de actieve modus is (waarbij de HDInsight-services worden uitgevoerd) en het hoofdknooppunt in standby-modus (in welke HDInsight services niet worden uitgevoerd) is. De knooppunten overschakelen actieve en passieve modus als de HDInsight-services worden onderbroken. Als een scriptactie wordt gebruikt om services te installeren op beide head knooppunten voor hoge beschikbaarheid, houd er rekening mee dat het mechanisme voor HDInsight failover kan niet automatisch een failover deze services gebruiker geïnstalleerd. Services op hoofdknooppunten HDInsight die naar verwachting maximaal beschikbaar te zijn dus door de gebruiker geïnstalleerd moeten hebben hun eigen mechanisme voor failover als deze in de modus actief / passief of in de actieve-actieve modus.

    Een HDInsight-scriptactie-opdracht wordt uitgevoerd op beide hoofdknooppunten wanneer de functie head-knooppunt is opgegeven als een waarde in de *ClusterRoleCollection* parameter. Wanneer u een aangepast script ontwerpt, zorg er dus dat uw script hoogte van deze installatie is. U moet niet uitvoeren op problemen waar dezelfde services zijn geïnstalleerd en gestart op zowel de hoofdknooppunten en ze met elkaar concurreren. Let op dat gegevens niet verloren tijdens teruggezet, zodat software is geïnstalleerd via een scriptactie te netwerkfouten dergelijke gebeurtenissen. Toepassingen moeten worden ontworpen voor gebruik met maximaal beschikbare gegevens die wordt gedistribueerd over veel knooppunten. Houd er rekening mee dat wel 1/5 van de knooppunten in een cluster met een installatiekopie op hetzelfde moment kunnen worden hersteld.
* De aangepaste onderdelen voor het gebruik van Azure Blob-opslag configureren

    De aangepaste onderdelen die u op de clusterknooppunten installeert wellicht een standaardconfiguratie Hadoop Distributed File System (HDFS) opslag gebruiken. U kunt de configuratie voor het gebruik van Azure Blob storage in plaats daarvan moet wijzigen. Terugzetten van een cluster de installatiekopie, het bestandssysteem HDFS opgehaald geformatteerd en verliest u gegevens die is opgeslagen. Azure Blob storage in plaats daarvan zorgt ervoor dat uw gegevens worden bewaard.

## <a name="common-usage-patterns"></a>Algemene gebruikspatronen
Deze sectie bevat richtlijnen over het implementeren van enkele van de algemene gebruikspatronen die u tegenkomen kunt tijdens het schrijven van uw eigen aangepaste scripts.

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren
Vaak in script actie ontwikkeling, u denkt dat de noodzaak om in te stellen van omgevingsvariabelen. Bijvoorbeeld, is een meest waarschijnlijke scenario wanneer u een binair bestand vanaf een externe site downloaden op het cluster installeren en voeg de locatie van waar het wordt geïnstalleerd in uw omgeving padvariabele. Het volgende fragment laat zien hoe u omgevingsvariabelen worden ingesteld in het aangepaste script.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Deze instructie stelt de omgevingsvariabele **MDS_RUNNER_CUSTOM_CLUSTER** op de waarde 'true' en ook het bereik van deze variabele op alle computers worden ingesteld. Soms is het belangrijk dat de omgevingsvariabelen worden ingesteld op het juiste bereik – computer of gebruiker. Raadpleeg [hier] [ 1] voor meer informatie over het instellen van omgevingsvariabelen.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Toegang tot locaties waar de aangepaste scripts worden opgeslagen
Scripts die worden gebruikt voor het aanpassen van een cluster moeten ofwel niet in het standaardopslagaccount voor het cluster of in een openbare alleen-lezen container op andere storage-account. Als uw script toegang heeft tot resources die zich ergens anders bevindt deze zich in een openbaar toegankelijke moeten (ten minste openbare alleen-lezen). U wilt bijvoorbeeld toegang tot een bestand en sla deze met de SaveFile HDI-opdracht.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

In dit voorbeeld moet u ervoor zorgen dat de 'somecontainer' in 'somestorageaccount' van de storage-account-container openbaar toegankelijk is. Anders wordt het script een 'Niet gevonden'-uitzondering genereert en mislukken.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Parameters doorgeven aan de cmdlet Add-AzureRmHDInsightScriptAction
Meerdere parameters doorgeven aan de cmdlet Add-AzureRmHDInsightScriptAction, moet u de indeling van de tekenreekswaarde bevat van alle parameters voor het script. Bijvoorbeeld:

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

of

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Uitzondering voor mislukte Clusterimplementatie
Als u nauwkeurig Blijf op de hoogte wilt van het feit dat de aanpassing van het cluster kan niet zoals verwacht, is het belangrijk is een uitzondering en mislukt het maken van het cluster. U wilt bijvoorbeeld een bestand niet verwerken als deze bestaat en verwerken fout waar het bestand niet bestaat. Dit zou ervoor zorgen dat het script wordt afgesloten zonder problemen en de status van het cluster goed bekend is. Het volgende fragment geeft een voorbeeld van hoe om dit te bereiken:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

In dit fragment als het bestand niet bestaat, zou dit leiden tot een status waarin het script daadwerkelijk wordt afgesloten zonder problemen na het afdrukken van het foutbericht en het cluster actief is, ervan uitgaande dat deze ' ' voltooid cluster aanpassing bereikt. Als u wilt nauwkeurig de hoogte gesteld van het feit dat de cluster-aanpassing in wezen is niet gelukt zoals verwacht vanwege een ontbrekend bestand, verdient het Veroorzaak een exception en de cluster aanpassing stap mislukt. U moet het volgende voorbeeld-codefragment in plaats daarvan gebruiken om dit te bereiken.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Controlelijst voor het implementeren van een scriptactie
Hier volgen de stappen die we bij de voorbereiding voor het implementeren van deze scripts duurde:

1. Plaats de bestanden die de aangepaste scripts op een locatie die toegankelijk is voor de clusterknooppunten tijdens de implementatie bevatten. Dit is de standaard of extra opslagaccounts die zijn opgegeven op het moment van implementatie van het cluster of een andere openbaar toegankelijke storage-container.
2. Controles in scripts om ervoor te zorgen dat ze idempotently, uitvoeren zodat het script kan meerdere keren worden uitgevoerd op hetzelfde knooppunt toevoegen.
3. Gebruik de **Write-Output** Azure PowerShell-cmdlet om af te drukken voor STDOUT en STDERR. Gebruik geen **Write-Host**.
4. Gebruik een tijdelijke map, zoals $env: TEMP te houden van het gedownloade bestand gebruikt door de scripts en vervolgens opschonen van nadat scripts hebt uitgevoerd.
5. Aangepaste software alleen op D:\ of C:\apps installeren. Andere locaties op het station C: mag niet worden gebruikt als deze gereserveerd zijn. Houd er rekening mee dat bestanden op het station C: buiten de map C:\apps installeren leiden setup kopieerfouten tijdens reimages van het knooppunt tot kan.
6. In het geval dat instellingen voor OS-niveau of configuratiebestanden voor Hadoop-service zijn gewijzigd, kunt u HDInsight-services opnieuw starten zodat ze kunnen alle instellingen OS-niveau, zoals de omgevingsvariabelen instellen in de scripts ophalen.

## <a name="debug-custom-scripts"></a>Fouten opsporen in aangepaste scripts
De script-foutenlogboeken zijn opgeslagen, samen met andere uitvoer, in het standaardopslagaccount die u hebt opgegeven voor het cluster bij het maken ervan. De logboeken worden opgeslagen in een tabel met de naam van de *u < \cluster-name-fragment >< \time-stamp > bestand*. Dit zijn de cumulatieve logboeken die records van alle knooppunten (hoofdknooppunt en worker-knooppunten) op waarop het script wordt uitgevoerd in het cluster hebben.

Er is een eenvoudige manier om te controleren van de logboeken met HDInsight Tools voor Visual Studio. Zie voor het installeren van de hulpprogramma's voor [aan de slag met Visual Studio Hadoop-hulpprogramma's voor HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md#install-and-upgrade-data-lake-tools-for-visual-studio)

**Controleer het logboek met Visual Studio**

1. Open Visual Studio.
2. Klik op **weergave**, en klik vervolgens op **Server Explorer**.
3. Met de rechtermuisknop op 'Azure', klikt u op verbinding maken met **Microsoft Azure-abonnementen**, en voer uw referenties.
4. Vouw **opslag**, vouw de Azure storage-account gebruikt als het standaardbestandssysteem uit, vouw **tabellen**, en dubbelklik vervolgens op de naam van de tabel.

U kunt ook externe in de clusterknooppunten om te zien zowel STDOUT en STDERR voor aangepaste scripts. De logboeken op elk knooppunt worden alleen op dat knooppunt en vastgelegd in **C:\HDInsightLogs\DeploymentAgent.log**. Deze logboekbestanden Leg alle uitvoerwaarden van de aangepaste scripts. Een voorbeeld logboek codefragment voor een Spark-scriptactie ziet er als volgt:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


In dit logboek is duidelijk dat de actie Spark-script is uitgevoerd op de virtuele machine met de naam HEADNODE0 en dat er geen uitzonderingen tijdens het uitvoeren veroorzaakt.

In het geval dat een uitvoeringsfout optreedt, wordt de uitvoer met een beschrijving van deze ook opgenomen in dit logboekbestand. De informatie in deze logboeken zijn te helpen bij foutopsporing script problemen die zich kunnen voordoen.

## <a name="see-also"></a>Zie ook
* [HDInsight-clusters met behulp van de scriptactie aanpassen][hdinsight-cluster-customize]
* [Installeren en gebruiken van Spark in HDInsight-clusters][hdinsight-install-spark]
* [Installeren en gebruiken van R op HDInsight-clusters][hdinsight-r-scripts]
* [Installeren en gebruiken van Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md).
* [Installeren en gebruiken van Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
