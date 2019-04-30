---
title: Een Azure Databricks-werkruimte in een Virtueelnetwerk maken
description: In dit artikel wordt beschreven hoe u Azure Databricks in uw virtuele netwerk implementeert.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 295b64b10f9f78ca6224d60fb84c6d1310aaa42e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60770611"
---
# <a name="quickstart-create-an-azure-databricks-workspace-in-a-virtual-network"></a>Quickstart: Een Azure Databricks-werkruimte in een Virtueelnetwerk maken

Deze quickstart laat zien hoe u een Azure Databricks-werkruimte maken in een virtueel netwerk. U maakt ook een Apache Spark-cluster in die werkruimte.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) aan.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer in de Azure portal, **een resource maken** > **netwerken** > **virtueel netwerk**.

2. Onder **virtueel netwerk maken**, toepassen van de volgende instellingen: 

    |Instelling|Voorgestelde waarde|Description|
    |-------|---------------|-----------|
    |Name|databricks-quickstart|Selecteer een naam voor het virtuele netwerk.|
    |Adresruimte|10.1.0.0/16|Het adresbereik van het virtuele netwerk in CIDR-notatie.|
    |Abonnement|\<Uw abonnement\>|Selecteer het Azure-abonnement dat u wilt gebruiken.|
    |Resourcegroep|databricks-quickstart|Selecteer **nieuw** en voert u een nieuwe Resourcegroepnaam voor uw account.|
    |Locatie|\<Selecteer de regio die het dichtst bij uw gebruikers is gelegen\>|Selecteer een geografische locatie waar u uw virtuele netwerk kunt hosten. Gebruik de locatie die het dichtst bij uw gebruikers is.|
    |Subnetnaam|standaardinstelling|Selecteer een naam voor de standaard-subnet in uw virtuele netwerk.|
    |Subnetadresbereik|10.1.0.0/24|Het adresbereik van het subnet in CIDR-notatie. Het moet zijn opgenomen in de adresruimte van het virtuele netwerk. Het adresbereik van een subnet dat gebruikt wordt, kan niet worden bewerkt.|

    ![Een virtueel netwerk maken in Azure portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network.png)

3. Zodra de implementatie voltooid is, gaat u naar het virtuele netwerk en selecteer **adresruimte** onder **instellingen**. In het vak *aanvullend adresbereik toevoegen*, invoegen `10.179.0.0/16` en selecteer **opslaan**.

    ![Met Azure virtual network-adresruimte](./media/quickstart-create-databricks-workspace-vnet-injection/add-address-space.png)

## <a name="create-an-azure-databricks-workspace"></a>Een Azure Databricks-werkruimte maken

1. Selecteer in de Azure portal, **een resource maken** > **Analytics** > **Databricks**.

2. Onder **Azure Databricks Service**, toepassen van de volgende instellingen:

    |Instelling|Voorgestelde waarde|Description|
    |-------|---------------|-----------|
    |Naam van de werkruimte|databricks-quickstart|Selecteer een naam voor uw Azure Databricks-werkruimte.|
    |Abonnement|\<Uw abonnement\>|Selecteer het Azure-abonnement dat u wilt gebruiken.|
    |Resourcegroep|databricks-quickstart|Selecteer de resourcegroep die u hebt gebruikt voor het virtuele netwerk.|
    |Locatie|\<Selecteer de regio die het dichtst bij uw gebruikers is gelegen\>|Kies de dezelfde locatie als het virtuele netwerk.|
    |Prijscategorie|Kiezen tussen Standard of Premium.|Zie voor meer informatie over Prijscategorieën, de [pagina met prijzen voor Databricks](https://azure.microsoft.com/pricing/details/databricks/).|
    |Azure Databricks-werkruimte in uw Virtueelnetwerk implementeren|Ja|Deze instelling kunt u een Azure Databricks-werkruimte in uw virtuele netwerk te implementeren.|
    |Virtual Network|databricks-quickstart|Selecteer het virtuele netwerk dat u in de vorige sectie hebt gemaakt.|
    |Openbare subnetnaam|public-subnet|Gebruik de standaardnaam van de openbare subnet.|
    |Openbaar Subnet CIDR-bereik|10.179.64.0/18|CIDR-bereik voor dit subnet moet liggen tussen /18 en /26.|
    |De naam van de privé-Subnet|private-subnet|Gebruik de standaardnaam van de privé-subnet.|
    |Privé-Subnet CIDR-bereik|10.179.0.0/18|CIDR-bereik voor dit subnet moet liggen tussen /18 en /26.|

    ![Een Azure Databricks-werkruimte maken in Azure portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace.png)

3. Zodra de implementatie voltooid is, gaat u naar de Azure Databricks-resource. U ziet dat de peering op virtueel netwerk is uitgeschakeld. Let ook op de resourcegroep en de beheerde resourcegroep op de overzichtspagina. 

    ![Overzicht van Azure Databricks in Azure portal](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-overview-portal.png)

    De beheerde resourcegroep bevat de fysieke locatie van het opslagaccount (DBFS), werknemer-sg (netwerkbeveiligingsgroep), werknemers-vnet (virtueel netwerk). Het is ook de locatie waar virtuele machines, schijven, IP-adres en de netwerkinterface wordt gemaakt. Deze resourcegroep is vergrendeld door standaard. maar als een cluster wordt gestart in het virtuele netwerk, wordt een netwerkinterface gemaakt tussen de werkrollen-vnet in de beheerde resourcegroep en het virtuele netwerk 'hub'.

    ![Azure Databricks beheerde resourcegroep](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group.png)

## <a name="create-a-cluster"></a>Een cluster maken

> [!NOTE]
> Als u een gratis account wilt gebruiken om het Azure Databricks-cluster te maken, gaat u voordat het cluster is gemaakt naar uw profiel en wijzigt u uw abonnement in **betalen per gebruik**. Zie [Gratis Azure-account](https://azure.microsoft.com/free/) voor meer informatie.

1. Ga terug naar uw Azure Databricks-service en selecteer **werkruimte starten** op de **overzicht** pagina.

2. Selecteer **Clusters** > **+ -Cluster maken**. Maak vervolgens een clusternaam op, zoals *databricks-snelstartgids-cluster*, en de overige standaardinstellingen accepteren. Selecteer **Cluster maken**.

    ![Azure Databricks-cluster maken](./media/quickstart-create-databricks-workspace-vnet-injection/create-cluster.png)

3. Zodra het cluster wordt uitgevoerd, kunt u terugkeren naar de beheerde resourcegroep in Azure portal. U ziet de nieuwe virtuele machines, schijven, IP-adres en netwerkinterfaces. Een netwerkinterface wordt gemaakt in elk van de openbare en particuliere subnetten met IP-adressen.  

    ![Azure voor de Databricks beheerde resourcegroep na het maken van clusters](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group2.png)

4. Ga terug naar uw Azure Databricks-werkruimte en selecteer het cluster dat u hebt gemaakt. Navigeer naar de **Executor** tabblad op de **Spark gebruikersinterface** pagina. U ziet dat de adressen van het stuurprogramma en de Executor binnen het bereik met privé-subnet. In dit voorbeeld wordt het stuurprogramma is 10.179.0.6 en Executor zijn 10.179.0.4 en 10.179.0.5. Uw IP-adressen kunnen afwijken.

    ![Azure Databricks Spark gebruikersinterface Executor](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-sparkui-executors.png)

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u het artikel hebt doorgenomen, kunt u het cluster beëindigen. Dit doet u door vanuit de Azure Databricks-werkruimte in het linkerdeelvenster **Clusters** te selecteren. Voor het cluster dat u wilt beëindigen, plaatst u de cursor op het weglatingsteken onder de kolom **Acties** en selecteert u het **beëindigingspictogram**. Hierdoor wordt voorkomen dat het cluster.

Als u het cluster niet handmatig beëindigt, stopt het cluster automatisch, op voorwaarde dat het selectievakje **Beëindigen na \_\_ minuten inactiviteit** is ingeschakeld tijdens het maken van het cluster. In dat geval stopt het cluster automatisch als het gedurende de opgegeven tijd inactief is geweest.

Als u niet gebruiken van het cluster wilt, kunt u de resourcegroep die u hebt gemaakt in Azure portal verwijderen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een Spark-cluster in Azure Databricks die u hebt geïmplementeerd met een virtueel netwerk gemaakt. Ga naar het volgende artikel voor informatie over het opvragen van een SQL Server Linux Docker-container in het virtuele netwerk met behulp van JDBC uit een Azure Databricks-notebook.  

> [!div class="nextstepaction"]
>[Query uitvoeren op een SQL Server Linux Docker-container in een virtueel netwerk van een Azure Databricks-notebook](vnet-injection-sql-server.md)
