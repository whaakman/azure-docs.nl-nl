---
title: Het maken van dynamische blauwdrukken via parameters in Azure blauwdrukken
description: Meer informatie over statische en dynamische parameters en hoe ze worden gemaakt met dynamische blauwdrukken.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: e1a1d736eb9b22cd444a75b94d112abfe3fbe5af
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993575"
---
# <a name="creating-dynamic-blueprints-through-parameters"></a>Het maken van dynamische blauwdrukken via parameters

Een blauwdruk volledig gedefinieerd met verschillende artefacten (zoals resourcegroepen, Resource Manager-sjablonen, beleidsregels of roltoewijzingen) biedt het maken van snelle en consistente inrichting van objecten in Azure. Om in te schakelen flexibiliteit voor het gebruik van deze herbruikbare ontwerppatronen en containers, ondersteunt Azure blauwdrukken parameters. De parameter maakt flexibiliteit, zowel tijdens de definitie en toewijzing, kunt u eigenschappen van de artefacten geïmplementeerd door de blauwdruk wijzigen.

Een eenvoudig voorbeeld is de resource-group-artefact. Wanneer een resourcegroep is gemaakt, heeft deze twee vereiste waarden op die moeten worden opgegeven: naam en locatie. Als u een resourcegroep toevoegt aan de blauwdruk als parameters niet bestaat, kunt u die naam en locatie voor elk gebruik van de blauwdruk zou definiëren. Hierdoor zou elke gebruik van de blauwdruk voor het maken van artefacten in dezelfde resourcegroep bevinden. Terwijl niet een probleem met de resourcegroep zelf, resources in die resourcegroep zou worden gedupliceerd en een conflict veroorzaken.

> [!NOTE]
> Het is niet een probleem voor twee verschillende blauwdrukken om op te nemen van een resourcegroep met dezelfde naam.
> Als er een resourcegroep die is opgenomen in een blauwdruk al bestaat, blijft de blauwdruk voor het maken van de bijbehorende artefacten in die resourcegroep. Dit kan een conflict veroorzaken als de twee resources met dezelfde naam en resourcetype binnen een abonnement kan niet bestaan.

Dit is waar de parameters in past. De waarde voor deze eigenschappen in het geval van de resourcegroep de naam en locatie-eigenschap, blauwdrukken, kunt u ze niet tijdens de definitie van de blauwdruk te definiëren, maar voor het definiëren van de waarden in plaats daarvan tijdens de toewijzing aan een abonnement. Dit maakt het mogelijk om een blauwdruk die wordt gemaakt van een resourcegroep en andere resources in één abonnement zonder conflict opnieuw te gebruiken.

## <a name="blueprint-parameters"></a>Blauwdrukparameters

Via de REST-API, kunnen de parameters op de blauwdruk zelf naast elk van de ondersteunde artefacten worden gemaakt. Als een parameter in de blauwdruk is gemaakt, kan deze worden gebruikt door de artefacten in deze blauwdruk. Een voorbeeld is mogelijk het voorvoegsel voor de naamgeving van de resourcegroep. Het artefact kunt vervolgens de blauwdrukparameter gebruiken om te maken van een parameter 'voornamelijk dynamische', zoals de kan nog steeds worden gedefinieerd tijdens de toewijzing van de parameter, maar heeft een consistentiecontrole die aan de naamgevingsregels van de organisatie voldoen kan. Zie voor stappen [instelling statische parameters - blauwdruk niveauparameter](#blueprint-level-parameter).

### <a name="using-securestring-and-secureobject-parameters"></a>Met behulp van secureString en secureObject parameters

Tijdens het Resource Manager-sjabloon _artefact_ biedt ondersteuning voor parameters van de **secureString** en **secureObject** typen, vereist Azure blauwdrukken elk zijn verbonden met een Azure Key Vault.
Dit voorkomt dat de onveilige praktijk geheimen samen met de blauwdruk om op te slaan en dienst van beveiligde patronen worden gebruikers aangemoedigd. Azure blauwdrukken dit is mogelijk door het detecteren van het opnemen van een beveiligde parameter in Resource Manager-sjabloon _artefact_ en dat u wordt gevraagd tijdens de toewijzing van blauwdruk voor de volgende Key Vault eigenschappen per gedetecteerd beveiligde parameter:

- Key Vault-resource-ID
- Naam van de Key Vault-geheim
- Key Vault geheime versie

De Sleutelkluis waarnaar wordt verwezen, moet bestaan in hetzelfde abonnement als de blauwdruk wordt toegewezen aan en moet ook beschikken over **inschakelen van toegang tot Azure Resource Manager voor sjabloonimplementatie** geconfigureerd op de Key Vault **toegang beleid** pagina. Zie voor instructies over het inschakelen van deze functie [Key Vault - sjabloonimplementatie inschakelen](../../../managed-applications/key-vault-access.md#enable-template-deployment).
Zie voor meer informatie over Azure Key Vault [Key Vault, overzicht](../../../key-vault/key-vault-overview.md).

## <a name="parameter-types"></a>Parametertypen

### <a name="static-parameters"></a>Statische parameters

Een parameterwaarde die is gedefinieerd in de definitie van een blauwdruk heet een **statische parameter**. Dit komt omdat elk gebruik van de blauwdruk implementeert het artefact dat statische waarde gebruiken. In het voorbeeld van de groep resource terwijl dit heeft geen zin om de naam van de resourcegroep, kan het zinvol voor de locatie. Vervolgens elke toewijzing van de blauwdruk de resourcegroep maakt, ongeacht deze wordt aangeroepen tijdens de toewijzing, op dezelfde locatie. Hiermee kunt u selectief worden in wat u definieert als vereiste vs wat kan worden gewijzigd tijdens de toewijzing.

#### <a name="setting-static-parameters-in-the-portal"></a>Statische parameters instelt in de portal

1. Start de blauwdrukken voor Azure-service in Azure portal door te klikken op **alle services** en het zoeken en selecteren **beleid** in het linkerdeelvenster. Op de **beleid** pagina, klikt u op **blauwdrukken**.

1. Selecteer **Blauwdrukdefinities** op de pagina aan de linkerkant.

1. Klik op een bestaande blauwdruk en klik vervolgens op **blauwdruk bewerken** of klik op **+ maken blauwdruk** en vul de informatie op de **basisbeginselen** tabblad.

1. Klik op **volgende: artefacten** of klik op de **artefacten** tabblad.

1. Artefacten toegevoegd aan de blauwdruk waarvoor parameteropties weergeven **X van Y-parameters ingevuld** in de **Parameters** kolom. Klik op de rij artefact om de parameters van het artefact te bewerken.

   ![Blauwdrukparameters](../media/parameters/parameter-column.png)

1. De **bewerken artefact** waardeopties geschikt is voor het artefact op hebt geklikt op pagina worden weergegeven. Elke parameter op het artefact heeft een titel en een waarde in een selectievakje. Het vak ingesteld op uitgeschakeld zodat deze een **statische parameter**. In het voorbeeld hieronder, alleen _locatie_ is een **statische parameter** als deze uitgeschakeld is en _groepsnaam voor Accountresources_ is ingeschakeld.

   ![Statische blauwdrukparameters](../media/parameters/static-parameter.png)

#### <a name="setting-static-parameters-from-rest-api"></a>Statische parameters van de instelling van de REST-API

Er zijn in de URI van elk REST-API, variabelen die worden gebruikt dat u wilt vervangen door uw eigen waarden:

- `{YourMG}` -Vervangen door de naam van uw beheergroep
- `{subscriptionId}` -Vervangen door uw abonnements-ID

##### <a name="blueprint-level-parameter"></a>Niveau blauwdrukparameter

Bij het maken van een blauwdruk via REST-API, is het mogelijk te maken [blauwdruk parameters](#blueprint-parameters). Om dit te doen, gebruikt u de volgende REST-API-URI en de hoofdtekst van de indeling:

- REST API-URI

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint?api-version=2017-11-11-preview
  ```

- Aanvraagtekst

  ```json
  {
      "properties": {
          "description": "This blueprint has blueprint level parameters.",
          "targetScope": "subscription",
          "parameters": {
              "owners": {
                  "type": "array",
                  "metadata": {
                      "description": "List of AAD object IDs that is assigned Owner role at the resource group"
                  }
              }
          },
          "resourceGroups": {
              "storageRG": {
                  "description": "Contains the resource template deployment and a role assignment."
              }
          }
      }
  }
  ```

Zodra een niveau blauwdrukparameter is gemaakt, kan deze worden gebruikt op de artefacten die zijn toegevoegd aan deze blauwdruk.
De volgende REST-API-voorbeeld maakt een roltoewijzingsartefact in de blauwdruk en maakt gebruik van de blauwdruk niveauparameter.

- REST API-URI

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/roleOwner?api-version=2017-11-11-preview
  ```

- Aanvraagtekst

  ```json
  {
      "kind": "roleAssignment",
      "properties": {
          "resourceGroup": "storageRG",
          "roleDefinitionId": "/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
          "principalIds": "[parameters('owners')]"
      }
  }
  ```

In dit voorbeeld wordt de **principalIds** eigenschap die het gebruik van de **eigenaren** niveauparameter door te geven van een waarde van de blauwdruk `[parameters('owners')]`. Als u een parameter op een artefact met behulp van een niveau blauwdrukparameter is nog steeds een voorbeeld van een **statische parameter** omdat deze kan niet worden ingesteld tijdens de blauwdruktoewijzing en deze waarde voor elke toewijzing is.

##### <a name="artifact-level-parameter"></a>De niveauparameter artefact

Het maken van **statische parameters** op een artefact is vergelijkbaar, maar neemt een enkelvoudige waarde in plaats van de `parameters()` functie. Het volgende voorbeeld maakt u twee statische parameters, **tagName** en **tagValue**. De waarde voor elke wordt rechtstreeks geleverd en maakt geen gebruik van een aanroep van de functie.

- REST API-URI

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{YourMG}/providers/Microsoft.Blueprint/blueprints/MyBlueprint/artifacts/policyStorageTags?api-version=2017-11-11-preview
  ```

- Aanvraagtekst

  ```json
  {
      "kind": "policyAssignment",
      "properties": {
          "description": "Apply storage tag and the parameter also used by the template to resource groups",
          "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/49c88fc8-6fd1-46fd-a676-f12d1d3a4c71",
          "parameters": {
              "tagName": {
                  "value": "StorageType"
              },
              "tagValue": {
                  "value": "Premium_LRS"
              }
          }
      }
  }
  ```

### <a name="dynamic-parameters"></a>Dynamische parameters

Het tegenovergestelde van een statische parameter is een **dynamische parameter**. Dit is een parameter die is niet gedefinieerd in de blauwdruk, maar in plaats daarvan tijdens elke toewijzing van de blauwdruk is gedefinieerd. In het voorbeeld van de groep resource zinvol dit is voor de naam van de resourcegroep, een andere naam voor elke toewijzing van de blauwdruk biedt.

#### <a name="setting-dynamic-parameters-in-the-portal"></a>Dynamische parameters instellen in de portal

1. Start de blauwdrukken voor Azure-service in Azure portal door te klikken op **alle services** en het zoeken en selecteren **beleid** in het linkerdeelvenster. Op de **beleid** pagina, klikt u op **blauwdrukken**.

1. Selecteer **Blauwdrukdefinities** op de pagina aan de linkerkant.

1. Met de rechtermuisknop op de blauwdruk die u wilt toewijzen en selecteer **blauwdruk toewijzen** of klik op de blauwdruk die u wilt toewijzen en klik vervolgens op de **blauwdruk toewijzen** knop.

1. Op de **blauwdruk toewijzen** pagina, zoek de **artefact parameters** sectie. Elke artefact met ten minste één **dynamische parameter** het artefact en de configuratie-opties worden weergegeven. Geef de vereiste waarden voor de parameters voordat u de blauwdruk toewijzen. In het onderstaande voorbeeld _naam_ is een **dynamische parameter** die moet worden gedefinieerd om blauwdruktoewijzing te voltooien.

   ![Blauwdruk dynamische parameter](../media/parameters/dynamic-parameter.png)

#### <a name="setting-dynamic-parameters-from-rest-api"></a>Dynamische parameters van de instelling van de REST-API

Instellen van **dynamische parameters** tijdens de toewijzing wordt uitgevoerd door de gewenste waarde rechtstreeks op te geven. In plaats van een functie, zoals `parameters()`, de opgegeven waarde is de juiste tekenreeks. Artefacten voor een resourcegroep met een 'sjabloonnaam", zijn gedefinieerd en **naam** en **locatie** eigenschappen. Alle andere parameters voor elke opgenomen artefact wordt gedefinieerd in **parameters** met een **\<naam\>** en **waarde** sleutelpaar. Als de blauwdruk is geconfigureerd voor een dynamische parameter die niet is opgegeven tijdens de toewijzing, mislukken de toewijzing.

- REST API-URI

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Blueprint/blueprintAssignments/assignMyBlueprint?api-version=2017-11-11-preview
  ```

- Aanvraagtekst

  ```json
  {
      "properties": {
          "blueprintId": "/providers/Microsoft.Management/managementGroups/{YourMG}  /providers/Microsoft.Blueprint/blueprints/MyBlueprint",
          "resourceGroups": {
              "storageRG": {
                  "name": "StorageAccount",
                  "location": "eastus2"
              }
          },
          "parameters": {
              "storageAccountType": {
                  "value": "Standard_GRS"
              },
              "tagName": {
                  "value": "CostCenter"
              },
              "tagValue": {
                  "value": "ContosoIT"
              },
              "contributors": {
                  "value": [
                      "7be2f100-3af5-4c15-bcb7-27ee43784a1f",
                      "38833b56-194d-420b-90ce-cff578296714"
                  ]
                },
              "owners": {
                  "value": [
                      "44254d2b-a0c7-405f-959c-f829ee31c2e7",
                      "316deb5f-7187-4512-9dd4-21e7798b0ef9"
                  ]
              }
          }
      },
      "identity": {
          "type": "systemAssigned"
      },
      "location": "westus"
  }
  ```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [blauwdruk levenscyclus](lifecycle.md)
- Meer informatie over het aanpassen van de [volgorde blauwdruk](sequencing-order.md)
- Ontdek hoe u het gebruik van [blauwdruk resource vergrendelen](resource-locking.md)
- Meer informatie over het [bestaande toewijzingen bijwerken](../how-to/update-existing-assignments.md)
- Problemen oplossen bij het toewijzen van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)