---
title: Gebruikers machtigen voor Ambari-weergaven - Azure HDInsight
description: Over het beheren van de Ambari-gebruikers en groepen machtigingen voor HDInsight-clusters met ESP ingeschakeld.
services: hdinsight
author: maxluk
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: cf001d86356f4dd5fd3735803f0e329aa1e0940d
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224019"
---
# <a name="authorize-users-for-apache-ambari-views"></a>Gebruikers machtigen voor Apache Ambari-weergaven

[Enterprise Security Package (ESP) ingeschakeld HDInsight-clusters](./domain-joined/apache-domain-joined-introduction.md) bieden geavanceerde mogelijkheden, met inbegrip van Azure Active Directory gebaseerde verificatie. U kunt [nieuwe gebruikers synchroniseren](hdinsight-sync-aad-users-to-cluster.md) toegevoegd aan Azure AD-groepen die toegang tot het cluster, zodat deze bepaalde gebruikers bepaalde acties uit te voeren. Werken met gebruikers, groepen en machtigingen in [Apache Ambari](https://ambari.apache.org/) wordt ondersteund voor zowel ESP HDInsight-clusters en standard HDInsight-clusters.

Active Directory: gebruikers kunnen zich aanmelden op de clusterknooppunten met hun domeinreferenties. Ze kunnen hun domeinreferenties ook gebruiken om te verifiëren van de cluster-interacties met andere goedgekeurde eindpunten, zoals [Hue](https://gethue.com/), Ambari-weergaven, ODBC, JDBC, PowerShell en REST-API's.

> [!WARNING]  
> Wijzig het wachtwoord van de Ambari-watchdog (hdinsightwatchdog) op uw Linux gebaseerde HDInsight-cluster niet. Wijzigen van het wachtwoord, verbreekt de mogelijkheid om te gebruiken van scriptacties of vergroten / verkleinen met uw cluster uit te voeren.

Als u hebt nog niet gedaan, voert u de [deze instructies](./domain-joined/apache-domain-joined-configure.md) voor het inrichten van een nieuw ESP-cluster.

## <a name="access-the-ambari-management-page"></a>Toegang tot de Ambari-beheerpagina

Om te gaan naar de **Ambari beheerpagina** op de [Apache Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md), blader naar **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`**. Voer de gebruikersnaam van de cluster-beheerder en het wachtwoord die u hebt gedefinieerd bij het maken van het cluster. Selecteer vervolgens in het dashboard Ambari **beheren Ambari** onder de **admin** menu:

![Manage Ambari](./media/hdinsight-authorize-users-to-ambari/manage-ambari.png)

## <a name="grant-permissions-to-apache-hive-views"></a>Machtigingen verlenen voor Apache Hive-weergaven

Ambari wordt geleverd met exemplaren weergeven voor [Apache Hive](https://hive.apache.org/) en [Apache TEZ](https://tez.apache.org/), onder andere. Om toegang te verlenen aan een of meer Hive exemplaren weergeven, gaat u naar de **Ambari beheerpagina**.

1. Selecteer op de beheerpagina de **weergaven** koppeling onder de **weergaven** kop menu aan de linkerkant.

    ![Weergaven koppelen](./media/hdinsight-authorize-users-to-ambari/views-link.png)

2. Vouw op de pagina weergaven uit de **HIVE** rij. Er is een standaard Hive-weergave die wordt gemaakt wanneer de Hive-service is toegevoegd aan het cluster. U kunt ook meer Hive-exemplaren naar behoefte maken. Selecteer een Hive-weergave:

    ![Weergaven - Hive-weergave](./media/hdinsight-authorize-users-to-ambari/views-hive-view.png)

3. Schuif naar de onderkant van de pagina bekijken. Onder de *machtigingen* sectie, hebt u twee opties voor het verlenen van domeingebruikers hun machtigingen naar de weergave:

**Machtigingen verlenen aan deze gebruikers** ![machtigingen verlenen aan deze gebruikers](./media/hdinsight-authorize-users-to-ambari/add-user-to-view.png)

**Machtigingen verlenen aan deze groepen** ![machtigingen verlenen aan deze groepen](./media/hdinsight-authorize-users-to-ambari/add-group-to-view.png)

1. Een gebruiker toevoegen, selecteert u de **gebruiker toevoegen** knop.

   * Begin te typen van de gebruikersnaam en u ziet een vervolgkeuzelijst met vooraf gedefinieerde namen.

     ![Gebruiker autocompletes](./media/hdinsight-authorize-users-to-ambari/user-autocomplete.png)

   * Selecteer of klaar bent met typen, de naam van de gebruiker. Selecteer om de naam van deze gebruiker toe als een nieuwe gebruiker de **nieuw** knop.

   * Om uw wijzigingen hebt opgeslagen, selecteert u de **Blauw selectievakje**.

     ![Gebruiker heeft ingevoerd](./media/hdinsight-authorize-users-to-ambari/user-entered.png)

1. Een groep toevoegen, selecteert u de **groep toevoegen** knop.

   * Begin met de naam van de typen. Het proces van het selecteren van de groepsnaam van een bestaande, of het toevoegen van een nieuwe groep is hetzelfde als voor het toevoegen van gebruikers.
   * Om uw wijzigingen hebt opgeslagen, selecteert u de **Blauw selectievakje**.

     ![Opgegeven groep](./media/hdinsight-authorize-users-to-ambari/group-entered.png)

Toevoegen van gebruikers rechtstreeks naar een weergave is handig als u machtigingen toewijzen aan een gebruiker te gebruiken die weergave wilt, maar niet wilt dat ze lid zijn van een groep met aanvullende machtigingen. Als u wilt de administratieve overhead verminderen, is het wellicht eenvoudiger machtigingen toewijzen aan groepen.

## <a name="grant-permissions-to-apache-tez-views"></a>Machtigingen verlenen aan weergaven van Apache TEZ

De [Apache TEZ](https://tez.apache.org/) exemplaren weergeven dat de gebruikers om te controleren en fouten opsporen in Tez-taken, ingediend door [Apache Hive](https://hive.apache.org/) query's en [Apache Pig](https://pig.apache.org/) scripts. Er is een standaard Tez weergave exemplaar dat wordt gemaakt wanneer het cluster is ingericht.

Als u wilt toewijzen van gebruikers en groepen naar een exemplaar van de weergave Tez, vouw de **TEZ** rij op de pagina weergaven, zoals eerder beschreven.

![Weergaven - Tez weergeven](./media/hdinsight-authorize-users-to-ambari/views-tez-view.png)

Gebruikers of groepen wilt toevoegen, herhaalt u stap 3-5 in de vorige sectie.

## <a name="assign-users-to-roles"></a>Gebruikers aan rollen toewijzen

Er zijn vijf beveiligingsrollen voor gebruikers en groepen uit die worden vermeld in de volgorde van toegangsmachtigingen verlagen:

* Clusterbeheer
* Cluster-Operator
* Servicebeheerder
* Service-Operator
* Clustergebruiker

Voor het beheren van rollen, gaat u naar de **Ambari beheerpagina**en selecteer vervolgens de **rollen** koppeling binnen de *Clusters* menugroep aan de linkerkant.

![Menu-koppeling rollen](./media/hdinsight-authorize-users-to-ambari/roles-link.png)

De lijst van machtigingen die aan elke rol wilt bekijken, klikt u op de blauwe vraagteken naast de **rollen** tabelkoptekst op de pagina functies.

![Menu-koppeling rollen](./media/hdinsight-authorize-users-to-ambari/roles-permissions.png)

Op deze pagina zijn er twee verschillende weergaven die u gebruiken kunt voor het beheren van rollen voor gebruikers en groepen: Blok- en lijst.

### <a name="block-view"></a>Weergave blokkeren

De weergave blokkeren wordt elke rol in een eigen rij weergegeven, en bevat de **rollen toewijzen aan deze gebruikers** en **rollen toewijzen aan deze groepen** opties zoals eerder beschreven.

![Rollen blokkeren weergave](./media/hdinsight-authorize-users-to-ambari/roles-block-view.png)

### <a name="list-view"></a>Lijstweergave

De lijstweergave biedt snelle bewerkingsmogelijkheden in twee categorieën: Gebruikers en groepen.

* De categorie van de gebruikers van de lijstweergave geeft een lijst van alle gebruikers, zodat u een rol voor elke gebruiker in de vervolgkeuzelijst selecteren.

    ![Rollen lijstweergave - gebruikers](./media/hdinsight-authorize-users-to-ambari/roles-list-view-users.png)

*  De categorie groepen van de lijstweergave geeft alle groepen en de rol die is toegewezen aan elke groep. In ons voorbeeld, de lijst met groepen worden gesynchroniseerd vanuit de Azure AD-beveiligingsgroepen die is opgegeven in de **gebruikersgroep openen** eigenschap van de instellingen van het domein van het cluster. Zie [een HDInsight-cluster maken met ESP ingeschakeld](./domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-a-hdinsight-cluster-with-esp).

    ![Rollen lijstweergave - groepen](./media/hdinsight-authorize-users-to-ambari/roles-list-view-groups.png)

    In de bovenstaande afbeelding is de groep 'hiveusers' toegewezen de *Clustergebruiker* rol. Dit is een alleen-lezen-rol waarmee de gebruikers van die groep te bekijken, maar niet wijzigen-configuraties en cluster metrische gegevens.

## <a name="log-in-to-ambari-as-a-view-only-user"></a>Als een alleen-lezen-gebruiker aanmelden bij de Ambari

We hebben onze Azure AD-domeingebruiker "hiveuser1" machtigingen toegewezen aan weergaven Hive en Tez. Wanneer we starten van de Ambari-Webinterface en voer de domeinreferenties van deze gebruiker (Azure AD de gebruikersnaam in de e-mailindeling en wachtwoord), wordt de gebruiker wordt omgeleid naar de pagina met Ambari-weergaven. Hier, kan de gebruiker een toegankelijk weergave selecteren. De gebruiker kan niet gaat u naar een ander deel van de site, met inbegrip van de pagina dashboard, services, hosts, waarschuwingen of beheerder.

![Gebruiker met alleen weergaven](./media/hdinsight-authorize-users-to-ambari/user-views-only.png)

## <a name="log-in-to-ambari-as-a-cluster-user"></a>Meld u aan bij de Ambari als de clustergebruiker van een

We hebben onze Azure AD-gebruiker van het domein 'hiveuser2' toegewezen aan de *Clustergebruiker* rol. Deze rol is toegang tot het dashboard en alle menu-items. Een cluster heeft minder toegestane opties dan een beheerder. Bijvoorbeeld: hiveuser2 configuraties voor elk van de services kunt bekijken, maar niet bewerken.

![Gebruiker met de Cluster-gebruikersrol](./media/hdinsight-authorize-users-to-ambari/user-cluster-user-role.png)

## <a name="next-steps"></a>Volgende stappen

* [Apache Hive-beleid configureren in HDInsight met ESP](./domain-joined/apache-domain-joined-run-hive.md)
* [ESP HDInsight-clusters beheren](./domain-joined/apache-domain-joined-manage.md)
* [De Apache Hive-weergave gebruiken met Apache Hadoop in HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [Synchroniseren van Azure AD-gebruikers aan het cluster](hdinsight-sync-aad-users-to-cluster.md)
