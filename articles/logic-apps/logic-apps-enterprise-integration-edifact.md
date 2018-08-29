---
title: EDIFACT-berichten voor B2B-bedrijfsintegratie - Azure Logic Apps | Microsoft Docs
description: EDIFACT-berichten uitwisselen EDI-indeling voor B2B-bedrijfsintegratie in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 2257d2c8-1929-4390-b22c-f96ca8b291bc
ms.date: 07/26/2016
ms.openlocfilehash: 2da672e1f55af1e38ae0a3fa90b7ecb10d2f17c7
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128374"
---
# <a name="exchange-edifact-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>EDIFACT-berichten uitwisselen voor B2B-bedrijfsintegratie in Azure Logic Apps met Enterprise Integration Pack

Voordat u EDIFACT-berichten voor Azure Logic Apps uitwisselen kan, moet u een EDIFACT-overeenkomst maken en opslaan van deze overeenkomst in uw integratie-account. Hier volgen de stappen voor het maken van een EDIFACT-overeenkomst.

> [!NOTE]
> Deze pagina bevat informatie over de EDIFACT-functies voor Azure Logic Apps. Zie voor meer informatie, [X12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement  
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw integratieaccount

> [!NOTE]
> Wanneer u een overeenkomst maakt, moet het overeenkomsttype overeenkomen met de inhoud van de berichten die u ontvangt of verzendt naar en van de partner.

Nadat u [maken van een integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) en [partners toevoegen](logic-apps-enterprise-integration-partners.md), kunt u een EDIFACT-overeenkomst maken door deze stappen te volgen.

## <a name="create-an-edifact-agreement"></a>Maken van een EDIFACT-overeenkomst 

1. Meld u aan bij [Azure Portal](http://portal.azure.com "Azure Portal"). 

2. Selecteer in het hoofdmenu van Azure **alle services**. Voer 'integration' in het zoekvak en selecteer vervolgens **integratieaccounts**.

   ![Uw integratie-account zoeken](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

   > [!TIP]
   > Als **alle services** niet wordt weergegeven, wordt u mogelijk eerst het menu uitvouwen. Selecteer aan de bovenkant van het menu samengevouwen **tekstlabels weergeven**.

3. Onder **Integratieaccounts**, selecteert u de integratieaccount waar u wilt maken van de overeenkomst.

   ![Integratie-account selecteren waar moet worden gemaakt van de overeenkomst](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. Kies **overeenkomsten**. Als u geen een tegel overeenkomsten, eerst de tegel toevoegen.   

   ![Kies 'Overeenkomsten' tegel](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. Kies op de pagina overeenkomsten **toevoegen**.

   ![Kies 'Toevoegen'](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. Onder **toevoegen**, voer een **naam** voor uw overeenkomst. Voor **overeenkomsttype**, selecteer **EDIFACT**. Selecteer de **Hostpartner**, **Hostidentiteit**, **Gastpartner**, en **Gastidentiteit** voor uw overeenkomst.

   ![Geef overeenkomstdetails](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

   | Eigenschap | Beschrijving |
   | --- | --- |
   | Naam |Naam van de overeenkomst |
   | Overeenkomsttype | Moet EDIFACT |
   | Hostpartner |Een overeenkomst moet een host en de Gast-partner. De hostpartner vertegenwoordigt de organisatie die Hiermee configureert u de overeenkomst. |
   | Hostidentiteit |Een id voor de hostpartner |
   | Gastpartner |Een overeenkomst moet een host en de Gast-partner. De gastpartner vertegenwoordigt de organisatie die wordt zakendoen met de hostpartner. |
   | Gastidentiteit |Een id voor de gastpartner |
   | Ontvangstinstellingen |Deze eigenschappen zijn van toepassing op alle berichten ontvangen door een overeenkomst. |
   | Verzendinstellingen |Deze eigenschappen zijn van toepassing op alle berichten die door een overeenkomst worden verzonden. |
   ||| 

## <a name="configure-how-your-agreement-handles-received-messages"></a>Configureren hoe uw overeenkomst verwerkt berichten ontvangen

Nu u de eigenschappen van de overeenkomst hebt ingesteld, kunt u configureren hoe deze overeenkomst identificeert en binnenkomende berichten ontvangen van uw partner via deze overeenkomst worden verwerkt.

1. Onder **toevoegen**, selecteer **instellingen ontvangen**.
Configureer deze eigenschappen op basis van uw overeenkomst met de partner die berichten met u worden uitgewisseld. Zie voor eigenschapbeschrijvingen, de tabellen in deze sectie.

   **Ontvangstinstellingen** is onderverdeeld in deze secties: id's, bevestiging, schema's, controlenummers, validatie en interne instellingen.

   !['Ontvangen instellingen' configureren](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. Als u klaar bent, zorg ervoor dat u uw instellingen opslaan door te kiezen **OK**.

De overeenkomst is nu gereed voor het afhandelen van berichten die aan de geselecteerde instellingen voldoen.

### <a name="identifiers"></a>Id's

| Eigenschap | Beschrijving |
| --- | --- |
| UNB6.1 (referentiewachtwoord van de ontvanger) |Voer een alfanumerieke waarde met een resultaat tussen 1 en 14 tekens. |
| UNB6.2 (referentiekwalificatie van de ontvanger) |Voer een alfanumerieke waarde met een minimaal één teken en maximaal twee tekens bestaan. |

### <a name="acknowledgments"></a>Bevestigingen

| Eigenschap | Beschrijving |
| --- | --- |
| Berichtontvangst (CONTRL) |Schakel dit selectievakje in om terug te keren een technische (CONTRL)-bevestiging naar de afzender uitwisseling. De bevestiging is verzonden naar de uitwisseling afzender op basis van de instellingen voor verzenden voor de overeenkomst. |
| Bevestiging (CONTRL) |Schakel dit selectievakje in om terug te keren een functionele (CONTRL)-bevestiging naar de afzender uitwisseling de bevestiging is verzonden naar de uitwisseling afzender op basis van de instellingen voor verzenden voor de overeenkomst. |

### <a name="schemas"></a>Schema 's

| Eigenschap | Beschrijving |
| --- | --- |
| UNH2.1 (TYPE) |Selecteer een type van de set transactie. |
| UNH2.2 (VERSIE) |Voer het versienummer van het bericht. (Minimum, één teken, maximum, drie tekens). |
| UNH2.3 (RELEASE) |Voer het versienummer van het bericht. (Minimum, één teken, maximum, drie tekens). |
| UNH2.5 (BIJBEHORENDE TOEGEWEZEN CODE) |Voer de code in toegewezen. (Maximaal zes tekens. Moet bestaan uit alfanumerieke tekens). |
| UNG2.1 (APP-AFZENDER-ID) |Voer een alfanumerieke waarde met minimaal één teken en maximaal 35 tekens in. |
| UNG2.2 (KWALIFICATIE VAN APP-AFZENDER CODE) |Voer een alfanumerieke waarde, met een maximum van vier tekens. |
| SCHEMA |Selecteer het eerder geüploade schema dat u wilt gebruiken uit het gekoppelde integratieaccount. |

### <a name="control-numbers"></a>Controlenummers
| Eigenschap | Beschrijving |
| --- | --- |
| Uitwisselingscontrolenummer duplicaten weigeren |Voor het blokkeren van dubbele uitwisselingen, selecteert u deze eigenschap. Indien geselecteerd, controleert de EDIFACT-decodering actie de uitwisseling controlenummer (UNB5) voor de ontvangen uitwisseling komt niet overeen met een controlenummer eerder verwerkte uitwisseling. Als een overeenkomst wordt gedetecteerd, klikt u vervolgens de uitwisseling niet is verwerkt. |
| Controleren op dubbele UNB5 elke (dagen) |Als u ervoor de controlenummers dubbele uitwisseling weigeren kiest, kunt u het aantal dagen opgeven voor het uitvoeren van de controle door middel van de juiste waarde voor deze instelling. |
| Dubbele groepscontrolenummers niet toestaan |Als u wilt blokkeren uitwisselingen met dubbele groep controlenummers (UNG5), selecteert u deze eigenschap. |
| Dubbele transactiereekscontrolenummers niet toestaan |Als u wilt blokkeren uitwisselingen met dubbele transactie set controlenummers (UNH1), selecteert u deze eigenschap. |
| EDIFACT-bevestigingscontrolenummer |Voer een waarde voor het voorvoegsel, een bereik van verwijzing getallen en een achtervoegsel om de nummers set transactie voor gebruik in een bevestiging. |

### <a name="validations"></a>Validaties

Wanneer elke rij van de validatie is voltooid, wordt een andere automatisch toegevoegd. Als u een van de regels niet opgeeft, wordt validatie van de rij 'Standaard'.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichttype |Selecteer het type EDI-bericht. |
| EDI-validatie |EDI-validatie op gegevenstypen zoals gedefinieerd door de schema's EDI eigenschappen, beperkingen voor de lengte, lege gegevenselementen en volgscheidingstekens uitvoeren. |
| Uitgebreide validatie |Als het gegevenstype niet EDI, valideren is op de gegevensvereiste-element en herhaling, opsommingen en element lengte gegevensvalidatie (min./Max.) toegestaan. |
| Voorloop-/volgnullen toestaan |Alle aanvullende voorloop- of afsluitende nul behouden en ruimte tekens. Niet deze tekens te verwijderen. |
| Voorloop-/volgnullen knippen |Voorloop- of volgspaties nul en spaties verwijderen. |
| Beleid voor volgscheidingstekens |Navolgende scheidingstekens genereren. <p>Selecteer **niet toegestaan** verbiedt navolgende scheidingstekens en scheidingstekens in de uitwisseling ontvangen. Als de uitwisseling navolgende scheidingstekens en scheidingstekens heeft, is de uitwisseling gedeclareerd niet geldig. <p>Selecteer **optioneel** uitwisselingen met of zonder afsluitende scheidingstekens en scheidingstekens accepteren. <p>Selecteer **verplichte** wanneer de ontvangen uitwisseling moet hebben navolgende scheidingstekens en scheidingstekens. |

### <a name="internal-settings"></a>Interne instellingen

| Eigenschap | Beschrijving |
| --- | --- |
| Lege XML-tags maken als volgscheidingstekens zijn toegestaan |Schakel dit selectievakje in om de uitwisseling afzender lege XML-tags voor navolgende scheidingstekens bevatten. |
| Uitwisseling splitsen in transactiereeksen - transactiereeksen onderbreken bij fout|Elke transactie instellen in een knooppunt in een afzonderlijke XML-document met de juiste envelop toe te passen op de transactieset worden geparseerd. Alleen de transactiereeksen die de validatie niet onderbreken. |
| Uitwisseling splitsen in transactiereeksen - uitwisseling onderbreken bij fout|Elke transactie instellen in een knooppunt in een afzonderlijke XML-document door toe te passen van de juiste envelop worden geparseerd. De gehele uitwisseling onderbreken bij een of meer transactiesets in de uitwisseling validatie is mislukt. | 
| Uitwisseling bewaren-transactiereeksen onderbreken bij fout |De uitwisseling intact blijven, maakt u een XML-document voor de gehele uitwisseling van de batch. Alleen de transactiereeksen die de validatie, terwijl u verdergaat met het verwerken van alle andere transactiesets niet onderbreken. |
| Uitwisseling bewaren - uitwisseling onderbreken bij fout |De uitwisseling intact blijven, maakt u een XML-document voor de gehele uitwisseling van de batch. De gehele uitwisseling onderbreken bij een of meer transactiesets in de uitwisseling validatie is mislukt. |

## <a name="configure-how-your-agreement-sends-messages"></a>Configureren hoe berichten worden verzonden door uw overeenkomst

U kunt configureren hoe deze overeenkomst identificeert en uitgaande berichten die u naar uw partners via deze overeenkomst verzendt worden verwerkt.

1.  Onder **toevoegen**, selecteer **instellingen voor verzenden**.
Configureer deze eigenschappen op basis van uw overeenkomst met uw partner die berichten worden uitgewisseld met u. Zie voor eigenschapbeschrijvingen, de tabellen in deze sectie.

    **Verzendinstellingen** is onderverdeeld in deze secties: id's, bevestiging, schema's, enveloppen, tekensets en scheidingstekens, controlenummers en bevestigingen.

    !["Verzenden" configureren](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. Als u klaar bent, zorg ervoor dat u uw instellingen opslaan door te kiezen **OK**.

De overeenkomst is nu gereed om af te handelen uitgaande berichten die aan de geselecteerde instellingen voldoen.

### <a name="identifiers"></a>Id's

| Eigenschap | Beschrijving |
| --- | --- |
| UNB1.2 (Syntaxisversie) |Selecteer een waarde tussen **1** en **4**. |
| UNB2.3 (omgekeerd routeringsadres van de afzendeer) |Voer een alfanumerieke waarde met een minimaal één teken en maximaal 14 tekens bestaan. |
| UNB3.3 (omgekeerd routeringsadres van de ontvanger) |Voer een alfanumerieke waarde met een minimaal één teken en maximaal 14 tekens bestaan. |
| UNB6.1 (referentiewachtwoord van de ontvanger) |Voer een alfanumerieke waarde met een minimaal een en maximaal 14 tekens bestaan. |
| UNB6.2 (referentiekwalificatie van de ontvanger) |Voer een alfanumerieke waarde met een minimaal één teken en maximaal twee tekens bestaan. |
| UNB7 (toepassingsreferentie-id) |Voer een alfanumerieke waarde met een minimaal één teken en maximaal 14 tekens |

### <a name="acknowledgment"></a>Bevestiging
| Eigenschap | Beschrijving |
| --- | --- |
| Berichtontvangst (CONTRL) |Schakel dit selectievakje in als de gehoste partner verwacht voor het ontvangen van een technische (CONTRL)-bevestiging. Deze instelling geeft aan dat de gehoste partner die het bericht verzendt, aanvragen van een bevestiging van de gastpartner. |
| Bevestiging (CONTRL) |Schakel dit selectievakje in als de gehoste partner verwacht voor het ontvangen van een functionele (CONTRL)-bevestiging. Deze instelling geeft aan dat de gehoste partner die het bericht verzendt, aanvragen van een bevestiging van de gastpartner. |
| SG1/SG4-lus genereren voor geaccepteerde transactiesets |Als u ervoor kiest om aan te vragen van een functionele bevestiging, selecteert u dit selectievakje in om af te dwingen van het genereren van SG1/SG4-lus in functionele CONTRL bevestigingen voor geaccepteerde transactiesets. |

### <a name="schemas"></a>Schema 's
| Eigenschap | Beschrijving |
| --- | --- |
| UNH2.1 (TYPE) |Selecteer een type van de set transactie. |
| UNH2.2 (VERSIE) |Voer het versienummer van het bericht. |
| UNH2.3 (RELEASE) |Voer het versienummer van het bericht. |
| SCHEMA |Selecteer het schema moet worden gebruikt. Schema's vindt u in uw integratie-account. Voor toegang tot uw schema's, moet u eerst uw integratie-account koppelen aan uw logische app. |

### <a name="envelopes"></a>Enveloppen
| Eigenschap | Beschrijving |
| --- | --- |
| UNB8 (prioriteitcode voor verwerking) |Voer een alfabetische waarde die niet meer dan één teken bestaan. |
| UNB10 (communicatieovereenkomst) |Voer een alfanumerieke waarde met een minimaal één teken en maximaal 40 tekens. |
| UNB11 (testindicator) |Schakel dit selectievakje in om aan te geven dat de uitwisseling gegenereerd testgegevens |
| UNA-segment toepassen (advies voor de servicetekenreeks) |Schakel dit selectievakje in voor het genereren van een UNA-segment voor de uitwisseling moet worden verzonden. |
| UNG-segmenten toepassen (functiegroepheader) |Schakel dit selectievakje in groeperen om segmenten te maken in de functiegroep-header in de berichten die worden verzonden naar de gastpartner. De volgende waarden worden gebruikt om de UNG-segmenten te maken: <p>Voor **UNG1**, voer een alfanumerieke waarde met een minimaal één teken en maximaal zes tekens. <p>Voor **UNG2.1**, voer een alfanumerieke waarde met een minimaal één teken en maximaal 35 tekens in. <p>Voor **UNG2.2**, voer een alfanumerieke waarde, met een maximum van vier tekens. <p>Voor **UNG3.1**, voer een alfanumerieke waarde met een minimaal één teken en maximaal 35 tekens in. <p>Voor **UNG3.2**, voer een alfanumerieke waarde, met een maximum van vier tekens. <p>Voor **UNG6**, voer een alfanumerieke waarde met een minimaal een en maximaal drie tekens bestaan. <p>Voor **UNG7.1**, voer een alfanumerieke waarde met een minimaal één teken en maximaal drie tekens bestaan. <p>Voor **UNG7.2**, voer een alfanumerieke waarde met een minimaal één teken en maximaal drie tekens bestaan. <p>Voor **UNG7.3**, voer een alfanumerieke waarde met een minimaal 1 teken en maximaal 6 tekens. <p>Voor **UNG8**, voer een alfanumerieke waarde met een minimaal één teken en maximaal 14 tekens bestaan. |

### <a name="character-sets-and-separators"></a>Tekensets en scheidingstekens

Anders dan het teken is ingesteld, kunt u een andere set scheidingstekens moet worden gebruikt voor elk berichttype invoeren. Als een tekenset voor een bepaald bericht-schema niet is opgegeven, wordt de standaardtekenset gebruikt.

| Eigenschap | Beschrijving |
| --- | --- |
| UNB1.1 (systeem-id) |Selecteer de EDIFACT-tekenset moet worden toegepast op de uitgaande uitwisseling. |
| Schema |Selecteer een schema in de vervolgkeuzelijst. Nadat u elke rij hebt voltooid, wordt er automatisch een nieuwe rij toegevoegd. Selecteer voor het geselecteerde schema, de set met scheidingstekens die u gebruiken wilt, op basis van de onderstaande beschrijvingen van scheidingsteken voor duizendtallen. |
| Invoertype |Selecteer een type gebruikersinvoer in de vervolgkeuzelijst. |
| Scheidingsteken voor onderdelen |Afzonderlijke elementen samengestelde gegevens, voert u één teken. |
| Scheidingsteken voor gegevenselementen |Afzonderlijke eenvoudige gegevenselementen in samengestelde elementen, voer één teken. |
| Segmenteindteken |Geeft een enkel teken aan het einde van een EDI-segment. |
| Achtervoegsel |Selecteer het teken dat wordt gebruikt met de segment-id. Als u een achtervoegsel opgeeft, kan klikt u vervolgens het segmenteindteken element niet leeg zijn. Als de segmenteindteken leeg wordt gelaten, moet u een achtervoegsel opgeven. |

### <a name="control-numbers"></a>Controlenummers
| Eigenschap | Beschrijving |
| --- | --- |
| UNB5 (uitwisselingscontrolenummer) |Voer een voorvoegsel, een bereik van waarden voor het controlenummer uitwisseling en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van een uitgaande uitwisseling. Het voorvoegsel en het achtervoegsel zijn optioneel, terwijl het controlenummer vereist is. Het controlenummer wordt verhoogd voor elk nieuw bericht; het voorvoegsel en het achtervoegsel blijven hetzelfde. |
| UNG5 (groepscontrolenummer) |Voer een voorvoegsel, een bereik van waarden voor het controlenummer uitwisseling en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van het controlenummer voor de groep. Het voorvoegsel en het achtervoegsel zijn optioneel, terwijl het controlenummer vereist is. Het controlenummer wordt voor elk nieuw bericht verhoogd, totdat de maximumwaarde is bereikt. het voorvoegsel en het achtervoegsel blijven hetzelfde. |
| UNH1 (referentienummer van de berichtheader) |Voer een voorvoegsel, een bereik van waarden voor het controlenummer uitwisseling en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van de referentienummer van de bericht-header. Het voorvoegsel en het achtervoegsel zijn optioneel, terwijl het nummer vereist is. Het nummer wordt verhoogd voor elk nieuw bericht; het voorvoegsel en het achtervoegsel blijven hetzelfde. |

### <a name="validations"></a>Validaties

Wanneer elke rij van de validatie is voltooid, wordt een andere automatisch toegevoegd. Als u een van de regels niet opgeeft, wordt validatie van de rij 'Standaard'.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichttype |Selecteer het type EDI-bericht. |
| EDI-validatie |EDI-validatie op gegevenstypen zoals gedefinieerd door de EDI-eigenschappen van het schema, beperkingen voor de lengte, lege gegevenselementen en volgscheidingstekens uitvoeren. |
| Uitgebreide validatie |Als het gegevenstype niet EDI, valideren is op de gegevensvereiste-element en herhaling, opsommingen en element lengte gegevensvalidatie (min./Max.) toegestaan. |
| Voorloop-/volgnullen toestaan |Alle aanvullende voorloop- of afsluitende nul behouden en ruimte tekens. Niet deze tekens te verwijderen. |
| Voorloop-/volgnullen knippen |Voorloop- of afsluitende nul tekens verwijderen. |
| Beleid voor volgscheidingstekens |Navolgende scheidingstekens genereren. <p>Selecteer **niet toegestaan** verbiedt navolgende scheidingstekens en scheidingstekens in de uitwisseling verzonden. Als de uitwisseling navolgende scheidingstekens en scheidingstekens heeft, is de uitwisseling gedeclareerd niet geldig. <p>Selecteer **optioneel** uitwisselingen met of zonder afsluitende scheidingstekens en scheidingstekens verzenden. <p>Selecteer **verplichte** als de verzonden uitwisseling moet navolgende scheidingstekens en scheidingstekens bevatten. |

## <a name="find-your-created-agreement"></a>Uw overeenkomst gevonden

1.  Nadat u klaar bent met het instellen van alle uw overeenkomst-eigenschappen op de **toevoegen** pagina, kies **OK** te maken van uw overeenkomst voltooien en keer terug naar uw integratie-account.

    Nu uw zojuist toegevoegde overeenkomst wordt weergegeven in uw **overeenkomsten** lijst.

2.  U kunt ook uw overeenkomsten weergeven in een overzicht van uw integratie-account. Kies uw integratie-account in het menu van **overzicht**en selecteer vervolgens de **overeenkomsten** tegel. 

    ![Kies 'Overeenkomsten' tegel](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="view-swagger-file"></a>Swagger-bestand weergeven
De details Swagger voor de EDIFACT-connector, Zie [EDIFACT](/connectors/edifact/).

## <a name="learn-more"></a>Meer informatie
* [Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  

