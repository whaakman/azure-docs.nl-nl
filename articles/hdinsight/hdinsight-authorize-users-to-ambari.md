---
title: Gebruikers machtigen voor Ambari-weergaven - Azure HDInsight | Microsoft Docs
description: Het Ambari-gebruikers en groepen machtigingen voor domein-HDInsight-clusters beheren.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: b2040bfad42f2913882792c0055225b2c3d521b5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="authorize-users-for-ambari-views"></a>Gebruikers machtigen voor Ambari-weergaven

[HDInsight-clusters domein](./domain-joined/apache-domain-joined-introduction.md) bieden bedrijfsniveau mogelijkheden, zoals verificatie op basis van Azure Active Directory. U kunt nieuwe gebruikers synchroniseren <!-- [synchronize new users](hdinsight-sync-aad-users-to-cluster.md) --> toegevoegd aan Azure AD-groepen die toegang tot het cluster, zodat deze bepaalde gebruikers bepaalde acties uit te voeren. Werken met gebruikers, groepen en machtigingen in Ambari wordt ondersteund voor zowel de HDInsight-cluster domein en de standaard HDInsight-cluster.

Active Directory: gebruikers kunnen zich aanmelden op de clusterknooppunten met hun domeinreferenties. Ze kunnen ook hun domeinreferenties gebruiken om te verifiëren, cluster-interacties met andere goedgekeurde eindpunten zoals Hue, Ambari-weergaven, ODBC, JDBC, PowerShell en de REST-API's.

> [!WARNING]
> Wijzig het wachtwoord van de Ambari-watchdog (hdinsightwatchdog) op uw Linux gebaseerde HDInsight-cluster niet. Het wachtwoord wilt wijzigen, verbreekt de mogelijkheid voor het gebruik van scriptacties of vergroten/verkleinen bewerkingen uitvoeren met uw cluster.

Als u dit nog niet hebt gedaan, volgt u [deze instructies](./domain-joined/apache-domain-joined-configure.md) voor het inrichten van een nieuw domein-cluster.

## <a name="access-the-ambari-management-page"></a>Toegang tot de Ambari-management-pagina

Om te krijgen tot de **Ambari management-pagina** op de [Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md), blader naar  **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** . Voer de gebruikersnaam voor de beheerder cluster en het wachtwoord die u hebt gedefinieerd bij het maken van het cluster. Selecteer vervolgens in het dashboard Ambari **beheren Ambari** onder de **admin** menu:

![Ambari beheren](./media/hdinsight-authorize-users-to-ambari/manage-ambari.png)

## <a name="grant-permissions-to-hive-views"></a>Machtigingen toekennen voor Hive-weergaven

Ambari wordt geleverd met exemplaren weergeven voor Hive en Tez, onder andere. Om toegang tot een of meer Hive exemplaren weergeven, gaat u naar de **Ambari management-pagina**.

1. Selecteer op de beheerpagina de **weergaven** koppeling onder de **weergaven** kop menu aan de linkerkant.

    ![Weergaven koppelen](./media/hdinsight-authorize-users-to-ambari/views-link.png)

2. Vouw op de pagina weergaven de **HIVE** rij. Er is een standaard Hive-weergave die wordt gemaakt wanneer de Hive-service wordt toegevoegd aan het cluster. U kunt ook meer Hive-exemplaren naar behoefte maken. Selecteer een Hive-weergave:

    ![Weergaven - Hive-weergave](./media/hdinsight-authorize-users-to-ambari/views-hive-view.png)

3. Schuif naar de onderkant van de pagina weergeven. Onder de *machtigingen* sectie, hebt u twee opties voor het verlenen van domeingebruikers hun machtigingen naar de weergave:

**Machtigingen toekennen aan deze gebruikers** ![machtigingen toekennen aan deze gebruikers](./media/hdinsight-authorize-users-to-ambari/add-user-to-view.png)

**Machtigingen toekennen aan deze groepen** ![machtigingen toekennen aan deze groepen](./media/hdinsight-authorize-users-to-ambari/add-group-to-view.png)

4. Als u wilt een gebruiker toevoegt, selecteert u de **gebruiker toevoegen** knop.

    * Begint te typen van de gebruikersnaam en u ziet een vervolgkeuzelijst met vooraf gedefinieerde namen.

    ![Gebruiker autocompletes](./media/hdinsight-authorize-users-to-ambari/user-autocomplete.png)

    * Selecteer of klaar bent met typen, de gebruikersnaam. Selecteer om de naam van deze gebruiker toe als een nieuwe gebruiker de **nieuw** knop.

    * Sla uw wijzigingen, selecteer de **Blauw selectievakje**.

    ![Gebruiker heeft ingevoerd](./media/hdinsight-authorize-users-to-ambari/user-entered.png)

5. Een groep toevoegen, selecteert u de **groep toevoegen** knop.

    * Typ de naam van de groep. Het proces van de groepsnaam van een bestaande selecteren of toevoegen van een nieuwe groep is hetzelfde als voor het toevoegen van gebruikers.
    * Sla uw wijzigingen, selecteer de **Blauw selectievakje**.

    ![Opgegeven groep](./media/hdinsight-authorize-users-to-ambari/group-entered.png)

Toevoegen van gebruikers rechtstreeks naar een weergave is handig als u machtigingen toewijzen aan een gebruiker wilt te gebruiken die weergave, maar niet dat ze wilt lid zijn van een groep die extra machtigingen heeft. Om de hoeveelheid administratieve overhead verminderen, is het mogelijk dat het eenvoudiger om machtigingen te wijzen aan groepen.

## <a name="grant-permissions-to-tez-views"></a>Machtigingen toekennen voor Tez-weergaven

De Tez-exemplaren kunnen de gebruikers om te controleren en fouten opsporen in Tez-taken, verzonden door het Hive-query's en Pig-scripts. Er is een standaard Tez weergave exemplaar dat wordt gemaakt wanneer het cluster is ingericht.

Voor gebruikers en groepen toewijzen aan een exemplaar van de weergave Tez, vouw de **TEZ** rij op de pagina weergaven, zoals eerder beschreven.

![Weergaven - Tez weergeven](./media/hdinsight-authorize-users-to-ambari/views-tez-view.png)

Gebruikers of groepen wilt toevoegen, herhaalt u stap 3-5 in de vorige sectie.

## <a name="assign-users-to-roles"></a>Gebruikers toewijzen aan rollen

Er zijn vijf beveiligingsrollen voor gebruikers en groepen die worden vermeld in de volgorde van toegangsmachtigingen verlagen:

* Clusterbeheer
* Cluster-Operator
* Servicebeheerder
* Service-Operator
* Cluster-gebruiker

Voor het beheren van rollen, gaat u naar de **Ambari management-pagina**, selecteer vervolgens de **rollen** koppeling binnen de *Clusters* menugroep aan de linkerkant.

![Rollen menu koppeling](./media/hdinsight-authorize-users-to-ambari/roles-link.png)

De lijst met machtigingen die u aan elke rol, klik op de blauwe vraagteken naast de **rollen** tabelkoptekst op de pagina functies.

![Rollen menu koppeling](./media/hdinsight-authorize-users-to-ambari/roles-permissions.png)

Op deze pagina zijn twee verschillende weergaven die u gebruiken kunt voor het beheren van rollen voor gebruikers en groepen: blokkeren en de lijst.

### <a name="block-view"></a>Weergave blokkeren

De weergave blokkeren weergegeven van elke rol in een eigen rij, en bevat de **rollen toewijzen aan deze gebruikers** en **rollen toewijzen aan deze groepen** zoals hierboven beschreven opties.

![Rollen blokkeren weergave](./media/hdinsight-authorize-users-to-ambari/roles-block-view.png)

### <a name="list-view"></a>Lijstweergave

De lijstweergave biedt mogelijkheden voor het bewerken van snelle in twee categorieën worden onderverdeeld: gebruikers en groepen.

* De categorie gebruikers van de lijstweergave geeft een lijst van alle gebruikers, zodat u een rol voor elke gebruiker in de vervolgkeuzelijst selecteren.

    ![Rollen lijstweergave - gebruikers](./media/hdinsight-authorize-users-to-ambari/roles-list-view-users.png)

* De categorie groepen van de lijstweergave wordt weergegeven voor alle groepen en de rol die is toegewezen aan elke groep. In ons voorbeeld wordt de lijst met groepen worden gesynchroniseerd vanuit de Azure AD-groepen die is opgegeven in de **toegang gebruikersgroep** eigenschap van het cluster domeininstellingen. Zie [maken HDInsight-cluster](./domain-joined/apache-domain-joined-configure.md#create-an-hdinsight-cluster-in-the-vnet).

    ![Rollen lijstweergave - groepen](./media/hdinsight-authorize-users-to-ambari/roles-list-view-groups.png)

    In de afbeelding hierboven, de groep 'hiveusers' is toegewezen, wordt de *Cluster gebruiker* rol. Dit is een alleen-lezen-rol waarmee de gebruikers van die groep bekijken maar niet wijzigen-configuraties en cluster metrische gegevens.

## <a name="log-in-to-ambari-as-a-view-only-user"></a>Meld u aan bij Ambari als een gebruiker alleen-weergeven

We hebben ons Azure AD-gebruiker met domein 'hiveuser1' machtigingen toegewezen aan weergaven Hive en Tez. Als we de Ambari-Webgebruikersinterface starten en deze gebruiker domeinreferenties (Azure AD de gebruikersnaam in de e-mailindeling en wachtwoord) invoert, wordt de gebruiker wordt omgeleid naar de pagina Ambari-weergaven. De gebruiker kan elke toegankelijk weergave selecteren vanaf deze locatie. De gebruiker kan niet gaat u naar andere delen van de site, inclusief de's dashboard, services, hosts, waarschuwingen of beheerder.

![Gebruiker met alleen weergaven](./media/hdinsight-authorize-users-to-ambari/user-views-only.png)

## <a name="log-in-to-ambari-as-a-cluster-user"></a>Meld u aan bij Ambari als een gebruiker van het cluster

We hebben ons Azure AD-gebruiker van het domein 'hiveuser2' toegewezen aan de *Cluster gebruiker* rol. Deze functie is toegang tot het dashboard en alle menu-items. Een cluster heeft minder toegestane opties dan een beheerder. Bijvoorbeeld: hiveuser2 configuraties voor elk van de services kunt bekijken, maar deze niet bewerken.

![Gebruiker met de gebruikersrol van het Cluster](./media/hdinsight-authorize-users-to-ambari/user-cluster-user-role.png)

## <a name="next-steps"></a>Volgende stappen

* [Op domein HDInsight Hive-beleid configureren](./domain-joined/apache-domain-joined-run-hive.md)
* [Domein-HDInsight-clusters beheren](./domain-joined/apache-domain-joined-manage.md)
* [De weergave Hive gebruiken met Hadoop in HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md)

<!-- * [Synchronize Azure AD users to the cluster](hdinsight-sync-aad-users-to-cluster.md) -->
