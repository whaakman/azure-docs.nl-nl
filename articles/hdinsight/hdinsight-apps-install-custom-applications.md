---
title: Uw eigen aangepaste Hadoop-toepassingen installeren op HDInsight | Microsoft Docs
description: Informatie over het installeren van HDInsight-toepassingen op HDInsight-toepassingen.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e556b29c-8176-4bc5-a90b-aa01abfd3aee
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/03/2017
ms.author: jgao
ms.openlocfilehash: 3bf8ff367fcf88c76c2dc711a6daeffdcb3258b7
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="install-custom-hadoop-applications-on-azure-hdinsight"></a>Aangepaste Hadoop-toepassingen installeren op Azure HDInsight

In dit artikel krijgt u informatie over het installeren van een Hadoop-toepassing op Azure HDInsight die niet is gepubliceerd in Azure Portal. De toepassing die u in dit artikel gaat installeren, heet [Hue](http://gethue.com/).

Een HDInsight-toepassing is een toepassing die gebruikers kunnen installeren op een op Linux gebaseerd HDInsight-cluster.  Deze toepassingen kunnen zijn ontwikkeld door Microsoft, door onafhankelijke softwareleveranciers (ISV) of door u zelf.  

Andere verwante artikelen:

* [HDInsight-toepassingen installeren](hdinsight-apps-install-applications.md): informatie over het installeren van een HDInsight-toepassing op uw clusters.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.

## <a name="prerequisites"></a>Vereisten
Als u HDInsight-toepassingen wilt installeren op een bestaand HDInsight-cluster, hebt u een HDInsight-cluster nodig. Zie [Clusters maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) voor informatie over het maken van een cluster. U kunt ook HDInsight-toepassingen installeren wanneer u een HDInsight-cluster maakt.

## <a name="install-hdinsight-applications"></a>HDInsight-toepassingen installeren
HDInsight-toepassingen kunnen worden geïnstalleerd wanneer u een cluster maakt of op een bestaand HDInsight-cluster. Zie [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: een HDInsight-toepassing installeren) voor het definiëren van Azure Resource Manager-sjablonen.

De bestanden die nodig zijn voor het implementeren van deze toepassing (Hue):

* [azuredeploy.json](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): de Resource Manager-sjabloon voor het installeren van de HDInsight-toepassing. Zie [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: een HDInsight-toepassing installeren) om uw eigen Resource Manager-sjabloon te ontwikkelen.
* [hue-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): de scriptactie die wordt aangeroepen met de Resource Manager-sjabloon om het Edge-knooppunt te configureren.
* [hue-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): het binaire Hue-bestand dat wordt aangeroepen vanuit hui-install_v0.sh.
* [hue-binaries-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): het binaire Hue-bestand dat wordt aangeroepen vanuit hui-install_v0.sh.
* [webwasb-tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): een voorbeeldweb-app (Tomcat) die wordt aangeroepen vanuit hui-install_v0.sh.

**Hue installeren op een bestaand HDInsight-cluster**

1. Klik op de volgende afbeelding om u aan te melden bij Azure en de Resource Manager-sjabloon in de Azure Portal te openen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Met deze knop opent u een Resource Manager-sjabloon in de Azure Portal.  U vindt de Resource Manager-sjabloon hier: [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).  Zie [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: een HDInsight-toepassing installeren) voor informatie over het schrijven van deze Resource Manager-sjabloon.
2. Voer op de blade **Parameters** het volgende in:

   * **Clusternaam**: voer de naam in van het cluster waarop u de toepassing wilt installeren. Dit cluster moet een bestaand cluster zijn.
3. Klik op **OK** om de parameters op te slaan.
4. Voer op de blade **Aangepaste implementatie** de **Resourcegroep** in.  De resourcegroep is een container waarin het cluster, het afhankelijke opslagaccount en andere resources zijn gegroepeerd. U moet dezelfde resourcegroep gebruiken als het cluster.
5. Klik op **Juridische voorwaarden** en vervolgens op **Maken**.
6. Controleer of het selectievakje **Vastmaken aan dashboard** is geselecteerd. Klik vervolgens op **Maken**. U kunt de installatiestatus zien op de tegel die is vastgemaakt aan het portaldashboard en de portalmelding. (Klik op het belpictogram boven aan de portal.)  Het duurt ongeveer 10 minuten om de toepassing te installeren.

**Hue installeren tijdens het maken van een cluster**

1. Klik op de volgende afbeelding om u aan te melden bij Azure en de Resource Manager-sjabloon in de Azure Portal te openen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Met deze knop opent u een Resource Manager-sjabloon in de Azure Portal.  U vindt de Resource Manager-sjabloon hier: [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).  Zie [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: een HDInsight-toepassing installeren) voor informatie over het schrijven van deze Resource Manager-sjabloon.
2. Volg de instructies om het cluster te maken en Hue te installeren. Zie [Op Linux gebaseerde Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie over het maken van HDInsight-clusters.

Naast de Azure Portal kunt u ook [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-using-powershell) en [Azure CLI](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-using-cli) gebruiken om Resource Manager-sjablonen aan te roepen.

## <a name="validate-the-installation"></a>De installatie valideren
Controleer de status van de toepassing in de Azure Portal om de installatie van de toepassing te valideren. Daarnaast kunt u ook valideren of alle HTTP-eindpunten zijn gegenereerd zoals verwacht. Bovendien kunt u de webpagina valideren (indien aanwezig):

**De Hue-portal openen**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het linkermenu op **HDInsight-clusters**.  Als u dit niet ziet, klikt u op **Bladeren** en vervolgens op **HDInsight-clusters**.
3. Klik op het cluster waarop u de toepassing hebt geïnstalleerd.
4. Klik op de blade **Instellingen** onder de categorie **Algemeen** op **Toepassingen**. U ziet nu **Hue** vermeld staan op de blade **Geïnstalleerde apps**.
5. Klik in de lijst op **Hue** om de eigenschappen ervan weer te geven.  
6. Klik op de webpagina-koppeling voor het valideren van de website; Open het HTTP-eindpunt in een browser om de Hue-webgebruikersinterface te valideren, opent u het gebruik van SSH SSH-eindpunt. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

## <a name="troubleshoot-the-installation"></a>Problemen met de installatie oplossen
U kunt de installatiestatus van de toepassing controleren in de portalmelding (klik boven aan de portal op het belpictogram).

Als de installatie van een toepassing is mislukt, kunt u de foutberichten en foutopsporingsgegevens op drie plekken bekijken:

* HDInsight-toepassingen: algemene foutgegevens.

    Open het cluster vanuit de portal en klik op de blade Instellingen op Toepassingen:

    ![Fout bij het installeren van HDInsight-toepassingen](./media/hdinsight-apps-install-applications/hdinsight-apps-error.png)
* HDInsight-scriptactie: als het foutbericht voor HDInsight-toepassingen een mislukte scriptactie aangeeft, wordt er meer informatie over deze fout weergegeven in het deelvenster Scriptacties.

    Klik op de blade Instellingen op Scriptactie. De foutberichten worden weergegeven in de geschiedenis van de scriptactie

    ![Scriptactiefout voor HDInsight-toepassingen](./media/hdinsight-apps-install-applications/hdinsight-apps-script-action-error.png)
* Ambari-webgebruikersinterface: als de fout wordt veroorzaakt door het installatiescript, gebruikt u de Ambari-webgebruikersinterface om volledige logboeken over de installatiescripts te controleren.

    Zie [Probleemoplossing](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) voor meer informatie .

## <a name="remove-hdinsight-applications"></a>HDInsight-toepassingen verwijderen
Er zijn verschillende manieren om HDInsight-toepassingen te verwijderen.

### <a name="use-portal"></a>De portal gebruiken
**Een toepassing verwijderen via de portal**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het linkermenu op **HDInsight-clusters**.  Als u dit niet ziet, klikt u op **Bladeren** en vervolgens op **HDInsight-clusters**.
3. Klik op het cluster waarop u de toepassing hebt geïnstalleerd.
4. Klik op de blade **Instellingen** onder de categorie **Algemeen** op **Toepassingen**. Er wordt een lijst met geïnstalleerde toepassingen weergegeven. Voor deze zelfstudie wordt **Hue** vermeld op de blade **Geïnstalleerde apps**.
5. Klik met de rechtermuisknop op de toepassing die u wilt verwijderen. Klik vervolgens op **Verwijderen**.
6. Klik op **Ja** om te bevestigen.

In de portal kunt u ook het cluster verwijderen, evenals de resourcegroep die de toepassing bevat.

### <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
Met behulp van Azure PowerShell kunt u het cluster of de resourcegroep verwijderen. Zie [Clusters verwijderen met behulp van Azure PowerShell](hdinsight-administer-use-powershell.md#delete-clusters).

### <a name="use-azure-cli"></a>Azure CLI gebruiken
Met behulp van Azure CLI kunt u het cluster of de resourcegroep verwijderen. Zie [Clusters verwijderen met behulp van Azure CLI](hdinsight-administer-use-command-line.md#delete-clusters).

## <a name="next-steps"></a>Volgende stappen
* [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: een HDInsight-toepassing installeren): informatie over het ontwikkelen van Resource Manager-sjablonen voor het implementeren van HDInsight-toepassingen.
* [HDInsight-toepassingen installeren](hdinsight-apps-install-applications.md): informatie over het installeren van een HDInsight-toepassing op uw clusters.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [Op Linux gebaseerde HDInsight-clusters aanpassen met behulp van een scriptactie](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van een scriptactie om extra toepassingen te installeren.
* [Op Linux gebaseerde Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): informatie over het aanroepen van Resource Manager-sjablonen om HDInsight-clusters te maken.
* [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) (Lege edge-knooppunten gebruiken in HDInsight): meer informatie over het gebruik van een leeg edge-knooppunt om toegang te krijgen tot het HDInsight-cluster, HDInsight toepassingen te testen en HDInsight-toepassingen te hosten.
