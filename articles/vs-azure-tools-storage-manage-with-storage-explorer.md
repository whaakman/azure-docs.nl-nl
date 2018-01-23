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
ms.openlocfilehash: cccab530e86373fee8a78b42c8cba532b05c1bab
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="get-started-with-storage-explorer-preview"></a>Aan de slag met Opslagverkenner (Preview)
## <a name="overview"></a>Overzicht
Azure Storage Explorer (Preview) is een zelfstandige app waarmee u eenvoudig met Azure Storage-gegevens kunt werken via Windows, macOS en Linux. In dit artikel leert u op welke manieren u verbinding kunt maken met Azure Storage-accounts en hoe u deze kunt beheren.

![Microsoft Azure Storage Explorer (voorbeeld)][15]

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

1. Selecteer in Storage Explorer (voorbeeld) **Azure-accountinstellingen**.

    ![Azure-accountinstellingen][0]

2. In het linkerdeelvenster ziet u alle Microsoft-accounts waarbij u bent aangemeld. Selecteer **Een account toevoegen** om verbinding te maken met een ander account en volg de instructies om u aan te melden met een Microsoft-account dat is gekoppeld aan ten minste één actief Azure-abonnement.

    >[!NOTE]
    >Het maken van verbinding met een landelijke Azure (zoals Azure Duitsland, Azure Government en Azure China via aanmelding) wordt momenteel niet ondersteund. Raadpleeg de sectie 'Koppelen aan een extern opslagaccount of de koppeling opheffen' voor informatie over hoe u verbinding maakt met landelijke Azure-opslagaccounts.

3. Nadat u bent aangemeld met een Microsoft-account, worden in het linkerdeelvenster de Azure-abonnementen weergegeven die aan dat account zijn gekoppeld. Selecteer de Azure-abonnementen waarmee u wilt werken en selecteer vervolgens **Toepassen**. (Door **Alle abonnementen** te selecteren selecteert u alle of geen Azure-abonnementen.)

    ![Selecteer Azure-abonnementen][3]  
    In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure-abonnementen zijn gekoppeld.

    ![Geselecteerde Azure-abonnementen][4]

## <a name="connect-to-an-azure-stack-subscription"></a>Verbinding maken met een Azure Stack-abonnement

Zie [Connect Storage Explorer to an Azure Stack subscription](azure-stack/user/azure-stack-storage-connect-se.md) (Opslagverkenner verbinden met een Azure Stack-abonnement) voor meer informatie over hoe u verbinding maakt met een Azure Stack-abonnement.

## <a name="work-with-local-development-storage"></a>Werken met lokale ontwikkelingsopslag
Met Opslagverkenner (Preview) kunt u met lokale opslag werken via de Azure-opslagemulator. Hiermee kunt u code schrijven voor opslag en opslag testen zonder dat u een opslagaccount hoeft te hebben geïmplementeerd in Azure, omdat het opslagaccount wordt gesimuleerd door de Azure-opslagemulator.

> [!NOTE]
> De Azure-opslagemulator wordt momenteel alleen door Windows ondersteund.
>
>

1. Vouw in het linkerdeelvenster van Opslagverkenner (Preview) de node **(Lokale en bijgevoegde)** > **Opslagaccounts** > **(ontwikkeling)** uit.

    ![Node lokale ontwikkeling][21]

2. Als u de Azure-opslagemulator nog niet hebt geïnstalleerd, wordt u via de informatiebalk gevraagd dit te doen. Wanneer de informatiebalk wordt weergegeven, selecteert u **De nieuwste versie downloaden** en installeert u de emulator.

    ![Azure Storage-emulator prompt downloaden][22]

3. Wanneer de emulator is geïnstalleerd, kunt u lokale blobs, wachtrijen en tabellen maken en hiermee werken. Als u wilt leren hoe u met elk type opslagaccount werkt, raadpleegt u een van de volgende onderwerpen:

    * [Resources voor Azure Blob Storage beheren](vs-azure-tools-storage-explorer-blobs.md)
    * Opslagresources voor het delen van Azure-bestanden beheren: *binnenkort beschikbaar*
    * Resources voor Azure Queue Storage beheren: *binnenkort beschikbaar*
    * Resources voor Azure Table Storage beheren: *binnenkort beschikbaar*

## <a name="attach-or-detach-an-external-storage-account"></a>Koppelen aan een extern opslagaccount of de koppeling opheffen
Met Opslagverkenner (Preview) kunt u externe opslagaccounts koppelen zodat opslagaccounts eenvoudig kunnen worden gedeeld. In dit gedeelte wordt uitgelegd hoe u externe opslagaccounts koppelt (en hoe u de koppeling opheft).

### <a name="get-the-storage-account-credentials"></a>De opslagaccountreferenties ophalen
Als u een extern opslagaccount wilt delen, moet de eigenaar van dat account eerst de referenties ontvangen (accountnaam en -sleutel) voor het account en daarna die informatie delen met de persoon die verbinding wil maken met dat (externe) account. Ga als volgt te werk om de opslagaccountreferenties te verkrijgen via de Azure Portal:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Bladeren**.

3. Selecteer **Opslagaccounts**.

4. Op de blade **Opslagaccounts** selecteert u het gewenste opslagaccount.

5. Op de blade **Instellingen** van het geselecteerde opslagaccount selecteert u **Toegangssleutels**.

    ![Optie Toegangssleutels][5]

6. Op de blade **Toegangssleutels** kopieert u de waarden voor **Opslagaccountnaam** en **key1** voor gebruik bij het koppelen aan het opslagaccount.

    ![Toegangssleutels][6]

### <a name="attach-to-an-external-storage-account"></a>Koppelen aan een extern opslagaccount
Als u verbinding wilt maken met een extern opslagaccount, moet u de accountaam en -sleutel opgeven. In het gedeelte 'De opslagaccountreferenties ophalen' wordt uitgelegd hoe u deze waarden verkrijgt in de Azure Portal. In de portal wordt de accountsleutel echter **key1** genoemd. Als Opslagverkenner (Preview) om een accountsleutel vraagt, geeft u dus de waarde **key1** op.

1. Selecteer in Storage Explorer (voorbeeld) **Verbinding maken met Azure Storage**.

    ![Verbinding maken met de optie Azure Storage][23]

2. Geef in het dialoogvenster **Verbinding maken met Azure Storage** de accountsleutel (de waarde **key1** van de Azure Portal) op en selecteer **Volgende**.

    > [!NOTE]
    > U kunt een Storage-verbindingsreeks van een opslagaccount invoeren in een landelijk Azure-onderdeel. Als u bijvoorbeeld verbinding wilt maken met opslagaccounts van Azure Duitsland, voert u verbindingsreeksen in die vergelijkbaar zijn met de volgende: 
    >
    >* DefaultEndpointsProtocol=https
    >* AccountName=cawatest03
    >* AccountKey=<storage_account_key>
    >* EndpointSuffix=core.cloudapi.de
    
    >U verkrijgt de verbindingsreeks van de Azure Portal op de manier die wordt beschreven in de sectie 'De opslagaccountreferenties ophalen'.

    ![Verbinding maken met het dialoogvenster Azure Storage][24]

3. In het dialoogvenster **Externe opslag koppelen** voert u de naam van het opslagaccount in het vak **Accountnaam** in en voert u andere gewenste instellingen in. Selecteer daarna **Volgende**.

    ![Dialoogvenster Externe opslag koppelen][8]

4. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u iets wilt wijzigen, selecteert u **Terug** en voert u de gewenste instellingen opnieuw in. 

5. Selecteer **Verbinden**.

6. Nadat de verbinding tot stand is gekomen, wordt het externe opslagaccount weergegeven en **(Extern)** aan de naam van het opslagaccount toegevoegd.

    ![Resultaat van koppelen met een extern opslagaccount][9]

### <a name="detach-from-an-external-storage-account"></a>De koppeling met een extern opslagaccount opheffen
1. Klik met de rechtermuisknop op het externe opslagaccount dat u wilt loskoppelen en selecteer **Loskoppelen**.

    ![Loskoppelen van de opslagoptie][10]

2. Selecteer in het bevestigingsbericht **Ja** om het loskoppelen van het externe opslagaccount te bevestigen.

## <a name="attach-a-storage-account-by-using-an-sas"></a>Een opslagaccount koppelen met behulp van een SAS
Met een [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) kan de beheerder van een Azure-abonnement tijdelijke toegang verlenen tot een opslagaccount zonder Azure-abonnementreferenties op te geven.

Een voorbeeld van dit scenario: stel gebruiker A is beheerder van een Azure-abonnement en gebruiker A wil gebruiker B een beperkte tijd toegang bieden tot een opslagaccount, met bepaalde machtigingen:

1. Gebruiker A genereert een SAS (bestaande uit de verbindingsreeks voor het opslagaccount) die een bepaalde tijd geldig is en de gewenste machtigingen heeft.

2. Gebruiker A deelt de SAS met de persoon die toegang wil tot het opslagaccount (in ons geval gebruiker B).  

3. Gebruiker B gebruikt Opslagverkenner (Preview) om verbinding te maken met het account van gebruiker A via de opgegeven SAS.

### <a name="get-an-sas-for-the-account-you-want-to-share"></a>Een SAS ophalen voor het account dat u wilt delen
1. Klik in de Storage Explorer (Preview) met de rechtermuisknop op het opslagaccount dat u wilt delen en selecteer **Shared Access Signature ophalen**.

    ![Optie SAS-contextmenu ophalen][13]

2. Geef in het dialoogvenster **Shared Access Signature** het tijdsbestek op en de machtigingen die u wilt gebruiken voor het account en selecteer **Maken**.

    ![Dialoogvenster SAS ophalen][14]  
    Het dialoogvenster **Shared Access Signature** wordt geopend met de SAS weergegeven.

3. Selecteer **Kopiëren** naast de **verbindingsreeks** om de handtekening naar het klembord te kopiëren. Selecteer daarna **Sluiten**.

### <a name="attach-to-the-shared-account-by-using-the-sas"></a>Koppelen aan het gedeelde account met behulp van de SAS
1. Selecteer in Storage Explorer (voorbeeld) **Verbinding maken met Azure Storage**.

    ![Verbinding maken met de optie Azure Storage][23]

2. Geef in het dialoogvenster **Verbinding maken met Azure Storage** de verbindingsreeks op en selecteer **Volgende**.

    ![Verbinding maken met het dialoogvenster Azure Storage][24]

3. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen wilt aanbrengen, selecteert u **Terug**, en voert u vervolgens de gewenste instellingen in. 

4. Selecteer **Verbinden**.

5. Nadat het opslagaccount is gekoppeld, wordt het weergegeven met de **(SAS)** toegevoegd aan de door u opgegeven accountnaam.

    ![Resultaat van koppeling met een account via SAS][17]

## <a name="attach-a-service-by-using-an-sas"></a>Een service koppelen met behulp van een SAS
In het gedeelte Een opslagaccount koppelen via SAS wordt uitgelegd hoe de beheerder van een Azure-abonnement tijdelijke toegang kan verlenen tot een opslagaccount door een SAS te genereren en te delen voor het opslagaccount. Op deze manier kunt u ook een SAS genereren voor een bepaalde service (blobcontainer, wachtrij of tabel) binnen een opslagaccount.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Een SAS genereren voor de service die u wilt delen
In deze context is een service een blobcontainer, een wachtrij of een tabel. Zie voor het genereren van de SAS voor een vermelde service:

* [De SAS ophalen voor een blobcontainer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* De SAS ophalen voor het delen van bestanden: *binnenkort beschikbaar*
* De SAS ophalen voor een wachtrij: *binnenkort beschikbaar*
* De SAS ophalen voor een tabel: *binnenkort beschikbaar*

### <a name="attach-to-the-shared-account-service-by-using-the-sas"></a>Koppelen aan de gedeelde-accountservice met behulp van de SAS
1. Selecteer in Storage Explorer (voorbeeld) **Verbinding maken met Azure Storage**.

    ![Verbinding maken met de optie Azure Storage][23]

2. Geef in het dialoogvenster **Verbinding maken met Azure Storage** de SAS-URI op en selecteer vervolgens **Volgende**.

    ![Verbinding maken met het dialoogvenster Azure Storage][24]

3. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen wilt aanbrengen, selecteert u **Terug**, en voert u vervolgens de gewenste instellingen in. 

4. Selecteer **Verbinden**.

5. Na het koppelen wordt de nieuw gekoppelde service weergegeven onder het knooppunt **(Service-SAS)**.

    ![Resultaat van het koppelen met een gedeelde-service met behulp van een SAS][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Verbinding maken met een Azure DB die Cosmos-account via een verbindingsreeks
Naast het beheer van Azure DB die Cosmos accounts via Azure-abonnement moet een andere manier van verbinding maken met een Cosmos Azure DB is het gebruik van een verbindingsreeks. Gebruik de volgende stappen uit om te verbinden via een verbindingsreeks.

1. Vinden **Local en gekoppelde** in het linkerdeelvenster, met de rechtermuisknop op **Azure Cosmos DB Accounts**, kies **verbinding maken met Azure Cosmos DB...**

    ![verbinding maken met Azure Cosmos DB met verbindingsreeks][33]

2. Kies Azure Cosmos DB API, plak uw **verbindingsreeks**, en klik vervolgens op **OK** om te verbinden Azure DB die Cosmos-account. Zie voor informatie over het ophalen van de verbindingsreeks, [de verbindingsreeks ophalen](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![connection-string][32]

## <a name="search-for-storage-accounts"></a>Zoeken naar opslagaccounts
Als u een lange lijst met opslagaccounts hebt, kunt u via het zoekvak boven aan het linkerdeelvenster snel bepaalde opslagaccounts zoeken.

Terwijl u in het zoekvak typt, worden in het linkerdeelvenster alleen de opslagaccounts weergegeven die overeenkomen met de zoekwaarde die u tot dan toe hebt ingevoerd. Op de volgende schermafbeelding wordt bijvoorbeeld een zoekopdracht weergegeven voor alle opslagaccounts waarvan de naam **tarcher** bevat:

![Zoeken naar Storage-account][11]

## <a name="next-steps"></a>Volgende stappen
* [Azure Blob Storage-resources beheren met Opslagverkenner (Preview)](vs-azure-tools-storage-explorer-blobs.md)
* [Beheren van Azure Cosmos DB in Azure Opslagverkenner (Preview)](./cosmos-db/storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
[32]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.PNG
[33]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.PNG
