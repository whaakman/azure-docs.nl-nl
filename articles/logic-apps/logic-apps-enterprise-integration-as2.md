---
title: AS2-berichten voor B2B enterprise integration - Azure Logic Apps | Microsoft Docs
description: Exchange AS2-berichten voor B2B enterprise integratie met Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 6a283d8772e48aa6671d88288c2083d891a220d5
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="exchange-as2-messages-for-enterprise-integration-with-logic-apps"></a>Exchange AS2-berichten voor enterprise-integratie met logic apps

Voordat u AS2 berichten voor Azure Logic Apps uitwisselen kunt, moet u een AS2-overeenkomst maken en opslaan van deze overeenkomst in uw account integratie. Hier volgen de stappen voor het maken van een AS2-overeenkomst.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig:

* Een [integratie account](../logic-apps/logic-apps-enterprise-integration-accounts.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw account integratie en geconfigureerd met de kwalificatie AS2 onder **Business identiteiten**

> [!NOTE]
> Wanneer u een overeenkomst maakt, moet het type licentieovereenkomst overeenkomen met de inhoud in de overeenkomst-bestand.    

Nadat u [maken van een account integratie](../logic-apps/logic-apps-enterprise-integration-accounts.md) en [partners toevoegen](logic-apps-enterprise-integration-partners.md), kunt u een AS2-overeenkomst met de volgende stappen.

## <a name="create-an-as2-agreement"></a>Maken van een AS2-overeenkomst

1.  Meld u aan bij [Azure Portal](http://portal.azure.com "Azure Portal").  

2.  Selecteer in het menu links **meer services**. Voer in het zoekvak **integratie** als filter. Selecteer in de lijst met resultaten **Integratieaccounts**.

    > [!TIP]
    > Als er geen **meer services**, mogelijk moet u eerst het menu uitvouwen. Selecteer aan de bovenkant van het menu samengevouwen **weergeven in het menu**.

    ![Meer services, filter op "-integratie", selecteren "Integratieaccounts"](./media/logic-apps-enterprise-integration-as2/overview-1.png)

3. In de **Integratieaccounts** blade die wordt geopend, selecteert u de integratie-account waarin u wilt maken van de overeenkomst.
Als er geen eventuele integratieaccounts [maken van een eerste](../logic-apps/logic-apps-enterprise-integration-accounts.md "alles over integratieaccounts").  

    ![Wanneer u de overeenkomst voor de integratie-account selecteren](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Kies de **overeenkomsten** tegel. Als u een tegel overeenkomsten geen hebt, eerst de tegel toevoegen.

    ![Kies 'Overeenkomsten' tegel](./media/logic-apps-enterprise-integration-as2/agreement-1.png)

5. Kies in de blade van de overeenkomsten die wordt geopend, **toevoegen**.

    ![Kies 'Add'](./media/logic-apps-enterprise-integration-as2/agreement-2.png)

6. Onder **toevoegen**, voer een **naam** voor de overeenkomst. Voor **type licentieovereenkomst**, selecteer **AS2**. Selecteer de **Host Partner**, **Hostidentiteit**, **Gast Partner**, en **Gast identiteit** voor de overeenkomst.

    ![Geef overeenkomstdetails](./media/logic-apps-enterprise-integration-as2/agreement-3.png)  

    | Eigenschap | Beschrijving |
    | --- | --- |
    | Naam |Naam van de overeenkomst |
    | Type licentieovereenkomst | AS2 moet |
    | Host-Partner |Een overeenkomst moet een host en de Gast-partner. De host-partner vertegenwoordigt de organisatie die de overeenkomst configureert. |
    | Identiteit van de host |Een id voor de host-partner |
    | Gast Partner |Een overeenkomst moet een host en de Gast-partner. De Gast-partner vertegenwoordigt de organisatie die bedrijven gebruiken met de partner host doet. |
    | Identiteit van de Gast |Een id voor de Gast-partner |
    | Instellingen te ontvangen |Deze eigenschappen zijn van toepassing op alle berichten ontvangen door een overeenkomst. |
    | Instellingen verzenden |Deze eigenschappen zijn van toepassing op alle berichten is verzonden door een overeenkomst. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Configureren hoe uw ingangen overeenkomst berichten ontvangen

Nu dat u de eigenschappen van de overeenkomst hebt ingesteld, kunt u configureren hoe deze overeenkomst identificeert en binnenkomende berichten ontvangen van uw partner via deze overeenkomst worden verwerkt.

1.  Onder **toevoegen**, selecteer **instellingen ontvangen**.
Configureer deze eigenschappen op basis van uw overeenkomst met de partner die u berichten worden uitgewisseld. Zie voor eigenschapbeschrijvingen, de tabel in deze sectie.

    !['Ontvangen instellingen' configureren](./media/logic-apps-enterprise-integration-as2/agreement-4.png)

2. Desgewenst kunt u de eigenschappen van binnenkomende berichten wijzigen door het selecteren van **overschrijven berichteigenschappen**.

3. Als u wilt alle inkomende berichten worden ondertekend, selecteer **bericht moet worden ondertekend**. Van de **certificaat** , selecteert u een bestaande [Gast partner openbaar certificaat](../logic-apps/logic-apps-enterprise-integration-certificates.md) voor het valideren van de handtekening op de berichten. Of het certificaat hebt gemaakt als u niet hebt.

4.  Als u wilt alle inkomende berichten moeten worden versleuteld, selecteer **bericht moet worden versleuteld**. Van de **certificaat** , selecteert u een bestaande [host partner persoonlijk certificaat](../logic-apps/logic-apps-enterprise-integration-certificates.md) voor het ontsleutelen van binnenkomende berichten. Of het certificaat hebt gemaakt als u niet hebt.

5. Selecteer om te vereisen berichten moeten worden gecomprimeerd, **bericht moet worden gecomprimeerd**.

6. Selecteer voor het verzenden van een synchrone disposition melding (MDN) voor ontvangen berichten **MDN verzenden**.

7. Selecteer voor het verzenden van ondertekende MDNs voor ontvangen berichten **verzenden ondertekend MDN**.

8. Selecteer voor het verzenden van asynchrone MDNs voor ontvangen berichten **verzenden van asynchrone MDN**.

9. Nadat u bent klaar, controleert u of uw instellingen opslaan door te kiezen **OK**.

Uw overeenkomst is nu gereed voor het verwerken van inkomende berichten die aan de geselecteerde instellingen voldoen.

| Eigenschap | Beschrijving |
| --- | --- |
| Berichteigenschappen van het overschrijven |Hiermee wordt aangegeven dat eigenschappen ontvangen berichten kunnen worden genegeerd. |
| Bericht moet worden ondertekend. |Vereist berichten digitaal worden ondertekend. Configureer het openbare certificaat van Gast partner voor handtekeningverificatie.  |
| Bericht moet worden versleuteld. |Vereist berichten te versleutelen. Niet-versleutelde berichten worden geweigerd. Configureer het persoonlijke host partner-certificaat voor het ontsleutelen van berichten.  |
| Bericht moet worden gecomprimeerd |Vereist berichten moeten worden gecomprimeerd. Niet-gecomprimeerde berichten worden geweigerd. |
| MDN tekst |De standaard disposition melding (MDN) worden verzonden naar de afzender. |
| MDN verzenden |Vereist MDNs worden verzonden. |
| Ondertekende MDN verzenden |Vereist MDNs zijn ondertekend. |
| MIC-algoritme |Selecteer het algoritme moet worden gebruikt voor het ondertekenen van berichten. |
| Asynchrone MDN verzenden | Vereist berichten asynchroon worden verzonden. |
| URL | Geef de URL waar de MDNs. |

## <a name="configure-how-your-agreement-sends-messages"></a>Configureren hoe uw overeenkomst verzendt berichten

U kunt configureren hoe deze overeenkomst identificeert en uitgaande berichten verwerkt die u naar uw partners via deze overeenkomst verzendt.

1.  Onder **toevoegen**, selecteer **instellingen voor verzenden**.
Configureer deze eigenschappen op basis van uw overeenkomst met de partner die u berichten worden uitgewisseld. Zie voor eigenschapbeschrijvingen, de tabel in deze sectie.

    ![Stel de eigenschappen 'Instellingen verzenden'](./media/logic-apps-enterprise-integration-as2/agreement-51.png)

2. Ondertekende om berichten te verzenden naar uw partner, selecteer **inschakelen ondertekenen van berichten**. Voor het ondertekenen van de berichten in de **MIC algoritme** lijst, selecteert de *host partner persoonlijk certificaat MIC algoritme*. En in de **certificaat** , selecteert u een bestaande [host partner persoonlijk certificaat](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. Voor het verzenden van versleutelde berichten naar de partner, selecteer **berichtversleuteling inschakelen**. Voor het versleutelen van de berichten de **versleutelingsalgoritme** lijst, selecteert de *Gast partner openbaar certificaat algoritme*.
En in de **certificaat** , selecteert u een bestaande [Gast partner openbaar certificaat](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. Selecteer voor het comprimeren van het bericht **bericht compressie inschakelen**.

5. Als u wilt de HTTP-header content-type uitgevouwen tot een enkele regel, selecteer **uitgevouwen HTTP-headers**.

6. Selecteer voor het ontvangen van synchrone MDNs voor de verzonden berichten **MDN aanvragen**.

7. Selecteer voor het ontvangen van ondertekende MDNs voor de verzonden berichten **aanvraag ondertekend MDN**.

8. Selecteer voor het ontvangen van asynchrone MDNs voor de verzonden berichten **aanvragen asynchrone MDN**. Als u deze optie selecteert, voert u de URL voor de locatie waar de MDNs verzenden.

9. Selecteer hiervoor niet-afwijzing van ontvangst **NRR inschakelen**.  

10. Als u wilt opgeven algoritme indeling moet worden gebruikt in de MIC of het aanmelden van de uitgaande kopteksten aan het AS2-bericht of MDN **SHA2 algoritme indeling**.  

11. Nadat u bent klaar, controleert u of uw instellingen opslaan door te kiezen **OK**.

Uw overeenkomst is nu gereed voor het verwerken van uitgaande berichten die aan de geselecteerde instellingen voldoen.

| Eigenschap | Beschrijving |
| --- | --- |
| Ondertekenen van berichten inschakelen |Vereist dat alle berichten die zijn verzonden vanaf de overeenkomst moet worden ondertekend. |
| MIC-algoritme |Het algoritme moet worden gebruikt voor het ondertekenen van berichten. Hiermee configureert u de host partner persoonlijk certificaat MIC-algoritme voor het ondertekenen van berichten. |
| Certificaat |Selecteer het certificaat moet worden gebruikt voor het ondertekenen van berichten. Hiermee configureert u de host partner persoonlijk certificaat voor ondertekening van de berichten. |
| Berichtcodering inschakelen |Versleuteling van alle berichten die worden verzonden vanuit deze overeenkomst is vereist. Hiermee configureert u het algoritme Gast partner openbaar certificaat te gebruiken voor het versleutelen van berichten. |
| Versleutelingsalgoritme |Het versleutelingsalgoritme voor berichtversleuteling. Hiermee configureert u de partner Gast openbaar certificaat voor het versleutelen van berichten. |
| Certificaat |Het certificaat moet worden gebruikt om berichten te versleutelen. Hiermee configureert u de partner Gast persoonlijk certificaat voor het versleutelen van berichten. |
| Bericht compressie inschakelen |Compressie van alle berichten die worden verzonden vanuit deze overeenkomst is vereist. |
| HTTP-headers uitgevouwen |De HTTP-inhoudstype-header op één regel plaatst. |
| Aanvraag MDN |Vereist een MDN voor alle berichten die van deze overeenkomst worden verzonden. |
| Aanvraag MDN ondertekend |Vereist dat alle MDNs die worden verzonden naar deze overeenkomst worden ondertekend. |
| Asynchrone MDN aanvraag |Asynchrone MDNs worden verzonden naar deze overeenkomst is vereist. |
| URL |Geef de URL waar de MDNs. |
| NRR inschakelen |Niet-afwijzing van ontvangst (NRR), een communicatiekenmerk dat bewijst vereist dat de gegevens zijn geadresseerd ontvangen. |
| Indeling van het SHA2-algoritme |Selecteer algoritme indeling in de MIC of de uitgaande kopteksten aan het AS2-bericht of MDN aanmelden |

## <a name="find-your-created-agreement"></a>De overeenkomst zoeken

1.  Als u klaar bent met het instellen van alle uw overeenkomst-eigenschappen op de **toevoegen** blade kiezen **OK** maken van uw overeenkomst is voltooid en Ga terug naar de blade van het integratie-account.

    Nu uw toegevoegde overeenkomst wordt weergegeven in uw **overeenkomsten** lijst.

2.  U kunt ook uw overeenkomsten weergeven in uw Accountoverzicht integratie. Kies op de blade van het integratie-account **overzicht**, selecteer vervolgens de **overeenkomsten** tegel. 

    ![Kies 'Overeenkomsten' tegel om weer te geven van alle overeenkomsten](./media/logic-apps-enterprise-integration-as2/agreement-6.png)

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [swagger details](/connectors/as2/). 

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  
