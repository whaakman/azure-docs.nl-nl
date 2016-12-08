---
title: Aan de slag met Data Lake Store | Microsoft Docs
description: De portal gebruiken om een Data Lake Store-account te maken en basisbewerkingen in Data Lake Store uit te voeren
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
ms.date: 11/21/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 7d02b9fd741310a39aaadee0358af6c7b5990cf2
ms.openlocfilehash: dfa7a41c7acd71eca39ac1360b1699bed9309e09


---
# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Aan de slag met Azure Data Lake Store met Azure Portal
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Lees hoe u met Azure Portal een Azure Data Lake Store-account maakt en basisbewerkingen uitvoert, zoals het maken van mappen, uploaden en downloaden van gegevensbestanden, verwijderen van uw account enzovoort. Zie [Overzicht van Azure Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake Store.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-faster-with-videos"></a>Leert u sneller met video's?
Bekijk dan de volgende video's om aan de slag te gaan met Data Lake Store.

* [Een Data Lake Store-account maken](https://mix.office.com/watch/1k1cycy4l4gen)
* [Gegevens in Data Lake Store beheren met de Data Explorer](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account maken
1. Meld u aan bij de nieuwe [Azure Portal](https://portal.azure.com).
2. Klik op **Nieuw**, klik op **Gegevens en opslag** en klik vervolgens op **Azure Data Lake Store**. Lees de informatie op de blade **Azure Data Lake Store** en klik vervolgens op **Maken** in de linkerbenedenhoek van het blad.
3. In de blade **Nieuwe Data Lake Store** geeft u de waarden op zoals u ze in onderstaande schermafbeelding ziet:
   
    ![Een nieuw Azure Data Lake Store-account maken](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Create a new Azure Data Lake account")
   
   * **Naam**. Voer een unieke naam in voor het Data Lake Store-account.
   * **Abonnement**. Selecteer het abonnement waarmee u een nieuw Data Lake Store-account wilt maken.
   * **Resourcegroep**. Selecteer een bestaande resourcegroep of selecteer de optie **Nieuwe maken** om er een te maken. Een resourcegroep is een container met verwante resources voor een toepassing. Zie [Resourcegroepen in Azure](../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie.
   * **Locatie**: selecteer een locatie waar u het Data Lake Store-account wilt maken.
   * **Versleutelingsinstellingen**. U kunt kiezen of u uw Data Lake Store-account wilt versleutelen. Als u wilt versleutelen, kunt u ook opgeven hoe u de hoofdversleutelingssleutel wilt beheren die u voor het versleutelen van de gegevens in uw account wilt gebruiken.
     
     * (Optioneel) Selecteer **Versleuteling niet inschakelen** in de vervolgkeuzelijst als u versleuteling wilt uitschakelen.
     * (Standaard) Selecteer **Sleutels gebruiken die worden beheerd door Azure Data Lake** als u uw versleutelingssleutels wilt beheren met Azure Data Lake Store.
       
         ![Data Lake Store-versleuteling](./media/data-lake-store-get-started-portal/adls-encryption-1.png "Data Lake Store encryption")
     * (Optioneel) Selecteer **Sleutels kiezen uit Azure Key Vault** als u uw eigen sleutels wilt gebruiken die u in uw Azure Key Vault hebt opgeslagen. Met deze optie kunt u ook een Key Vault-account en sleutels maken als u er nog geen hebt.
       
         ![Data Lake Store-versleuteling](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Data Lake Store encryption")
       
       Klik op **OK** in de blade **Versleutelingsinstellingen**.
       
       > [!NOTE]
       > Als u de sleutels uit een Azure Key Vault gebruikt om versleuteling voor het Data Lake Store-account te configureren, moet u machtigingen voor het Azure Data Lake Store-account toewijzen voor toegang tot de Azure Key Vault. Voor meer informatie raadpleegt u [Machtigingen toewijzen aan de Azure Key Vault](#assign-permissions-to-the-azure-key-vault)
       > 
       > 
4. Klik op **Create**. Als u ervoor kiest om het account vast te maken aan het dashboard, gaat u terug naar het dashboard en ziet u de voortgang van de inrichting van uw Data Lake Store-account. Wanneer het Data Lake Store-account is ingericht, wordt de accountblade weergegeven.

## <a name="assign-permissions-to-the-azure-key-vault"></a>Machtigingen toewijzen aan de Azure Key Vault
Als u de sleutels uit een Azure Key Vault hebt gebruikt om versleuteling voor het Data Lake Store-account te configureren, moet u de toegang tussen het Azure Data Lake Store-account en de Azure Key Vault configureren. Volg hiervoor de volgende stappen.

1. Als u sleutels uit de Azure Key Vault hebt gebruikt, wordt boven aan de blade voor het Data Lake Store-account een waarschuwing weergegeven. Klik op de waarschuwing om de blade **Key Vault-machtigingen configureren** te openen.
   
    ![Data Lake Store-versleuteling](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Data Lake Store encryption")
2. Op de blade ziet u twee opties om toegang te configureren.
   
   * Klik in de eerste optie op **Toestemming geven** om toegang te configureren. De eerste optie is alleen ingeschakeld als de gebruiker die het Data Lake Store-account heeft gemaakt ook een beheerder is voor de Azure Key Vault.
   * De andere optie is om de PowerShell-cmdlet uit te voeren die op de blade wordt weergegeven. U moet de eigenaar van de Azure Key Vault zijn of zijn gemachtigd om machtigingen voor de Azure Key Vault te verlenen. Wanneer u de cmdlet hebt uitgevoerd, keert u terug naar de blade en klikt u op **Inschakelen** om toegang te configureren.

## <a name="a-namecreatefolderacreate-folders-in-azure-data-lake-store-account"></a><a name="createfolder"></a>Mappen maken in Azure Data Lake Store-account
U kunt onder uw Data Lake Store-account mappen maken om gegevens te beheren en op te slaan.

1. Open het Data Lake Store-account dat u zojuist hebt gemaakt. Klik in het linkerdeelvenster op **Bladeren**, klik op **Data Lake Store** en klik vervolgens op de blade Data Lake Store op de accountnaam waaronder u mappen wilt maken. Als u het account hebt vastgemaakt aan het startboard, klikt u op de tegel voor dat account.
2. Klik op de blade van het Data Lake Store-account op **Gegevensverkenner**.
   
    ![Mappen maken in Data Lake Store-account](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Create folders in Data Lake Store account")
3. Klik op de blade van het Data Lake Store-account op **Nieuwe map**, voer een naam in voor de nieuwe map en klik op **OK**.
   
    ![Mappen maken in Data Lake Store-account](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Create folders in Data Lake Store account")
   
    De zojuist gemaakte map wordt vermeld op de blade **Gegevensverkenner**. U kunt geneste mappen maken tot elk gewenst niveau.
   
    ![Mappen maken in Data Lake-account](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Create folders in Data Lake account")

## <a name="a-nameuploaddataaupload-data-to-azure-data-lake-store-account"></a><a name="uploaddata"></a>Gegevens uploaden naar Azure Data Lake Store-account
Als u gegevens uploadt naar een Azure Data Lake Store-account, kunt u dat direct naar het hoofdniveau doen of naar een map die u in het account hebt gemaakt. Volg de stappen in onderstaande schermafbeelding om een bestand vanuit de blade **Gegevensverkenner** te uploaden naar een submap. In deze schermafbeelding is het bestand geüpload naar een submap die wordt weergegeven in de breadcrumbs (in een rood kader gemarkeerd).

Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken.

![Gegevens uploaden](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload data")

## <a name="a-namepropertiesaproperties-and-actions-available-on-the-stored-data"></a><a name="properties"></a>Eigenschappen en acties die beschikbaar zijn voor de opgeslagen gegevens
Klik op het zojuist toegevoegde bestand om de blade **Eigenschappen** te openen. De eigenschappen van het bestand en de acties die u met het bestand kunt uitvoeren, zijn beschikbaar in deze blade. U kunt ook het volledige pad naar het bestand kopiëren in uw Azure Data Lake Store-account. Dit is in onderstaande schermafbeelding met een rood kader gemarkeerd.

![Eigenschappen van de gegevens](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Properties on the data")

* Klik op **Voorbeeld** als u rechtstreeks vanuit de browser een voorbeeld van het bestand wilt bekijken. U kunt ook de indeling van het voorbeeld opgeven. Klik op **Voorbeeld**, klik op de blade **Bestandsvoorbeeld** op **Indeling** en geef op de blade **Indeling bestandsvoorbeeld** opties op als het aantal rijen dat u wilt weergeven, de codering die u wilt gebruiken, het gewenste scheidingsteken, enzovoort.
  
  ![Indeling bestandsvoorbeeld](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "File preview format")
* Klik op **Downloaden** om het bestand naar uw computer te downloaden.
* Klik op **Bestandsnaam wijzigen** als u de naam van het bestand wilt wijzigen.
* Klik op **Bestand verwijderen** als u het bestand wilt verwijderen.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
U kunt de gegevens die zijn opgeslagen in uw Azure Data Lake Store-account beveiligen met Azure Active Directory en toegangsbeheer (ACL's). Als u wilt weten hoe u dat doet, raadpleeg dan [Gegevens beveiligen in Azure Data Lake Store](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Azure Data Lake Store-account verwijderen
Als u een Azure Data Lake Store-account wilt verwijderen, klikt u in de Data Lake Store-blade op **Verwijderen**. Om de actie te bevestigen, wordt u gevraagd de naam in te voeren van het account dat u wilt verwijderen. Voer de naam in van het account en klik op **Verwijderen**.

![Data Lake-account verwijderen](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Delete Data Lake account")

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Diagnostische logboeken openen voor Data Lake Store](data-lake-store-diagnostic-logs.md)




<!--HONumber=Nov16_HO4-->


