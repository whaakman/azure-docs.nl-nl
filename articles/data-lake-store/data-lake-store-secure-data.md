---
title: Beveiligen van gegevens die zijn opgeslagen in Azure Data Lake Storage Gen1 | Microsoft Docs
description: Meer informatie over het beveiligen van gegevens in Azure Data Lake Storage Gen1 met behulp van groepen en toegangsbeheerlijsten
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: cebdff5ed233516683df3330e8fd3332ded664e5
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885669"
---
# <a name="securing-data-stored-in-azure-data-lake-storage-gen1"></a>Het beveiligen van gegevens die zijn opgeslagen in Azure Data Lake Storage Gen1
Gegevens beveiligen in Azure Data Lake Storage Gen1 is een benadering drie stappen.  Beide op rollen gebaseerd toegangsbeheer (RBAC) en toegangsbeheerlijsten (ACL's) moeten worden ingesteld op volledig toegang tot gegevens voor gebruikers en beveiligingsgroepen in te schakelen.

1. Beginnen met het maken van beveiligingsgroepen in Azure Active Directory (AAD). Deze beveiligingsgroepen worden gebruikt voor het implementeren van op rollen gebaseerd toegangsbeheer (RBAC) in Azure portal. Zie voor meer informatie, [Role-based Access Control in Microsoft Azure](../role-based-access-control/role-assignments-portal.md).
2. De AAD-beveiligingsgroepen toewijzen aan de Gen1 van Data Lake Storage-account. Hiermee bepaalt u toegang tot aan het Gen1 van Data Lake Storage-account van de portal en beheerbewerkingen uit de portal of API's.
3. De AAD-beveiligingsgroepen toewijzen toegangsbeheerlijsten (ACL's) op het bestandssysteem van Data Lake Storage Gen1.
4. U kunt daarnaast ook een IP-adresbereik voor clients die toegang de gegevens in Data Lake Storage Gen1 tot krijgen instellen.

In dit artikel bevat instructies over het gebruik van de Azure-portal de bovenstaande taken uit te voeren. Zie voor gedetailleerde informatie over hoe beveiliging op het niveau van het account en gegevens in Data Lake Storage Gen1 wordt geïmplementeerd, [beveiliging in Azure Data Lake Storage Gen1](data-lake-store-security-overview.md). Zie voor uitgebreide informatie over hoe ACL's zijn geïmplementeerd in Data Lake Storage Gen1, [overzicht van Access Control in Data Lake Storage Gen1](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **De account van een Data Lake Storage Gen1**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Beveiligingsgroepen maken in Azure Active Directory
Zie voor instructies over het maken van AAD-beveiligingsgroepen en gebruikers toevoegen aan de groep, [beheren van beveiligingsgroepen in Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

> [!NOTE] 
> U kunt zowel gebruikers- en andere groepen toevoegen aan een groep in Azure AD met behulp van de Azure portal. Echter, als u wilt een service-principal toevoegen aan een groep gebruiken [van Azure AD PowerShell-module](../active-directory/users-groups-roles/groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-data-lake-storage-gen1-accounts"></a>Gebruikers of beveiligingsgroepen toewijzen aan Gen1 van Data Lake Storage-accounts
Wanneer u gebruikers of beveiligingsgroepen aan Gen1 van Data Lake Storage-accounts toewijst, kunt u toegang tot de bewerkingen voor het account met de Azure portal en Azure Resource Manager-API's beheren. 

1. Open een Gen1 van Data Lake Storage-account. Klik in het linkerdeelvenster op **alle resources**, en klik in de blade alle resources op de accountnaam die u wilt toewijzen van een gebruiker of gebruikersgroep.

2. Klik in de blade van het Data Lake Storage Gen1-account op **Access Control (IAM)**. De blade standaard bevat de eigenaars van abonnementen als de eigenaar.
   
    ![Beveiligingsgroep toewijzen aan Azure Data Lake Storage Gen1 account](./media/data-lake-store-secure-data/adl.select.user.icon1.png "beveiligingsgroep toewijzen aan Azure Data Lake Storage Gen1-account")

3. In de **Access Control (IAM)** blade, klikt u op **toevoegen** openen de **machtigingen toevoegen** blade. In de **machtigingen toevoegen** blade, selecteer een **rol** voor de gebruikersgroep. Zoek naar de beveiligingsgroep die u eerder in Azure Active Directory gemaakt en selecteer deze. Als u een groot aantal gebruikers en groepen te zoeken vanaf hebt, gebruikt de **Selecteer** in het tekstvak om te filteren op naam van de groep. 
   
    ![Toevoegen van een rol voor de gebruiker](./media/data-lake-store-secure-data/adl.add.user.1.png "een rol voor de gebruiker toevoegen")
   
    De **eigenaar** en **Inzender** rol voor toegang tot een verscheidenheid aan functies voor beheer op de data lake-account. Voor gebruikers die wordt interactie met gegevens in data lake, maar nog steeds nodig om beheergegevens account weer te geven, kunt u toevoegen aan de **lezer** rol. Het bereik van deze rollen is beperkt tot de beheerbewerkingen die betrekking hebben op de Gen1 van Data Lake Storage-account.
   
    Voor gegevensbewerkingen bepalen afzonderlijke machtigingen voor het bestandssysteem wat de gebruikers kunnen doen. Daarom een gebruiker met een rol van lezer kunt beheerinstellingen die zijn gekoppeld aan het account alleen weergeven, maar kan mogelijk gegevens lezen en schrijven op basis van machtigingen voor het bestandssysteem aan hen toegewezen. Data Lake Storage Gen1 bestandssysteemmachtigingen worden beschreven op [toewijzen beveiligingsgroep als ACL's naar het bestandssysteem van Azure Data Lake Storage Gen1](#filepermissions).

    > [!IMPORTANT]
    > Alleen de **eigenaar** rol kunnen automatisch toegang tot bestandssysteem. De **Inzender**, **lezer**, en alle andere functies vereisen ACL's om in te schakelen van elk niveau van toegang tot bestanden en mappen.  De **eigenaar** rol biedt supergebruiker bestands- en mapmachtigingen die via de ACL's kunnen niet worden overschreven. Zie voor meer informatie over hoe RBAC-beleidsregels worden toegewezen aan toegang tot gegevens, [RBAC voor accountbeheer](data-lake-store-security-overview.md#rbac-for-account-management).

4. Als u toevoegen van een groep/gebruiker die niet wordt vermeld wilt in de **machtigingen toevoegen** blade kunt u ze uitnodigen door hun e-mailadres in de **Selecteer** tekstvak in en klik vervolgens in de lijst te selecteren.
   
    ![Toevoegen van een beveiligingsgroep](./media/data-lake-store-secure-data/adl.add.user.2.png "voegt u een beveiligingsgroep")
   
5. Klik op **Opslaan**. Hier ziet u de beveiligingsgroep toegevoegd zoals hieronder wordt weergegeven.
   
    ![Beveiligingsgroep toegevoegd](./media/data-lake-store-secure-data/adl.add.user.3.png "beveiligingsgroep toegevoegd")

6. De gebruiker/groep heeft nu toegang tot het Data Lake Storage Gen1-account. Als u toegang bieden aan specifieke gebruikers wilt, kunt u ze kunt toevoegen aan de beveiligingsgroep. Op dezelfde manier als u wilt toegang voor een gebruiker in te trekken, kunt u ze verwijderen uit de beveiligingsgroep. U kunt ook meerdere beveiligingsgroepen toewijzen aan een account. 

## <a name="filepermissions"></a>Gebruikers of beveiligingsgroepen als ACL's toewijzen aan het bestandssysteem van Data Lake Storage Gen1
Gebruikers/beveiligingsgroepen aan het bestandssysteem van Data Lake Storage Gen1 toewijst, moet u toegangsbeheer voor de gegevens die zijn opgeslagen in Data Lake Storage Gen1 instellen.

1. Klik in de blade van het Data Lake Storage Gen1-account op **Data Explorer**.
   
    ![Inzien via Data Explorer](./media/data-lake-store-secure-data/adl.start.data.explorer.png "inzien via Data Explorer")
2. In de **Data Explorer** blade, klikt u op de map waarvoor u wilt de ACL configureren, en klik vervolgens op **toegang**. Als u wilt toewijzen ACL's naar een bestand, moet u eerst het bestand om te bekijken en klik vervolgens op klikken **toegang** uit de **bestandsvoorbeeld** blade.
   
    ![ACL's ingesteld op Data Lake Storage Gen1 file system](./media/data-lake-store-secure-data/adl.acl.1.png "ACL's ingesteld op Data Lake Storage Gen1 file system")
3. De **toegang** blade geeft een lijst van de eigenaars en machtigingen die zijn al toegewezen aan de hoofdmap. Klik op de **toevoegen** pictogram voor het toevoegen van extra toegangs-ACL's.
    > [!IMPORTANT]
    > Instellen van machtigingen voor toegang voor één bestand verleent per se een gebruiker of groep toegang tot dit bestand. Het pad naar het bestand moet toegankelijk zijn voor de toegewezen gebruiker/groep. Zie voor meer informatie en voorbeelden, [algemene scenario's met betrekking tot machtigingen](data-lake-store-access-control.md#common-scenarios-related-to-permissions).
   
    ![Lijst met standaardentiteiten en aangepaste toegang](./media/data-lake-store-secure-data/adl.acl.2.png "lijst met standaardentiteiten en aangepaste toegang")
   
   * De **eigenaren** en **van iedereen** bieden toegang tot UNIX-stijl, waarin u opgeeft lezen, schrijven, uitvoeren (LSU) op drie verschillende gebruikersklassen: eigenaar, groep en anderen.
   * **Machtigingen toegewezen** komt overeen met de POSIX ACL's waarmee u kunt machtigingen instellen voor specifieke benoemde gebruikers of groepen buiten de eigenaar van het bestand of de groep. 
     
     Zie voor meer informatie, [HDFS-ACL's](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Zie voor meer informatie over hoe ACL's zijn geïmplementeerd in Data Lake Storage Gen1 [Access Control in Data Lake Storage Gen1](data-lake-store-access-control.md).
4. Klik op de **toevoegen** pictogram opent de **machtigingen toe te wijzen** blade. Klik op deze blade **gebruiker of groep selecteren**, en klik vervolgens in **gebruiker of groep selecteren** blade, zoekt u de beveiligingsgroep die u eerder in Azure Active Directory gemaakt. Als u een groot aantal groepen om te zoeken naar van hebt, gebruikt u in het tekstvak aan de bovenkant om te filteren op naam van de groep. Klik op de groep die u wilt toevoegen en klik vervolgens op **Selecteer**.
   
    ![Een groep toevoegen](./media/data-lake-store-secure-data/adl.acl.3.png "een groep toevoegen")
5. Klik op **machtigingen selecteren**, selecteer de machtigingen, of de machtigingen moeten worden toegepast op recursief en of u wilt de machtigingen toewijzen als een toegang ACL, standaard-ACL of beide. Klik op **OK**.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-secure-data/adl.acl.4.png "machtigingen toewijzen aan de groep")
   
    Zie voor meer informatie over de machtigingen in Data Lake Storage Gen1 en standaard/toegangs-ACL's, [Access Control in Data Lake Storage Gen1](data-lake-store-access-control.md).
6. Nadat u hebt geklikt **Ok** in de **machtigingen selecteren** blade, de nieuwe groep en de bijbehorende machtigingen wordt nu weergegeven in de **toegang** blade.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-secure-data/adl.acl.5.png "machtigingen toewijzen aan de groep")
   
   > [!IMPORTANT]
   > In de huidige versie, hebt u maximaal 28 vermeldingen onder **machtigingen toegewezen**. Als u wilt meer dan 28 gebruikers toevoegen, moet u beveiligingsgroepen maken gebruikers toevoegen aan beveiligingsgroepen, toe te voegen toegang bieden aan de beveiligingsgroepen voor het Data Lake Storage Gen1-account.
   > 
   > 
7. Indien nodig, kunt u ook de machtigingen wijzigen nadat u de groep hebt toegevoegd. Schakel het selectievakje voor elk machtigingstype (lezen, schrijven, uitvoeren) op basis van of u wilt verwijderen of deze machtiging toewijzen aan de beveiligingsgroep of uit. Klik op **opslaan** de wijzigingen op te slaan of **negeren** om de wijzigingen ongedaan te maken.

## <a name="set-ip-address-range-for-data-access"></a>IP-adresbereik voor gegevenstoegang instellen
Data Lake Storage Gen1 kunt u verder het vergrendelen van toegang tot uw gegevensopslag op netwerkniveau. U kunt firewall inschakelen, Geef een IP-adres of een IP-adresbereik voor de vertrouwde clients definiëren. Eenmaal is ingeschakeld, worden alleen clients met de IP-adressen binnen het gedefinieerde bereik kunnen verbinden met de store.

![Firewall-instellingen en IP-toegang hebben tot](./media/data-lake-store-secure-data/firewall-ip-access.png "Firewall-instellingen en IP-adres")

## <a name="remove-security-groups-for-a-data-lake-storage-gen1-account"></a>Beveiligingsgroepen voor een Data Lake Storage Gen1-account verwijderen
Wanneer u beveiligingsgroepen van Data Lake Storage Gen1 accounts verwijdert, wijzigt u alleen toegang aan de bewerkingen voor het account met de Azure Portal en Azure Resource Manager-API's.  

Toegang tot gegevens is niet gewijzigd en wordt nog steeds beheerd door de toegang tot de ACL's.  De uitzondering hierop zijn de gebruikers/groepen in de rol van eigenaar.  Gebruikers/groepen verwijderd uit de rol eigenaar zijn niet langer Supergebruikers en hun toegang terugvalt op toegang ACL-instellingen. 

1. Klik in de blade van het Data Lake Storage Gen1-account op **Access Control (IAM)**. 
   
    ![Beveiligingsgroep toewijzen aan Gen1 van Data Lake Storage-account](./media/data-lake-store-secure-data/adl.select.user.icon.png "beveiligingsgroep toewijzen aan Gen1 van Data Lake Storage-account")
2. In de **Access Control (IAM)** blade, klikt u op de beveiliging-groepen die u wilt verwijderen. Klik op **Verwijderen**.
   
    ![Beveiligingsgroep verwijderd](./media/data-lake-store-secure-data/adl.remove.group.png "beveiligingsgroep verwijderd")

## <a name="remove-security-group-acls-from-a-data-lake-storage-gen1-file-system"></a>Beveiligingsgroep ACL's van een bestandssysteem voor Data Lake Storage Gen1 verwijderen
Wanneer u beveiligingsgroep ACL's van een bestandssysteem voor Data Lake Storage Gen1 verwijdert, moet u toegang tot de gegevens in het Data Lake Storage Gen1-account wijzigen.

1. Klik in de blade van het Data Lake Storage Gen1-account op **Data Explorer**.
   
    ![Mappen maken in Data Lake Storage Gen1 account](./media/data-lake-store-secure-data/adl.start.data.explorer.png "mappen maken in Data Lake Storage Gen1-account")
2. In de **Data Explorer** blade, klikt u op de map waarvoor u wilt de ACL verwijderen en klik vervolgens op **toegang**. Als u wilt verwijderen van ACL's voor een bestand, moet u eerst het bestand om te bekijken en klik vervolgens op klikken **toegang** uit de **bestandsvoorbeeld** blade. 
   
    ![ACL's ingesteld op Data Lake Storage Gen1 file system](./media/data-lake-store-secure-data/adl.acl.1.png "ACL's ingesteld op Data Lake Storage Gen1 file system")
3. In de **toegang** blade, klikt u op de beveiligingsgroep die u wilt verwijderen. In de **toegang tot gegevens** blade, klikt u op **verwijderen**.
   
    ![Machtigingen toewijzen aan de groep](./media/data-lake-store-secure-data/adl.remove.acl.png "machtigingen toewijzen aan de groep")

## <a name="see-also"></a>Zie ook
* [Overzicht van Azure Data Lake Storage Gen1](data-lake-store-overview.md)
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Aan de slag met Data Lake Storage Gen1 met behulp van PowerShell](data-lake-store-get-started-powershell.md)
* [Aan de slag met Data Lake Storage Gen1 met .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Diagnostische logboeken openen voor Data Lake Storage Gen1](data-lake-store-diagnostic-logs.md)

