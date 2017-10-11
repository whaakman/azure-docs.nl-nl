---
title: Domein-HDInsight-clusters - Azure beheren | Microsoft Docs
description: Informatie over het beheren van domein HDInsight-clusters
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 9b56ce6cc5bdd3b8d48d047751e978cad08598e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a>Domein-HDInsight-clusters (Preview) beheren
Informatie over de gebruikers en de rollen in HDInsight domein en het domein van de HDInsight-clusters beheren.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>Gebruikers van domein HDInsight-clusters
Een HDInsight-cluster is niet domein heeft twee gebruikersaccounts die zijn gemaakt tijdens het maken van het cluster:

* **Ambari admin**: dit account wordt ook wel *Hadoop gebruiker* of *HTTP gebruiker*. Dit account kan worden gebruikt voor aanmelding bij Ambari op https://&lt;clustername >. azurehdinsight.net. Het kan ook worden gebruikt voor query's uitvoeren op de Ambari-weergaven, taken via externe hulpprogramma's (dat wil zeggen PowerShell, Templeton, Visual Studio) uitvoeren en verifiëren met de Hive ODBC-stuurprogramma en de BI-tools (dat wil zeggen Excel, Power BI of Tableau).
* **SSH-gebruiker**: dit account kan worden gebruikt met SSH en sudo-opdrachten uit te voeren. Deze heeft bevoegdheden van de hoofdmap voor de virtuele Linux-machines.

Een domein HDInsight-cluster heeft drie nieuwe gebruikers naast Ambari Admin en SSH-gebruiker.

* **Zwerver admin**: dit account is het lokale beheerdersaccount van Apache Zwerver. Het is niet een gebruiker met active directory-domein. Dit account kan worden gebruikt beleidsregels instellen en zorgen dat andere gebruikers, beheerders of gedelegeerde beheerders (zodat die gebruikers u beleid beheren kunnen). De gebruikersnaam is standaard *admin* en het wachtwoord is hetzelfde als de Ambari Administrator-wachtwoord. Het wachtwoord kan worden bijgewerkt via de pagina instellingen in Zwerver.
* **Gebruiker met beheerdersrechten domein cluster**: dit account is een active directory-domeingebruiker aangewezen als de Hadoop-cluster beheerder zoals Ambari en Zwerver. Tijdens het maken van het cluster moet u de referenties van deze gebruiker opgeven. Deze gebruiker heeft de volgende bevoegdheden:

  * Computers toevoegen aan het domein en plaats deze in de organisatie-eenheid die u tijdens het maken van het cluster opgeeft.
  * Service-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft maken.
  * Omgekeerde DNS-vermeldingen te maken.

    Let op dat de AD-gebruikers hebben deze rechten.

    Er zijn een aantal eindpunten binnen het cluster (bijvoorbeeld Templeton) die niet worden beheerd door Zwerver en daarom zijn niet beveiligd. Deze eindpunten zijn vergrendeld voor alle gebruikers behalve de domeingebruiker van de cluster-beheerder.
* **Reguliere**: tijdens het maken van het cluster, kunt u meerdere active directory-groepen opgeven. De gebruikers in deze groepen worden gesynchroniseerd naar Zwerver en Ambari. Deze gebruikers zijn domeingebruikers en hebben toegang tot alleen Zwerver beheerde eindpunten (bijvoorbeeld Hiveserver2). Alle het RBAC beleid en de controle zal van toepassing zijn op deze gebruikers.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Rollen zijn van een domein HDInsight-clusters
HDInsight domein hebben de volgende rollen:

* Clusterbeheer
* Cluster-Operator
* Servicebeheerder
* Service-Operator
* Cluster-gebruiker

**Om te zien van de machtigingen van deze rollen**

1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **rollen**.
3. Klik op de blauwe vraagteken om te zien welke machtigingen:

    ![HDInsight-rolmachtigingen domein](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>De gebruikersinterface voor het beheer van de Ambari openen
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open uw HDInsight-cluster in een blade. Zie [lijst en geeft weer clusters](hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klik op **Dashboard** in het menu bovenaan om Ambari te openen.
4. Meld u bij de Ambari met het cluster administrator domeingebruikersnaam en wachtwoord.
5. Klik op de **Admin** vervolgkeuzemenu in de rechterbovenhoek hoek naar rechts en klik vervolgens op **Ambari beheren**.

    ![HDInsight domein Ambari beheren](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    De gebruikersinterface ziet eruit als:

    ![Domein HDInsight Ambari de gebruikersinterface voor beheer](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Lijst van domeingebruikers gesynchroniseerd vanuit uw Active Directory
1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **gebruikers**. U ziet alle gebruikers gesynchroniseerd vanuit uw Active Directory met het HDInsight-cluster.

    ![Domein HDInsight Ambari management UI gebruikers weergeven](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>De domein-groepen die gesynchroniseerd vanuit uw Active Directory
1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **groepen**. U ziet de groepen die zijn gesynchroniseerd vanuit uw Active Directory met het HDInsight-cluster.

    ![Domein HDInsight Ambari-gebruikersinterface lijst beheergroepen](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Hive-weergaven machtigingen configureren
1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **weergaven**.
3. Klik op **HIVE** om de details weer te geven.

    ![Domein HDInsight Ambari management UI Hive-weergaven](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klik op de **Hive-weergave** koppelen aan het Hive-weergaven configureren.
5. Schuif omlaag naar de **machtigingen** sectie.

    ![Domein HDInsight Ambari management UI Hive-weergaven machtigingen configureren](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klik op **gebruiker toevoegen** of **groep toevoegen**, en geef vervolgens de gebruikers of groepen die Hive-weergaven kunnen gebruiken.

## <a name="configure-users-for-the-roles"></a>Gebruikers voor de rollen configureren
 Zie voor een lijst met functies en hun machtigingen [rollen van domein HDInsight-clusters](#roles-of-domain---joined-hdinsight-clusters).

1. Ambari Management UI te openen.  Zie [opent de gebruikersinterface voor het beheer van de Ambari](#open-the-ambari-management-ui).
2. Klik in het menu links op **rollen**.
3. Klik op **gebruiker toevoegen** of **groep toevoegen** gebruikers en groepen toewijzen aan andere rollen.

## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Aan een domein gekoppelde HDInsight-clusters configureren) om een HDInsight-cluster te configureren dat is gekoppeld aan een domein.
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie voor het uitvoeren van Hive-query's met SSH op domein HDInsight-clusters [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
