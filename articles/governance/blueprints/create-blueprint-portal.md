---
title: Een blauwdruk maken in de portal
description: Gebruik Azure blauwdrukken maken, definiëren en implementeren van de artefacten tot en met de Azure-portal.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/11/2019
ms.topic: quickstart
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 28fef394ee400949f9911983bdbca41d6bfcb458
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60683380"
---
# <a name="define-and-assign-a-blueprint-in-the-portal"></a>Definiëren en toewijzen van een blauwdruk in de portal

Als u meer informatie over het maken en toewijzen van blauwdrukken, kunt u algemene patronen voor het ontwikkelen van herbruikbare en Snel implementeerbare configuraties op basis van Azure Resource Manager-sjablonen, beleid, beveiliging en meer kunt definiëren. In deze zelfstudie leert u blauwdrukken voor Azure enkele van de algemene taken met betrekking tot het maken, publiceren en toewijzen van een blauwdruk binnen uw organisatie gebruiken. Deze taken omvatten:

> [!div class="checklist"]
> - Een nieuwe blauwdruk maken en verschillende ondersteunde artefacten toevoegen
> - Wijzigingen aanbrengen aan een bestaande blauwdruk die nog een **Concept** is
> - Blauwdrukken markeren als gereed om toe te wijzen met **Gepubliceerd**
> - Een blauwdruk toewijzen aan een bestaand abonnement
> - De status en voortgang van een toegewezen blauwdruk controleren
> - Een blauwdruk verwijderen die aan een abonnement is toegewezen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="create-a-blueprint"></a>Een blauwdruk maken

De eerste stap bij het definiëren van een standaardpatroon voor naleving bestaat uit het samenstellen van een blauwdruk uit de beschikbare resources. In dit voorbeeld maakt u een nieuwe blauwdruk met de naam **MyBlueprint** toewijzingen voor rollen en beleid voor het abonnement te configureren. Vervolgens voegen een nieuwe resourcegroep en de toewijzing van een Resource Manager-sjabloon en de rol op de nieuwe resourcegroep maken.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer **blauwdruk definities** op de pagina aan de linkerkant en selecteer de **+ maken blauwdruk** knop aan de bovenkant van de pagina.

   Of selecteer **maken** uit de **aan de slag** pagina waarmee u rechtstreeks naar een blauwdruk maken.

   ![Een blauwdruk maken vanaf de pagina van de definities van blauwdruk](./media/create-blueprint-portal/create-blueprint-button.png)

1. Geef een **blauwdruknaam** zoals **MyBlueprint**. (Gebruik maximaal 48 letters en cijfers, maar er zijn geen spaties of speciale tekens). Laat **beschrijving van blauwdruk** voorlopig leeg.

1. In de **definitielocatie** vak, selecteer het weglatingsteken aan de rechterkant, selecteer de [beheergroep](../management-groups/overview.md) of abonnement waarvoor u wilt opslaan van de blauwdruk en kies **Selecteer**.

1. Controleer of de gegevens juist zijn. De **blauwdruknaam** en **definitielocatie** velden kunnen later worden gewijzigd. Selecteer vervolgens **volgende: artefacten** onderaan de pagina of op het tabblad **Artefacten** bovenaan de pagina.

1. Een roltoewijzing toevoegen op het abonnementsniveau van:

   1. Selecteer de **+ toevoegen artefact** rij onder **abonnement**. De **toevoegen artefact** venster wordt geopend aan de rechterkant van de browser.

   1. Selecteer **roltoewijzing** voor **type artefact**.

   1. Onder **rol**, selecteer **Inzender**. Laat de **gebruiker, app of groep toevoegen** vak met het selectievakje waarmee een dynamische parameter.

   1. Selecteer **toevoegen** dit artefact toevoegen aan de blauwdruk.

   ![Roltoewijzing voor een artefact blauwdruk](./media/create-blueprint-portal/add-role-assignment.png)

   > [!NOTE]
   > De meeste artefacten ondersteuning voor parameters. Een parameter die een waarde is toegewezen tijdens het maken van de blauwdruk is een *statische parameter*. Als de parameter is toegewezen tijdens de blauwdruktoewijzing, is het een *dynamische parameter*. Zie [Blauwdrukparameters](./concepts/parameters.md) voor meer informatie.

1. Een beleidstoewijzing toevoegen op het abonnementsniveau van:

   1. Selecteer de **+ toevoegen artefact** rij onder de roltoewijzingsartefact.

   1. Selecteer **beleidstoewijzing** voor **type artefact**.

   1. Wijziging **Type** naar **ingebouwde**. In **zoeken**, voer **tag**.

   1. Klik buiten het veld **Zoeken** om de filtering weer te geven. Selecteer **tag en bijbehorende standaardwaarde toepassen op resourcegroepen**.

   1. Selecteer **toevoegen** dit artefact toevoegen aan de blauwdruk.

1. Selecteer de rij van de beleidstoewijzing **tag en bijbehorende standaardwaarde toepassen op resourcegroepen**.

1. Het venster voor de parameters voor het artefact als onderdeel van de blauwdrukdefinitie wordt geopend en kunt instellen dat de parameters voor alle toewijzingen (statische parameters) op basis van deze blauwdruk in plaats van tijdens de toewijzing (dynamische parameters). In dit voorbeeld maakt gebruik van dynamische parameters tijdens de blauwdruktoewijzing, dus laat de standaardwaarden en selecteer **annuleren**.

1. Voeg een resourcegroep toe op het abonnementsniveau van:

   1. Selecteer de **+ toevoegen artefact** rij onder **abonnement**.

   1. Selecteer **resourcegroep** voor **type artefact**.

   1. Laat de **artefact weergavenaam**, **groepsnaam voor Accountresources**, en **locatie** selectievakjes leeg, maar zorg ervoor dat het selectievakje is ingeschakeld voor elke eigenschap parameter om ze te maken dynamische parameters.

   1. Selecteer **toevoegen** dit artefact toevoegen aan de blauwdruk.

1. Een sjabloon onder de resourcegroep toevoegen:

   1. Selecteer de **+ toevoegen artefact** rij onder de **ResourceGroup** vermelding.

   1. Selecteer **Azure Resource Manager-sjabloon** voor **type artefact**, stel **artefact weergavenaam** naar **StorageAccount**, en laat u  **Beschrijving** leeg.

   1. Op het tabblad **Sjabloon** van het editorvak plakt u de volgende Resource Manager-sjabloon.
      Nadat u de sjabloon plakken, selecteert u de **Parameters** tabblad en merk op dat de sjabloonparameters **Opslagaccounttype** en **locatie** zijn gedetecteerd. Elke parameter is automatisch gedetecteerd en gevuld, maar geconfigureerd als een dynamische parameter.

      > [!IMPORTANT]
      > Als u de sjabloon importeren wilt, zorgt u ervoor dat het bestand alleen JSON is en bevat geen HTML-code. Zorg ervoor dat u hebt geselecteerd wanneer u die naar een URL op GitHub verwijst bent, **RAW** om op te halen van het pure JSON-bestand en niet de HTML ingepakt om weer te geven op GitHub. Er treedt een fout op als de geïmporteerde sjabloon geen zuivere JSON is.

      ```json
      {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
              "storageAccountType": {
                  "type": "string",
                  "defaultValue": "Standard_LRS",
                  "allowedValues": [
                      "Standard_LRS",
                      "Standard_GRS",
                      "Standard_ZRS",
                      "Premium_LRS"
                  ],
                  "metadata": {
                      "description": "Storage Account type"
                  }
              },
              "location": {
                  "type": "string",
                  "defaultValue": "[resourceGroup().location]",
                  "metadata": {
                      "description": "Location for all resources."
                  }
              }
          },
          "variables": {
              "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
          },
          "resources": [{
              "type": "Microsoft.Storage/storageAccounts",
              "name": "[variables('storageAccountName')]",
              "location": "[parameters('location')]",
              "apiVersion": "2018-07-01",
              "sku": {
                  "name": "[parameters('storageAccountType')]"
              },
              "kind": "StorageV2",
              "properties": {}
          }],
          "outputs": {
              "storageAccountName": {
                  "type": "string",
                  "value": "[variables('storageAccountName')]"
              }
          }
      }
      ```

   1. Schakel de **Opslagaccounttype** selectievakje en houd er rekening mee dat de vervolgkeuzelijst bevat alleen de waarden die zijn opgenomen in het Resource Manager-sjabloon onder **allowedValues**. Selecteer het vak waarde weer instellen op een dynamische parameter.

   1. Selecteer **toevoegen** dit artefact toevoegen aan de blauwdruk.

   ![Resource Manager-sjabloon voor het artefact blauwdruk](./media/create-blueprint-portal/add-resource-manager-template.png)

1. De voltooide blauwdruk moet er nu ongeveer als volgt uitzien. U ziet dat elke artefact heeft  **_x_ van _y_ parameters ingevuld** in de **Parameters** kolom. De dynamische parameters zijn ingesteld tijdens de toewijzing van de blauwdruk.

   ![De blauwdrukdefinitie is voltooid](./media/create-blueprint-portal/completed-blueprint.png)

1. Nu dat alle geplande artefacten zijn toegevoegd, selecteert u **concept opslaan** aan de onderkant van de pagina.

## <a name="edit-a-blueprint"></a>Een blauwdruk bewerken

In [een blauwdruk maken](#create-a-blueprint), u niet hebt een beschrijving opgeven of de roltoewijzing toevoegen aan de nieuwe resourcegroep. U kunt dit oplossen door de volgende stappen:

1. Selecteer **Blauwdrukdefinities** op de pagina aan de linkerkant.

1. In de lijst met blauwdrukken, met de rechtermuisknop op de optie die u eerder hebt gemaakt en selecteer **bewerken blauwdruk**.

1. Geef in **Beschrijving van blauwdruk** informatie op over de blauwdruk en de artefacten waaruit deze bestaat. Voer in dit geval iets als het volgende in: **Deze blauwdruk ingesteld beleid en de rol van de tag niet toewijzen voor het abonnement, een ResourceGroup maakt en implementeert de toewijzing van een resource-sjabloon en de rol in die resourcegroep.**

1. Selecteer **Volgende : artefacten** onderaan de pagina of op het tabblad **Artefacten** bovenaan de pagina.

1. Een roltoewijzing onder de resourcegroep toevoegen:

   1. Selecteer de **+ toevoegen artefact** rij direct onder de **ResourceGroup** vermelding.

   1. Selecteer **roltoewijzing** voor **type artefact**.

   1. Onder **rol**, selecteer **eigenaar**, en schakel het selectievakje onder de **gebruiker, app of groep toevoegen** vak.

   1. Zoek en selecteer een gebruiker, app of groep toe te voegen. Dit artefact maakt gebruik van een statische parameterset hetzelfde in elke toewijzing van deze blauwdruk.

   e. Selecteer **toevoegen** dit artefact toevoegen aan de blauwdruk.

   ![Tweede roltoewijzing voor de blauwdruk artefact](./media/create-blueprint-portal/add-role-assignment-2.png)

1. De voltooide blauwdruk moet er nu ongeveer als volgt uitzien. U ziet dat de zojuist toegevoegde roltoewijzing toont **1 van 1-parameters ingevuld**. Dit betekent dat het een statische parameter is.

   ![Tweede definitie voor de voltooide blauwdruk](./media/create-blueprint-portal/completed-blueprint-2.png)

1. Selecteer **concept opslaan** nu dat deze is bijgewerkt.

## <a name="publish-a-blueprint"></a>Een blauwdruk publiceren

Nu alle geplande artefacten aan de blauwdruk zijn toegevoegd, kunt u deze gaan publiceren.
Publiceren kunt u de blauwdruk beschikbaar moet worden toegewezen aan een abonnement.

1. Selecteer **Blauwdrukdefinities** op de pagina aan de linkerkant.

1. In de lijst met blauwdrukken, met de rechtermuisknop op het account dat u eerder hebt gemaakt en selecteer **blauwdruk publiceren**.

1. Geef in het deelvenster dat wordt geopend, een **versie** (letters, cijfers en afbreekstreepjes bevatten met een maximale lengte van 20 tekens), zoals **v1**. (Optioneel) Voer tekst in **notities wijzigen**, zoals **eerst publiceren**.

1. Selecteer **publiceren** aan de onderkant van de pagina.

## <a name="assign-a-blueprint"></a>Een blauwdruk toewijzen

Nadat een blauwdruk is gepubliceerd, kan deze worden toegewezen aan een abonnement. De blauwdruk die u hebt gemaakt aan een van de abonnementen onder uw beheerhiërarchie toewijzen. Als de blauwdruk is opgeslagen in een abonnement, kan deze alleen aan dat abonnement worden toegewezen.

1. Selecteer **Blauwdrukdefinities** op de pagina aan de linkerkant.

1. In de lijst met blauwdrukken, met de rechtermuisknop op de optie die u eerder hebt gemaakt (of Selecteer het weglatingsteken) en selecteer **blauwdruk toewijzen**.

1. Op de **blauwdruk toewijzen** pagina, in de **abonnement** vervolgkeuzelijst, selecteer de abonnementen die u wilt deze blauwdruk te implementeren.

   Als er ondersteunde Enterprise-aanbiedingen beschikbaar zijn via [Azure Billing](../../billing/index.md), wordt de koppeling **Nieuw** onder het vak **Abonnement** geactiveerd. Volg deze stappen:

   1. Klik op de koppeling **Nieuw** om een nieuw abonnement te maken in plaats van een bestaand abonnement te selecteren.

   1. Geef een **weergavenaam** voor het nieuwe abonnement op.

   1. Selecteer de beschikbare **bieden** uit de vervolgkeuzelijst.

   1. Gebruik het beletselteken om te selecteren de [beheergroep](../management-groups/index.md) er is een onderliggend element van het abonnement.

   1. Selecteer **Maken** onderaan de pagina.

   ![Een abonnement voor een abonnement van de toewijzing van blauwdruk maken](./media/create-blueprint-portal/assignment-create-subscription.png)

   > [!IMPORTANT]
   > Het nieuwe abonnement is gemaakt, onmiddellijk nadat u hebt geselecteerd **maken**.

   > [!NOTE]
   > Er wordt een toewijzing gemaakt voor elk abonnement dat u selecteert. U kunt wijzigingen aanbrengen aan de toewijzing van een enkel abonnement op een later tijdstip zonder wijzigingen in de rest van de geselecteerde abonnementen.

1. Voor **opdrachtnaam**, Geef een unieke naam voor deze toewijzing.

1. In **locatie**, selecteer een regio voor het beheerde identiteits- en abonnement implementatieobject moeten worden gemaakt. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.

1. Laat de **blauwdruk definitieversies** vervolgkeuzelijst selectie van **gepubliceerd** versies op de **v1** vermelding. (De standaardwaarde is de meest recent gepubliceerde versie.)

1. Laat **Toewijzing vergrendelen** staan op de standaardwaarde **Niet vergrendelen**. Zie voor meer informatie, [blauwdrukken resource vergrendelen](./concepts/resource-locking.md).

   ![Vergrendelen en beheerde identiteiten voor de toewijzing](./media/create-blueprint-portal/assignment-locking-mi.png)

1. Onder **beheerde identiteit**, laat de standaardwaarde **systeem toegewezen**.

1. Voor roltoewijzing op abonnementsniveau **[naam gebruikersgroep of toepassing]: inzender** zoekt u naar en selecteert u een gebruiker, app of groep.

1. Voor de toewijzing van het beleid op abonnementsniveau abonnement instellen **tagnaam** naar **CostCenter** en de **tagwaarde** naar **ContosoIT**.

1. Voor **ResourceGroup**, bieden een **naam** van **StorageAccount** en een **locatie** van **VS-Oost 2** uit de vervolgkeuzelijst.

   > [!NOTE]
   > Voor elke artefact die u tijdens de blauwdrukdefinitie onder de resourcegroep hebt toegevoegd, wordt dit artefact zodat deze overeenkomt met de resourcegroep of het object dat u met implementeert ingesprongen weergegeven.
   > Artefacten die niet worden parameters of geen parameters worden gedefinieerd op toewijzing hebben worden alleen voor contextuele informatie weergegeven.

1. Op de Azure Resource Manager-sjabloon **StorageAccount**, selecteer **Standard_GRS** voor de **Opslagaccounttype** parameter.

1. Lees de informatievak aan de onderkant van de pagina en selecteer vervolgens **toewijzen**.

## <a name="track-deployment-of-a-blueprint"></a>Implementatie van een blauwdruk bijhouden

Wanneer een blauwdruk is toegewezen aan een of meer abonnementen, gebeuren er twee dingen:

- De blauwdruk wordt toegevoegd aan de **toegewezen blauwdrukken** pagina voor elk abonnement.
- Het proces van de implementatie van de artefacten die zijn gedefinieerd door de blauwdruk begint.

Nu dat de blauwdruk is toegewezen aan een abonnement, Controleer de voortgang van de implementatie:

1. Selecteer **Toegewezen blauwdrukken** op de pagina aan de linkerkant.

1. In de lijst met blauwdrukken, met de rechtermuisknop op de map die u eerder hebt toegewezen en selecteer **Toewijzingsdetails weergeven**.

   ![Toewijzingsdetails weergeven op de pagina van toegewezen blauwdrukken](./media/create-blueprint-portal/view-assignment-details.png)

1. Op de **toewijzing van blauwdruk** pagina, valideert u dat alle artefacten zijn geïmplementeerd en dat er geen fouten zijn opgetreden tijdens de implementatie. Als er fouten zijn opgetreden, Zie [probleemoplossing blauwdrukken](./troubleshoot/general.md) voor stappen om te bepalen wat er mis ging.

## <a name="unassign-a-blueprint"></a>De toewijzing van een blauwdruk ongedaan maken

Als u een blauwdruktoewijzing van de niet meer nodig hebt, het verwijderen van een abonnement. De blauwdruk is mogelijk vervangen door een nieuwere blauwdruk met bijgewerkte patronen, beleidsregels en mogelijk te maken. Wanneer een blauwdruk wordt verwijderd, blijven de artefacten die als onderdeel van die blauwdruk zijn toegewezen, achter. Voer de volgende stappen uit als u een blauwdruktoewijzing wilt verwijderen:

1. Selecteer **Toegewezen blauwdrukken** op de pagina aan de linkerkant.

1. Selecteer in de lijst met blauwdrukken, de blauwdruk die u wilt intrekken. Selecteer vervolgens de **toewijzing verwijderen blauwdruk** knop aan de bovenkant van de pagina.

1. Lees het bevestigingsbericht en selecteer vervolgens **OK**.

## <a name="delete-a-blueprint"></a>Een blauwdruk verwijderen

1. Selecteer **Blauwdrukdefinities** op de pagina aan de linkerkant.

1. Met de rechtermuisknop op de blauwdruk die u wilt verwijderen en selecteer **verwijderen blauwdruk**. Selecteer vervolgens **Ja** in het bevestigingsvenster.

> [!NOTE]
> Een blauwdruk bij deze methode verwijderen, verwijdert u ook alle gepubliceerde versies van de geselecteerde blauwdruk.
> Als u wilt verwijderen van een enkele versie, de blauwdruk openen, selecteert u de **gepubliceerde versies** tabblad, selecteert u de versie die u wilt verwijderen en selecteer vervolgens **deze versie verwijderen**. U kunt ook een blauwdruk niet verwijderen totdat u alle blauwdruktoewijzing van deze blauwdrukdefinitie hebt verwijderd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [blauwdruk lifecycle](./concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](./concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](./concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](./concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](./how-to/update-existing-assignments.md).
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](./troubleshoot/general.md).