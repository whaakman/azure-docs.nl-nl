---
title: Azure HDInsight-clusters aanpassen met behulp van scriptacties
description: Aangepaste onderdelen toevoegen aan HDInsight op basis van Linux-clusters met behulp van scriptacties. Scriptacties zijn Bash-scripts die kunnen worden gebruikt voor het aanpassen van de configuratie van het cluster of toevoegen van extra services en hulpprogramma's, zoals Hue, Solr of R.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: fe0fec082ace997a3bd66ca7c7575ce8dce3be1a
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885567"
---
# <a name="customize-azure-hdinsight-clusters-by-using-script-actions"></a>Azure HDInsight-clusters aanpassen met behulp van scriptacties

Azure HDInsight biedt een configuratiemethode aangeroepen **scriptacties** die aangepaste scripts om aan te passen van het cluster aanroept. Deze scripts worden gebruikt om extra onderdelen installeren en configuratie-instellingen wijzigen. Scriptacties kunnen worden gebruikt tijdens of na het maken van clusters.

Scriptacties kunnen ook in de Azure Marketplace worden gepubliceerd als een HDInsight-toepassing. Zie voor meer informatie over HDInsight-toepassingen, [een HDInsight-toepassing publiceren in de Azure Marketplace](hdinsight-apps-publish-applications.md).

## <a name="permissions"></a>Machtigingen

Er zijn twee Apache Ambari-machtigingen die vereist zijn wanneer u scriptacties met het cluster gebruiken voor een domein gekoppeld HDInsight-cluster:

* **AMBARI. VOER\_AANGEPASTE\_OPDRACHT**. De Ambari-beheerdersrol beschikt over deze machtiging standaard.
* **HET CLUSTER. VOER\_AANGEPASTE\_OPDRACHT**. De beheerder van de HDInsight-Cluster en de Ambari-beheerder hebben deze machtiging standaard.

Zie voor meer informatie over het werken met machtigingen hebben in aan domein gekoppelde HDInsight [beheren HDInsight-clusters met Enterprise-beveiligingspakket](./domain-joined/apache-domain-joined-manage.md).

## <a name="access-control"></a>Toegangsbeheer

Als u niet de beheerder of eigenaar van uw Azure-abonnement, uw account hebt ten minste Inzender-toegang tot de resourcegroep waarin het HDInsight-cluster.

Als u een HDInsight-cluster, iemand met minimaal Inzender-toegang tot het Azure-abonnement moet eerder de provider hebben geregistreerd voor HDInsight. Registratie van een provider vindt plaats wanneer een gebruiker met toegang tot het abonnement op het niveau van Inzender, voor het eerst een resource maakt onder het abonnement. Het kan ook worden uitgevoerd zonder het maken van een resource als u [een provider registreren met behulp van REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Haal meer informatie over het werken met toegangsbeheer:

* [Aan de slag met toegangsbeheer in de Azure-portal](../role-based-access-control/overview.md)
* [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md)

## <a name="understand-script-actions"></a>Scriptacties begrijpen

Een scriptactie is Bash-script dat wordt uitgevoerd op de knooppunten in een HDInsight-cluster. Kenmerken en functies van scriptacties zijn als volgt:

* Moeten worden opgeslagen op een URI die toegankelijk is vanaf het HDInsight-cluster. Hieronder volgen mogelijke opslaglocaties:

    * Een Azure Data Lake Storage-account dat toegankelijk is via het HDInsight-cluster. Zie voor meer informatie over het gebruik van Azure Data Lake Storage met HDInsight [Quick Start: clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

        De URI-notatie voor scripts die zijn opgeslagen in Data Lake Storage Gen1 is `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]  
        > De HDInsight gebruikt voor toegang tot Data Lake Storage service-principal moet leestoegang hebben tot het script.

    * Een blob in een Azure Storage-account dat is de primaire of extra storage-account voor het HDInsight-cluster. HDInsight krijgt toegang tot beide van deze typen opslagaccounts tijdens het maken van clusters.

    * Een openbare bestandsdeling service. Voorbeelden zijn Azure-Blob, GitHub, OneDrive en Dropbox.

        Bijvoorbeeld URI's, Zie [voorbeeldscripts script actie](#example-script-action-scripts).

        > [!WARNING]  
        > HDInsight biedt alleen ondersteuning voor Blob in Azure Storage-accounts met een standard-prestatielaag. 

* Als u wilt uitvoeren op alleen bepaalde knooppunttypen kan worden beperkt. Voorbeelden zijn hoofd- of worker-knooppunten.

* Kan worden persistent gemaakt of ad hoc.

    Persistente scripts worden gebruikt voor het aanpassen van de nieuwe werkrolknooppunten toegevoegd aan het cluster via het schalen herverdelen. Een persistent script kan ook wijzigingen aan een ander knooppunt toepassen wanneer vergroten / verkleinen optreden. Een voorbeeld is een hoofdknooppunt.

  > [!IMPORTANT]  
  > Persistente scriptacties moeten een unieke naam hebben.

    Ad-hoc scripts worden niet opgeslagen. Ze worden niet toegepast op de worker-knooppunten die zijn toegevoegd aan het cluster nadat het script is uitgevoerd. Vervolgens kunt u een ad-hoc-script om een persistent script te promoten of degraderen van een persistent script naar een ad-hoc-script.

  > [!IMPORTANT]  
  > Scriptacties gebruikt tijdens het maken van het cluster worden automatisch doorgevoerd.
  >
  > Scripts die niet voldoen aan zijn niet persistent gemaakt, zelfs als u speciaal aan te geven dat ze moeten zijn.

* Parameters die worden gebruikt door het script tijdens het uitvoeren van geaccepteerd.

* Met de bevoegdheden op hoofdniveau uitvoeren op de clusterknooppunten.

* Kan worden gebruikt via Azure portal, Azure PowerShell, de klassieke Azure CLI of de HDInsight .NET SDK.

Het cluster houdt een geschiedenis bij van alle scripts die zijn uitgevoerd. De geschiedenis is nuttig als u moet de ID van een script voor promotie of degradatie bewerkingen vinden.

> [!IMPORTANT]  
> Er is geen automatische manier om de wijzigingen van een scriptactie ongedaan te maken. Een handmatig de wijzigingen ongedaan maken of het bieden van een script dat ze worden omgekeerd.

### <a name="script-action-in-the-cluster-creation-process"></a>Scriptactie tijdens het maken van het cluster

Scriptacties gebruikt tijdens het maken van een cluster zijn enigszins afwijken van scriptacties uitvoeren op een bestaand cluster:

* Het script worden automatisch opgeslagen.

* Een fout in het script kan leiden tot het proces voor het maken van cluster mislukt.

Het volgende diagram illustreert wanneer scriptactie wordt uitgevoerd tijdens het maakproces:

![HDInsight-cluster aanpassen en -fasen tijdens het maken van clusters][img-hdi-cluster-states]

Het script wordt uitgevoerd terwijl HDInsight wordt geconfigureerd. Het script wordt parallel uitgevoerd op de opgegeven knooppunten in het cluster. Deze wordt uitgevoerd met bevoegdheden op hoofdniveau op de knooppunten.

> [!NOTE]  
> U kunt bewerkingen zoals het stoppen en starten van services, met inbegrip van Apache Hadoop-gerelateerde services uitvoeren. Als u services stoppen, Controleer of de Ambari-service en andere Hadoop-gerelateerde services worden uitgevoerd voordat het script is voltooid. Deze services zijn vereist om te bepalen is de status en de status van het cluster terwijl deze wordt gemaakt.


Tijdens het maken van een cluster, kunt u veel scriptacties in één keer gebruiken. Deze scripts worden aangeroepen in de volgorde waarin ze zijn opgegeven.

> [!IMPORTANT]  
> Scriptacties moeten worden voltooid binnen 60 minuten, of dat ze time-out. Het script wordt uitgevoerd tijdens de clusterinrichting, samen met andere processen instellen en configureren. Concurrentie voor resources, zoals CPU-tijd of netwerk bandbreedte kan leiden tot het script duurt langer dan het geval in uw ontwikkelingsomgeving is voltooien.
>
> Vermijd om te beperken van de tijd die nodig zijn om uit te voeren van het script, taken, zoals het downloaden en toepassingen van de bron compileren. Voorcompileren van toepassingen en het binaire bestand opslaan in Azure Storage.


### <a name="script-action-on-a-running-cluster"></a>Scriptactie op een actieve cluster

Een fout in een script uitvoeren op een reeds actief cluster niet automatisch leidt tot het cluster om te wijzigen in een foutstatus. Nadat een script is voltooid, wordt het cluster moet terug naar een status running doorbrengt.

> [!IMPORTANT]  
> Zelfs als het cluster een status running doorbrengt heeft, kan het script is mislukt dingen zijn verbroken. Een script kan bijvoorbeeld bestanden die nodig zijn voor het cluster te verwijderen.
>
> Scripts acties uitvoeren met bevoegdheden op hoofdniveau. Zorg ervoor dat u begrijpt wat een script doet voordat u deze op uw cluster toepassen.

Wanneer u een script op een cluster toepassen, de clusterstatus gewijzigd van **met** naar **geaccepteerde**. Vervolgens wordt gewijzigd in **HDInsight configuratie** en ten slotte terug naar **met** voor geslaagde scripts. De status van het script wordt geregistreerd in de geschiedenis van de scriptactie. Deze informatie geeft aan of het script is geslaagd of mislukt. Bijvoorbeeld, de `Get-AzHDInsightScriptActionHistory` PowerShell-cmdlet toont de status van een script. Er wordt informatie weergegeven die vergelijkbaar is met de volgende tekst:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!IMPORTANT]  
> Als u de clustergebruiker, de beheerder, het wachtwoord wijzigt nadat het cluster is gemaakt, mislukken scriptacties uitvoeren op dit cluster. Als u een persistente scriptacties die doel worker-knooppunten hebt, wordt deze scripts kunnen mislukken als u het cluster schaalt.

## <a name="example-script-action-scripts"></a>Voorbeeldscripts script actie

Actie-script-scripts kunnen worden gebruikt door de volgende hulpprogramma's:

* Azure Portal
* Azure PowerShell
* De klassieke Azure-CLI
* Een HDInsight .NET SDK

HDInsight biedt scripts voor het installeren van de volgende onderdelen in HDInsight-clusters:

| Name | Script |
| --- | --- |
| Een Azure Storage-account toevoegen |`https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh`. Zie [extra opslagaccounts toevoegen aan HDInsight](hdinsight-hadoop-add-storage.md). |
| Hue installeren |`https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh`. Zie [installeren en gebruiken Hue op HDInsight Hadoop-clusters](hdinsight-hadoop-hue-linux.md). |
| Presto installeren |`https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`. Zie [installeren en gebruiken Presto op HDInsight Hadoop-gebaseerde clusters](hdinsight-hadoop-install-presto.md). |
| Giraph installeren |`https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh`. Zie [Apache Giraph installeren op HDInsight Hadoop-clusters](hdinsight-hadoop-giraph-install-linux.md). |
| Hive-bibliotheken vooraf laden |`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`. Zie [aangepaste Apache Hive-bibliotheken toevoegen bij het maken van uw HDInsight-cluster](hdinsight-hadoop-add-hive-libraries.md). |
| Mono installeren of bijwerken | `https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash`. Zie [installeren of bijwerken Mono op HDInsight](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Een scriptactie tijdens het maken van clusters gebruiken

Deze sectie wordt uitgelegd dat de verschillende manieren waarop u kunt scriptacties gebruiken wanneer u een HDInsight-cluster maakt.

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a>Een scriptactie tijdens het maken van de Azure-portal gebruiken

1. Beginnen met het maken van een cluster, zoals beschreven in [clusters instellen in HDInsight met Apache Hadoop, Apache Spark en Apache Kafka](hdinsight-hadoop-provision-linux-clusters.md). Tijdens het maken van een cluster, wordt er uitziet een __samenvatting voor Cluster__ pagina. Uit de __samenvatting voor Cluster__ weergeeft, schakelt de __bewerken__ koppelen voor __geavanceerde instellingen__.

    ![Geavanceerde instellingen koppelen](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. Uit de __geavanceerde instellingen__ sectie, selecteer __scriptacties__. Uit de __scriptacties__ sectie, selecteer __+ nieuwe indienen__.

    ![Een nieuwe scriptactie verzenden](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Gebruik de __selecteert u een script__ vermelding voor een vooraf gemaakte script te selecteren. Selecteer voor het gebruik van een aangepast script __aangepaste__. Geef vervolgens de __naam__ en __Bash-script-URI__ voor het script.

    ![Een script in het formulier selecteren script toevoegen](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    De volgende tabel beschrijft de elementen in het formulier:

    | Eigenschap | Value |
    | --- | --- |
    | Een script selecteren | Selecteer voor het gebruik van uw eigen script __aangepaste__. Anders selecteert u een van de geleverde scripts. |
    | Name |Geef een naam voor de scriptactie. |
    | Bash-script-URI |Hiermee geeft u de URI van het script. |
    | HEAD/Worker/Zookeeper |Geef op de knooppunten waarop het script wordt uitgevoerd: **HEAD**, **Worker**, of **ZooKeeper**. |
    | Parameters |Geef de parameters op, indien vereist door het script. |

    Gebruik de __deze scriptactie__ vermelding om ervoor te zorgen dat het script wordt toegepast tijdens het schalen herverdelen.

5. Selecteer __maken__ om op te slaan van het script. Vervolgens kunt u __+ nieuwe indienen__ om toe te voegen een ander script.

    ![Meerdere scriptacties](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    Wanneer u klaar bent met het toevoegen van scripts, selecteert u de __Selecteer__ knop en vervolgens de __volgende__ terug te keren naar de __samenvatting voor Cluster__ sectie.

3. Voor het maken van het cluster selecteert __maken__ uit de __samenvatting voor Cluster__ selectie.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Een scriptactie van Azure Resource Manager-sjablonen gebruiken

Scriptacties kunnen worden gebruikt met Azure Resource Manager-sjablonen. Zie voor een voorbeeld [HDInsight Linux-Cluster maken en uitvoeren van een scriptactie](https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/).

In dit voorbeeld wordt de scriptactie toegevoegd met behulp van de volgende code:

```json
"scriptActions": [
    {
        "name": "setenvironmentvariable",
        "uri": "[parameters('scriptActionUri')]",
        "parameters": "headnode"
    }
]
```

Meer informatie over het implementeren van een sjabloon ophalen:

* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli)

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Een scriptactie tijdens het maken van Azure PowerShell gebruiken

In deze sectie maakt u de [toevoegen AzHDInsightScriptAction](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightscriptaction) cmdlet voor het aanroepen van scripts voor het aanpassen van een cluster. Voordat u begint, controleert u installeert en configureert u Azure PowerShell. Als u deze PowerShell-opdrachten, moet u de [AZ Module](https://docs.microsoft.com/powershell/azure/overview).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Het volgende script toont hoe u een scriptactie toepassen wanneer u een cluster maken met behulp van PowerShell:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Het kan enkele minuten duren voordat het cluster is gemaakt.

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a>Een scriptactie tijdens het maken van de HDInsight .NET SDK gebruiken

De HDInsight .NET SDK bevat clientbibliotheken waardoor het gemakkelijker om te werken met HDInsight, via een .NET-toepassing. Zie voor een codevoorbeeld, [clusters in HDInsight op basis van Linux maken met behulp van de .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-to-a-running-cluster"></a>Een scriptactie toepassen op een actief cluster

In deze sectie wordt uitgelegd hoe u een actief cluster script maatregelen treffen.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a>Een scriptactie toepassen op een actief cluster vanuit Azure portal

Ga naar de [Azure-portal](https://portal.azure.com):

1. Selecteer in het menu links **alle services**.

1. Onder **ANALYTICS**, selecteer **HDInsight-clusters**.

1. Selecteer uw cluster in de lijst, die wordt geopend de standaardweergave.

1. De standaardweergave onder **instellingen**, selecteer **scriptacties**.

1. Vanaf de bovenkant van de **scriptacties** weergeeft, schakelt **+ nieuwe indienen**.

    ![Een script toevoegen aan een actieve cluster](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Gebruik de __selecteert u een script__ vermelding voor een vooraf gemaakte script te selecteren. Selecteer voor het gebruik van een aangepast script __aangepaste__. Geef vervolgens de __naam__ en __Bash-script-URI__ voor het script.

    ![Een script in het formulier selecteren script toevoegen](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    De volgende tabel beschrijft de elementen in het formulier:

    | Eigenschap | Value |
    | --- | --- |
    | Een script selecteren | Selecteer voor het gebruik van uw eigen script __aangepaste__. Selecteer anders een opgegeven script. |
    | Name |Geef een naam voor de scriptactie. |
    | Bash-script-URI |Hiermee geeft u de URI van het script. |
    | HEAD/Worker/Zookeeper |Geef op de knooppunten waarop het script wordt uitgevoerd: **HEAD**, **Worker**, of **ZooKeeper**. |
    | Parameters |Geef de parameters op, indien vereist door het script. |

    Gebruik de __deze scriptactie__ vermelding die zorg ervoor dat het script wordt toegepast tijdens het schalen herverdelen.

5. Selecteer ten slotte de **maken** knop om toe te passen van het script aan het cluster.

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a>Een scriptactie toepassen op een actief cluster van Azure PowerShell

Als u deze PowerShell-opdrachten, moet u de [AZ Module](https://docs.microsoft.com/powershell/azure/overview).

Het volgende voorbeeld laat zien hoe een scriptactie toepassen op een actief cluster:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

Nadat de bewerking is voltooid, ontvangt u informatie die vergelijkbaar is met de volgende tekst:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a>Een scriptactie toepassen op een actief cluster vanuit de Azure CLI

Voordat u begint, controleert u installeert en configureert u de Azure CLI. Zie voor meer informatie, [de klassieke Azure-CLI installeren](https://docs.microsoft.com/cli/azure/install-classic-cli?view=azure-cli-latest).

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

1. Schakel over naar modus Azure Resource Manager:

    ```bash
    azure config mode arm
    ```

2. Verifiëren met uw Azure-abonnement:

    ```bash
    azure login
    ```

3. Een scriptactie van toepassing op een actief cluster:

    ```bash
    azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>
    ```

    Als u parameters voor deze opdracht niet opgeeft, wordt u gevraagd voor hen. Als het script dat u met opgeeft `-u` accepteert parameters, kunt u opgeven met behulp van de `-p` parameter.

    Geldige typen zijn `headnode`, `workernode`, en `zookeeper`. Als het script moet worden toegepast op verschillende typen, geeft u de typen die zijn gescheiden door een puntkomma `;`. Bijvoorbeeld `-n headnode;workernode`.

    Als u wilt behouden in het script, toevoegen `--persistOnSuccess`. U kunt het script ook later behouden met behulp van `azure hdinsight script-action persisted set`.

    Nadat de taak is voltooid, krijgt u uitvoer zoals de volgende tekst:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-to-a-running-cluster-by-using-rest-api"></a>Een scriptactie toepassen op een actief cluster met behulp van REST-API

Zie [REST-API in Azure HDInsight-Cluster](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a>Een scriptactie toepassen op een actief cluster van de HDInsight .NET SDK

Zie voor een voorbeeld van het gebruik van de .NET SDK om toe te passen van scripts in een cluster, [toepassen van een scriptactie in een actief op Linux gebaseerde HDInsight-cluster](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-and-promote-and-demote-script-actions"></a>Geschiedenis weergeven en verhogen en verlagen van scriptacties

### <a name="the-azure-portal"></a>Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer in het menu links **alle services**.

1. Onder **ANALYTICS**, selecteer **HDInsight-clusters**.

1. Selecteer uw cluster in de lijst, die wordt geopend de standaardweergave.

1. De standaardweergave onder **instellingen**, selecteer **scriptacties**.

4. Een overzicht van scripts voor dit cluster worden weergegeven in de sectie van de acties script. Deze informatie omvat een lijst met persistente scripts. De volgende schermafbeelding ziet u dat het script Solr is uitgevoerd op dit cluster. De schermopname weergegeven niet persistente scripts.

    ![Scriptacties](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. Selecteer een script uit de geschiedenis om weer te geven de **eigenschappen** sectie voor dit script. U kunt vanaf de bovenkant van het scherm, voert u het script opnieuw uit of stel deze.

    ![Scriptacties, eigenschappen](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. U kunt ook selecteren met het weglatingsteken **...** , aan de rechterkant van de items in de sectie script acties acties uit te voeren.

    ![Scriptacties, beletselteken](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="azure-powershell"></a>Azure PowerShell

| cmdlet | Function |
| --- | --- |
| `Get-AzHDInsightPersistedScriptAction` |Informatie over het persistente scriptacties ophalen. |
| `Get-AzHDInsightScriptActionHistory` |Een geschiedenis van scriptacties toegepast op het cluster of de details voor een specifiek script ophalen. |
| `Set-AzHDInsightPersistedScriptAction` |Een ad-hoc scriptactie naar een persistente scriptactie promoten. |
| `Remove-AzHDInsightPersistedScriptAction` |Een persistente scriptactie naar een ad-hoc actie degraderen. |

> [!IMPORTANT]  
> `Remove-AzHDInsightPersistedScriptAction` de acties die worden uitgevoerd door een script niet ongedaan worden gemaakt. Deze cmdlet worden alleen de persistente vlag verwijderd.

Het volgende voorbeeldscript ziet u de cmdlets gebruiken om te promoten en vervolgens het degraderen van een script.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="the-azure-classic-cli"></a>De klassieke Azure-CLI

| cmdlet | Function |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |Een lijst met persistente scriptacties ophalen. |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |De gegevens op een specifieke persistente scriptactie ophalen. |
| `azure hdinsight script-action history list <clustername>` |Een geschiedenis van scriptacties toegepast op het cluster worden opgehaald. |
| `azure hdinsight script-action history show <clustername> <scriptname>` |De gegevens op een specifieke scriptactie ophalen. |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |Een ad-hoc scriptactie naar een persistente scriptactie promoten. |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Een persistente scriptactie naar een ad-hoc actie degraderen. |

> [!IMPORTANT]  
> `azure hdinsight script-action persisted delete` de acties die worden uitgevoerd door een script niet ongedaan worden gemaakt. Deze cmdlet worden alleen de persistente vlag verwijderd.

### <a name="the-hdinsight-net-sdk"></a>De HDInsight .NET SDK

Voor een voorbeeld van het script geschiedenis ophalen uit een cluster met behulp van de .NET SDK, verhogen of verlagen van scripts, raadpleegt [ toepassen van een scriptactie in een actief op Linux gebaseerde HDInsight-cluster](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]  
> In dit voorbeeld laat ook zien over het installeren van een HDInsight-toepassing met behulp van de .NET SDK.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Ondersteuning voor open source-software die wordt gebruikt op HDInsight-clusters

De Microsoft Azure HDInsight-service maakt gebruik van een ecosysteem van open-source-technologieën die zijn gevormd rond Apache Hadoop. Microsoft Azure biedt een algemeen niveau van ondersteuning voor open-source technologieën. Zie voor meer informatie de **reikwijdte van ondersteuning** sectie van [Veelgestelde vragen over Azure-ondersteuning](https://azure.microsoft.com/support/faq/). De HDInsight-service biedt een extra beveiligingsniveau van ondersteuning voor ingebouwde onderdelen.

Er zijn twee typen open source-componenten beschikbaar in de HDInsight-service:

* **Ingebouwde onderdelen**. Deze onderdelen zijn voorgeïnstalleerd op HDInsight-clusters en geef de kernfunctionaliteit van het cluster. Deze categorie deel uitmaken van de volgende onderdelen:

  * [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) ResourceManager.
  * De Hive-query-language [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).
  * [Apache Mahout](https://mahout.apache.org/). 
    
    Een volledige lijst van clusteronderdelen van het is beschikbaar in [wat zijn de Apache Hadoop-onderdelen en versies die beschikbaar met HDInsight?](hdinsight-component-versioning.md)

* **Aangepaste onderdelen**. U kunt installeren of gebruiken in uw werkbelasting een onderdeel is beschikbaar in de community of door u gemaakte als een gebruiker van het cluster.

> [!WARNING]  
> Onderdelen van het HDInsight-cluster worden volledig ondersteund. Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen ontvangen commercieel redelijke ondersteuning zodat u het probleem verder op te lossen. Microsoft Support mogelijk het probleem op te lossen. Of kunnen ze u contact opnemen met beschikbare kanalen voor de open-source-technologieën waar uitgebreide expertise voor deze technologie wordt gevonden vragen. Veel communitysites kunnen worden gebruikt. Voorbeelden zijn [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) en [Stack Overflow](https://stackoverflow.com). 
>
> Apache-projecten ook de project-sites hebben op de [Apache website](https://apache.org). Een voorbeeld is [Hadoop](https://hadoop.apache.org/).

De HDInsight-service biedt verschillende manieren om te gebruiken van aangepaste onderdelen. Hetzelfde niveau van ondersteuning is van toepassing, ongeacht hoe een onderdeel gebruikt of is geïnstalleerd op het cluster. De volgende lijst bevat de meest voorkomende manieren dat aangepaste onderdelen in HDInsight-clusters worden gebruikt:

1. **Taak indienen**. Hadoop- of andere typen taken die worden uitgevoerd of het gebruik van aangepaste onderdelen kunnen worden verzonden naar het cluster.

2. **Clusteraanpassing**. Tijdens het maken van een cluster, kunt u aanvullende instellingen en aangepaste onderdelen die zijn geïnstalleerd op de clusterknooppunten opgeven.

3. **Voorbeelden**. Voor populaire aangepaste onderdelen, kunnen Microsoft en andere voorbeelden van hoe deze onderdelen kunnen worden gebruikt op HDInsight-clusters bieden. Deze voorbeelden worden geleverd zonder ondersteuning.

## <a name="troubleshooting"></a>Problemen oplossen

U kunt de Ambari-Webgebruikersinterface gebruiken om informatie wordt vastgelegd door scriptacties weer te geven. Als het script is mislukt tijdens het maken van een cluster, zijn de logboeken ook beschikbaar in het standaardopslagaccount dat is gekoppeld aan het cluster. Deze sectie bevat informatie over het ophalen van de logboeken met behulp van deze beide opties.

### <a name="the-apache-ambari-web-ui"></a>Apache Ambari-Webinterface

1. Ga in uw browser naar https://CLUSTERNAME.azurehdinsight.net. Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.

    Wanneer u hierom wordt gevraagd, voert u de accountnaam van beheerder **admin**, en het wachtwoord voor het cluster. U moet mogelijk opnieuw in de beheerdersreferenties in een webformulier.

2. Selecteer in de balk boven aan de pagina, de **ops** vermelding. Een lijst geeft de huidige en vorige bewerkingen die op het cluster via Ambari weer.

    ![Ambari web UI balk met ops geselecteerd](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. De items waarvoor vinden **uitvoeren\_customscriptaction** in de **Operations** kolom. Deze vermeldingen worden gemaakt wanneer de script-bewerkingen worden uitgevoerd.

    ![Schermafbeelding van bewerkingen](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    Om weer te geven de **STDOUT** en **STDERR** uitvoer, selecteer de **run\customscriptaction** vermelding en inzoomen op via de koppelingen. Deze uitvoer wordt gegenereerd wanneer het script wordt uitgevoerd en er nuttige informatie.

### <a name="access-logs-from-the-default-storage-account"></a>Toegang tot logboeken van het standaardopslagaccount

Als het maken van een cluster mislukt vanwege een fout in het script, worden de logboeken bewaard in de storage-account van het cluster.

* De opslaglogboeken van de zijn beschikbaar op `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![Schermafbeelding van bewerkingen](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    In deze map de logboeken zijn ingedeeld afzonderlijk voor **hoofdknooppunt**, **worker-knooppunt**, en **zookeeper-knooppunt**. Zie de volgende voorbeelden:

    * **Hoofdknooppunt**: `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Worker-knooppunt**: `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper-knooppunt**: `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Alle **stdout** en **stderr** van de bijbehorende host naar het opslagaccount dat is geüpload. Er is een **uitvoer -\*.txt** en **fouten -\*.txt** voor afzonderlijke scriptacties. De **uitvoer *.txt** bestand bevat informatie over de URI van het script dat is uitgevoerd op de host. De volgende tekst is een voorbeeld van deze informatie:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Het is mogelijk dat u een cluster met script actie herhaaldelijk met dezelfde naam maken. In dat geval kunt u onderscheid maken tussen de relevante logboekbestanden op basis van de **datum** mapnaam. Bijvoorbeeld, de mapstructuur voor een cluster **mijncluster**, gemaakt op een andere datum wordt weergegeven die vergelijkbaar is met de volgende vermeldingen:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Als u een cluster met script actie met dezelfde naam op dezelfde dag maakt, kunt u het unieke voorvoegsel voor het identificeren van de relevante logboekbestanden.

* Als u een cluster in de buurt van 12:00 uur, middernacht, is het mogelijk dat de logboekbestanden moet worden verdeeld over twee dagen. In dat geval ziet u twee verschillende mappen voor hetzelfde cluster.

* Logboekbestanden worden geüpload naar de standaard-container kan tot vijf minuten, met name voor grote clusters duren. Dus als u toegang tot de logboeken wilt, mag niet onmiddellijk verwijdert u het cluster als een scriptactie is mislukt.

### <a name="ambari-watchdog"></a>Ambari watchdog

> [!WARNING]  
> Het wachtwoord voor de Ambari-watchdog, hdinsightwatchdog op uw Linux gebaseerde HDInsight-cluster niet te wijzigen. Het wachtwoord voor dit account wilt wijzigen, verbreekt de mogelijkheid om uit te voeren nieuwe scriptacties op het HDInsight-cluster.

### <a name="cant-import-name-blobservice"></a>Kan de naam BlobService niet importeren.

__Symptomen__. De scriptactie is mislukt. Tekst die vergelijkbaar is met de volgende fout wordt weergegeven wanneer u de bewerking in Ambari bekijkt:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Oorzaak__. Deze fout treedt op als u een upgrade uitvoert van de Python Azure Storage-client die is opgenomen in het HDInsight-cluster. HDInsight wordt verwacht dat de Azure Storage client 0.20.0.

__Resolutie__. U kunt deze fout oplossen door handmatig verbinding maken met elk clusterknooppunt met behulp van `ssh`. Voer de volgende opdracht om de versie van de juiste opslag-client opnieuw te installeren:

```bash
sudo pip install azure-storage==0.20.0
```

Zie voor meer informatie over verbinding maken met het cluster met SSH [verbinding maken met HDInsight (Apache Hadoop) met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-the-scripts-used-during-cluster-creation"></a>Geschiedenis van wordt niet de scripts die worden gebruikt tijdens het maken van een cluster weergegeven

Als uw cluster is gemaakt vóór 15 maart 2016, ziet u mogelijk niet een vermelding in de geschiedenis van de scriptactie. Het cluster vergroten of verkleinen zorgt ervoor dat de scripts die worden weergegeven in de geschiedenis van de scriptactie.

Er zijn twee uitzonderingen:

* Uw cluster is gemaakt vóór 1 September 2015. Deze datum wordt als scriptacties werden geïntroduceerd. Een cluster die zijn gemaakt vóór deze datum kan niet worden scriptacties hebt gebruikt voor het maken van clusters.

* U hebt meerdere scriptacties tijdens het maken van een cluster gebruikt. Of u dezelfde naam voor meerdere scripts of de dezelfde naam, dezelfde URI, maar verschillende parameters voor meerdere scripts gebruikt. In dergelijke gevallen krijgt u de volgende fout:

    Er zijn geen nieuwe scriptacties kunnen op dit cluster worden uitgevoerd vanwege conflicterende scriptnamen in bestaande scripts. Scriptnamen die zijn opgegeven bij het maken van clusters moeten alle uniek zijn. Bestaande scripts worden uitgevoerd op grootte.

## <a name="next-steps"></a>Volgende stappen

* [Actie-script-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions-linux.md)
* [Installeren en Apache Giraph gebruikt op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md)
* [Extra opslag toevoegen aan een HDInsight-cluster](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fasen tijdens het maken van clusters"
