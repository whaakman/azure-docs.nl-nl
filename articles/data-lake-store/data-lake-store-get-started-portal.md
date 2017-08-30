---
title: Aan de slag met Azure Data Lake Store met Azure Portal | Microsoft Docs
description: Azure Portal gebruiken om een Data Lake Store-account te maken en basisbewerkingen in Data Lake Store uit te voeren
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: fea324d0-ad1a-4150-81f0-8682ddb4591c
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: fa13266993017374ba49709f8e22fbe6b03a28c7
ms.contentlocale: nl-nl
ms.lasthandoff: 08/07/2017

---
# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Aan de slag met Azure Data Lake Store met Azure Portal
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET-SDK](data-lake-store-get-started-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Lees hoe u met Azure Portal een Azure Data Lake Store-account maakt en basisbewerkingen uitvoert, zoals het maken van mappen, uploaden en downloaden van gegevensbestanden, verwijderen van uw account, enzovoort. Zie [Overzicht van Azure Data Lake Store](data-lake-store-overview.md) voor meer informatie.

De volgende twee video's bevatten dezelfde informatie als beschreven in dit artikel:

* [Een Data Lake Store-account maken](https://mix.office.com/watch/1k1cycy4l4gen)
* [Gegevens in Data Lake Store beheren met de Data Explorer](https://mix.office.com/watch/icletrxrh6pc)

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account maken

1. Meld u aan bij de nieuwe [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw**, klik op **Gegevens en opslag** en klik vervolgens op **Azure Data Lake Store**. Lees de informatie op de blade **Azure Data Lake Store** en klik vervolgens op **Maken** in de linkerbenedenhoek van het blad.
3. In de blade **Nieuwe Data Lake Store** geeft u de waarden op zoals u ze in de volgende schermafbeelding ziet:
   
    ![Een nieuw Azure Data Lake Store-account maken](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Een nieuw Azure Data Lake Store-account maken")
   
   * **Naam**. Voer een unieke naam in voor het Data Lake Store-account.
   * **Abonnement**. Selecteer het abonnement waarmee u een nieuw Data Lake Store-account wilt maken.
   * **Resourcegroep**. Selecteer een bestaande resourcegroep of selecteer de optie **Nieuwe maken** om er een te maken. Een resourcegroep is een container met verwante resources voor een toepassing. Zie [Resourcegroepen in Azure](../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie.
   * **Locatie**: selecteer een locatie waar u het Data Lake Store-account wilt maken.
   * **Versleutelingsinstellingen**. Er zijn drie opties:
     
     * **Geen versleuteling inschakelen**.
     * **Sleutels gebruiken die worden beheerd door Azure Data Lake**.  Als u wilt dat Azure Data Lake Store uw versleutelingssleutels beheert.
     * **Sleutels kiezen in Azure Key Vault**. U kunt een bestaande Azure Key Vault selecteren of een nieuwe Key Vault maken. Als u de sleutels uit een Key Vault wilt gebruiken, moet u machtigingen voor het Azure Data Lake Store-account toewijzen voor toegang tot de Azure Key Vault. Zie [Machtigingen toewijzen aan Azure Key Vault](#assign-permissions-to-azure-key-vault) voor instructies.
       
        ![Versleuteling van Data Lake Store](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Versleuteling van Data Lake Store")
       
        Klik op **OK** in de blade **Versleutelingsinstellingen**.

        Zie [Gegevens versleutelen in Azure Data Lake Store](./data-lake-store-encryption.md)voor meer informatie.

4. Klik op **Create**. Als u ervoor kiest om het account vast te maken aan het dashboard, gaat u terug naar het dashboard en ziet u de voortgang van de inrichting van uw Data Lake Store-account. Wanneer het Data Lake Store-account is ingericht, wordt de accountblade weergegeven.

U kunt ook een Data Lake Store-account maken met behulp van Azure Resource Manager-sjablonen. Deze sjablonen zijn toegankelijk vanaf [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=data+lake+store):

- Zonder gegevensversleuteling: [Azure Data Lake Store-account implementeren zonder gegevensversleuteling](https://azure.microsoft.com/en-us/resources/templates/101-data-lake-store-no-encryption/).
- Met gegevensversleuteling met behulp van Data Lake Store: [Data Lake Store-account implementeren met versleuteling (Data Lake)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
- Met gegevensversleuteling met behulp van Azure Key Vault: [Data Lake Store-account implementeren met versleuteling (Key Vault)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).

### <a name="assign-permissions-to-azure-key-vault"></a>Machtigingen toewijzen aan Azure Key Vault
Als u de sleutels uit een Azure Key Vault hebt gebruikt om versleuteling voor het Data Lake Store-account te configureren, moet u de toegang tussen het Azure Data Lake Store-account en de Azure Key Vault configureren. Volg hiervoor de volgende stappen.

1. Als u sleutels uit de Azure Key Vault hebt gebruikt, wordt boven aan de blade voor het Data Lake Store-account een waarschuwing weergegeven. Klik op de waarschuwing om de blade **Key Vault-machtigingen configureren** te openen.
   
    ![Versleuteling van Data Lake Store](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Versleuteling van Data Lake Store")
2. Op de blade ziet u twee opties om toegang te configureren.
   
   * Klik in de eerste optie op **Toestemming geven** om toegang te configureren. De eerste optie is alleen ingeschakeld als de gebruiker die het Data Lake Store-account heeft gemaakt ook een beheerder is voor de Azure Key Vault.
   * De andere optie is om de PowerShell-cmdlet uit te voeren die op de blade wordt weergegeven. U moet de eigenaar van de Azure Key Vault zijn of zijn gemachtigd om machtigingen voor de Azure Key Vault te verlenen. Wanneer u de cmdlet hebt uitgevoerd, keert u terug naar de blade en klikt u op **Inschakelen** om toegang te configureren.

## <a name="createfolder"></a>Mappen maken in Azure Data Lake Store-account
U kunt onder uw Data Lake Store-account mappen maken om gegevens te beheren en op te slaan.

1. Open het Data Lake Store-account dat u hebt gemaakt. Klik in het linkerdeelvenster op **Bladeren**, klik op **Data Lake Store** en klik vervolgens op de blade Data Lake Store op de accountnaam waaronder u mappen wilt maken. Als u het account hebt vastgemaakt aan het startboard, klikt u op de tegel voor dat account.
2. Klik op de blade van het Data Lake Store-account op **Gegevensverkenner**.
   
    ![Mappen maken in het Data Lake Store-account](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Mappen maken in het Data Lake Store-account")
3. Klik op de blade van het Data Lake Store-account op **Nieuwe map**, voer een naam in voor de nieuwe map en klik op **OK**.
   
    ![Mappen maken in het Data Lake Store-account](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Mappen maken in het Data Lake Store-account")
   
    De zojuist gemaakte map wordt vermeld op de blade **Data Explorer**. U kunt geneste mappen maken tot elk gewenst niveau.
   
    ![Mappen maken in het Data Lake-account](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Mappen maken in het Data Lake-account")

## <a name="uploaddata"></a>Gegevens uploaden naar Azure Data Lake Store-account
Als u gegevens uploadt naar een Azure Data Lake Store-account, kunt u dat direct naar het hoofdniveau doen of naar een map die u in het account hebt gemaakt. Volg de stappen in onderstaande schermafbeelding om een bestand vanuit de blade **Data Explorer** te uploaden naar een submap. In deze schermafbeelding is het bestand geüpload naar een submap die wordt weergegeven in de breadcrumbs (in een rood kader gemarkeerd).

Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken.

![Gegevens uploaden](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Gegevens uploaden")

## <a name="properties"></a>Eigenschappen en acties die beschikbaar zijn voor de opgeslagen gegevens
Klik op het zojuist toegevoegde bestand om de blade **Eigenschappen** te openen. De eigenschappen van het bestand en de acties die u met het bestand kunt uitvoeren, zijn beschikbaar in deze blade. U kunt ook het volledige pad naar het bestand kopiëren in uw Azure Data Lake Store-account. Dit is in onderstaande schermafbeelding met een rood kader gemarkeerd:

![Eigenschappen van de gegevens](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Eigenschappen van de gegevens")

* Klik op **Voorbeeld** als u rechtstreeks vanuit de browser een voorbeeld van het bestand wilt bekijken. U kunt ook de indeling van het voorbeeld opgeven. Klik op **Voorbeeld**, klik op de blade **Bestandsvoorbeeld** op **Indeling** en geef op de blade **Indeling bestandsvoorbeeld** opties op als het aantal rijen dat u wilt weergeven, de codering die u wilt gebruiken, het gewenste scheidingsteken, enzovoort.
  
  ![Indeling van het bestandsvoorbeeld](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Indeling van het bestandsvoorbeeld")
* Klik op **Downloaden** om het bestand naar uw computer te downloaden.
* Klik op **Bestandsnaam wijzigen** als u de naam van het bestand wilt wijzigen.
* Klik op **Bestand verwijderen** als u het bestand wilt verwijderen.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
U kunt de gegevens die zijn opgeslagen in uw Azure Data Lake Store-account beveiligen met Azure Active Directory en toegangsbeheer (ACL's). Als u wilt weten hoe u dat doet, raadpleeg dan [Gegevens beveiligen in Azure Data Lake Store](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Azure Data Lake Store-account verwijderen
Als u een Azure Data Lake Store-account wilt verwijderen, klikt u in de Data Lake Store-blade op **Verwijderen**. Om de actie te bevestigen, wordt u gevraagd de naam in te voeren van het account dat u wilt verwijderen. Voer de naam in van het account en klik op **Verwijderen**.

![Data Lake-account verwijderen](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Data Lake-account verwijderen")

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Diagnostische logboeken openen voor Data Lake Store](data-lake-store-diagnostic-logs.md)


