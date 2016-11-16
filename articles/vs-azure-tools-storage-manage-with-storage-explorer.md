---
title: Aan de slag met Opslagverkenner (Preview) | Microsoft Docs
description: Azure Storage-resources beheren met Opslagverkenner (Preview)
services: storage
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d5c003c9240d8625d2921666e1361ce0512138bb


---
# <a name="getting-started-with-storage-explorer-preview"></a>Aan de slag met Opslagverkenner (Preview)
## <a name="overview"></a>Overzicht
Microsoft Azure Storage Exporer (Preview) is een zelfstandige app waardoor u eenvoudig met Azure Storage-gegevens kunt werken via Windows, OSX en Linux. In dit artikel leert u op welke manieren u verbinding kunt maken met Azure Storage-accounts en hoe u deze kunt beheren.

![Microsoft Azure Storage Explorer (voorbeeld)][15]

## <a name="prerequisites"></a>Vereisten
* [Opslagverkenner (preview) downloaden en installeren](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Verbinding maken met een opslagaccount of -service
Opslagverkenner (Preview) biedt allerlei manieren om verbinding te maken met opslagaccounts. Dit omvat het verbinden met de opslagaccounts die zijn gekoppeld aan uw Azure-abonnementen, het verbinden met de opslagaccounts en services die worden gedeeld via andere Azure-abonnementen en het verbinden met en beheren van lokale opslag met de Azure-opslagemulator:

* [Verbinding maken met een Azure-abonnement](#connect-to-an-azure-subscription): beheer opslagresources die deel uitmaken van uw Azure-abonnement.
* [Verbinding maken met lokale opslag](#work-with-local-development-storage) - Beheer lokale opslag met de Azure Storage-emulator. 
* [Koppelen aan externe opslag](#attach-or-detach-an-external-storage-account): beheer de opslagresources die deel uitmaken van een ander Azure-abonnement via de naam en sleutel van het opslagaccount.
* [Een account koppelen via SAS](#attach-storage-account-using-sas) - Beheer de opslagresources van een ander Azure-abonnement met een SAS.
* [Een service koppelen via SAS](#attach-service-using-sas): beheer een specifieke opslagservice (blobcontainer, wachtrij of tabel) die deel uitmaakt van een ander Azure-abonnement. Dit gebeurt middels een SAS.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement
> [!NOTE]
> Als u geen Azure-account hebt, kunt u zich [aanmelden voor een gratis proefversie](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) of [gebruikmaken van uw voordelen als Visual Studio-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
> 
> 

1. Selecteer in Storage Explorer (voorbeeld) **Azure-accountinstellingen**. 
   
    ![Azure-accountinstellingen][0]
2. In het linkerdeelvenster verschijnen alle Microsoft-accounts waarop u bent aangemeld. Selecteer **Een account toevoegen** om verbinding te maken met een ander account en volg de dialoogvensters om aan te melden met een Microsoft-account dat is gekoppeld aan ten minste één actief Azure-abonnement.
   
    ![Een account toevoegen][1]
3. Nadat u succesvol bent aangemeld met een Microsoft-account, verschijnen in het linkerdeelvenster de Azure-abonnementen gekoppeld aan dat account. Selecteer de Azure-abonnementen waarmee u wilt werken en selecteer vervolgens **Toepassen**. (Door **Alle abonnementen** te selecteren selecteert u alle of geen Azure-abonnementen.)
   
    ![Selecteer Azure-abonnementen][3]
4. In het linkerdeelvenster worden de opslagaccounts weergegeven die zijn gekoppeld aan de geselecteerde Azure-abonnementen.
   
    ![Geselecteerde Azure-abonnementen][4]

## <a name="work-with-local-development-storage"></a>Werken met lokale ontwikkelingsopslag
Met Opslagverkenner (Preview) kunt u met lokale opslag werken via de Azure-opslagemulator. Hiermee kunt u code schrijven voor opslag en deze testen zonder dat u een opslagaccount hoeft te hebben geïmplementeerd in Azure (omdat het opslagaccount wordt gesimuleerd door de Azure-opslagemulator).

> [!NOTE]
> De Azure-opslagemulator wordt momenteel alleen door Windows ondersteund. 
> 
> 

1. Klap in het linkerdeelvenster van de Storage Explorer (voorbeeld) de node **(lokale en bijgevoegde ** > **Opslagaccounts** > **(ontwikkeling)** uit.
   
    ![Node lokale ontwikkeling][21]
2. Als u de Azure-opslagemulator nog niet hebt geïnstalleerd, wordt u via de informatiebalk gevraagd dit te doen. Wanneer de informatiebalk wordt weergegeven, selecteert u **De nieuwste versie downloaden** en installeert u de emulator. 
   
    ![Azure Storage-emulator prompt downloaden][22]
3. Wanneer de emulator is geïnstalleerd, hebt u de mogelijkheid om lokale blobs, wachtrijen en tabellen te maken en ermee te werken. Als u wilt leren hoe u met elk type opslagaccount werkt, selecteert u de bijbehorende koppelingen hieronder:
   
   * [Resources voor Azure Blob Storage beheren](vs-azure-tools-storage-explorer-blobs.md)
   * Opslagresources voor het delen van Azure-bestanden beheren - *Binnenkort beschikbaar*
   * Resources voor Azure Queue Storage beheren - *Binnenkort beschikbaar*
   * Resources voor Azure Table Storage beheren - *Binnenkort beschikbaar*

## <a name="attach-or-detach-an-external-storage-account"></a>Koppelen aan een extern opslagaccount of de koppeling opheffen
Opslagverkenner (Preview) biedt de mogelijkheid om externe opslagaccounts te koppelen zodat u opslagaccounts eenvoudig kunt delen. In dit gedeelte wordt uitgelegd hoe u externe opslagaccounts koppelt (en hoe u de koppeling opheft).

### <a name="get-the-storage-account-credentials"></a>De opslagaccountreferenties ophalen
Als u een extern opslagaccount wilt delen, moet de eigenaar van dat account eerst de referenties ontvangen (accountnaam en -sleutel) voor het account en daarna die informatie delen met de persoon die wil verbinden met het (externe) account. U kunt de opslagaccountreferenties verkrijgen via de Azure Portal door deze stappen te volgen: 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Selecteer **Bladeren**.
3. Selecteer **Opslagaccounts**.
4. Op de blade **Opslagaccounts** selecteert u het gewenste opslagaccount.
5. Op de blade **Instellingen** van het geselecteerde opslagaccount selecteert u **Toegangssleutels**.
   
   ![Optie Toegangssleutels][5]
6. In de blade **Toegangssleutels** kopieert u de waarden **STORAGE ACCOUNT NAME** en **KEY 1** voor gebruik bij het koppelen aan het opslagaccount. 
   
   ![Toegangssleutels][6]

### <a name="attach-to-an-external-storage-account"></a>Koppelen aan een extern opslagaccount
Om een extern opslagaccount te koppelen, moet u de accountaam en -sleutel opgeven. In het gedeelte *De opslagaccountreferenties ophalen* wordt uitgelegd hoe u deze waarden verkrijgt in het Azure-portal. Merk op dat in het portaal de accountsleutel “sleutel 1” wordt genoemd, dus als de Storage Explorer (voorbeeld) vraagt naar een accountsleutel, geeft u (of plakt u) de waarde van “sleutel 1” in. 

1. Selecteer in Storage Explorer (voorbeeld) **Verbinding maken met Azure Storage**.
   
   ![Verbinding maken met de optie Azure Storage][23]
2. Geef in het dialoogvenster **Verbinding maken met Azure Storage** de accountsleutel (waarde "sleutel 1" van het Azure-portal in), en selecteer **Volgende**.
   
   ![Verbinding maken met het dialoogvenster Azure Storage][24] 
3. In het dialoogvenster **Externe opslag koppelen** voert u de naam van het opslagaccount in het vak **Accountnaam** en voert u andere gewenste instellingen in. Selecteer **Volgende** als u klaar bent. 
   
   ![Dialoogvenster Externe opslag koppelen][8]
4. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen aanbrengen wilt, selecteer **Terug** en geef de gewenste instellingen in. Selecteer **Verbinding maken** als u klaar bent.
5. Na het koppelen wordt het externe opslagaccount weergegeven met de tekst **(Extern)** toegevoegd aan de opslagaccountnaam. 
   
   ![Resultaat van koppelen met een extern opslagaccount][9]

### <a name="detach-from-an-external-storage-account"></a>De koppeling met een extern opslagaccount opheffen
1. Klik met de rechtermuisknop op het externe opslagaccount dat u wilt loskoppelen en selecteer in het contextmenu **Loskoppelen**.
   
   ![Loskoppelen van de opslagoptie][10]
2. Wanneer het bevestigingsbericht wordt weergegeven, selecteert u **Ja** om het loskoppelen van het externe opslagaccount te bevestigen.

## <a name="attach-storage-account-using-sas"></a>Een opslagaccount koppelen via SAS
Een [SAS (Shared Access Signature)](storage/storage-dotnet-shared-access-signature-part-1.md) biedt de beheerder van een Azure-abonnement de mogelijkheid om tijdelijk toegang te verlenen tot een opslagaccount zonder dat daarvoor de Azure-abonnementsreferenties hoeven worden opgegeven. 

Een voorbeeld: stel gebruiker A is beheerder van een Azure-abonnement en gebruiker A wil gebruiker B een beperkte tijd toegang bieden tot een opslagaccount, met bepaalde machtigingen:

1. Gebruiker A genereert een SAS (bestaande uit de verbindingsreeks voor het opslagaccount) die een bepaalde tijd geldig is en de gewenste machtigingen heeft.
2. Gebruiker A deelt de SAS met de persoon die toegang wil tot het opslagaccount (gebruiker B, in dit geval).  
3. Gebruiker B gebruikt Opslagverkenner (Preview) om verbinding te maken met het account van gebruiker A middels de opgegeven SAS. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Een SAS ophalen voor het account dat u wilt delen
1. In de Storage Explorer (voorbeeld) klikt u met de rechtermuisknop op het Storage Account dat u wilt delen. Selecteer in het contextmenu de optie **Shared Access Signature ophalen**.
   
   ![Optie SAS-contextmenu ophalen][13]
2. In het dialoogvenster **Shared Access Signature** geeft u het tijdsbestek op en de machtigingen die u wilt gebruiken voor het account. Selecteer vervolgens **Maken**.
   
    ![SAS-dialoogvenster ophalen][14]
3. Er wordt een tweede dialoogvenster **Shared Access Signature** weergegeven. Hierin staat de SAS. Selecteer **Kopiëren** naast de **verbindingsreeks** om de handtekening naar het klembord te kopiëren. Selecteer **Sluiten** om het dialoogvenster te sluiten.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Koppelen aan het gedeelde account met behulp van de SAS
1. Selecteer in Storage Explorer (voorbeeld) **Verbinding maken met Azure Storage**.
   
   ![Verbinding maken met de optie Azure Storage][23]
2. Geef op het dialoogvenster **Verbinding maken** met Azure Storage de verbindingsreeks op en selecteer dan **Volgende**.
   
   ![Verbinding maken met het dialoogvenster Azure Storage][24] 
3. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen aanbrengen wilt, selecteer **Terug** en geef de gewenste instellingen in. Selecteer **Verbinding maken** als u klaar bent.
4. Na het koppelen wordt het opslagaccount weergegeven met de tekst (SAS) toegevoegd aan de opgegeven accountnaam.
   
   ![Resultaat van gekoppeld met een account via SAS][17]

## <a name="attach-service-using-sas"></a>Een service koppelen via SAS
In het gedeelte [Een account koppelen via SAS](#attach-storage-account-using-sas) leest u hoe de beheerder van een Azure-abonnement tijdelijke toegang kan verlenen tot een opslagaccount door een SAS te genereren (en delen) voor het opslagaccount. Op deze manier kunt u ook een SAS genereren voor een bepaalde service (blobcontainer, wachtrij of tabel) binnen een opslagaccount.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Een SAS genereren voor de service die u wilt delen
In deze context is een service een blobcontainer, een wachtrij of een tabel. In de volgende gedeelten wordt uitgelegd hoe u een SAS maakt voor de vermelde service:

* [De SAS ophalen voor een blobcontainer](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* De SAS ophalen voor het delen van bestanden - *Binnenkort beschikbaar*
* De SAS ophalen voor een wachtrij - *binnenkort beschikbaar*
* De SAS ophalen voor een tabel - *binnenkort beschikbaar*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Koppelen aan de gedeelde-accountservice met behulp van de SAS
1. Selecteer in Storage Explorer (voorbeeld) **Verbinding maken met Azure Storage**.
   
   ![Verbinding maken met de optie Azure Storage][23]
2. Op de **verbinding maken met Azure Storage** dialoogvenster, geef de SAS-URI en selecteer vervolgens **volgende**.
   
   ![Verbinding maken met het dialoogvenster Azure Storage][24] 
3. In het dialoogvenster **Samenvatting verbinding** controleert u de gegevens. Als u wijzigingen aanbrengen wilt, selecteer **Terug** en geef de gewenste instellingen in. Selecteer **Verbinding maken** als u klaar bent.
4. Na het koppelen wordt de service weergegeven onder het knooppunt **(service-SAS)**.
   
   ![Resultaat van het koppelen met een gedeelde-service met SAS][20]

## <a name="search-for-storage-accounts"></a>Zoeken naar opslagaccounts
Als u een lange lijst met opslagaccounts hebt, kunt u via het zoekvak boven aan het linkerdeelvenster snel bepaalde opslagaccounts zoeken. 

Wanneer u in het zoekvak typt, worden in het linkerdeelvenster alleen de opslagaccounts weergegeven die overeenkomen met de zoekwaarde die u tot dan toe hebt ingevoerd. In de volgende schermafbeelding ziet u een voorbeeld van een zoekopdracht. De naam van het opslagaccount moet de tekst "tarcher" bevatten.

![Zoeken naar Storage-account][11]

Als u de zoekopdracht wilt wissen, klikt u op **x** in het zoekvak.

## <a name="next-steps"></a>Volgende stappen
* [Azure Blob Storage-resources beheren met Opslagverkenner (Preview)](vs-azure-tools-storage-explorer-blobs.md)

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



<!--HONumber=Nov16_HO2-->


