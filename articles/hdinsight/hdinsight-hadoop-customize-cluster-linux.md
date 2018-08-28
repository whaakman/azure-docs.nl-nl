---
title: HDInsight clusters aanpassen met scriptacties - Azure
description: Aangepaste onderdelen toevoegen aan HDInsight op basis van Linux-clusters met behulp van scriptacties. Scriptacties zijn Bash-scripts die kunnen worden gebruikt voor het aanpassen van de configuratie van het cluster of toevoegen van extra services en hulpprogramma's, zoals Hue, Solr of R.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: jasonh
ms.openlocfilehash: e12379dbb63e57ee12e1cebb1761f0b103d50ef1
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43110524"
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-actions"></a>HDInsight Linux gebaseerde clusters aanpassen met scriptacties

HDInsight biedt een configuratiemethode aangeroepen **scriptacties** die aangepaste scripts om aan te passen van het cluster aanroept. Deze scripts worden gebruikt om extra onderdelen installeren en configuratie-instellingen wijzigen. Scriptacties kunnen worden gebruikt tijdens of na het maken van clusters.

> [!IMPORTANT]
> De mogelijkheid scriptacties gebruiken op een reeds actief cluster is alleen beschikbaar voor Linux gebaseerde HDInsight-clusters.
>
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

Scriptacties kunnen ook in de Azure Marketplace worden gepubliceerd als een HDInsight-toepassing. Zie voor meer informatie over HDInsight-toepassingen, [publiceren HDInsight-toepassingen in Azure Marketplace](hdinsight-apps-publish-applications.md).

## <a name="permissions"></a>Machtigingen

Als u van een domein gekoppeld HDInsight-cluster gebruikmaakt, zijn er twee Ambari-machtigingen die vereist zijn wanneer u met behulp van scriptacties met het cluster:

* **AMBARI. Voer\_aangepaste\_opdracht**: de Ambari-beheerdersrol beschikt over deze machtiging standaard.
* **HET CLUSTER. Voer\_aangepaste\_opdracht**: zowel de HDInsight-Clusterbeheer en Ambari beheerder hebben deze machtiging standaard.

Zie voor meer informatie over het werken met machtigingen hebben in aan domein gekoppelde HDInsight [aan domein gekoppelde HDInsight-clusters beheren](./domain-joined/apache-domain-joined-manage.md).

## <a name="access-control"></a>Toegangsbeheer

Als u niet de beheerder/eigenaar van uw Azure-abonnement, uw account hebt ten minste **Inzender** toegang tot de resourcegroep waarin het HDInsight-cluster.

Bovendien, als u een HDInsight-cluster, iemand met ten minste maakt **Inzender** toegang tot het Azure-abonnement moet eerder de provider hebben geregistreerd voor HDInsight. Registratie van een provider vindt plaats wanneer een gebruiker met toegang tot het abonnement op het niveau van Inzender, voor het eerst een resource maakt onder het abonnement. Dit kan ook worden bereikt zonder dat er een resource wordt gemaakt door [een provider te registreren met behulp van REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Zie de volgende documenten voor meer informatie over werken met toegangsbeheer:

* [Aan de slag met toegangsbeheer in Azure Portal](../role-based-access-control/overview.md)
* [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md)

## <a name="understanding-script-actions"></a>Informatie over scriptacties

Een scriptactie is Bash-script dat wordt uitgevoerd op de knooppunten in een HDInsight-cluster. Hier volgen opmaakeigenschappen en -functies van scriptacties.

* Moeten worden opgeslagen op een URI die toegankelijk is vanaf het HDInsight-cluster. Hieronder volgen mogelijke opslaglocaties:

    * Een **Azure Data Lake Store** account dat toegankelijk is via het HDInsight-cluster. Zie voor meer informatie over het gebruik van Azure Data Lake Store met HDInsight [Quick Start: clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

        Wanneer u een script dat is opgeslagen in Data Lake Store, de indeling van de URI is `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]
        > De service-principal die HDInsight gebruikt voor toegang tot Data Lake Store moet leestoegang hebben tot het script.

    * Een blob in een **Azure Storage-account** dat wil zeggen een van beide de primaire of extra storage-account voor het HDInsight-cluster. HDInsight krijgt toegang tot beide van deze typen opslagaccounts tijdens het maken van clusters.

    * Een bestand met openbare sharing-service, zoals Azure Blob, GitHub, OneDrive, Dropbox, enzovoort.

        Bijvoorbeeld URI's, Zie de [voorbeeldscripts script actie](#example-script-action-scripts) sectie.

        > [!WARNING]
        > HDInsight biedt alleen ondersteuning voor __voor algemeen gebruik__ Azure Storage-accounts. Het biedt momenteel geen ondersteuning het __Blob storage__ accounttype.

* Kan worden beperkt tot **uitvoeren op alleen bepaalde typen**voor voorbeeld van de hoofdknooppunten of worker-knooppunten.

* Kan **persistent** of **ad hoc**.

    **Vastgelegde** scripts worden gebruikt voor het aanpassen van de nieuwe werkrolknooppunten toegevoegd aan het cluster via het schalen herverdelen. Een persistent script kan ook wijzigingen toepassen op een ander knooppunttype, zoals een hoofdknooppunt, wanneer vergroten / verkleinen optreden.

  > [!IMPORTANT]
  > Persistente scriptacties moeten een unieke naam hebben.

    **Ad hoc** scripts niet permanent worden opgeslagen. Ze worden niet toegepast op worker-knooppunten die zijn toegevoegd aan het cluster nadat het script is uitgevoerd. U kunt vervolgens een ad-hoc-script om een persistent script te promoten of degraderen van een persistent script naar een ad-hoc-script.

  > [!IMPORTANT]
  > Scriptacties gebruikt tijdens het maken van het cluster worden automatisch doorgevoerd.
  >
  > Scripts die mislukt zijn niet opgeslagen, zelfs als u speciaal aan te geven dat ze moeten zijn.

* Kan accepteren **parameters** die worden gebruikt door het script tijdens de uitvoering.

* Uitvoeren met **hoofdmap bevoegdheden** op de clusterknooppunten.

* Kan worden gebruikt via de **Azure-portal**, **Azure PowerShell**, **Azure CLI v1.0**, of **HDInsight .NET SDK**

Het cluster houdt een geschiedenis bij van alle scripts die zijn is uitgevoerd. De geschiedenis is handig wanneer u moet de ID van een script voor promotie of degradatie bewerkingen vinden.

> [!IMPORTANT]
> Er is geen automatische manier om de wijzigingen van een scriptactie ongedaan te maken. Een handmatig de wijzigingen ongedaan maken of het bieden van een script dat ze worden omgekeerd.

### <a name="script-action-in-the-cluster-creation-process"></a>Scriptactie tijdens het maken van het cluster

Scriptacties gebruikt tijdens het maken van een cluster zijn enigszins afwijken van de acties die worden uitgevoerd op een bestaand cluster script:

* Het script is **automatisch persistente**.

* Een **fout** in het script kan ertoe leiden dat het proces voor het maken van cluster mislukt.

Het volgende diagram illustreert wanneer scriptactie wordt uitgevoerd tijdens het maakproces:

![HDInsight-cluster aanpassen en -fasen tijdens het maken van clusters][img-hdi-cluster-states]

Het script wordt uitgevoerd terwijl HDInsight wordt geconfigureerd. Het script wordt parallel uitgevoerd op de opgegeven knooppunten in het cluster, en met bevoegdheden op hoofdniveau wordt uitgevoerd op de knooppunten.

> [!NOTE]
> U kunt bewerkingen zoals het stoppen en starten van services, waaronder Hadoop-gerelateerde services uitvoeren. Als u services stoppen, moet u ervoor zorgen dat de Ambari-service en andere Hadoop-gerelateerde services uitgevoerd voordat het script is voltooid. Deze services zijn vereist om te bepalen is de status en de status van het cluster terwijl deze wordt gemaakt.


Tijdens het maken van een cluster, kunt u meerdere scriptacties in één keer gebruiken. Deze scripts worden aangeroepen in de volgorde waarin ze zijn opgegeven.

> [!IMPORTANT]
> Scriptacties moeten binnen 60 minuten, of een time-out voltooien. Het script wordt uitgevoerd tijdens de clusterinrichting, samen met andere processen instellen en configureren. Concurrentie voor resources, zoals CPU-tijd of netwerk bandbreedte kan ertoe leiden dat het script duurt langer dan het geval in uw ontwikkelingsomgeving is voltooien.
>
> Om te beperken van de tijd die het duurt voordat het script uit te voeren, te voorkomen dat taken zoals het downloaden en toepassingen van bron compileren. Vooraf compileren toepassingen en het binaire bestand opslaan in Azure Storage.


### <a name="script-action-on-a-running-cluster"></a>Scriptactie op een actieve cluster

Een fout in een script uitgevoerd op een reeds actief cluster automatisch niet leiden tot het cluster om te wijzigen in een foutstatus. Nadat een script is voltooid, moet het cluster naar een status 'running' retourneren.

> [!IMPORTANT]
> Zelfs als het cluster heeft een status 'running', kan het script is mislukt dingen zijn verbroken. Een script kan bijvoorbeeld bestanden die nodig zijn voor het cluster te verwijderen.
>
> Scripts acties uitvoeren met bevoegdheden op hoofdniveau. Zorg ervoor dat u wat een script begrijpt voordat u deze toepast op uw cluster doet.

Bij het toepassen van een script naar een cluster, wordt de clusterstatus verandert van **met** naar **geaccepteerde**, klikt u vervolgens **HDInsight-configuratie**, en ten slotte terug naar  **Met** voor geslaagde scripts. De scriptstatus wordt geregistreerd in de geschiedenis van de scriptactie en u kunt deze informatie gebruiken om te bepalen of het script is geslaagd of mislukt. Bijvoorbeeld, de `Get-AzureRmHDInsightScriptActionHistory` PowerShell-cmdlet kan worden gebruikt om de status van een script weer te geven. Er wordt informatie weergegeven die vergelijkbaar is met de volgende tekst:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!IMPORTANT]
> Als u kunt het wachtwoord van de cluster-gebruiker (beheerder) zijn gewijzigd nadat het cluster is gemaakt, mislukken script acties uitgevoerd op dit cluster. Als u een persistente scriptacties die doel worker-knooppunten hebt, mislukken deze scripts wanneer u het cluster schaalt.

## <a name="example-script-action-scripts"></a>Voorbeeldscripts script actie

Actie-script-scripts kunnen worden gebruikt door de volgende hulpprogramma's:

* Azure Portal
* Azure PowerShell
* Azure CLI v1.0
* HDInsight .NET-SDK

HDInsight biedt scripts voor het installeren van de volgende onderdelen in HDInsight-clusters:

| Naam | Script |
| --- | --- |
| **Een Azure Storage-account toevoegen** |https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh. Zie [extra opslag toevoegen aan een HDInsight-cluster](hdinsight-hadoop-add-storage.md). |
| **Hue installeren** |https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Zie [installeren en gebruiken Hue op HDInsight-clusters](hdinsight-hadoop-hue-linux.md). |
| **Presto installeren** |https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh. Zie [installeren en gebruiken Presto op HDInsight-clusters](hdinsight-hadoop-install-presto.md). |
| **Solr installeren** |https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh. Zie [installeren en gebruiken Solr op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md). |
| **Giraph installeren** |https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh. Zie [installeren en gebruiken Giraph op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md). |
| **Hive-bibliotheken vooraf laden** |https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh. Zie [toevoegen Hive-bibliotheken op HDInsight-clusters](hdinsight-hadoop-add-hive-libraries.md). |
| **Mono installeren of bijwerken** | https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash. Zie [installeren of bijwerken Mono op HDInsight](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Een scriptactie tijdens het maken van clusters gebruiken

Deze sectie bevat voorbeelden over de verschillende manieren waarop die u scriptacties gebruiken kunt bij het maken van een HDInsight-cluster.

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a>Een scriptactie tijdens het maken van de Azure-portal gebruiken

1. Beginnen met het maken van een cluster, zoals beschreven op [Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Tijdens het maken van een cluster, wordt er uitziet een __samenvatting voor Cluster__ pagina. Uit de __samenvatting voor Cluster__ weergeeft, schakelt de __bewerken__ koppelen voor __geavanceerde instellingen__.

    ![Geavanceerde instellingen koppelen](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. Uit de __geavanceerde instellingen__ sectie, selecteer __scriptacties__. Uit de __scriptacties__ sectie, selecteer __+ nieuwe verzenden__

    ![Een nieuwe scriptactie verzenden](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Gebruik de __selecteert u een script__ vermelding voor een vooraf gemaakte script te selecteren. Selecteer voor het gebruik van een aangepast script __aangepaste__ en geef vervolgens de __naam__ en __Bash-script-URI__ voor het script.

    ![Een script in het formulier selecteren script toevoegen](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    De volgende tabel beschrijft de elementen in het formulier:

    | Eigenschap | Waarde |
    | --- | --- |
    | Een script selecteren | Selecteer voor het gebruik van uw eigen script __aangepaste__. Anders selecteert u een van de geleverde scripts. |
    | Naam |Geef een naam voor de scriptactie. |
    | Bash-script-URI |Hiermee geeft u de URI van het script. |
    | HEAD/Worker/Zookeeper |Geef de knooppunten (**Head**, **Worker**, of **ZooKeeper**) op waarmee het script is uitgevoerd. |
    | Parameters |Geef de parameters op, indien vereist door het script. |

    Gebruik de __deze scriptactie__ vermelding om ervoor te zorgen dat het script wordt toegepast tijdens het schalen herverdelen.

5. Selecteer __maken__ om op te slaan van het script. Vervolgens kunt u __+ nieuwe indienen__ om toe te voegen een ander script.

    ![Meerdere scriptacties](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    Wanneer u klaar bent met het toevoegen van scripts, gebruikt u de __Selecteer__ knop, en vervolgens de __volgende__ terug te keren naar de __samenvatting voor Cluster__ sectie.

3. Voor het maken van het cluster selecteert __maken__ uit de __samenvatting voor Cluster__ selectie.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Een scriptactie van Azure Resource Manager-sjablonen gebruiken

Scriptacties kunnen worden gebruikt met Azure Resource Manager-sjablonen. Zie voor een voorbeeld [ https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/ ](https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/).

In dit voorbeeld wordt de scriptactie toegevoegd met de volgende code:

```json
"scriptActions": [
    {
        "name": "setenvironmentvariable",
        "uri": "[parameters('scriptActionUri')]",
        "parameters": "headnode"
    }
]
```

Zie de volgende documenten voor informatie over het implementeren van een sjabloon:

* [Resources implementeren met sjablonen en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

* [Resources implementeren met sjablonen en Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli)

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Een scriptactie tijdens het maken van Azure PowerShell gebruiken

In deze sectie maakt u de [toevoegen AzureRmHDInsightScriptAction](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/add-azurermhdinsightscriptaction) cmdlet voor het aanroepen van scripts voor het aanpassen van een cluster. Voordat u doorgaat, zorg ervoor dat u hebt geïnstalleerd en geconfigureerd, Azure PowerShell. Zie voor meer informatie over het configureren van een werkstation voor het uitvoeren van HDInsight PowerShell-cmdlets [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

Het volgende script laat zien hoe u een scriptactie toegepast bij het maken van een cluster met behulp van PowerShell:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Het kan enkele minuten duren voordat het cluster is gemaakt.

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a>Een scriptactie tijdens het maken van de HDInsight .NET SDK gebruiken

De HDInsight .NET SDK bevat clientbibliotheken die het makkelijker is om te werken met HDInsight, via een .NET-toepassing maakt. Zie voor een codevoorbeeld, [maken Linux gebaseerde clusters in HDInsight met behulp van de .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-to-a-running-cluster"></a>Een scriptactie toepassen op een actief cluster

In deze sectie leert u hoe u scriptacties toepassen op een actief cluster.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a>Een scriptactie toepassen op een actief cluster vanuit Azure portal

1. Uit de [Azure-portal](https://portal.azure.com), selecteer uw HDInsight-cluster.

2. Selecteer in het overzicht van de HDInsight-cluster de **scriptacties** tegel.

    ![Script acties tegel](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > U kunt ook selecteren **alle instellingen** en selecteer vervolgens **scriptacties** uit het gedeelte instellingen.

3. Vanaf de bovenkant van het script Actiesectie, selecteer **indienen nieuwe**.

    ![Een script toevoegen aan een actieve cluster](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Gebruik de __selecteert u een script__ vermelding voor een vooraf gemaakte script te selecteren. Selecteer voor het gebruik van een aangepast script __aangepaste__ en geef vervolgens de __naam__ en __Bash-script-URI__ voor het script.

    ![Een script in het formulier selecteren script toevoegen](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    De volgende tabel beschrijft de elementen in het formulier:

    | Eigenschap | Waarde |
    | --- | --- |
    | Een script selecteren | Selecteer voor het gebruik van uw eigen script __aangepaste__. Selecteer anders een opgegeven script. |
    | Naam |Geef een naam voor de scriptactie. |
    | Bash-script-URI |Hiermee geeft u de URI van het script. |
    | HEAD/Worker/Zookeeper |Geef de knooppunten (**Head**, **Worker**, of **ZooKeeper**) op waarmee het script is uitgevoerd. |
    | Parameters |Geef de parameters op, indien vereist door het script. |

    Gebruik de __deze scriptactie__ vermelding die zorg ervoor dat het script wordt toegepast tijdens het schalen herverdelen.

5. Gebruik tot slot de **maken** knop om toe te passen van het script aan het cluster.

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a>Een scriptactie toepassen op een actief cluster van Azure PowerShell

Voordat u doorgaat, zorg ervoor dat u hebt geïnstalleerd en geconfigureerd, Azure PowerShell. Zie voor meer informatie over het configureren van een werkstation voor het uitvoeren van HDInsight PowerShell-cmdlets [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

Het volgende voorbeeld ziet u hoe u een scriptactie van toepassing op een actief cluster:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

Nadat de bewerking is voltooid, ontvangt u informatie die vergelijkbaar is met de volgende tekst:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a>Een scriptactie toepassen op een actief cluster vanuit de Azure CLI

Voordat u doorgaat, zorg ervoor dat u hebt geïnstalleerd en de Azure CLI geconfigureerd. Zie voor meer informatie, [installeren van de Azure CLI 1.0](../cli-install-nodejs.md).

> [!IMPORTANT]
> HDInsight is vereist voor Azure CLI 1.0. Momenteel biedt Azure CLI 2.0 geen opdrachten voor het werken met HDInsight.

1. Als u wilt overschakelen naar de Azure Resource Manager-modus, gebruikt u de volgende opdracht uit op de opdrachtregel:

    ```bash
    azure config mode arm
    ```

2. Gebruik de volgende voor de verificatie bij uw Azure-abonnement.

    ```bash
    azure login
    ```

3. Gebruik de volgende opdracht om toe te passen van een scriptactie naar een actieve cluster

    ```bash
    azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>
    ```

    Als u parameters voor deze opdracht niet opgeeft, wordt u gevraagd voor hen. Als het script dat u met opgeeft `-u` accepteert parameters, kunt u opgeven met behulp van de `-p` parameter.

    Geldige typen zijn `headnode`, `workernode`, en `zookeeper`. Als het script moet worden toegepast op meerdere typen, geeft u de typen van elkaar gescheiden door een ';'. Bijvoorbeeld `-n headnode;workernode`.

    Als u wilt behouden in het script, toevoegen de `--persistOnSuccess`. U kunt het script ook later behouden met behulp van `azure hdinsight script-action persisted set`.

    Nadat de taak is voltooid, ontvangt u uitvoer die vergelijkbaar is met de volgende tekst:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-to-a-running-cluster-using-rest-api"></a>Een scriptactie toepassen op een actieve cluster met behulp van REST-API

Zie [scriptacties uitvoeren op een actieve cluster](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a>Een scriptactie toepassen op een actief cluster van de HDInsight .NET SDK

Zie voor een voorbeeld van het gebruik van de .NET SDK om toe te passen van scripts in een cluster, [ https://github.com/Azure-Samples/hdinsight-dotnet-script-action ](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-promote-and-demote-script-actions"></a>Geschiedenis weergeven, verhogen en verlagen van scriptacties

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken

1. Uit de [Azure-portal](https://portal.azure.com), selecteer uw HDInsight-cluster.

2. Selecteer in het overzicht van de HDInsight-cluster de **scriptacties** tegel.

    ![Script acties tegel](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > U kunt ook selecteren **alle instellingen** en selecteer vervolgens **scriptacties** uit het gedeelte instellingen.

4. Een overzicht van scripts voor dit cluster wordt weergegeven in de sectie script acties. Deze informatie omvat een lijst met persistente scripts. In de onderstaande schermafbeelding ziet u dat de Solr script is uitgevoerd op dit cluster. Persistente scripts niet wordt weergegeven in de schermafbeelding.

    ![De sectie Acties script](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. Een script selecteren in de geschiedenis, geeft de sectie met eigenschappen voor dit script weer. U kunt vanaf de bovenkant van het scherm, voert u het script opnieuw uit of stel deze.

    ![Eigenschappen van de script-acties](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. U kunt ook de **...**  aan de rechterkant van de items in de sectie script acties acties uit te voeren.

    ![Scriptacties... Gebruik](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a>Azure PowerShell gebruiken

| Gebruik de volgende... | Aan... |
| --- | --- |
| Get-AzureRmHDInsightPersistedScriptAction |Ophalen van informatie over het persistente scriptacties |
| Get-AzureRmHDInsightScriptActionHistory |Een geschiedenis van scriptacties toegepast op het cluster, of de details voor een specifiek script ophalen |
| Set-AzureRmHDInsightPersistedScriptAction |De energiestatus van een ad-hoc scriptactie naar een persistente scriptactie |
| Remove-AzureRmHDInsightPersistedScriptAction |Een persistente scriptactie naar een ad-hoc actie verlagen |

> [!IMPORTANT]
> Met behulp van `Remove-AzureRmHDInsightPersistedScriptAction` die de acties die worden uitgevoerd door een script wordt niet ongedaan gemaakt. Deze cmdlet worden alleen de persistente vlag verwijderd.

Het volgende voorbeeldscript ziet u de cmdlets gebruiken om te promoten en degraderen van een script.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-the-azure-cli"></a>Azure CLI gebruiken

| Gebruik de volgende... | Aan... |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |Een lijst met persistente scriptacties ophalen |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |Ophalen van gegevens op een specifieke persistente scriptactie |
| `azure hdinsight script-action history list <clustername>` |Een geschiedenis van scriptacties toegepast op het cluster ophalen |
| `azure hdinsight script-action history show <clustername> <scriptname>` |Ophalen van gegevens op een specifieke scriptactie |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |De energiestatus van een ad-hoc scriptactie naar een persistente scriptactie |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Een persistente scriptactie naar een ad-hoc actie verlagen |

> [!IMPORTANT]
> Met behulp van `azure hdinsight script-action persisted delete` die de acties die worden uitgevoerd door een script wordt niet ongedaan gemaakt. Deze cmdlet worden alleen de persistente vlag verwijderd.

### <a name="using-the-hdinsight-net-sdk"></a>Met behulp van de HDInsight .NET SDK

Voor een voorbeeld van het script geschiedenis ophalen uit een cluster met behulp van de .NET SDK, verhogen of verlagen van scripts, raadpleegt [ https://github.com/Azure-Samples/hdinsight-dotnet-script-action ](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]
> In dit voorbeeld laat ook zien hoe u een HDInsight-toepassing met de .NET SDK installeren.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Ondersteuning voor open source-software die wordt gebruikt op HDInsight-clusters

De Microsoft Azure HDInsight-service maakt gebruik van een ecosysteem van open-source-technologieën die zijn gevormd rond Hadoop. Microsoft Azure biedt een algemeen niveau van ondersteuning voor open-source technologieën. Zie voor meer informatie de **reikwijdte van ondersteuning** sectie van de [ondersteuning Veelgestelde vragen over Azure-website](https://azure.microsoft.com/support/faq/). De HDInsight-service biedt een extra beveiligingsniveau van ondersteuning voor ingebouwde onderdelen.

Er zijn twee typen van open source-componenten die beschikbaar in de HDInsight-service zijn:

* **Ingebouwde onderdelen** -deze onderdelen vooraf zijn geïnstalleerd op HDInsight-clusters en bieden de kernfunctionaliteit van het cluster. Bijvoorbeeld, behoren YARN ResourceManager, de Hive-query-taal (HiveQL) en de Mahout-bibliotheek tot deze categorie. Een volledige lijst van clusteronderdelen van het is beschikbaar in [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight](hdinsight-component-versioning.md).
* **Aangepaste onderdelen** -u, als een gebruiker van het cluster kunt installeren of gebruiken in uw werkbelasting een onderdeel is beschikbaar in de community of door u gemaakte.

> [!WARNING]
> Onderdelen van het HDInsight-cluster worden volledig ondersteund. Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Microsoft-ondersteuning kan mogelijk zijn om het probleem te verhelpen of ze kunnen vragen, moet u contact opnemen met beschikbare kanalen voor de open source-technologieën waar uitgebreide expertise voor deze technologie kan worden gevonden. Bijvoorbeeld, er zijn veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ http://apache.org ](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).

De HDInsight-service biedt verschillende manieren om te gebruiken van aangepaste onderdelen. Hetzelfde niveau van ondersteuning is van toepassing, ongeacht hoe een onderdeel gebruikt of is geïnstalleerd op het cluster. De volgende lijst bevat de meest voorkomende manieren dat aangepaste onderdelen in HDInsight-clusters kunnen worden gebruikt:

1. Taken verzenden - Hadoop- of andere typen taken die worden uitgevoerd of het gebruik van aangepaste onderdelen kan worden verzonden naar het cluster.

2. Aanpassing van de cluster - tijdens het maken van een cluster, kunt u aanvullende instellingen en aangepaste onderdelen die zijn geïnstalleerd op de clusterknooppunten opgeven.

3. Voorbeelden van-voor populaire aangepaste onderdelen, Microsoft en andere mogelijk voorbeelden van hoe deze onderdelen kunnen worden gebruikt in de HDInsight-clusters bieden. Deze voorbeelden worden geleverd zonder ondersteuning.

## <a name="troubleshooting"></a>Problemen oplossen

Ambari-Webgebruikersinterface kunt u weergeven van informatie die wordt geregistreerd door scriptacties. Als het script is mislukt tijdens het maken van een cluster, zijn de logboeken ook beschikbaar in het standaardopslagaccount dat is gekoppeld aan het cluster. Deze sectie bevat informatie over het ophalen van de logboeken met behulp van deze beide opties.

### <a name="using-the-ambari-web-ui"></a>Met behulp van de Ambari-Webgebruikersinterface

1. Ga in de browser naar https://CLUSTERNAME.azurehdinsight.net. CLUSTERNAME vervangen door de naam van uw HDInsight-cluster.

    Wanneer u hierom wordt gevraagd, typt u de accountnaam van beheerder (beheerder) en het wachtwoord voor het cluster. U moet de beheerdersreferenties in een webformulier opnieuw invoeren.

2. Selecteer in de balk boven aan de pagina, de **ops** vermelding. Een lijst met huidige en vorige bewerkingen die worden uitgevoerd op het cluster via Ambari wordt weergegeven.

    ![Ambari web UI balk met ops geselecteerd](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. De items waarvoor vinden **uitvoeren\_customscriptaction** in de **Operations** kolom. Deze vermeldingen worden gemaakt wanneer de script-bewerkingen worden uitgevoerd.

    ![Schermafbeelding van bewerkingen](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    Als u de uitvoer STDOUT en STDERR, selecteer de vermelding run\customscriptaction en inzoomen op de koppelingen. Deze uitvoer wordt gegenereerd wanneer het script wordt uitgevoerd, en nuttige informatie kan bevatten.

### <a name="access-logs-from-the-default-storage-account"></a>Toegang tot logboeken van het standaardopslagaccount

Als het maken van een cluster mislukt vanwege een fout in het script, worden de logboeken bewaard in de storage-account van het cluster.

* De opslaglogboeken van de zijn beschikbaar op `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![Schermafbeelding van bewerkingen](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    De logboeken zijn in deze map afzonderlijk ingedeeld voor het hoofdknooppunt workernode en zookeeper-knooppunten. Een aantal voorbeelden:

    * **Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Worker-knooppunt** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper-knooppunt** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Alle stdout en stderr van de bijbehorende host is geüpload naar het opslagaccount. Er is een **uitvoer -\*.txt** en **fouten -\*.txt** voor afzonderlijke scriptacties. De uitvoer-txt-bestand bevat informatie over de URI van het script dat is uitgevoerd op de host. De volgende tekst is een voorbeeld van deze informatie:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Het is mogelijk dat u een cluster met script actie herhaaldelijk met dezelfde naam maken. In dit geval is, kunt u de relevante logboekbestanden op basis van de naam van de datum-map onderscheiden. Bijvoorbeeld weergegeven, de mapstructuur voor een cluster (mijncluster) gemaakt op datums verschillende die vergelijkbaar is met de volgende vermeldingen:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Als u een cluster met script actie met dezelfde naam op dezelfde dag maakt, kunt u het unieke voorvoegsel voor het identificeren van de relevante logboekbestanden.

* Als u een cluster in de buurt van 12:00 uur (middernacht) maakt, is het mogelijk dat de logboekbestanden moet worden verdeeld over twee dagen. In dergelijke gevallen ziet u twee verschillende mappen voor hetzelfde cluster.

* Logboekbestanden worden geüpload naar de standaardcontainer duurt maximaal 5 minuten, met name voor grote clusters. Dus als u toegang tot de logboeken wilt, verwijdert niet onmiddellijk u het cluster als een scriptactie is mislukt.

### <a name="ambari-watchdog"></a>Ambari watchdog

> [!WARNING]
> Het wachtwoord niet wijzigen voor de Ambari-Watchdog (hdinsightwatchdog) op uw Linux gebaseerde HDInsight-cluster. Het wachtwoord voor dit account wilt wijzigen, verbreekt de mogelijkheid om uit te voeren nieuwe scriptacties op het HDInsight-cluster.

### <a name="cant-import-name-blobservice"></a>Kan de naam BlobService niet importeren.

__Symptomen__: het script actie mislukt. Tekst die vergelijkbaar is met de volgende fout wordt weergegeven wanneer u de bewerking in Ambari weergeven:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Oorzaak__: deze fout treedt op als u een upgrade uitvoert van de Python Azure Storage-client die is opgenomen in het HDInsight-cluster. HDInsight wordt verwacht dat de Azure Storage client 0.20.0.

__Resolutie__: U kunt deze fout oplossen door handmatig verbinding maken met elke cluster-knooppunt met `ssh` en gebruik de volgende opdracht om de versie van de juiste opslag-client opnieuw te installeren:

```bash
sudo pip install azure-storage==0.20.0
```

Zie voor meer informatie over verbinding maken met het cluster met SSH [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a>Geschiedenis van wordt niet scripts die worden gebruikt tijdens het maken van een cluster weergegeven

Als uw cluster is gemaakt vóór 15 maart 2016, ziet u niet een vermelding in de geschiedenis van de scriptactie. Het cluster vergroten of verkleinen zorgt ervoor dat de scripts die worden weergegeven in de geschiedenis van de scriptactie.

Er zijn twee uitzonderingen:

* Als uw cluster is gemaakt vóór 1 September 2015. Deze datum wordt als scriptacties werden geïntroduceerd. Een cluster die zijn gemaakt vóór deze datum kan niet hebt gebruikt scriptacties voor het maken van clusters.

* Als u meerdere scriptacties tijdens het maken van het cluster gebruikt en gebruikt dezelfde naam voor meerdere scripts of de dezelfde naam, dezelfde URI, maar verschillende parameters voor meerdere scripts. In dergelijke gevallen moet u het volgende foutbericht krijgt:

    Er is geen nieuwe script acties kunnen worden uitgevoerd op dit cluster vanwege conflicterende scriptnamen in bestaande scripts. Scriptnamen van het op de cluster maken moet alle uniek zijn. Bestaande scripts worden op grootte uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

* [Actie-script-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions-linux.md)
* [Solr installeren en gebruiken op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md)
* [Giraph installeren en gebruiken op HDInsight-clusters](hdinsight-hadoop-giraph-install-linux.md)
* [Extra opslag toevoegen aan een HDInsight-cluster](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fasen tijdens het maken van clusters"
