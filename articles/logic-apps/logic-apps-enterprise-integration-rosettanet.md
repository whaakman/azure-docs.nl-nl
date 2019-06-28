---
title: RosettaNet berichten voor B2B-bedrijfsintegratie - Azure Logic Apps
description: Exchange RosettaNet berichten in Azure Logic Apps met Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 88e02f3fbbca8007fdf479bb973f50c42a878d6e
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332886"
---
# <a name="exchange-rosettanet-messages-for-b2b-enterprise-integration-in-azure-logic-apps"></a>Exchange RosettaNet berichten voor B2B in enterprise integration in Azure Logic Apps 

[RosettaNet](https://resources.gs1us.org) is een non-profitorganisaties consortium dat standaard processen voor het delen van bedrijfsgegevens ingesteld. Deze standaarden worden vaak gebruikt voor toeleveringsprocessen en wijdverbreid in de industrie semiconductor electronics en logistiek zijn. Het consortium RosettaNet maakt en onderhoudt Partner Interface processen (ogen), waardoor algemene definities van bedrijfsprocessen voor alle RosettaNet berichten worden uitgewisseld. RosettaNet is gebaseerd op XML en bericht richtlijnen, interfaces voor zakelijke processen en implementatie-frameworks voor communicatie tussen bedrijven definieert.

In [Azure Logic Apps](../logic-apps/logic-apps-overview.md), de connector RosettaNet helpt u bij het maken van oplossingen voor gegevensintegratie waarmee RosettaNet standaarden ondersteunen. De connector is gebaseerd op RosettaNet implementatie Framework (RNIF) versie 2.0.01. RNIF is een open netwerk-toepassingsframework waarmee zakelijke partners om uit te voeren samen RosettaNet ogen. Dit framework definieert de berichtstructuur, de noodzaak van bevestigingen, Multipurpose Internet Mail Extensions (MIME)-codering en de digitale handtekening.

Met name biedt de connector deze mogelijkheden:

* Codeer of RosettaNet berichten ontvangen.
* Decoderen of RosettaNet berichten te verzenden.
* Wacht tot de respons en de generatie van de melding van de fout.

Voor deze mogelijkheden, biedt de connector ondersteuning voor alle ogen die zijn gedefinieerd door RNIF 2.0.01. Communicatie met de partner kan synchroon of asynchroon zijn.

## <a name="rosettanet-concepts"></a>RosettaNet concepten

Hier volgen enkele concepten en termen die uniek zijn voor de specificatie RosettaNet en zijn belangrijk bij het bouwen van integraties RosettaNet op basis van:

* **PIP**

  De organisatie RosettaNet maakt en onderhoudt Partner Interface processen (ogen), waardoor algemene definities van bedrijfsprocessen voor alle RosettaNet berichten worden uitgewisseld. Elke PIP-specificatie bevat een document type definition (DTD)-bestand en een bericht dat document. Het DTD-bestand definieert u de service-inhoud berichtstructuur. De bericht-richtlijn-document, die een leesbare HTML-bestand, Hiermee geeft u de beperkingen van de element-niveau. Deze bestanden bieden samen een volledige definitie van het bedrijfsproces.

   Ogen zijn gecategoriseerd door een functie op hoog niveau business, of een cluster, en een subfunctie of segment. Bijvoorbeeld, "3A4" is de PIP voor inkooporder, terwijl '3' is de functie Order Management en "3A" is de subfunctie offerte & invoeren. Zie voor meer informatie de [RosettaNet site](https://resources.gs1us.org).

* **Actie**

  Onderdeel van een PIP actieberichten business-berichten die worden uitgewisseld tussen partners zijn.

* **Signaal**

   Onderdeel van een PIP signaal berichten bevestigingen die zijn verzonden in antwoord op actieberichten zijn.

* **Één actie en dubbele**

  Voor een PIP één actie is alleen een antwoord een signaal bevestigingsbericht. Voor een double-action-PIP de initiator een antwoordbericht ontvangt en reageert met een bevestiging naast de stroom met één actie.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u een Azure-abonnement nog geen [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) voor het opslaan van uw overeenkomst en andere B2B-artefacten. Deze integratie-account moet worden gekoppeld aan uw Azure-abonnement.

* Ten minste twee [partners](../logic-apps/logic-apps-enterprise-integration-partners.md) die zijn gedefinieerd in uw integratie-account en geconfigureerd met de kwalificatie van de 'DUNS' onder **Bedrijfsidentiteiten**

* Een PIP-Procesconfiguratie, die is vereist voor het verzenden of ontvangen van berichten voor RosettaNet, in het integratieaccount. De Procesconfiguratie worden de kenmerken voor de configuratie van PIP opgeslagen. U kunt deze configuratie vervolgens verwijst bij het maken van een overeenkomst met de partner. Zie voor informatie over het maken van de configuratie van een PIP-proces in uw integratieaccount [toevoegen PIP Procesconfiguratie](#add-pip).

* Optionele [certificaten](../logic-apps/logic-apps-enterprise-integration-certificates.md) voor versleutelen, ontsleutelen of ondertekenen van de berichten die u naar het integratieaccount uploadt. Certificaten zijn vereist alleen als u gebruik ondertekening of versleuteling.

<a name="add-pip"></a>

## <a name="add-pip-process-configuration"></a>Configuratie van de PIP-proces toevoegen

Als u wilt een PIP-proces-configuratie toevoegen aan uw integratie-account, de volgende stappen uit:

1. In de [Azure-portal](https://portal.azure.com), zoeken en openen van uw integratie-account.

1. Op de **overzicht** venster de **RosettaNet PIP** tegel.

   ![Kies de tegel RosettaNet](media/logic-apps-enterprise-integration-rosettanet/select-rosettanet-tile.png)

1. Onder **RosettaNet PIP**, kiest u **toevoegen**. Geef de details van uw PIP.

   ![RosettaNet PIP details toevoegen](media/logic-apps-enterprise-integration-rosettanet/add-rosettanet-pip.png)

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Naam** | Ja | De naam van uw PIP |
   | **PIP-Code** | Ja | De code van PIP drie cijfers. Zie voor meer informatie, [RosettaNet ogen](https://docs.microsoft.com/biztalk/adapters-and-accelerators/accelerator-rosettanet/rosettanet-pips). |
   | **Versie van PIP** | Ja | Het versienummer PIP, die beschikbaar op basis van uw geselecteerde PIP-code is |
   ||||

   Voor meer informatie over deze eigenschappen PIP, gaat u naar de [RosettaNet website](https://resources.gs1us.org/RosettaNet-Standards/Standards-Library/PIP-Directory#1043208-pipsreg).

1. Wanneer u klaar bent, kiest u **OK**, waarmee de PIP-configuratie wordt gemaakt.

1. Als u wilt weergeven of bewerken van de Procesconfiguratie, selecteert u de PIP en kiest **bewerken as JSON**.

   Alle configuratie-instellingen zijn afkomstig van de specificaties van de PIP verwerken. Logic Apps wordt de meeste van de instellingen met de standaardwaarden die het meest gewoonlijk gebruikte waarden voor deze eigenschappen worden gevuld.

   ![RosettaNet PIP-configuratie bewerken](media/logic-apps-enterprise-integration-rosettanet/edit-rosettanet-pip.png)

1. Controleer of de instellingen overeenkomen met de waarden in de juiste PIP-specificatie en voldoen aan de behoeften van uw bedrijf. Indien nodig, werk de waarden in JSON en sla de wijzigingen.

## <a name="create-rosettanet-agreement"></a>Overeenkomst voor RosettaNet maken

1. In de [Azure-portal](https://portal.azure.com), zoeken en openen van uw integratie-account, als dit al geopend.

1. Op de **overzicht** venster de **overeenkomsten** tegel.

   ![Kies dat overeenkomsten tegel](media/logic-apps-enterprise-integration-rosettanet/select-agreement-tile.png)

1. Onder **overeenkomsten**, kiest u **toevoegen**. Geef uw gegevens van de overeenkomst.

   ![Gegevens van overeenkomst toevoegen](media/logic-apps-enterprise-integration-rosettanet/add-agreement-details.png)

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Naam** | Ja | De naam van de overeenkomst |
   | **Overeenkomsttype selecteren** | Ja | Selecteer **RosettaNet**. |
   | **Hostpartner** | Ja | Een overeenkomst is vereist voor een host en de Gast-partner. De hostpartner vertegenwoordigt de organisatie die Hiermee configureert u de overeenkomst. |
   | **Identiteit van de host** | Ja | Een id voor de hostpartner |
   | **Gastpartner** | Ja | Een overeenkomst is vereist voor een host en de Gast-partner. De gastpartner vertegenwoordigt de organisatie die wordt zakendoen met de hostpartner. |
   | **Gastidentiteit** | Ja | Een id voor de gastpartner |
   | **Ontvangstinstellingen** | Varieert | Deze eigenschappen zijn van toepassing op alle berichten ontvangen door de hostpartner |
   | **Verzendinstellingen** | Varieert | Deze eigenschappen zijn van toepassing op alle berichten die zijn verzonden door de hostpartner |  
   | **RosettaNet PIP verwijzingen** | Ja | De PIP-verwijzingen voor de overeenkomst. Alle berichten voor RosettaNet vereisen PIP configuraties. |
   ||||

1. Als u uw overeenkomst voor het ontvangen van binnenkomende berichten van de gastpartner instelt, selecteert u **instellingen ontvangen**.

   ![Ontvangstinstellingen](media/logic-apps-enterprise-integration-rosettanet/add-agreement-receive-details.png)

   1. Om in te schakelen-ondertekening of versleuteling voor inkomende berichten onder **berichten**, selecteer **bericht moet ondertekend zijn** of **bericht moet worden versleuteld** respectievelijk.

      | Eigenschap | Vereist | Description |
      |----------|----------|-------------|
      | **Bericht moet ondertekend zijn** | Nee | Meld u binnenkomende berichten met het geselecteerde certificaat. |
      | **Certificaat** | Ja, als ondertekening is ingeschakeld | Het certificaat voor ondertekening |
      | **Berichtversleuteling inschakelen** | Nee | Versleutelen van berichten met het geselecteerde certificaat. |
      | **Certificaat** | Ja, als versleuteling is ingeschakeld | Het certificaat moet worden gebruikt voor versleuteling |
      ||||

   1. Selecteer de desbetreffende onder elke selectie [certificaat](./logic-apps-enterprise-integration-certificates.md), die u eerder hebt toegevoegd aan uw integratie-account, moet worden gebruikt voor ondertekening of versleuteling.

1. Als u uw overeenkomst voor het verzenden van berichten naar de gastpartner instelt, selecteert u **instellingen voor verzenden**.

   ![Verzendinstellingen](media/logic-apps-enterprise-integration-rosettanet/add-agreement-send-details.png)

   1. Om in te schakelen-ondertekening of versleuteling voor uitgaande berichten, onder **berichten**, selecteer **Berichtondertekening inschakelen** of **berichtversleuteling inschakelen** respectievelijk. Selecteer de respectieve algoritme onder elke selectie en [certificaat](./logic-apps-enterprise-integration-certificates.md), die u eerder hebt toegevoegd aan uw integratie-account, moet worden gebruikt voor ondertekening of versleuteling.

      | Eigenschap | Vereist | Description |
      |----------|----------|-------------|
      | **Berichtondertekening inschakelen** | Nee | Meld u aan uitgaande berichten met de geselecteerde algoritme voor ondertekening en het certificaat. |
      | **Handtekeningalgoritme** | Ja, als ondertekening is ingeschakeld | Het algoritme voor ondertekening moet worden gebruikt, op basis van het geselecteerde certificaat |
      | **Certificaat** | Ja, als ondertekening is ingeschakeld | Het certificaat voor ondertekening |
      | **Berichtversleuteling inschakelen** | Nee | Versleutelen met de geselecteerde versleutelingsalgoritme en het certificaat van uitgaande. |
      | **Versleutelingsalgoritme** | Ja, als versleuteling is ingeschakeld | Het versleutelingsalgoritme te gebruiken, op basis van het geselecteerde certificaat |
      | **Certificaat** | Ja, als versleuteling is ingeschakeld | Het certificaat moet worden gebruikt voor versleuteling |
      ||||

   1. Onder **eindpunten**, geef de vereiste URL's te gebruiken voor het verzenden van actieberichten en bevestigingen.

      | Eigenschap | Vereist | Description |
      |----------|----------|-------------|
      | **Actie-URL** |  Ja | De URL moet worden gebruikt voor het verzenden van actieberichten. De URL is een verplicht veld voor synchrone en asynchrone berichten. |
      | **Acknowledgment URL** | Ja | De URL moet worden gebruikt voor het verzenden van bevestigingsberichten. De URL is een verplicht veld voor asynchrone berichten. |
      ||||

1. Als u uw overeenkomst met de RosettaNet PIP-verwijzingen voor partners instelt, selecteert u **RosettaNet PIP verwijst naar**. Onder **PIP naam**, selecteert u de naam voor uw eerder gemaakte PIP.

   ![PIP-verwijzingen](media/logic-apps-enterprise-integration-rosettanet/add-agreement-pip-details.png)

   Uw selectie vult de overige eigenschappen die zijn gebaseerd op de PIP die u hebt ingesteld in uw integratie-account. Indien nodig, kunt u de **PIP rol**.

   ![Geselecteerde PIP](media/logic-apps-enterprise-integration-rosettanet/add-agreement-selected-pip.png)

Nadat u deze stappen hebt voltooid, u kunt RosettaNet berichten verzenden of ontvangen.

## <a name="rosettanet-templates"></a>RosettaNet sjablonen

Versnel de ontwikkeling en integratiepatronen aangeraden, kunt u sjablonen voor logische Apps voor RosettaNet-berichten coderen. Wanneer u een logische app maakt, kunt u selecteren uit de galerie met sjablonen in Logic App Designer. U vindt hier ook deze sjablonen in de [GitHub-opslagplaats voor Azure Logic Apps](https://github.com/Azure/logicapps).

![RosettaNet sjablonen](media/logic-apps-enterprise-integration-rosettanet/decode-encode-rosettanet-templates.png)

## <a name="receive-or-decode-rosettanet-messages"></a>Ontvangen of RosettaNet-berichten decoderen

1. [Maak een lege logische app](quickstart-create-first-logic-app-workflow.md).

1. [Uw integratieaccount koppelen](logic-apps-enterprise-integration-create-integration-account.md#link-account) aan uw logische app.

1. Voordat u een actie voor het decoderen van het bericht RosettaNet toevoegen kunt, moet u een trigger voor het starten van uw logische app, zoals een aanvraag als trigger toevoegen.

1. Na het toevoegen van de trigger, kies **nieuwe stap**.

   ![Aanvraag-trigger toevoegen](media/logic-apps-enterprise-integration-rosettanet/request-trigger.png)

1. Voer 'rosettanet' in het zoekvak en selecteer deze actie: **RosettaNet decoderen**

   ![Zoek en selecteer de actie "RosettaNet decoderen"](media/logic-apps-enterprise-integration-rosettanet/select-decode-rosettanet-action.png)

1. Geef de informatie voor de eigenschappen van de actie:

   ![Geef details over gebeurtenisacties](media/logic-apps-enterprise-integration-rosettanet/decode-action-details.png)

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Bericht** | Ja | Het bericht RosettaNet moet worden gedecodeerd  |
   | **Headers** | Ja | De HTTP-headers die de waarden voor de versie, de versie die wordt RNIF, en het antwoordtype, geeft het communicatietype tussen de partners en kan worden synchroon of asynchroon leveren |
   | **Rol** | Ja | De rol van de hostpartner in de PIP |
   ||||

   De actie RosettaNet decoderen, de uitvoer, samen met andere eigenschappen bevat **uitgaande signaal**, die u kunt kiezen om te coderen en terugkeren naar de partner of een andere actie ondernemen op die uitvoer.

## <a name="send-or-encode-rosettanet-messages"></a>Verzenden of RosettaNet-berichten coderen

1. [Maak een lege logische app](quickstart-create-first-logic-app-workflow.md).

1. [Uw integratieaccount koppelen](logic-apps-enterprise-integration-create-integration-account.md#link-account) aan uw logische app.

1. Voordat u een actie voor het coderen van het bericht RosettaNet toevoegen kunt, moet u een trigger voor het starten van uw logische app, zoals een aanvraag als trigger toevoegen.

1. Na het toevoegen van de trigger, kies **nieuwe stap**.

   ![Aanvraag-trigger toevoegen](media/logic-apps-enterprise-integration-rosettanet/request-trigger.png)

1. Voer 'rosettanet' in het zoekvak en selecteer deze actie: **RosettaNet Encode**

   ![Zoek en selecteer de actie "RosettaNet coderen"](media/logic-apps-enterprise-integration-rosettanet/select-encode-rosettanet-action.png)

1. Geef de informatie voor de eigenschappen van de actie:

   ![Geef details over gebeurtenisacties](media/logic-apps-enterprise-integration-rosettanet/encode-action-details.png)

   | Eigenschap | Vereist | Description |
   |----------|----------|-------------|
   | **Bericht** | Ja | Het bericht RosettaNet coderen  |
   | **Hostpartner** | Ja | De hostnaam van de partner |
   | **Gastpartner** | Ja | De naam van de Gast-partner |
   | **PIP-code** | Ja | De PIP-code |
   | **Versie van PIP** | Ja | De versie van PIP |  
   | **PIP-exemplaar-id** | Ja | De unieke id voor dit bericht PIP |  
   | **Berichttype** | Ja | Het type van het bericht moet worden gecodeerd |  
   | **Rol** | Ja | De rol van de hostpartner |
   ||||

   Het gecodeerde bericht is nu gereed om te verzenden naar de partner.

1. Het gecodeerde bericht te verzenden, in dit voorbeeld wordt de **HTTP** actie, die is gewijzigd in 'HTTP - bericht verzenden gecodeerd naar partner'.

   ![HTTP-actie voor het verzenden van RosettaNet bericht](media/logic-apps-enterprise-integration-rosettanet/send-rosettanet-message-to-partner.png)

   Per RosettaNet standaarden, zakelijke transacties worden beschouwd als volledige alleen wanneer alle stappen die zijn gedefinieerd door de PIP voltooid zijn.

1. Nadat de host wordt het gecodeerde bericht naar partner verzonden, wacht de host voor het signaal en de bevestiging. Als u wilt deze taak wordt uitgevoerd, toevoegen de **RosettaNet wachten op reactie** actie.

   !["RosettaNet wachten op antwoord"-actie toevoegen](media/logic-apps-enterprise-integration-rosettanet/rosettanet-wait-for-response-action.png)

   De duur moet worden gebruikt voor wachten en het aantal nieuwe pogingen zijn gebaseerd op de PIP-configuratie in uw integratie-account. Als het antwoord wordt ontvangen, wordt door deze actie een melding van de fout genereert. Voor het afhandelen van nieuwe pogingen, plaatst u altijd de **coderen** en **wachten op reactie** acties in een **totdat** lus.

   ![Totdat-lus met RosettaNet acties](media/logic-apps-enterprise-integration-rosettanet/rosettanet-loop.png)

## <a name="next-steps"></a>Volgende stappen

* Informatie over het transformeren, valideren en andere berichtbewerkingen met de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md)
* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
