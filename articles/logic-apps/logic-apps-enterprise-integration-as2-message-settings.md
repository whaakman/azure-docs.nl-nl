---
title: Instellingen voor AS2-bericht - Azure Logic Apps
description: Naslaggids voor AS2 verzenden en ontvangen van de instellingen in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 04/22/2019
ms.openlocfilehash: ead92094b9af1dff56ff68e1ff58a3a4cdd9dca5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "63769449"
---
# <a name="reference-for-as2-message-settings-in-azure-logic-apps-with-enterprise-integration-pack"></a>Naslaginformatie voor instellingen van de AS2-bericht in Azure Logic Apps met Enterprise Integration Pack

Deze referentie bevat de eigenschappen die u instellen kunt voor het op te geven hoe een AS2-overeenkomst worden verzonden en ontvangen tussen handelspartners berichten verwerkt. Instellen van deze eigenschappen op basis van uw overeenkomst met de partner die berichten met u worden uitgewisseld.

<a name="AS2-incoming-messages"></a>

## <a name="as2-receive-settings"></a>Instellingen voor AS2 ontvangen

![Selecteer 'Ontvangen instellingen'](./media/logic-apps-enterprise-integration-as2-message-settings/receive-settings.png)

| Eigenschap | Vereist | Description |
|----------|----------|-------------|
| **Berichteigenschappen negeren** | Nee | Onderdrukt de eigenschappen op inkomende berichten met de eigenschapsinstellingen van de. |
| **Bericht moet ondertekend zijn** | Nee | Hiermee geeft u op of alle binnenkomende berichten moeten digitaal worden ondertekend. Als u nodig u zich aanmeldt hebt, uit de **certificaat** , selecteert u een bestaand Gast partner openbare certificaat voor het valideren van de handtekening van de berichten. Als u niet een certificaat hebt, meer informatie over [certificaten toe te voegen](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **Bericht moet worden versleuteld** | Nee | Hiermee geeft u op of alle inkomende berichten moeten worden versleuteld. Niet-versleutelde berichten worden geweigerd. Als u versleuteling, van vereisen de **certificaat** , selecteert u een bestaande host partner persoonlijk certificaat voor het ontsleutelen van binnenkomende berichten. Als u niet een certificaat hebt, meer informatie over [certificaten toe te voegen](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **Bericht moet worden gecomprimeerd** | Nee | Hiermee geeft u op of alle inkomende berichten moeten worden gecomprimeerd. Niet-gecomprimeerde berichten worden geweigerd. |
| **Dubbele bericht-ID niet toestaan** | Nee | Hiermee geeft u op of u wilt toestaan dat berichten met dubbele id. Als u dubbele id's niet toestaan, selecteert u het aantal dagen tussen controles. U kunt ook kiezen of u wilt onderbreken van duplicaten. |
| **MDN-tekst** | Nee | Hiermee geeft u de standaard toestand melding (MDN) die u wilt dat naar de afzender van het bericht verzonden. |
| **MDN verzenden** | Nee | Geeft aan of synchrone MDN's voor de ontvangen berichten te verzenden.  |
| **Ondertekende MDN verzenden** | Nee | Hiermee bepaalt u of voor het verzenden van ondertekende MDN's voor de ontvangen berichten. Als u nodig u zich aanmeldt hebt, uit de **MIC-algoritme** , selecteert u het algoritme moet worden gebruikt voor het ondertekenen van berichten. |
| **Asynchrone MDN verzenden** | Nee | Geeft aan of MDN's asynchroon verzenden. Als u asynchrone MDN's, selecteert u in de **URL** vak, geef de URL voor het verzenden van de MDN's. |
||||

<a name="AS2-outgoing-messages"></a>

## <a name="as2-send-settings"></a>Instellingen voor AS2-verzenden

![Selecteer "Verzenden"](./media/logic-apps-enterprise-integration-as2-message-settings/send-settings.png)

| Eigenschap | Vereist | Description |
|----------|----------|-------------|
| **Berichtondertekening inschakelen** | Nee | Hiermee geeft u op of alle uitgaande berichten moeten digitaal worden ondertekend. Als u de ondertekening vereisen, selecteert u deze waarden: <p>-Van de **algoritme voor ondertekening** , selecteert u het algoritme moet worden gebruikt voor het ondertekenen van berichten. <br>-Van de **certificaat** , selecteert u een bestaande host partner persoonlijk certificaat voor het ondertekenen van berichten. Als u niet een certificaat hebt, meer informatie over [certificaten toe te voegen](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **Berichtversleuteling inschakelen** | Nee | Hiermee geeft u op of alle uitgaande berichten moeten worden versleuteld. Als u versleuteling vereist, selecteert u deze waarden: <p>-Van de **versleutelingsalgoritme** , selecteert u het algoritme Gast partner openbaar certificaat te gebruiken voor het versleutelen van berichten. <br>-Van de **certificaat** , selecteert u een bestaand Gast partner persoonlijke certificaat voor het versleutelen van berichten. Als u niet een certificaat hebt, meer informatie over [certificaten toe te voegen](../logic-apps/logic-apps-enterprise-integration-certificates.md). |
| **Berichtcompressie inschakelen** | Nee | Hiermee geeft u op of alle uitgaande berichten moeten worden gecomprimeerd. |
| **HTTP-headers uitvouwen** | Nee | Hebt u de HTTP `content-type` header op één regel. |
| **Bestandsnaam in MIME-header verzenden** | Nee | Hiermee geeft u op of de bestandsnaam in de MIME-header worden opgenomen. |
| **MDN aanvragen** | Nee | Hiermee geeft u op of u meldingen wilt ontvangen bericht toestand (MDN's) voor alle uitgaande berichten. |
| **Ondertekende MDN aanvragen** | Nee | Hiermee bepaalt u of voor het ontvangen van ondertekende MDN's voor alle uitgaande berichten. Als u nodig u zich aanmeldt hebt, uit de **MIC-algoritme** , selecteert u het algoritme moet worden gebruikt voor het ondertekenen van berichten. |
| **Asynchrone MDN aanvragen** | Nee | Geeft aan of MDN's asynchroon ontvangen. Als u asynchrone MDN's, selecteert u in de **URL** vak, geef de URL voor het verzenden van de MDN's. |
| **Enable NRR** | Nee | Hiermee geeft u op of niet-afwijzing ontvangst (NRR). Dit communicatiekenmerk aantoont dat de gegevens als opgelost is ontvangen. |
| **De indeling SHA2-algoritme** | Nee | Hiermee geeft u de indeling van de MIC-algoritme voor ondertekening in de headers voor de uitgaande AS2-berichten of MDN |
||||

## <a name="next-steps"></a>Volgende stappen

[AS2-berichten uitwisselen](../logic-apps/logic-apps-enterprise-integration-as2.md)
