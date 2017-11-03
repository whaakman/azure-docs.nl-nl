---
title: Hadoop-clusters aanpassen voor het Team gegevens wetenschap proces | Microsoft Docs
description: Populaire Python-modules beschikbaar gesteld in aangepaste Azure HDInsight Hadoop-clusters.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 522e33b399f2648427464b439bc4405e9e8097cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Azure HDInsight Hadoop-clusters aanpassen voor Team Data Science Process
In dit artikel beschrijft hoe een HDInsight Hadoop-cluster aanpassen door het 64-bits Anaconda (Python 2.7) installeren op elk knooppunt wanneer het cluster is ingericht als een HDInsight-service. Ook ziet u hoe voor toegang tot de headnode voor het verzenden van aangepaste taken aan het cluster. Deze aanpassing wordt veel populaire Python-modules, die zijn opgenomen in Anaconda, gemakkelijk beschikbaar voor gebruik in de gebruiker gedefinieerde functies (UDF's) die zijn ontworpen voor het verwerken van Hive-records in het cluster. Zie voor instructies voor de procedures in dit scenario gebruikt, [het indienen van Hive-query's](move-hive-tables.md#submit).

De volgende menukoppelingen naar onderwerpen waarin wordt beschreven hoe u voor het instellen van de verschillende gegevens wetenschappelijke omgevingen die worden gebruikt door de [Team gegevens wetenschap proces (TDSP)](overview.md).

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

## <a name="customize"></a>Azure HDInsight Hadoop-Cluster aanpassen
Voor het maken van aangepaste HDInsight Hadoop-cluster, starten door in te loggen bij [ **klassieke Azure-portal**](https://manage.windowsazure.com/), klikt u op **nieuw** op de links linksonder en selecteer vervolgens DATA SERVICES - > HDINSIGHT -> **aangepast maken** online zetten van de **Clusterdetails** venster. 

![Werkruimte maken](./media/customize-hadoop-cluster/customize-cluster-img1.png)

De naam van het cluster moet worden gemaakt op de configuratiepagina 1 invoeren en accepteer de standaardwaarden voor de andere velden. Klik op de pijl naar de volgende pagina van de configuratie. 

![Werkruimte maken](./media/customize-hadoop-cluster/customize-cluster-img1.png)

Voer het aantal op de configuratiepagina 2 **GEGEVENSKNOOPPUNTEN**, selecteer de **regio/VIRTUEEL netwerk**, en selecteert u de grootte van de **HOOFDKNOOPPUNT** en de **gegevens KNOOPPUNT**. Klik op de pijl naar de volgende pagina van de configuratie.

> [!NOTE]
> De **regio/VIRTUEEL netwerk** moet hetzelfde zijn als de regio van het opslagaccount dat u wilt worden gebruikt voor het HDInsight Hadoop-cluster. Anders in de vierde pagina van de configuratie van het opslagaccount wordt niet weergegeven op de vervolgkeuzelijst van **accountnaam**.
> 
> 

![Werkruimte maken](./media/customize-hadoop-cluster/customize-cluster-img3.png)

Geef een gebruikersnaam en wachtwoord voor het HDInsight Hadoop-cluster op de configuratiepagina 3. **Geen** selecteert u de *Voer de Hive/Oozie-Metastore*. Klik vervolgens op de pijl naar de volgende pagina van de configuratie. 

![Werkruimte maken](./media/customize-hadoop-cluster/customize-cluster-img4.png)

Geef de naam van het opslagaccount, de standaardcontainer van HDInsight Hadoop-cluster op de configuratiepagina 4. Als u selecteert *maken standaardcontainer* in de **STANDAARDCONTAINER** dropdown wilt weergeven, een container met dezelfde naam als het cluster wordt gemaakt. Klik op de pijl om door te gaan naar de laatste configuratiepagina.

![Werkruimte maken](./media/customize-hadoop-cluster/customize-cluster-img5.png)

Op het uiteindelijke **scriptacties** configuratiepagina, klikt u op **scriptactie toevoegen** knop en de tekstvelden vullen met de volgende waarden.

* **NAAM** -elke tekenreeks als de naam van deze scriptactie
* **KNOOPPUNTTYPE** : Selecteer **alle knooppunten**
* **SCRIPT-URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
  * *publicscripts* is een openbare container in het opslagaccount 
  * *getgoing* wij gebruiken voor het delen van bestanden van de PowerShell-script wilt vergemakkelijken werk van de gebruiker in Azure
* **PARAMETERS** -(leeg laten)

Tot slot klikt u op het vinkje voor het starten van het maken van aangepaste HDInsight Hadoop-cluster. 

![Werkruimte maken](./media/customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Toegang tot het hoofdknooppunt van Hadoop-Cluster
Voordat u toegang hebt tot het hoofdknooppunt van het Hadoop-cluster via RDP, moet u externe toegang tot het Hadoop-cluster in Azure inschakelen. 

1. Meld u aan bij de [ **klassieke Azure-portal**](https://manage.windowsazure.com/), selecteer **HDInsight** aan de linkerkant Hadoop-cluster in de lijst met clusters, klik op de **configuratie**  tabblad en klik vervolgens op de **externe inschakelen** pictogram aan de onderkant van de pagina.
   
    ![Werkruimte maken](./media/customize-hadoop-cluster/enable-remote-access-1.png)
2. In de **extern bureaublad configureren** venster de velden gebruikersnaam en wachtwoord invoeren en selecteer de vervaldatum voor externe toegang. Klik vervolgens op het vinkje om de externe toegang met het hoofdknooppunt van het Hadoop-cluster te maken.
   
    ![Werkruimte maken](./media/customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> De gebruikersnaam en wachtwoord voor de externe toegang zijn niet de gebruikersnaam en wachtwoord op dat u bij het maken van het Hadoop-cluster. Dit is een afzonderlijke set referenties. Daarnaast heeft de vervaldatum van de RAS-binnen 7 dagen vanaf de huidige datum.
> 
> 

Nadat u externe toegang is ingeschakeld, klikt u op **CONNECT** aan de onderkant van de pagina op afstand verbinding met het hoofdknooppunt. U aanmelden met het hoofdknooppunt van het Hadoop-cluster door te voeren van de referenties voor de RAS-gebruiker die u eerder hebt opgegeven.

![Werkruimte maken](./media/customize-hadoop-cluster/enable-remote-access-3.png)

De volgende stappen in het proces geavanceerde analyses zijn toegewezen de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) en bevatten mogelijk stappen die gegevens in HDInsight, verplaatsen en vervolgens verwerken en het voorbeeld ter voorbereiding van het leren van de gegevens met Azure Machine Learning.

Zie [het indienen van Hive-query's](move-hive-tables.md#submit) voor instructies over het toegang krijgen tot de Python-modules die zijn opgenomen in Anaconda van het hoofdknooppunt van het cluster in de gebruiker gedefinieerde functies (UDF's) die worden gebruikt voor het verwerken van Hive-records die zijn opgeslagen in de cluster.

