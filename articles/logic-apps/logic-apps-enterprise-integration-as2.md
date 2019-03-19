---
title: AS2-berichten voor B2B-bedrijfsintegratie - Azure Logic Apps | Microsoft Docs
description: AS2-berichten uitwisselen voor B2B-bedrijfsintegratie in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.date: 06/08/2017
ms.openlocfilehash: 3413b235d9202530eb1a3129637e3746bbe6585b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57872556"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>AS2-berichten uitwisselen voor B2B-bedrijfsintegratie in Azure Logic Apps met Enterprise Integration Pack

Voordat u AS2-berichten voor Azure Logic Apps uitwisselen kan, moet u een AS2-overeenkomst maken en opslaan van deze overeenkomst in uw integratie-account. Hier volgen de stappen voor het maken van een AS2-overeenkomst.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-accounts.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw integratie-account en geconfigureerd met de kwalificatie van de AS2 onder **Bedrijfsidentiteiten**

> [!NOTE]
> Wanneer u een overeenkomst maakt, moet het overeenkomsttype overeenkomen met de inhoud in de overeenkomst-bestand.    

Nadat u [maken van een integratieaccount](../logic-apps/logic-apps-enterprise-integration-accounts.md) en [partners toevoegen](logic-apps-enterprise-integration-partners.md), kunt u een AS2-overeenkomst maken door deze stappen te volgen.

## <a name="create-an-as2-agreement"></a>Maken van een AS2-overeenkomst

1.  Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal").  

2. Selecteer in het hoofdmenu van Azure **alle services**. Voer 'integration' in het zoekvak en selecteer vervolgens **integratieaccounts**.

   ![Uw integratie-account zoeken](./media/logic-apps-enterprise-integration-as2/overview-1.png)

   > [!TIP]
   > Als er geen **alle services**, mogelijk hebt u eerst het menu uitvouwen. Selecteer aan de bovenkant van het menu samengevouwen **tekstlabels weergeven**.

3. Onder **Integratieaccounts**, selecteert u de integratieaccount waar u wilt maken van de overeenkomst.

   ![Selecteer het integratieaccount waar moet worden gemaakt van de overeenkomst](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Kies de **overeenkomsten** tegel. Als u geen een tegel overeenkomsten, eerst de tegel toevoegen.

    ![Kies 'Overeenkomsten' tegel](./media/logic-apps-enterprise-integration-as2/agreement-1.png)

5. Onder **overeenkomsten**, kiest u **toevoegen**.

    ![Kies 'Toevoegen'](./media/logic-apps-enterprise-integration-as2/agreement-2.png)

6. Onder **toevoegen**, voer een **naam** voor uw overeenkomst. Voor **overeenkomsttype**, selecteer **AS2**. Selecteer de **Hostpartner**, **Hostidentiteit**, **Gastpartner**, en **Gastidentiteit** voor uw overeenkomst.

    ![Geef overeenkomstdetails](./media/logic-apps-enterprise-integration-as2/agreement-3.png)  

    | Eigenschap | Description |
    | --- | --- |
    | Name |Naam van de overeenkomst |
    | Overeenkomsttype | Moet AS2 |
    | Hostpartner |Een overeenkomst moet een host en de Gast-partner. De hostpartner vertegenwoordigt de organisatie die Hiermee configureert u de overeenkomst. |
    | Hostidentiteit |Een id voor de hostpartner |
    | Gastpartner |Een overeenkomst moet een host en de Gast-partner. De gastpartner vertegenwoordigt de organisatie die wordt zakendoen met de hostpartner. |
    | Gastidentiteit |Een id voor de gastpartner |
    | Ontvangstinstellingen |Deze eigenschappen zijn van toepassing op alle berichten ontvangen door een overeenkomst. |
    | Verzendinstellingen |Deze eigenschappen zijn van toepassing op alle berichten die door een overeenkomst worden verzonden. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Configureren hoe uw overeenkomst verwerkt berichten ontvangen

Nu u de eigenschappen van de overeenkomst hebt ingesteld, kunt u configureren hoe deze overeenkomst identificeert en binnenkomende berichten ontvangen van uw partner via deze overeenkomst worden verwerkt.

1.  Onder **toevoegen**, selecteer **instellingen ontvangen**.
Configureer deze eigenschappen op basis van uw overeenkomst met de partner die berichten met u worden uitgewisseld. Zie voor eigenschapbeschrijvingen, de tabel in deze sectie.

    !['Ontvangen instellingen' configureren](./media/logic-apps-enterprise-integration-as2/agreement-4.png)

2. U kunt eventueel de eigenschappen van binnenkomende berichten overschrijven door te selecteren **berichteigenschappen negeren**.

3. Selecteer om te vereisen dat alle inkomende berichten moeten worden ondertekend, **bericht moet ondertekend zijn**. Uit de **certificaat** , selecteert u een bestaande [Gast partner openbaar certificaat](../logic-apps/logic-apps-enterprise-integration-certificates.md) voor het valideren van de handtekening van de berichten. Of maak het certificaat, als u dit niet hebt.

4.  Selecteer om te vereisen dat alle inkomende berichten moeten worden versleuteld, **bericht moet worden versleuteld**. Uit de **certificaat** , selecteert u een bestaande [host partner persoonlijk certificaat](../logic-apps/logic-apps-enterprise-integration-certificates.md) voor het ontsleutelen van binnenkomende berichten. Of maak het certificaat, als u dit niet hebt.

5. Selecteer om te vereisen dat berichten moeten worden gecomprimeerd, **bericht moet worden gecomprimeerd**.

6. Selecteer voor het verzenden van een synchrone toestand melding (MDN) voor de ontvangen berichten, **MDN verzenden**.

7. Selecteer voor het verzenden van ondertekende MDN's voor de ontvangen berichten, **ondertekende MDN verzenden**.

8. Selecteer voor het verzenden van asynchrone MDN's voor de ontvangen berichten, **verzenden van asynchrone MDN**.

9. Als u klaar bent, zorg ervoor dat u uw instellingen opslaan door te kiezen **OK**.

De overeenkomst is nu gereed voor het afhandelen van berichten die aan de geselecteerde instellingen voldoen.

| Eigenschap | Description |
| --- | --- |
| Berichteigenschappen negeren |Geeft aan dat de eigenschappen van de ontvangen berichten kunnen worden genegeerd. |
| Het bericht moet ondertekend zijn |Vereist dat berichten naar digitaal worden ondertekend. Configureer het openbare certificaat van Gast partner voor controle van de handtekening.  |
| Het bericht moet worden versleuteld |Vereist berichten moeten worden versleuteld. Niet-versleutelde berichten worden geweigerd. Configureer het host-partner persoonlijke certificaat voor het ontsleutelen van de berichten.  |
| Het bericht moet worden gecomprimeerd |Vereist berichten moeten worden gecomprimeerd. Niet-gecomprimeerde berichten worden geweigerd. |
| MDN-tekst |De standaard toestand melding (MDN) moet worden verzonden naar de afzender van het bericht. |
| MDN verzenden |Vereist MDN's worden verzonden. |
| Ondertekende MDN verzenden |Vereist MDN's moeten worden ondertekend. |
| MIC-algoritme |Selecteer het algoritme moet worden gebruikt voor het ondertekenen van berichten. |
| Asynchrone MDN verzenden | Vereist berichten asynchroon worden verzonden. |
| URL | Geef de URL op waar voor het verzenden van de MDN's. |

## <a name="configure-how-your-agreement-sends-messages"></a>Configureren hoe berichten worden verzonden door uw overeenkomst

U kunt configureren hoe deze overeenkomst identificeert en uitgaande berichten die u naar uw partners via deze overeenkomst verzendt worden verwerkt.

1.  Onder **toevoegen**, selecteer **instellingen voor verzenden**.
Configureer deze eigenschappen op basis van uw overeenkomst met de partner die berichten met u worden uitgewisseld. Zie voor eigenschapbeschrijvingen, de tabel in deze sectie.

    ![Stel de eigenschappen 'Instellingen verzenden'](./media/logic-apps-enterprise-integration-as2/agreement-51.png)

2. Ondertekende om berichten te verzenden naar uw partner, selecteer **Berichtondertekening inschakelen**. Voor het ondertekenen van de berichten in de **MIC-algoritme** in de lijst met de *host partner persoonlijk certificaat MIC-algoritme*. En in de **certificaat** , selecteert u een bestaande [host partner persoonlijk certificaat](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. Selecteer voor het verzenden van versleutelde berichten naar de partner, **berichtversleuteling inschakelen**. Voor het versleutelen van de berichten de **versleutelingsalgoritme** in de lijst met de *Gast partner openbaar certificaat algoritme*.
En in de **certificaat** , selecteert u een bestaande [Gast partner openbaar certificaat](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. Selecteer voor het comprimeren van het bericht, **berichtcompressie inschakelen**.

5. Als u wilt de HTTP-header content-type uitvouwen tot één regel, selecteer **uitvouwen HTTP-headers**.

6. Selecteer voor het ontvangen van synchrone MDN's voor de verzonden berichten **MDN aanvragen**.

7. Selecteer voor het ontvangen van ondertekende MDN's voor de verzonden berichten **ondertekende MDN aanvragen**.

8. Selecteer voor het ontvangen van asynchrone MDN's voor de verzonden berichten **asynchrone MDN aanvragen**. Als u deze optie selecteert, voert u de URL voor het verzenden van de MDN's.

9. Selecteer om te vereisen dat niet-afwijzing van ontvangst, **NRR inschakelen**.  

10. Als u de algoritme indeling moet worden gebruikt in de MIC of in de uitgaande headers van de AS2-bericht of MDN-ondertekening, schakelt u **SHA2-algoritme indeling**.  

11. Als u klaar bent, zorg ervoor dat u uw instellingen opslaan door te kiezen **OK**.

De overeenkomst is nu gereed om af te handelen uitgaande berichten die aan de geselecteerde instellingen voldoen.

| Eigenschap | Description |
| --- | --- |
| Berichtondertekening inschakelen |Vereist dat alle berichten die zijn verzonden vanaf de overeenkomst moet worden ondertekend. |
| MIC-algoritme |Het algoritme moet worden gebruikt voor het ondertekenen van berichten. Hiermee configureert u de host partner persoonlijk certificaat MIC-algoritme voor het ondertekenen van berichten. |
| Certificaat |Selecteer het certificaat moet worden gebruikt voor het ondertekenen van berichten. Hiermee configureert u de host partner persoonlijk certificaat voor ondertekening van de berichten. |
| Berichtversleuteling inschakelen |Versleuteling van alle berichten die worden verzonden van deze overeenkomst vereist. Hiermee configureert u het algoritme Gast partner openbaar certificaat voor het versleutelen van berichten. |
| Versleutelingsalgoritme |Het versleutelingsalgoritme te gebruiken voor codering. Hiermee configureert u het openbare certificaat van Gast-partner voor het versleutelen van berichten. |
| Certificaat |Het certificaat te gebruiken om berichten te versleutelen. Hiermee configureert u de partner Gast persoonlijk certificaat voor het versleutelen van berichten. |
| Berichtcompressie inschakelen |Compressie van alle berichten die zijn verzonden vanaf deze overeenkomst vereist. |
| HTTP-headers uitvouwen |Hiermee plaatst u de inhoud van het type HTTP-header op één regel. |
| MDN aanvragen |Vereist een MDN voor alle berichten die van deze overeenkomst worden verzonden. |
| Ondertekende MDN aanvragen |Vereist dat alle MDN's die worden verzonden naar deze overeenkomst moet worden ondertekend. |
| Asynchrone MDN aanvragen |Asynchrone MDN's worden verzonden naar deze overeenkomst vereist. |
| URL |Geef de URL op waar voor het verzenden van de MDN's. |
| NRR inschakelen |Vereist niet-afwijzing van ontvangst (NRR), een communicatiekenmerk die aantoont dat de gegevens als opgelost is ontvangen. |
| Indeling van het SHA2-algoritme |Selecteer de indeling algoritme moet worden gebruikt in de MIC of in de uitgaande headers van de AS2-bericht of MDN-ondertekening |

## <a name="find-your-created-agreement"></a>Uw overeenkomst gevonden

1. Nadat u klaar bent met het instellen van alle uw overeenkomst-eigenschappen op de **toevoegen** pagina, kies **OK** te maken van uw overeenkomst voltooien en keer terug naar uw integratie-account.

    Nu uw zojuist toegevoegde overeenkomst wordt weergegeven in uw **overeenkomsten** lijst.

2. U kunt ook uw overeenkomsten weergeven in een overzicht van uw integratie-account. Kies uw integratie-account in het menu van **overzicht**en selecteer vervolgens de **overeenkomsten** tegel. 

   ![Kies 'Overeenkomsten' tegel als u wilt weergeven van alle overeenkomsten](./media/logic-apps-enterprise-integration-as2/agreement-6.png)

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [details swagger](/connectors/as2/). 

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack")  
