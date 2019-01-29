---
title: Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters
description: Informatie over het maken van Azure Data Lake Storage Gen2 met Azure HDInsight-clusters gebruiken.
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: howto
ms.date: 01/10/2019
ms.author: hrasheed
ms.openlocfilehash: 9a1d0775c12d424c35e9e9d366f69e07ec9b1468
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55096973"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters

Gen2 met Azure Data Lake Storage (Data Lake Storage) is een verscheidenheid aan functies die zijn toegewezen aan de analyse van big data, gebouwd op Azure Blob-opslag. Data Lake Storage Gen2 is het resultaat van het combineren van de mogelijkheden van Azure Blob storage en Azure Data Lake Storage Gen1. Het resultaat is een service die functies van Azure Data Lake Storage Gen1, zoals semantiek van het bestandssysteem, map en bestand niveau veiligheid en schaal samen met lage kosten, gelaagde opslag, mogelijkheden voor het herstel van hoge beschikbaarheid/noodherstel van Azure-Blob biedt opslag.

## <a name="data-lake-storage-gen2-availability"></a>Data Lake Storage Gen2 beschikbaarheid

Azure Data Lake Storage Gen2 is beschikbaar als een opslagoptie voor vrijwel alle Azure HDInsight-clustertypen als zowel standaard als een extra opslagaccount. HBase, maar kan slechts één Data Lake Storage Gen2 account hebben.

> [!Note] 
> Als u Data Lake Storage Gen2 als uw **primaire opslagtype**, u kunt een Gen1 van Data Lake Storage-account niet selecteren als extra opslag.

## <a name="creating-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Het maken van een HDInsight-cluster met Data Lake Storage Gen2

Voor het maken van een HDInsight-cluster, wat een Data Lake Storage Gen2 voor opslag gebruikt, gebruik de volgende stappen uit om te maken van een Data Lake Storage Gen2-account juist is geconfigureerd.

1. Maak een beheerde identiteit voor de gebruiker toegewezen, als u er nog geen hebt. Zie [maken, list, delete of wijs een rol aan een gebruiker toegewezen beheerde identiteit met Azure portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity).

    ![Een door de gebruiker toegewezen beheerde identiteit maken](./media/hdinsight-hadoop-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

1. Maak een Azure Data Lake Storage Gen2 storage-account. Zorg ervoor dat de **hiërarchisch bestandssysteem** optie is ingeschakeld. Zie [Quickstart: Maken van een storage-account van Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-quickstart-create-account.md) voor meer informatie.

    ![Schermafbeelding van de opslagaccount is gemaakt in Azure portal](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)
 
1. Toewijzen van de beheerde identiteit op de **Gegevensbijdrager voor Blob (Preview)** -rol op het storage-account. Zie [beheren toegangsrechten tot Azure BLOB Storage en Queue gegevens met RBAC (Preview)](../storage/common/storage-auth-aad-rbac.md#assign-a-role-scoped-to-the-storage-account-in-the-azure-portal)

    1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account.
    1. Selecteer uw storage-account en selecteer vervolgens **toegangsbeheer (IAM)** om instellingen voor toegangsbeheer voor het account weer te geven. Selecteer de **roltoewijzingen** tabblad om te bekijken van de lijst van roltoewijzingen.
    
        ![Schermafbeelding van de instellingen voor toegangsbeheer opslag](./media/hdinsight-hadoop-data-lake-storage-gen2/portal-access-control.png)
    
    1. Klik op de **roltoewijzing toevoegen** knop een nieuwe rol toe te voegen.
    1. In de **roltoewijzing toevoegen** venster de **Gegevensbijdrager voor Blob (Preview)** rol. Selecteer het abonnement dat de beheerde identiteits- en storage-account is. Vervolgens zoekt u naar de gebruiker toegewezen beheerde identiteit die u eerder hebt gemaakt. Selecteer ten slotte de beheerde identiteit en wordt weergegeven onder **geselecteerde leden**.
    
        ![Schermopname die laat zien hoe u een RBAC-rol toewijzen](./media/hdinsight-hadoop-data-lake-storage-gen2/add-rbac-role2.png)
    
    1. Klik op **Opslaan**. De gebruiker toegewezen identiteit die u hebt geselecteerd, wordt nu weergegeven onder de **Inzender** rol.

    1. Nadat de initiële installatie is voltooid, kunt u een cluster via de portal kunt maken. Het cluster moet zich in dezelfde Azure-regio als het opslagaccount. In de **opslag** sectie van het cluster maken-menu selecteert u de volgende opties:
        
        * Voor **primaire opslagtype**, klikt u op **Azure Data Lake Storage Gen2**.
        * Onder **selecteert u een Opslagaccount**, zoek en selecteer het zojuist gemaakte opslagaccount voor Data Lake Storage Gen2.
        
            ![Instellingen voor de opslag voor het gebruik van Data Lake Storage Gen2 met Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/primary-storage-type-adls-gen2.png)
        
        * Onder **identiteit** selecteert u het juiste abonnement en het zojuist gemaakte gebruiker toegewezen beheerde identiteit.
        
            ![Instellingen van de identiteit voor het gebruik van Data Lake Storage Gen2 met Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/managed-identity-cluster-creation.png)

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Toegangsbeheer voor Data Lake Storage Gen2 in HDInsight

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>Wat voor soort machtigingen biedt ondersteuning voor Data Lake Storage Gen2?

Azure Data Lake Storage Gen2 implementeert een model voor toegangsbeheer die ondersteuning biedt voor zowel Azure rollen gebaseerd toegangsbeheer (RBAC) en POSIX-achtige toegangsbeheerlijsten (ACL's). Data Lake toegangsbeheerlijsten Storage Gen1 alleen ondersteund voor het beheren van toegang tot gegevens.

Azure Role-based Access Control (RBAC) maakt gebruik van roltoewijzingen effectief sets machtigingen toepassen op gebruikers, groepen en service-principals voor Azure-resources. Deze Azure-resources worden normaal gesproken beperkt tot op het hoogste niveau van resources (bijvoorbeeld Azure Storage-accounts). Dit mechanisme is voor Azure Storage, en ook in Azure Data Lake Storage Gen2 uitgebreid naar het bestand system resource.

 Zie voor meer informatie over machtigingen voor bestanden met RBAC [Azure Role-based Access Control (RBAC)](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac).

Zie voor meer informatie over machtigingen voor bestanden met ACL's [toegangsbeheerlijsten voor bestanden en mappen](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).


### <a name="how-do-i-control-access-to-my-data-in-gen2"></a>Hoe beheers ik toegang tot mijn gegevens in Gen2?

De mogelijkheid voor uw HDInsight-cluster te krijgen tot bestanden in Data Lake Storage Gen2 wordt geregeld via beheerde identiteiten. Een beheerde identiteit is een identiteit die is geregistreerd bij Azure AD, waarvan de referenties worden beheerd door Azure. U hoeft niet te service-principals in Azure AD registreren en beheren van referenties, zoals certificaten.

Er zijn twee soorten beheerde identiteiten voor een Azure-services: systeem toegewezen en de gebruiker toegewezen. Azure HDInsight maakt gebruik van beheerde identiteiten voor toegang tot Azure Data Lake Storage Gen2 gebruiker toegewezen. Een gebruiker toegewezen beheerde identiteit wordt gemaakt als zelfstandige Azure-resource. Via een productieproces maakt Azure een identiteit in de Azure AD-tenant, die wordt vertrouwd door het gebruikte abonnement. Nadat de identiteit is gemaakt, kan deze worden toegewezen aan een of meer Azure-service-exemplaren. De levenscyclus van een door de gebruiker toegewezen identiteit wordt afzonderlijk beheerd van de levenscyclus van de Azure Service-exemplaren waaraan de identiteit is toegewezen. Zie voor meer informatie over beheerde identiteiten [hoe werkt de beheerde identiteit voor Azure-resources werk](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-using-hive-or-other-services"></a>Hoe stel ik machtigingen voor Azure AD-gebruikers om gegevens te doorzoeken in Data Lake Storage Gen2 met behulp van Hive of andere services?

Azure AD-beveiligingsgroepen gebruiken als de toegewezen principal in de ACL's. Geen individuele gebruikers of service-principals met toegangsmachtigingen voor bestanden rechtstreeks toewijzen. Wanneer u AD-beveiligingsgroepen gebruikt voor het beheren van de stroom van machtigingen, kunt u deze kunt toevoegen en verwijderen van gebruikers of service-principals zonder ACL's toepassen op een volledige mapstructuur. U hoeft alleen te toevoegen of verwijderen van de gebruikers van de juiste Azure AD-beveiligingsgroep. ACL's worden niet overgenomen en opnieuw toepassen van ACL's is vereist voor het bijwerken van de ACL voor elk bestand en de submap.

## <a name="next-steps"></a>Volgende stappen

* [Gen2 Preview van Azure Data Lake Storage gebruiken met Azure HDInsight-clusters](../storage/blobs/data-lake-storage-use-hdi-cluster.md)
* [Integratie van Azure HDInsight met Data Lake Storage Gen2 preview - ACL en beveiliging bijwerken](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)
* [Inleiding tot de preview van Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
