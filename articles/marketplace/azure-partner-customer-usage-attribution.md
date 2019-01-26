---
title: Gebruik Azure attribution voor partners en klanten
description: Overzicht van hoe u voor het bijhouden van klanten te worden gebruikt voor Azure Marketplace-oplossingen
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: yijenj
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/17/2018
ms.author: yijenj
ms.openlocfilehash: 66f850225faba8d45eaa07493a0817c9ed0ed52b
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55075581"
---
# <a name="azure-partner-customer-usage-attribution"></a>Azure-partner klant gebruik attribution

Uw oplossingen vereisen een Azure-onderdelen of ze moeten rechtstreeks op de infrastructuur van Azure worden geïmplementeerd als een software-partner voor Azure. Klanten die een partneroplossing implementeren en inrichten van hun eigen Azure-resources kunnen vinden het moeilijk om meer inzicht verkrijgen in de status van de implementatie en beeldverwerkingstoepassingen krijgen tot de impact op de groei van Azure. Wanneer u een hoger niveau van zichtbaarheid toevoegt, moet u zijn afgestemd op de Microsoft-verkoopteams en tegoed voor Microsoft-partner-programma's krijgen. 

Microsoft biedt nu een methode om te kunnen bijhouden beter gebruik van Azure van implementaties van klanten van de software op Azure-partners. De nieuwe methode maakt gebruik van Azure Resource Manager voor het indelen van de implementatie van Azure-services.

Als een Microsoft-partner, kunt u gebruik van Azure kunt koppelen aan alle Azure-resources die u namens een klant inricht. U kunt de koppeling via de Azure Marketplace, de Quick Start-opslagplaats, persoonlijke GitHub-opslagplaatsen en praat klantbetrokkenheid vormen. Als u wilt bijhouden inschakelt, zijn twee methoden beschikbaar:

- Azure Resource Manager-sjablonen: Resource Manager-sjablonen of sjablonen voor oplossingen met de Azure-services voor het uitvoeren van de partner software implementeren. Partners kunnen maken van een Resource Manager-sjabloon voor het definiëren van de infrastructuur en configuratie van hun Azure-oplossing. Resource Manager-sjabloon kunt u en uw klanten om uw oplossing gedurende de levenscyclus te implementeren. U kunt er zeker van te zijn dat uw resources worden geïmplementeerd in een consistente status zijn. 
- Azure Resource Manager-API's: Partners kunnen de Resource Manager-API's rechtstreeks naar het Resource Manager-sjabloon implementeren of voor het genereren van de API-aanroepen voor het inrichten van Azure-services rechtstreeks aanroepen. 

Klant gebruik attribution is vereist op [aanbieding van Azure-toepassing](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer) gepubliceerde op Azure Marketplace.

## <a name="use-resource-manager-templates"></a>Resource Manager-sjablonen gebruiken

Veel oplossingen van partners zijn geïmplementeerd op het abonnement van een klant met behulp van Resource Manager-sjablonen. Hebt u een Resource Manager-sjabloon die beschikbaar is in de Azure Marketplace, op GitHub, of als een snelstartgids, moet het proces voor het wijzigen van de sjabloon om in te schakelen van een methode voor het nieuwe bijhouden rechtstreeks doorsturen.

Zie voor meer informatie over het maken en publiceren van sjablonen voor oplossingen

* [Uw eerste Resource Manager-sjabloon maken en implementeren](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).
* [Aanbieding voor Azure Application](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/azure-applications/cpp-azure-app-offer).
* Video: [Het bouwen van Oplossingssjablonen en beheerde toepassingen voor Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603).


## <a name="add-a-guid-to-your-template"></a>Een GUID toevoegen aan uw sjabloon

Als u wilt toevoegen een globally unique identifier (GUID), moet u een één wijziging van de belangrijkste sjabloonbestand maken:

1. [Maak een GUID](#create-guids) met behulp van de voorgestelde methode en [registreert de GUID](#register-guids-and-offers).

1. Open de Resource Manager-sjabloon.

1. Een nieuwe resource toevoegen in de belangrijkste sjabloonbestand. De resource moet zich in de **mainTemplate.json** of **azuredeploy.json** bestand alleen, en niet in een geneste of gekoppelde sjablonen.

1. Voer de GUID-waarde na de **pid -** voorvoegsel (zoals pid-eb7927c8-dd66-43e1-b0cf-c346a422063).

1. Controleer de sjabloon op fouten.

1. De sjabloon in de juiste opslagplaatsen publiceren.

1. [Controleren of GUID in de sjabloonimplementatie](#verify-the-guid-deployment).

### <a name="sample-resource-manager-template-code"></a>Voorbeeld van Resource Manager sjablooncode

Om in te schakelen bijhouden resources voor uw sjabloon, moet u de volgende aanvullende bron onder de sectie met resources toevoegen. Zorg ervoor dat u wijzigt de onderstaande voorbeeldcode met uw eigen invoer wanneer u deze aan de belangrijkste sjabloonbestand toevoegen.
De resource moet worden toegevoegd de **mainTemplate.json** of **azuredeploy.json** bestand alleen, en niet in een geneste of gekoppelde sjablonen.
```
// Make sure to modify this sample code with your own inputs where applicable

{ // add this resource to the resources section in the mainTemplate.json (do not add the entire file)
    "apiVersion": "2018-02-01",
    "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your generated GUID here
    "type": "Microsoft.Resources/deployments",
    "properties": {
        "mode": "Incremental",
        "template": {
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "resources": []
        }
    }
} // remove all comments from the file when complete
```

## <a name="use-the-resource-manager-apis"></a>De Resource Manager-API's gebruiken

In sommige gevallen moet u liever aanroepen rechtstreeks op basis van de Resource Manager REST API's naar Azure-services implementeren. [Azure biedt ondersteuning voor meerdere SDK's](https://docs.microsoft.com/azure/#pivot=sdkstools) zodat deze aanroepen. U kunt een van de SDK's gebruiken, of aanroepen van de REST-API's direct voor het implementeren van resources.

Als u een Resource Manager-sjabloon, moet u uw oplossing labelen door de eerder beschreven instructies te volgen. Als u niet met behulp van Resource Manager-sjabloon en die directe API-aanroepen, kunt u nog steeds uw implementatie om te koppelen van het gebruik van Azure-resources te labelen. 

### <a name="tag-a-deployment-with-the-resource-manager-apis"></a>Label van een implementatie met de Resource Manager-API 's

Bij het ontwerpen van uw API-aanroepen, wordt een GUID voor deze benadering bijhouden opnemen in de koptekst van de gebruiker agent in de aanvraag. De GUID voor elke aanbieding en SKU toevoegen. De tekenreeks met de **pid -** voorvoegsel en de partner gegenereerde GUID zijn. Hier volgt een voorbeeld van de GUID-indeling voor invoegen in de gebruikersagent: 

![Voorbeeld van de GUID-indeling](media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG)

> [!Note]
> De indeling van de tekenreeks is belangrijk. Als de **pid -** voorvoegsel niet is opgenomen, het is niet mogelijk om op te vragen van de gegevens. Verschillende SDK's bijhouden anders. Bekijk de ondersteuning en benadering bijhouden voor uw gewenste Azure-SDK voor het implementeren van deze methode. 

#### <a name="example-the-python-sdk"></a>Voorbeeld: De Python-SDK

Voor Python, gebruikt u de **config** kenmerk. U kunt het kenmerk alleen toevoegen aan een UserAgent. Hier volgt een voorbeeld:

![Het kenmerk toevoegen aan een van de gebruikersagent](media/marketplace-publishers-guide/python-for-lu.PNG)

> [!Note]
> Voeg het kenmerk voor elke client. Er is geen globale statische configuratie. U kunt een factory client om er zeker van te zijn dat is het bijhouden van elke client mogelijk taggen. Zie voor meer informatie dit [client factory voorbeeld op GitHub](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79).

#### <a name="tag-a-deployment-by-using-the-azure-powershell"></a>Label van een implementatie met behulp van Azure PowerShell

Als u resources via Azure PowerShell implementeert, moet u uw GUID toevoegen met behulp van de volgende methode:

```
[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")
```

#### <a name="tag-a-deployment-by-using-the-azure-cli"></a>Label van een implementatie met behulp van de Azure CLI

Wanneer u de Azure-CLI gebruiken voor uw GUID toevoegen, stelt de **AZURE_HTTP_USER_AGENT** omgevingsvariabele. U kunt deze variabele instellen binnen het bereik van een script. U kunt ook de variabele wereldwijd voor shell bereik instellen:

```
export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'
```

## <a name="create-guids"></a>GUID's maken

Een GUID is een unieke referentienummer met 32 hexadecimale cijfers. Voor het maken van GUID's voor het bijhouden, moet u een GUID-generator. Het Azure Storage-team heeft gemaakt een [GUID generator formulier](https://aka.ms/StoragePartners) die e-mail een GUID van de juiste indeling en kunnen worden hergebruikt binnen de systemen verschillende bijhouden. 

> [!Note]
> Het is raadzaam dat u [van Azure Storage GUID-generator formulier](https://aka.ms/StoragePartners) uw GUID maken. Zie voor meer informatie onze [Veelgestelde vragen over](#faq).

U wordt aangeraden u een unieke GUID voor elke aanbieding en de distributie-kanaal voor elk product. U kunt kiezen voor het gebruik van een enkele GUID voor meerdere distributiekanalen van het product als u niet wilt dat reporting om te worden verdeeld. 

Als u een product implementeren met behulp van een sjabloon en deze beschikbaar op de Azure Marketplace en GitHub is, kunt u maken en registreren van 2 afzonderlijke GUID's:

*   Product A in Azure Marketplace 
*   Product een op GitHub

Reporting wordt uitgevoerd door de partner-waarde (Microsoft Partner-ID) en de GUID's. 

Ook kunt u bijhouden van GUID's op een meer gedetailleerd niveau, zoals de SKU, waarbij SKU's varianten van een aanbieding zijn.

## <a name="register-guids-and-offers"></a>Registreert de GUID's en aanbiedingen

Als u wilt opnemen een GUID in onze bijhouden, moet de GUID worden geregistreerd.  

Alle registraties voor de sjabloon-GUID's moeten worden uitgevoerd via de Azure Marketplace Cloud Partner Portal (CPP). 

Nadat u de GUID's aan uw sjabloon of in de gebruikersagent toevoegen en de GUID in de CPP registreert, worden alle implementaties worden bijgehouden. 

1. Van toepassing op [Azure Marketplace](https://aka.ms/listonazuremarketplace) en krijg toegang tot de CPP.

   * Partners zijn vereist voor het [hebt u een profiel in CPP](https://docs.microsoft.com/azure/marketplace/become-publisher). U kunt aangemoedigd om de aanbieding in Azure Marketplace of AppSource weer te geven.
   * Partners kunnen zich registreren voor meerdere GUID's.
   * Partners kunnen zich registreren voor een GUID voor de sjablonen voor niet-Marketplace-oplossingen en aanbiedingen.
 
1. Aanmelden bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

1. Selecteer het pictogram van uw account in de rechterbovenhoek en selecteer vervolgens **uitgeverprofiel**.

   ![Profiel van de uitgever selecteren](media/marketplace-publishers-guide/guid-image-for-lu.png)

1. Op de **profielpagina**, selecteer **bijhouden van GUID toevoegen.**

   ![Selecteer toevoegen voor het bijhouden van GUID](media/marketplace-publishers-guide/guid-how-to-add-tracking.png)

1. In de **bijhouden van GUID** voert u uw bijhouden GUID. Voer alleen de GUID zonder de **pid -** voorvoegsel. In de **aangepaste beschrijving** voert u uw aanbiedingsnaam of beschrijving.

   ![Profielpagina](media/marketplace-publishers-guide/guid-dev-center-login.png)
   
   ![Voer de GUID en bieden van beschrijving](media/marketplace-publishers-guide/guid-dev-center-example.png)

1. Selecteer voor het registreren van meer dan één GUID **bijhouden van GUID toevoegen** opnieuw. Extra vakken op de pagina.

   ![Bijhouden van GUID toevoegen opnieuw selecteren](media/marketplace-publishers-guide/guid-dev-center-example-add.png)
   
   ![Voer een andere GUID en bieden van beschrijving](media/marketplace-publishers-guide/guid-dev-center-example-description.png)

1. Selecteer **Opslaan**.

   ![Opslaan selecteren](media/marketplace-publishers-guide/guid-dev-center-save.png)

Nadat u de GUID's aan uw sjabloon of in de gebruikersagent toevoegen en de GUID in de CPP registreert, worden alle implementaties worden bijgehouden. 

## <a name="verify-the-guid-deployment"></a>Controleer of de GUID-implementatie 

Nadat u de sjabloon aanpassen en een test-implementatie uitvoert, gebruikt u de volgende PowerShell-script om op te halen van de resources die u hebt geïmplementeerd en gelabeld. 

U kunt het script gebruiken om te controleren dat de GUID wordt toegevoegd aan uw Resource Manager-sjabloon. Het script niet van toepassing op Resource Manager-API-implementatie.

Meld u aan bij Azure. Selecteer het abonnement met de implementatie die u wilt controleren voordat u het script uitvoert. Voer het script in de context van het abonnement van de implementatie.

De **GUID** en **resourceGroup** naam van de implementatie zijn vereiste parameters.

U kunt krijgen [het oorspronkelijke script](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1) op GitHub.

```
Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName
)

# Get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

# Find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

# Find all deploymentOperations in a deployment by name
# PowerShell doesn't surface outputResources on the deployment
# or correlationId on the deploymentOperation

foreach ($deployment in $deployments){

# Get deploymentOperations by deploymentName
# then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}
```

## <a name="notify-your-customers"></a>Informeer uw klanten

Partners moeten hun klanten informeren over implementaties die gebruikmaken van Resource Manager-GUID bijhouden. Microsoft rapporteert het gebruik van Azure die is gekoppeld aan deze implementaties op met de partner. De volgende voorbeelden bevatten inhoud die u gebruiken kunt voor uw klanten informeren over deze implementaties. Vervang in de voorbeelden \<PARTNER > met de naam van uw bedrijf. Partners moeten controleren of dat de melding wordt uitgelijnd met hun gegevens privacy en verzameling beleid, inclusief opties voor klanten moeten worden uitgesloten van bijhouden. 

### <a name="notification-for-resource-manager-template-deployments"></a>Melding voor sjabloonimplementaties van Resource Manager

Wanneer u deze sjabloon implementeert, Microsoft is in staat om te identificeren van de installatie van \<PARTNER > software met de Azure-resources die zijn geïmplementeerd. Microsoft is kunnen correleren van de Azure-resources die worden gebruikt om de software te ondersteunen. Microsoft verzamelt deze informatie om de beste ervaringen met hun producten en om te werken van hun bedrijf. De gegevens worden verzameld en onderworpen aan het privacybeleid van Microsoft, die kunnen worden gevonden op https://www.microsoft.com/trustcenter. 

### <a name="notification-for-sdk-or-api-deployments"></a>Melding voor SDK of API-implementaties

Wanneer u implementeert \<PARTNER > software, Microsoft is in staat om te identificeren van de installatie van \<PARTNER > software met de Azure-resources die zijn geïmplementeerd. Microsoft is kunnen correleren van de Azure-resources die worden gebruikt om de software te ondersteunen. Microsoft verzamelt deze informatie om de beste ervaringen met hun producten en om te werken van hun bedrijf. De gegevens worden verzameld en onderworpen aan het privacybeleid van Microsoft, die kunnen worden gevonden op https://www.microsoft.com/trustcenter.

## <a name="get-support"></a>Ondersteuning krijgen

Als u hulp nodig hebt, volgt u deze stappen.

1. Ga naar de [ondersteuningspagina](https://go.microsoft.com/fwlink/?linkid=844975). 

1. Onder **probleemtype**, selecteer **Marketplace Onboarding**.

1. Kies de **categorie** voor uw probleem:

   - Voor gebruik van koppeling problemen, selecteert u **andere**.
   - Voor toegang tot problemen met de Azure Marketplace-CPP, selecteert u **probleem met toegang tot**.
   
    ![Kies de probleemcategorie](media/marketplace-publishers-guide/lu-article-incident.png)

1. Selecteer **aanvraag starten**.

1. Voer de vereiste waarden op de volgende pagina. Selecteer **Doorgaan**.

1. Voer de vereiste waarden op de volgende pagina.

   > [!Important] 
   > In de **Incident titel** Voer **ISV gebruik bijhouden**. Beschrijf het probleem in detail.
   
   ![Voer ISV gebruik bijhouden voor de titel van incident](media/marketplace-publishers-guide/guid-dev-center-help-hd%201.png)

1. Vul het formulier in en selecteer vervolgens **indienen**.

## <a name="faq"></a>Veelgestelde vragen

**Wat is het voordeel van de GUID toe te voegen aan de sjabloon?**

Microsoft biedt partners met een weergave van implementaties van klanten hun sjablonen en inzichten op het gebruik van hun beïnvloed. Deze gegevens dichter bij betrokkenheid tussen verkoopteams kunnen gebruiken door zowel Microsoft als de partner. Zowel Microsoft als de partner kunnen u de gegevens gebruiken om op te halen van een consistente weergave van de impact van een afzonderlijke partner op Azure groei. 

**Wie kan een GUID toevoegen aan een sjabloon?**

De tracerings-resource is bedoeld om van de partner-oplossing verbinden met gebruik van Azure van de klant. Gegevens over gebruik is gekoppeld aan de identiteit van een partner Microsoft Partner Network (MPN-ID). Rapportage is beschikbaar voor partners in de CPP.

**Nadat een GUID wordt toegevoegd, kan het worden gewijzigd?**
 
Ja, een klant of implementatiepartner kan de sjabloon aanpassen en kunt wijzigen of verwijderen van de GUID. Het is raadzaam dat partners proactief de rol van de resource en de GUID voor hun klanten en partners beschrijven om te voorkomen dat de verwijdering of bewerkingen voor het bijhouden van GUID. De GUID wijzigt, worden alleen nieuwe, niet-bestaande implementaties en resources verwijderd.

**Wanneer reporting beschikbaar zal zijn?**

Een bètaversie van rapportage moet zijn binnenkort beschikbaar. Reporting wordt in de CPP worden geïntegreerd.

**Kan ik de sjablonen die zijn geïmplementeerd vanuit een niet-Microsoft-opslagplaats, zoals GitHub bijhouden?**

Ja, zolang de GUID gebruikt wordt als de sjabloon wordt geïmplementeerd, wordt gebruik bijgehouden. Partners moeten een profiel hebt in het CPP registreren verwante sjablonen die buiten de Azure Marketplace worden gepubliceerd. 

**Is er een verschil als de sjabloon wordt geïmplementeerd op Azure Marketplace ten opzichte van andere opslagplaatsen zoals GitHub?**

Ja, kunnen partners die het publiceren van aanbiedingen in de Azure Marketplace meer gedetailleerde gegevens over implementaties ontvangen van de Azure Marketplace. Partners profiteren van hun aanbieding aan klanten in de portal van Azure Marketplace en in Azure portal om vrij te geven. Aanbiedingen in de Azure Marketplace een leads voor de partner.

**Wat gebeurt er als ik een aangepaste sjabloon voor een afzonderlijke klantcontacten maken?**

U kunt nog steeds Welkom bij de GUID toevoegen aan de sjabloon. Als u een bestaande geregistreerde GUID, het opgenomen in de rapportage. Als u een nieuwe GUID maakt, moet u de nieuwe GUID om deze opgenomen in de tracering te laten registreren.

**De klant's ontvangen ook melden?**

Klanten kunnen hun gebruik van afzonderlijke resources of resourcegroepen in Azure portal door de klant gedefinieerde bijhouden.   

**Is deze methodologie vergelijkbaar voor de digitale Partner van Record (DPOR)?**

Deze nieuwe methode voor het koppelen van de implementatie en het gebruik aan van een partner-oplossing biedt een mechanisme om te koppelen van een partneroplossing aan het gebruik van Azure. DPOR is bedoeld om een advies (systeemintegrator) of management (Managed Service Provider) werken samen met een klant Azure-abonnement te koppelen.   

**Wat is het voordeel van Azure Storage GUID-Generator formulier?**

Azure Storage GUID-Generator formulier kan worden gegarandeerd voor het genereren van een GUID van de vereiste indeling. Bovendien, als u van Azure-Storage-gegevenslaag methoden bijhouden gebruikmaakt, kunt u gebruikmaken van dezelfde GUID voor de controlelaag Marketplace bijhouden. Hiermee kunt u gebruikmaken van een GUID voor een uniforme voor Partner attribution zonder te onderhouden afzonderlijke GUID's.
