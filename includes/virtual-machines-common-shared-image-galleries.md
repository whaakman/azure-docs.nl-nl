---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 56f48f6676b3c878effe788316024e642c17ff5a
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400202"
---
Gedeelde afbeeldingengalerie is een service waarmee u structuur en de organisatie om uw aangepaste VM-installatiekopieën maken. Galerie met installatiekopieën voor gedeelde biedt drie belangrijkste waardevoorstellen
- Eenvoudig beheer
- Schalen van uw klant-installatiekopieën
- Delen van uw installatiekopieën - delen van uw installatiekopieën aan andere gebruikers, service-principals of AD-groepen binnen uw organisatie, evenals de verschillende regio's met behulp van de replicatie voor meerdere regio 's

Een beheerde installatiekopie is een kopie van een volledige VM (inclusief eventuele gekoppelde gegevensschijven) of alleen de besturingssysteemschijf, afhankelijk van hoe u de installatiekopie maken. Wanneer u een virtuele machine van de installatiekopie maakt, wordt de kopie van de VHD's in de afbeelding worden gebruikt voor het maken van de schijven voor de nieuwe virtuele machine. De installatiekopie van het beheerde blijft in de opslag en telkens opnieuw kan worden gebruikt om nieuwe virtuele machines maken.

Als u een groot aantal beheerde installatiekopieën die u moet onderhouden en wil graag zodat ze beschikbaar zijn in uw organisatie hebt, kunt u een galerie met installatiekopieën van de gedeelde als een opslagplaats waarmee u eenvoudig om te werken en het delen van uw afbeeldingen. De kosten voor het gebruik van een galerie met installatiekopieën van de gedeelde zijn alleen de kosten voor de opslag die door de installatiekopieën, plus eventuele kosten voor uitgaand netwerkverkeer voor het repliceren van installatiekopieën uit de regio van de gegevensbron naar de gepubliceerde regio's.

De galerie met installatiekopieën van gedeelde functie heeft meerdere resourcetypen:

| Resource | Beschrijving|
|----------|------------|
| **Beheerde installatiekopie** | Dit is een basisinstallatiekopie die kan worden alleen wordt gebruikt of worden gebruikt voor het maken van meerdere **gedeeld versies van een installatiekopie** in een galerie met installatiekopieën.|
| **Galerie met installatiekopieën** | De Azure Marketplace, zoals een **afbeeldingengalerie** is een opslagplaats voor het beheren en delen van afbeeldingen, maar u bepaalt wie toegang heeft. |
| **Image z galerie** | Afbeeldingen worden gedefinieerd in een galerie en informatie over de installatiekopie en de vereisten voor het gebruik van deze intern uitvoeren. Dit omvat of de installatiekopie Windows of Linux, release-opmerkingen en vereisten voor minimale en maximale hoeveelheid geheugen is. Dit type installatiekopie is een bron in het Resource Manager-implementatiemodel, maar deze wordt niet gebruikt voor het maken van virtuele machines. Het is een definitie van een type van de installatiekopie. |
| **De versie van gedeelde installatiekopie** | Een **installatiekopieversie** is wat u gebruikt om een virtuele machine maken wanneer u een galerie. U kunt meerdere versies van een installatiekopie kunt hebben, indien nodig voor uw omgeving. Een beheerde installatiekopie, bijvoorbeeld wanneer u een **installatiekopieversie** voor het maken van een virtuele machine, de versie van de installatiekopie die wordt gebruikt voor het maken van nieuwe schijven voor de virtuele machine. Versies van een installatiekopie kunnen meerdere keren worden gebruikt. |

<br>


![Afbeelding die laat zien hoe u meerdere versies van een installatiekopie kunt hebben in de galerie](./media/shared-image-galleries/shared-image-gallery.png)

### <a name="regional-support"></a>Regionale ondersteuning

Regionale ondersteuning voor gedeelde afbeeldingsgalerieën is beperkt, maar na verloop van tijd wilt uitbreiden. Voor Preview-versie, worden hier de lijsten van waar u, galerieën en regio's maken kunt waar een galerie kunt repliceren: 

| Galerie In maken  | Versie om te repliceren |
|--------------------|----------------------|
| US - west-centraal    |US - zuid-centraal|
| US - oost 2          |US - oost|
| US - zuid-centraal   |US - oost 2|
| Azië - zuidoost     |US - west|
| Europa -west        |US - west 2|
|                    |US - centraal|
|                    |US - noord-centraal|
|                    |Canada - midden|
|                    |Canada - oost|
|                    |Europa - noord|
|                    |Europa -west|
|                    |India - zuid|
|                    |Azië - zuidoost|



## <a name="scaling"></a>Schalen
Gedeelde galerie met installatiekopieën kunt u het aantal replica's die u wilt dat Azure te houden voor de installatiekopieën opgeven. Dit helpt bij implementatiescenario's voor meerdere VM's als de VM-implementaties kunnen worden verspreid naar verschillende replica's verminderen de kans dat het proces voor het maken van exemplaar wordt beperkt vanwege een overbelasting van een enkele replica.

![Afbeelding die laat zien hoe u installatiekopieën kunt schalen](./media/shared-image-galleries/scaling.png)


## <a name="replication"></a>Replicatie
Gedeelde galerie met installatiekopieën kunt u uw installatiekopieën automatisch naar andere Azure-regio's repliceren. Elke versie van de installatiekopie gedeeld kan worden gerepliceerd naar verschillende regio's, afhankelijk van wat zinvol is voor uw organisatie. Een voorbeeld hiervan is altijd de meest recente installatiekopie repliceren in meerdere regio's, terwijl alle oudere versies alleen beschikbaar in de regio 1 zijn. Dit kan helpen opslaan op de opslagkosten voor versies van een gedeelde installatiekopie. De regio's gerepliceerd naar de versie van de installatiekopie van een gedeeld kunnen worden bijgewerkt na de aanmaaktijd. De tijd die nodig zijn om te repliceren naar verschillende regio's, is afhankelijk van de hoeveelheid gegevens wordt gekopieerd en het aantal regio's de versie is te repliceren. Dit kan enkele uren duren in sommige gevallen. Tijdens de replicatie plaatsvindt, kunt u de status van replicatie weergeven per regio. Nadat de installatiekopie-replicatie voltooid in een regio is, kunt u vervolgens een virtuele machine of met behulp van deze versie in de regio VMSS implementeren.

![Afbeelding die laat zien hoe u installatiekopieën kunt repliceren](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Access
Als de gedeelde afbeeldingengalerie gedeelde installatiekopie en de versie van gedeelde installatiekopie zijn alle resources, ze kunnen worden gedeeld met de ingebouwde die systeemeigen Azure RBAC bepaalt. Met RBAC kunt u deze resources aan andere gebruikers, service-principals en groepen in uw organisatie delen. Het bereik van het delen van deze resources is binnen dezelfde AD-tenant. Wanneer een gebruiker toegang tot de versie van de gedeelde-installatiekopie heeft, kunnen ze een virtuele machine of een virtuele-machineschaalset in een van de abonnementen die ze toegang hebben tot binnen hetzelfde AD-tenant als de versie van de gedeelde-installatiekopie implementeren.  Hier volgt de delen matrix die helpt te begrijpen wat de gebruiker krijgt toegang tot:

| Gedeeld met gebruiker     | Galerie met gedeelde installatiekopieën | Gedeelde installatiekopie | De versie van gedeelde installatiekopie |
|----------------------|----------------------|--------------|----------------------|
| Galerie met gedeelde installatiekopieën | Ja                  | Ja          | Ja                  |
| Gedeelde installatiekopie         | Nee                   | Ja          | Ja                  |
| De versie van gedeelde installatiekopie | Nee                   | Nee           | Ja                  |



## <a name="billing"></a>Billing
Er zijn geen extra kosten voor het gebruik van de galerie met installatiekopieën van gedeelde-service. U wordt gefactureerd voor de volgende resources:
- Kosten voor opslag om de versies van een gedeelde installatiekopie op te slaan. Dit is afhankelijk van het aantal replica's van de versie en het aantal regio's die worden gerepliceerd naar de versie.
- Kosten voor uitgaand verkeer voor replicatie van de regio van de gegevensbron van de versie in de gerepliceerde regio's op het netwerk.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen 

**V:** Hoe registreer ik me voor de gedeelde installatiekopie galerie openbare Preview?
 
 A. Om u te registreren voor de galerie met installatiekopieën van gedeelde openbare preview, moet u registreren voor de functie door het uitvoeren van de volgende opdrachten uit elk van de abonnementen waarin u van plan bent om te maken van een galerie met installatiekopieën van de gedeelde, de definitie van installatiekopie of installatiekopie versie resources, en ook waar u van plan bent om te implementeren van virtuele Machines met behulp van de versies van een installatiekopie.

**CLI**: 

```bash 
az feature register --namespace Microsoft.Compute --name GalleryPreview
az provider register -n Microsoft.Compute
```

**PowerShell**: 

```powershell
Register-AzureRmProviderFeature -FeatureName GalleryPreview -ProviderNamespace Microsoft.Compute
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

**V:** Hoe kan ik weergeven van alle galerie met installatiekopieën van gedeelde resources tussen abonnementen? 
 
 A. Als u wilt weergeven van alle galerie met installatiekopieën van gedeelde resources tussen abonnementen dat u toegang tot op de Azure-portal hebben, de volgende stappen uit te voeren:

 1. Open de [Azure Portal](https://portal.azure.com).
 1. Ga naar **alle Resources**.
 1. Selecteer alle abonnementen op grond waarvan u wilt weergeven van alle resources.
 1. Zoeken naar resources van het type **privégalerie**.
 
 Als u wilt zien de definities van de installatiekopie en de versies van een installatiekopie, moet u ook selecteren **verborgen typen weergeven**.
 
 Als u de resources voor de galerie met gedeelde installatiekopie voor abonnementen waarvoor u machtigingen hebt, gebruikt u de volgende opdracht uit in de Azure CLI:

 ```bash
 az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
 ```


**V:** Hoe moet ik mijn installatiekopieën delen tussen abonnementen?
 
 A. U kunt installatiekopieën delen tussen abonnementen met behulp van rollen gebaseerd toegangsbeheer (RBAC). Elke gebruiker die heeft voor de versie van een installatiekopie, zelfs tussen abonnementen leesmachtigingen, zich op een virtuele Machine met behulp van de versie van de installatiekopie implementeren.


**V:** Kan ik mijn bestaande installatiekopie verplaatsen naar de galerie met installatiekopieën van de gedeelde?
 
 A. Ja. Er zijn 3 's op basis van de typen installatiekopieën die u hebt.

 Scenario 1: Als u een beheerde installatiekopie hebt, klikt u vervolgens kunt u een definitie van de installatiekopie en de versie van installatiekopie uit.

 Scenario 2: Als u een niet-beheerde gegeneraliseerde installatiekopie hebt, kunt u een beheerde installatiekopie maken op basis van deze, en maakt u een definitie van de installatiekopie en de versie van installatiekopie van het. 

 Scenario 3: Als u een VHD in het lokale bestandssysteem hebt, moet u de VHD te uploaden, een beheerde installatiekopie maken en vervolgens kunt u maken en definitie- en image-versie van deze afbeelding. 
    - Als de VHD van een Windows-VM, Zie [een gegeneraliseerde VHD uploaden](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
    - Als de VHD voor een Linux-VM is, Zie [een VHD uploaden](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd)


**V:** Kan ik de versie van een installatiekopie maken van een gespecialiseerde schijf?

 A. Nee, we op dit moment ondersteunen geen gespecialiseerde schijf als afbeeldingen. Als u een gespecialiseerde schijf hebt, moet u [een virtuele machine maken van de VHD](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) door de specializeddisk koppelen aan een nieuwe virtuele machine. Nadat u een actieve virtuele machine hebt, moet u Volg de instructies voor een beheerde installatiekopie maken van de [Windows-VM] (https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) of [Linux-VM](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Zodra u een gegeneraliseerde installatiekopie van de beheerde hebt, kunt u het proces voor het maken van een gedeelde installatiekopie beschrijving en de versie van installatiekopie kunt starten.


**V:** Kan ik een galerie met installatiekopieën van gedeelde, de definitie van installatiekopie en de versie van de installatiekopie via de Azure-portal maken?

 A. Nee, momenteel we bieden geen ondersteuning het maken van een van de galerie met installatiekopieën van gedeelde resources via Azure portal. We bieden echter ondersteuning voor het maken van de galerie met installatiekopieën van gedeelde resources via de CLI, sjablonen en SDK's. PowerShell ook binnenkort release.

 
**V:** Nadat u hebt gemaakt, kan ik bijwerken definitie van de installatiekopie of versie van de installatiekopie? Wat voor soort gegevens kan ik aanpassen?

 A. De details die kunnen worden bijgewerkt op elk van de resources worden hieronder vermeld:
 
Galerie met installatiekopieën van de gedeelde:
- Beschrijving

de definitie van de installatiekopie:
- Aanbevolen vcpu 's
- Geheugen
- Beschrijving
- Einde van de levensduur van datum

Installatiekopieversie:
- Aantal regionale replica 's
- Doelregio's
- Uitsluiting van de meest recente
- Einde van de levensduur van datum


**V:** Eenmaal gemaakt, kan ik Verplaats de galerie met installatiekopieën van gedeelde resource naar een ander abonnement?

 A. U kunt de gedeelde installatiekopie galeriebron Nee, niet verplaatsen naar een ander abonnement. Echter, kunt u zich voor het repliceren van de versies van een installatiekopie in de galerie in andere regio's waar nodig.

**V:** Kan ik mijn versies van een installatiekopie repliceren tussen clouds – Azure China 21Vianet, Azure Duitsland en Azure Government-Cloud? 

 A. Nee, niet versies van een installatiekopie te repliceren tussen clouds.

**V:** Kan ik mijn versies van een installatiekopie repliceren tussen abonnementen? 

 A. Nee, mag u de versies van een installatiekopie repliceert tussen regio's in een abonnement en worden gebruikt in andere abonnementen via RBAC.

**V:** Kan ik versies van een installatiekopie voor AD-tenants delen? 

 A. Momenteel gedeelde afbeeldingengalerie biedt Nee, geen ondersteuning voor het delen van de versies van een installatiekopie voor AD-tenants. Echter, u kunt de functie voor persoonlijke aanbiedingen op Azure Marketplace om dit te bereiken.


**V:** Hoe lang duurt het repliceren van de versies van een installatiekopie in de doelregio's?

 A. De tijd van de replicatie van de installatiekopie versie is volledig afhankelijk van de grootte van de installatiekopie en het aantal regio's die wordt gerepliceerd naar. Echter, als een best practice het verdient aanbeveling dat u de installatiekopie van het kleine houden, en sluit u de bron- en -regio's voor de beste resultaten. U kunt de status van de replicatie met behulp van de vlag - ReplicationStatus controleren.


**V:** Hoeveel gedeelde installatiekopie galerieën kunnen ik in een abonnement maken?

 A. De quotumlimiet is 
- 10 gedeelde afbeeldingsgalerieën per abonnement per regio
- 200 definities van de installatiekopie, per abonnement per regio
- 2000 versies van een installatiekopie, per abonnement per regio


**V:** Wat is het verschil tussen de regio van de gegevensbron en doelregio?

 A. Regio van de gegevensbron is de regio waarin uw versie van de installatiekopie wordt gemaakt en doelregio's zijn de regio's waarin een kopie van de versie van de installatiekopie wordt opgeslagen. U kunt slechts één regio van de gegevensbron hebben voor elke versie van de installatiekopie. Zorg er ook voor dat u de locatie van de regio als een van de doelregio's doorgeven bij het maken van de versie van een installatiekopie.  


**V:** Hoe geef ik de bronregio tijdens het maken van de versie van de installatiekopie?

 A. Tijdens het maken van de versie van een installatiekopie, kunt u de **--locatie** tag op in CLI en de **-locatie** tag op in PowerShell om op te geven van de bronregio. Zorg ervoor dat de beheerde installatiekopie die u als de basisinstallatiekopie gebruikt voor het maken van de versie van de installatiekopie wordt op dezelfde locatie als de locatie waarin u van plan bent om te maken van de versie van de installatiekopie. Zorg er ook voor dat u de locatie van de regio als een van de doelregio's doorgeven bij het maken van de versie van een installatiekopie.  


**V:** Hoe geef ik het aantal installatiekopie versie replica's in elke regio worden gemaakt?

 A. Er zijn twee manieren kunt u het nummer van de installatiekopie van versie replica's in elke regio worden gemaakt:
 
1. Het aantal regionale replica's die Hiermee geeft u het aantal replica's die u wilt maken per regio. 
2. De algemene aantal replica's dit de standaardinstelling per regio aantal is in het geval aantal regionale replica's is niet opgegeven. 

Als u wilt het aantal regionale replica's opgeven, geeft u de locatie, samen met het aantal replica's die u wilt maken in die regio als volgt: "Zuid-centraal VS = 2 '. 

Als het aantal regionale replica's niet is opgegeven met elke locatie, worden het aantal replica's de algemene aantal replica's die u hebt opgegeven. 

De algemene aantal replica's in de CLI, gebruikt u de **--replica-count** argument in de `az sig image-version create` opdracht.


**V:** Kan ik de gedeelde afbeeldingengalerie maken in een andere locatie dan die waar ik wil de definitie van de installatiekopie en de versie van installatiekopie maken?

 A. Ja, dit is mogelijk. Maar als een best practice, raden we u aan de resourcegroep, gedeelde afbeeldingengalerie, de definitie van installatiekopie en de versie van installatiekopie houden op dezelfde locatie.


**V:** Wat zijn de kosten voor het gebruik van de galerie met installatiekopieën gedeeld?

 A. Er zijn geen kosten voor het gebruik van de galerie met installatiekopieën van gedeelde-service, met uitzondering van de kosten voor opslag voor het opslaan van de versies van een installatiekopie en de kosten voor netwerk-uitgaand verkeer voor het repliceren van de versies van een installatiekopie van de bronregio naar de doelregio's.

**V:** Welke API-versie moet ik gebruiken om galerie met installatiekopieën van gedeelde, de definitie van installatiekopie, Installatiekopieversie en VM/VMSS buiten-versie van de installatiekopie te maken?

 A. Voor de virtuele machine en virtuele-machineschaalsets ingesteld met behulp van de versie van een installatiekopie implementaties, raden wij aan u API-versie 2018-04-01 of hoger. Als u wilt werken met gedeelde installatiekopie galerieën, definities van de installatiekopie en versies van een installatiekopie, raden wij aan u API-versie 2018-06-01. 