---
title: Aan de slag met Opslagverkenner (Preview) | Microsoft Docs
description: Azure Storage-resources beheren met Opslagverkenner (Preview)
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: cawa
ms.openlocfilehash: d2b93eec9d3ac575e771bceb0ac45823254c142d
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="get-started-with-storage-explorer-preview"></a>Aan de slag met Opslagverkenner (Preview)
## <a name="overview"></a>Overzicht
Azure Storage Explorer (Preview) is een zelfstandige app waarmee u eenvoudig met Azure Storage-gegevens kunt werken via Windows, macOS en Linux. In dit artikel leert u verschillende manieren om verbinding te maken met en beheren van uw Azure storage-accounts.

![Microsoft Azure Storage Explorer (voorbeeld)][0]

## <a name="prerequisites"></a>Vereisten
* [Opslagverkenner (Preview) downloaden en installeren](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Verbinding maken met een opslagaccount of -service
Opslagverkenner (Preview) biedt verschillende manieren om verbinding te maken met opslagaccounts. U kunt bijvoorbeeld:
* Verbinding maken met de opslagaccounts die zijn gekoppeld aan uw Azure-abonnementen.
* Verbinding maken met de opslagaccounts en -services die via andere Azure-abonnementen worden gedeeld.
* Verbinding maken met lokale opslag en lokale opslag beheren met de Azure-opslagemulator. 

Bovendien kunt u wereldwijd en nationaal werken met opslagaccounts in Azure:

* [Verbinding maken met een Azure-abonnement](#connect-to-an-azure-subscription): beheer opslagresources die deel uitmaken van uw Azure-abonnement.
* [Verbinding maken met lokale opslag](#work-with-local-development-storage): beheer lokale opslag met de Azure-opslagemulator.
* [Koppelen aan externe opslag](#attach-or-detach-an-external-storage-account): beheer de opslagresources die deel uitmaken van een ander Azure-abonnement of landelijke Azure-clouds, via de naam, sleutel en eindpunten van het opslagaccount.
* [Een opslagaccount koppelen met behulp van een SAS](#attach-storage-account-using-sas): beheer de opslagresources die deel uitmaken van een ander Azure-abonnement via een handtekening voor gedeelde toegang (SAS).
* [Een service koppelen met behulp van een SAS](#attach-service-using-sas): beheer een specifieke opslagservice (blobcontainer, wachtrij of tabel) die deel uitmaakt van een ander Azure-abonnement via een handtekening voor gedeelde toegang (SAS).
* [Verbinding maken met een Azure DB die Cosmos-account via een verbindingsreeks](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): account van de Cosmos-DB beheren met behulp van een verbindingsreeks.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement
> [!NOTE]
> Als u geen Azure-account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [gebruikmaken van uw voordelen als Visual Studio-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. Selecteer in Opslagverkenner (Preview) **Accounts beheren** naar de **Account Management Panel**.

    ![Accounts beheren][1]

2. De Azure accounts die u zich hebt aangemeld bij worden nu weergegeven in het linkerdeelvenster. Voor verbinding met een ander account, selecteert u **account toevoegen**

3. Als u zich aanmelden bij een nationale cloud of een Azure-Stack wilt, klikt u op de **Azure-omgeving** dropdown selecteren Azure-cloud die u wilt gebruiken. Nadat u uw omgeving hebt gekozen, klikt u op de **aanmelden...**  knop. Als u zijn aangemeld bij Azure Stack, Zie [Opslagverkenner verbinding maken met een Azure-Stack-abonnement](azure-stack/user/azure-stack-storage-connect-se.md) voor meer informatie.

    ![Meld u aan de optie][2]

3. Nadat u zich hebt aangemeld met een Azure-account, worden het account en de Azure-abonnementen die zijn gekoppeld aan dat account toegevoegd aan het linkerdeelvenster. Selecteer de Azure-abonnementen die u wilt gebruiken en selecteer vervolgens **toepassen** (Selecting **alle abonnementen:** -of uitschakelen als u alle of geen van de vermelde Azure-abonnementen).

    ![Selecteer Azure-abonnementen][3]

    In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure-abonnementen zijn gekoppeld.

    ![Geselecteerde Azure-abonnementen][4]

## <a name="work-with-local-development-storage"></a>Werken met lokale ontwikkelingsopslag
Met Opslagverkenner (Preview) kunt u met lokale opslag werken via de Azure-opslagemulator. Deze aanpak kunt u werken met Azure Storage simuleren zonder dat hiervoor een opslagaccount dat is geïmplementeerd in Azure, omdat het opslagaccount wordt gesimuleerd door de Azure-Opslagemulator.

> [!NOTE]
> De Azure-opslagemulator wordt momenteel alleen door Windows ondersteund.
>
>

> [!NOTE]
> De Azure-Opslagemulator biedt geen ondersteuning voor bestandsshares.
>
>

1. Vouw in het linkerdeelvenster van Opslagverkenner (Preview), de **(lokaal en Attached)** > **Opslagaccounts** > **(ontwikkeling)**  >  **Blob-Containers** knooppunt.

    ![Node lokale ontwikkeling][5]

2. Als u de Azure-Opslagemulator nog niet hebt geïnstalleerd, wordt u gevraagd om dit te doen via een informatiebalk. Wanneer de informatiebalk wordt weergegeven, selecteert u **De nieuwste versie downloaden** en installeert u de emulator.

    ![Azure Storage-emulator prompt downloaden][6]

3. Wanneer de emulator is geïnstalleerd, kunt u lokale blobs, wachtrijen en tabellen maken en hiermee werken. Raadpleeg voor meer informatie over het werken met elk opslagaccounttype, de volgende handleidingen:

    * [Resources voor Azure Blob Storage beheren](vs-azure-tools-storage-explorer-blobs.md)
    * Opslagresources voor het delen van Azure-bestanden beheren: *binnenkort beschikbaar*
    * Resources voor Azure Queue Storage beheren: *binnenkort beschikbaar*
    * Resources voor Azure Table Storage beheren: *binnenkort beschikbaar*

## <a name="attach-or-detach-an-external-storage-account"></a>Koppelen aan een extern opslagaccount of de koppeling opheffen
Met Opslagverkenner (Preview) kunt u externe opslagaccounts koppelen zodat opslagaccounts eenvoudig kunnen worden gedeeld. In dit gedeelte wordt uitgelegd hoe u externe opslagaccounts koppelt (en hoe u de koppeling opheft).

### <a name="get-the-storage-account-credentials"></a>De opslagaccountreferenties ophalen
Als u wilt delen een extern opslagaccount, moet de eigenaar van dat account eerst de referenties (accountnaam en -sleutel) ontvangen voor het account en vervolgens delen dat gegevens met de persoon die wil koppelen aan deze account. U kunt de opslagaccountreferenties via de Azure-portal verkrijgen tijdens het doorzoeken van de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Bladeren**.

3. Selecteer **Opslagaccounts**.

4. In de lijst met **Opslagaccounts**, selecteer het gewenste opslagaccount.

5. Onder **instellingen**, selecteer **toegangssleutels**.

    ![Optie Toegangssleutels][7]

6. Kopieer de **opslagaccountnaam** en **key1**.

    ![Toegangssleutels][8]

### <a name="attach-to-an-external-storage-account"></a>Koppelen aan een extern opslagaccount
Als u verbinding wilt maken met een extern opslagaccount, moet u de accountaam en -sleutel opgeven. In het gedeelte 'De opslagaccountreferenties ophalen' wordt uitgelegd hoe u deze waarden verkrijgt in de Azure Portal. In de portal wordt de accountsleutel echter **key1** genoemd. Dus wanneer Opslagverkenner (Preview) om een accountsleutel wordt gevraagd, voert u de **key1** waarde.

1. Open in Opslagverkenner (Preview), de **dialoogvenster verbinding**.

    ![Verbinding maken met de optie Azure Storage][9]

2. In de **dialoogvenster verbinding**, kies **een naam van het opslagaccount en de sleutel gebruiken**

    ![Met de naam en sleutel optie toevoegen][10]

3. Plak de accountnaam van uw in de **accountnaam** tekst vak en plak uw accountsleutel (de **key1** waarde van de Azure-portal) in de **accountsleutel** tekstvak in en selecteer vervolgens **Volgende**.

    ![Naam en sleutel pagina][11]

    > [!NOTE]
    > Als u een naam en sleutel uit een nationale cloud, gebruikt de **opslag eindpunten domein:** dropdown het juiste eindpunten domein selecteren: 
    >
    >

4. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u iets wilt wijzigen, selecteert u **Terug** en voert u de gewenste instellingen opnieuw in. 

5. Selecteer **Verbinden**.

6. Nadat het opslagaccount is toegevoegd, wordt het opslagaccount weergegeven met **(extern)** toegevoegd aan de naam ervan.

    ![Resultaat van koppelen met een extern opslagaccount][12]

### <a name="detach-from-an-external-storage-account"></a>De koppeling met een extern opslagaccount opheffen
1. Klik met de rechtermuisknop op het externe opslagaccount dat u wilt loskoppelen en selecteer **Loskoppelen**.

    ![Loskoppelen van de opslagoptie][13]

2. Selecteer in het bevestigingsbericht **Ja** om het loskoppelen van het externe opslagaccount te bevestigen.

## <a name="attach-a-storage-account-by-using-a-shared-access-signature-sas"></a>Koppelen van een opslagaccount met behulp van een Shared Access Signature (SAS)
Een Shared Access Signature of [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md), kan de beheerder van een Azure-abonnement tijdelijke toegang verlenen tot een opslagaccount zonder referenties van de Azure-abonnement op te geven.

Een voorbeeld van dit scenario: stel gebruiker A is beheerder van een Azure-abonnement en gebruiker A wil gebruiker B een beperkte tijd toegang bieden tot een opslagaccount, met bepaalde machtigingen:

1. Gebruiker a genereert een SAS-verbindingsreeks voor een bepaalde periode en de gewenste machtigingen.

2. Gebruiker a deelt de SAS met de persoon die wil de toegang tot het opslagaccount (gebruiker b, in dit voorbeeld).  

3. Gebruiker B gebruikt Opslagverkenner (Preview) om verbinding te maken met het account van gebruiker A via de opgegeven SAS.

### <a name="generate-a-sas-connection-string-for-the-account-you-want-to-share"></a>Genereren van een SAS-verbindingsreeks voor de account die u wilt delen
1. In Opslagverkenner (Preview) met de rechtermuisknop op het opslagaccount dat u wilt delen, en selecteer vervolgens **Shared Access Signature ophalen...** .

    ![Optie SAS-contextmenu ophalen][14]

2. In de **Shared Access Signature genereren** dialoogvenster geeft u de periode en de machtigingen die u wilt gebruiken voor het account en klik vervolgens op de **maken** knop.

    ![Dialoogvenster SAS ophalen][15]  

3. Naast de **verbindingsreeks** in het tekstvak, selecteer **kopie** naar het Klembord kopiëren en klik vervolgens op **sluiten**.

### <a name="attach-to-a-storage-account-by-using-a-sas-connection-string"></a>Koppelen aan een opslagaccount met behulp van een SAS-verbindingsreeks
1. Open in Opslagverkenner (Preview), de **dialoogvenster verbinding**.

    ![Verbinding maken met de optie Azure Storage][9]

2. In de **dialoogvenster verbinding** dialoogvenster kiezen **gebruiken een shared access signature voor URI of een verbindingsreeks** en klik vervolgens op **volgende**.

    ![Verbinding maken met het dialoogvenster Azure Storage][16]

3. Kies **gebruik een verbindingsreeks** en plak de verbindingsreeks in de **verbindingsreeks:** veld. Klik op de **volgende** knop.

    ![Verbinding maken met het dialoogvenster Azure Storage][17]

4. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen wilt aanbrengen, selecteert u **Terug**, en voert u vervolgens de gewenste instellingen in. 

5. Selecteer **Verbinden**.

6. Nadat het opslagaccount is toegevoegd, wordt het opslagaccount weergegeven met **(SAS)** toegevoegd aan de naam ervan.

    ![Resultaat van koppeling met een account via SAS][18]

## <a name="attach-a-service-by-using-a-shared-access-signature-sas"></a>Een service koppelen via een Shared Access Signature (SAS)
De sectie 'Een storage-account koppelen via een SAS' wordt uitgelegd hoe de beheerder van een Azure-abonnement kunt tijdelijke toegang verlenen tot een opslagaccount door te genereren en delen van een SAS voor het opslagaccount. Een SAS kan op dezelfde manier worden gegenereerd voor een specifieke service (blobcontainer, wachtrij, tabel of bestandsshare) binnen een opslagaccount.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Een SAS genereren voor de service die u wilt delen
In deze context is een service een blobcontainer, wachtrij, tabel, of bestandsshare. Zie voor het genereren van de SAS voor een vermelde service:

* [De SAS ophalen voor een blobcontainer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* De SAS ophalen voor het delen van bestanden: *binnenkort beschikbaar*
* De SAS ophalen voor een wachtrij: *binnenkort beschikbaar*
* De SAS ophalen voor een tabel: *binnenkort beschikbaar*

### <a name="attach-to-the-shared-account-service-by-using-a-sas-uri"></a>Koppelen aan de gedeelde-accountservice met behulp van een SAS-URI
1. Open in Opslagverkenner (Preview), de **dialoogvenster verbinding**.

    ![Verbinding maken met de optie Azure Storage][9]

2. In de **dialoogvenster verbinding** dialoogvenster Kies **gebruiken een shared access signature voor URI of een verbindingsreeks** en klik vervolgens op **volgende**.

    ![Verbinding maken met het dialoogvenster Azure Storage][16]

3. Kies **een SAS-URI gebruikt** en plak uw URI in de **URI:** veld. Klik op de **volgende** knop.

    ![Verbinding maken met het dialoogvenster Azure Storage][19]

3. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen wilt aanbrengen, selecteert u **Terug**, en voert u vervolgens de gewenste instellingen in. 

4. Selecteer **Verbinden**.

5. Nadat de service is toegevoegd, wordt de service weergegeven onder de **(SAS-Attached Services)** knooppunt.

    ![Resultaat van het koppelen met een gedeelde-service met behulp van een SAS][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Verbinding maken met een Azure DB die Cosmos-account via een verbindingsreeks
Naast het beheer van Azure DB die Cosmos accounts via Azure-abonnement moet een andere manier van verbinding maken met een Cosmos Azure DB is het gebruik van een verbindingsreeks. Gebruik de volgende stappen om verbinding te maken met behulp van een verbindingsreeks.

1. Ga naar **Lokaal en gekoppeld** in het linkerdeelvenster, klik met de rechtermuisknop op **Azure Cosmos DB-accounts**, en kies **Verbinding maken met Azure Cosmos DB...**

    ![verbinding maken met Azure Cosmos DB met verbindingsreeks][21]

2. Kies Azure Cosmos DB API, plak uw **verbindingsreeks**, en klik vervolgens op **OK** om te verbinden Azure DB die Cosmos-account. Zie [De verbindingsreeks ophalen](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string) voor informatie over het ophalen van de verbindingsreeks.

    ![connection-string][22]

## <a name="search-for-storage-accounts"></a>Zoeken naar opslagaccounts
Als u wilt zoeken naar een opslagresource en waar deze zich niet meer weet, kunt u het zoekvak boven aan het linkerdeelvenster om te zoeken naar de resource.

Terwijl u in het zoekvak typt, worden alle bronnen die overeenkomen met de zoekwaarde die u hebt ingevoerd naar dat punt weergegeven in het linkerdeelvenster. Bijvoorbeeld, een zoekopdracht voor **eindpunten** wordt weergegeven in de volgende schermafbeelding:

![Zoeken naar Storage-account][23]

> [!NOTE]
> Gebruik de **Account Management Panel** om op te heffen geen abonnementen die het item dat u zoekt voor het verbeteren van de uitvoeringstijd van de zoekopdracht geen bevatten. U kunt ook met de rechtermuisknop op een knooppunt en kies **zoeken vanaf hier** begint met zoeken van een specifiek knooppunt.
>
>

## <a name="next-steps"></a>Volgende stappen
* [Azure Blob Storage-resources beheren met Opslagverkenner (Preview)](vs-azure-tools-storage-explorer-blobs.md)
* [Beheren van Azure Cosmos DB in Azure Opslagverkenner (Preview)](./cosmos-db/storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/DevelopmentNode.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/EmulatorNotInstalled.png
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
