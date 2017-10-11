---
title: Schema's voor XML-validatie - Azure Logic Apps | Microsoft Docs
description: XML-documenten met schema's voor Azure Logic Apps en Enterprise Integration Pack valideren
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 4f58a587c1f10aea1cee89e46fa9ec340e0d21c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-the-enterprise-integration-pack"></a>XML-validatie met schema's voor Azure Logic Apps en de Enterprise-integratiepakket

Schema's Bevestig dat de XML-documenten die u ontvangt geldig zijn en de verwachte gegevens in een vooraf gedefinieerde indeling hebben. Schema's kunnen ook berichten die worden uitgewisseld in een B2B-scenario te valideren.

## <a name="add-a-schema"></a>Een schema toevoegen

1. Selecteer in de Azure-portal **meer services**.

    ![Azure-portal 'Meer services'](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Voer in het zoekvak filter **integratie**, en selecteer **Integratieaccounts** uit de lijst met resultaten.

    ![Zoekvak filteren](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Selecteer de **integratie account** waar u het schema worden toegevoegd.

    ![Lijst met integratieaccounts](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Kies de **schema's** tegel.

    ![Voorbeeld van de integratie-account, 'Schema'](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>Toevoegen van een schemabestand dat kleiner is dan 2 MB

1. In de **schema's** blade die wordt geopend (van de voorgaande stappen), kiest **toevoegen**.

    ![Schema's blade "Toevoegen"](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Voer een naam voor uw schema. Uploaden van het schemabestand door het selecteren van het pictogram naast de **Schema** vak. Nadat het uploadproces is voltooid, selecteert u **OK**.

    ![Schermopname van 'Toevoegen Schema', met 'Kleine file' gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-to-8-mb-maximum"></a>Toevoegen van een schemabestand groter is dan 2 MB (maximaal 8 MB)

Deze stappen verschillen op basis van het toegangsniveau van de blob-container: **openbare** of **geen anonieme toegang**.

**Dit toegangsniveau bepalen**

1.  Open **Azure Opslagverkenner**. 

2.  Onder **Blob-Containers**, de blob-container die u wilt selecteren. 

3.  Selecteer **beveiliging**, **toegangsniveau**.

Als het toegangsniveau voor beveiliging van blob is **openbare**, als volgt te werk.

![Azure Storage Explorer met de 'Blob-Containers', 'Beveiliging' en 'Openbare' gemarkeerd](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Het schema uploaden naar uw opslagaccount en kopieer de URI.

    ![Storage-account met URI gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. In **Schema toevoegen**, selecteer **groot bestand**, en geeft u de URI in de **inhoud URI** in het tekstvak.

    ![Schema's met de knop 'Toevoegen' en 'Grote file' gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Als het toegangsniveau voor beveiliging van blob is **geen anonieme toegang**, als volgt te werk.

![Azure Storage Explorer met de 'Blob-Containers', 'Beveiliging' en 'Geen anonieme toegang' gemarkeerd](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Het schema uploaden naar uw opslagaccount.

    ![Storage-account](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. Genereer een shared access signature voor het schema.

    ![Storage-account met gedeelde toegang handtekeningen tabblad gemarkeerd](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. In **Schema toevoegen**, selecteer **groot bestand**, en geef de shared access signature URI in de **inhoud URI** in het tekstvak.

    ![Schema's met de knop 'Toevoegen' en 'Grote file' gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. In de **schema's** blade van uw account voor de integratie van uw zojuist toegevoegde schema moet worden weergegeven.

    ![Uw account integratie met 'Schema' en het nieuwe schema gemarkeerd](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Schema's bewerken

1. Kies de **schema's** tegel.

2. Na de **schema's** blade wordt geopend, selecteert u het schema dat u wilt bewerken.

3. Op de **schema's** blade kiezen **bewerken**.

    ![Schema's-blade](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Selecteer het schemabestand dat u wilt bewerken, en selecteer vervolgens **Open**.

    ![Open het schemabestand te bewerken](media/logic-apps-enterprise-integration-schemas/edit-31.png)

Azure ziet u een bericht of het schema is geüpload.

## <a name="delete-schemas"></a>Schema's verwijderen

1. Kies de **schema's** tegel.

2. Na de **schema's** blade wordt geopend, selecteer het schema dat u wilt verwijderen.

3. Op de **schema's** blade kiezen **verwijderen**.

    ![Schema's-blade](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. Om te bevestigen dat u wilt verwijderen van het geselecteerde schema, kies **Ja**.

    ![Bevestigingsbericht 'Schema verwijderen'](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    In de **schema's** blade de schema-lijst wordt vernieuwd en bevat niet langer het schema dat u hebt verwijderd.

    ![Uw integratie met 'Schema' gemarkeerd-Account](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over het enterprise-integratiepakket").  

