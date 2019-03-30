---
title: Nieuwe resources beveiligen met resourcevergrendelingen op basis van een blauwdruk
description: Leer hoe u met de blauwdrukken voor Azure-resourcevergrendelingen alleen-lezen en verwijder niet beveiligen zojuist geïmplementeerde resources.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/28/2019
ms.topic: tutorial
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: f39d59ef7ab3f555637aef69b301a0e77c00fc24
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629218"
---
# <a name="protect-new-resources-with-azure-blueprints-resource-locks"></a>Nieuwe resources beveiligen met blauwdrukken voor Azure-resourcevergrendelingen

Azure blauwdrukken [resourcevergrendelingen](../concepts/resource-locking.md) maken het mogelijk is zojuist geïmplementeerde resources beveiligen tegen wordt geknoeid, zelfs niet door een account met de _eigenaar_ rol. Deze beveiliging kan worden toegevoegd aan resources die zijn gemaakt door een Resource Manager-sjabloon-artefact in het blauwdrukdefinitie van de.

De volgende stappen worden behandeld:

> [!div class="checklist"]
> - De blauwdrukdefinitie van een nieuwe maken
> - Markeren als de blauwdrukdefinitie van de **gepubliceerd**
> - De blauwdrukdefinitie van de toewijzen aan een bestaand abonnement
> - Inspecteer de nieuwe resourcegroep
> - Toewijzing van de blauwdruk om de vergrendelingen te verwijderen

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze zelfstudie hebt voltooid, wordt een Azure-abonnement nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="create-new-blueprint-definition"></a>Nieuwe blauwdrukdefinitie maken

Maak eerst de nieuwe blauwdrukdefinitie.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Uit de **aan de slag** pagina aan de linkerkant, selecteer de **maken** knop onder _een blauwdruk maken_.

1. Zoek de **lege blauwdruk** blauwdruk voorbeeld aan de bovenkant van de pagina en selecteer **beginnen met een lege blauwdruk**.

1. Voer de _basisbeginselen_ van de blauwdruk-voorbeeld:

   - **De blauwdruknaam van de**: Geef een naam voor uw exemplaar van de blauwdruk-voorbeeld. Voor deze zelfstudie gebruiken we de naam _vergrendeld storageaccount_.
   - **Beschrijving van blauwdruk**: Beschrijving van de blauwdrukdefinitie. Gebruik 'voor blauwdruk testbron vergrendelen op de geïmplementeerde resources."
   - **Definitielocatie**: Gebruik het beletselteken en selecteer de beheergroep of het abonnement op te slaan van de blauwdrukdefinitie van de aan.

1. Selecteer de _artefacten_ tabblad aan de bovenkant van de pagina of **volgende: Artefacten** aan de onderkant van de pagina.

1. Resourcegroep toevoegen aan een abonnement: Selecteer de **+ artefact toevoegen...**  rij onder **abonnement**.
   Selecteer Resourcegroep als _Type artefact_. Stel de _artefact weergavenaam_ naar **RGtoLock**.
   Laat de velden _Naam resourcegroep_ en _Locatie_ leeg, maar zorg dat het selectievakje voor elke eigenschap is ingeschakeld zodat het **dynamische parameters** zijn. Klik op **Toevoegen** om dit artefact toe te voegen aan de blauwdruk.

1. Sjabloon toevoegen aan een resourcegroep: Selecteer de **+ artefact toevoegen...** rij onder de **RGtoLock** vermelding. Selecteer Azure Resource Manager-sjabloon als _Type artefact_, stel _Weergavenaam van artefact_ in op StorageAccount en laat de eigenschap _Beschrijving_ leeg. Op het tabblad **Sjabloon** van het editorvak plakt u de volgende Resource Manager-sjabloon. Na het plakken van de sjabloon, selecteert u **toevoegen** dit artefact toevoegen aan de blauwdruk.

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

Zodra de **opslaan blauwdrukdefinitie is** portalmelding wordt weergegeven, verplaatsen naar de volgende stap.

## <a name="publish-the-blueprint-definition"></a>De blauwdrukdefinitie publiceren

De blauwdrukdefinitie is gemaakt in uw omgeving. Deze gemaakt **Draft** modus en moet **gepubliceerd** voordat deze kunnen worden toegewezen en geïmplementeerd.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om te zoeken de _vergrendeld storageaccount_ blauwdruk definitie en selecteer dit.

1. Selecteer **blauwdruk publiceren** aan de bovenkant van de pagina. Geef in het nieuwe deelvenster aan de rechterkant **versie** als _1.0_. Deze eigenschap is handig voor als u later een wijziging aanbrengt. Geef **notities wijzigen** zoals "eerste versie is gepubliceerd voor het vergrendelen van blauwdruk geïmplementeerd resources." Selecteer vervolgens **publiceren** aan de onderkant van de pagina.

Deze stap maakt het mogelijk is de blauwdruk toewijzen aan een abonnement. Zodra gepubliceerd, kunnen nog steeds wijzigingen worden aangebracht. Aanvullende wijzigingen publiceren met een nieuwe vereisen **versie** waarde voor het bijhouden van de verschillen tussen verschillende versies van dezelfde blauwdrukdefinitie voor.

Zodra de **publiceren is voltooid de definitie van blauwdruk** portalmelding wordt weergegeven, verplaatsen naar de volgende stap.

## <a name="assign-the-blueprint-definition"></a>De blauwdrukdefinitie van de toewijzen

Zodra de blauwdrukdefinitie is **gepubliceerd**, deze kan worden toegewezen aan een abonnement in de beheergroep is opgeslagen op. Deze stap is waar de parameters worden opgegeven voor elke implementatie van de blauwdrukdefinitie uniek te maken.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om te zoeken de _vergrendeld storageaccount_ blauwdruk definitie en selecteer dit.

1. Selecteer **blauwdruk toewijzen** aan de bovenkant van de pagina van de blauwdruk-definitie.

1. Geef de parameterwaarden voor de blauwdruktoewijzing:

   - Basisbeginselen

     - **Abonnementen**: Selecteer een of meer van de abonnementen die in de beheergroep die u zijn aan de blauwdrukdefinitie van de opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing worden gemaakt voor elk gebruik van de ingevoerde parameters.
     - **Naam van de roltoewijzing**: De naam is ingevuld op basis van de naam van de blauwdrukdefinitie. We willen dat deze toewijzing te vertegenwoordigen vergrendelen van de nieuwe resourcegroep, wijzig de naam van de toewijzing aan _toewijzing-vergrendeld-storageaccount-TestingBPLocks_.
     - **Locatie**: Selecteer een regio voor de beheerde identiteit moet worden gemaakt. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
       Selecteer voor deze zelfstudie _VS-Oost 2_.
     - **De versie van blauwdruk**: Kies de **gepubliceerd** versie _1.0_ van de blauwdrukdefinitie.

   - Toewijzing vergrendelen

     Selecteer de _alleen-lezen_ blauwdruk lock-modus. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../concepts/resource-locking.md).

   - Beheerde identiteit

     Laat de standaardwaarde _systeem toegewezen_ optie. Zie voor meer informatie, [beheerde identiteiten](../../../active-directory/managed-identities-azure-resources/overview.md).

   - Artefactparameters

     De gedefinieerde parameters in deze sectie zijn van toepassing op het artefact waarin deze gedefinieerd. Deze parameters zijn [dynamische parameters](../concepts/parameters.md#dynamic-parameters) omdat ze tijdens de toewijzing van de blauwdruk zijn gedefinieerd. Voor elke artefact, stelt u de waarde van parameter op wat is gedefinieerd de **waarde** kolom.

     |Naam van het artefact|Type artefact|Parameternaam|Value|Description|
     |-|-|-|-|-|
     |Resourcegroep RGtoLock|Resourcegroep|Name|TestingBPLocks|Bepaalt de naam van de nieuwe resourcegroep om toe te passen van blauwdruk vergrendelingen.|
     |Resourcegroep RGtoLock|Resourcegroep|Locatie|US - west 2|Definieert de locatie van de nieuwe resourcegroep om toe te passen van blauwdruk vergrendelingen.|
     |Opslagaccount|Resource Manager-sjabloon|storageAccountType (StorageAccount)|Standard_GRS|Selecteer de opslag-SKU. Standaardwaarde is _Standard_LRS_.|

1. Zodra alle parameters hebt ingevoerd, selecteert u **toewijzen** aan de onderkant van de pagina.

In deze stap implementeert u de gedefinieerde resources en configureert u de geselecteerde **vergrendeling toewijzing**. Blauwdruk vergrendelingen kunnen maximaal 30 minuten om toe te passen.

Zodra de **toewijzen blauwdrukdefinitie is** portalmelding wordt weergegeven, verplaatsen naar de volgende stap.

## <a name="inspect-resources-deployed-by-the-assignment"></a>Resources die zijn geïmplementeerd door de toewijzing controleren

De toewijzing van de resourcegroep gemaakt _TestingBPLocks_ en het opslagaccount dat is geïmplementeerd door het artefact Resource Manager-sjabloon. De nieuwe resourcegroep en de status van de geselecteerde vergrendeling worden weergegeven op de pagina met details van toewijzing.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **toegewezen blauwdrukken** pagina aan de linkerkant. De filters gebruiken om te zoeken de _toewijzing-vergrendeld-storageaccount-TestingBPLocks_ blauwdruk toewijzen en selecteer dit.

   Op deze pagina ziet u de toewijzing is voltooid en de resources zijn geïmplementeerd met de nieuwe status van de blauwdruk-vergrendeling. Als de toewijzing is bijgewerkt, de **toewijzingsbewerking** vervolgkeuzelijst bevat gedetailleerde informatie over de implementatie van elke versie van de definitie. De resourcegroep kan worden geklikt om de eigenschappenpagina rechtstreeks worden geopend.

1. Selecteer de **TestingBPLocks** resourcegroep.

1. Selecteer de **toegangsbeheer (IAM)** pagina aan de linkerkant en vervolgens de **roltoewijzingen** tabblad.

   Hier zien we dat de _toewijzing-vergrendeld-storageaccount-TestingBPLocks_ blauwdruktoewijzing is de _eigenaar_ rol die is gebruikt voor het implementeren en een vergrendeling van de resourcegroep.

1. Selecteer de **weigeren toewijzingen** tabblad.

   De blauwdruktoewijzing gemaakt een [toewijzing weigeren](../../../role-based-access-control/deny-assignments.md) voor de geïmplementeerde resourcegroep om af te dwingen de _alleen-lezen_ blauwdruk lock-modus. De toewijzing weigeren voorkomt dat iemand met de juiste rechten heeft op de _roltoewijzingen_ tabblad uitvoeren van specifieke acties. Is van invloed op de toewijzing weigeren _alle principals_.

   Zie voor meer informatie over het uitsluiten van een principal van een toewijzing weigeren [blauwdrukken resource vergrendelen](../concepts/resource-locking.md#exclude-a-principal-from-a-deny-assignment).

1. Selecteer de toewijzing weigeren en selecteer vervolgens de **geweigerd machtigingen** pagina aan de linkerkant.

   De toewijzing weigeren wordt voorkomen dat alle bewerkingen met de **\*** en **actie** configuratie, maar geeft leestoegang door uit te sluiten  **\* /lezen**via **NotActions**.

1. Selecteer in de Azure portal breadcrumb **TestingBPLocks - toegangsbeheer (IAM)**. Selecteer vervolgens de **overzicht** pagina aan de linkerkant en vervolgens de **resourcegroep verwijderen** knop. Voer de naam _TestingBPLocks_ om te bevestigen van het verwijderen en selecteer **verwijderen** aan de onderkant van het deelvenster.

   De portalmelding **TestingBPLocks is mislukt voor resourcegroep verwijderen** wordt weergegeven. De fout gemeld dat terwijl uw account is gemachtigd om de resourcegroep te verwijderen, de toegang is geweigerd door de blauwdruktoewijzing. Houd er rekening mee dat we geselecteerd de _alleen-lezen_ blauwdruk vergrendelingsmodus tijdens de blauwdruktoewijzing. De blauwdruk vergrendeling voorkomt dat een account met machtigingen, zelfs _eigenaar_, uit de bron te verwijderen. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../concepts/resource-locking.md).

Deze stappen laten zien dat onze geïmplementeerde resources nu zijn beveiligd met blauwdruk vergrendelingen die voorkomen ongewenste verwijderen, zelfs bij een account met machtigingen dat.

## <a name="unassign-the-blueprint"></a>De blauwdruk intrekken

De laatste stap is de toewijzing van de blauwdrukdefinitie verwijderen. De toewijzing verwijdert, niet de aangeraakt artefacten.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **toegewezen blauwdrukken** pagina aan de linkerkant. De filters gebruiken om te zoeken de _toewijzing-vergrendeld-storageaccount-TestingBPLocks_ blauwdruk toewijzen en selecteer dit.

1. Selecteer de **toewijzing verwijderen blauwdruk** knop aan de bovenkant van de pagina. Lees de waarschuwing in het bevestigingsvenster en selecteer vervolgens **OK**.

   Met de blauwdruktoewijzing is verwijderd, worden ook de blauwdruk vergrendelingen verwijderd. De gemaakte resources kunnen opnieuw worden verwijderd door een account met machtigingen.

1. Selecteer **resourcegroepen** vanuit het menu van Azure, selecteert u vervolgens **TestingBPLocks**.

1. Selecteer de **toegangsbeheer (IAM)** pagina aan de linkerkant en vervolgens de **roltoewijzingen** tabblad.

De beveiliging voor de resourcegroep laat zien dat de blauwdruktoewijzing heeft niet langer _eigenaar_ toegang.

Zodra de **verwijderen blauwdruktoewijzing is voltooid** portalmelding wordt weergegeven, verplaatsen naar de volgende stap.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met deze zelfstudie, verwijdert u de volgende resources:

- Resourcegroep _TestingBPLocks_
- De definitie van blauwdruk _vergrendeld storageaccount_

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [levenscyclus van een blauwdruk](../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../concepts/resource-locking.md).
- Meer informatie over hoe u de [blauwdrukvolgorde](../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md).
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md).