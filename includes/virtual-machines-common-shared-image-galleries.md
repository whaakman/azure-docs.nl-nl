---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/06/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 4063e79a9415ac35b09cc77d0110c04e191b49c7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145879"
---
Gedeelde afbeeldingengalerie is een service waarmee u structuur en de organisatie om uw aangepaste beheerde VM-installatiekopieën maken. Gedeelde Afbeeldingsgalerieën bieden:

- Beheerde globale replicatie van afbeeldingen.
- Versiebeheer en groepering van beelden voor eenvoudiger beheer.
- Uw installatiekopieën maximaal beschikbaar met Zone-redundante opslag (ZRS)-accounts maken in regio's die ondersteuning voor Beschikbaarheidszones. ZRS biedt betere tolerantie tegen fouten in de zonegebonden.
- Delen tussen abonnementen en zelfs tussen tenants, met RBAC.

U kunt met behulp van de galerie met installatiekopieën van een gedeelde afbeeldingen aan andere gebruikers, service-principals of AD-groepen delen binnen uw organisatie. Gedeelde afbeeldingen kunnen worden gerepliceerd naar meerdere regio's voor het sneller schalen van uw implementaties.

Een beheerde installatiekopie is een kopie van een volledige VM (inclusief eventuele gekoppelde gegevensschijven) of alleen de besturingssysteemschijf, afhankelijk van hoe u de installatiekopie maken. Wanneer u een virtuele machine van de installatiekopie maakt, wordt een kopie van de VHD's in de afbeelding worden gebruikt voor het maken van de schijven voor de nieuwe virtuele machine. De installatiekopie van het beheerde blijft in de opslag en telkens opnieuw kan worden gebruikt om nieuwe virtuele machines maken.

Als u een groot aantal beheerde installatiekopieën die u moet onderhouden en wil graag zodat ze beschikbaar zijn in uw organisatie hebt, kunt u de galerie met installatiekopieën van een gedeeld als een opslagplaats waarmee u eenvoudig voor het delen van uw installatiekopieën. 

De galerie met installatiekopieën van gedeelde functie heeft meerdere resourcetypen:

| Resource | Description|
|----------|------------|
| **Beheerde installatiekopie** | Een basic-installatiekopie die kan worden alleen wordt gebruikt of worden gebruikt voor het maken van een **installatiekopieversie** in een galerie met installatiekopieën. Beheerde installatiekopieën zijn gemaakt op basis van gegeneraliseerde VM's. Een beheerde installatiekopie is een speciaal type VHD die kan worden gebruikt voor het maken van meerdere virtuele machines en kan nu worden gebruikt om gedeelde installatiekopie versies te maken. |
| **Galerie met installatiekopieën** | De Azure Marketplace, zoals een **afbeeldingengalerie** is een opslagplaats voor het beheren en delen van afbeeldingen, maar u bepaalt wie toegang heeft. |
| **De definitie van installatiekopie** | Afbeeldingen worden gedefinieerd in een galerie en informatie over de installatiekopie en de vereisten voor het gebruik van deze binnen uw organisatie uitvoeren. U kunt opnemen, zoals of de afbeelding Windows of Linux, minimale en maximale hoeveelheid geheugenvereisten is, en opmerkingen bij de release. Het is een definitie van een type van de installatiekopie. |
| **De versie van installatiekopie** | Een **installatiekopieversie** is wat u gebruikt om een virtuele machine maken wanneer u een galerie. U kunt meerdere versies van een installatiekopie kunt hebben, indien nodig voor uw omgeving. Een beheerde installatiekopie, bijvoorbeeld wanneer u een **installatiekopieversie** voor het maken van een virtuele machine, de versie van de installatiekopie die wordt gebruikt voor het maken van nieuwe schijven voor de virtuele machine. Versies van een installatiekopie kunnen meerdere keren worden gebruikt. |

<br>


![Afbeelding die laat zien hoe u meerdere versies van een installatiekopie kunt hebben in de galerie](./media/shared-image-galleries/shared-image-gallery.png)

## <a name="image-definitions"></a>installatiekopiedefinities

Definities van de installatiekopie van zijn een logische groepering van versies van een afbeelding. Definitie van de installatiekopie bevat informatie over waarom de installatiekopie is gemaakt, welke OS voor en informatie over het gebruik van de installatiekopie. De definitie van een installatiekopie is vergelijkbaar met een plan voor alle gegevens over het maken van een specifieke installatiekopie. U kunt een virtuele machine van de definitie van een installatiekopie, maar vanaf de versie van de installatiekopie gemaakt op basis van de definitie niet implementeren.


Er zijn drie parameters voor de definitie van de installatiekopie die worden gebruikt in combinatie - **Publisher**, **bieden** en **SKU**. Deze worden gebruikt om de definitie van een specifieke installatiekopie te vinden. U kunt de versies van een installatiekopie die delen van één of twee, maar niet alle drie waarden hebben.  Dit zijn bijvoorbeeld drie definities die installatiekopie en de bijbehorende waarden:

|Definitie van installatiekopie|Uitgever|Aanbieding|Sku|
|---|---|---|---|
|myImage1|Contoso|Financiën|Back-end|
|myImage2|Contoso|Financiën|Front-end|
|myImage3|Testen|Financiën|Front-end|

Alle drie deze hebben unieke sets waarden. De indeling is vergelijkbaar met hoe u op dit moment opgeven uitgever, aanbieding en SKU voor de kunt [Azure Marketplace-installatiekopieën](../articles/virtual-machines/windows/cli-ps-findimage.md) in Azure PowerShell om op te halen van de meest recente versie van een Marketplace-installatiekopie. De definitie van elke afbeelding moet een unieke set van deze waarden hebben.

Hier volgen andere parameters die kunnen worden ingesteld voor de definitie van uw installatiekopie, zodat u resources eenvoudiger kunt volgen:

* Status van besturingssysteem - u kunt de status van het besturingssysteem instellen op gegeneraliseerd of gespecialiseerde, maar alleen gegeneraliseerd wordt momenteel ondersteund. Installatiekopieën van virtuele machines die is gegeneraliseerd met Sysprep voor Windows moeten worden gemaakt of `waagent -deprovision` voor Linux.
* Besturingssysteem - kan Windows of Linux zijn.
* Beschrijving - Gebruik beschrijving geven u meer gedetailleerde informatie over waarom de definitie van de installatiekopie bestaat. Bijvoorbeeld, mogelijk hebt u de definitie van een installatiekopie voor de front-endwebserver waarop de toepassing die vooraf zijn geïnstalleerd.
* EULA - kan worden gebruikt om te verwijzen naar een gebruiksrechtovereenkomst die specifiek zijn voor de definitie van de installatiekopie.
* Opmerkingen bij privacyverklaring en Release - opmerkingen bij de release en privacyverklaringen opslaan in Azure storage en geeft u een URI voor de toegang tot deze als onderdeel van de definitie van de installatiekopie.
* Einde van de levenscyclus datum: een datum einde van de levenscyclus koppelen aan de definitie van de afbeelding om te kunnen automation gebruiken om te verwijderen van oude installatiekopie definities.
* Tag - kunt u de labels toevoegen bij het maken van de definitie van uw installatiekopie. Zie voor meer informatie over tags [tags gebruiken om uw resources te organiseren](../articles/azure-resource-manager/resource-group-using-tags.md)
* Minimale en maximale vCPU en aanbevelingen voor geheugen - als uw installatiekopie de aanbevelingen voor de vCPU en geheugen heeft, u kunt koppelen die informatie aan de definitie van uw installatiekopie.
* Niet-toegestane schijftypen - u kunt informatie over de benodigde opslag voor uw virtuele machine opgeven. Bijvoorbeeld, als de installatiekopie is niet voor standaard harde schijven geschikt, toevoegen u deze aan de lijst niet toestaan.


## <a name="regional-support"></a>Regionale ondersteuning

Bron-regio's worden weergegeven in de onderstaande tabel. Alle openbare regio's kunnen doelregio's, maar als u wilt repliceren naar Australië-centraal en Australië centraal 2 moet u beschikken over uw abonnement in de whitelist opgenomen. Om aan te vragen in een whitelist opnemen, gaat u naar: https://www.microsoft.com/en-au/central-regions-eligibility/


| Bron-regio 's |
|---------------------|-----------------|------------------|-----------------|
| Australië - centraal   | US - centraal EUAP | Korea Centraal    | VK Zuid 2      |
| Australië - centraal 2 | Azië - oost       | Korea - zuid      | VK West         |
| Australië - oost      | US - oost         | US - noord-centraal | US - west-centraal |
| Australië - zuidoost | US - oost 2       | Europa - noord     | Europa -west     |
| Brazilië - zuid        | US - oost 2 EUAP  | US - zuid-centraal | India - west      |
| Canada - midden      | Frankrijk - centraal  | India - zuid      | US - west         |
| Canada Oost         | Frankrijk - zuid    | Azië - zuidoost   | US - west         |
| India - centraal       | Japan - oost      | VK Noord         | US - west 2       |
| US - centraal          | Japan - west      | Verenigd Koninkrijk Zuid         |                 |



## <a name="limits"></a>Limits 

Er zijn limieten per abonnement, voor het implementeren van resources met behulp van gedeelde Afbeeldingsgalerieën:
- 100 gedeelde afbeeldingsgalerieën per abonnement per regio
- 1000 definities van de installatiekopie, per abonnement per regio
- 10.000 versies van een installatiekopie, per abonnement per regio

Zie voor meer informatie, [brongebruik op basis van limieten controleren](https://docs.microsoft.com/azure/networking/check-usage-against-limits) voor voorbeelden over het controleren van uw huidige gebruik.
 

## <a name="scaling"></a>Schalen
Gedeelde galerie met installatiekopieën kunt u het aantal replica's die u wilt dat Azure te houden van de installatiekopieën opgeven. Dit helpt bij implementatiescenario's voor meerdere VM's als de VM-implementaties kunnen worden verspreid naar verschillende replica's verminderen de kans dat het maken van de instantie wordt beperkt vanwege een overbelasting van een enkele replica verwerken.

![Afbeelding die laat zien hoe u installatiekopieën kunt schalen](./media/shared-image-galleries/scaling.png)


## <a name="replication"></a>Replicatie
Gedeelde galerie met installatiekopieën kunt u uw installatiekopieën automatisch naar andere Azure-regio's repliceren. De versie van elke installatiekopie gedeeld kan worden gerepliceerd naar verschillende regio's, afhankelijk van wat zinvol is voor uw organisatie. Een voorbeeld hiervan is altijd de meest recente installatiekopie repliceren in meerdere regio's, terwijl alle oudere versies alleen beschikbaar in de regio 1 zijn. Dit kan helpen opslaan op de opslagkosten voor versies van een gedeelde installatiekopie. 

De regio's voor die de versie van een gedeelde installatiekopie wordt gerepliceerd naar kunnen worden bijgewerkt na de aanmaaktijd. De tijd die nodig zijn om te repliceren naar verschillende regio's, is afhankelijk van de hoeveelheid gegevens wordt gekopieerd en het aantal regio's die worden gerepliceerd naar de versie. Dit kan enkele uren duren in sommige gevallen. Tijdens de replicatie plaatsvindt, kunt u de status van replicatie weergeven per regio. Nadat de installatiekopie-replicatie voltooid in een regio is, kunt u vervolgens een virtuele machine of een schaalset met behulp van de versie van die installatiekopie in de regio implementeren.

![Afbeelding die laat zien hoe u installatiekopieën kunt repliceren](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Toegang

Als de galerie met installatiekopieën van gedeelde, gedeelde installatiekopie en de versie van gedeelde installatiekopie moet u alle resources zijn, kunnen ze worden gedeeld met de ingebouwde die systeemeigen Azure RBAC bepaalt. Met RBAC kunt u deze resources aan andere gebruikers, service-principals en -groepen delen. U kunt ook toegang tot personen buiten de tenant die zijn gemaakt in delen. Wanneer een gebruiker toegang tot de versie van de installatiekopie gedeeld heeft, kunnen ze een virtuele machine of een virtuele-Machineschaalset implementeren.  Hier volgt de delen matrix die helpt te begrijpen wat de gebruiker krijgt toegang tot:

| Gedeeld met gebruiker     | Gedeelde installatiekopiegalerie | Gedeelde installatiekopie | De versie van gedeelde installatiekopie |
|----------------------|----------------------|--------------|----------------------|
| Gedeelde installatiekopiegalerie | Ja                  | Ja          | Ja                  |
| Gedeelde installatiekopie         | Nee                   | Ja          | Ja                  |
| De versie van gedeelde installatiekopie | Nee                   | Nee           | Ja                  |

Het is raadzaam om eerst te delen op het niveau van de galerie voor de beste ervaring. Zie voor meer informatie over RBAC [beheren van toegang tot Azure-resources met RBAC](../articles/role-based-access-control/role-assignments-portal.md).

Afbeeldingen kunnen ook worden gedeeld, op schaal, met tenants met behulp van de registratie van een app met meerdere tenants. Zie voor meer informatie over het delen van installatiekopieën voor tenants [galerie VM-installatiekopieën in Azure-tenants delen](../articles/virtual-machines/linux/share-images-across-tenants.md).

## <a name="billing"></a>Facturering
Er zijn geen extra kosten verbonden aan het gebruik van de Shared Image Gallery-service. U wordt gefactureerd voor de volgende resources:
- Kosten voor opslag om de versies van een gedeelde installatiekopie op te slaan. Kosten zijn afhankelijk van het aantal replica's van de versie van de installatiekopie en het aantal regio's die worden gerepliceerd naar de versie. Bijvoorbeeld, als u installatiekopieën van 2 en beide zijn gerepliceerd naar 3 regio's, zal vervolgens u worden gewijzigd voor 6 beheerde schijven op basis van de grootte. Zie voor meer informatie, [Managed Disks-prijzen](https://azure.microsoft.com/pricing/details/managed-disks/).
- Kosten voor uitgaand verkeer voor de replicatie van de eerste installatiekopieversie van de regio van de gegevensbron naar de gerepliceerde regio's op het netwerk. Volgende replica's worden verwerkt binnen de regio, zodat er geen extra kosten zijn. 

## <a name="updating-resources"></a>De resources worden bijgewerkt

Zodra u hebt gemaakt, kunt u enkele wijzigingen aan de resources van de galerie met afbeelding. Dit zijn beperkt tot:
 
Galerie met installatiekopieën van de gedeelde:
- Description

de definitie van de installatiekopie:
- Aanbevolen vcpu 's
- Aanbevolen geheugen
- Description
- Einde van de levensduur van datum

Installatiekopieversie:
- Aantal regionale replica 's
- Doelregio's
- Uitsluiting van de meest recente
- Einde van de levensduur van datum


## <a name="sdk-support"></a>SDK-ondersteuning

De volgende SDK's bieden ondersteuning voor het maken van gedeelde Afbeeldingsgalerieën:

- [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/virtualmachines/management?view=azure-dotnet)
- [Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable)
- [Node.js](https://docs.microsoft.com/javascript/api/azure-arm-compute/?view=azure-node-latest)
- [Python](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)
- [Go](https://docs.microsoft.com/go/azure/)

## <a name="templates"></a>Sjablonen

U kunt de galerie met installatiekopieën van gedeelde resources met behulp van sjablonen maken. Er zijn verschillende Azure-Snelstartsjablonen beschikbaar: 

- [Een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [De definitie van een installatiekopie in een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [De versie van een installatiekopie in een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Een virtuele machine maken vanaf de versie van installatiekopie](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen 

**V:** Hoe kan ik weergeven van alle galerie met installatiekopieën van gedeelde resources tussen abonnementen? 
 
 A. Als u wilt weergeven van alle galerie met installatiekopieën van gedeelde resources tussen abonnementen dat u toegang tot op de Azure-portal hebben, de volgende stappen uit te voeren:

1. Open de [Azure Portal](https://portal.azure.com).
1. Ga naar **alle Resources**.
1. Selecteer alle abonnementen op grond waarvan u wilt weergeven van alle resources.
1. Zoeken naar resources van het type **privégalerie**.
 
   Als u wilt zien de definities van de installatiekopie en de versies van een installatiekopie, moet u ook selecteren **verborgen typen weergeven**.
 
   Als u de galerie met installatiekopieën van gedeelde resources tussen abonnementen waarvoor u machtigingen hebt, gebruik de volgende opdracht in de Azure CLI:

   ```bash
   az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
   ```


**V:** Kan ik mijn bestaande installatiekopie verplaatsen naar de galerie met installatiekopieën van de gedeelde?
 
 A. Ja. Er zijn 3 's op basis van de typen installatiekopieën die u hebt.

 Scenario 1: Als u een beheerde installatiekopie hebt, kunt klikt u vervolgens u een definitie van de installatiekopie en de versie van installatiekopie uit.

 Scenario 2: Als u een niet-beheerde gegeneraliseerde installatiekopie hebt, kunt u een beheerde installatiekopie maken op basis van deze en maakt u een definitie van de installatiekopie en de versie van installatiekopie van het. 

 Scenario 3: Als u een VHD in het lokale bestandssysteem hebt, moet u de VHD te uploaden, een beheerde installatiekopie maken en vervolgens kunt u maken en definitie- en image-versie van deze afbeelding.
- Als de VHD van een Windows-VM, Zie [een gegeneraliseerde VHD uploaden](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
- Als de VHD voor een Linux-VM is, Zie [een VHD uploaden](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd)


**V:** Kan ik de versie van een installatiekopie maken van een gespecialiseerde schijf?

 A. Nee, we op dit moment ondersteunen geen gespecialiseerde schijf als afbeeldingen. Als u een gespecialiseerde schijf hebt, moet u [een virtuele machine maken van de VHD](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) door de gespecialiseerde schijf koppelen aan een nieuwe virtuele machine. Nadat u een actieve virtuele machine hebt, moet u Volg de instructies voor het maken van een beheerde installatiekopie van het [Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) of [Linux-VM](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Zodra u een gegeneraliseerde installatiekopie van de beheerde hebt, kunt u het proces voor het maken van een gedeelde installatiekopie beschrijving en de versie van installatiekopie kunt starten.

 
**V:** Eenmaal gemaakt, kan ik Verplaats de galerie met installatiekopieën van gedeelde resource naar een ander abonnement?

 A. U kunt de gedeelde installatiekopie galeriebron Nee, niet verplaatsen naar een ander abonnement. Echter, kunt u zich voor het repliceren van de versies van een installatiekopie in de galerie in andere regio's waar nodig.

**V:** Kan ik mijn versies van een installatiekopie repliceren tussen clouds – Azure China 21Vianet, Azure Duitsland en Azure Government-Cloud? 

 A. Nee, niet versies van een installatiekopie te repliceren tussen clouds.

**V:** Kan ik mijn versies van een installatiekopie repliceren tussen abonnementen? 

 A. Nee, mag u de versies van een installatiekopie repliceert tussen regio's in een abonnement en worden gebruikt in andere abonnementen via RBAC.

**V:** Kan ik versies van een installatiekopie in Azure AD-tenants delen? 

 A. Ja, u kunt RBAC gebruiken om te delen met personen voor tenants. Maar om te delen op schaal, Zie met behulp van "Share galerie afbeeldingen voor Azure tenants" [PowerShell](../articles/virtual-machines/windows/share-images-across-tenants.md) of [CLI](../articles/virtual-machines/linux/share-images-across-tenants.md).


**V:** Hoe lang duurt het repliceren van de versies van een installatiekopie in de doelregio's?

 A. De tijd van de replicatie van de installatiekopie versie is volledig afhankelijk van de grootte van de installatiekopie en het aantal regio's die wordt gerepliceerd naar. Echter, als een best practice het verdient aanbeveling dat u de installatiekopie van het kleine houden, en sluit u de bron- en -regio's voor de beste resultaten. U kunt de status van de replicatie met behulp van de vlag - ReplicationStatus controleren.


**V:** Wat is het verschil tussen de regio van de gegevensbron en doelregio?

 A. Regio van de gegevensbron is de regio waarin uw versie van de installatiekopie wordt gemaakt en doelregio's zijn de regio's waarin een kopie van de versie van de installatiekopie wordt opgeslagen. U kunt slechts één regio van de gegevensbron hebben voor elke versie van de installatiekopie. Zorg er ook voor dat u de locatie van de regio als een van de doelregio's doorgeven bij het maken van de versie van een installatiekopie.  


**V:** Hoe geef ik de bronregio tijdens het maken van de versie van de installatiekopie?

 A. Tijdens het maken van de versie van een installatiekopie, kunt u de **--locatie** tag op in CLI en de **-locatie** tag op in PowerShell om op te geven van de bronregio. Zorg ervoor dat de beheerde installatiekopie die u als de basisinstallatiekopie gebruikt voor het maken van de versie van de installatiekopie wordt op dezelfde locatie als de locatie waarin u van plan bent om te maken van de versie van de installatiekopie. Zorg er ook voor dat u de locatie van de regio als een van de doelregio's doorgeven bij het maken van de versie van een installatiekopie.  


**V:** Hoe geef ik het aantal installatiekopie versie replica's in elke regio worden gemaakt?

 A. Er zijn twee manieren kunt u het nummer van de installatiekopie van versie replica's in elke regio worden gemaakt:
 
1. Het aantal regionale replica's die Hiermee geeft u het aantal replica's die u wilt maken per regio. 
2. De algemene aantal replica's dit de standaardinstelling per regio aantal is in het geval aantal regionale replica's is niet opgegeven. 

Als u het aantal regionale replica's, geeft de locatie, samen met het aantal replica's die u wilt maken in deze regio: "VS Zuid-centraal 2 = '. 

Als het aantal regionale replica's niet is opgegeven met elke locatie, worden het aantal replica's de algemene aantal replica's die u hebt opgegeven. 

De algemene aantal replica's in de CLI, gebruikt u de **--replica-count** argument in de `az sig image-version create` opdracht.


**V:** Kan ik de gedeelde afbeeldingengalerie maken in een andere locatie dan die waar ik wil de definitie van de installatiekopie en de versie van installatiekopie maken?

 A. Ja, dat is mogelijk. Maar als een best practice, raden we u aan de resourcegroep, gedeelde afbeeldingengalerie, de definitie van installatiekopie en de versie van installatiekopie houden op dezelfde locatie.


**V:** Wat zijn de kosten voor het gebruik van de galerie met installatiekopieën gedeeld?

 A. Er zijn geen kosten voor het gebruik van de galerie met installatiekopieën van gedeelde-service, met uitzondering van de kosten voor opslag voor het opslaan van de versies van een installatiekopie en de kosten voor netwerk-uitgaand verkeer voor het repliceren van de versies van een installatiekopie van de bronregio naar de doelregio's.

**V:** Welke API-versie moet ik gebruiken om galerie met installatiekopieën van gedeelde, de definitie van installatiekopie, Installatiekopieversie en VM/VMSS buiten-versie van de installatiekopie te maken?

 A. Voor implementaties van virtuele machine en virtuele-Machineschaalset met behulp van de versie van een installatiekopie, raden wij aan u API-versie 2018-04-01 of hoger. Als u wilt werken met gedeelde installatiekopie galerieën, definities van de installatiekopie en versies van een installatiekopie, raden wij aan u API-versie 2018-06-01. Zone-redundante opslag (ZRS) is versie 2019-03-01 vereist of hoger.
