---
title: Gebruik Azure attribution voor partners en klanten
description: Overzicht van hoe u voor het bijhouden van klanten te worden gebruikt voor Azure Marketplace-oplossingen
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/26/2018
ms.author: ellacroi
ms.openlocfilehash: 95ad327380707dcfe14aa5aa3d91b8da2309eb05
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630888"
---
# <a name="azure-partner-customer-usage-attribution"></a>Azure-partner klant gebruik attribution

Als een software-partner voor Azure, uw oplossingen een Azure-onderdelen vereisen of rechtstreeks in infrastructuurservices van Azure worden geïmplementeerd.  Vandaag de dag wanneer een klant een partneroplossing implementeert en hun eigen Azure-resources inricht, is het moeilijk voor de partner te krijgen van de zichtbaarheid van de status van deze implementaties en lastig om beeldverwerkingstoepassingen in gevolgen voor de groei van Azure. Toevoegen van een hoger niveau van zichtbaarheid helpt partners die zijn afgestemd op de Microsoft-verkoopteams en tegoed voor Microsoft-partner-programma's krijgen.   

Microsoft is het maken van een nieuwe methode zodat partners beter gebruik van Azure die het gevolg is van een klant implementeren van uw software op Azure bijhouden. Deze nieuwe methode is gebaseerd op Azure Resource Manager gebruiken voor het indelen van de implementatie van Azure-services.

Als een Microsoft-partner, kunt u gebruik van Azure kunt koppelen aan alle Azure-resources die u in te op andere gebruikers van een klant richten.  Deze koppeling kan worden gedaan via de Azure Marketplace, de Quick Start-opslagplaats, persoonlijke github-opslagplaatsen en zelfs 1 op 1 klantbetrokkenheid.  Als u wilt bijhouden inschakelt, zijn twee manieren beschikbaar:

- Azure Resource Manager-sjablonen: Azure Resource Manager-sjablonen of sjablonen voor oplossingen met de Azure-services voor het uitvoeren van de partner software implementeren. Partners kunnen Azure Resource Manager-sjabloon die de infrastructuur en configuratie van uw Azure-oplossing definieert maken. Het maken van een Azure Resource Manager-sjabloon kunt u en uw klanten gedurende de levenscyclus van uw oplossing implementeren en erop vertrouwen dat die uw resources worden geïmplementeerd in een consistente status. 

- Azure Resource Manager-API's: partners kunnen de Azure Resource Manager API's om ofwel een Azure Resource Manager-sjabloon implementeren rechtstreeks te worden aangeroepen of voor het genereren van de API-aanroepen voor het inrichten van rechtstreeks Azure-services. 

## <a name="method-1-azure-resource-manager-templates"></a>Methode 1: Azure Resource Manager-sjablonen 

Tegenwoordig zijn veel oplossingen van partners geïmplementeerd op het abonnement van een klant met behulp van Azure Resource Manager-sjablonen.  Als u al een Azure Resource Manager-sjabloon beschikbaar in de Azure Marketplace op GitHub, of als een snelstartgids, moet het proces van het wijzigen van de sjabloon voor het inschakelen van deze nieuwe methode voor het bijhouden van rechtstreeks doorsturen.  Als u vandaag nog niet een Azure Resource Manager-sjabloon gebruikt, worden hier enkele koppelingen naar informatie waarmee u beter inzicht in de Azure Resource Manager-sjablonen en hoe u een te maken: 

*   [Uw eerste Azure Resource Manager-sjabloon maken en implementeren](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)
*   [Handleiding voor het maken van een oplossingssjabloon voor Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-solution-template-creation)

## <a name="instructions-add-a-guid-to-your-existing-azure-resource-manager-template"></a>Instructies: een GUID toevoegen aan uw bestaande Azure Resource Manager-sjabloon

Toevoegen van de GUID is een één wijziging van de belangrijkste sjabloon:
 1. Maak een GUID, Stel dat de gegenereerde waarde eb7927c8-dd66-43e1-b0cf-c346a422063
 2. Open de Azure Resource Manager-sjabloon
 3. Een nieuwe resource toevoegen in de belangrijkste sjabloonbestand. De resource alleen moet zich in de mainTemplate.json of azuredeploy.json, niet in een geneste of gekoppelde sjablonen.
 4. Voer de GUID na de 'pid-' zoals u hierboven ziet.

   Dit ziet er ongeveer als in dit voorbeeld: `pid-eb7927c8-dd66-43e1-b0cf-c346a422063`

 5. Controleer de sjabloon voor fouten
 6. De sjabloon in de juiste opslagplaatsen publiceren

## <a name="sample-template-code"></a>Voorbeeldcode van sjabloon

![](https://raw.githubusercontent.com/ellacroi/azure-docs-pr/lu-images-again-dangit-all/articles/marketplace/media/marketplace-publishers-guide/tracking-sample-code-for-lu-1.PNG?token=Ak8ZDB0JzsBdUGlKEIeHNJRS7b0BWn4Gks5bbMwwwA%3D%3D)


## <a name="method-2-azure-resource-manager-apis"></a>Methode 2: Azure Resource Manager API 's

In sommige gevallen moet u liever aanroepen rechtstreeks op basis van de Azure Resource Manager REST API's naar Azure-services implementeren. [Azure biedt ondersteuning voor meerdere SDK's](https://docs.microsoft.com/azure/#pivot=sdkstools) inschakelen dit.  U kunt een van de SDK's gebruiken, of aanroepen van de REST-API's direct voor het implementeren van resources.

Als u een Azure Resource Manager-sjabloon gebruikt, moet u de oplossing met behulp van de bovenstaande instructies taggen.  Als u niet met behulp van een Azure Resource Manager-sjabloon en die directe API-aanroepen kunt u nog steeds uw implementatie om te koppelen van het gebruik van Azure-resources te labelen. 

**Informatie over het taggen van een implementatie met behulp van de Azure Resource Manager API's:** voor deze benadering gebruikt bij het ontwerpen van uw API-aanroepen dat u een GUID in de koptekst van de gebruiker agent in de aanvraag bevat. De GUID moet worden toegevoegd voor elke aanbieding of SKU.  De tekenreeks moet zijn geformatteerd met het voorvoegsel van de pid - en neemt u de partner gegenereerde GUID.   

![](https://raw.githubusercontent.com/ellacroi/azure-docs-pr/lu-images-again-dangit-all/articles/marketplace/media/marketplace-publishers-guide/tracking-sample-guid-for-lu-2.PNG?token=Ak8ZDDiokRcj4PJj0aMkZmfF8BdOuOTzks5bbM35wA%3D%3D)

>[!Note] 
>GUID-indeling voor invoegen in de gebruikersagent: pid-eb7927c8-dd66-43e1-b0cf-c346a422063 / / Voer uw GUID na de 'pid-'

De indeling van de tekenreeks is belangrijk. Als het voorvoegsel 'pid-' niet opgenomen is, is het niet mogelijk de gegevens op te vragen. Verschillende SDK's doen dit anders.  Voor het implementeren van deze methode, moet u de ondersteuning en een nieuwe benadering bekijken voor uw gewenste Azure-SDK. 

**Voorbeeld met behulp van de Python-SDK:** voor Python, moet u het kenmerk "configuratie" gebruiken. U kunt alleen toevoegen aan een UserAgent. Hier volgt een voorbeeld:

![](https://raw.githubusercontent.com/ellacroi/azure-docs-pr/lu-images-again-dangit-all/articles/marketplace/media/marketplace-publishers-guide/python-for-lu.PNG?token=Ak8ZDK5Um4J6oY-7x25tuBpa168BEiYMks5bbMuUwA%3D%3D)

>Dit moet worden uitgevoerd voor elke client, er is geen globale statische configuratie (u kunt kiezen een factory client om er zeker van dat elke client zoiets doen. 
>[Aanvullende referentie-informatie](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79)

Informatie over het taggen van een implementatie met behulp van Azure PowerShell of Azure CLI: als u resources via AzurePowerShell implementeert, kunt u uw GUID toevoegen met behulp van de volgende methode:

```

[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")


```

Als u wilt uw GUID toevoegen bij het gebruik van de Azure CLI, stel de omgevingsvariabele AZURE_HTTP_USER_AGENT.  U kunt deze variabele instellen binnen het bereik van een script of wereldwijd instellen voor gebruik van shell-bereik:

```

export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'


```

## <a name="registering-guidsoffers"></a>Registreren van GUID's / aanbiedingen

In de volgorde voor de GUID moet worden opgenomen in onze bijhouden, moet deze worden geregistreerd.  

Alle registraties voor de sjabloon-GUID's worden uitgevoerd via de Azure Marketplace Cloud Partner Portal (CPP). 

Van toepassing op [Azure Marketplace](http://aka.ms/listonazuremarketplace) vandaag en krijgen toegang tot de Cloud Partner-portal.

*   Partners moeten [hebt u een profiel in CPP](https://docs.microsoft.com/azure/marketplace/become-publisher) en aangemoedigd om de aanbieding in Azure Marketplace of AppSource weer te geven 
*   Partners is mogelijk meerdere GUID's registreren 
*   Partners worden ook een GUID voor de oplossing voor niet-Marketplace-sjablonen/aanbiedingen registreren

Nadat u hebt de GUID wordt toegevoegd aan de sjabloon of in de gebruikersagent, en de GUID geregistreerd in het CPP alle implementaties worden bijgehouden. 

## <a name="verification-of-guid-deployment"></a>Verificatie van de GUID-implementatie 

Nadat u hebt uw sjabloon is gewijzigd en een test-implementatie wordt uitgevoerd, kunt u het volgende PowerShell-script kunt gebruiken om op te halen van de resources die u geïmplementeerd en gelabeld. 

U kunt deze gebruiken om te controleren of de GUID is toegevoegd aan uw Azure Resource Manager-sjabloon is. Dit geldt niet voor Azure Resource Manager API-implementatie.

Meld u aan bij Azure en selecteer het abonnement met de implementatie die u controleren wilt voordat u het script is uitgevoerd. Deze moet worden uitgevoerd binnen de context van het abonnement van de implementatie.

De GUID en de resourceGroup-naam van de implementatie zijn vereiste parameters.

U vindt het oorspronkelijke script [hier](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1).

```

Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName'
)

#get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

#find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

#find all deploymentOperations in a deployment by name (since PowerShell does not surface outputResources on the deployment or correlationId on the deploymentOperation)

foreach ($deployment in $deployments){

#get deploymentOperations by deploymentName and then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}


```

## <a name="guidance-on-creating-guids"></a>Informatie over het maken van GUID 's

Een GUID (globally unique identifier) is een uniek nummer 32 hexadecimale cijfers. Voor het maken van een GUID, moet u een GUID-generator te maken van hun GUID's voor het bijhouden.  Er zijn meerdere [online GUID generatoren](https://www.bing.com/search?q=guid%20generator&qs=n&form=QBRE&sp=-1&ghc=2&pq=guid%20g&sc=8-6&sk=&cvid=0BAFAFCD70B34E4296BB97FBFA3E1B4E) u kunt gebruiken.

U wordt aangeraden te maken van een unieke GUID voor elke aanbieding en de distributie-kanaal.  Bijvoorbeeld, hebt u twee oplossingen voor beide zijn geïmplementeerd via een sjabloon en zijn beschikbaar in de Azure Marketplace en op GitHub.  Vier GUID's maken:

*   Bieden van een in Azure Marketplace 
*   Bieden van een op GitHub
*   Bieden van B in Azure Marketplace 
*   Aanbieding B op GitHub

Reporting wordt gedaan door partner (Microsoft Partner-ID) en de GUID. 

U kunt ook kiezen voor het bijhouden van GUID's op een meer gedetailleerd niveau dat wil zeggen SKU (SKU's zijn waarbij varianten van een aanbieding).

## <a name="guidance-on-privacy-and-data-collection"></a>Informatie over privacy en gegevensverzameling

Partners moeten opgeven om te informeren over hun klanten die Microsoft voor het rapporteren van het gebruik van Azure die zijn gekoppeld aan deze implementaties met de partner-implementaties waarin het bijhouden van Azure Resource Manager-GUID, kunnen berichten.  Er is een voorbeeld van de taal hieronder. Indien deze 'Partners' geeft aan dat moet u de bedrijfsnaam van uw eigen invullen. Partners moeten bovendien voor zorgen dat de taal worden uitgelijnd met hun eigen gegevens beleidsregels voor privacy en verzameling, inclusief opties voor klanten moeten worden uitgesloten van bijhouden: 

**Voor sjabloonimplementaties van Azure Resource Manager**

Bij het implementeren van deze sjabloon, Microsoft is mogelijk de installatie van software van de PARTNER identificeren met de Azure-resources geïmplementeerd.  Microsoft is kunnen correleren van de Azure-resources gebruikt ter ondersteuning van de software.  Microsoft verzamelt deze informatie om de beste ervaringen met hun producten en om te werken van hun bedrijf. Deze gegevens worden verzameld en onderworpen aan het privacybeleid van Microsoft, die kunnen worden gevonden op https://www.microsoft.com/trustcenter. 

**Voor implementaties van SDK of -API**

Wanneer implementeren PARTNER van software, Microsoft mogelijk identificeert u de installatie van software van de PARTNER met de Azure-resources geïmplementeerd.  Microsoft is kunnen correleren van de Azure-resources gebruikt ter ondersteuning van de software.  Microsoft verzamelt deze informatie om de beste ervaringen met hun producten en om te werken van hun bedrijf. Deze gegevens worden verzameld en onderworpen aan het privacybeleid van Microsoft, die kunnen worden gevonden op https://www.microsoft.com/trustcenter.

## <a name="support"></a>Ondersteuning

Hulp nodig hebt, volgt u de onderstaande stappen te volgen:
 1. Ga naar de ondersteuningspagina te vinden op [go.microsoft.com/fwlink/?linkid=844975](https://go.microsoft.com/fwlink/?linkid=844975)
 2. Selecteer het probleemtype voor problemen met het gebruik van koppeling -: **Marketplace Onboarding** en categorie: **andere** en klik vervolgens op **aanvraag starten.** 
>[!Note]
>Voor problemen met toegang tot de Azure Marketplace Cloud Partner Portal - Selecteer probleemtype: **Marketplace Onboarding** en categorie: **probleem met toegang tot** en klik vervolgens op **aanvraag starten.**
 3. Vul de vereiste velden op de volgende pagina en klik op **doorgaan.**
 4. Vul de velden vrije tekst op de volgende pagina.  
 

 
>[!Important] 
>Vul in de titel van Incident met **'ISV gebruik bijhouden'** en beschrijf het probleem in de details in het veld grote vrije tekst na.  Voltooi de rest van het formulier en klikt u op **indienen**.

## <a name="faqs"></a>Veelgestelde vragen

**Wat is het voordeel van de GUID toe te voegen aan de sjabloon?**

Microsoft biedt partners met een weergave van implementaties van klanten hun sjablonen en inzichten op het gebruik van hun beïnvloed.  Zowel Microsoft als de partner kunnen u deze gegevens dichter bij betrokkenheid tussen verkoopteams ook gebruiken. Zowel Microsoft als de partner kunnen u deze ook gebruiken om op te halen van een consistente weergave van de impact van een afzonderlijke partner op Azure groei. 

**Wie kan een GUID toevoegen aan een sjabloon?**

De tracerings-resource is bedoeld om van de partner-oplossing verbinden met de Azure-klanten gebruik.  Gegevens over gebruik is gekoppeld aan de identiteit van een partner Microsoft Partner Network (MPN-ID) en rapportage is beschikbaar voor partners in de Cloud Partner Portal (CPP).  

**Zodra een GUID is toegevoegd kan worden gewijzigd?**
 
Ja, een partner of implementatie van de klant kan de sjabloon aanpassen en kan wijzigen of verwijderen de GUID. Het is raadzaam dat partners proactief de rol van de resource en de GUID voor hun klanten en partners beschrijven om te voorkomen dat de verwijdering of bewerkingen voor het bijhouden van GUID.  Als u de GUID wijzigt, wordt dit alleen van invloed op de nieuwe, niet-bestaande implementaties en resources.

**Wanneer reporting beschikbaar zal zijn?**

Een bètaversie van rapportage moet zijn binnenkort beschikbaar.  Reporting wordt geïntegreerd in de Cloud Partner Portal (CPP).

**Kan ik de sjablonen die zijn geïmplementeerd vanuit een niet-Microsoft-opslagplaats, zoals GitHub bijhouden?**

Ja, zolang de GUID gebruikt wordt als de sjabloon wordt geïmplementeerd, gebruik wordt bijgehouden.  
Partners is vereist om een profiel in de Cloud Partner-Portal voor het registreren van de sjablonen met betrekking tot buiten de Azure Marketplace worden gepubliceerd. 

**Is er een verschil als de sjabloon wordt geïmplementeerd op Azure Marketplace ten opzichte van andere opslagplaatsen zoals GitHub?**

Ja, partners die het publiceren van aanbiedingen in de Azure Marketplace ontvangt mogelijk meer gedetailleerde gegevens over implementaties van Azure Marketplace.  Partners profiteren van hun aanbieding aan klanten in de portal van Azure Marketplace en in de Azure-beheerportal om vrij te geven. Aanbiedingen in Azure marketplace een leads voor de partner.

**Wat gebeurt er als ik een aangepaste sjabloon voor een afzonderlijke klantcontacten maken?**

U bent nog steeds Welkom bij de GUID toevoegen aan de sjabloon.  Als u een bestaande GUID die is geregistreerd, worden deze opgenomen in rapporten.  Als u een nieuwe GUID maakt, moet u de nieuwe GUID om hiermee aan opgenomen in de tracering te registreren.

**De klant's ontvangen ook melden?**

Klanten kunnen op dit moment om bij te houden van hun gebruik van afzonderlijke resources of door de klant gedefinieerde resourcegroepen binnen de Azure management portal.   

**Is deze methodologie vergelijkbaar voor de digitale Partner van Record (DPOR)?**

Deze nieuwe methode voor het koppelen van de implementatie en het gebruik aan van een partner-oplossing is bedoeld om u te bieden een mechanisme om te koppelen van een partneroplossing aan het gebruik van Azure.  DPOR is bedoeld om een advies (systeemintegrator) of management (Managed Service Provider) werken samen met een klant Azure-abonnement te koppelen.   
