---
title: De beveiliging van gegevens die zijn opgeslagen in Azure Data Lake Store | Microsoft Docs
description: Informatie over het beveiligen van gegevens in Azure Data Lake Store met behulp van groepen en toegangsbeheerlijsten
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: e246abaf8d1d7939765e1c878f6dfaf6375294f6
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>De beveiliging van gegevens die zijn opgeslagen in Azure Data Lake Store
Gegevens beveiligen in Azure Data Lake Store is een benadering drie stappen.

1. Begint met het maken van beveiligingsgroepen in Azure Active Directory (AAD). Deze beveiligingsgroepen worden gebruikt voor het implementeren van op rollen gebaseerde toegangsbeheer (RBAC) in Azure-portal. Zie voor meer informatie [toegangsbeheer op basis van rollen in Microsoft Azure](../active-directory/role-based-access-control-configure.md).
2. De AAD-beveiligingsgroepen toewijzen aan het Azure Data Lake Store-account. Controleert de toegang voor het Data Lake Store-account van de portal en beheerbewerkingen zijn van de portal of API's.
3. De AAD-beveiligingsgroepen toewijzen als toegang toegangsbeheerlijsten (ACL's) op het Data Lake Store-bestandssysteem.
4. Bovendien kunt u ook een IP-adresbereik instellen voor clients die toegang de gegevens in Data Lake Store tot.

In dit artikel bevat instructies over het gebruik van de Azure-portal de bovenstaande taken uitvoeren. Zie voor gedetailleerde informatie over hoe Data Lake Store implementeert voor beveiliging op het niveau van het account en gegevens [beveiliging in Azure Data Lake Store](data-lake-store-security-overview.md). Zie voor informatie over hoe ACL's worden geïmplementeerd in Azure Data Lake Store dieper [overzicht van toegangsbeheer in Data Lake Store](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Beveiligingsgroepen maken in Azure Active Directory
Zie voor instructies over het maken van AAD-beveiligingsgroepen en gebruikers toevoegen aan de groep, [beheren in Azure Active Directory-beveiligingsgroepen](../active-directory/active-directory-groups-create-azure-portal.md).

> [!NOTE] 
> U kunt zowel gebruikers als andere groepen toevoegen aan een groep in Azure AD met behulp van de Azure-portal. Echter gebruiken om een service-principal toevoegt aan een groep, [Azure AD PowerShell-module](../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Gebruikers of beveiligingsgroepen toewijzen aan Azure Data Lake Store-accounts
Als u gebruikers of beveiligingsgroepen op Azure Data Lake Store-accounts toewijst, kunt u toegang tot de beheerbewerkingen op het account met de Azure-portal en Azure Resource Manager-API's beheren. 

1. Open een Azure Data Lake Store-account. Klik in het linkerdeelvenster op **Bladeren**, klikt u op **Data Lake Store**, en klik vervolgens op de accountnaam die u wilt een gebruiker of beveiligingsgroep groep toewijzen aan de blade Data Lake Store.

2. Klik in de instellingenblade van Data Lake Store-account op **Access Control (IAM)**. De blade door standaardlijsten **abonnementsbeheerders** als eigenaar van een groep.
   
    ![Beveiligingsgroep toewijzen aan Azure Data Lake Store-account](./media/data-lake-store-secure-data/adl.select.user.icon.png "beveiligingsgroep toewijzen aan Azure Data Lake Store-account")

    Er zijn twee manieren aan een groep toevoegen en relevante rollen toewijzen.
   
    * Een gebruiker of groep toevoegen aan het account en vervolgens een rol toewijzen of
    * Een functie toevoegen en vervolgens gebruikers/groepen toewijzen aan rollen.
     
    In dit gedeelte kijken we de eerste benadering een groep toe te voegen en vervolgens het toewijzen van rollen. U kunt vergelijkbare stappen voor het eerst een rol selecteren en vervolgens groepen toewijzen aan die rol uitvoeren.
4. In de **gebruikers** blade, klikt u op **toevoegen** openen de **toegang toevoegen** blade. In de **toegang toevoegen** blade, klikt u op **Selecteer een rol**, en selecteer vervolgens een rol voor de gebruiker of groep.
   
    ![Toevoegen van een rol voor de gebruiker](./media/data-lake-store-secure-data/adl.add.user.1.png "een rol voor de gebruiker toevoegen")
   
    De **eigenaar** en **Inzender** rol voor toegang tot een aantal functies voor beheer op de data lake-account. Voor gebruikers die met de gegevens in de data lake communiceren, u kunt deze toevoegen aan de ** lezer ** rol. Het bereik van deze rollen is beperkt tot de beheerbewerkingen die betrekking hebben op het Azure Data Lake Store-account.
   
    Voor gegevens definiëren operations afzonderlijke bestandssysteemmachtigingen wat de gebruikers kunnen doen. Daarom een gebruiker met een lezersrol kunt beheerinstellingen voor het account alleen weergeven, maar kan mogelijk gegevens lezen en schrijven op basis van bestandssysteemmachtigingen aan hen toegewezen. Data Lake Store-bestandssysteemmachtigingen wordt beschreven op [beveiligingsgroep toewijzen als ACL's naar het bestandssysteem van Azure Data Lake Store](#filepermissions).
5. In de **toegang toevoegen** blade, klikt u op **gebruikers toevoegen** openen de **gebruikers toevoegen** blade. In deze blade, zoekt u de beveiligingsgroep die u eerder in Azure Active Directory gemaakt. Als u een groot aantal groepen om te zoeken vanaf hebt, gebruikt u in het tekstvak boven kunt u filteren op naam van de groep. Klik op **Selecteren**.
   
    ![Een beveiligingsgroep toevoegt](./media/data-lake-store-secure-data/adl.add.user.2.png "beveiligingsgroep toevoegen")
   
    Als u toevoegen van een groep/gebruiker die niet wordt vermeld wilt, kunt u ze uitnodigen met behulp van de **uitnodigen** pictogram en het e-mailadres voor de gebruikersgroep op te geven.
6. Klik op **OK**. Hier ziet u de beveiligingsgroep toegevoegd zoals hieronder wordt weergegeven.
   
    ![Beveiligingsgroep toegevoegd](./media/data-lake-store-secure-data/adl.add.user.3.png "beveiligingsgroep toegevoegd")

7. De gebruiker/beveiligingsgroep heeft nu toegang tot het Azure Data Lake Store-account. Als u toegang bieden tot specifieke gebruikers wilt, kunt u ze kunt toevoegen aan de beveiligingsgroep. Op dezelfde manier als u wilt toegang voor een gebruiker in te trekken, kunt u deze uit de beveiligingsgroep. U kunt ook meerdere beveiligingsgroepen toewijzen aan een account. 

## <a name="filepermissions"></a>Gebruikers- of beveiligingsgroep als ACL's toewijzen aan het bestandssysteem van Azure Data Lake Store
Gebruiker/beveiligingsgroepen aan het bestandssysteem van Azure Data Lake toewijst, kunt u toegangsbeheer voor gegevens die zijn opgeslagen in Azure Data Lake Store instellen.

1. Klik op de blade van het Data Lake Store-account op **Gegevensverkenner**.
   
    ![Mappen maken in Data Lake Store-account](./media/data-lake-store-secure-data/adl.start.data.explorer.png "mappen maken in Data Lake-account")
2. In de **Data Explorer** blade, klikt u op het bestand of map waarvoor u wilt de ACL configureren en klik vervolgens op **toegang**. Als u wilt toewijzen ACL naar een bestand, klikt u op **toegang** van de **bestandsvoorbeeld** blade.
   
    ![ACL's ingesteld op Data Lake-bestandssysteem](./media/data-lake-store-secure-data/adl.acl.1.png "ACL's ingesteld op Data Lake-bestandssysteem")
3. De **toegang** blade bevat de standaard toegang en aangepaste toegang is al toegewezen aan de hoofdmap. Klik op de **toevoegen** pictogram Aangepast niveau ACL's toevoegen.
   
    ![Standaard- en aangepaste toegang lijst](./media/data-lake-store-secure-data/adl.acl.2.png "lijst standaard en aangepaste toegang")
   
   * **Standaard toegang** is de toegang van de UNIX-stijl, waarbij u lezen opgeeft, schrijven, uitvoeren (rwx) op drie verschillende gebruikersklassen: eigenaar, groep en anderen.
   * **Aangepaste toegang** komt overeen met de POSIX-ACL's waarmee u kunt machtigingen instellen voor specifieke benoemde gebruikers of groepen, en niet alleen het bestand eigenaar of groep. 
     
     Zie voor meer informatie [HDFS-ACL's](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Zie voor meer informatie over hoe de ACL's worden geïmplementeerd in Data Lake Store, [toegangsbeheer in Data Lake Store](data-lake-store-access-control.md).
4. Klik op de **toevoegen** pictogram openen de **aangepaste toegang toevoegen** blade. Klik op deze blade **gebruiker of groep selecteren**, en klik vervolgens in **gebruiker of groep selecteren** blade, zoekt u de beveiligingsgroep die u eerder in Azure Active Directory gemaakt. Als u een groot aantal groepen om te zoeken vanaf hebt, gebruikt u in het tekstvak boven kunt u filteren op naam van de groep. Klik op de groep die u wilt toevoegen en klik vervolgens op **Selecteer**.
   
    ![Een groep toevoegen](./media/data-lake-store-secure-data/adl.acl.3.png "een groep toevoegen")
5. Klik op **Selecteer machtigingen**, selecteer de machtigingen en of u de machtigingen toewijzen als een standaard ACL wilt, toegang krijgen tot ACL of beide. Klik op **OK**.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-secure-data/adl.acl.4.png "machtigingen toewijzen aan de groep")
   
    Zie voor meer informatie over de machtigingen in Data Lake Store en/toegang ACL's, [toegangsbeheer in Data Lake Store](data-lake-store-access-control.md).
6. In de **aangepaste toegang toevoegen** blade, klikt u op **OK**. De toegevoegde groep met de bijbehorende machtigingen wordt nu weergegeven in de **toegang** blade.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-secure-data/adl.acl.5.png "machtigingen toewijzen aan de groep")
   
   > [!IMPORTANT]
   > In de huidige release kunt u slechts 9 vermeldingen onder hebt **aangepaste toegang**. Als u wilt meer dan 9-gebruikers toevoegen, moet u beveiligingsgroepen maken gebruikers toevoegen aan beveiligingsgroepen, het toevoegen van de beveiligingsgroepen die toegang bieden voor het Data Lake Store-account.
   > 
   > 
7. Indien nodig, kunt u ook de machtigingen wijzigen nadat u de groep hebt toegevoegd. Schakel het selectievakje voor elk machtigingstype (lezen, schrijven, uitvoeren) op basis van of u wilt verwijderen of deze machtiging toewijzen aan de beveiligingsgroep of uit. Klik op **opslaan** de wijzigingen wilt opslaan of **negeren** om de wijzigingen ongedaan te maken.

## <a name="set-ip-address-range-for-data-access"></a>Instellen van IP-adresbereik voor toegang tot gegevens
Azure Data Lake Store kunt u verdere vergrendelen toegang tot uw data store op netwerkniveau. U kunt firewall inschakelen, Geef een IP-adres of een IP-adresbereik voor de vertrouwde clients te definiëren. Eenmaal is ingeschakeld, worden alleen clients waarvoor de IP-adressen binnen het gedefinieerde bereik kunnen verbinden met de store.

![Firewall-instellingen en IP-toegang tot](./media/data-lake-store-secure-data/firewall-ip-access.png "Firewall-instellingen en IP-adres")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Beveiligingsgroepen voor een Azure Data Lake Store-account verwijderen
Wanneer u beveiligingsgroepen uit Azure Data Lake Store-accounts verwijderen, wijzigt u alleen toegang aan de beheerbewerkingen op het account met de Azure Portal en Azure Resource Manager-API's.

1. Klik in de blade van het Data Lake Store-account op **instellingen**. Van de **instellingen** blade, klikt u op **gebruikers**.
   
    ![Beveiligingsgroep toewijzen aan Azure Data Lake-account](./media/data-lake-store-secure-data/adl.select.user.icon.png "beveiligingsgroep toewijzen aan Azure Data Lake-account")
2. In de **gebruikers** blade klikt u op de beveiligingsgroep die u wilt verwijderen.
   
    ![Beveiligingsgroep verwijderen](./media/data-lake-store-secure-data/adl.add.user.3.png "beveiligingsgroep verwijderen")
3. Klik op de blade voor de beveiligingsgroep **verwijderen**.
   
    ![Beveiligingsgroep verwijderd](./media/data-lake-store-secure-data/adl.remove.group.png "beveiligingsgroep verwijderd")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Verwijderen van beveiligingsgroep ACL's van Azure Data Lake Store-bestandssysteem
Wanneer u beveiligingsgroepen ACL's van Azure Data Lake Store-bestandssysteem verwijdert, wijzigt u de toegang tot de gegevens in de Data Lake Store.

1. Klik op de blade van het Data Lake Store-account op **Gegevensverkenner**.
   
    ![Mappen maken in Data Lake-account](./media/data-lake-store-secure-data/adl.start.data.explorer.png "mappen maken in Data Lake-account")
2. In de **Data Explorer** blade, klikt u op het bestand of map die u wilt verwijderen van de ACL, en klik vervolgens op de blade van uw account, klikt u op de **toegang** pictogram. Als u wilt verwijderen ACL voor een bestand, klikt u op **toegang** van de **bestandsvoorbeeld** blade.
   
    ![ACL's ingesteld op Data Lake-bestandssysteem](./media/data-lake-store-secure-data/adl.acl.1.png "ACL's ingesteld op Data Lake-bestandssysteem")
3. In de **toegang** blade van de **aangepaste toegang** sectie, klikt u op de beveiligingsgroep die u wilt verwijderen. In de **aangepaste toegang** blade, klikt u op **verwijderen** en klik vervolgens op **OK**.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-secure-data/adl.remove.acl.png "machtigingen toewijzen aan de groep")

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Aan de slag met Data Lake Store met behulp van PowerShell](data-lake-store-get-started-powershell.md)
* [Aan de slag met Data Lake Store met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Diagnostische logboeken openen voor Data Lake Store](data-lake-store-diagnostic-logs.md)

