---
title: Azure Data Lake Store-resources beheren in Azure Storage Explorer
description: Geeft gebruikers toegang tot uw ADLS-gegevens en -resources en stelt hen in staat om die te beheren in Azure Storage Explorer
Keywords: Azure Data Lake Store, Azure Storage Explorer
services: Data Lake Store
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
ms.assetid: 
ms.service: Data Lake Store
ms.custom: Azure Data Lake Store
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/05/2018
ms.author: jejiang
ms.openlocfilehash: 4c69acebceac6719cf3f12ff0a93879f1b5a8943
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="manage-azure-data-lake-store-resources-with-storage-explorer-preview"></a>Azure Data Lake Store-resources beheren met Storage Explorer (preview)

[Azure Data Lake Store (ADLS)](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview) is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekstgegevens of binaire gegevens. Gebruikers kunnen overal toegang tot de gegevens krijgen via HTTP of HTTPS. ADLS in Azure Storage Explorer geeft gebruikers toegang tot ADLS-gegevens en resources plus andere Azure-entiteiten, zoals blobs en wachtrijen, en stelt hen in staat die te beheren. Nu kunnen gebruikers hetzelfde hulpprogramma gebruiken om hun verschillende Azure-entiteiten op één plek te beheren.

Een ander voordeel is dat gebruikers geen toestemming op basis van een abonnement nodig hebben om ADLS-gegevens te beheren. In Storage Explorer kunnen gebruikers het ADLS-pad toevoegen aan het knooppunt ‘Lokaal en gekoppeld’ mits anderen de toestemming geven.

## <a name="prerequisites"></a>Vereisten
U moet de volgende vereiste zaken hebben om de stappen in dit artikel uit te voeren:

*   Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial).
*   Een Azure Data Lake Store-account. Zie [Aan de slag met Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal) voor instructies over hoe u zo’n account maakt

## <a name="installation"></a>Installatie

Installeer hier de nieuwste Azure Storage Explorer-bits: [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/). We bieden nu ondersteuning voor Windows, Linux en Mac.

## <a name="connect-to-an-azure-subscription"></a>Verbinding maken met een Azure-abonnement

1. Klik nadat u **Azure Storage Explorer** heb geïnstalleerd op het pictogram **invoegtoepassing** aan de linkerkant, zoals in de volgende afbeelding.
       
   ![Pictogram invoegtoepassing](./media/data-lake-store-in-storage-explorer/plug-in-icon.png)
 
2. Selecteer **Een Azure-account toevoegen** en klik op **Aanmelden**.

   ![Verbinding maken met Azure-abonnement](./media/data-lake-store-in-storage-explorer/connect-to-azure-subscription.png)

2. Selecteer **Aanmelden** in het **aanmeldingsvenster van Azure** en voer uw Azure-referenties in.

    ![Aanmelden](./media/data-lake-store-in-storage-explorer/sign-in.png)

3. Selecteer uw abonnement in de lijst en klik op **Toepassen**.

    ![Toepassen](./media/data-lake-store-in-storage-explorer/apply-subscription.png)

    Het Verkenner-venster wordt bijgewerkt en de accounts in het geselecteerde abonnement worden weergegeven.

    ![Lijst met accounts](./media/data-lake-store-in-storage-explorer/account-list.png)

    U hebt uw **Azure Data Lake Store** gekoppeld aan uw Azure-abonnement.

## <a name="connect-to-data-lake-store"></a>Verbinding maken met Data Lake Store
Als u toegang wilt krijgen tot de resources die niet aanwezig zijn in uw abonnement, maar anderen de URI voor de resources aan u geven. In dit geval kunt u de URI gebruiken om verbinding te maken met Data Lake Store nadat u bent aangemeld. Bekijk de volgende stappen.
1. Open Opslagverkenner (Preview).
2. Vouw **Lokaal en gekoppeld** uit in het linkerdeelvenster.
3. Klik met uw rechtermuisknop op **Data Lake Store** en selecteer **Verbinding maken met Data Lake Store...** in het contextmenu.

      ![contextmenu Verbinding maken met Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Voer de URI in. Vervolgens navigeert het hulpprogramma naar de locatie van de URL die u zojuist hebt ingevoerd.

      ![dialoogvenster Verbinding maken met Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

      ![resultaat Verbinding maken met Data Lake Store](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="view-an-azure-data-lake-store-accounts-contents"></a>De inhoud van een Azure Data Lake Store-account bekijken
De resources van een Azure Data Lake Store-account bevatten mappen en bestanden.

Volg de volgende stappen om de inhoud van een ADLS-account weer te geven binnen Storage Explorer (preview):

1. Open Opslagverkenner (Preview).
2. Vouw het abonnement met het Azure Data Lake Store-account dat u wilt bekijken, uit in het linkerdeelvenster.
3. Vouw **Data Lake Store** uit.
4. Klik met uw rechtermuisknop op het knooppunt van het Azure Data Lake Store-account dat u wilt bekijken en selecteer **Openen** in het contextmenu. U kunt ook dubbelklikken op het ADLS-account om het te openen. 
5. In het hoofdvenster wordt de inhoud van het ADLS-account weergegeven.

     ![hoofdvenster](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-toolbar-mainpane.png) 

## <a name="azure-data-lake-store-resources-management"></a>Beheer van Azure Data Lake Store-resources

U kunt Azure Data Lake Store-resources beheren door de volgende bewerkingen uit te voeren:
*   Navigeer door ADLS-resources in meerdere ADL-accounts.  
*   Gebruik een verbindingsreeks om rechtstreeks verbinding te maken met ADLS en om ADLS rechtstreeks te beheren. 
*   Bekijk ADLS-resources die door anderen via ACL zijn gedeeld onder ‘Lokaal en gekoppeld’.
*   CRUD-bewerkingen uitvoeren voor bestanden/mappen: ondersteuning voor recursieve map en meervoudig geselecteerde bestanden. 
*   Sleep een map naar Snelle toegang en Recente locaties. Dit zorgt voor een ervaring die vergelijkbaar is met de verkenner op het bureaublad. 
*   Kopieer de ADL-hyperlink en open die met één muisklik in Storage Explorer. 
*   Geef het activiteitenlogboek weer in het deelvenster rechtsonder om de activiteitsstatus te bekijken.
*   Geef de mapstatistieken en de bestandseigenschap weer.

## <a name="manage-resources-in-azure-storage-explorer"></a>Beheer resources in Azure Storage Explorer
Zodra u een Azure Data Lake Store-account hebt gemaakt, kunt u mappen en bestanden uploaden en resources downloaden en openen op uw lokale computer. En u hebt de opties Vastmaken aan Snelle toegang, Nieuwe map, URL kopiëren, Alles selecteren. Bovendien hebt u ook nog de opties kopiëren, plakken, naam wijzigen, verwijderen, mapstatistieken, vernieuwen.

De volgende items laten zien hoe u resources binnen een Azure Data Lake Store-account kunt beheren. Volg hieronder de stappen voor de bewerking die u wilt uitvoeren:
   * **Bestanden uploaden**

     1. Selecteer **Uploaden** op de werkbalk van het hoofdvenster en selecteer vervolgens **Bestanden uploaden** in de vervolgkeuzelijst.

        ![Menu voor Bestanden uploaden](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-menu.png) 

     2. Selecteer de bestanden die u wilt uploaden in het dialoogvenster **Bestanden selecteren voor uploaden**.

        ![Dialoogvenster voor Map uploaden](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-files-dialog.png)

     3. Selecteer **Openen** om te beginnen met uploaden.
   * **Een map uploaden**

     1. Selecteer **Uploaden** op de werkbalk van het hoofdvenster en selecteer vervolgens **Map uploaden** in de vervolgkeuzelijst.

        ![Menu voor uploaden van map](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-menu.png) 
     2. Selecteer een map die u wilt uploaden in het dialoogvenster **Map selecteren voor uploaden**.

        ![Dialoogvenster voor Map uploaden](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-dialog.png)      

     3. Selecteer **Map selecteren** om te beginnen met het uploaden van mappen.

        ![Dialoogvenster voor Map uploaden](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-upload-folder-drag.png) 

      > [!NOTE] 
          > U kunt de mappen en bestanden rechtstreeks in de lokale computer slepen om de upload uit te voeren. 
       
   * **Mappen of bestanden downloaden naar uw lokale computer**

     1. Selecteer de mappen of bestanden die u wilt downloaden.
     2. Selecteer **Downloaden** op de werkbalk van het hoofdvenster.
     3. Geef de locaties waarnaar u de mappen of bestanden wilt downloaden op in het dialoogvenster **Een map selecteren om de gedownloade bestanden in op te slaan**. En geef de naam op die u aan de map wilt geven.
     4. Selecteer **Opslaan**.
   * **Een bestand of map openen uit uw lokale computer**

     1. Selecteer de map of het bestand dat u wilt openen.
     2. Selecteer **Openen** op de werkbalk van het hoofdvenster of klik met uw rechtermuisknop op de geselecteerde map of het geselecteerde bestand en klik vervolgens op **Openen** in het contextmenu.
     3. Het bestand wordt gedownload en geopend met de toepassing die is gekoppeld aan het onderliggende bestandstype van het bestand. Of de map wordt in het hoofdvenster geopend.

        ![bestand openen](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-open.png) 

   * **Mappen of bestanden kopiëren naar het klembord**

     1. Selecteer de mappen of bestanden die u wilt kopiëren.
     2. Selecteer **Kopiëren** op de werkbalk van het hoofdvenster of klik met uw rechtermuisknop op de geselecteerde mappen of bestanden en klik vervolgens op **Kopiëren** in het contextmenu.
     3. Ga in het linkerdeelvenster naar een ander ADLS-account en dubbelklik hierop om het weer te geven in het hoofdvenster.
     4. Selecteer **Plakken** op de werkbalk van het hoofdvenster om een kopie te maken. Of klik op **Plakken** in het contextmenu in het doel.

        ![map of bestand kopiëren plakken](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-copy-paste.png)

      > [!NOTE] 
          > + Kopiëren en plakken tussen verschillende opslagtypen **wordt niet** ondersteund. U kunt ADLS-mappen of -bestanden kopiëren en in een ander ADLS-account plakken. Maar u kunt ADLS-mappen of -bestanden **niet** kopiëren en vervolgens in Blob Storage plakken, of andersom. 
          > + Het kopiëren en plakken werkt als u de mappen of bestanden naar de lokale opslag downloadt en vervolgens naar het doel uploadt. Het hulpprogramma voert de actie **niet** uit in de back-end. Het kopiëren en plakken van grote bestanden verloopt traag. Het geavanceerd kopiëren en verplaatsen van bestanden wordt momenteel geoptimaliseerd.
   * **Mappen of bestanden verwijderen**

     1. Selecteer de mappen of bestanden die u wilt verwijderen.
     2. Selecteer **Verwijderen** op de werkbalk van het hoofdvenster of klik met uw rechtermuisknop op de geselecteerde mappen of bestanden en klik vervolgens op **Verwijderen** in het contextmenu.
     3. Selecteer **Ja** in het bevestigingsvenster.

        ![map of bestand kopiëren plakken](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-delete.png)

   * **Vastmaken aan Snelle toegang**

     1. Selecteer de map die u wilt vastmaken.
     2. Selecteer **Vastmaken aan Snelle toegang** op de werkbalk van het hoofdvenster.
     3. In het linkerdeelvenster ziet u dat de geselecteerde map is toegevoegd aan het knooppunt **Snelle toegang**.

        ![vastmaken aan snelle toegang](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-quick-access.png)
     4. Nadat u Snelle toegang hebt gemaakt, kunt u eenvoudig toegang krijgen tot de resources.
   * **Dieptekoppelingen**
     1. Als u een URL hebt, kunt u gewoon de URL invoeren in het adrespad in de **Verkenner** of in de browser.
     2. Vervolgens wordt **Storage Explorer.exe** automatisch gestart en navigeert die naar de locatie van de URL die u zojuist hebt ingevoerd.

        ![dieptekoppeling in verkenner](./media/data-lake-store-in-storage-explorer/storageexplorer-adls-deep-link.png)


## <a name="next-steps"></a>Volgende stappen
* De [meest recente releaseopmerkingen en video's van Opslagverkenner (preview)](http://www.storageexplorer.com) bekijken.
* Informatie over [Het beheer van Azure Cosmos DB in Azure Storage Explorer](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer)
* Meer informatie over Storage Explorer[Aan de slag met Storage Explorer (Preview)](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* Aan de slag met Azure Data Lake Store (ADLS)[Overzicht van Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-overview)
* Bekijk de volgende video voor informatie over [het gebruik van Azure Cosmos DB in Azure Storage Explorer](https://www.youtube.com/watch?v=iNIbg1DLgWo&feature=youtu.be)
