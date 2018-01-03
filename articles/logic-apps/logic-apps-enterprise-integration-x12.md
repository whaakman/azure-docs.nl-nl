---
title: X12 berichten voor B2B enterprise integration - Azure Logic Apps | Microsoft Docs
description: Exchange X12 berichten in EDI-indeling voor B2B enterprise integratie met Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 7422d2d5-b1c7-4a11-8c9b-0d8cfa463164
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: bfad01d8c14cdd972ebe8e4038f226ffe0da93b1
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="exchange-x12-messages-for-enterprise-integration-with-logic-apps"></a>Exchange X12 berichten voor enterprise-integratie met logic apps

Voordat u kunt X12 uitwisselen berichten voor Azure Logic Apps, moet u een X12 overeenkomst en op te slaan die overeenkomst in uw account integratie. Hier volgen de stappen voor het maken van een X12 overeenkomst.

> [!NOTE]
> Deze pagina de X12 dekt functies voor Azure Logic Apps. Zie voor meer informatie [EDIFACT](logic-apps-enterprise-integration-edifact.md).

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een [integratie account](../logic-apps/logic-apps-enterprise-integration-accounts.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement
* Ten minste twee [partners](../logic-apps/logic-apps-enterprise-integration-partners.md) die zijn gedefinieerd in uw account integratie en geconfigureerd met de X12 id onder **Business identiteiten**    
* Een vereiste [schema](../logic-apps/logic-apps-enterprise-integration-schemas.md) voor het uploaden naar uw [integratie-account](../logic-apps/logic-apps-enterprise-integration-accounts.md)

Nadat u [maken van een account integratie](../logic-apps/logic-apps-enterprise-integration-accounts.md), [partners toevoegen](logic-apps-enterprise-integration-partners.md), en hebben een [schema](../logic-apps/logic-apps-enterprise-integration-schemas.md) dat u wilt gebruiken, kunt u een X12 overeenkomst met de volgende stappen.

## <a name="create-an-x12-agreement"></a>Maken van een X12 overeenkomst

1.  Meld u aan bij [Azure Portal](http://portal.azure.com "Azure Portal"). Selecteer in het menu links **meer services**. 

    > [!TIP]
    > Als er geen **meer services**, mogelijk moet u eerst het menu uitvouwen. Selecteer aan de bovenkant van het menu samengevouwen **weergeven in het menu**.

    ![Selecteer op de linkermenu 'Meer services'](./media/logic-apps-enterprise-integration-x12/account-1.png)

2.  Typ in het zoekvak "-integratie" als filter. Selecteer in de lijst met resultaten **Integratieaccounts**.  

    ![Filteren op "-integratie", selecteert u 'Integratieaccounts'](./media/logic-apps-enterprise-integration-x12/account-2.png)

3. In de **Integratieaccounts** blade die wordt geopend, selecteert u de integratie-account waar u de overeenkomst toevoegen.
Als er geen eventuele integratieaccounts [maken van een eerste](../logic-apps/logic-apps-enterprise-integration-accounts.md "alles over integratieaccounts").

    ![Integratie-account selecteren wanneer u de overeenkomst](./media/logic-apps-enterprise-integration-x12/account-3.png)

4. Selecteer **overzicht**, selecteer vervolgens de **overeenkomsten** tegel. Als u een tegel overeenkomsten geen hebt, eerst de tegel toevoegen. 

    ![Kies 'Overeenkomsten' tegel](./media/logic-apps-enterprise-integration-as2/agreement-1.png)

5. Kies in de blade van de overeenkomsten die wordt geopend, **toevoegen**.

    ![Kies 'Add'](./media/logic-apps-enterprise-integration-as2/agreement-2.png)     

6. Onder **toevoegen**, voer een **naam** voor de overeenkomst. Selecteer voor het type licentieovereenkomst **X12**. Selecteer de **Host Partner**, **Hostidentiteit**, **Gast Partner**, en **Gast identiteit** voor de overeenkomst. Zie voor meer eigenschapdetails, de tabel in deze stap.

    ![Geef overeenkomstdetails](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

    | Eigenschap | Beschrijving |
    | --- | --- |
    | Naam |Naam van de overeenkomst |
    | Type licentieovereenkomst | Moet X12 |
    | Host-Partner |Een overeenkomst moet een host en de Gast-partner. De host-partner vertegenwoordigt de organisatie die de overeenkomst configureert. |
    | Identiteit van de host |Een id voor de host-partner |
    | Gast Partner |Een overeenkomst moet een host en de Gast-partner. De Gast-partner vertegenwoordigt de organisatie die bedrijven gebruiken met de partner host doet. |
    | Identiteit van de Gast |Een id voor de Gast-partner |
    | Instellingen te ontvangen |Deze eigenschappen zijn van toepassing op alle berichten ontvangen door een overeenkomst. |
    | Instellingen verzenden |Deze eigenschappen zijn van toepassing op alle berichten is verzonden door een overeenkomst. |  

  > [!NOTE]
  > De resolutie van X12 overeenkomst is afhankelijk van die overeenkomt met de kwalificatie afzender en -id en de ontvanger kwalificatie en gedefinieerd in de partner en het binnenkomende bericht-id. Als deze waarden voor uw partner wijzigen, kunt u de overeenkomst bijwerken.

## <a name="configure-how-your-agreement-handles-received-messages"></a>Configureren hoe uw ingangen overeenkomst berichten ontvangen

Nu dat u de eigenschappen van de overeenkomst hebt ingesteld, kunt u configureren hoe deze overeenkomst identificeert en binnenkomende berichten ontvangen van uw partner via deze overeenkomst worden verwerkt.

1.  Onder **toevoegen**, selecteer **instellingen ontvangen**.
Configureer deze eigenschappen op basis van uw overeenkomst met de partner die u berichten worden uitgewisseld. Zie voor eigenschapbeschrijvingen, de tabellen in deze sectie.

    **Ontvangen instellingen** is onderverdeeld in deze secties: id's, bevestiging, schema's, enveloppen, besturingselement getallen, validaties en interne instellingen.

2. Nadat u bent klaar, controleert u of uw instellingen opslaan door te kiezen **OK**.

Uw overeenkomst is nu gereed voor het verwerken van inkomende berichten die aan de geselecteerde instellingen voldoen.

### <a name="identifiers"></a>Id 's

![Id-eigenschappen instellen](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Eigenschap | Beschrijving |
| --- | --- |
| ISA1 (autorisatie kwalificatie) |Selecteer de waarde van de kwalificatie autorisatie in de vervolgkeuzelijst. |
| ISA2 |Optioneel. Geef autorisatie-informatie-waarde. Als de waarde die u hebt opgegeven voor ISA1 dan 00 is, voert u een minimum van een alfanumeriek teken en maximaal 10. |
| ISA3 (beveiliging kwalificatie) |Selecteer de waarde voor de kwalificatie in de vervolgkeuzelijst. |
| ISA4 |Optioneel. Voer de waarde voor informatie. Als de waarde die u hebt opgegeven voor ISA3 dan 00 is, voert u een minimum van een alfanumeriek teken en maximaal 10. |

### <a name="acknowledgment"></a>Bevestiging

![Bevestiging eigenschappen instellen](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| TA1 verwacht |Retourneert een technische bevestiging naar de DIF-afzender |
| VA verwacht |Retourneert een functionele bevestiging naar de DIF-afzender. Selecteer vervolgens of u de 997 of 999 bevestigingen wilt, op basis van de versie van het schema |
| AK2/IK2 lus opnemen |Het genereren van AK2 lussen kunnen in functionele bevestigingen van geaccepteerde transactie instellen |

### <a name="schemas"></a>Schema 's

Selecteer een schema voor elke transactietype (ST1) en de afzender-toepassing (GS2). De pijplijn receive worden het binnenkomende bericht ontleed door de waarden voor ST1 en GS2 in het binnenkomende bericht op met de waarden die u hier instelt en het schema van het binnenkomende bericht met het schema dat u hier instelt.

![Selecteer schema](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Versie |Selecteer de X12 versie |
| Transactietype (ST01) |Selecteer het transactietype |
| Afzender-toepassing (GS02) |De afzender-toepassing selecteren |
| Schema |Selecteer het schema dat u wilt gebruiken. Schema's worden toegevoegd aan uw account integratie. |

> [!NOTE]
> Configureer de vereiste [Schema](../logic-apps/logic-apps-enterprise-integration-schemas.md) die is geüpload naar uw [integratie account](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Enveloppen

![Geef het scheidingsteken in een set transactie: standaard-id of herhaling scheidingsteken kiezen](./media/logic-apps-enterprise-integration-x12/x12-34.png)

| Eigenschap | Beschrijving |
| --- | --- |
| Gebruik ISA11 |Hiermee geeft u het scheidingsteken moet worden gebruikt in een set met transactie: <p>Selecteer **standaard identifier** ontvangen voor het gebruik van een punt (.) voor de decimale notatie, in plaats van de notatie van het binnenkomende document in de EDI pijplijn. <p>Selecteer **herhaling scheidingsteken** om op te geven van het scheidingsteken voor herhaalde exemplaren van een eenvoudige gegevenselement of een herhaalde gegevensstructuur. Bijvoorbeeld, wordt meestal de karaat (^) als scheidingsteken gebruikt de herhaling. U kunt alleen de karaat HIPAA schema's gebruiken. |

### <a name="control-numbers"></a>Besturingselement cijfers

![Selecteer hoe nummer duplicaten besturingselement verwerkt](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Controle-aantal Interchange duplicaten weigeren |Dubbele uitgewisseld blokkeren. Controleert de DIF-controle-aantal (ISA13) voor het aantal ontvangen DIF-besturingselement. Als een overeenkomst wordt gedetecteerd, verwerken niet de pijplijn ontvangen de DIF. U kunt opgeven dat het aantal dagen voor het uitvoeren van de controle door middel van een waarde voor *controleren op dubbele ISA13 elke (dagen)*. |
| Groep besturingselement nummer duplicaten weigeren |Blok uitwisselingen met dubbele groep besturingselement nummers. |
| Transactie set besturingselement nummer duplicaten weigeren |Blok uitwisselingen met dubbele transactie set besturingselement nummers. |

### <a name="validations"></a>Validaties

![Validatie-eigenschappen instellen voor ontvangen berichten](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

Wanneer elke rij van de validatie is voltooid, wordt automatisch een andere toegevoegd. Als u geen regels opgeeft, gebruikt validatie van de rij 'Standaard'.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichttype |Selecteer het type EDI-bericht. |
| EDI-validatie |EDI-validatie wordt uitgevoerd op gegevenstypen zoals gedefinieerd door het schema EDI-eigenschappen, beperkingen voor de lengte, leeg gegevenselementen en afsluitende scheidingstekens. |
| Uitgebreide validatie |Als het gegevenstype niet EDI, validatie is op de gegevensvereiste-element en herhaling, opsommingen en element lengte gegevensvalidatie (min/max) toegestaan. |
| Voorloop-/ Volg nullen toestaan |Alle aanvullende voorloop- of afsluitende nul behouden en ruimte tekens. Verwijder niet deze tekens bevatten. |
| Trim voorloop-/ Volg nullen |Voorloop- of volgspaties nul en spaties verwijderen. |
| Afsluitende scheidingsteken beleid |Afsluitende scheidingstekens genereren. <p>Selecteer **niet toegestaan** verbiedt afsluitende scheidingstekens en scheidingstekens in het knooppunt ontvangen. Als de uitwisseling afsluitende scheidingstekens en scheidingstekens heeft, is het knooppunt gedeclareerd niet geldig. <p>Selecteer **optioneel** uitgewisseld met of zonder een afsluitende scheidingstekens en scheidingstekens accepteren. <p>Selecteer **verplichte** wanneer het knooppunt moet hebben afsluitende scheidingstekens en scheidingstekens. |

### <a name="internal-settings"></a>Interne instellingen

![Instellingen voor interne selecteren](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Impliciete decimale notatie 'Mm' converteren naar een numerieke waarde met grondtal 10 |Converteert een getal EDI die is opgegeven met de notatie 'Mm' in een numerieke waarde grondtal 10 |
| Lege XML-labels maken als afsluitende scheidingstekens zijn toegestaan. |Schakel dit selectievakje in om de DIF-afzender lege XML-codes voor afsluitende scheidingstekens bevatten. |
| Gesplitste Interchange als transaction sets - onderbreken transactie sets bij fout|Analyseert elke transactie instellen in een knooppunt in een afzonderlijke XML-document met de juiste envelop toepassen op de set transactie. Alleen de transacties onderbreekt waarbij de validatie is mislukt. |
| Gesplitste Interchange als transaction sets - onderbreken uitwisseling bij fout|Elke transactie instellen in een knooppunt in een afzonderlijke XML-document met het toepassen van de juiste envelop parseert. Onderbreekt de gehele uitwisseling wanneer een of meer sets transactie in het knooppunt validatie is mislukt. | 
| Behouden DIF - transactie sets onderbreken bij fout |Laat de DIF intact, maakt een XML-document voor de hele batch uitwisseling. Onderbreekt de transactie-sets die niet voldoen aan validatie, terwijl alle andere sets transactie verwerken. |
| Behouden DIF - uitwisseling onderbreken bij fout |Laat de DIF intact, maakt een XML-document voor de hele batch uitwisseling. Onderbreekt de gehele uitwisseling wanneer een of meer sets transactie in het knooppunt validatie is mislukt. |

## <a name="configure-how-your-agreement-sends-messages"></a>Configureren hoe uw overeenkomst verzendt berichten

U kunt configureren hoe deze overeenkomst identificeert en uitgaande berichten verwerkt die u naar uw partner via deze overeenkomst verzendt.

1.  Onder **toevoegen**, selecteer **instellingen voor verzenden**.
Deze eigenschappen op basis van uw overeenkomst met uw partner die berichten worden uitgewisseld met u configureren. Zie voor eigenschapbeschrijvingen, de tabellen in deze sectie.

    **Instellingen verzenden** is onderverdeeld in deze secties: id's, bevestiging, schema's, enveloppen, tekensets en scheidingstekens, Control Numbers en validatie.

2. Nadat u bent klaar, controleert u of uw instellingen opslaan door te kiezen **OK**.

Uw overeenkomst is nu gereed voor het verwerken van uitgaande berichten die aan de geselecteerde instellingen voldoen.

### <a name="identifiers"></a>Id 's

![Id-eigenschappen instellen](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Eigenschap | Beschrijving |
| --- | --- |
| Autorisatie kwalificatie (ISA1) |Selecteer de waarde van de kwalificatie autorisatie in de vervolgkeuzelijst. |
| ISA2 |Geef autorisatie-informatie-waarde. Als deze waarde dan 00 is, en voer vervolgens een minimum van een alfanumeriek teken en maximaal 10. |
| Beveiliging-kwalificatie (ISA3) |Selecteer de waarde voor de kwalificatie in de vervolgkeuzelijst. |
| ISA4 |Voer de waarde voor informatie. Als deze waarde dan 00, voor het tekstvak waarde (ISA4 is) en voer vervolgens een alfanumerieke waarde minimaal en maximaal 10. |

### <a name="acknowledgment"></a>Bevestiging

![Bevestiging eigenschappen instellen](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Eigenschap | Beschrijving |
| --- | --- |
| TA1 verwacht |Een technische bevestiging (TA1) terug naar de DIF-afzender. Deze instelling geeft aan dat de host-partner die het bericht verzendt aanvragen van een bevestiging van de Gast partner in de overeenkomst. Deze bevestigingen worden verwacht door de partner-host op basis van de instellingen voor het ontvangen van de overeenkomst. |
| VA verwacht |Een functionele bevestiging (VA) terug naar de DIF-afzender. Kies of u de 997 of 999 bevestigingen wilt, op basis van de schema-versies die u met werkt. Deze bevestigingen worden verwacht door de partner-host op basis van de instellingen voor het ontvangen van de overeenkomst. |
| VA-versie |Selecteer de VA-versie |

### <a name="schemas"></a>Schema 's

![Selecteer schema te gebruiken](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Eigenschap | Beschrijving |
| --- | --- |
| Versie |Selecteer de X12 versie |
| Transactietype (ST01) |Selecteer het transactietype |
| SCHEMA |Selecteer het schema te gebruiken. Schema's bevinden zich in uw account integratie. Als u eerst schema selecteert, configureert het automatisch versie en transactie type  |

> [!NOTE]
> Configureer de vereiste [Schema](../logic-apps/logic-apps-enterprise-integration-schemas.md) die is geüpload naar uw [integratie account](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Enveloppen

![Geef het scheidingsteken in een set transactie: standaard-id of herhaling scheidingsteken kiezen](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Gebruik ISA11 |Hiermee geeft u het scheidingsteken moet worden gebruikt in een set met transactie: <p>Selecteer **standaard identifier** ontvangen voor het gebruik van een punt (.) voor de decimale notatie, in plaats van de notatie van het binnenkomende document in de EDI pijplijn. <p>Selecteer **herhaling scheidingsteken** om op te geven van het scheidingsteken voor herhaalde exemplaren van een eenvoudige gegevenselement of een herhaalde gegevensstructuur. Bijvoorbeeld, wordt meestal de karaat (^) als scheidingsteken gebruikt de herhaling. U kunt alleen de karaat HIPAA schema's gebruiken. |

### <a name="control-numbers"></a>Besturingselement cijfers

![Controle-aantal eigenschappen opgeven](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Het versienummer besturingselement (ISA12) |Selecteer de versie van de standaard X12 |
| Gebruiksindicator (ISA15) |Selecteer in de context van een knooppunt.  De waarden zijn informatie productiegegevens, of gegevens te testen |
| Schema |Genereert de GS en ST segmenten voor een X12 gecodeerd uitwisselen die worden verzonden aan de pijplijn verzenden |
| GS1 |Optioneel, selecteert u een waarde voor de functionele code uit de vervolgkeuzelijst |
| GS2 |Optioneel, toepassing afzender |
| GS3 |Optioneel, toepassing ontvanger |
| GS4 |Optioneel, selecteer CCYYMMDD of JJMMDD |
| GS5 |Optioneel, selecteer UUMM, UUMMSS of HHMMSSdd |
| GS7 |Optioneel, selecteert u een waarde voor het verantwoordelijk agency uit de vervolgkeuzelijst |
| GS8 |Optioneel, versie van het document |
| Interchange besturingselementnummer (ISA13) |Vereist, voer een bereik van waarden voor het besturingselement interchange nummer. Geef een numerieke waarde met een minimum van 1 en 999999999 maximaal |
| Besturingselement groepsnummer (GS06) |Vereist, voer een bereik van getallen in voor het nummer van het besturingselement. Geef een numerieke waarde met een minimum van 1 en 999999999 maximaal |
| Transactie besturingselementnummer (ST02) |Vereist is, voer een bereik van de getallen voor het besturingselement voor transactie ingesteld. Geef een bereik van de numerieke waarden met een minimum van 1 en 999999999 maximaal |
| Voorvoegsel |Optionele, toegewezen voor het bereik van transactie set besturingselement getallen in de bevestiging gebruikt. Voer een numerieke waarde voor de middelste twee velden en een alfanumerieke waarde (indien gewenst) voor de voor- en achtervoegsel velden. De middelste velden zijn vereist en de minimale en maximale waarden voor het besturingselement bevatten |
| Achtervoegsel |Optioneel, voor het bereik van de transactie set besturingselement getallen gebruikt in een bevestiging is aangewezen. Voer een numerieke waarde voor de middelste twee velden en een alfanumerieke waarde (indien gewenst) voor de voor- en achtervoegsel velden. De middelste velden zijn vereist en de minimale en maximale waarden voor het besturingselement bevatten |

### <a name="character-sets-and-separators"></a>Teken Sets en scheidingstekens

Andere dan de tekenset, kunt u een andere set scheidingstekens voor elk berichttype. Als een tekenset voor een opgegeven bericht-schema niet is opgegeven, wordt de standaardtekenset gebruikt.

![Geef de scheidingstekens voor berichttypen](./media/logic-apps-enterprise-integration-x12/x12-9.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Tekenset moet worden gebruikt |Valideren van de eigenschappen, selecteer de X12 tekenset. De opties zijn Basic, uitgebreid en UTF8. |
| Schema |Selecteer een schema in de vervolgkeuzelijst. Nadat u elke rij hebt voltooid, wordt er automatisch een nieuwe rij toegevoegd. Voor het geselecteerde schema, de scheidingstekens set selecteren die u gebruiken wilt, op basis van de volgende beschrijvingen van het scheidingsteken. |
| Invoertype |Selecteer een type invoer in de vervolgkeuzelijst. |
| Onderdeel scheidingselement |Afzonderlijke elementen van de samengestelde gegevensbron, voert u één teken. |
| Gegevens Element scheidingselement |Afzonderlijke eenvoudige gegevenselementen in de elementen van de samengestelde gegevensbron, voert u één teken. |
| Vervangend teken |Voer een vervangend teken dat wordt gebruikt voor het vervangen van alle scheidingstekens in de nettolading van gegevens bij het genereren van de uitgaande X12 bericht. |
| Segment eindteken |Geeft een enkel teken aan het einde van een EDI-segment. |
| Achtervoegsel |Selecteer het teken dat wordt gebruikt met de segment-id. Als u een achtervoegsel opgeeft, kan het gegevenselement voor segment eindteken leeg zijn. Als de segment-terminator leeg wordt gelaten, moet u een achtervoegsel toewijzen. |

> [!TIP]
> Als u speciale tekenwaarden, de overeenkomst bewerken als JSON en geef de ASCII-waarde voor speciaal teken.

### <a name="validation"></a>Validatie

![Validatie-eigenschappen instellen voor het verzenden van berichten](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

Wanneer elke rij van de validatie is voltooid, wordt automatisch een andere toegevoegd. Als u geen regels opgeeft, gebruikt validatie van de rij 'Standaard'.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichttype |Selecteer het type EDI-bericht. |
| EDI-validatie |EDI-validatie wordt uitgevoerd op gegevenstypen zoals gedefinieerd door het schema EDI-eigenschappen, beperkingen voor de lengte, leeg gegevenselementen en afsluitende scheidingstekens. |
| Uitgebreide validatie |Als het gegevenstype niet EDI, validatie is op de gegevensvereiste-element en herhaling, opsommingen en element lengte gegevensvalidatie (min/max) toegestaan. |
| Voorloop-/ Volg nullen toestaan |Alle aanvullende voorloop- of afsluitende nul behouden en ruimte tekens. Verwijder niet deze tekens bevatten. |
| Trim voorloop-/ Volg nullen |Voorloop- of afsluitende nul tekens verwijderen. |
| Afsluitende scheidingsteken beleid |Afsluitende scheidingstekens genereren. <p>Selecteer **niet toegestaan** verbiedt afsluitende scheidingstekens en scheidingstekens in het knooppunt verzonden. Als de uitwisseling afsluitende scheidingstekens en scheidingstekens heeft, is het knooppunt gedeclareerd niet geldig. <p>Selecteer **optioneel** uitgewisseld met of zonder een afsluitende scheidingstekens en scheidingstekens verzenden. <p>Selecteer **verplichte** als de verzonden uitwisseling afsluitende scheidingstekens en scheidingstekens nodig hebt. |

## <a name="find-your-created-agreement"></a>De overeenkomst zoeken

1.  Als u klaar bent met het instellen van alle uw overeenkomst-eigenschappen op de **toevoegen** blade kiezen **OK** maken van uw overeenkomst is voltooid en Ga terug naar de blade van het integratie-account.

    Nu uw toegevoegde overeenkomst wordt weergegeven in uw **overeenkomsten** lijst.

2.  U kunt ook uw overeenkomsten weergeven in uw Accountoverzicht integratie. Kies op de blade van het integratie-account **overzicht**, selecteer vervolgens de **overeenkomsten** tegel.

    ![Kies 'Overeenkomsten' tegel om weer te geven van alle overeenkomsten](./media/logic-apps-enterprise-integration-x12/x12-1-5.png)   

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [swagger details](/connectors/x12/). 

## <a name="learn-more"></a>Meer informatie
* [Meer informatie over het Enterprise-integratiepakket](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  

