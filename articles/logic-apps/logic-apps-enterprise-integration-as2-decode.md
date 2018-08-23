---
title: AS2-berichten - Azure Logic Apps-decodering | Microsoft Docs
description: Over het gebruik van de AS2-decodeerfunctie in het Enterprise Integration Pack voor Azure Logic Apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: jeconnoc
editor: ''
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: LADocs; padmavc
ms.openlocfilehash: a1b2d68a4a9433dc5c68d65552bf6bd509463958
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42054440"
---
# <a name="decode-as2-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>AS2-berichten decoderen voor Azure Logic Apps met het Enterprise Integration Pack 

Voor het maken van beveiliging en betrouwbaarheid bij het verzenden van berichten, door de AS2-decodering bericht-connector te gebruiken. Deze connector biedt digitale ondertekening, ontsleuteling en bevestigingen via bericht toestand meldingen (MDN).

## <a name="before-you-start"></a>Voordat u begint

Hier volgt de items die u nodig hebt:

* Een Azure-account; u kunt maken een [gratis account](https://azure.microsoft.com/free)
* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement. Hebt u een integratieaccount om de AS2-decodering bericht-connector te gebruiken.
* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die al zijn gedefinieerd in uw integratieaccount
* Een [AS2-overeenkomst](logic-apps-enterprise-integration-as2.md) die al gedefinieerd in uw integratieaccount

## <a name="decode-as2-messages"></a>AS2-berichten decoderen

1. [Maak een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. De AS2-decodering bericht connector geen triggers, dus moet u een trigger voor het starten van uw logische app, zoals een aanvraag als trigger toevoegen. Een trigger toevoegen in de Logic App Designer en klikt u vervolgens een actie toevoegen aan uw logische app.

3.  Voer in het zoekvak 'AS2' voor uw filter. Selecteer **AS2 - decodering AS2-bericht**.
   
    ![Zoek naar "AS2"](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage1.png)

4. Als u geen verbindingen voor uw integratieaccount eerder hebt gemaakt, wordt u gevraagd om nu deze verbinding te maken. Naam van de verbinding en selecteer de integratieaccount waarmee u verbinding wilt maken.
   
    ![Integratie-verbinding maken](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage2.png)

    Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Details |
    | --- | --- |
    | Verbindingsnaam * |Voer een naam voor de verbinding. |
    | Integratie-Account * |Voer een naam voor uw integratie-account. Zorg ervoor dat uw integratie-account en logica-app zich in dezelfde Azure-locatie. |

5.  Wanneer u klaar bent, moeten uw verbindingsgegevens lijken op het volgende voorbeeld. Kies voor het voltooien van het maken van uw verbinding, **maken**.

    ![integratie-verbindingsgegevens](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage3.png)

6. Nadat de verbinding is gemaakt, zoals wordt weergegeven in dit voorbeeld, selecteer **hoofdtekst** en **Headers** uit de uitvoer van de aanvraag.
   
    ![integratie van verbinding is gemaakt](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage4.png) 

    Bijvoorbeeld:

    ![Selecteer hoofdtekst en koppen uit de uitvoer van de aanvraag](media/logic-apps-enterprise-integration-as2-decode/as2decodeimage5.png) 


## <a name="as2-decoder-details"></a>Details van de AS2-decodeerfunctie

Deze taken worden uitgevoerd door de AS2-decodering-connector: 

* AS2/HTTP-headers worden verwerkt
* Controleert of de handtekening (indien geconfigureerd)
* Hiermee ontsleutelt u de berichten (indien geconfigureerd)
* Decomprimeren van het bericht (indien geconfigureerd)
* Controleer en bericht-ID duplicaten weigeren (indien geconfigureerd)
* Voor overeenstemming zorgt met een MDN ontvangen met de oorspronkelijke uitgaand multicastverkeer voor message
* Updates en correleert records in de database niet-afwijzing
* Schrijft voor AS2-statusrapportage
* De inhoud van de uitvoer-nettolading is base64-gecodeerd
* Bepaalt of een MDN vereist is, en of de MDN synchrone moet of asynchrone op basis van configuratie in AS2-overeenkomst
* Genereert een synchrone of asynchrone MDN (op basis van overeenkomst configuraties)
* Hiermee stelt u de correlatietokens en eigenschappen van de MDN


  > [!NOTE]
  > Als u Azure Key Vault voor het beheren van certificaten gebruikt, zorg ervoor dat u configureert de sleutels om toe te staan de **ontsleutelen** bewerking.
  > Anders wordt de AS2-decodering mislukken.
  >
  > ![Hiermee ontsleutelt u Key Vault](media/logic-apps-enterprise-integration-as2-decode/keyvault1.png)

## <a name="try-this-sample"></a>Probeer dit voorbeeld

Als u wilt proberen om de implementatie van een volledig operationeel logische app en voorbeeld AS2-scenario, Zie de [AS2 scenario en de sjabloon voor logische app](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/).

## <a name="view-the-swagger"></a>De swagger weergeven
Zie de [details swagger](/connectors/as2/). 

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md) 

