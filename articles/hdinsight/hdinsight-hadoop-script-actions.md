---
title: Ontwikkeling van scriptacties met HDInsight - Azure
description: Informatie over het aanpassen van Hadoop-clusters met scriptacties. Script Action kan worden gebruikt om aanvullende software die wordt uitgevoerd op een Hadoop-cluster te installeren of te wijzigen van de configuratie van toepassingen worden geïnstalleerd op een cluster.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: f26a11e623148eecc8096b45c6ac9df68b8e102a
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39599153"
---
# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Voor clusters op basis van HDInsight Windows Script Action-scripts ontwikkelen
Informatie over het schrijven van scripts met Script Action voor HDInsight. Zie voor meer informatie over het gebruik van Script Action-scripts [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster.md). Zie voor hetzelfde artikel is geschreven voor Linux gebaseerde HDInsight-clusters, [ontwikkelen Script Action-scripts voor HDInsight](hdinsight-hadoop-script-actions-linux.md).



> [!IMPORTANT]
> De stappen in dit document kan uitsluitend worden gebruikt voor HDInsight op basis van een Windows-clusters. HDInsight is alleen beschikbaar voor Windows voor versies lager dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor meer informatie over het gebruik van scriptacties met Linux gebaseerde clusters [ontwikkelen via scriptacties met HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).
>
>



Script Action kan worden gebruikt om aanvullende software die wordt uitgevoerd op een Hadoop-cluster te installeren of te wijzigen van de configuratie van toepassingen worden geïnstalleerd op een cluster. Scriptacties zijn scripts die worden uitgevoerd op de clusterknooppunten bij het HDInsight-clusters worden geïmplementeerd en ze worden uitgevoerd wanneer de knooppunten in het cluster HDInsight-configuratie voltooien. Een scriptactie wordt uitgevoerd onder het account systeembeheerdersbevoegdheden en biedt volledige toegangsrechten voor de clusterknooppunten. Elk cluster kan worden opgegeven met een lijst met scriptacties moet worden uitgevoerd in de volgorde waarin ze zijn opgegeven.

> [!NOTE]
> Als u de volgende strekking weergegeven:
>
> System.Management.Automation.CommandNotFoundException; ExceptionMessage: De term 'Opslaan-HDIFile' wordt niet herkend als de naam van een cmdlet, functie, scriptbestand of programma. Controleer de spelling van de naam, of als een pad opgenomen is, dat het pad juist is en probeer het opnieuw.
> Dit is omdat u niet de helpermethoden hebt opgenomen.  Zie [helpermethoden voor aangepaste scripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).
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

Het script neemt vier parameters, de naam van de configuratie, de eigenschap die u wijzigen wilt, de waarde die u instellen wilt, en een beschrijving. Bijvoorbeeld:

    hive-site.xml hive.metastore.client.socket.timeout 90

Deze parameters voor het instellen van de waarde hive.metastore.client.socket.timeout in 90 in het bestand hive-site.xml.  De standaardwaarde is 60 seconden.

Met dit voorbeeldscript kan ook worden gevonden op [ https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1 ](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

HDInsight biedt verschillende scripts voor het installeren van extra onderdelen in HDInsight-clusters:

| Naam | Script |
| --- | --- |
| **Spark installeren** | `https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1`. Zie [installeren en gebruiken Spark in HDInsight-clusters][hdinsight-install-spark]. |
| **R installeren** | `https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1`. Zie [installeren en gebruiken R op HDInsight-clusters](r-server/r-server-hdinsight-manage.md#install-additional-r-packages-on-the-cluster). |
| **Solr installeren** | `https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1`. Zie [installeren en gebruiken Solr op HDInsight-clusters](hdinsight-hadoop-solr-install.md). |
| **Giraph installeren** | `https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1`. Zie [installeren en gebruiken Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install.md). |
| **Hive-bibliotheken vooraf laden** | `https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1`. Zie [toevoegen Hive-bibliotheken op HDInsight-clusters](hdinsight-hadoop-add-hive-libraries.md) |


Script Action kan worden geïmplementeerd via Azure portal, Azure PowerShell of met behulp van de HDInsight .NET SDK.  Zie voor meer informatie, [aanpassen HDInsight-clusters met Script Action][hdinsight-cluster-customize].

> [!NOTE]
> De voorbeelden van scripts werken alleen met HDInsight-clusterversie 3.1 of hoger. Zie voor meer informatie over HDInsight-clusterversies [HDInsight-clusterversies](hdinsight-component-versioning.md).
>
>

## <a name="helper-methods-for-custom-scripts"></a>Help-methoden voor aangepaste scripts
Script actie helpermethoden zijn hulpprogramma's die u gebruiken kunt tijdens het schrijven van aangepaste scripts. Deze methoden zijn gedefinieerd in [ https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1 ](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1), en kan worden opgenomen in uw scripts met behulp van het volgende voorbeeld:

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

Hier volgen de methoden die worden geboden door dit script:

| Help-methode | Beschrijving |
| --- | --- |
| **Save-HDIFile** |Download een bestand van de opgegeven Uniform Resource Identifier (URI) naar een locatie op de lokale schijf die is gekoppeld aan het Azure-VM-knooppunt dat is toegewezen aan het cluster. |
| **Vouw HDIZippedFile** |Pak een ZIP-bestand. |
| **Invoke-HDICmdScript** |Een script uitvoeren vanaf cmd.exe. |
| **Write-HDILog** |Uitvoer van het aangepaste script gebruikt voor de scriptactie van een schrijven. |
| **Get-Services** |Haal een lijst van de services die worden uitgevoerd op de machine waarop het script wordt uitgevoerd. |
| **Get-Service** |Met de naam van de specifieke service als invoer, krijgt u gedetailleerde informatie voor een bepaalde service (servicenaam, proces-ID, status, enzovoort) op de machine waarop het script wordt uitgevoerd. |
| **Get-HDIServices** |Haal een lijst van HDInsight-services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIService** |Met de specifieke HDInsight-servicenaam als invoer, krijgt u gedetailleerde informatie voor een bepaalde service (servicenaam, proces-ID, status, enzovoort) op de machine waarop het script wordt uitgevoerd. |
| **Get-ServicesRunning** |Haal een lijst met services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-ServiceRunning** |Controleer of een bepaalde service (met name) wordt uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIServicesRunning** |Haal een lijst van HDInsight-services die worden uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIServiceRunning** |Controleer of een specifieke HDInsight-service (met name) wordt uitgevoerd op de computer waarop het script wordt uitgevoerd. |
| **Get-HDIHadoopVersion** |Download de versie van Hadoop is geïnstalleerd op de computer waarop het script wordt uitgevoerd. |
| **Test-IsHDIHeadNode** |Controleer of de computer waarop het script wordt uitgevoerd een hoofdknooppunt is. |
| **Test-IsActiveHDIHeadNode** |Controleer of de computer waarop het script wordt uitgevoerd een actieve hoofdknooppunt is. |
| **Test-IsHDIDataNode** |Controleer of de computer waarop het script wordt uitgevoerd een gegevensknooppunt is. |
| **Edit-HDIConfigFile** |Bewerk de config-bestanden hive-site.xml, core-site.xml, hdfs-site.xml, mapred-site.xml of yarn-site.xml. |

## <a name="best-practices-for-script-development"></a>Aanbevolen procedures voor het ontwikkelen van script
Wanneer u een aangepast script voor een HDInsight-cluster ontwikkelt, zijn er enkele aanbevolen procedures waarmee u rekening moet houden:

* Controleer voor de Hadoop-versie

    Alleen HDInsight versie 3.1 (2.4 Hadoop) en hoger aangepaste onderdelen installeren op een cluster met Script Action-ondersteuning. In het aangepaste script, moet u de **Get-HDIHadoopVersion** Help-methode om te controleren of de Hadoop-versie voordat u doorgaat met het uitvoeren van andere taken in het script.
* Stabiele koppelingen naar scriptbronnen

    Gebruikers moeten ervoor zorgen dat alle scripts en andere artefacten in het aanpassen van een cluster gebruikt gedurende de levensduur van het cluster beschikbaar blijven en dat de versies van deze bestanden niet voor de duur wijzigen zijn. Deze resources zijn vereist als de installatiekopie werd teruggezet van knooppunten in het cluster vereist is. De aanbevolen procedure is om te downloaden en te archiveren alles in een Storage-account dat de gebruiker bepaalt. Dit account kan worden het standaardaccount voor opslag of een van de extra opslagaccounts opgegeven op het moment van implementatie voor een aangepaste cluster.
    In de Spark- en R aangepast cluster voorbeelden opgegeven in de documentatie, bijvoorbeeld: er een lokale kopie van de resources in dit Storage-account is: https://hdiconfigactions.blob.core.windows.net/.
* Zorg ervoor dat het cluster aanpassing-script idempotent is

    U moet verwachten dat de knooppunten van een HDInsight-cluster tijdens de levensduur van het cluster worden teruggezet. Het cluster aanpassing-script wordt uitgevoerd wanneer een cluster wordt teruggezet. Met dit script moet worden ontworpen om te worden idempotent zijn in de zin dat bij installatiekopie terugzetten, het script ervoor zorgen moet dat het cluster wordt geretourneerd naar de dezelfde aangepast staat waarin deze zich bevond vlak nadat het script is uitgevoerd voor de eerste keer wanneer het cluster in eerste instantie is gemaakt. Bijvoorbeeld, als een toepassing op D:\AppLocation een aangepast script geïnstalleerd voor de eerste keer uitvoert, klikt u vervolgens op elke volgende uitvoering, op de installatiekopie terugzetten, het script moet controleren of de toepassing op de locatie D:\AppLocation bestaat voordat u doorgaat met de overige stappen in de script.
* Aangepaste onderdelen installeren op de optimale locatie

    Als clusterknooppunten worden teruggezet, kunnen de resource-station C:\ en D:\ systeemstation worden geformatteerd, wat resulteert in het verlies van gegevens en toepassingen die was geïnstalleerd op deze schijven. Dit verlies kan ook gebeuren als een knooppunt virtuele Azure-machine (VM) die deel uitmaakt van het cluster uitvalt en wordt vervangen door een nieuw knooppunt. U kunt onderdelen installeren op station D:\ of op de locatie C:\apps op het cluster. Alle andere locaties op het station C:\ zijn gereserveerd. Geef de locatie waar toepassingen of bibliotheken zijn in het cluster aanpassing script moet worden geïnstalleerd.
* Zorg voor hoge beschikbaarheid van de clusterarchitectuur

    HDInsight is een actief-passief-architectuur voor hoge beschikbaarheid, waarin een hoofdknooppunt in de actieve modus is (waarbij de HDInsight-services worden uitgevoerd) en het hoofdknooppunt in de stand-by-modus is (in welke HDInsight services worden niet uitgevoerd). De knooppunten overschakelen actieve en passieve modus als HDInsight-services worden onderbroken. Als een scriptactie wordt gebruikt om services te installeren op beide hoofdknooppunten voor een hoge beschikbaarheid, houd er rekening mee dat het mechanisme voor HDInsight failover kan niet automatisch failover wilt uitvoeren voor deze gebruikers geïnstalleerde services. Zodat gebruikers geïnstalleerde services op HDInsight-hoofdknooppunten die naar verwachting maximaal beschikbaar moeten hebben hun eigen mechanisme voor failover als deze in de modus actief-passief of zich in de modus actief-actief.

    Een HDInsight-scriptactie-opdracht wordt uitgevoerd op beide hoofdknooppunten wanneer de rol hoofdknooppunt is opgegeven als een waarde in de *ClusterRoleCollection* parameter. Dus bij het ontwerpen van een aangepast script, zorg ervoor dat uw script op de hoogte van deze installatie. U moet niet worden uitgevoerd in de problemen waar dezelfde services zijn geïnstalleerd en gestart op beide van de hoofdknooppunten en ze met elkaar concurreren. Ook rekening mee dat er gegevens verloren tijdens de installatiekopie werd teruggezet zijn, zodat software is geïnstalleerd via scriptactie te zijn tegen dergelijke gebeurtenissen. Toepassingen moeten worden ontworpen om te werken met maximaal beschikbare gegevens die zijn verdeeld over veel knooppunten. Maximaal 1/5 van de knooppunten in een cluster kan worden teruggezet op hetzelfde moment.
* Configureren van de aangepaste onderdelen voor het gebruik van Azure Blob-opslag

    De aangepaste onderdelen die u op de clusterknooppunten installeert mogelijk een standaardconfiguratie Hadoop Distributed File System (HDFS)-opslag gebruiken. De configuratie voor het gebruik van Azure Blob-opslag in plaats daarvan moet u wijzigen. De HDFS-bestandssysteem wordt geformatteerd en verliest u gegevens die is opgeslagen op een installatiekopie cluster. Met behulp van Azure Blob-opslag in plaats daarvan zorgt ervoor dat uw gegevens worden bewaard.

## <a name="common-usage-patterns"></a>Algemene gebruikspatronen
Deze sectie bevat richtlijnen over het implementeren van enkele van de algemene gebruikspatronen die u kunt tegenkomen tijdens het schrijven van uw eigen aangepaste scripts.

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren
In de ontwikkeling van scriptacties kunt u vaak de noodzaak om in te stellen van omgevingsvariabelen. Bijvoorbeeld, is een meest waarschijnlijke scenario wanneer u een binair bestand vanaf een externe site downloaden, installeer deze op het cluster en voeg de locatie van waar het wordt geïnstalleerd in uw omgeving padvariabele. Het volgende fragment toont hoe u omgevingsvariabelen worden ingesteld in het aangepaste script.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Deze instructie stelt de omgevingsvariabele **MDS_RUNNER_CUSTOM_CLUSTER** op de waarde 'true' en ook het bereik van deze variabele op alle computers worden ingesteld. Het is belangrijk dat de omgevingsvariabelen worden ingesteld op het juiste bereik-computer of gebruiker. Raadpleeg [hier] [ 1] voor meer informatie over het instellen van omgevingsvariabelen.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Toegang tot locaties waar de aangepaste scripts worden opgeslagen
Scripts die worden gebruikt voor het aanpassen van een cluster moeten aan een zich in het standaardopslagaccount voor het cluster of in een openbare container voor de alleen-lezen op een andere storage-account. Als uw script toegang heeft tot resources die zich ergens anders bevinden moet de resources openbaar leesbaar zijn. U wilt bijvoorbeeld een bestand openen en opslaan met behulp van de opdracht SaveFile HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

In dit voorbeeld moet u ervoor zorgen dat de container `somecontainer` in storage-account `somestorageaccount` openbaar toegankelijk is. Anders wordt het script een 'Niet gevonden'-uitzondering genereert en mislukken.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Parameters doorgeven aan de cmdlet Add-AzureRmHDInsightScriptAction
Meerdere parameters doorgeven aan de cmdlet Add-AzureRmHDInsightScriptAction, moet u de indeling van de tekenreekswaarde bevat alle parameters voor het script. Bijvoorbeeld:

    "-CertifcateUri wasb:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

of

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Uitzondering voor de implementatie van het cluster is mislukt
Als u nauwkeurig een melding ontvangen wilt van het feit dat de cluster-aanpassing is niet gelukt zoals verwacht, is het belangrijk dat u een uitzondering en mislukt het maken van een cluster. U wilt bijvoorbeeld een bestand niet verwerken als deze bestaat en verwerken van de foutaanvraag waar het bestand niet bestaat. Dit zou ervoor zorgen dat het script zonder problemen wordt afgesloten en de status van het cluster goed bekend is. In het volgende codefragment wordt een voorbeeld van hoe u om dit te bereiken:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

In dit fragment als het bestand niet bestaat, zou dit leiden tot een status waarin het script daadwerkelijk wordt afgesloten zonder problemen na het afdrukken van het foutbericht en het cluster actief is, ervan uitgaande dat deze 'is' cluster aanpassingsproces voltooid is bereikt. Als u wilt nauwkeurig worden geïnformeerd over het feit dat clusteraanpassing in feite is niet gelukt zoals verwacht vanwege een ontbrekend bestand, meer geschikt is voor een uitzondering en het uitvoeren van de cluster aanpassing stap. Hiervoor moet u het volgende voorbeeld-codefragment in plaats daarvan gebruiken.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Controlelijst voor het implementeren van een scriptactie
Hier volgen de stappen die werd bij het maken van deze scripts te implementeren:

1. Plaats de bestanden die de aangepaste scripts in een locatie die toegankelijk is voor de clusterknooppunten is tijdens de implementatie bevatten. Dit kan zijn dat een van de standaard- of extra opslagaccounts die zijn opgegeven op het moment van implementatie van het cluster of een andere openbaar toegankelijke storage-container.
2. Controles in scripts om ervoor te zorgen dat ze idempotently uitvoeren, zodat het script kan meerdere keren worden uitgevoerd op hetzelfde knooppunt toevoegen.
3. Gebruik de `Write-Output` Azure PowerShell-cmdlet om af te drukken op STDOUT en STDERR. Gebruik geen `Write-Host`.
4. Gebruik een tijdelijke map, zoals `$env:TEMP`, te houden van het gedownloade bestand dat wordt gebruikt door de scripts en schoon ze nadat scripts hebt uitgevoerd.
5. Aangepaste software alleen op D:\ of C:\apps installeren. Andere locaties op het station C: mag niet worden gebruikt omdat deze gereserveerd zijn. Bestanden op het station C: buiten de map C:\apps installeren, kan dit leiden tot fouten met setup tijdens reimages van het knooppunt.
6. In het geval dat niveau van de OS of Hadoop-service-configuratiebestanden zijn gewijzigd, kunt u HDInsight-services opnieuw starten zodat ze verder kunnen gaan alle instellingen voor OS-niveau, zoals de omgevingsvariabelen instellen in de scripts.

## <a name="debug-custom-scripts"></a>Fouten opsporen in aangepaste scripts
De foutenlogboeken van het script worden opgeslagen, samen met andere uitvoer, in het standaardopslagaccount die u hebt opgegeven voor het cluster op het is gemaakt. De logboeken worden opgeslagen in een tabel met de naam van de *u < \cluster-name-fragment >< \time-stamp > bestand*. Dit zijn de samengevoegde logboeken die records uit alle van de knooppunten (hoofdknooppunt en worker-knooppunten) waarop het script wordt uitgevoerd in het cluster hebben.

Een eenvoudige manier om te controleren of de logboeken is het gebruik van HDInsight Tools voor Visual Studio. Zie voor het installeren van de hulpprogramma's, [aan de slag met Visual Studio Hadoop-hulpprogramma's voor HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md#install-or-update-data-lake-tools-for-visual-studio)

**Om te controleren of het logboek met Visual Studio**

1. Open Visual Studio.
2. Klik op **weergave**, en klik vervolgens op **Server Explorer**.
3. Met de rechtermuisknop op 'Azure', klikt u op verbinding maken met **Microsoft Azure-abonnementen**, en voer vervolgens uw referenties.
4. Vouw **opslag**, vouw de Azure storage-account gebruikt als het standaardbestandssysteem uit, vouw **tabellen**, en dubbelklik vervolgens op de naam van de tabel.

U kunt ook externe in de clusterknooppunten voor zowel STDOUT en STDERR voor aangepaste scripts. De logboeken op elk knooppunt worden alleen op dat knooppunt en bent aangemeld bij **C:\HDInsightLogs\DeploymentAgent.log**. Deze logboekbestanden Noteer alle uitvoer van het aangepaste script. Een voorbeeld van de logboekbestanden fragment voor de actie van een Spark-script ziet er als volgt:

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


In dit logboek is het duidelijk dat de actie van de Spark-script is uitgevoerd op de virtuele machine met de naam HEADNODE0 en dat er geen uitzonderingen zijn opgetreden tijdens de uitvoering.

In het geval dat een uitvoeringsfout optreedt, wordt de uitvoer met een beschrijving van het is ook opgenomen in dit logboekbestand. De informatie in deze logboeken moet nuttig zijn bij het opsporen van fouten in script-problemen die zich kunnen voordoen.

## <a name="see-also"></a>Zie ook
* [HDInsight clusters aanpassen met Script Action][hdinsight-cluster-customize]
* [Installeren en gebruiken van Spark op HDInsight-clusters][hdinsight-install-spark]
* [Solr installeren en gebruiken op HDInsight-clusters](hdinsight-hadoop-solr-install.md).
* [Giraph installeren en gebruiken op HDInsight-clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
