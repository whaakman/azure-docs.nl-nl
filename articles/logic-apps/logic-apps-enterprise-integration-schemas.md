---
title: Schema's voor XML-validatie - Azure Logic Apps toevoegen | Microsoft Docs
description: Schema's die worden gevalideerd, XML-documenten in Azure Logic Apps met Enterprise Integration Pack maken
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.date: 07/29/2016
ms.openlocfilehash: e03346da1c2b77f885c39d5329f990684979c56e
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123070"
---
# <a name="validate-xml-with-schemas-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML valideren met schema's in Azure Logic Apps met Enterprise Integration Pack

Schema's bevestigen dat de XML-documenten die u ontvangt geldig zijn en de verwachte gegevens in een vooraf gedefinieerde indeling hebben. Schema's kunnen ook berichten die worden uitgewisseld in een B2B-scenario te valideren.

## <a name="add-a-schema"></a>Een schema toevoegen

1. Selecteer in de Azure portal, **alle services**.

    ![Azure-portal, 'Alle services'](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Voer in het zoekvak filter **integratie**, en selecteer **Integratieaccounts** in de lijst met resultaten.

    ![Zoekvak filteren](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Selecteer de **integratieaccount** waaraan u wilt toevoegen van het schema.

    ![Overzicht van integratieaccounts](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Kies de **schema's** tegel.

    ![Voorbeeld van de integratie-account, 'Schema'](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>Toevoegen van een schemabestand dat kleiner is dan 2 MB

1. In de **schema's** blade die wordt geopend (uit de bovenstaande stappen), kies **toevoegen**.

    ![Schema's blade 'Toevoegen'](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Voer een naam voor uw schema. Uploaden van het schemabestand hiervoor het pictogram van de map naast de **Schema** vak. Nadat de upload is voltooid, selecteert u **OK**.

    ![Schermafbeelding van 'Schema toevoegen', met 'Kleine file' is gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-to-8-mb-maximum"></a>Toevoegen van een schemabestand groter is dan 2 MB (maximaal 8 MB)

Deze stappen zijn afhankelijk van het toegangsniveau van de blob-container: **openbare** of **geen anonieme toegang**.

**Dit toegangsniveau bepalen**

1.  Open **Azure Storage Explorer**. 

2.  Onder **Blobcontainers**, selecteert u de blobcontainer die u wilt. 

3.  Selecteer **Security**, **toegangsniveau**.

Als de blob-niveau voor beveiliging toegang **openbare**, als volgt te werk.

![Azure Storage Explorer, met "Blob-Containers", "Beveiliging" en 'Openbare' is gemarkeerd](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Het schema uploaden naar uw opslagaccount en kopieer de URI.

    ![Storage-account met de URI is gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. In **Schema toevoegen**, selecteer **grote bestanden**, en geeft u de URI in de **inhouds-URI** in het tekstvak.

    ![Schema's, met de knop 'Toevoegen' en 'Grote file' is gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Als de blob-niveau voor beveiliging toegang **geen anonieme toegang**, als volgt te werk.

![Azure Storage Explorer met de "Blob-Containers", "Beveiliging" en "geen anonieme toegang' is gemarkeerd](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Upload het schema naar uw storage-account.

    ![Storage-account](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. Een shared access signature voor het schema genereren.

    ![Storage-account, met tabblad van handtekeningen voor gedeelde toegang is gemarkeerd](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. In **Schema toevoegen**, selecteer **grote bestanden**, en geeft u de shared access signature URI in de **inhouds-URI** in het tekstvak.

    ![Schema's, met de knop 'Toevoegen' en 'Grote file' is gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. In de **schema's** blade van uw integratie-account en het zojuist toegevoegde schema moet worden weergegeven.

    ![Uw integratie-account, met 'Schema' en het nieuwe schema gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Schema bewerken

1. Kies de **schema's** tegel.

2. Na de **schema's** blade wordt geopend, selecteert u het schema dat u wilt bewerken.

3. Op de **schema's** blade, kiest u **bewerken**.

    ![Schema's-blade](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Selecteer het schemabestand dat u wilt bewerken, en selecteer vervolgens **Open**.

    ![Open schemabestand te bewerken](media/logic-apps-enterprise-integration-schemas/edit-31.png)

Azure bevat een bericht dat het schema is geüpload.

## <a name="delete-schemas"></a>Schema's verwijderen

1. Kies de **schema's** tegel.

2. Na de **schema's** blade wordt geopend, selecteert u het schema dat u wilt verwijderen.

3. Op de **schema's** blade, kiest u **verwijderen**.

    ![Schema's-blade](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. Om te bevestigen dat u wilt verwijderen van het geselecteerde schema, kies **Ja**.

    ![Bevestigingsbericht "Schema verwijderen"](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    In de **schema's** blade de schema-lijst wordt vernieuwd en bevat niet langer het schema dat u hebt verwijderd.

    ![Uw integratie-Account, met 'Schema' is gemarkeerd](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over het enterprise integration pack").  

