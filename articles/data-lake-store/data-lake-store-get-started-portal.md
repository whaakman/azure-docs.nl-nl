---
title: Gebruik Azure-portal aan de slag met Azure Data Lake Storage Gen1 | Microsoft Docs
description: Gebruik de Azure-portal een Azure Data Lake Storage Gen1-account maakt en basisbewerkingen uitvoert, in het Data Lake Storage Gen1-account.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: da238a30a55f37607abb3cfe63c3ba8f33ca2bf2
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125167"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-the-azure-portal"></a>Aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure portal

> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
>
> 

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Informatie over het gebruik van de Azure-portal een Azure Data Lake Storage Gen1-account maakt en basisbewerkingen uitvoert zoals zoals het maken van mappen, uploaden en downloaden van gegevensbestanden, verwijderen van uw account, enzovoort. Zie voor meer informatie, [overzicht van Azure Data Lake Storage Gen1](data-lake-store-overview.md).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-data-lake-storage-gen1-account"></a>Een Data Lake Storage Gen1-account maken

1. Meld u aan bij de nieuwe [Azure Portal](https://portal.azure.com).
2. Klik op **een resource maken > opslag > Data Lake Storage Gen1**.
3. In de **nieuwe Data Lake Storage Gen1** blade, geef de waarden op zoals weergegeven in de volgende schermafbeelding:
   
    ![Maak een nieuw account voor Data Lake Storage Gen1](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "een nieuw Data Lake Storage Gen1-account maken")
   
   * **Naam**. Voer een unieke naam voor de Gen1 van Data Lake Storage-account.
   * **Abonnement**. Selecteer het abonnement waarin u wilt een nieuw Data Lake Storage Gen1-account maken.
   * **Resourcegroep**. Selecteer een bestaande resourcegroep of selecteer de optie **Nieuwe maken** om er een te maken. Een resourcegroep is een container met verwante resources voor een toepassing. Zie [Resourcegroepen in Azure](../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie.
   * **Locatie**: Selecteer een locatie waar u de Gen1 van Data Lake Storage-account maken.
   * **Versleutelingsinstellingen**. Er zijn drie opties:
     
     * **Geen versleuteling inschakelen**.
     * **Gebruik van sleutels die worden beheerd door Data Lake Storage Gen1**, als u wilt dat Data Lake Storage Gen1 uw versleutelingssleutels wilt beheren.
     * **Sleutels gebruiken uit uw eigen Key Vault**. U kunt een bestaande Azure Key Vault selecteren of een nieuwe Key Vault maken. Voor het gebruik van de sleutels uit een Key Vault, moet u machtigingen voor het Data Lake Storage Gen1-account voor toegang tot de Azure Key Vault toewijzen. Zie [Machtigingen toewijzen aan Azure Key Vault](#assign-permissions-to-azure-key-vault) voor instructies.
       
        ![Versleuteling van Data Lake Storage Gen1](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Gen1 van Data Lake Storage-versleuteling")
       
        Klik op **OK** in de blade **Versleutelingsinstellingen**.

        Zie voor meer informatie, [versleuteling van gegevens in Azure Data Lake Storage Gen1](./data-lake-store-encryption.md).

4. Klik op **Create**. Als u ervoor kiest om het account aan het dashboard vast te maken, gaat u terug naar het dashboard en u kunt de voortgang van het inrichten van uw Data Lake Storage Gen1 bekijken. Zodra de Gen1 van Data Lake Storage-account is ingericht, wordt de accountblade weergegeven.

## <a name="assign-permissions-to-azure-key-vault"></a>Machtigingen toewijzen aan Azure Key Vault
Als u sleutels uit een Azure Key Vault gebruikt om versleuteling te configureren op de Gen1 van Data Lake Storage-account, moet u de toegang tussen de Data Lake Storage Gen1 en de Azure Key Vault-account configureren. Volg hiervoor de volgende stappen.

1. Als u sleutels uit de Azure Key Vault gebruikt, wordt in de blade voor het Data Lake Storage Gen1-account wordt een waarschuwing weergegeven aan de bovenkant. Klik op de waarschuwing om **Versleuteling** te openen.
   
    ![Versleuteling van Data Lake Storage Gen1](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Gen1 van Data Lake Storage-versleuteling")
2. Op de blade ziet u twee opties om toegang te configureren.

    ![Versleuteling van Data Lake Storage Gen1](./media/data-lake-store-get-started-portal/adls-encryption-4.png "Gen1 van Data Lake Storage-versleuteling")
   
   * Klik in de eerste optie op **Machtigingen verlenen** om toegang te configureren. De eerste optie is alleen ingeschakeld wanneer de gebruiker die het Gen1 van Data Lake Storage-account hebt gemaakt, ook een beheerder is voor de Azure Key Vault is.
   * De andere optie is om de PowerShell-cmdlet uit te voeren die op de blade wordt weergegeven. U moet de eigenaar van de Azure Key Vault zijn of zijn gemachtigd om machtigingen voor de Azure Key Vault te verlenen. Wanneer u de cmdlet hebt uitgevoerd, keert u terug naar de blade en klikt u op **Inschakelen** om toegang te configureren.

> [!NOTE]
> U kunt ook een Gen1 van Data Lake Storage-account met behulp van Azure Resource Manager-sjablonen maken. Deze sjablonen zijn toegankelijk vanaf [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/?term=data+lake+store):
    - Zonder gegevensversleuteling: [implementeren Azure Data Lake Storage Gen1 account zonder gegevensversleuteling](https://azure.microsoft.com/resources/templates/101-data-lake-store-no-encryption/).
    - Met gegevensversleuteling met behulp van Data Lake Storage Gen1: [account van de Data Lake Storage Gen1 implementeren met versleuteling (Data Lake)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
    - Met gegevensversleuteling met behulp van Azure Key Vault: [implementeren Data Lake Storage Gen1-account met versleuteling (Key Vault)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).
> 
> 



## <a name="createfolder"></a>Mappen maken in een Data Lake Storage Gen1-account
U kunt mappen maken onder uw Data Lake Storage Gen1-account te beheren en opslaan van gegevens.

1. Open het Data Lake Storage Gen1-account dat u hebt gemaakt. Klik in het linkerdeelvenster op **Alle resources** en klik in de blade Alle resources op de accountnaam waaronder u mappen wilt maken. Als u het account hebt vastgemaakt aan het startboard, klikt u op de tegel voor dat account.
2. Klik in de blade van het Data Lake Storage Gen1-account op **Data Explorer**.
   
    ![Mappen maken in een Data Lake Storage Gen1 account](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "mappen maken in een Data Lake Storage Gen1-account")
3. Klik op de blade Data Explorer op **Nieuwe map**, voer een naam in voor de nieuwe map en klik op **OK**.
   
    ![Mappen maken in een Data Lake Storage Gen1 account](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "mappen maken in een Data Lake Storage Gen1-account")
   
    De zojuist gemaakte map wordt vermeld op de blade **Data Explorer**. U kunt geneste mappen maken tot elk gewenst niveau.
   
    ![Mappen maken in een Data Lake-account](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "mappen maken in een Data Lake-account")

## <a name="uploaddata"></a>Gegevens uploaden naar een Gen1 van Data Lake Storage-account
U kunt uw gegevens uploaden naar een Gen1 van Data Lake Storage-account rechtstreeks naar het hoogste niveau doen of naar een map die u in het account hebt gemaakt. 

1. Klik op de blade **Data Explorer** op **Uploaden**. 
2. Navigeer op de blade **Bestanden uploaden** naar de bestanden die u wilt uploaden en klik op **Geselecteerde bestanden toevoegen**. U kunt meer dan één bestand selecteren om te uploaden.

    ![Gegevens uploaden](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Gegevens uploaden")

Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken.

## <a name="properties"></a>Acties die beschikbaar zijn voor de opgeslagen gegevens
Klik op het beletselteken bij een bestand en klik in het pop-upmenu op de actie die u wilt uitvoeren met de gegevens.

![Eigenschappen van de gegevens](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Eigenschappen van de gegevens") 

## <a name="secure-your-data"></a>Uw gegevens beveiligen
U kunt de gegevens die zijn opgeslagen in uw Data Lake Storage Gen1-account met behulp van Azure Active Directory en access control (ACL's) kunt beveiligen. Zie voor instructies over hoe u dat doet, [gegevens beveiligen in Azure Data Lake Storage Gen1](data-lake-store-secure-data.md).

## <a name="delete-a-data-lake-storage-gen1-account"></a>Een Data Lake Storage Gen1-account verwijderen
Als een Gen1 van Data Lake Storage-account verwijderen uit de blade Data Lake Storage Gen1, klikt u op **verwijderen**. Om de actie te bevestigen, wordt u gevraagd de naam in te voeren van het account dat u wilt verwijderen. Voer de naam in van het account en klik op **Verwijderen**.

![Gen1 van Data Lake Storage-account verwijderen](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Data Lake-account verwijderen")

## <a name="next-steps"></a>Volgende stappen
* [Azure Data Lake Storage Gen1 gebruiken voor big data-vereisten](data-lake-store-data-scenarios.md) 
* [Gegevens beveiligen in Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)

