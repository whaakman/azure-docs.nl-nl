---
title: EDIFACT-berichten voor B2B enterprise integration - Azure Logic Apps | Microsoft Docs
description: Exchange EDIFACT berichten in EDI-indeling voor B2B enterprise integratie met Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 2257d2c8-1929-4390-b22c-f96ca8b291bc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/26/2016
ms.author: LADocs; jonfan
ms.openlocfilehash: fc9a0068de5f9464133eec0b043fbba1dc0fbde7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="exchange-edifact-messages-for-enterprise-integration-with-logic-apps"></a>Exchange EDIFACT berichten voor enterprise-integratie met logic apps

Voordat u EDIFACT berichten voor Azure Logic Apps uitwisselen kunt, moet u een overeenkomst EDIFACT maken en opslaan van deze overeenkomst in uw account integratie. Hier volgen de stappen voor het maken van een overeenkomst EDIFACT.

> [!NOTE]
> Deze pagina bevat informatie over de functies EDIFACT voor Azure Logic Apps. Zie voor meer informatie [X12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een [integratie account](../logic-apps/logic-apps-enterprise-integration-accounts.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement  
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw account integratie

> [!NOTE]
> Wanneer u een overeenkomst maakt, moet het type licentieovereenkomst overeenkomen met de inhoud van de berichten die u ontvangt of verzendt naar en van de partner.

Nadat u [maken van een account integratie](../logic-apps/logic-apps-enterprise-integration-accounts.md) en [partners toevoegen](logic-apps-enterprise-integration-partners.md), kunt u een overeenkomst EDIFACT maken met de volgende stappen.

## <a name="create-an-edifact-agreement"></a>Maken van een overeenkomst EDIFACT 

1.  Meld u aan bij [Azure Portal](http://portal.azure.com "Azure Portal"). Selecteer in het menu links **meer services**.

    > [!TIP]
    > Als er geen **meer services**, mogelijk moet u eerst het menu uitvouwen. Selecteer aan de bovenkant van het menu samengevouwen **weergeven in het menu**.

    ![Selecteer op de linkermenu 'Meer services'](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

2. Typ in het zoekvak '-integratie"voor uw filter. Selecteer in de lijst met resultaten **Integratieaccounts**.

    ![Filteren op "-integratie", selecteert u 'Integratieaccounts'](./media/logic-apps-enterprise-integration-edifact/edifact-1-3.png)

3. In de **Integratieaccounts** blade die wordt geopend, selecteert u de integratie-account waarin u wilt maken van de overeenkomst.
Als er geen eventuele integratieaccounts [maken van een eerste](../logic-apps/logic-apps-enterprise-integration-accounts.md "alles over integratieaccounts").  

    ![Integratie-account selecteren wanneer u de overeenkomst](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. Kies de **overeenkomsten** tegel. Als u een tegel overeenkomsten geen hebt, eerst de tegel toevoegen.   

    ![Kies 'Overeenkomsten' tegel](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. Kies in de blade van de overeenkomsten die wordt geopend, **toevoegen**.

    ![Kies 'Add'](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. Onder **toevoegen**, voer een **naam** voor de overeenkomst. Voor **type licentieovereenkomst**, selecteer **EDIFACT**. Selecteer de **Host Partner**, **Hostidentiteit**, **Gast Partner**, en **Gast identiteit** voor de overeenkomst.

    ![Geef overeenkomstdetails](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

    | Eigenschap | Beschrijving |
    | --- | --- |
    | Naam |Naam van de overeenkomst |
    | Type licentieovereenkomst | EDIFACT moet |
    | Host-Partner |Een overeenkomst moet een host en de Gast-partner. De host-partner vertegenwoordigt de organisatie die de overeenkomst configureert. |
    | Identiteit van de host |Een id voor de host-partner |
    | Gast Partner |Een overeenkomst moet een host en de Gast-partner. De Gast-partner vertegenwoordigt de organisatie die bedrijven gebruiken met de partner host doet. |
    | Identiteit van de Gast |Een id voor de Gast-partner |
    | Instellingen te ontvangen |Deze eigenschappen zijn van toepassing op alle berichten ontvangen door een overeenkomst. |
    | Instellingen verzenden |Deze eigenschappen zijn van toepassing op alle berichten is verzonden door een overeenkomst. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Configureren hoe uw ingangen overeenkomst berichten ontvangen

Nu dat u de eigenschappen van de overeenkomst hebt ingesteld, kunt u configureren hoe deze overeenkomst identificeert en binnenkomende berichten ontvangen van uw partner via deze overeenkomst worden verwerkt.

1.  Onder **toevoegen**, selecteer **instellingen ontvangen**.
Configureer deze eigenschappen op basis van uw overeenkomst met de partner die u berichten worden uitgewisseld. Zie voor eigenschapbeschrijvingen, de tabellen in deze sectie.

    **Ontvangen instellingen** is onderverdeeld in deze secties: id's, bevestiging, schema's, besturingselement getallen, validatie en interne instellingen.

    !['Ontvangen instellingen' configureren](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. Nadat u bent klaar, controleert u of uw instellingen opslaan door te kiezen **OK**.

Uw overeenkomst is nu gereed voor het verwerken van inkomende berichten die aan de geselecteerde instellingen voldoen.

### <a name="identifiers"></a>Id 's

| Eigenschap | Beschrijving |
| --- | --- |
| UNB6.1 (ontvanger verwijzing wachtwoord) |Voer een alfanumerieke waarde opgeven tussen 1 en 14 tekens. |
| UNB6.2 (ontvanger verwijzing kwalificatie) |Voer een alfanumerieke waarde met een minimum van één teken en maximaal twee tekens bestaan. |

### <a name="acknowledgments"></a>Bevestigingen

| Eigenschap | Beschrijving |
| --- | --- |
| Berichtontvangst (CONTRL) |Schakel dit selectievakje dat een bevestiging van de technische (CONTRL) naar de DIF-afzender. De bevestiging is verzonden naar de DIF-afzender op basis van de instellingen voor verzenden voor de overeenkomst. |
| Bevestiging (CONTRL) |Selecteer dit vakje voor een functionele (CONTRL) bevestiging terug naar de afzender interchange de bevestiging is verzonden naar de DIF-afzender op basis van de instellingen voor verzenden voor de overeenkomst. |

### <a name="schemas"></a>Schema 's

| Eigenschap | Beschrijving |
| --- | --- |
| UNH2.1 (TYPE) |Selecteer een set transactietype. |
| UNH2.2 (VERSIE) |Voer het versienummer van het bericht. (Minimum, één teken; maximum drie tekens). |
| UNH2.3 (VERSIE) |Voer het nummer van het release. (Minimum, één teken; maximum drie tekens). |
| UNH2.5 (GEKOPPELDE TOEGEWEZEN CODE) |Voer de code toegewezen. (Maximaal zes tekens. Moet alfanumerieke). |
| UNG2.1 (APP AFZENDER-ID) |Voer een alfanumerieke waarde met een minimum van één teken en maximaal 35 tekens. |
| UNG2.2 (APP AFZENDER CODE KWALIFICATIE) |Voer een alfanumerieke waarde met een maximum van vier tekens. |
| SCHEMA |Selecteer het eerder geüploade schema dat u uit uw account gekoppeld integratie wilt gebruiken. |

### <a name="control-numbers"></a>Besturingselement cijfers
| Eigenschap | Beschrijving |
| --- | --- |
| Controle-aantal Interchange duplicaten weigeren |Voor het blokkeren van dubbele uitgewisseld, selecteert u deze eigenschap. Indien geselecteerd, controleert de actie voor het decoderen van EDIFACT interchange besturingselementnummer (UNB5) voor het ontvangen uitwisselen komt niet overeen met een getal eerder verwerkte DIF-besturingselement. Als een overeenkomst wordt gedetecteerd, is klikt u vervolgens het knooppunt niet verwerkt. |
| Controleren op dubbele UNB5 elke (dagen) |Als u ervoor kiest de dubbele interchange besturingselement cijfers niet toestaan, kunt u het aantal dagen opgeven voor het uitvoeren van de controle door middel van de juiste waarde voor deze instelling. |
| Dubbele groepscontrolenummers niet toestaan |Als u wilt blokkeren uitgewisseld met dubbele groep besturingselement-nummers (UNG5), selecteert u deze eigenschap. |
| Dubbele transactiereekscontrolenummers niet toestaan |Als u wilt blokkeren uitgewisseld met dubbele transactie set besturingselement getallen (UNH1), selecteert u deze eigenschap. |
| EDIFACT bevestiging controle-aantal |Voer een waarde voor het voorvoegsel, een bereik van referenties en een achtervoegsel om de transactie set nummers voor gebruik in een bevestiging. |

### <a name="validations"></a>Validaties

Wanneer elke rij van de validatie is voltooid, wordt automatisch een andere toegevoegd. Als u geen regels opgeeft, gebruikt validatie van de rij 'Standaard'.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichttype |Selecteer het type EDI-bericht. |
| EDI-validatie |EDI-validatie wordt uitgevoerd op gegevenstypen zoals gedefinieerd door het schema EDI-eigenschappen, beperkingen voor de lengte, leeg gegevenselementen en afsluitende scheidingstekens. |
| Uitgebreide validatie |Als het gegevenstype niet EDI, validatie is op de gegevensvereiste-element en herhaling, opsommingen en element lengte gegevensvalidatie (min/max) toegestaan. |
| Voorloop-/ Volg nullen toestaan |Alle aanvullende voorloop- of afsluitende nul behouden en ruimte tekens. Verwijder niet deze tekens bevatten. |
| Trim voorloop-/ Volg nullen |Voorloop- of volgspaties nul en spaties verwijderen. |
| Afsluitende scheidingsteken beleid |Afsluitende scheidingstekens genereren. <p>Selecteer **niet toegestaan** verbiedt afsluitende scheidingstekens en scheidingstekens in het knooppunt ontvangen. Als de uitwisseling afsluitende scheidingstekens en scheidingstekens heeft, is het knooppunt gedeclareerd niet geldig. <p>Selecteer **optioneel** uitgewisseld met of zonder een afsluitende scheidingstekens en scheidingstekens accepteren. <p>Selecteer **verplichte** wanneer de ontvangen uitwisseling afsluitende scheidingstekens en scheidingstekens moet hebben. |

### <a name="internal-settings"></a>Interne instellingen

| Eigenschap | Beschrijving |
| --- | --- |
| Lege XML-tags maken als volgscheidingstekens zijn toegestaan |Schakel dit selectievakje in om de DIF-afzender lege XML-codes voor afsluitende scheidingstekens bevatten. |
| Uitwisseling splitsen in transactiereeksen - transactiereeksen onderbreken bij fout|Analyseert elke transactie instellen in een knooppunt in een afzonderlijke XML-document met de juiste envelop toepassen op de set transactie. Onderbreken van de transactie-sets die validatie is mislukt. |
| Uitwisseling splitsen in transactiereeksen - uitwisseling onderbreken bij fout|Elke transactie instellen in een knooppunt in een afzonderlijke XML-document met het toepassen van de juiste envelop parseert. De gehele uitwisseling onderbreken wanneer een of meer sets transactie in het knooppunt validatie is mislukt. | 
| Behouden DIF - transactie sets onderbreken bij fout |Laat de DIF intact, maakt een XML-document voor de hele batch uitwisseling. Onderbreken van de transactie-sets die niet voldoen aan validatie, terwijl alle andere sets transactie verwerken. |
| Uitwisseling bewaren - uitwisseling onderbreken bij fout |Laat de DIF intact, maakt een XML-document voor de hele batch uitwisseling. De gehele uitwisseling onderbreken wanneer een of meer sets transactie in het knooppunt validatie is mislukt. |

## <a name="configure-how-your-agreement-sends-messages"></a>Configureren hoe uw overeenkomst verzendt berichten

U kunt configureren hoe deze overeenkomst identificeert en uitgaande berichten verwerkt die u naar uw partners via deze overeenkomst verzendt.

1.  Onder **toevoegen**, selecteer **instellingen voor verzenden**.
Deze eigenschappen op basis van uw overeenkomst met uw partner die berichten worden uitgewisseld met u configureren. Zie voor eigenschapbeschrijvingen, de tabellen in deze sectie.

    **Instellingen verzenden** is onderverdeeld in deze secties: id's, bevestiging, schema's, enveloppen, tekensets en scheidingstekens, Control Numbers en validaties.

    ![Configureer 'Verzenden instellingen'](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. Nadat u bent klaar, controleert u of uw instellingen opslaan door te kiezen **OK**.

Uw overeenkomst is nu gereed voor het verwerken van uitgaande berichten die aan de geselecteerde instellingen voldoen.

### <a name="identifiers"></a>Id 's

| Eigenschap | Beschrijving |
| --- | --- |
| UNB1.2 (syntaxis versie) |Selecteer een waarde tussen **1** en **4**. |
| UNB2.3 (omgekeerd routeringsadres van de afzendeer) |Voer een alfanumerieke waarde met een minimum van één teken en maximaal 14 tekens. |
| UNB3.3 (omgekeerd routeringsadres van de ontvanger) |Voer een alfanumerieke waarde met een minimum van één teken en maximaal 14 tekens. |
| UNB6.1 (ontvanger verwijzing wachtwoord) |Voer een alfanumerieke waarde met een minimaal en maximaal 14 tekens. |
| UNB6.2 (ontvanger verwijzing kwalificatie) |Voer een alfanumerieke waarde met een minimum van één teken en maximaal twee tekens bestaan. |
| UNB7 (toepassing verwijzings-ID) |Voer een alfanumerieke waarde met een minimum van één teken en maximaal 14 tekens |

### <a name="acknowledgment"></a>Bevestiging
| Eigenschap | Beschrijving |
| --- | --- |
| Berichtontvangst (CONTRL) |Schakel dit selectievakje in als de gehoste partner verwacht te ontvangen van een bevestiging van de technische (CONTRL). Deze instelling geeft aan dat de gehoste partner die het bericht verzendt, aanvragen van een bevestiging van de Gast-partner. |
| Bevestiging (CONTRL) |Schakel dit selectievakje in als de gehoste partner verwacht te ontvangen van een functionele (CONTRL) bevestiging. Deze instelling geeft aan dat de gehoste partner die het bericht verzendt, aanvragen van een bevestiging van de Gast-partner. |
| SG1/SG4-lus genereren voor geaccepteerde transactiesets |Als u hebt gekozen om aan te vragen van een functionele bevestiging, selecteert u dit selectievakje in om af te dwingen van het genereren van SG1/SG4 lussen in functionele CONTRL bevestigingen voor geaccepteerde transactie sets. |

### <a name="schemas"></a>Schema 's
| Eigenschap | Beschrijving |
| --- | --- |
| UNH2.1 (TYPE) |Selecteer een set transactietype. |
| UNH2.2 (VERSIE) |Voer het versienummer van het bericht. |
| UNH2.3 (VERSIE) |Voer het nummer van het release. |
| SCHEMA |Selecteer het schema te gebruiken. Schema's bevinden zich in uw account integratie. Voor toegang tot uw schema, moet u eerst uw integratie-account koppelen aan uw logische app. |

### <a name="envelopes"></a>Enveloppen
| Eigenschap | Beschrijving |
| --- | --- |
| UNB8 (prioriteitscode Processing) |Een alfabetische waarde invoeren die is niet meer dan één teken lang. |
| UNB10 (overeenkomst communicatie) |Voer een alfanumerieke waarde met een minimum van één teken en maximaal 40 tekens. |
| UNB11 (testen Indicator) |Schakel dit selectievakje in om aan te geven dat de uitwisseling gegenereerd testgegevens |
| UNA-segment toepassen (advies voor de servicetekenreeks) |Schakel dit selectievakje in voor het genereren van een segment UNA voor de uitwisseling worden verzonden. |
| UNG-segmenten toepassen (functiegroepheader) |Selecteer dit vakje voor groepering segmenten maken in de koptekst functionele groep in de berichten die worden verzonden naar de Gast-partner. De volgende waarden worden gebruikt om de segmenten UNG te maken: <p>Voor **UNG1**, voer een alfanumerieke waarde met een minimum van één teken en maximaal zes tekens. <p>Voor **UNG2.1**, voer een alfanumerieke waarde met een minimum van één teken en maximaal 35 tekens. <p>Voor **UNG2.2**, voer een alfanumerieke waarde, met een maximum van vier tekens. <p>Voor **UNG3.1**, voer een alfanumerieke waarde met een minimum van één teken en maximaal 35 tekens. <p>Voor **UNG3.2**, voer een alfanumerieke waarde, met een maximum van vier tekens. <p>Voor **UNG6**, voer een alfanumerieke waarde met een minimaal en maximaal drie tekens bestaan. <p>Voor **UNG7.1**, voer een alfanumerieke waarde met ten minste één teken en maximaal drie tekens bestaan. <p>Voor **UNG7.2**, voer een alfanumerieke waarde met ten minste één teken en maximaal drie tekens bestaan. <p>Voor **UNG7.3**, voer een alfanumerieke waarde met een minimum van 1 teken en maximaal 6 tekens. <p>Voor **UNG8**, voer een alfanumerieke waarde met een minimum van één teken en maximaal 14 tekens. |

### <a name="character-sets-and-separators"></a>Tekensets en scheidingstekens

Andere dan de tekenset, kunt u een andere set scheidingstekens moet worden gebruikt voor elk berichttype. Als een tekenset voor een opgegeven bericht-schema niet is opgegeven, wordt de standaardtekenset gebruikt.

| Eigenschap | Beschrijving |
| --- | --- |
| UNB1.1 (System Identifier) |Selecteer de EDIFACT tekenset worden toegepast op het uitgaande uitwisselen. |
| Schema |Selecteer een schema in de vervolgkeuzelijst. Nadat u elke rij hebt voltooid, wordt er automatisch een nieuwe rij toegevoegd. Voor het geselecteerde schema, de scheidingstekens set selecteren die u gebruiken wilt, op basis van de volgende beschrijvingen van het scheidingsteken. |
| Invoertype |Selecteer een type invoer in de vervolgkeuzelijst. |
| Onderdeel scheidingselement |Afzonderlijke elementen van de samengestelde gegevensbron, voert u één teken. |
| Gegevens Element scheidingselement |Afzonderlijke eenvoudige gegevenselementen in de elementen van de samengestelde gegevensbron, voert u één teken. |
| Segment eindteken |Geeft een enkel teken aan het einde van een EDI-segment. |
| Achtervoegsel |Selecteer het teken dat wordt gebruikt met de segment-id. Als u een achtervoegsel opgeeft, kan het gegevenselement voor segment eindteken leeg zijn. Als de segment-terminator leeg wordt gelaten, moet u een achtervoegsel toewijzen. |

### <a name="control-numbers"></a>Besturingselement cijfers
| Eigenschap | Beschrijving |
| --- | --- |
| UNB5 (Interchange controle-aantal) |Voer een voorvoegsel, een bereik van waarden voor het besturingselement interchange getal en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van een uitgaande knooppunt. Voor- en achtervoegsel zijn optioneel, terwijl het besturingselementnummer vereist is. Het besturingselementnummer wordt verhoogd voor elk nieuw bericht; voor- en achtervoegsel blijven hetzelfde. |
| UNG5 (groep controle-aantal) |Voer een voorvoegsel, een bereik van waarden voor het besturingselement interchange getal en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van het nummer van het besturingselement. Voor- en achtervoegsel zijn optioneel, terwijl het besturingselementnummer vereist is. Het besturingselementnummer wordt voor elk nieuw bericht verhoogd tot de maximale waarde is bereikt; voor- en achtervoegsel blijven hetzelfde. |
| UNH1 (Message Header referentienummer) |Voer een voorvoegsel, een bereik van waarden voor het besturingselement interchange getal en een achtervoegsel. Deze waarden worden gebruikt voor het genereren van het referentienummer voor bericht-header. Voor- en achtervoegsel zijn optioneel, terwijl het referentienummer vereist is. Het nummer wordt verhoogd voor elk nieuw bericht; voor- en achtervoegsel blijven hetzelfde. |

### <a name="validations"></a>Validaties

Wanneer elke rij van de validatie is voltooid, wordt automatisch een andere toegevoegd. Als u geen regels opgeeft, gebruikt validatie van de rij 'Standaard'.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichttype |Selecteer het type EDI-bericht. |
| EDI-validatie |EDI-validatie wordt uitgevoerd op gegevenstypen zoals gedefinieerd door de EDI-eigenschappen van het schema, beperkingen voor de lengte, leeg gegevenselementen en afsluitende scheidingstekens. |
| Uitgebreide validatie |Als het gegevenstype niet EDI, validatie is op de gegevensvereiste-element en herhaling, opsommingen en element lengte gegevensvalidatie (min/max) toegestaan. |
| Voorloop-/ Volg nullen toestaan |Alle aanvullende voorloop- of afsluitende nul behouden en ruimte tekens. Verwijder niet deze tekens bevatten. |
| Trim voorloop-/ Volg nullen |Voorloop- of afsluitende nul tekens verwijderen. |
| Afsluitende scheidingsteken beleid |Afsluitende scheidingstekens genereren. <p>Selecteer **niet toegestaan** verbiedt afsluitende scheidingstekens en scheidingstekens in het knooppunt verzonden. Als de uitwisseling afsluitende scheidingstekens en scheidingstekens heeft, is het knooppunt gedeclareerd niet geldig. <p>Selecteer **optioneel** uitgewisseld met of zonder een afsluitende scheidingstekens en scheidingstekens verzenden. <p>Selecteer **verplichte** als de verzonden uitwisseling afsluitende scheidingstekens en scheidingstekens nodig hebt. |

## <a name="find-your-created-agreement"></a>De overeenkomst zoeken

1.  Als u klaar bent met het instellen van alle uw overeenkomst-eigenschappen op de **toevoegen** blade kiezen **OK** maken van uw overeenkomst is voltooid en Ga terug naar de blade van het integratie-account.

    Nu uw toegevoegde overeenkomst wordt weergegeven in uw **overeenkomsten** lijst.

2.  U kunt ook uw overeenkomsten weergeven in uw Accountoverzicht integratie. Kies op de blade van het integratie-account **overzicht**, selecteer vervolgens de **overeenkomsten** tegel. 

    ![Kies 'Overeenkomsten' tegel om weer te geven van alle overeenkomsten](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="view-swagger-file"></a>Swagger-bestand weergeven
De Swagger-details voor de connector EDIFACT Zie [EDIFACT](/connectors/edifact/).

## <a name="learn-more"></a>Meer informatie
* [Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  

