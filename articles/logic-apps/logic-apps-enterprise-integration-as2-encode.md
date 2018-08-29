---
title: Gecodeerd AS2-berichten - Azure Logic Apps | Microsoft Docs
description: Als berichten met Azure Logic Apps en Enterprise Integration Pack coderen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 332fb9e3-576c-4683-bd10-d177a0ebe9a3
ms.date: 08/08/2018
ms.openlocfilehash: 6bb19199929a004ee5668a3a6e057a69c24dd752
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122710"
---
# <a name="encode-as2-messages-with-azure-logic-apps-and-enterprise-integration-pack"></a>Met Azure Logic Apps en Enterprise Integration Pack AS2-berichten coderen

Voor het maken van beveiliging en betrouwbaarheid bij het verzenden van berichten, de coderen AS2-bericht-connector te gebruiken. Deze connector biedt digitale ondertekening, versleuteling en bevestigingen via bericht toestand meldingen (MDN), die ook leiden tot ondersteuning voor niet-afwijzing.

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een Azure-account; u kunt maken een [gratis account](https://azure.microsoft.com/free)
* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. Hebt u een integratieaccount om de coderen AS2-bericht-connector te gebruiken.
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw integratieaccount
* Een [AS2-overeenkomst](logic-apps-enterprise-integration-as2.md) die al gedefinieerd in uw integratieaccount

## <a name="encode-as2-messages"></a>AS2-berichten coderen

1. [Maak een logische app](quickstart-create-first-logic-app-workflow.md).

2. De connector coderen AS2-bericht geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag als trigger toevoegen. Een trigger toevoegen in de Logic App Designer en klikt u vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'AS2' voor uw filter. Selecteer **AS2 - codering AS2-bericht**.
   
    ![Zoek naar "AS2"](./media/logic-apps-enterprise-integration-as2-encode/as2decodeimage1.png)

4. Als u geen verbindingen voor uw integratieaccount eerder hebt gemaakt, wordt u gevraagd om nu deze verbinding te maken. Naam van de verbinding en selecteer de integratieaccount waarmee u verbinding wilt maken. 
   
    ![Maak verbinding met de integratie-account](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage1.png)  

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie-Account * |Voer een naam voor uw integratie-account. Zorg ervoor dat uw integratie-account en logica-app zich in dezelfde Azure-locatie. |

5.  Wanneer u klaar bent, moeten uw verbindingsgegevens lijken op het volgende voorbeeld. Kies voor het voltooien van het maken van uw verbinding, **maken**.
   
    ![integratie-verbindingsgegevens](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage2.png)

6. Nadat de verbinding is gemaakt, zoals wordt weergegeven in dit voorbeeld, Geef details op voor **AS2-uit**, **AS2-naar-id's** zoals geconfigureerd in de overeenkomst en **hoofdtekst**, die is de berichtnettolading van het.
   
    ![verplichte velden bevatten](./media/logic-apps-enterprise-integration-as2-encode/as2encodeimage3.png)

## <a name="as2-encoder-details"></a>Details van de AS2-codering

De connector coderen AS2 voert deze taken uit: 

* Van toepassing is AS2/HTTP-headers
* Symptomen uitgaande berichten (indien geconfigureerd)
* Uitgaande berichten versleutelt (indien geconfigureerd)
* Comprimeren van het bericht (indien geconfigureerd)
* Bestandsnaam in MIME-header verzenden (indien geconfigureerd)


  > [!NOTE]
  > Als u Azure Key Vault voor het beheren van certificaten gebruikt, zorg ervoor dat u configureert de sleutels om toe te staan de **versleutelen** bewerking.
  > Anders wordt de AS2-codering mislukken.
  >
  > ![Hiermee ontsleutelt u Key Vault](media/logic-apps-enterprise-integration-as2-encode/keyvault1.png)

## <a name="try-this-sample"></a>Probeer dit voorbeeld

Als u wilt proberen om de implementatie van een volledig operationeel logische app en voorbeeld AS2-scenario, Zie de [AS2 scenario en de sjabloon voor logische app](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [details swagger](/connectors/as2/). 

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack") 

