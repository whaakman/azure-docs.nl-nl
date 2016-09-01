<properties
   pageTitle="Overzicht van Azure Resource Manager | Microsoft Azure"
   description="Hierin wordt beschreven hoe u Azure Resource Manager kunt gebruiken voor implementatie, beheer, en beheer van toegang tot resources in Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="tomfitz"/>

# Overzicht van Azure Resource Manager

De infrastructuur voor uw toepassing bestaat meestal uit veel onderdelen, zoals een virtuele machine, een opslagaccount en een virtueel netwerk, of een webtoepassing, database, databaseserver en services van derden. Deze onderdelen moet u niet zien als afzonderlijke entiteiten, maar als onderdelen die één entiteit vormen en aan elkaar zijn gerelateerd en afhankelijk zijn van elkaar. U implementeert, beheert en bewaakt deze onderdelen als groep. Met Azure Resource Manager kunt u met de resources als groep in uw oplossing werken. U kunt alle resources voor uw oplossing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking. Voor implementatie gebruikt u een sjabloon. Deze sjabloon kan voor verschillende omgevingen worden gebruikt, zoals testen, faseren en productie. Resource Manager biedt beveiliging, controle en tagfuncties die u na de implementatie helpen bij het beheren van uw resources. 

## Terminologie

Als u nog geen ervaring hebt met de Azure Resource Manager, zijn er enkele termen die u mogelijk niet kent.

- **resource** - een beheerbaar item dat beschikbaar is via Azure. Sommige algemene resources zijn een virtuele machine, opslagaccount, webtoepassing en virtueel netwerk, maar er zijn er veel meer.
- **resourcegroep** - een container met gerelateerde resources voor een toepassing. De resourcegroep kan alle resources voor een applicatie of enkel de resources die u groepeert bevatten. U kunt bepalen hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. Zie [Resourcegroepen](#resource-groups).
- **resourceprovider** - Een service die de resources levert die u kunt implementeren en beheren met de Resource Manager. Elke resourceprovider biedt bewerkingen voor het werken met de resources die zijn geïmplementeerd. Enkele veelvoorkomende resourceproviders zijn Microsoft.Compute, dat voor de resource van de virtuele machine zorgt, Microsoft.Storage dat voor de resource van de opslagaccount zorgt, en Microsoft.Web, dat zorgt voor resources met betrekking tot web-apps. Zie [Resourceproviders](#resource-providers).
- **Resource Manager sjabloon** - Een JavaScript Object Notation (JSON)-bestand dat één of meer resources definieert die worden geïmplementeerd op een resourcegroep. Het definieert ook de afhankelijkheden tussen de geïmplementeerde resources. De sjabloon kan worden gebruikt om de resources consistent en herhaaldelijk te implementeren. Zie [Sjabloonimplementatie](#template-deployment).
- **declaratieve syntaxis** - Een syntaxis waarmee u kunt aangeven “Dit is wat ik wil maken” zonder hiervoor de nodige sequentie programmeeropdrachten te hoeven maken. De sjabloon Resource Manager is een voorbeeld van een declaratieve syntaxis. In het bestand definieert u de eigenschappen voor de infrastructuur te implementeren naar Azure. 

## De voordelen van Resource Manager

Resource Manager biedt diverse voordelen:

- U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.
- U kunt gedurende de ontwikkelingscyclus uw oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.
- U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.
- U kunt de afhankelijkheden tussen resources zo definiëren dat deze in de juiste volgorde worden geïmplementeerd.
- U kunt toegangsbeheer toepassen op alle services in de resourcegroep omdat op rollen gebaseerd toegangsbeheer (RBAC) is geïntegreerd in het beheerplatform.
- U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.
- U kunt facturering voor uw organisatie verduidelijken door de samengevoegde kosten voor de hele groep weer te geven of voor een groep resources die dezelfde tag delen.  

Resource Manager biedt een nieuwe manier om uw oplossingen te implementeren en te beheren. Als u het eerdere implementatiemodel hebt gebruikt en meer te weten wilt komen over de wijzigingen, leest u [Resource Manager-implementatie en klassieke implementatie begrijpen](resource-manager-deployment-model.md).

## Richtlijnen

Met de volgende tips kunt u profiteren van alle mogelijkheden die Resource Manager voor uw oplossingen biedt.

1. Definieer en implementeer uw infrastructuur via de declaratieve syntaxis in de Resource Manager-sjablonen, in plaats van via imperatieve opdrachten.
2. Definieer alle implementatie- en configuratiestappen in de sjabloon. Handmatige stappen voor het installeren van uw oplossing zijn niet nodig.
3. Voer imperatieve opdrachten uit voor het beheren van uw resources, zoals het starten of stoppen van een app of machine.
4. Breng resources met dezelfde levenscyclus onder in een resourcegroep. Gebruik tags voor het organiseren van alle andere resources.

Zie voor meer aanbevelingen [Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](resource-manager-template-best-practices.md).

## Resourcegroepen

Er zijn een aantal belangrijke factoren waarmee u rekening moet houden bij het definiëren van de resourcegroep:

1. Alle resources in uw groep moeten dezelfde levenscyclus hebben. U implementeert ze samen, werkt ze samen bij en verwijdert ze samen. Als een resource, zoals een databaseserver, in een andere implementatiecyclus moet werken, moet deze in een andere resourcegroep worden geplaatst.
2. Elke resource kan in slechte één resourcegroep voorkomen.
3. U kunt een resource op elk gewenst moment toevoegen aan of verwijderen uit een resourcegroep.
4. U kunt een resource van de ene naar de andere resourcegroep verplaatsen. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](resource-group-move-resources.md).
4. Een resourcegroep kan resources uit verschillende regio’s bevatten.
5. Een resourcegroep kan worden gebruikt voor het bepalen van de mate van toegangsbeheer voor beheertaken.
6. Een resource kan communiceren met een resource in een andere resourcegroep als de twee resources met elkaar verbonden zijn, maar niet dezelfde levenscyclus delen (bijvoorbeeld web-apps die verbinding maken met een database).

## Resourceproviders

Elke resourceprovider biedt een set resources en bewerkingen voor werken met technisch gebied. Als u bijvoorbeeld sleutels en geheimen wilt opslaan, werkt u met de resourceprovider **Microsoft.KeyVault**. Deze resourceprovider biedt een resourcetype genaamd **kluizen** voor het maken van de sleutelkluis en een resourcetype genaamd **kluizen/geheimen** voor het maken van een geheim in de sleutelkluis. Bovendien vindt u bewerkingen via [Key Vault REST API-bewerkingen](https://msdn.microsoft.com/library/azure/dn903609.aspx). U kunt rechtstreeks contact opnemen met de REST-API of u kunt de [Key Vault PowerShell cmdlets](https://msdn.microsoft.com/library/dn868052.aspx) en [Key Vault Azure CLI](./key-vault/key-vault-manage-with-cli.md) gebruiken om de Key Vault te beheren. U kunt ook een aantal programmeertalen gebruiken om te werken met de meeste resources. Zie [SDKs en voorbeelden](#sdks-and-samples) voor meer informatie. 

Als u uw infrastructuur wilt implementeren en beheren, moet u gedetailleerde kennis hebben van de resourceproviders. Zoals welke resourcetypen de provider biedt, het versienummer van de REST-API-bewerkingen, de bewerkingen die worden ondersteund, en het schema dat moet worden gebruikt bij het instellen van de waarden voor het te maken resourcetype. Zie voor meer informatie over de ondersteunde resourceproviders [Resource Manager-providers, regio's, API-versies en schema's](resource-manager-supported-services.md).

## Sjabloonimplementatie

Met Resource Manager kunt u een eenvoudige sjabloon maken (in JSON-indeling) waarin de implementatie en configuratie van uw toepassing worden gedefinieerd. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus van uw app de app herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd. Azure Resource Manager analyseert afhankelijkheden om ervoor te zorgen dat de resources in de juiste volgorde worden gemaakt. Zie voor meer informatie [Afhankelijkheden definiëren in Azure Resource Manager-sjablonen](resource-group-define-dependencies.md).

Wanneer u een oplossing vanaf de portal maakt, bevat de oplossing automatisch een sjabloon voor de implementatie. U hoeft de sjabloon niet helemaal zelf te maken. U kunt beginnen met de sjabloon voor uw oplossing en deze aanpassen aan uw specifieke behoeften. U kunt een sjabloon voor een bestaande resourcegroep ophalen door de huidige status van de resourcegroep te exporteren naar een sjabloon of door de sjabloon die is gebruikt voor een bepaalde implementatie, weer te geven. Raadplegen van de geëxporteerde sjabloon is een handige manier om de syntaxis van de sjabloon te leren kennen. Zie voor meer informatie over het werken met geëxporteerde sjablonen [Een Azure Resource Manager-sjabloon uit bestaande resources exporteren](resource-manager-export-template.md).

U hoeft niet uw volledige infrastructuur in één sjabloon te definiëren. Vaak is het handiger om uw implementatievereisten te verdelen over een aantal gerichte sjablonen met een specifiek doel. U kunt deze sjablonen eenvoudig opnieuw gebruiken voor verschillende oplossingen. Voor het implementeren van een bepaalde oplossing kunt u een basissjabloon gebruiken die is gekoppeld aan alle vereiste sjablonen. Zie voor meer informatie [Gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).

U kunt de sjabloon ook gebruiken voor updates aan de infrastructuur. U kunt bijvoorbeeld een nieuwe resource toevoegen aan uw app en configuratieregels toevoegen voor de resources die al zijn geïmplementeerd. Als de sjabloon een nieuwe resource specificeert, maar de betreffende resource al bestaat, wordt er geen nieuwe asset gemaakt. In plaats daarvan voert Azure Resource Manager een update uit. Azure Resource Manager werkt de bestaande asset bij naar dezelfde toestand als een nieuwe asset. U kunt ook opgeven dat Resource Manager alle resources moet verwijderen die niet zijn opgegeven in de sjabloon. Zie [Een toepassing implementeren met de Azure Resource Manager-sjabloon](resource-group-template-deploy.md) voor inzicht in de verschillende opties bij het implementeren. 

U kunt in de sjabloon specifieke parameters opgeven, zodat er aanpassingen kunnen worden gemaakt en er in de implementatie flexibiliteit bestaat. U kunt bijvoorbeeld parameterwaarden doorgeven die de implementatie aanpassen aan uw testomgeving. Door de parameters op te geven, kunt u dezelfde sjabloon voor implementatie gebruiken in alle omgevingen van uw app.

Resource Manager biedt uitbreidingen voor scenario's waarin aanvullende bewerkingen moeten worden uitgevoerd, zoals het installeren van bepaalde software die niet is opgenomen in de installatie. Als u al een configuratiebeheerservice gebruikt, zoals DSC, Chef of Puppet, kunt u via uitbreidingen met deze service blijven werken.

De sjabloon wordt uiteindelijk onderdeel van de broncode van uw app. U kunt de broncode inchecken in uw broncodeopslag en deze bijwerken naarmate uw app zich verder ontwikkelt. U kunt de sjabloon bewerken met Visual Studio.

Zie voor meer informatie over het definiëren van sjablonen [Azure Resource Manager-sjablonen ontwerpen](resource-group-authoring-templates.md).

Zie voor stapsgewijze instructies over het maken van een sjabloon [Overzicht voor Resource Manager-sjabloon](resource-manager-template-walkthrough.md).

Zie voor instructies over het implementeren van uw oplossing in verschillende omgevingen [Ontwikkeling en testomgevingen in Microsoft Azure](solution-dev-test-environments.md). 

## Tags

Resource Manager biedt een tagfunctie waarmee u resources kunt categoriseren volgens uw vereisten voor beheer of facturering. Het is verstandig om tags te gebruiken wanneer u een verzameling complexe resourcegroepen en resources hebt en u deze assets op een handige manier moet visualiseren. U kunt bijvoorbeeld resources taggen die een vergelijkbare rol hebben in uw organisatie of bij dezelfde afdeling horen. Zonder tags kunnen gebruikers in uw organisatie meerdere resources maken die later mogelijk moeilijk zijn te identificeren en te beheren. Als u bijvoorbeeld alle resources voor een bepaald project wilt verwijderen, moet u deze handmatig zoeken als deze resources niet voor het project zijn getagd. Taggen kan een belangrijke manier zijn om onnodige kosten in uw abonnement te voorkomen. 

Resources hoeven niet tot dezelfde resourcegroep te behoren om een tag te kunnen delen. U kunt uw eigen tagtaxonomie maken om ervoor te zorgen dat alle gebruikers in uw organisatie gebruikmaken van veelgebruikte tags en niet per ongeluk afwijkende tags toepassen (zoals 'afd' in plaats van 'afdeling').

Zie voor meer informatie over tags [Tags gebruiken om uw Azure-resources te organiseren](resource-group-using-tags.md). U kunt een [aangepast beleid](#manage-resources-with-customized-policies) maken dat vereist dat er tijdens de implementatie tags aan resources worden toegevoegd.

## Toegangsbeheer

Met Resource Manager kunt u bepalen wie er toegang heeft tot bepaalde acties voor uw organisatie. Op open authorization en rollen gebaseerd toegangsbeheer (RBAC) is in het beheerplatform geïntegreerd. Resource Manager past dat toegangsbeheer toe op alle services in uw resourcegroep. U kunt gebruikers toevoegen aan vooraf gedefinieerde platformrollen en resourcespecifieke rollen, en deze rollen toepassen op een abonnement, resourcegroep of resource om de toegang te beperken. U kunt bijvoorbeeld gebruikmaken van de vooraf gedefinieerde rol met de naam 'SQL DB Contributor' waarmee gebruikers wel databases kunnen beheren, maar geen databaseservers of beveiligingsbeleid. U voegt gebruikers in uw organisatie toe die dit type toegang tot de rol 'SQL DB Contributor' nodig hebben en past de rol toe op het abonnement, de resourcegroep of de resource.

Resource Manager registreert ter controle automatisch acties van gebruikers. Zie voor meer informatie over het werken met de controlelogboeken [Bewerkingen controleren met Resource Manager](resource-group-audit.md).

Zie voor meer informatie over op rollen gebaseerd toegangsbeheer [Op rollen gebaseerd Azure-toegangsbeheer](./active-directory/role-based-access-control-configure.md). Het onderwerp [RBAC: ingebouwde rollen](./active-directory/role-based-access-built-in-roles.md) biedt een overzicht van de ingebouwde rollen en de toegestane acties. Tot de ingebouwde rollen behoren algemene rollen, zoals Eigenaar, Lezer en Inzender, maar ook servicespecifieke rollen, zoals Virtual Machine Contributor, Virtual Network Contributor en SQL Security Manager (om een paar van de beschikbare rollen te noemen).

U kunt kritieke resources ook expliciet vergrendelen om te voorkomen dat gebruikers deze wijzigen of verwijderen. Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](resource-group-lock-resources.md).

Zie voor aanbevolen procedures [Beveiligingsoverwegingen voor Azure Resource Manager](best-practices-resource-manager-security.md)

## Beheer van resources met aangepast beleid

Met Resource Manager kunt u aangepaste beleidsregels maken voor het beheer van resources. De typen beleid die u maakt, kunnen uit verschillende scenario’s bestaan, zoals het afdwingen van een naamgevingsconventie voor resources, het beperken van de typen en exemplaren van resources die kunnen worden geïmplementeerd, het beperken van de regio's die als host kunnen fungeren voor een type resource, of het vereisen van een tagwaarde voor resources om facturering op afdelingsniveau mogelijk te maken. U maakt beleid om kostenverlaging te stimuleren en consistentie binnen uw abonnement te waarborgen. Zie voor meer informatie [Beleid gebruiken voor het beheren van resources en toegang](resource-manager-policy.md).

## Consistente beheerlaag

Resource Manager biedt volledig compatibele bewerkingen via Azure PowerShell, Azure CLI voor Mac, Linux en Windows, de Azure Portal of REST-API. U kunt de interface gebruiken die voor u het meest geschikt is en zonder problemen snel wisselen tussen de interfaces. De portal geeft zelfs meldingen weer voor acties die worden uitgevoerd buiten de portal.

Zie voor meer informatie over PowerShell [Azure PowerShell gebruiken met Resource Manager](powershell-azure-resource-manager.md) en [Azure Resource Manager-cmdlets](https://msdn.microsoft.com/library/azure/dn757692.aspx).

Zie voor informatie over Azure CLI [De Azure CLI voor Mac, Linux en Windows gebruiken met Azure-resource-management](xplat-cli-azure-resource-manager.md).

Zie voor meer informatie over de REST-API [Naslaginformatie over REST API voor Azure Resource Manager](https://msdn.microsoft.com/library/azure/dn790568.aspx). Zie voor het weergeven van REST-bewerkingen voor uw geïmplementeerde resources [Azure Resource Explorer gebruiken voor het weergeven en wijzigen van resources](resource-manager-resource-explorer.md).

Zie voor meer informatie over het gebruik van de portal [Resources implementeren met Resource Manager-sjablonen en Azure Portal](resource-group-template-deploy-portal.md).

Azure Resource Manager biedt ondersteuning voor het delen van cross-origin-resources (CORS). Met CORS kunt u de REST-API van Resource Manager of een REST-API van een Azure-service aanroepen vanuit een webtoepassing die zich in een ander domein bevindt. Zonder CORS-ondersteuning zou de webbrowser voorkomen dat een app in één domein toegang zou krijgen tot resources in een ander domein. Resource Manager gebruikt CORS voor alle aanvragen met geldige verificatiereferenties.

## SDK's en voorbeelden

Azure SDK's zijn beschikbaar voor meerdere talen en platforms.
Elk van deze taalimplementaties is beschikbaar via zijn ecosystem package manager en GitHub.

De code in elk van deze SDK's wordt gegenereerd op basis van Azure RESTful-API-specificaties.
Deze specificaties zijn open source en gebaseerd op de Swagger 2.0-specificatie.
De SDK-code wordt gegenereerd via een open source-project met de naam AutoRest.
AutoRest transformeert deze RESTful-API-specificaties in clientbibliotheken in meerdere talen.
Als u aspecten van de gegenereerde code in de SDK’s wilt verbeteren, is er een open, gratis beschikbare set hulpprogramma’s om SDK’s te maken, gebaseerd op een algemeen aangenomen API-specificatieformaat.

**Voorbeelden**: snel aan de slag in de taal van uw keuze.

- [.NET](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=dotnet)
- [Java](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=java)
- [Node.js](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=nodejs)
- [Python](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=python)
- [PHP](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=php) *binnenkort beschikbaar*
- [Ruby](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=ruby)

**Open Source SDK-opslagplaatsen**: we verwelkomen feedback, problemen en Pull-aanvragen.

- [.NET](https://github.com/Azure/azure-sdk-for-net)
- [Java](https://github.com/Azure/azure-sdk-for-java)
- [Node.js](https://github.com/Azure/azure-sdk-for-node)
- [PHP](https://github.com/Azure/azure-sdk-for-php)
- [Python](https://github.com/Azure/azure-sdk-for-python)
- [Ruby](https://github.com/Azure/azure-sdk-ruby)

> [AZURE.NOTE] Als de SDK niet de vereiste functionaliteit biedt, kunt u ook de [Azure REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) ook rechtstreeks aanroepen.

## Volgende stappen

- Zie voor een eenvoudige introductie in het werken met sjablonen [Een Azure Resource Manager-sjabloon uit bestaande resources exporteren](resource-manager-export-template.md).
- Zie voor een uitgebreider overzicht van het maken van een sjabloon [Overzicht voor Resource Manager-sjabloon](resource-manager-template-walkthrough.md).
- Zie [Sjabloonfuncties](resource-group-template-functions.md) voor inzicht in de functies die u in een sjabloon kunt gebruiken.
- Zie voor meer informatie over het gebruik van Visual Studio met Resource Manager [Azure-resourcegroepen maken en implementeren met Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
- Zie voor meer informatie over het gebruik van VS Code met Resource Manager [werken met Azure Resource Manager-sjablonen in Visual Studio-Code](resource-manager-vs-code.md).

Hier volgt een videodemonstratie van dit overzicht:

[AZURE.VIDEO azure-resource-manager-overview]



<!--HONumber=ago16_HO4-->


