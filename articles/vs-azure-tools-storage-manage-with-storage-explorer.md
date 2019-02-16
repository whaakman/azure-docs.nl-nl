---
title: Aan de slag met Storage Explorer | Microsoft Docs
description: Azure storage-resources beheren met Opslagverkenner
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: cawa
ms.openlocfilehash: 9917901146eb0a050531a66ab24fbd88c91c02c9
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56311202"
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

* Ubuntu 16.04 x64 (aanbevolen)
* Ubuntu 17.10 x64
* Ubuntu 14.04 x64

Azure Storage Explorer werkt in een andere distributie, maar de enige die hierboven vermeld worden officieel ondersteund.

U moet ook de volgende afhankelijkheden/bibliotheken geïnstalleerd voor uitvoering van Azure Storage Explorer op Linux hebben:

* [.NET Core 2.x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* libsecret (Opmerking: libsecret 1.so.0 moet beschikbaar zijn op uw computer. Als u een andere versie van libsecret geïnstalleerd hebt, kunt u proberen voorlopig de .so-bestand te koppelen aan libsecret 1.so.0)
* libgconf-2-4
* Recente GCC

Azure Storage Explorer [opmerkingen bij de Release](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) specifieke stappen voor het aantal distributies bevatten.

[Storage Explorer downloaden en installeren](https://www.storageexplorer.com)

---

## <a name="connect-to-a-storage-account-or-service"></a>Verbinding maken met een opslagaccount of -service

Storage Explorer biedt verschillende manieren om verbinding te maken met opslagaccounts. U kunt bijvoorbeeld:

* Verbinding maken met de opslagaccounts die zijn gekoppeld aan uw Azure-abonnementen.
* Verbinding maken met de opslagaccounts en -services die via andere Azure-abonnementen worden gedeeld.
* Verbinding maken met lokale opslag en lokale opslag beheren met de Azure-opslagemulator.

Bovendien kunt u wereldwijd en nationaal werken met opslagaccounts in Azure:

* [Verbinding maken met een Azure-abonnement](#connect-to-an-azure-subscription): Beheer de opslagresources die deel uitmaken van uw Azure-abonnement.
* [Werken met lokale opslag](#work-with-local-development-storage): Lokale opslag beheren met behulp van de Azure-Opslagemulator.
* [Koppelen aan externe opslag](#attach-or-detach-an-external-storage-account): Beheer de opslagresources die deel uitmaken van een ander Azure-abonnement of die zijn landelijke Azure-Clouds met behulp van de naam, sleutel en eindpunten van het storage-account.
* [Een opslagaccount koppelen met behulp van een SAS](#attach-a-storage-account-by-using-a-shared-access-signature-sas): Beheer de opslagresources die deel uitmaken van een andere Azure-abonnement met behulp van een shared access signature (SAS).
* [Een service koppelen via SAS](#attach-a-service-by-using-a-shared-access-signature-sas): Beheer een specifieke opslagservice (blobcontainer, wachtrij of tabel) die deel uitmaakt van een andere Azure-abonnement met behulp van een SAS.
* [Verbinding maken met een Azure Cosmos DB-account met behulp van een verbindingsreeks](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): Cosmos DB-account beheren met behulp van een verbindingsreeks.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement

> [!NOTE]
> Als u geen Azure-account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [gebruikmaken van uw voordelen als Visual Studio-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. Selecteer in Storage Explorer **Accounts beheren** naar de **Account Management Panel**.

    ![Accounts beheren][1]

2. In het linkerdeelvenster ziet u nu alle Azure accounts die u hebt aangemeld bij. Voor verbinding met een ander account, selecteert u **een account toevoegen**

3. Als u zich aanmelden bij een nationale cloud of een Azure Stack wilt, klikt u op de **Azure-omgeving** vervolgkeuzelijst te selecteren welke Azure-cloud u wilt gebruiken. Nadat u uw omgeving hebt gekozen, klikt u op de **aanmelden...**  knop. Als u met Azure Stack aanmeldt zich, Zie [Storage Explorer verbinden met een Azure Stack-abonnement](azure-stack/user/azure-stack-storage-connect-se.md) voor meer informatie.

    ![Meld u aan optie][2]

4. Nadat u zich hebt aangemeld met een Azure-account, worden het account en de Azure-abonnementen die zijn gekoppeld aan dat account toegevoegd aan het linkerdeelvenster. Selecteer het Azure-abonnementen die u wilt werken en selecteer vervolgens **toepassen** (selecteren **alle abonnementen:** te selecteren selecteert u alle of geen van de Azure-abonnementen).

    ![Selecteer Azure-abonnementen][3]

    In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure-abonnementen zijn gekoppeld.

    ![Geselecteerde Azure-abonnementen][4]

## <a name="work-with-local-development-storage"></a>Werken met lokale ontwikkelingsopslag

Met Storage Explorer kunt u met lokale opslag werken via een emulator. Deze aanpak kunt u werken met Azure Storage te simuleren, zonder dat een geïmplementeerd in Azure storage-account hoeft te hebben.

Vanaf versie 1.1.0 is de emulator van de lokale opslag wordt ondersteund op alle platforms. Storage Explorer verbinden met elke geëmuleerde-service te hebben geluisterd naar de Standaardeindpunten voor lokale opslag.

> [!NOTE]
> Ondersteuning voor storage-services en functies kan aanzienlijk verschillen, afhankelijk van uw keuze van de emulator. Zorg ervoor dat de emulator ondersteunt de services en functies die u van plan bent om te werken met.

1. Configureren van de services van uw keuze om te luisteren naar een niet-gebruikte poort-emulator.

   Geëmuleerde Service | Eindpunt
   -----------------|-------------------------
   Blobs            | `http://127.0.0.1:10000`
   Wachtrijen           | `http://127.0.0.1:10001`
   Tabellen           | `http://127.0.0.1:10002`

2. Start de emulator.
   > [!IMPORTANT]
   > Storage Explorer uw emulator niet automatisch wordt gestart. U moet deze zelf starten.

3. In Storage Explorer, klikt u op de **-Account toevoegen** knop. Selecteer **koppelen aan een lokale emulator** en klikt u op **volgende**.

4. Voer de poortnummers voor de services die u hierboven (laat leeg als u niet van plan bent om deze service te gebruiken) hebt geconfigureerd. Klik op **volgende** vervolgens **Connect** om de verbinding te maken.

5. Vouw de **lokaal en gekoppeld** > **Opslagaccounts** > knooppunten, vouw vervolgens de serviceknooppunten onder het knooppunt dat overeenkomt met de emulator-verbinding.

   U kunt dit knooppunt gebruiken om te maken en werken met lokale blobs, wachtrijen en tabellen. Zie voor meer informatie over het werken met elk opslagaccounttype, de volgende handleidingen:

   * [Azure Blob storage-resources beheren](vs-azure-tools-storage-explorer-blobs.md)
   * [Azure File storage-resources beheren](vs-azure-tools-storage-explorer-files.md)

## <a name="attach-or-detach-an-external-storage-account"></a>Koppelen aan een extern opslagaccount of de koppeling opheffen

Met Storage Explorer kunt u externe opslagaccounts koppelen zodat opslagaccounts eenvoudig kunnen worden gedeeld. In dit gedeelte wordt uitgelegd hoe u externe opslagaccounts koppelt (en hoe u de koppeling opheft).

### <a name="get-the-storage-account-credentials"></a>De opslagaccountreferenties ophalen

Voor het delen van een extern opslagaccount, moet de eigenaar van dat account eerst de referenties (de naam en sleutel) ontvangen voor het account en klikt u vervolgens delen dat gegevens met de persoon die wil koppelen aan deze account. De referenties van het storage-account via Azure portal kunt u verkrijgen door de volgende stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Bladeren**.

3. Selecteer **Opslagaccounts**.

4. In de lijst met **Opslagaccounts**, selecteer het gewenste opslagaccount.

5. Selecteer **Toegangssleutels** bij **Instellingen**.

    ![Optie Toegangssleutels][7]

6. Kopieer de **opslagaccountnaam** en **key1**.

    ![Toegangssleutels][8]

### <a name="attach-to-an-external-storage-account"></a>Koppelen aan een extern opslagaccount

Als u verbinding wilt maken met een extern opslagaccount, moet u de accountaam en -sleutel opgeven. In het gedeelte 'De opslagaccountreferenties ophalen' wordt uitgelegd hoe u deze waarden verkrijgt in de Azure Portal. In de portal wordt de accountsleutel echter **key1** genoemd. Dus wanneer Storage Explorer om een accountsleutel vraagt, voert u de **key1** waarde.

1. Open in Storage Explorer, de **dialoogvenster verbinding maken met**.

    ![Verbinding maken met de optie Azure Storage][9]

2. In de **dialoogvenster verbinding maken met**, kiest u **een opslagaccountnaam en -sleutel gebruiken**

    ![Met de naam en sleutel optie toevoegen][10]

3. Plak de accountnaam van uw in de **accountnaam** tekst in en plak uw accountsleutel (de **key1** waarde uit de Azure portal) in de **accountsleutel** tekstvak in en selecteer vervolgens **Volgende**.

    ![Naam en sleutel pagina][11]

    > [!NOTE]
    > Als u een naam en sleutel van een nationale cloud, gebruikt de **domein van opslageindpunten:** vervolgkeuzelijst te selecteren van het domein van de juiste eindpunten:
    >
    >

4. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u iets wilt wijzigen, selecteert u **Terug** en voert u de gewenste instellingen opnieuw in.

5. Selecteer **Verbinden**.

6. Nadat het opslagaccount is gekoppeld, wordt het opslagaccount weergegeven met **(extern)** toegevoegd aan de naam.

    ![Resultaat van koppelen met een extern opslagaccount][12]

### <a name="detach-from-an-external-storage-account"></a>De koppeling met een extern opslagaccount opheffen

1. Klik met de rechtermuisknop op het externe opslagaccount dat u wilt loskoppelen en selecteer **Loskoppelen**.

    ![Loskoppelen van de opslagoptie][13]

2. Selecteer in het bevestigingsbericht **Ja** om het loskoppelen van het externe opslagaccount te bevestigen.

## <a name="attach-a-storage-account-by-using-a-shared-access-signature-sas"></a>Een opslagaccount koppelen met behulp van een Shared Access Signature (SAS)

Een Shared Access Signature of [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md), kan de beheerder van een Azure-abonnement tijdelijke toegang verlenen tot een opslagaccount zonder Azure-abonnement referenties op te geven.

Een voorbeeld van dit scenario: stel gebruiker A is beheerder van een Azure-abonnement en gebruiker A wil gebruiker B een beperkte tijd toegang bieden tot een opslagaccount, met bepaalde machtigingen:

1. Gebruiker a genereert een SAS-verbindingsreeks voor een bepaalde periode en de gewenste machtigingen.

2. Gebruiker a deelt de SAS met de persoon die toegang wil tot het opslagaccount (gebruiker b, in dit voorbeeld).

3. Gebruiker b gebruikt Opslagverkenner koppelen aan het account die deel uitmaakt van gebruiker a met behulp van de opgegeven SAS.

### <a name="generate-a-sas-query-string-for-the-account-you-want-to-share"></a>Genereren van een SAS-queryreeks voor het account dat u wilt delen

1. In Storage Explorer met de rechtermuisknop op het opslagaccount dat u wilt delen en selecteer vervolgens **Shared Access Signature ophalen...** .

    ![Optie SAS-contextmenu ophalen][14]

2. In de **Shared Access Signature genereren** dialoogvenster vak, geeft u de periode en de machtigingen die u wilt gebruiken voor het account en klik vervolgens op de **maken** knop.

    ![Dialoogvenster SAS ophalen][15]

3. Naast de **queryreeks** tekstvak, selecteer **kopie** naar het Klembord kopiëren en klik vervolgens op **sluiten**.

### <a name="attach-to-a-storage-account-by-using-a-sas-connection-string"></a>Koppelen aan een opslagaccount met behulp van een SAS-verbindingsreeks

1. Open in Storage Explorer, de **dialoogvenster verbinding maken met**.

    ![Verbinding maken met de optie Azure Storage][9]

2. In de **dialoogvenster verbinding maken met** dialoogvenster kiezen **een verbindingsreeks of handtekening voor gedeelde toegang URI gebruiken** en klik vervolgens op **volgende**.

    ![Verbinding maken met het dialoogvenster Azure Storage][16]

3. Kies **gebruik een verbindingsreeks** en plak de verbindingsreeks in de **verbindingsreeks:** veld. Klik op de knop **Next**

    ![Verbinding maken met het dialoogvenster Azure Storage][17]

4. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen wilt aanbrengen, selecteert u **Terug**, en voert u vervolgens de gewenste instellingen in.

5. Selecteer **Verbinden**.

6. Nadat het opslagaccount is gekoppeld, wordt het opslagaccount weergegeven met **(SAS)** toegevoegd aan de naam.

    ![Resultaat van koppeling met een account via SAS][18]

## <a name="attach-a-service-by-using-a-shared-access-signature-sas"></a>Een service koppelen met behulp van een Shared Access Signature (SAS)

Het gedeelte 'Een opslagaccount koppelen met behulp van een SAS' wordt uitgelegd hoe de beheerder van een Azure-abonnement tijdelijke toegang tot een opslagaccount kunt verlenen door te genereren en het delen van een SAS voor het opslagaccount. Op deze manier een SAS genereren voor een bepaalde service (blobcontainer, wachtrij, tabel of bestandsshare) binnen een storage-account.

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Een SAS genereren voor de service die u wilt delen

In deze context is een service een blobcontainer, wachtrij, tabel, of bestandsshare. Zie voor het genereren van de SAS voor een vermelde service:

* [De SAS ophalen voor een blobcontainer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

### <a name="attach-to-the-shared-account-service-by-using-a-sas-uri"></a>Koppelen aan de gedeelde-accountservice met behulp van een SAS-URI

1. Open in Storage Explorer, de **dialoogvenster verbinding maken met**.

    ![Verbinding maken met de optie Azure Storage][9]

2. In de **dialoogvenster verbinding maken met** dialoogvenster vak, kiest u **een verbindingsreeks of handtekening voor gedeelde toegang URI gebruiken** en klik vervolgens op **volgende**.

    ![Verbinding maken met het dialoogvenster Azure Storage][16]

3. Kies **gebruik van een SAS-URI** en plak de URI in de **URI:** veld. Klik op de knop **Next**

    ![Verbinding maken met het dialoogvenster Azure Storage][19]

4. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen wilt aanbrengen, selecteert u **Terug**, en voert u vervolgens de gewenste instellingen in.

5. Selecteer **Verbinden**.

6. Nadat de service is gekoppeld, wordt de service weergegeven onder de **(SAS-Attached Services)** knooppunt.

    ![Resultaat van het koppelen met een gedeelde-service met behulp van een SAS][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Verbinding maken met een Azure Cosmos DB-account met behulp van een verbindingsreeks

Naast Azure Cosmos DB-accounts via Azure-abonnement te beheren, een andere manier om verbinding te maken met een Azure Cosmos DB is om een verbindingsreeks te gebruiken. Gebruik de volgende stappen om verbinding te maken met behulp van een verbindingsreeks.

1. Ga naar **Lokaal en gekoppeld** in het linkerdeelvenster, klik met de rechtermuisknop op **Azure Cosmos DB-accounts**, en kies **Verbinding maken met Azure Cosmos DB...**

    ![verbinding maken met Azure Cosmos DB met een verbindingsreeks][21]

2. Kies Azure Cosmos DB API, plak uw **Connection String**, en klik vervolgens op **OK** om Azure Cosmos DB-account verbinding te maken. Zie [De verbindingsreeks ophalen](https://docs.microsoft.com/azure/cosmos-db/manage-account) voor informatie over het ophalen van de verbindingsreeks.

    ![connection-string][22]

## <a name="connect-to-azure-data-lake-store-by-uri"></a>Verbinding maken met Azure Data Lake Store via een URI

Als u toegang wilt krijgen tot de resources die niet aanwezig zijn in uw abonnement, maar anderen de URI voor de resources aan u geven. In dit geval kunt u de URI gebruiken om verbinding te maken met Data Lake Store nadat u bent aangemeld. Bekijk de volgende stappen.

1. Open Storage Explorer.
2. Vouw **Lokaal en gekoppeld** uit in het linkerdeelvenster.
3. Klik met uw rechtermuisknop op **Data Lake Store** en selecteer **Verbinding maken met Data Lake Store...** in het contextmenu.

    ![contextmenu Verbinding maken met Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Voer de URI in. Vervolgens navigeert het hulpprogramma naar de locatie van de URL die u zojuist hebt ingevoerd.

    ![dialoogvenster Verbinding maken met Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![resultaat Verbinding maken met Data Lake Store](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)

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
