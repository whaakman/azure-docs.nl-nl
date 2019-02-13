---
title: Overzicht van Azure Resource Manager | Microsoft Docs
description: Hierin wordt beschreven hoe u Azure Resource Manager kunt gebruiken voor implementatie, beheer, en beheer van toegang tot resources in Azure.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 76df7de1-1d3b-436e-9b44-e1b3766b3961
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2019
ms.author: tomfitz
ms.openlocfilehash: fbfbb4e452032073d92e7c2b93f78669da0ceaf5
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55691554"
---
# <a name="azure-resource-manager-overview"></a>Overzicht van Azure Resource Manager

Azure Resource Manager is de implementatie- en beheersservice voor Azure. Het biedt een consistente beheerlaag waarmee u resources in uw Azure-abonnement kunt maken, bijwerken en verwijderen. U kunt toegangsbeheer, controle en tagfuncties gebruiken om uw resources te beveiligen en te organiseren na de implementatie.

Wanneer u acties onderneemt via de portal, PowerShell, Azure CLI, REST-API's of client-SDK's, verwerkt de API van Azure Resource Manager uw aanvraag. Omdat alle aanvragen worden verwerkt via dezelfde API, ziet u consistente resultaten en mogelijkheden in de verschillende hulpprogramma's.

In de volgende afbeelding ziet u hoe alle hulpprogramma's werken met de Azure Resource Manager-API. Via de API worden aanvragen doorgegeven aan de Resource Manager-service, waar de aanvragen worden geverifieerd en geautoriseerd. De aanvragen worden vervolgens via Resource Manager naar de juiste service geleid.

![Resource Manager-aanvraagmodel](./media/resource-group-overview/consistent-management-layer.png)

## <a name="terminology"></a>Terminologie

Als u nog geen ervaring hebt met de Azure Resource Manager, zijn er enkele termen die u mogelijk niet kent.

* **resource** - een beheerbaar item dat beschikbaar is via Azure. Virtuele machines, opslagaccounts, web-apps, databases en virtuele netwerken zijn voorbeelden van resources.
* **resourcegroep** - een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep bevat die resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. Zie [Resourcegroepen](#resource-groups).
* **resourceprovider** - een service die zorgt voor Azure-resources. Voorbeeld: een veelgebruikte resourceprovider is **Microsoft.Compute**, die de resource van de virtuele machine levert. **Microsoft.Storage** is een andere algemene resourceprovider. Zie [Resourceproviders](#resource-providers).
* **Resource Manager-sjabloon** - Een JavaScript Object Notation (JSON)-bestand dat één of meer resources definieert die worden geïmplementeerd in een resourcegroep of abonnement. De sjabloon kan worden gebruikt om de resources consistent en herhaaldelijk te implementeren. Zie [Sjabloonimplementatie](#template-deployment).
* **declaratieve syntaxis** - een syntaxis waarmee u kunt aangeven 'Dit is wat ik wil maken' zonder hiervoor de nodige reeks programmeeropdrachten te hoeven maken. De sjabloon Resource Manager is een voorbeeld van een declaratieve syntaxis. In het bestand definieert u de eigenschappen voor de infrastructuur te implementeren naar Azure.

## <a name="the-benefits-of-using-resource-manager"></a>De voordelen van Resource Manager

Resource Manager biedt diverse voordelen:

* U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.
* U kunt gedurende de ontwikkelingscyclus uw oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.
* U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.
* U kunt de afhankelijkheden tussen resources zo definiëren dat deze in de juiste volgorde worden geïmplementeerd.
* U kunt toegangsbeheer toepassen op alle services in de resourcegroep omdat op rollen gebaseerd toegangsbeheer (RBAC) is geïntegreerd in het beheerplatform.
* U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.
* U kunt de facturering van uw organisatie transparanter maken door te kijken naar de kosten voor een groep resources met dezelfde tag.

## <a name="guidance"></a>Richtlijnen
Met de volgende tips kunt u profiteren van alle mogelijkheden die Resource Manager voor uw oplossingen biedt.

* Definieer en implementeer uw infrastructuur via de declaratieve syntaxis in de Resource Manager-sjablonen, in plaats van via imperatieve opdrachten.
* Definieer alle implementatie- en configuratiestappen in de sjabloon. Handmatige stappen voor het installeren van uw oplossing zijn niet nodig.
* Voer imperatieve opdrachten uit voor het beheren van uw resources, zoals het starten of stoppen van een app of machine.
* Breng resources met dezelfde levenscyclus onder in een resourcegroep. Gebruik tags voor het organiseren van alle andere resources.

Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance?toc=%2fazure%2fazure-resource-manager%2ftoc.json).

Ga voor aanbevelingen over het maken van Resource Manager-sjablonen naar [Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](template-best-practices.md).

## <a name="resource-groups"></a>Resourcegroepen
Er zijn een aantal belangrijke factoren waarmee u rekening moet houden bij het definiëren van de resourcegroep:

* Alle resources in uw groep moeten dezelfde levenscyclus hebben. U implementeert ze samen, werkt ze samen bij en verwijdert ze samen. Als een resource, zoals een databaseserver, in een andere implementatiecyclus moet werken, moet deze in een andere resourcegroep worden geplaatst.
* Elke resource kan in slechte één resourcegroep voorkomen.
* U kunt een resource op elk gewenst moment toevoegen aan of verwijderen uit een resourcegroep.
* U kunt een resource van de ene naar de andere resourcegroep verplaatsen. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](resource-group-move-resources.md).
* Een resourcegroep kan resources uit verschillende regio’s bevatten.
* Een resourcegroep kan worden gebruikt voor het bepalen van de mate van toegangsbeheer voor beheertaken.
* Een resource kan communiceren met resources in andere resourcegroepen. Deze interactie is gebruikelijk wanneer er een relatie bestaat tussen de twee resources, maar deze niet dezelfde levenscyclus delen (bijvoorbeeld web-apps die verbinding maken met een database).

Als u een resourcegroep maakt, moet u voor die resourcegroep een locatie opgeven. U vraagt zich misschien af: 'Waarom heeft een resourcegroep een locatie nodig? En als de resources andere locaties kunnen hebben dan de resourcegroep, wat is dan het nut van een locatie voor de resourcegroep?' De resourcegroep slaat metagegevens op over de resources. Dat is de reden waarom u moet aangeven waar die metagegevens moeten worden opgeslagen als u een locatie voor de resourcegroep opgeeft. In verband met nalevingsvereisten moet u er mogelijk voor zorgen dat uw gegevens worden opgeslagen in een bepaalde regio.

## <a name="resource-providers"></a>Resourceproviders

Elke resourceprovider biedt een set resources en bewerkingen voor het werken met deze resources. Als u bijvoorbeeld sleutels en geheimen wilt opslaan, werkt u met de resourceprovider **Microsoft.KeyVault**. Deze resourceprovider biedt een resourcetype **kluizen** voor het maken van de sleutelkluis.

De naam van een resourcetype heeft deze syntaxis: **{resourceprovider}/{resourcetype}**. Het resourcetype voor een sleutelkluis is **Microsoft.keyvault\vaults**.

Voordat u aan de slag gaat met de implementatie van uw resources, dient u inzicht te hebben in de beschikbare resourceproviders. Als u de namen van resourceproviders en resources kent, bent u beter in staat resources te definiëren die u wilt implementeren in Azure. Bovendien moet u voor elk resourcetype de geldige locaties en API-versies weten. Zie [Resourceproviders en -typen](resource-manager-supported-services.md) voor meer informatie.

Voor alle bewerkingen die worden aangeboden door resourceproviders, raadpleegt u de [Azure REST API's](/rest/api/azure/).

## <a name="template-deployment"></a>Sjabloonimplementatie

Met Resource Manager kunt u een sjabloon maken (in JSON-indeling) waarmee de infrastructuur en configuratie van uw Azure-oplossing worden gedefinieerd. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus de oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.

Zie [Inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md) voor meer informatie over de indeling van de sjabloon en hoe u deze samenstelt. Als u de JSON-syntaxis voor resourcetypen wilt bekijken, raadpleegt u [Define resources in Azure Resource Manager templates](/azure/templates/) (Resources definiëren in Azure Resource Manager-sjablonen).

Resource Manager verwerkt de sjabloon zoals alle andere aanvragen. De sjabloon wordt geparseerd en de bijbehorende syntaxis wordt geconverteerd naar REST API-bewerkingen voor de juiste resourceproviders. Bijvoorbeeld wanneer Resource Manager een sjabloon ontvangt met de volgende resourcedefinitie:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

De definitie wordt geconverteerd naar de volgende REST API-bewerking, die wordt verzonden naar de Microsoft.Storage-resourceprovider:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "Storage"
}
```

U kunt helemaal zelf bepalen hoe u sjablonen en resourcegroepen definieert en hoe u de oplossing beheert. U kunt de toepassing met drie lagen bijvoorbeeld met een enkele sjabloon implementeren in een enkele resourcegroep.

![sjabloon met drie lagen](./media/resource-group-overview/3-tier-template.png)

Maar u hoeft uw volledige infrastructuur niet te definiëren in één sjabloon. Vaak is het handiger om uw implementatievereisten te verdelen over een aantal gerichte sjablonen met een specifiek doel. U kunt deze sjablonen eenvoudig opnieuw gebruiken voor verschillende oplossingen. Voor het implementeren van een bepaalde oplossing kunt u een basissjabloon gebruiken die is gekoppeld aan alle vereiste sjablonen. In de volgende afbeelding ziet u hoe u een oplossing met drie lagen kunt implementeren via een bovenliggende sjabloon die drie geneste sjablonen bevat.

![sjabloon met geneste lagen](./media/resource-group-overview/nested-tiers-template.png)

De lagen hebben afzonderlijke levenscycli, zodat u ze kunt toepassen op verschillende resourcegroepen. Opmerking: de resources kunnen nog steeds worden gekoppeld aan resources in andere resourcegroepen.

![sjabloon met lagen](./media/resource-group-overview/tier-templates.png)

Zie [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md) (Gekoppelde sjablonen gebruiken met Azure Resource Manager) voor meer informatie over geneste sjablonen.

Azure Resource Manager analyseert afhankelijkheden om ervoor te zorgen dat de resources in de juiste volgorde worden gemaakt. Als een resource afhankelijk is van een waarde uit een andere resource (zoals een virtuele machine die een opslagaccount nodig heeft voor schijven), stelt u een afhankelijkheid in. Zie voor meer informatie [Afhankelijkheden definiëren in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md).

U kunt de sjabloon ook gebruiken voor updates aan de infrastructuur. U kunt bijvoorbeeld een resource toevoegen aan uw oplossing en configuratieregels toevoegen voor de resources die al zijn geïmplementeerd. Als de sjabloon een resource definieert die al bestaat, werkt Resource Manager de bestaande resource bij in plaats van een nieuwe te maken.

Resource Manager biedt uitbreidingen voor scenario's waarin aanvullende bewerkingen moeten worden uitgevoerd, zoals het installeren van bepaalde software die niet is opgenomen in de installatie. Als u al een configuratiebeheerservice gebruikt, zoals DSC, Chef of Puppet, kunt u via uitbreidingen met deze service blijven werken. Zie voor meer informatie over de extensies van virtuele machines [Informatie over extensies en functies van virtuele machines](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Wanneer u een oplossing vanaf de portal maakt, bevat de oplossing automatisch een sjabloon voor de implementatie. U hoeft de sjabloon niet helemaal zelf te maken. U kunt beginnen met de sjabloon voor uw oplossing en deze aanpassen aan uw specifieke behoeften. Ga voor een voorbeeld naar [Snelstartgids: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md). U kunt ook een sjabloon voor een bestaande resourcegroep ophalen door de huidige status van de resourcegroep te exporteren of door de sjabloon die is gebruikt voor een bepaalde implementatie weer te geven. Raadplegen van de [geëxporteerde sjabloon](resource-manager-export-template.md) is een handige manier om de syntaxis van de sjabloon te leren kennen.

De sjabloon wordt uiteindelijk onderdeel van de broncode van uw app. U kunt de broncode inchecken in uw broncodeopslag en deze bijwerken naarmate uw app zich verder ontwikkelt. U kunt de sjabloon bewerken met Visual Studio.

Nadat u de sjabloon hebt gedefinieerd, kunt u de resources in Azure implementeren. Zie voor het implementeren van de resources:

* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](resource-group-template-deploy.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](resource-group-template-deploy-cli.md)
* [Resources implementeren met Resource Manager-sjablonen en Azure Portal](resource-group-template-deploy-portal.md)
* [Resources implementeren met Resource Manager-sjablonen en Resource Manager REST API](resource-group-template-deploy-rest.md)

## <a name="safe-deployment-practices"></a>Veilige implementatiemethoden

Wanneer u een complexe service in Azure implementeert, moet u uw service mogelijk implementeren in meerdere regio's en de status ervan controleren voordat u doorgaat met de volgende stap. Gebruik [Azure Deployment Manager](deployment-manager-overview.md) voor de coördinatie van een gefaseerde implementatie van de service. Door de implementatie van uw service te faseren, kunt u potentiële problemen opsporen voordat de service is geïmplementeerd voor alle regio's. Als u deze voorzorgsmaatregelen niet nodig hebt, zijn de implementaties uit de voorgaande sectie de betere optie.

Deployment Manager bevindt zich momenteel in beperkte preview.

[!INCLUDE [arm-tutorials-quickstarts](../../includes/resource-manager-tutorials-quickstarts.md)]

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u Azure Resource Manager gebruikt voor de implementatie, het beheer en het toegangsbeheer van resources in Azure. Ga door naar het volgende artikel om te zien hoe u uw eerste Azure Resource Manager-sjabloon maakt.

> [!div class="nextstepaction"]
> [Snelstart: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md)
