---
title: X12 berichten voor B2B-bedrijfsintegratie - Azure Logic Apps | Microsoft Docs
description: Exchange X12 berichten in de EDI-indeling voor B2B-bedrijfsintegratie in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 7422d2d5-b1c7-4a11-8c9b-0d8cfa463164
ms.date: 01/31/2017
ms.openlocfilehash: c4ee56f4ddcccb1fc4ddd84aa1c1b16dea9754d9
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123954"
---
# <a name="exchange-x12-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Exchange X12 berichten voor B2B-bedrijfsintegratie in Azure Logic Apps met Enterprise Integration Pack

Voordat u kunt exchange-X12 berichten voor Azure Logic Apps, moet u een X12 overeenkomst en op te slaan die overeenkomst in het integratieaccount. Hier volgen de stappen voor het maken van een X12 overeenkomst.

> [!NOTE]
> Deze pagina behandelt de X12-functies voor Azure Logic Apps. Zie voor meer informatie, [EDIFACT](logic-apps-enterprise-integration-edifact.md).

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement
* Ten minste twee [partners](../logic-apps/logic-apps-enterprise-integration-partners.md) die zijn gedefinieerd in uw integratie-account en geconfigureerd met de X12 id onder **Bedrijfsidentiteiten**    
* Een vereiste [schema](../logic-apps/logic-apps-enterprise-integration-schemas.md) die u kunt uploaden naar uw integratie-account

Nadat u [maken van een integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), [partners toevoegen](logic-apps-enterprise-integration-partners.md), en hebben een [schema](../logic-apps/logic-apps-enterprise-integration-schemas.md) dat u wilt gebruiken, kunt u een X12 overeenkomst door deze stappen te volgen.

## <a name="create-an-x12-agreement"></a>Maken van een X12 overeenkomst

1. Meld u aan bij [Azure Portal](http://portal.azure.com "Azure Portal"). 

2. Selecteer in het hoofdmenu van Azure **alle services**. Voer 'integration' in het zoekvak en selecteer vervolgens **integratieaccounts**.  

   ![Uw integratie-account zoeken](./media/logic-apps-enterprise-integration-x12/account-1.png)

   > [!TIP]
   > Als **alle services** niet wordt weergegeven, wordt u mogelijk eerst het menu uitvouwen. Selecteer aan de bovenkant van het menu samengevouwen **weergeven in het menu**.

3. Onder **Integratieaccounts**, selecteert u de integratieaccount waar u de overeenkomst toevoegen.

   ![Integratie-account selecteren waar moet worden gemaakt van de overeenkomst](./media/logic-apps-enterprise-integration-x12/account-3.png)

4. Selecteer **overzicht**en selecteer vervolgens de **overeenkomsten** tegel. Als u geen een tegel overeenkomsten, eerst de tegel toevoegen. 

   ![Kies 'Overeenkomsten' tegel](./media/logic-apps-enterprise-integration-as2/agreement-1.png)

5. Onder **overeenkomsten**, kiest u **toevoegen**.

   ![Kies 'Toevoegen'](./media/logic-apps-enterprise-integration-as2/agreement-2.png)     

6. Onder **toevoegen**, voer een **naam** voor uw overeenkomst. Selecteer voor het overeenkomsttype **X12**. Selecteer de **Hostpartner**, **Hostidentiteit**, **Gastpartner**, en **Gastidentiteit** voor uw overeenkomst. Zie voor meer informatie van de eigenschap, de tabel in deze stap.

    ![Geef overeenkomstdetails](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

    | Eigenschap | Beschrijving |
    | --- | --- |
    | Naam |Naam van de overeenkomst |
    | Overeenkomsttype | Moet X12 |
    | Hostpartner |Een overeenkomst moet een host en de Gast-partner. De hostpartner vertegenwoordigt de organisatie die Hiermee configureert u de overeenkomst. |
    | Hostidentiteit |Een id voor de hostpartner |
    | Gastpartner |Een overeenkomst moet een host en de Gast-partner. De gastpartner vertegenwoordigt de organisatie die wordt zakendoen met de hostpartner. |
    | Gastidentiteit |Een id voor de gastpartner |
    | Ontvangstinstellingen |Deze eigenschappen zijn van toepassing op alle berichten ontvangen door een overeenkomst. |
    | Verzendinstellingen |Deze eigenschappen zijn van toepassing op alle berichten die door een overeenkomst worden verzonden. |  

  > [!NOTE]
  > Resolutie van X12 overeenkomst is afhankelijk van die overeenkomt met de afzenderkwalificatie van de en -id en de ontvangerskwalificatie van de en gedefinieerd in de partner en het binnenkomende bericht-id. Als deze waarden voor uw partner wijzigen, ook de overeenkomst bijwerken.

## <a name="configure-how-your-agreement-handles-received-messages"></a>Configureren hoe uw overeenkomst verwerkt berichten ontvangen

Nu u de eigenschappen van de overeenkomst hebt ingesteld, kunt u configureren hoe deze overeenkomst identificeert en binnenkomende berichten ontvangen van uw partner via deze overeenkomst worden verwerkt.

1.  Onder **toevoegen**, selecteer **instellingen ontvangen**.
Configureer deze eigenschappen op basis van uw overeenkomst met de partner die berichten met u worden uitgewisseld. Zie voor eigenschapbeschrijvingen, de tabellen in deze sectie.

    **Ontvangstinstellingen** is onderverdeeld in deze secties: id's, bevestiging, schema's, enveloppen, controlenummers, validaties en interne instellingen.

2. Als u klaar bent, zorg ervoor dat u uw instellingen opslaan door te kiezen **OK**.

De overeenkomst is nu gereed voor het afhandelen van berichten die aan de geselecteerde instellingen voldoen.

### <a name="identifiers"></a>Id's

![Id-eigenschappen instellen](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Eigenschap | Beschrijving |
| --- | --- |
| ISA1 (autorisatiekwalificatie) |Selecteer de waarde van de kwalificatie van de autorisatie uit de vervolgkeuzelijst. |
| ISA2 |Optioneel. Autorisatie-informatie waarde invoeren. Als de waarde die u hebt ingevoerd voor ISA1 dan 00 is, voert u minimaal één alfanumeriek teken en maximaal 10. |
| ISA3 (beveiligingskwalificatie) |Selecteer de waarde van de kwalificatie van de beveiliging van de vervolgkeuzelijst. |
| ISA4 |Optioneel. Voer de waarde voor informatie. Als de waarde die u hebt ingevoerd voor ISA3 dan 00 is, voert u minimaal één alfanumeriek teken en maximaal 10. |

### <a name="acknowledgment"></a>Bevestiging

![Bevestiging eigenschappen instellen](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| TA1 verwacht |Retourneert een technische bevestiging naar de afzender uitwisseling |
| FA verwacht |Retourneert een functionele bevestiging naar de afzender uitwisseling. Selecteer vervolgens of u wilt dat de bevestigingen 997 of 999, op basis van de schemaversie |
| AK2-/ IK2-lus opnemen |Het genereren van AK2 lussen inschakelen in functionele bevestigingen voor geaccepteerde transactiesets |

### <a name="schemas"></a>Schema 's

Selecteer een schema voor elke transactietype (ST1) en Zendtoepassing (GS2). De pijplijn ontvangen ook het binnenkomende bericht worden ontleed door die overeenkomt met de waarden voor ST1 en GS2 in het binnenkomende bericht op met de waarden die u hier instelt en het schema van het binnenkomende bericht met het schema dat u hier instelt.

![Selecteer schema](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Versie |Selecteer de X12 versie |
| Transactietype (ST01) |Selecteer het transactietype |
| Zendtoepassing (GS02) |Selecteer de afzender-toepassing |
| Schema |Selecteer het schema dat u wilt gebruiken. Schema's worden toegevoegd aan uw integratie-account. |

> [!NOTE]
> Configureer de vereiste [Schema](../logic-apps/logic-apps-enterprise-integration-schemas.md) die wordt geüpload naar uw [integratieaccount](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Enveloppen

![Geef het scheidingsteken in een transactieset: standaard-id of het scheidingsteken voor herhalingen kiezen](./media/logic-apps-enterprise-integration-x12/x12-34.png)

| Eigenschap | Beschrijving |
| --- | --- |
| ISA11-gebruik |Hiermee geeft u het scheidingsteken gebruiken in een transactieset: <p>Selecteer **standaard id** ontvangen voor het gebruik van een punt (.) voor de decimale notatie, in plaats van de decimale notatie van het binnenkomende document in de EDI pijplijn. <p>Selecteer **scheidingsteken voor herhalingen** om op te geven van het scheidingsteken voor herhaalde exemplaren van een eenvoudige gegevenselement of een herhaalde gegevensstructuur. Bijvoorbeeld, wordt meestal de karaat (^) gebruikt als het scheidingsteken voor herhalingen. Voor HIPAA schema's, kunt u alleen de karaat gebruiken. |

### <a name="control-numbers"></a>Controlenummers

![Selecteer hoe u voor het afhandelen van dubbele](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Uitwisselingscontrolenummer duplicaten weigeren |Dubbele uitwisselingen blokkeren. Controleert of het controlenummer uitwisseling (ISA13) voor het controlenummer ontvangen uitwisseling. Als een overeenkomst wordt gedetecteerd, kan de pijplijn ontvangen de uitwisseling niet verwerken. U kunt opgeven dat het aantal dagen voor het uitvoeren van de controle door middel van een waarde voor *controleren op dubbele ISA13 elke (dagen)*. |
| Dubbele groepscontrolenummers niet toestaan |Blok uitwisselingen met dubbele groep controlenummers. |
| Dubbele transactiereekscontrolenummers niet toestaan |Blok uitwisselingen met dubbele transactie set controlenummers. |

### <a name="validations"></a>Validaties

![Validatie-eigenschappen instellen voor de ontvangen berichten](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

Wanneer elke rij van de validatie is voltooid, wordt een andere automatisch toegevoegd. Als u een van de regels niet opgeeft, wordt validatie van de rij 'Standaard'.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichttype |Selecteer het type EDI-bericht. |
| EDI-validatie |EDI-validatie op gegevenstypen zoals gedefinieerd door de schema's EDI eigenschappen, beperkingen voor de lengte, lege gegevenselementen en volgscheidingstekens uitvoeren. |
| Uitgebreide validatie |Als het gegevenstype niet EDI, valideren is op de gegevensvereiste-element en herhaling, opsommingen en element lengte gegevensvalidatie (min./Max.) toegestaan. |
| Voorloop-/volgnullen toestaan |Alle aanvullende voorloop- of afsluitende nul behouden en ruimte tekens. Niet deze tekens te verwijderen. |
| Voorloop-/volgnullen knippen |Voorloop- of volgspaties nul en spaties verwijderen. |
| Beleid voor volgscheidingstekens |Navolgende scheidingstekens genereren. <p>Selecteer **niet toegestaan** verbiedt navolgende scheidingstekens en scheidingstekens in de uitwisseling ontvangen. Als de uitwisseling navolgende scheidingstekens en scheidingstekens heeft, is de uitwisseling gedeclareerd niet geldig. <p>Selecteer **optioneel** uitwisselingen met of zonder afsluitende scheidingstekens en scheidingstekens accepteren. <p>Selecteer **verplichte** wanneer de uitwisseling moet hebben navolgende scheidingstekens en scheidingstekens. |

### <a name="internal-settings"></a>Interne instellingen

![Interne instellingen selecteren](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Geïmpliceerde decimale notatie 'Nn' converteren naar een base 10 numerieke waarde |Converteert een getal EDI die is opgegeven met de indeling 'Nn' in een numerieke waarde grondtal 10 |
| Lege XML-tags maken als volgscheidingstekens zijn toegestaan |Schakel dit selectievakje in om de uitwisseling afzender lege XML-tags voor navolgende scheidingstekens bevatten. |
| Uitwisseling splitsen in transactiereeksen - transactiereeksen onderbreken bij fout|Elke transactie instellen in een knooppunt in een afzonderlijke XML-document met de juiste envelop toe te passen op de transactieset worden geparseerd. Wordt alleen de transacties onderbroken wanneer de validatie is mislukt. |
| Uitwisseling splitsen in transactiereeksen - uitwisseling onderbreken bij fout|Elke transactie instellen in een knooppunt in een afzonderlijke XML-document door toe te passen van de juiste envelop worden geparseerd. Onderbreekt de gehele uitwisseling wanneer een of meer transactiesets in de uitwisseling validatie is mislukt. | 
| Uitwisseling bewaren-transactiereeksen onderbreken bij fout |De uitwisseling intact blijven, maakt u een XML-document voor de gehele uitwisseling van de batch. Wordt alleen de transactiesets die de validatie, terwijl u verdergaat met het verwerken van alle andere transactiesets niet onderbroken. |
| Uitwisseling bewaren - uitwisseling onderbreken bij fout |De uitwisseling intact blijven, maakt u een XML-document voor de gehele uitwisseling van de batch. Onderbreekt de gehele uitwisseling wanneer een of meer transactiesets in de uitwisseling validatie is mislukt. |

## <a name="configure-how-your-agreement-sends-messages"></a>Configureren hoe berichten worden verzonden door uw overeenkomst

U kunt configureren hoe deze overeenkomst identificeert en uitgaande berichten die u naar uw partner via deze overeenkomst verzendt worden verwerkt.

1.  Onder **toevoegen**, selecteer **instellingen voor verzenden**.
Configureer deze eigenschappen op basis van uw overeenkomst met uw partner die berichten worden uitgewisseld met u. Zie voor eigenschapbeschrijvingen, de tabellen in deze sectie.

    **Verzendinstellingen** is onderverdeeld in deze secties: id's, bevestiging, schema's, enveloppen, tekensets en scheidingstekens, controlenummers en validatie.

2. Als u klaar bent, zorg ervoor dat u uw instellingen opslaan door te kiezen **OK**.

De overeenkomst is nu gereed om af te handelen uitgaande berichten die aan de geselecteerde instellingen voldoen.

### <a name="identifiers"></a>Id's

![Id-eigenschappen instellen](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Eigenschap | Beschrijving |
| --- | --- |
| Autorisatiekwalificatie (ISA1) |Selecteer de waarde van de kwalificatie van de autorisatie uit de vervolgkeuzelijst. |
| ISA2 |Autorisatie-informatie waarde invoeren. Als deze waarde dan 00 is, voert u minimaal één alfanumeriek teken en maximaal 10. |
| Security-kwalificatie (ISA3) |Selecteer de waarde van de kwalificatie van de beveiliging van de vervolgkeuzelijst. |
| ISA4 |Voer de waarde voor informatie. Als deze waarde dan 00, voor het tekstvak waarde (ISA4 is) en voer vervolgens een minimum van een alfanumerieke waarde en maximaal 10. |

### <a name="acknowledgment"></a>Bevestiging

![Bevestiging eigenschappen instellen](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Eigenschap | Beschrijving |
| --- | --- |
| TA1 verwacht |Een technische bevestiging (TA1) terug naar de afzender uitwisseling. Deze instelling geeft aan dat de hostpartner die het bericht verzendt aanvragen van een bevestiging van de gastpartner in de overeenkomst. Deze ontvangstbevestigingen worden verwacht door de hostpartner op basis van de instellingen voor het ontvangen van de overeenkomst. |
| FA verwacht |Een functionele bevestigingen (VA) terug naar de afzender uitwisseling. Selecteer of u de bevestigingen 997 of 999 wilt, op basis van de schema-versies die u met werkt. Deze ontvangstbevestigingen worden verwacht door de hostpartner op basis van de instellingen voor het ontvangen van de overeenkomst. |
| FA-versie |Selecteer de FA-versie |

### <a name="schemas"></a>Schema 's

![Selecteer schema moet worden gebruikt](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Eigenschap | Beschrijving |
| --- | --- |
| Versie |Selecteer de X12 versie |
| Transactietype (ST01) |Selecteer het transactietype |
| SCHEMA |Selecteer het schema moet worden gebruikt. Schema's vindt u in uw integratie-account. Als u eerst schema selecteert, wordt automatisch geconfigureerd versie en transactie type  |

> [!NOTE]
> Configureer de vereiste [Schema](../logic-apps/logic-apps-enterprise-integration-schemas.md) die wordt geüpload naar uw [integratieaccount](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Enveloppen

![Geef het scheidingsteken in een transactieset: standaard-id of het scheidingsteken voor herhalingen kiezen](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| ISA11-gebruik |Hiermee geeft u het scheidingsteken gebruiken in een transactieset: <p>Selecteer **standaard id** ontvangen voor het gebruik van een punt (.) voor de decimale notatie, in plaats van de decimale notatie van het binnenkomende document in de EDI pijplijn. <p>Selecteer **scheidingsteken voor herhalingen** om op te geven van het scheidingsteken voor herhaalde exemplaren van een eenvoudige gegevenselement of een herhaalde gegevensstructuur. Bijvoorbeeld, wordt meestal de karaat (^) gebruikt als het scheidingsteken voor herhalingen. Voor HIPAA schema's, kunt u alleen de karaat gebruiken. |

### <a name="control-numbers"></a>Controlenummers

![Geef de eigenschappen van het controlenummer](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Controleversienummer (ISA12) |Selecteer de versie van de standaard X12 |
| Gebruiksindicator (ISA15) |Selecteer de context van een knooppunt.  De waarden zijn gegevens, productiegegevens, of gegevens te testen |
| Schema |Genereert de GS en ST segmenten voor een X12 gecodeerde-uitwisseling die worden verzonden aan de pijplijn verzenden |
| GS1 |Optioneel, selecteert u een waarde voor de functionele code uit de vervolgkeuzelijst |
| GS2 |Optioneel, toepassing-afzender |
| GS3 |Optionele toepassing ontvanger |
| GS4 |Optioneel, selecteer CCYYMMDD of JJMMDD |
| GS5 |Optioneel, selecteer UUMM, HHMMSS of HHMMSSdd |
| GS7 |Optioneel, selecteert u een waarde voor de verantwoordelijk instantie uit de vervolgkeuzelijst |
| GS8 |Optioneel, versie van het document |
| Uitwisselingscontrolenummer (ISA13) |Vereist, voer een bereik van waarden in voor het controlenummer voor de uitwisseling. Voer een numerieke waarde met een minimaal 1 en 999999999 maximaal |
| Groepscontrolenummer (GS06) |Vereist, voer een bereik van de getallen voor het groepscontrolenummer. Voer een numerieke waarde met een minimaal 1 en 999999999 maximaal |
| Transactiereekscontrolenummer (ST02) |Vereist, voer een bereik van de getallen voor het controlenummer transactie instellen. Voer een bereik met numerieke waarden met een minimaal 1 en 999999999 maximaal |
| Voorvoegsel |Optioneel; de aangewezen voor het bereik van de transactie set-controlenummers in bevestiging gebruikt. Voer een numerieke waarde voor de middelste twee velden, en een alfanumerieke waarde (indien gewenst) voor de voor- en achtervoegsel velden. De middelste velden zijn vereist en bevatten de minimale en maximale waarden voor het controlenummer |
| Achtervoegsel |Optioneel; de aangewezen voor het bereik van de controlenummers die transactie set die wordt gebruikt in een bevestiging. Voer een numerieke waarde voor de middelste twee velden en een alfanumerieke waarde (indien gewenst) voor de voor- en achtervoegsel velden. De middelste velden zijn vereist en bevatten de minimale en maximale waarden voor het controlenummer |

### <a name="character-sets-and-separators"></a>Tekensets en scheidingstekens

Anders dan het teken is ingesteld, kunt u een andere set scheidingstekens voor elk berichttype. Als een tekenset voor een bepaald bericht-schema is niet opgegeven, wordt de standaardtekenset gebruikt.

![Scheidingstekens voor berichttypen opgeven](./media/logic-apps-enterprise-integration-x12/x12-9.png) 

| Eigenschap | Beschrijving |
| --- | --- |
| Tekenset die moet worden gebruikt |De eigenschappen, selecteer de X12 tekenset valideren. De opties zijn Basic, uitgebreid en UTF8. |
| Schema |Selecteer een schema in de vervolgkeuzelijst. Nadat u elke rij hebt voltooid, wordt er automatisch een nieuwe rij toegevoegd. Selecteer voor het geselecteerde schema, de set met scheidingstekens die u gebruiken wilt, op basis van de onderstaande beschrijvingen van scheidingsteken voor duizendtallen. |
| Invoertype |Selecteer een type gebruikersinvoer in de vervolgkeuzelijst. |
| Scheidingsteken voor onderdelen |Afzonderlijke elementen samengestelde gegevens, voert u één teken. |
| Scheidingsteken voor gegevenselementen |Afzonderlijke eenvoudige gegevenselementen in samengestelde elementen, voer één teken. |
| Vervangingsteken |Voer een vervangende teken dat wordt gebruikt voor het vervangen van alle scheidingstekens in de nettolading van gegevens bij het genereren van de uitgaande X12 bericht. |
| Segmenteindteken |Geeft een enkel teken aan het einde van een EDI-segment. |
| Achtervoegsel |Selecteer het teken dat wordt gebruikt met de segment-id. Als u een achtervoegsel opgeeft, kan klikt u vervolgens het segmenteindteken element niet leeg zijn. Als de segmenteindteken leeg wordt gelaten, moet u een achtervoegsel opgeven. |

> [!TIP]
> Voor speciaal tekenwaarden, de overeenkomst als JSON bewerken en geef de ASCII-waarde voor de speciale tekens.

### <a name="validation"></a>Validatie

![Validatie-eigenschappen instellen voor het verzenden van berichten](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

Wanneer elke rij van de validatie is voltooid, wordt een andere automatisch toegevoegd. Als u een van de regels niet opgeeft, wordt validatie van de rij 'Standaard'.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichttype |Selecteer het type EDI-bericht. |
| EDI-validatie |EDI-validatie op gegevenstypen zoals gedefinieerd door de schema's EDI eigenschappen, beperkingen voor de lengte, lege gegevenselementen en volgscheidingstekens uitvoeren. |
| Uitgebreide validatie |Als het gegevenstype niet EDI, valideren is op de gegevensvereiste-element en herhaling, opsommingen en element lengte gegevensvalidatie (min./Max.) toegestaan. |
| Voorloop-/volgnullen toestaan |Alle aanvullende voorloop- of afsluitende nul behouden en ruimte tekens. Niet deze tekens te verwijderen. |
| Voorloop-/volgnullen knippen |Voorloop- of afsluitende nul tekens verwijderen. |
| Beleid voor volgscheidingstekens |Navolgende scheidingstekens genereren. <p>Selecteer **niet toegestaan** verbiedt navolgende scheidingstekens en scheidingstekens in de uitwisseling verzonden. Als de uitwisseling navolgende scheidingstekens en scheidingstekens heeft, is de uitwisseling gedeclareerd niet geldig. <p>Selecteer **optioneel** uitwisselingen met of zonder afsluitende scheidingstekens en scheidingstekens verzenden. <p>Selecteer **verplichte** als de verzonden uitwisseling moet navolgende scheidingstekens en scheidingstekens bevatten. |

## <a name="find-your-created-agreement"></a>Uw overeenkomst gevonden

1.  Nadat u klaar bent met het instellen van alle uw overeenkomst-eigenschappen op de **toevoegen** pagina, kies **OK** te maken van uw overeenkomst voltooien en keer terug naar uw integratie-account.

    Nu uw zojuist toegevoegde overeenkomst wordt weergegeven in uw **overeenkomsten** lijst.

2.  U kunt ook uw overeenkomsten weergeven in een overzicht van uw integratie-account. Kies uw integratie-account in het menu van **overzicht**en selecteer vervolgens de **overeenkomsten** tegel.

    ![Kies 'Overeenkomsten' tegel](./media/logic-apps-enterprise-integration-x12/x12-1-5.png)   

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [details swagger](/connectors/x12/). 

## <a name="learn-more"></a>Meer informatie
* [Meer informatie over het Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  

