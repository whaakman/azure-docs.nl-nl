---
title: XML valideren met schema's - Azure Logic Apps | Microsoft Docs
description: Schema's voor het valideren van XML-documenten in Azure Logic Apps met Enterprise Integration Pack toevoegen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.date: 02/06/2019
ms.openlocfilehash: 3cca995b353b88cc481cbda68df4211a724f7f09
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57727311"
---
# <a name="validate-xml-with-schemas-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML valideren met schema's in Azure Logic Apps met Enterprise Integration Pack

Als u wilt controleren die documenten geldig XML-bestand gebruiken en de verwachte gegevens in de vooraf gedefinieerde indeling voor enterprise integration-scenario's in Azure Logic Apps hebt, kunt uw logische app schema's gebruiken. Een schema kan ook berichten die exchange logic-apps in scenario's voor business-to-business (B2B) valideren.

Zie voor beperkingen met betrekking tot integratieaccounts en artefacten, zoals schema's, [limieten en configuratie-informatie voor Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, <a href="https://azure.microsoft.com/free/" target="_blank">meld u dan aan voor een gratis Azure-account</a>.

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) waar u uw schema's en andere artefacten voor bedrijfsintegratie en oplossingen, business-to-business (B2B) opslaan. 

  Als uw schema [2 MB of kleiner](#smaller-schema), kunt u uw schema toevoegen aan uw integratie-account rechtstreeks vanuit de Azure portal. Echter, als het schema groter dan 2 MB, maar niet groter zijn dan is de [schema groottelimiet](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits), kunt u uw schema uploaden naar Azure storage-account. 
  Als u wilt dat schema toevoegen aan uw integratie-account, kunt u vervolgens koppelen aan uw storage-account van uw integratie-account. 
  Dit zijn de items die u nodig hebt voor deze taak: 

  * [Azure storage-account](../storage/common/storage-account-overview.md) waarin u een blob-container maken voor uw schema. Meer informatie over het [een opslagaccount maken](../storage/common/storage-quickstart-create-account.md). 

  * BLOB-container voor het opslaan van uw schema. Meer informatie over het [maken van een blob-container](../storage/blobs/storage-quickstart-blobs-portal.md). 
  U nodig inhouds-URI van uw container later wanneer u het schema aan uw integratie-account toevoegen is bereikt.

  * [Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md), die u kunt gebruiken voor het beheren van opslagaccounts en blob-containers. 
  Kies een van beide opties hier voor het gebruik van Storage Explorer:
  
    * In de Azure-portal, zoek en selecteer uw storage-account. 
    Selecteer in het menu van uw opslagaccount, **Opslagverkenner**.

    * Voor de desktopversie [downloaden en installeren van Azure Storage Explorer](https://www.storageexplorer.com/). 
    Koppel vervolgens Storage Explorer aan uw storage-account met de volgende stappen in [aan de slag met Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md). 
    Raadpleeg voor meer informatie [Snelstart: Een blob maken in objectopslag met Azure Storage Explorer](../storage/blobs/storage-quickstart-blobs-storage-explorer.md).

U hoeft geen een logische app bij het maken en schema's toe te voegen. Voor het gebruik van een schema, uw logische app moet echter koppelen aan een integratieaccount waar u dit schema wilt opslaan. Informatie over [logische apps koppelen aan integratieaccounts](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account). Als u een logische app nog niet hebt, krijgt u informatie [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-schemas"></a>Schema's toevoegen

1. Gebruik de referenties van uw Azure-account om u aan melden bij het <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

1. Als u wilt zoeken en openen van uw integratie-account, op de Azure-hoofdmenu in, selecteer **alle services**. Voer in het zoekvak ' integratieaccount'. Selecteer **integratieaccounts**.

   ![Integratie-account zoeken](./media/logic-apps-enterprise-integration-schemas/find-integration-account.png)

1. Selecteer het integratieaccount waar u uw schema, bijvoorbeeld toevoegen:

   ![Integratie-account selecteren](./media/logic-apps-enterprise-integration-schemas/select-integration-account.png)

1. Op uw integratie-account **overzicht** pagina onder **onderdelen**, selecteer de **schema's** tegel.

   ![Selecteer 'Schema'](./media/logic-apps-enterprise-integration-schemas/select-schemas.png)

1. Na de **schema's** pagina wordt geopend, kiest u **toevoegen**.

   ![Kies 'Toevoegen'](./media/logic-apps-enterprise-integration-schemas/add-schema.png)

Op basis van de grootte van uw (.xsd)-schemabestand, volgt u de stappen voor het uploaden van een schema dat is [maximaal 2 MB](#smaller-schema) of [meer dan 2 MB, 8 MB](#larger-schema).

<a name="smaller-schema"></a>

### <a name="add-schemas-up-to-2-mb"></a>Schema's toevoegen tot 2 MB

1. Onder **Schema toevoegen**, voer een naam in voor uw schema. 
   Houd **klein bestand** geselecteerde. Naast de **Schema** Kies het pictogram van de map. Zoek en selecteer het schema dat u, bijvoorbeeld uploadt:

   ![Kleinere schema uploaden](./media/logic-apps-enterprise-integration-schemas/upload-smaller-schema-file.png)

1. Wanneer u klaar bent, kiest u **OK**.

   Nadat het schema geüpload is, het schema wordt weergegeven in de **schema's** lijst.

<a name="larger-schema"></a>

### <a name="add-schemas-more-than-2-mb"></a>Toevoegen van schema's van meer dan 2 MB

Om toe te voegen grotere schema's, kunt u uw schema uploaden naar een Azure blob-container in uw Azure storage-account. De stappen voor het toevoegen van schema's verschillen op basis van of uw blob-container, openbare leestoegang heeft. Dus eerst controleren of uw blob-container openbare leestoegang heeft door de volgende stappen: [Openbaar toegangsniveau voor blob-container instellen](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### <a name="check-container-access-level"></a>Niveau van de container toegang controleren

1. Open Azure Storage Explorer. Vouw in het Explorer-venster uit uw Azure-abonnement als nog niet is uitgevouwen.

1. Vouw **Opslagaccounts** > {*uw storage-account*} > **Blob-Containers**. Selecteer uw blob-container.

1. Selecteer in het snelmenu van uw blobcontainer, **niveau openbare toegang instelt**.

   * Als uw blob-container ten minste openbare toegang heeft, kiest u **annuleren**, en volg deze stappen verderop op deze pagina: [Uploaden naar containers met openbare toegang](#public-access)

     ![Openbare toegang](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * Als uw blob-container geen openbare toegang heeft, kiest u **annuleren**, en volg deze stappen verderop op deze pagina: [Uploaden naar containers zonder openbare toegang](#public-access)

     ![Er zijn geen openbare toegang](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access"></a>

#### <a name="upload-to-containers-with-public-access"></a>Uploaden naar containers met openbare toegang

1. Upload het schema naar uw storage-account. 
   Kies in het rechter venster **uploaden**.

1. Nadat u klaar bent met het uploaden, selecteert u uw geüploade schema. Kies op de werkbalk **kopie URL** zodat u het schema van de URL kopiëren.

1. Ga terug naar de Azure-portal waar de **Schema toevoegen** deelvenster is geopend. 
   Voer een naam voor de assembly. 
   Kies **groot bestand (groter dan 2 MB)**. 

   De **inhouds-URI** in het nu wordt weergegeven, in plaats van de **Schema** vak.

1. In de **inhouds-URI** vak, plak de URL van uw schema's. 
   Voltooi uw schema toe te voegen.

Nadat het schema geüpload is, het schema wordt weergegeven in de **schema's** lijst. Op uw integratie-account **overzicht** pagina onder **onderdelen**, wordt de **schema's** tegel toont nu het aantal geüploade schema's.

<a name="no-public-access"></a>

#### <a name="upload-to-containers-without-public-access"></a>Uploaden naar containers zonder openbare toegang

1. Upload het schema naar uw storage-account. 
   Kies in het rechter venster **uploaden**.

1. Nadat u klaar bent met het uploaden, genereert u een shared access signature (SAS) voor uw schema. 
   Selecteer in het snelmenu van het schema, **Shared Access Signature ophalen**.

1. In de **handtekening voor gedeelde toegang** venster **handtekening van de container op serverniveau gedeelde toegang genereren URI** > **maken**. 
   Nadat de SAS-URL wordt gegenereerd, naast de **URL** Kies **kopie**.

1. Ga terug naar de Azure-portal waar de **Schema toevoegen** deelvenster is geopend. Kies **grote bestanden**.

   De **inhouds-URI** in het nu wordt weergegeven, in plaats van de **Schema** vak.

1. In de **inhouds-URI** vak, plak de SAS-URI u eerder hebt gegenereerd. Voltooi uw schema toe te voegen.

Nadat het schema geüpload is, het schema wordt weergegeven in de **schema's** lijst. Op uw integratie-account **overzicht** pagina onder **onderdelen**, wordt de **schema's** tegel toont nu het aantal geüploade schema's.

## <a name="edit-schemas"></a>Schema bewerken

Voor het bijwerken van een bestaand schema, moet u een nieuw schemabestand met de wijzigingen die u wilt uploaden. U kunt echter eerst de bestaande schema's voor het bewerken van downloaden.

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, zoeken en openen van uw integratie-account, als dit al geopend.

1. Selecteer in het hoofdmenu van Azure **alle services**. 
   Voer in het zoekvak ' integratieaccount'. 
   Selecteer **integratieaccounts**.

1. Selecteer het integratieaccount waar u uw schema bijwerken.

1. Op uw integratie-account **overzicht** pagina onder **onderdelen**, selecteer de **schema's** tegel.

1. Na de **schema's** pagina wordt geopend, selecteert u uw schema. 
   Om te downloaden en het schema eerst bewerken, kiest u **downloaden**, en sla het schema.

1. Wanneer u bent klaar om te uploaden van het schema bijgewerkt op de **schema's** pagina, selecteert u het schema dat u wilt bijwerken, en kies **bijwerken**.

1. Zoek en selecteer het bijgewerkte schema dat u wilt uploaden. 
   Nadat uw schemabestand geüpload is, wordt het bijgewerkte schema weergegeven in de **schema's** lijst.

## <a name="delete-schemas"></a>Schema's verwijderen

1. In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, zoeken en openen van uw integratie-account, als dit al geopend.

1. Selecteer in het hoofdmenu van Azure **alle services**. 
   Voer in het zoekvak ' integratieaccount'. 
   Selecteer **integratieaccounts**.

1. Selecteer het integratieaccount waar u uw schema verwijderen.

1. Op uw integratie-account **overzicht** pagina onder **onderdelen**, selecteer de **schema's** tegel.

1. Na de **schema's** pagina wordt geopend, selecteert u uw schema en kies **verwijderen**.

1. Om te bevestigen dat u wilt verwijderen van het schema, kies **Ja**.

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)
* [Meer informatie over maps](../logic-apps/logic-apps-enterprise-integration-maps.md)
* [Meer informatie over transformaties](../logic-apps/logic-apps-enterprise-integration-transform.md)
