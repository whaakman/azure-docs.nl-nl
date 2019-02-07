---
title: XML transformeren met XSLT-toewijzingen - Azure Logic Apps | Microsoft Docs
description: XSLT-toewijzingen voor het transformeren van XML-code in Azure Logic Apps met Enterprise Integration Pack toevoegen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
manager: carmonm
ms.topic: article
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.date: 02/06/2019
ms.openlocfilehash: da5b099a5574d34c3676819c930f3e89610cf4ad
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55767432"
---
# <a name="transform-xml-with-maps-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML transformeren met kaarten in Azure Logic Apps met Enterprise Integration Pack

Om over te dragen XML-gegevens tussen notaties voor scenario's enterprise integration in Azure Logic Apps, uw logische app kaarten kunt gebruiken of meer specifiek, Stylesheet transformaties XSLT (Extensible Language) wordt toegewezen. Een kaart is een XML-document dat wordt beschreven hoe u gegevens uit een XML-document converteren naar een andere indeling. 

Stel bijvoorbeeld dat u B2B orders of facturen regelmatig van een klant die gebruikmaakt van de datumnotatie YYYMMDD ontvangt. Echter, de datumnotatie MMDDYYY maakt gebruik van uw organisatie. U kunt definiëren en gebruiken van een kaart waarop de datumnotatie YYYMMDD naar de indeling MMDDYYY voordat u de details van de volgorde of per factuur opslaat in uw klantendatabase activiteit transformeert.

Zie voor beperkingen met betrekking tot integratieaccounts en artefacten, zoals kaarten, [limieten en configuratie-informatie voor Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, <a href="https://azure.microsoft.com/free/" target="_blank">meld u dan aan voor een gratis Azure-account</a>.

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) waar u uw maps en andere artefacten voor bedrijfsintegratie en oplossingen, business-to-business (B2B) opslaan.

* Als uw kaart verwijst naar een externe assembly, u moet uploaden *zowel de assembly en de kaart* aan uw integratie-account. Zorg ervoor dat u *eerst uw assembly uploaden*, en upload de toewijzing die verwijst naar de assembly.

  Als uw assembly 2 MB is of kleiner, u uw assembly aan uw integratieaccount toevoegen kunt *rechtstreeks* vanuit Azure portal. Echter, als uw assembly of de kaart groter dan 2 MB, maar niet groter zijn dan is de [maximale grootte voor assembly's of kaarten](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits), beschikt u over deze opties:

  * Voor assembly's moet u een Azure blob-container waarin u uw assembly en de locatie van de container kunt uploaden. Op die manier kunt u opgeven die locatie later wanneer u de assembly aan uw integratie-account toevoegen. 
  Voor deze taak moet u deze items:

    | Item | Description |
    |------|-------------|
    | [Azure Storage-account](../storage/common/storage-account-overview.md) | In dit account door een Azure blob-container te maken voor uw assembly. Informatie over [over het maken van een storage-account](../storage/common/storage-quickstart-create-account.md). |
    | Blob container | In deze container, kunt u uw assembly uploaden. U moet ook de locatie van de container wanneer u de assembly aan uw integratie-account toevoegen. Meer informatie over het [maken van een blob-container](../storage/blobs/storage-quickstart-blobs-portal.md). |
    | [Azure-opslagverkenner](../vs-azure-tools-storage-manage-with-storage-explorer.md) | Dit hulpprogramma kunt u meer eenvoudig het beheer van opslagaccounts en blob-containers. Gebruik van Storage Explorer, ofwel [downloaden en installeren van Azure Storage Explorer](https://www.storageexplorer.com/). Koppel vervolgens Storage Explorer aan uw storage-account met de volgende stappen in [aan de slag met Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md). Raadpleeg voor meer informatie [Snelstart: Een blob maken in objectopslag met Azure Storage Explorer](../storage/blobs/storage-quickstart-blobs-storage-explorer.md). <p>Of, in de Azure-portal, zoek en selecteer uw storage-account. Selecteer in het menu van uw opslagaccount, **Opslagverkenner**. |
    |||

  * Voor kaarten, kunt u momenteel groter toewijzingen toevoegen met behulp van de [REST API van Azure Logic Apps - kaarten](https://docs.microsoft.com/rest/api/logic/maps/createorupdate).

U hoeft geen een logische app bij het maken en toevoegen van maps. Voor het gebruik van een kaart, uw logische app moet echter koppelen aan een integratieaccount waar u die zijn toegewezen opslaat. Informatie over [logische apps koppelen aan integratieaccounts](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account). Als u een logische app nog niet hebt, krijgt u informatie [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-referenced-assemblies"></a>Waarnaar wordt verwezen, assembly's toevoegen

1. Gebruik de referenties van uw Azure-account om u aan melden bij het <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

1. Als u wilt zoeken en openen van uw integratie-account, op de Azure-hoofdmenu in, selecteer **alle services**. 
   Voer in het zoekvak ' integratieaccount'. 
   Selecteer **integratieaccounts**.

   ![Integratie-account zoeken](./media/logic-apps-enterprise-integration-maps/find-integration-account.png)

1. Selecteer het integratieaccount waar u wilt toevoegen de assembly, bijvoorbeeld:

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-maps/select-integration-account.png)

1. Op uw integratie-account **overzicht** pagina onder **onderdelen**, selecteer de **assembly's** tegel.

   ![Selecteer "Assembly's"](./media/logic-apps-enterprise-integration-maps/select-assemblies.png)

1. Na de **assembly's** pagina wordt geopend, kiest u **toevoegen**.

   ![Kies 'Toevoegen'](./media/logic-apps-enterprise-integration-maps/add-assembly.png)

Op basis van de grootte van de assemblybestand, volgt u de stappen voor het uploaden van een assembly die ofwel [maximaal 2 MB](#smaller-assembly) of [meer dan 2 MB, maar alleen tot 8 MB](#larger-assembly).
Zie voor de limieten voor assembly hoeveelheden in de integratieaccounts, [limieten en configuratie voor Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits).

<a name="smaller-assembly"></a>

### <a name="add-assemblies-up-to-2-mb"></a>Assembly's toevoegen tot 2 MB

1. Onder **Assembly toevoegen**, voer een naam voor de assembly. Houd **klein bestand** geselecteerde. Naast de **Assembly** Kies het pictogram van de map. Zoek en selecteer de assembly die u, bijvoorbeeld uploadt:

   ![Kleinere assembly uploaden](./media/logic-apps-enterprise-integration-maps/upload-assembly-file.png)

   In de **assemblynaam** eigenschap, de bestandsnaam van de assembly wordt automatisch weergegeven nadat u de assembly hebt geselecteerd.

1. Wanneer u klaar bent, kiest u **OK**.

   Nadat uw assemblybestand geüpload is, de assembly wordt weergegeven in de **assembly's** lijst.

   ![Lijst met geüploade assembly 's](./media/logic-apps-enterprise-integration-maps/uploaded-assemblies-list.png)

   Op uw integratie-account **overzicht** pagina onder **onderdelen**, wordt de **assembly's** tegel wordt nu het aantal geüploade assembly's, bijvoorbeeld:

   ![Geüploade assembly 's](./media/logic-apps-enterprise-integration-maps/uploaded-assemblies.png)

<a name="larger-assembly"></a>

### <a name="add-assemblies-more-than-2-mb"></a>Assembly's meer dan 2 MB toevoegen

Om toe te voegen grotere assembly's, kunt u uw assembly uploaden naar een Azure blob-container in uw Azure storage-account. De stappen voor het toevoegen van assembly's verschilt op basis van of uw blob-container, openbare leestoegang heeft. Dus eerst controleren of uw blob-container openbare leestoegang heeft door de volgende stappen: [Openbaar toegangsniveau voor blob-container instellen](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### <a name="check-container-access-level"></a>Niveau van de container toegang controleren

1. Open Azure Storage Explorer. Vouw in het Explorer-venster uit uw Azure-abonnement als nog niet is uitgevouwen.

1. Vouw **Opslagaccounts** > {*uw storage-account*} > **Blob-Containers**. Selecteer uw blob-container.

1. Selecteer in het snelmenu van uw blobcontainer, **niveau openbare toegang instelt**.

   * Als uw blob-container ten minste openbare toegang heeft, kiest u **annuleren**, en volg deze stappen verderop op deze pagina: [Uploaden naar containers met openbare toegang](#public-access-assemblies)

     ![Openbare toegang](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * Als uw blob-container geen openbare toegang heeft, kiest u **annuleren**, en volg deze stappen verderop op deze pagina: [Uploaden naar containers zonder openbare toegang](#no-public-access-assemblies)

     ![Er zijn geen openbare toegang](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access-assemblies"></a>

#### <a name="upload-to-containers-with-public-access"></a>Uploaden naar containers met openbare toegang

1. De assembly uploaden naar uw opslagaccount. 
   Kies in het rechter venster **uploaden**.

1. Nadat u klaar bent met het uploaden, selecteert u de geüploade assembly. Kies op de werkbalk **kopie URL** zodat u de URL van de assembly kopiëren.

1. Ga terug naar de Azure-portal waar de **Assembly toevoegen** deelvenster is geopend. 
   Voer een naam voor de assembly. 
   Kies **groot bestand (groter dan 2 MB)**.

   De **inhouds-URI** in het nu wordt weergegeven, in plaats van de **Assembly** vak.

1. In de **inhouds-URI** vak, plak de URL van de assembly. 
   Voltooi uw assembly toe te voegen.

Nadat de assembly geüpload is, het schema wordt weergegeven in de **assembly's** lijst.
Op uw integratie-account **overzicht** pagina onder **onderdelen**, wordt de **assembly's** tegel toont nu het aantal geüploade assembly's.

<a name="no-public-access-assemblies"></a>

#### <a name="upload-to-containers-without-public-access"></a>Uploaden naar containers zonder openbare toegang

1. De assembly uploaden naar uw opslagaccount. 
   Kies in het rechter venster **uploaden**.

1. Nadat u klaar bent met het uploaden, moet u een shared access signature (SAS) genereren voor de assembly. 
   Selecteer in het snelmenu van de assembly, **Shared Access Signature ophalen**.

1. In de **handtekening voor gedeelde toegang** venster **handtekening van de container op serverniveau gedeelde toegang genereren URI** > **maken**. 
   Nadat de SAS-URL wordt gegenereerd, naast de **URL** Kies **kopie**.

1. Ga terug naar de Azure-portal waar de **Assembly toevoegen** deelvenster is geopend. 
   Voer een naam voor de assembly. 
   Kies **groot bestand (groter dan 2 MB)**.

   De **inhouds-URI** in het nu wordt weergegeven, in plaats van de **Assembly** vak.

1. In de **inhouds-URI** vak, plak de SAS-URI u eerder hebt gegenereerd. Voltooi uw assembly toe te voegen.

Nadat de assembly geüpload is, de assembly wordt weergegeven in de **schema's** lijst. Op uw integratie-account **overzicht** pagina onder **onderdelen**, wordt de **assembly's** tegel toont nu het aantal geüploade assembly's.

## <a name="create-maps"></a>Kaarten maken

Als u wilt een XSLT-document dat u als een kaart gebruiken kunt maakt, kunt u Visual Studio 2015 gebruiken voor het maken van een BizTalk-integratie-project met behulp van de [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md). In dit project bouwt u een integratie van toewijzingsbestand, waarmee u visueel items tussen twee bestanden van de XML-schema worden toegewezen. Nadat u dit project bouwt, krijgt u een XSLT-document.
Zie voor limieten op de kaart hoeveelheden in integratieaccounts [limieten en configuratie voor Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits). 

## <a name="add-maps"></a>Toewijzingen toevoegen

Na het uploaden van een assembly's waarnaar wordt verwezen naar de kaart, kunt u nu uw kaart uploaden.

1. Als u al dit nog niet hebt aangemeld, aanmelden bij de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> met de referenties van uw Azure-account. 

1. Als uw integratie-account nog niet geopend, in het hoofdmenu van Azure is, selecteert u **alle services**. 
   Voer in het zoekvak ' integratieaccount'. 
   Selecteer **integratieaccounts**.

   ![Integratie-account zoeken](./media/logic-apps-enterprise-integration-maps/find-integration-account.png)

1. Selecteer het integratieaccount waar u wilt toevoegen van uw map, bijvoorbeeld:

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-maps/select-integration-account.png)

1. Op uw integratie-account **overzicht** pagina onder **onderdelen**, selecteer de **Maps** tegel.

   ![Selecteer "Kaarten"](./media/logic-apps-enterprise-integration-maps/select-maps.png)

1. Na de **Maps** pagina wordt geopend, kiest u **toevoegen**.

   ![Kies 'Toevoegen'](./media/logic-apps-enterprise-integration-maps/add-map.png)  

<a name="smaller-map"></a>

### <a name="add-maps-up-to-2-mb"></a>Toewijzingen toevoegen tot 2 MB

1. Onder **toewijzing toevoegen**, voer een naam in voor uw kaart. 

1. Onder **type toewijzing**, selecteer het type, bijvoorbeeld: **Liquid**, **XSLT**, **XSLT 2.0**, of **XSLT 3.0**.

1. Houd **klein bestand** geselecteerde. Naast de **kaart** Kies het pictogram van de map. Zoek en selecteer de kaart die u, bijvoorbeeld uploadt:

   ![Map uploaden](./media/logic-apps-enterprise-integration-maps/upload-map-file.png)

   Als u de **naam** eigenschap leeg, de bestandsnaam van de kaart automatisch wordt weergegeven in die eigenschap automatisch nadat u de kaart-bestand hebt geselecteerd. 
   U kunt echter een unieke naam gebruiken.

1. Wanneer u klaar bent, kiest u **OK**. 
   Nadat uw kaart-bestand geüpload is, de kaart wordt weergegeven in de **Maps** lijst.

   ![Geüploade maps-lijst](./media/logic-apps-enterprise-integration-maps/uploaded-maps-list.png)

   Op uw integratie-account **overzicht** pagina onder **onderdelen**, wordt de **toegewezen** tegel wordt nu het aantal geüploade maps, bijvoorbeeld:

   ![Geüploade maps](./media/logic-apps-enterprise-integration-maps/uploaded-maps.png)

<a name="larger-map"></a>

### <a name="add-maps-more-than-2-mb"></a>Voeg kaarten meer dan 2 MB

Op dit moment om toe te voegen grotere maps, gebruiken de [REST API van Azure Logic Apps - kaarten](https://docs.microsoft.com/rest/api/logic/maps/createorupdate).

<!--

To add larger maps, you can upload your map to 
an Azure blob container in your Azure storage account. 
Your steps for adding maps differ based whether your 
blob container has public read access. So first, check 
whether or not your blob container has public read 
access by following these steps: 
[Set public access level for blob container](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### Check container access level

1. Open Azure Storage Explorer. In the Explorer window, 
   expand your Azure subscription if not already expanded.

1. Expand **Storage Accounts** > {*your-storage-account*} > 
   **Blob Containers**. Select your blob container.

1. From your blob container's shortcut menu, 
   select **Set Public Access Level**.

   * If your blob container has at least public access, choose **Cancel**, 
   and follow these steps later on this page: 
   [Upload to containers with public access](#public-access)

     ![Public access](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * If your blob container doesn't have public access, choose **Cancel**, 
   and follow these steps later on this page: 
   [Upload to containers without public access](#public-access)

     ![No public access](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access-maps"></a>

### Add maps to containers with public access

1. Upload the map to your storage account. 
   In the right-hand window, choose **Upload**. 

1. After you finish uploading, select your 
   uploaded map. On the toolbar, choose **Copy URL** 
   so that you copy the map's URL.

1. Return to the Azure portal where the 
   **Add Map** pane is open. Choose **Large file**. 

   The **Content URI** box now appears, 
   rather than the **Map** box.

1. In the **Content URI** box, paste your map's URL. 
   Finish adding your map.

After your map finishes uploading, 
the map appears in the **Maps** list.

<a name="no-public-access-maps"></a>

### Add maps to containers with no public access

1. Upload the map to your storage account. 
   In the right-hand window, choose **Upload**.

1. After you finish uploading, generate a 
   shared access signature (SAS) for your schema. 
   From your map's shortcut menu, 
   select **Get Shared Access Signature**.

1. In the **Shared Access Signature** pane, select 
   **Generate container-level shared access signature URI** > **Create**. 
   After the SAS URL gets generated, next to the **URL** box, choose **Copy**.

1. Return to the Azure portal where the 
   **Add Maps** pane is open. Choose **Large file**.

   The **Content URI** box now appears, 
   rather than the **Map** box.

1. In the **Content URI** box, paste the SAS URI 
   you previously generated. Finish adding your map.

After your map finishes uploading, 
the map appears in the **Maps** list.

-->

## <a name="edit-maps"></a>Toewijzingen bewerken

Voor het bijwerken van een bestaande toewijzing, moet u een nieuwe kaart-bestand met de wijzigingen die u wilt uploaden. U kunt echter eerst de bestaande toewijzing voor het bewerken van downloaden.

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, zoeken en openen van uw integratie-account, als dit al geopend.

1. Selecteer in het hoofdmenu van Azure **alle services**. Voer in het zoekvak ' integratieaccount'. Selecteer **integratieaccounts**.

1. Selecteer het integratieaccount waar u wilt bijwerken van de kaart.

1. Op uw integratie-account **overzicht** pagina onder **onderdelen**, selecteer de **Maps** tegel.

1. Na de **Maps** pagina wordt geopend, selecteert u de kaart. 
   Om te downloaden en de kaart eerst bewerken, kiest u **downloaden**, en sla de kaart.

1. Wanneer u bent klaar om te uploaden van de bijgewerkte kaart op de **Maps** pagina, selecteert u de kaart die u wilt bijwerken, en kies **bijwerken**.

1. Zoek en selecteer de bijgewerkte toewijzing die u wilt uploaden. 
   Nadat uw kaart-bestand geüpload is, wordt de bijgewerkte kaart weergegeven in de **Maps** lijst.

## <a name="delete-maps"></a>Maps verwijderen

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, zoeken en openen van uw integratie-account, als dit al geopend.

1. Selecteer in het hoofdmenu van Azure **alle services**. 
   Voer in het zoekvak ' integratieaccount'. 
   Selecteer **integratieaccounts**.

1. Selecteer het integratieaccount waar u wilt verwijderen van de kaart.

1. Op uw integratie-account **overzicht** pagina onder **onderdelen**, selecteer de **Maps** tegel.

1. Na de **Maps** pagina wordt geopend, selecteert u de kaart en kies **verwijderen**.

1. Als u wilt bevestigen dat u wilt verwijderen van de kaart, kies **Ja**.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)  
* [Meer informatie over schema 's](../logic-apps/logic-apps-enterprise-integration-schemas.md)
* [Meer informatie over transformaties](../logic-apps/logic-apps-enterprise-integration-transform.md)