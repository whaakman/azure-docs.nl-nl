---
title: 'Zelfstudie: nieuwe resources beveiligen met resourcevergrendelingen voor blauwdruk'
description: In deze zelfstudie leert u de Azure-blauwdrukken resource wordt vergrendeld om opties te gebruiken is alleen-lezen en verwijder niet beveiligen zojuist geïmplementeerde resources.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/28/2019
ms.topic: tutorial
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 274c437acd8df50d631727fc352c4b9ebecead18
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479973"
---
# <a name="tutorial-protect-new-resources-with-azure-blueprints-resource-locks"></a>Zelfstudie: Nieuwe resources beveiligen met blauwdrukken voor Azure-resourcevergrendelingen

Met Azure blauwdrukken [resourcevergrendelingen](../concepts/resource-locking.md), kunt u zojuist geïmplementeerde resources beveiligen tegen het worden gemanipuleerd, zelfs niet door een account met de _eigenaar_ rol. U kunt deze bescherming toevoegen in de blauwdrukdefinities van resources die zijn gemaakt door een artefact Resource Manager-sjabloon.

In deze zelfstudie hebt u de volgende stappen uitvoeren:

> [!div class="checklist"]
> - De blauwdrukdefinitie van een maken
> - Markeren als de blauwdrukdefinitie van de **gepubliceerd**
> - De blauwdrukdefinitie van de toewijzen aan een bestaand abonnement
> - Inspecteer de nieuwe resourcegroep
> - Toewijzing van de blauwdruk om de vergrendelingen te verwijderen

## <a name="prerequisites"></a>Vereisten

U hebt een Azure-abonnement nodig om deze zelfstudie te voltooien. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="create-a-blueprint-definition"></a>De blauwdrukdefinitie van een maken

Maak eerst de blauwdrukdefinitie van de.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Op de **aan de slag** pagina aan de linkerkant, selecteer **maken** onder **een blauwdruk maken**.

1. Zoek de **lege blauwdruk** blauwdruk voorbeeld aan de bovenkant van de pagina. Selecteer **beginnen met een lege blauwdruk**.

1. Deze informatie kunt invoeren op de **basisbeginselen** tabblad:

   - **De blauwdruknaam van de**: Geef een naam voor uw exemplaar van de blauwdruk-voorbeeld. Voor deze zelfstudie gebruiken we de naam **vergrendeld storageaccount**.
   - **Beschrijving van blauwdruk**: Een beschrijving voor de blauwdrukdefinitie van de toevoegen. Gebruik **geïmplementeerde resources voor het testen blauwdruk resource vergrendelen op**.
   - **Definitielocatie**: Selecteer de knop met het weglatingsteken (...) en selecteer vervolgens de beheergroep of het abonnement op te slaan van de blauwdrukdefinitie van de aan.

1. Selecteer de **artefacten** tabblad aan de bovenkant van de pagina, of selecteer **volgende: Artefacten** aan de onderkant van de pagina.

1. Voeg een resourcegroep toe op het abonnementsniveau van:
   1. Selecteer de **toevoegen artefact** rij onder **abonnement**.
   1. Selecteer **resourcegroep** onder **type artefact**.
   1. Stel de **artefact weergavenaam** naar **RGtoLock**.
   1. Laat de **groepsnaam voor Accountresources** en **locatie** selectievakjes leeg, maar zorg ervoor dat het selectievakje is ingeschakeld voor elke eigenschap zodat ze **dynamische parameters**.
   1. Selecteer **toevoegen** het artefact toevoegen aan de blauwdruk.

1. Een sjabloon onder de resourcegroep toevoegen:
   1. Selecteer de **toevoegen artefact** rij onder de **RGtoLock** vermelding. 
   1. Selecteer **Azure Resource Manager-sjabloon** onder **type artefact**, stel **artefact weergavenaam** naar **StorageAccount**, en laat u  **Beschrijving** leeg. 
   1. Op de **sjabloon** tabblad, plak de volgende Resource Manager-sjabloon in het vak-editor. Nadat u hebt geplakt in de sjabloon, selecteert u **toevoegen** het artefact toevoegen aan de blauwdruk.

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
           }
       },
       "variables": {
           "storageAccountName": "[concat('store', uniquestring(resourceGroup().id))]"
       },
       "resources": [{
           "type": "Microsoft.Storage/storageAccounts",
           "name": "[variables('storageAccountName')]",
           "location": "[resourceGroup().location]",
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

1. Selecteer **concept opslaan** aan de onderkant van de pagina.

Deze stap maakt u de blauwdrukdefinitie van de in de geselecteerde beheergroep of het abonnement.

Na de **opslaan blauwdrukdefinitie is** portalmelding wordt weergegeven, gaat u naar de volgende stap.

## <a name="publish-the-blueprint-definition"></a>De blauwdrukdefinitie publiceren

De blauwdrukdefinitie is gemaakt in uw omgeving. Deze gemaakt **Draft** modus en moet worden gepubliceerd voordat deze kunnen worden toegewezen en geïmplementeerd.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om te zoeken de **vergrendeld storageaccount** blauwdruk definitie en selecteer dit.

1. Selecteer **blauwdruk publiceren** aan de bovenkant van de pagina. Voer in het nieuwe deelvenster aan de rechterkant **1.0** als de **versie**. Deze eigenschap is handig als u later een wijziging aanbrengt. Voer **notities wijzigen**, zoals **eerste versie gepubliceerd voor het vergrendelen van blauwdruk geïmplementeerd resources**. Selecteer vervolgens **publiceren** aan de onderkant van de pagina.

Deze stap maakt het mogelijk is de blauwdruk toewijzen aan een abonnement. Nadat de blauwdrukdefinitie is gepubliceerd, kunt u nog steeds wijzigingen aanbrengen. Als u wijzigingen aanbrengt, moet u de definitie met een nieuwe versiewaarde voor het bijhouden van de verschillen tussen versies van dezelfde blauwdrukdefinitie voor publiceren.

Na de **publiceren is voltooid de definitie van blauwdruk** portalmelding wordt weergegeven, gaat u naar de volgende stap.

## <a name="assign-the-blueprint-definition"></a>De blauwdrukdefinitie van de toewijzen

Nadat de blauwdrukdefinitie is gepubliceerd, kunt u deze toewijzen aan een abonnement in de beheergroep waar deze zijn opgeslagen. In deze stap maakt opgeven u parameters zodat elke implementatie van de blauwdrukdefinitie unieke.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om te zoeken de **vergrendeld storageaccount** blauwdruk definitie en selecteer dit.

1. Selecteer **blauwdruk toewijzen** aan de bovenkant van de pagina van de blauwdruk-definitie.

1. Geef de parameterwaarden voor de blauwdruktoewijzing:

   - **Basisinstellingen**

     - **Abonnementen**: Selecteer een of meer van de abonnementen die zich in de beheergroep waar u de blauwdrukdefinitie van de hebt opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing voor elk abonnement, met behulp van de parameters die u invoert worden gemaakt.
     - **Naam van de roltoewijzing**: De naam is ingevuld op basis van de naam van de blauwdrukdefinitie. We willen dat deze toewijzing te vertegenwoordigen vergrendelen van de nieuwe resourcegroep, wijzig de naam van de toewijzing aan **toewijzing-vergrendeld-storageaccount-TestingBPLocks**.
     - **Locatie**: Selecteer een regio waarin u wilt maken van de beheerde identiteit. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
       Selecteer voor deze zelfstudie **VS-Oost 2**.
     - **De versie van blauwdruk**: Selecteer de gepubliceerde versie **1.0** van de blauwdrukdefinitie.

   - **Lock Assignment**

     Selecteer de **alleen-lezen** blauwdruk lock-modus. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../concepts/resource-locking.md).

   - **Beheerde identiteit**

     De standaardoptie gebruiken: **Systeem toegewezen**. Zie voor meer informatie, [beheerde identiteiten](../../../active-directory/managed-identities-azure-resources/overview.md).

   - **Artefact parameters**

     De gedefinieerde parameters in deze sectie zijn van toepassing op het artefact waarin ze zijn gedefinieerd. Deze parameters zijn [dynamische parameters](../concepts/parameters.md#dynamic-parameters) omdat ze tijdens de toewijzing van de blauwdruk zijn gedefinieerd. Voor elke artefact, stelt u de parameterwaarde op wat u ziet in de **waarde** kolom.

     |Naam van het assemblyartefact|Artefacttype|Parameternaam|Value|Description|
     |-|-|-|-|-|
     |Resourcegroep RGtoLock|Resourcegroep|Name|TestingBPLocks|Bepaalt de naam van de nieuwe resourcegroep om toe te passen van blauwdruk vergrendelingen.|
     |Resourcegroep RGtoLock|Resourcegroep|Locatie|US - west 2|Definieert de locatie van de nieuwe resourcegroep om toe te passen van blauwdruk vergrendelingen.|
     |StorageAccount|Resource Manager-sjabloon|storageAccountType (StorageAccount)|Standard_GRS|De opslag-SKU. De standaardwaarde is _Standard_LRS_.|

1. Nadat u alle parameters hebt ingevoerd, selecteert u **toewijzen** aan de onderkant van de pagina.

In deze stap implementeert u de gedefinieerde resources en configureert u de geselecteerde **vergrendeling toewijzing**. Het kan tot 30 minuten om toe te passen van blauwdruk vergrendelingen duren.

Na de **toewijzen blauwdrukdefinitie is** portalmelding wordt weergegeven, gaat u naar de volgende stap.

## <a name="inspect-resources-deployed-by-the-assignment"></a>Resources die zijn geïmplementeerd door de toewijzing controleren

De toewijzing wordt gemaakt van de resourcegroep _TestingBPLocks_ en het opslagaccount dat is geïmplementeerd door het artefact Resource Manager-sjabloon. De nieuwe resourcegroep en de status van de geselecteerde vergrendeling worden op de pagina met details van toewijzing weergegeven.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **toegewezen blauwdrukken** pagina aan de linkerkant. De filters gebruiken om te zoeken de **toewijzing-vergrendeld-storageaccount-TestingBPLocks** blauwdruk toewijzen en selecteer dit.

   Op deze pagina ziet dat de toewijzing is voltooid en dat de resources zijn geïmplementeerd met de nieuwe status van de blauwdruk-vergrendeling. Als de toewijzing is bijgewerkt, de **toewijzingsbewerking** vervolgkeuzelijst bevat gedetailleerde informatie over de implementatie van elke versie van de definitie. U kunt de resourcegroep op de eigenschappenpagina te openen.

1. Selecteer de **TestingBPLocks** resourcegroep.

1. Selecteer de **toegangsbeheer (IAM)** pagina aan de linkerkant. Selecteer vervolgens de **roltoewijzingen** tabblad.

   Hier zien we dat de _toewijzing-vergrendeld-storageaccount-TestingBPLocks_ blauwdruktoewijzing is de _eigenaar_ rol. Deze rol heeft omdat deze rol is gebruikt voor het implementeren en een vergrendeling van de resourcegroep.

1. Selecteer de **weigeren toewijzingen** tabblad.

   De blauwdruktoewijzing gemaakt een [toewijzing weigeren](../../../role-based-access-control/deny-assignments.md) voor de geïmplementeerde resourcegroep om af te dwingen de **alleen-lezen** blauwdruk lock-modus. De toewijzing weigeren voorkomt dat iemand met de juiste rechten heeft op de **roltoewijzingen** tabblad uitvoeren van specifieke acties. Is van invloed op de toewijzing weigeren _alle principals_.

   Zie voor meer informatie over het uitsluiten van een principal van een toewijzing weigeren [blauwdrukken resource vergrendelen](../concepts/resource-locking.md#exclude-a-principal-from-a-deny-assignment).

1. Selecteer de toewijzing weigeren en selecteer vervolgens de **geweigerd machtigingen** pagina aan de linkerkant.

   De toewijzing weigeren wordt voorkomen dat alle bewerkingen met de **\*** en **actie** configuratie, maar het geeft leestoegang door uit te sluiten  **\* /lezen**via **NotActions**.

1. Selecteer in de Azure portal breadcrumb **TestingBPLocks - toegangsbeheer (IAM)** . Selecteer vervolgens de **overzicht** pagina aan de linkerkant en vervolgens de **resourcegroep verwijderen** knop. Voer de naam **TestingBPLocks** om te bevestigen van verwijderen en selecteer vervolgens **verwijderen** aan de onderkant van het deelvenster.

   De portalmelding **TestingBPLocks is mislukt voor resourcegroep verwijderen** wordt weergegeven. De fout gemeld dat hoewel uw account is gemachtigd om de resourcegroep te verwijderen, de toegang is geweigerd door de blauwdruktoewijzing. Houd er rekening mee dat we geselecteerd de **alleen-lezen** blauwdruk vergrendelingsmodus tijdens de blauwdruktoewijzing. De blauwdruk vergrendeling voorkomt dat een account met machtigingen, zelfs _eigenaar_, uit de bron te verwijderen. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../concepts/resource-locking.md).

Deze stappen laten zien dat onze geïmplementeerde resources nu zijn beveiligd met blauwdruk vergrendelingen die voorkomen dat ongewenste verwijderd, zelfs via een account dat is gemachtigd om de resources te verwijderen.

## <a name="unassign-the-blueprint"></a>De blauwdruk intrekken

De laatste stap is de toewijzing van de blauwdrukdefinitie verwijderen. De toewijzing verwijdert, niet de bijbehorende artefacten.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **toegewezen blauwdrukken** pagina aan de linkerkant. De filters gebruiken om te zoeken de **toewijzing-vergrendeld-storageaccount-TestingBPLocks** blauwdruk toewijzen en selecteer dit.

1. Selecteer **toewijzing verwijderen blauwdruk** aan de bovenkant van de pagina. Lees de waarschuwing in het bevestigingsdialoogvenster en selecteer vervolgens **OK**.

   Wanneer de blauwdruktoewijzing is verwijderd, worden de blauwdruk vergrendelingen worden ook verwijderd. De resources kunnen opnieuw worden verwijderd door een account met de juiste machtigingen.

1. Selecteer **resourcegroepen** vanuit het menu van Azure en selecteer vervolgens **TestingBPLocks**.

1. Selecteer de **toegangsbeheer (IAM)** pagina aan de linkerkant en selecteer vervolgens de **roltoewijzingen** tabblad.

De beveiliging voor de resourcegroep laat zien dat de blauwdruktoewijzing heeft niet langer _eigenaar_ toegang.

Na de **verwijderen blauwdruktoewijzing is voltooid** portalmelding wordt weergegeven, gaat u naar de volgende stap.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar met deze zelfstudie bent, verwijdert u deze resources:

- Resourcegroep _TestingBPLocks_
- De definitie van blauwdruk _vergrendeld storageaccount_

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [blauwdruk lifecycle](../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../concepts/parameters.md) gebruikt.
- Meer informatie over het gebruik van [blauwdruk resource vergrendelen](../concepts/resource-locking.md).
- Meer informatie over hoe u de [blauwdrukvolgorde](../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md).
- [Oplossen van problemen met](../troubleshoot/general.md) tijdens de toewijzing van blauwdruk.
