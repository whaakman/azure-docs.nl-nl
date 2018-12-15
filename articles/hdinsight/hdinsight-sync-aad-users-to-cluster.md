---
title: Synchronisatie van Azure Active Directory-gebruikers in een cluster - Azure HDInsight
description: Geverifieerde gebruikers van Azure Active Directory naar een cluster synchroniseert.
services: hdinsight
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: mamccrea
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: ebb1e3614309f92fc21442100a13c53291b3acbb
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407284"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Synchronisatie van Azure Active Directory-gebruikers met een HDInsight-cluster

[HDInsight-clusters met Enterprise Security Package (ESP)](hdinsight-domain-joined-introduction.md) kunt sterke verificatie gebruiken met Azure Active Directory (Azure AD)-gebruikers, evenals gebruiken *op rollen gebaseerd toegangsbeheer* (RBAC)-beleidsregels. Als u gebruikers en groepen aan Azure AD toevoegen, kunt u de gebruikers die toegang nodig tot uw cluster kunt synchroniseren.

## <a name="prerequisites"></a>Vereisten

Als u hebt nog niet gedaan, [een HDInsight-cluster maken met Enterprise-beveiligingspakket](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Toevoegen van nieuwe Azure AD-gebruikers

Voor de hosts, opent u de Ambari-Webgebruikersinterface. Elk knooppunt wordt bijgewerkt met nieuwe zonder toezicht upgrade-instellingen.

1. In de [Azure-portal](https://portal.azure.com), Ga naar de Azure AD-map die is gekoppeld aan uw ESP-cluster.

2. Selecteer **alle gebruikers** in het menu links, selecteer **nieuwe gebruiker**.

    ![Deelvenster voor alle gebruikers](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Voltooi het gebruikersformulier van de nieuwe. Selecteer de groepen die u voor het toewijzen van machtigingen op basis van een cluster hebt gemaakt. In dit voorbeeld maakt u een groep met de naam 'HiveUsers', waarmee u nieuwe gebruikers kunt toewijzen. De [voorbeeld instructies](hdinsight-domain-joined-configure.md) voor het maken van een cluster ESP omvatten het toevoegen van twee groepen `HiveUsers` en `AAD DC Administrators`.

    ![Nieuwe gebruiker-deelvenster](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. Selecteer **Maken**.

## <a name="use-the-apache-ambari-rest-api-to-synchronize-users"></a>De Apache Ambari REST-API gebruiken om gebruikers te synchroniseren

Gebruikersgroepen die zijn opgegeven tijdens het maken van het cluster worden gesynchroniseerd op dat moment. Gebruikerssynchronisatie automatisch één keer per uur uitgevoerd. De gebruikers onmiddellijk synchroniseren, of om te synchroniseren van een groep dan de groepen die zijn opgegeven tijdens het maken, gebruikt u de Ambari REST-API.

De volgende methode maakt gebruik van POST met de Ambari REST-API. Zie voor meer informatie, [beheren HDInsight-clusters met behulp van de Apache Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [Verbinding maken met uw cluster met SSH](hdinsight-hadoop-linux-use-ssh-unix.md). Selecteer in het overzichtsvenster voor uw cluster in Azure portal, de **Secure Shell (SSH)** knop.

    ![Secure Shell (SSH)](./media/hdinsight-sync-aad-users-to-cluster/ssh.png)

2. Kopieer de weergegeven `ssh` opdracht en plak deze in de SSH-client. Voer de ssh gebruikerswachtwoord wanneer hierom wordt gevraagd.

3. Na verificatie, voer de volgende opdracht:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    Het antwoord moet er als volgt:

    ```json
    {
      "resources" : [
        {
          "href" : "http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

4. Als u wilt zien van de synchronisatiestatus, uitvoeren van een nieuwe `curl` opdracht:

    ```bash
    curl -u admin:<YOUR PASSWORD> https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events/1
    ```
    
    Het antwoord moet er als volgt:
    
    ```json
    {
      "href" : "http://hn0-hadoop.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
      "Event" : {
        "id" : 1,
        "specs" : [
          {
            "sync_type" : "existing",
            "principal_type" : "groups"
          }
        ],
        "status" : "COMPLETE",
        "status_detail" : "Completed LDAP sync.",
        "summary" : {
          "groups" : {
            "created" : 0,
            "removed" : 0,
            "updated" : 0
          },
          "memberships" : {
            "created" : 1,
            "removed" : 0
          },
          "users" : {
            "created" : 1,
            "removed" : 0,
            "skipped" : 0,
            "updated" : 0
          }
        },
        "sync_time" : {
          "end" : 1497994072182,
          "start" : 1497994071100
        }
      }
    }
    ```

5. Dit resultaat geeft aan dat de status is **voltooid**, een nieuwe gebruiker is gemaakt en een lidmaatschap aan de gebruiker is toegewezen. In dit voorbeeld wordt de gebruiker is toegewezen aan de 'HiveUsers' gesynchroniseerd LDAP-groep, omdat de gebruiker is toegevoegd aan die dezelfde groep in Azure AD.

> [!NOTE]  
> Alleen synchroniseert de Azure AD-beveiligingsgroepen die is opgegeven in de vorige methode de **gebruikersgroep openen** eigenschap van de domeininstellingen tijdens het maken van clusters. Zie voor meer informatie, [maken van een HDInsight-cluster](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Controleer of het zojuist toegevoegde Azure AD-gebruiker

Open de [Apache Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md) om te controleren of de nieuwe Azure AD-gebruiker is toegevoegd. Toegang tot de Ambari-Webgebruikersinterface door te bladeren naar **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`**. Voer de gebruikersnaam van de cluster-beheerder en het wachtwoord.

1. Selecteer in het dashboard Ambari **beheren Ambari** onder de **admin** menu.

    ![Ambari beheren](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Selecteer **gebruikers** onder de **gebruiker en groepsbeheer** menugroep aan de linkerkant van de pagina.

    ![Menu-item voor gebruikers](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. De nieuwe gebruiker moet worden vermeld in de tabel-gebruikers. Het Type is ingesteld op `LDAP` in plaats van `Local`.

    ![Pagina gebruikers](./media/hdinsight-sync-aad-users-to-cluster/users.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Als de nieuwe gebruiker aanmelden bij de Ambari

Als de nieuwe gebruiker (of een andere domeingebruiker) zich bij de Ambari aanmeldt, gebruiken ze hun Azure AD-gebruiker en het domein-referenties.  Ambari geeft een gebruikersalias, de weergavenaam van de gebruiker in Azure AD is. De nieuwe van de voorbeeldgebruiker heeft de naam van de gebruiker `hiveuser3@contoso.com`. In de Ambari, deze nieuwe gebruiker wordt weergegeven als `hiveuser3` , maar de gebruiker zich aanmeldt Ambari als `hiveuser3@contoso.com`.

## <a name="see-also"></a>Zie ook

* [Apache Hive-beleid configureren in HDInsight met ESP](hdinsight-domain-joined-run-hive.md)
* [HDInsight-clusters met ESP beheren](hdinsight-domain-joined-manage.md)
* [Toestaan dat gebruikers Apache Ambari](hdinsight-authorize-users-to-ambari.md)
