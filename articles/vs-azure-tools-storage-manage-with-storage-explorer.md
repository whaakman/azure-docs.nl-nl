---
title: Aan de slag met Storage Explorer | Microsoft Docs
description: Azure storage-resources beheren met Opslagverkenner
services: storage
author: cawaMS
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.date: 04/22/2019
ms.author: cawa
ms.openlocfilehash: f7dd6d3d30f34ba2c69b40111bb28d484ce572e7
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508735"
---
# <a name="get-started-with-storage-explorer"></a>Aan de slag met Storage Explorer

## <a name="overview"></a>Overzicht

Azure Storage Explorer is een zelfstandige app waarmee u eenvoudig met Azure Storage-gegevens kunt werken via Windows, macOS en Linux. In dit artikel leert u enkele manieren om verbinding te maken met en beheren van uw Azure storage-accounts.

![Microsoft Azure Storage Explorer][0]

## <a name="prerequisites"></a>Vereisten

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Azure Storage Explorer wordt ondersteund op de volgende versies van Windows:

* Windows 10 (aanbevolen)
* Windows 8
* Windows 7

Voor alle versies van Windows, .NET Framework 4.6.2 of hoger is vereist.

[Storage Explorer downloaden en installeren](https://www.storageexplorer.com)

# <a name="macostabmacos"></a>[MacOS](#tab/macos)

Azure Storage Explorer wordt op de volgende versies van macOS ondersteund:

* macOS 10.12 'Sierra' en latere versies

[Storage Explorer downloaden en installeren](https://www.storageexplorer.com)

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Azure Storage Explorer wordt ondersteund op de volgende distributies van Linux:

* Ubuntu 18.04 x64
* Ubuntu 16.04 x64
* Ubuntu 14.04 x64

Azure Storage Explorer werkt in een andere distributie, maar de enige die hierboven vermeld worden officieel ondersteund.

Zie voor meer informatie over Storage Explorer installeren op Linux, de [Troubleshooting Guide](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies).

Azure Storage Explorer [opmerkingen bij de Release](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) specifieke stappen voor het aantal distributies bevatten.

[Storage Explorer downloaden en installeren](https://www.storageexplorer.com)

---

## <a name="connect-to-a-storage-account-or-service"></a>Verbinding maken met een opslagaccount of -service

Storage Explorer biedt verschillende manieren om verbinding te maken met opslagaccounts. In het algemeen kunt u deze:

* [Aanmelden bij Azure voor toegang tot uw abonnementen en hun resources](#sign-in-to-azure)
* [Koppelen van een specifieke resource voor opslag of CosmosDB](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

> [!NOTE]
> Volledig na de aanmelding toegang krijgen tot bronnen, vereist Opslagverkenner management (ARM) en layer-machtigingen voor gegevens. Dit betekent dat u Azure AD-machtigingen die u toegang tot uw Storage-account, de containers in het account en de gegevens in de containers geven nodig hebt. Als u alleen machtigingen op het niveau van de gegevens hebt, kunt u overwegen [koppelen met Azure AD](#add-a-resource-via-azure-ad). Zie voor meer informatie over de exacte machtigingen Storage Explorer is vereist, de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#role-based-access-control-permission-issues).
>
>

1. Selecteer in Storage Explorer **Accounts beheren** naar de **Account Management Panel**.

    ![Accounts beheren][1]

2. In het linkerdeelvenster ziet u nu alle Azure accounts die u hebt aangemeld bij. Voor verbinding met een ander account, selecteert u **een account toevoegen**

3. Als u zich aanmelden bij een nationale cloud of een Azure Stack wilt, klikt u op de **Azure-omgeving** vervolgkeuzelijst te selecteren welke Azure-cloud u wilt gebruiken. Nadat u uw omgeving hebt gekozen, klikt u op de **aanmelden...**  knop. Als u met Azure Stack aanmeldt zich, Zie [Storage Explorer verbinden met een Azure Stack-abonnement](/azure-stack/user/azure-stack-storage-connect-se) voor meer informatie.

    ![Meld u aan optie][2]

4. Nadat u zich hebt aangemeld met een Azure-account, worden het account en de Azure-abonnementen die zijn gekoppeld aan dat account toegevoegd aan het linkerdeelvenster. Selecteer het Azure-abonnementen die u wilt werken en selecteer vervolgens **toepassen** (selecteren **alle abonnementen:** te selecteren selecteert u alle of geen van de Azure-abonnementen).

    ![Selecteer Azure-abonnementen][3]

    In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure-abonnementen zijn gekoppeld.

    ![Geselecteerde Azure-abonnementen][4]

### <a name="attach-a-specific-resource"></a>Een specifieke resource koppelen
    
Er zijn diverse opties voor het koppelen van een resource aan Storage Explorer. U kunt:

* [Een resource via Azure AD toevoegen](#add-a-resource-via-azure-ad): Als u alleen machtigingen op het niveau van de gegevens hebt, kunt u deze optie gebruiken om een Blob-container of een ADLS Gen2 Blob-container te voegen.
* [Gebruik een verbindingsreeks](#use-a-connection-string): Als u een verbindingsreeks naar een Opslagaccount hebt. Storage Explorer biedt ondersteuning voor zowel de sleutel en [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) verbindingsreeksen.
* [Een SAS-URI gebruiken](#use-a-sas-uri): Als u hebt een [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) URI naar een Blob-Container, -bestandsshare, wachtrij of tabel. Als u een SAS-URI, kunt u ofwel [Opslagverkenner](#generate-a-sas-in-storage-explorer) of de [Azure-portal](https://portal.azure.com).
* [Een naam en sleutel gebruiken](#use-a-name-and-key): Als u een van de sleutels naar uw Opslagaccount weet, kunt u deze optie om snel verbinding te maken. De sleutels voor uw Opslagaccount bevinden zich op het Opslagaccount **toegangssleutels** blade op de [Azure-portal](https://portal.azure.com).
* [Koppelen aan een lokale emulator](#attach-to-a-local-emulator): Als u een van de beschikbare Azure Storage-emulator gebruikt, moet u deze optie gebruiken eenvoudig verbinding maken met de emulator.
* [Verbinding maken met een Azure Cosmos DB-account met behulp van een verbindingsreeks](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): Als u een verbindingsreeks naar een cosmos DB-exemplaar hebt.
* [Verbinding maken met Azure Data Lake Store via een URI](#connect-to-azure-data-lake-store-by-uri): Als u een URI naar een Azure Data Lake Store.

#### <a name="add-a-resource-via-azure-ad"></a>Een resource via Azure AD toevoegen

1. Open de **dialoogvenster verbinding maken met** door te klikken op de **verbinding maken met knop** op de links, verticale werkbalk.

    ![Verbinding maken met de optie Azure Storage][9]

2. Als u hebt nog niet gedaan, gebruikt u de **een Azure-Account toevoegen** optie om aan te melden bij de Azure-Account die toegang tot de resource heeft. Nadat het aan te melden in het **dialoogvenster verbinding maken met**.

3. Selecteer de **toevoegen van een resource via Azure Active Directory (Azure AD)** optie en klik op **volgende**.

4. Selecteer het Azure-Account dat toegang tot de Storage-resource die u wilt koppelen en het abonnement waarin de resource zich heeft bevindt en klik vervolgens op **volgende**.

5. Kies het type resource dat u wilt koppelen, en voer vervolgens de gegevens die nodig zijn om verbinding te maken. De invoer op deze pagina verandert afhankelijk van het type resource dat u wilt toevoegen. Zorg ervoor dat u kiest u het juiste type resource. Nadat u hebt ingevuld, klikt u op de vereiste gegevens op **volgende**.

6. Bekijk de samenvatting verbinding en zorg ervoor dat alle informatie juist is. Als alle informatie die overeenkomt met uw wijzigingen klikt u op **Connect**, anders Ga terug naar de vorige pagina's met de **terug** knop eventuele onjuiste gegevens te corrigeren.

Zodra de verbinding is toegevoegd, wordt de resourcestructuur van de wordt automatisch navigeren naar het knooppunt van de verbinding. Als voor een of andere reden niet, kijk onder **lokaal en gekoppeld** → **Opslagaccounts** → **(Containers die zijn gekoppeld)** → **Blobcontainers** . Als u Storage Explorer kan niet worden toegevoegd, de verbinding is, of als u kan geen toegang tot uw gegevens na het toevoegen van de verbinding is, raadpleegt u de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor hulp.

#### <a name="use-a-connection-string"></a>Gebruik een verbindingsreeks

1. Open de **dialoogvenster verbinding maken met** door te klikken op de **verbinding maken met knop** op de links, verticale werkbalk.

    ![Verbinding maken met de optie Azure Storage][9]

2. Selecteer de **gebruik een verbindingsreeks** optie en klik op **volgende**.

3. Kies een weergavenaam voor uw verbinding en voer in de verbindingsreeks. Klik op **Volgende**.

4. Bekijk de samenvatting verbinding en zorg ervoor dat alle informatie juist is. Als alle informatie die overeenkomt met uw wijzigingen klikt u op **Connect**, anders Ga terug naar de vorige pagina's met de **terug** knop eventuele onjuiste gegevens te corrigeren.

Zodra de verbinding is toegevoegd, wordt de resourcestructuur van de wordt automatisch navigeren naar het knooppunt van de verbinding. Als voor een of andere reden niet, kijk onder **lokaal en gekoppeld** → **Opslagaccounts**. Als u Storage Explorer kan niet worden toegevoegd, de verbinding is, of als u kan geen toegang tot uw gegevens na het toevoegen van de verbinding is, raadpleegt u de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor hulp.

#### <a name="use-a-sas-uri"></a>Een SAS-URI gebruiken

1. Open de **dialoogvenster verbinding maken met** door te klikken op de **verbinding maken met knop** op de links, verticale werkbalk.

    ![Verbinding maken met de optie Azure Storage][9]

2. Selecteer de **gebruiken een shared access signature (SAS) URI** optie en klik op **volgende**.

3. Kies een weergavenaam voor uw verbinding en voer uw SAS-URI. Het service-eindpunt voor het type resource dat u wilt toevoegen, moet automatisch doorvoeren. Als u een aangepast eindpunt is het mogelijk is dat het niet mogelijk. Klik op **volgende**.

4. Bekijk de samenvatting verbinding en zorg ervoor dat alle informatie juist is. Als alle informatie die overeenkomt met uw wijzigingen klikt u op **Connect**, anders Ga terug naar de vorige pagina's met de **terug** knop eventuele onjuiste gegevens te corrigeren.

Zodra de verbinding is toegevoegd, wordt de resourcestructuur van de wordt automatisch navigeren naar het knooppunt van de verbinding. Als voor een of andere reden niet, kijk onder **lokaal en gekoppeld** → **Opslagaccounts** → **(Containers die zijn gekoppeld)** → **het serviceknooppunt voor het type het door u bijgevoegde container**. Als u Storage Explorer kan niet worden toegevoegd, de verbinding is, of als u kan geen toegang tot uw gegevens na het toevoegen van de verbinding is, raadpleegt u de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor hulp.

#### <a name="use-a-name-and-key"></a>Een naam en sleutel gebruiken

1. Open de **dialoogvenster verbinding maken met** door te klikken op de **verbinding maken met knop** op de links, verticale werkbalk.

    ![Verbinding maken met de optie Azure Storage][9]

2. Selecteer de **een opslagaccountnaam en -sleutel gebruiken** optie en klik op **volgende**.

3. Kies een weergavenaam voor uw verbinding.

4. Voer in de naam van uw Opslagaccount en een van de toegangssleutels ervan.

5. Kies de **opslagdomein** gebruiken en klik vervolgens op **volgende**.

4. Bekijk de samenvatting verbinding en zorg ervoor dat alle informatie juist is. Als alle informatie die overeenkomt met uw wijzigingen klikt u op **Connect**, anders Ga terug naar de vorige pagina's met de **terug** knop eventuele onjuiste gegevens te corrigeren.

Zodra de verbinding is toegevoegd, wordt de resourcestructuur van de wordt automatisch navigeren naar het knooppunt van de verbinding. Als voor een of andere reden niet, kijk onder **lokaal en gekoppeld** → **Opslagaccounts**. Als u Storage Explorer kan niet worden toegevoegd, de verbinding is, of als u kan geen toegang tot uw gegevens na het toevoegen van de verbinding is, raadpleegt u de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor hulp.

#### <a name="attach-to-a-local-emulator"></a>Koppelen aan een lokale emulator

Storage Explorer biedt ondersteuning voor emulators op alle platforms. De twee officiële beschikbaar emulators zijn momenteel:
* [Azure-opslagemulator](storage/common/storage-use-emulator.md) (alleen Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, macOS of Linux)

Als de emulator wordt uitgevoerd op de standaard-poorten kunt u de **Emulator - standaardpoorten** knooppunt, kunt u altijd vinden onder **lokaal en gekoppeld** → **Storage-Accounts** , snel toegang krijgt tot de emulator. Als u wilt een andere naam voor de verbinding gebruiken of als de emulator wordt niet uitgevoerd op de standaard-poorten, volgt u de onderstaande stappen.

1. Start de emulator. Wanneer u dit doet, neem notitie van welke poorten luistert de emulator op voor elk servicetype. U moet over deze informatie later opnieuw.

   > [!IMPORTANT]
   > Storage Explorer uw emulator niet automatisch wordt gestart. U moet deze zelf starten.

2. Open de **dialoogvenster verbinding maken met** door te klikken op de **verbinding maken met knop** op de links, verticale werkbalk.

    ![Verbinding maken met de optie Azure Storage][9]

3. Selecteer de **koppelen aan een lokale emulator** optie en klik op **volgende**.

4. Kies een weergavenaam voor uw verbinding en voer in de poorten die uw emulator op voor elk servicetype luistert. Standaard bevat de tekstvakken de standaardwaarden van de poort voor de meeste emulators. De **bestanden poort** ook leeg is standaard als geen van de officiële emulators momenteel ondersteuning voor de Files-service. Als de emulator u ondersteuning biedt voor deze echter, kunt klikt u vervolgens u in de poort die wordt gebruikt. Klik op **volgende**.

5. Bekijk de samenvatting verbinding en zorg ervoor dat alle informatie juist is. Als alle informatie die overeenkomt met uw wijzigingen klikt u op **Connect**, anders Ga terug naar de vorige pagina's met de **terug** knop eventuele onjuiste gegevens te corrigeren.

Zodra de verbinding is toegevoegd, wordt de resourcestructuur van de wordt automatisch navigeren naar het knooppunt van de verbinding. Als voor een of andere reden niet, kijk onder **lokaal en gekoppeld** → **Opslagaccounts**. Als u Storage Explorer kan niet worden toegevoegd, de verbinding is, of als u kan geen toegang tot uw gegevens na het toevoegen van de verbinding is, raadpleegt u de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor hulp.

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Verbinding maken met een Azure Cosmos DB-account met behulp van een verbindingsreeks

Naast Azure Cosmos DB-accounts via Azure-abonnement te beheren, een andere manier om verbinding te maken met een Azure Cosmos DB is om een verbindingsreeks te gebruiken. Gebruik de volgende stappen om verbinding te maken met behulp van een verbindingsreeks.

1. Ga naar **Lokaal en gekoppeld** in het linkerdeelvenster, klik met de rechtermuisknop op **Azure Cosmos DB-accounts**, en kies **Verbinding maken met Azure Cosmos DB...**

    ![verbinding maken met Azure Cosmos DB met een verbindingsreeks][21]

2. Kies Azure Cosmos DB API, plak uw **Connection String**, en klik vervolgens op **OK** om Azure Cosmos DB-account verbinding te maken. Zie [De verbindingsreeks ophalen](https://docs.microsoft.com/azure/cosmos-db/manage-account) voor informatie over het ophalen van de verbindingsreeks.

    ![connection-string][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>Verbinding maken met Azure Data Lake Store via een URI

Als u toegang wilt krijgen tot de resources die niet aanwezig zijn in uw abonnement, maar anderen de URI voor de resources aan u geven. In dit geval kunt u de URI gebruiken om verbinding te maken met Data Lake Store nadat u bent aangemeld. Bekijk de volgende stappen.

1. Open Storage Explorer.
2. Vouw **Lokaal en gekoppeld** uit in het linkerdeelvenster.
3. Klik met uw rechtermuisknop op **Data Lake Store** en selecteer **Verbinding maken met Data Lake Store...** in het contextmenu.

    ![contextmenu Verbinding maken met Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Voer de URI in. Vervolgens navigeert het hulpprogramma naar de locatie van de URL die u zojuist hebt ingevoerd.

    ![dialoogvenster Verbinding maken met Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![resultaat Verbinding maken met Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)


## <a name="generate-a-sas-in-storage-explorer"></a>Een SAS genereren in Storage Explorer

### <a name="account-level-sas"></a>Een SAS op accountniveau

1. Met de rechtermuisknop op het opslagaccount dat u wilt delen en selecteer vervolgens **Shared Access Signature ophalen...** .

    ![Optie SAS-contextmenu ophalen][14]

2. In de **Shared Access Signature genereren** dialoogvenster vak, geeft u de periode en de machtigingen die u wilt gebruiken voor het account en klik vervolgens op de **maken** knop.

    ![Dialoogvenster SAS ophalen][15]

3. U kunt nu Kopieer de **Connection string** of de onbewerkte **querytekenreeks** naar het Klembord.

### <a name="service-level-sas"></a>SAS op serviceniveau

[Hoe u een SAS ophalen voor een blob-container in Storage Explorer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

## <a name="search-for-storage-accounts"></a>Zoeken naar opslagaccounts

Als u wilt een opslagresource vinden en waar deze zich niet meer weet, kunt u het zoekvak boven aan het linkerdeelvenster om te zoeken naar de resource.

Als u in het zoekvak typt, wordt in het linkerdeelvenster alle resources die overeenkomen met de zoekwaarde die u hebt ingevoerd tot dat moment weergegeven. Bijvoorbeeld een zoekopdracht voor **eindpunten** wordt weergegeven in de volgende schermafbeelding:

![Zoeken naar Storage-account][23]

> [!NOTE]
> Gebruik de **Account Management Panel** om op te heffen alle abonnementen waarvoor geen het item dat u zoekt voor het verbeteren van de uitvoeringstijd van uw zoekopdracht. U kunt ook met de rechtermuisknop op een knooppunt en kies **zoeken vanaf hier** begint met zoeken van een specifiek knooppunttype.
>
>

## <a name="next-steps"></a>Volgende stappen

* [Azure Blob Storage-resources beheren met Opslagverkenner](vs-azure-tools-storage-explorer-blobs.md)
* [Azure Cosmos DB beheren in Azure Storage Explorer (Preview)](./cosmos-db/storage-explorer.md)
* [Azure Data Lake Store-resources beheren met Opslagverkenner](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/GetSharedAccessSignature.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SharedAccessSignatureDialog.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Search.png
