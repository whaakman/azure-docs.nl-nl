---
title: Synchronisatie van Azure Active Directory-gebruikers naar een cluster - Azure HDInsight | Microsoft Docs
description: Geverifieerde gebruikers van Azure Active Directory naar een cluster worden gesynchroniseerd.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: ad1586a6e358dfb1ca2391474ecdd9bee2f6226d
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Synchronisatie van Azure Active Directory-gebruikers met een HDInsight-cluster

[HDInsight-clusters domein](hdinsight-domain-joined-introduction.md) kunt sterke verificatie gebruiken met Azure Active Directory (Azure AD)-gebruikers, alsmede gebruiken *toegangsbeheer op basis van rollen* (RBAC)-beleid. Als u gebruikers en groepen aan Azure AD toevoegen, kunt u de gebruikers die toegang nodig tot het cluster kunt synchroniseren.

## <a name="prerequisites"></a>Vereisten

Als u nog niet hebt gedaan, [maken van een domein HDInsight-cluster](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Toevoegen van nieuwe Azure AD-gebruikers

Om uw hosts bekijken, opent u de Ambari-Webgebruikersinterface. Elk knooppunt wordt bijgewerkt met nieuwe upgrade zonder toezicht-instellingen.

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de Azure AD-directory die is gekoppeld aan uw domein-cluster.

2. Selecteer **alle gebruikers** in het menu links, selecteer **nieuwe gebruiker**.

    ![Deelvenster voor alle gebruikers](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Vul het gebruikersformulier met nieuwe. Selecteer de groepen die u hebt gemaakt voor het toewijzen van machtigingen op basis van het cluster. In dit voorbeeld maakt u een groep met de naam 'HiveUsers', waarmee u nieuwe gebruikers kunt toewijzen. De [Voorbeeldinstructies](hdinsight-domain-joined-configure.md) voor het maken van een cluster domein omvatten het toevoegen van twee groepen `HiveUsers` en `AAD DC Administrators`.

    ![Nieuwe gebruiker deelvenster](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. Selecteer **Maken**.

## <a name="use-the-ambari-rest-api-to-synchronize-users"></a>De Ambari REST-API gebruiken om gebruikers te synchroniseren

Gebruikersgroepen die zijn opgegeven tijdens het maken van het cluster worden gesynchroniseerd op dat moment. Gebruikerssynchronisatie automatisch één keer per uur uitgevoerd. De gebruikers direct synchroniseren, of om te synchroniseren van een andere groep dan de groepen die zijn opgegeven tijdens het maken, gebruikt u de Ambari REST-API.

De volgende methode gebruikt een bericht met de Ambari REST-API. Zie voor meer informatie [HDInsight-clusters beheren met behulp van de Ambari REST-API](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [Verbinding maken met uw cluster met SSH](hdinsight-hadoop-linux-use-ssh-unix.md). Selecteer in het overzichtsvenster voor uw cluster in de Azure portal, de **Secure Shell (SSH)** knop.

    ![Secure Shell (SSH)](./media/hdinsight-sync-aad-users-to-cluster/ssh.png)

2. Kopieer de weergegeven `ssh` opdracht en plak deze in uw SSH-client. Voer de ssh gebruikerswachtwoord wanneer u wordt gevraagd.

3. Als de verificatie is gelukt, voer de volgende opdracht:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    Het antwoord ziet er als volgt:

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

4. Overzicht van de synchronisatiestatus van het uitvoeren van een nieuwe `curl` opdracht, met behulp van de `href` waarde geretourneerd van de vorige opdracht:

    ```bash
    curl -u admin:<YOUR PASSWORD> http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1
    ```
    
    Het antwoord ziet er als volgt:
    
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

5. Dit resultaat wordt aangegeven dat de status **COMPLETE**, een nieuwe gebruiker is gemaakt en de gebruiker een lidmaatschap is toegewezen. In dit voorbeeld wordt de gebruiker is toegewezen aan de 'HiveUsers' gesynchroniseerd LDAP-groep, omdat de gebruiker is toegevoegd aan die groep in Azure AD.

> [!NOTE]
> De vorige methode synchroniseert alleen de Azure AD-groepen die is opgegeven in de **gebruikersgroep toegang** eigenschap van de domeininstellingen tijdens het maken van het cluster. Zie voor meer informatie [maken van een HDInsight-cluster](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Controleer of de zojuist toegevoegde Azure AD-gebruiker

Open de [Ambari-Webgebruikersinterface](hdinsight-hadoop-manage-ambari.md) om te controleren of de nieuwe Azure AD-gebruiker is toegevoegd. Toegang tot de Ambari-Webgebruikersinterface door te bladeren naar  **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** . Voer de gebruikersnaam voor de beheerder cluster en het wachtwoord.

1. Selecteer in het dashboard Ambari **beheren Ambari** onder de **admin** menu.

    ![Ambari beheren](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Selecteer **gebruikers** onder de **gebruikers- + groepsbeheer** menugroep aan de linkerkant van de pagina.

    ![Gebruikers menu-item](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. De nieuwe gebruiker moet worden vermeld in de tabel gebruikers. Het Type is ingesteld op `LDAP` plaats `Local`.

    ![Pagina gebruikers](./media/hdinsight-sync-aad-users-to-cluster/users.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Meld u aan bij Ambari als de nieuwe gebruiker

Wanneer de nieuwe gebruiker (of een andere domeingebruiker) meldt zich aan bij Ambari, ze hun volledige Azure AD-gebruiker en het domein-referenties gebruiken.  Ambari geeft een gebruikersalias, de weergavenaam van de gebruiker in Azure AD is. De nieuwe voorbeeld van de gebruiker heeft de gebruikersnaam `hiveuser3@contoso.com`. In de Ambari, deze nieuwe gebruiker wordt weergegeven als `hiveuser3` , maar de gebruiker zich aanmeldt bij Ambari als `hiveuser3@contoso.com`.

## <a name="see-also"></a>Zie ook

* [Op domein HDInsight Hive-beleid configureren](hdinsight-domain-joined-run-hive.md)
* [HDInsight-clusters domein beheren](hdinsight-domain-joined-manage.md)
* [Ambari gebruikers autoriseren](hdinsight-authorize-users-to-ambari.md)
