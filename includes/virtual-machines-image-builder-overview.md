---
author: cynthn
ms.author: cynthn
ms.date: 04/30/2019
ms.topic: include
ms.service: virtual-machines-linux
manager: jeconnoc
ms.openlocfilehash: e1b3b5fe603072069cb3a19c7597fcc1872fefd7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66111458"
---
Installatiekopieën van gestandaardiseerde virtuele machines (VM) kunnen organisaties om te migreren naar de cloud en zorgen voor consistentie in de implementaties. Afbeeldingen bevatten doorgaans vooraf gedefinieerde beveiliging en configuratie-instellingen en software die nodig zijn. Instellen van uw eigen installatiekopieën pijplijn vereist tijd, infrastructuur en configuratie, maar met Azure VM Image Builder alleen bieden een eenvoudige configuratie met een beschrijving van uw installatiekopie, het verzenden naar de service en de installatiekopie is gebouwd en gedistribueerd.
 
De Azure VM Image Builder (Azure Image Builder) kunt u beginnen met een Windows of Linux-gebaseerde Azure Marketplace-installatiekopie, bestaande aangepaste installatiekopieën of Red Hat Enterprise Linux (RHEL), ISO en beginnen met uw eigen aanpassingen toe te voegen. Omdat de Image Builder is gebouwd op [HashiCorp Packer](https://packer.io/), u kunt ook uw bestaande Packer provisioner shellscripts importeren. U kunt ook opgeven waar wilt u uw installatiekopieën die worden gehost in de Azure Shared galerie met installatiekopieën (virtual-machines-common-shared-image-galleries.md), als een beheerde installatiekopie of een VHD.

> [!IMPORTANT]
> Azure Image Builder is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="preview-features"></a>Evaluatiefuncties

Voor de Preview-versie, worden deze functies worden ondersteund:

- Maken van installatiekopieën golden basislijn, die de minimumvereisten voor de beveiliging en zakelijke configuraties bevat, en om afdelingen de mogelijkheid om het verder aanpassen aan hun behoeften.
- Patching van de bestaande installatiekopieën, kunt Image Builder u voor het vullen van voortdurend bestaande aangepaste installatiekopieën.
- Integratie met de galerie met Azure Shared installatiekopieën, kunt u om te distribueren, versie, en schaal globaal afbeeldingen en biedt u een image management-systeem.
- Integratie met bestaande installatiekopie pijplijnen bouwen, alleen Image Builder aanroepen vanuit uw pijplijn of gebruikt u de taak eenvoudig Preview Image Builder Azure DevOps.
- Migreer een bestaande installatiekopie aanpassing pijplijn naar Azure. Gebruik uw bestaande scripts, opdrachten en processen voor het aanpassen van installatiekopieën.
- Breng uw eigen abonnement op Red Hat-ondersteuning gebruiken. Red Hat Enterprise-installatiekopieën voor gebruik met uw Red Hat-abonnementen die in aanmerking komen, niet-gebruikte maken.
- Het maken van afbeeldingen in VHD-indeling.
 

## <a name="regions"></a>Regio's
De Image Builder-Service van Azure zijn beschikbaar voor Preview-versie in deze regio's. Installatiekopieën kunnen worden gedistribueerd buiten deze regio's.
- US - oost
- US - oost 2
- US - west-centraal
- US - west
- US - west 2

## <a name="os-support"></a>Ondersteuning voor het besturingssysteem
AIB ondersteunt basisinstallatiekopieën OS voor Azure Marketplace:
- Ubuntu 18.04
- Ubuntu 16.04
- RHEL 7,6
- CentOS 7,6
- Windows 2016
- Windows 2019


## <a name="how-it-works"></a>Hoe het werkt


![Concepttekening van een Azure-functie voor installatiekopieën maken](./media/virtual-machines-image-builder-overview/image-builder.png)

De Azure-Image Builder is een volledig beheerde Azure-service die toegankelijk is via een Azure-resourceprovider. Het Azure Image Builder-proces heeft drie belangrijke onderdelen: de gegevensbron, aanpassen en distribueren, worden deze weergegeven in een sjabloon. In het onderstaande diagram ziet u de onderdelen, met een aantal van de bijbehorende eigenschappen. 
 


**Image Builder-proces** 

![Concepttekening van het Azure Image Builder-proces](./media/virtual-machines-image-builder-overview/image-builder-process.png)

1. De installatiekopie-sjabloon maken als een .json-bestand. Dit .json-bestand bevat informatie over de bron van installatiekopie, aanpassingen en distributie. Er zijn meerdere voorbeelden in de [Azure Image Builder GitHub-opslagplaats](https://github.com/danielsollondon/azvmimagebuilder/tree/master/quickquickstarts).
1. Indienen bij de service, Hiermee maakt u een installatiekopie sjabloon artefact in de resourcegroep die u opgeeft. Op de achtergrond downloadt Image Builder de broninstallatiekopie of ISO en -scripts naar behoefte. Deze worden opgeslagen in een afzonderlijke resourcegroep die automatisch wordt gemaakt in uw abonnement, in de indeling: IT_<DestinationResourceGroup>_<TemplateName>. 
1. Nadat de sjabloon van de installatiekopie is gemaakt, kunt u vervolgens de installatiekopie te bouwen. Op de achtergrond Image Builder maakt gebruik van de sjabloon en de bron-bestanden te maken van een virtuele machine, netwerk en opslag in de IT_<DestinationResourceGroup>_<TemplateName> resourcegroep.
1. Als onderdeel van het maken van de installatiekopieën, Image builder distribueert de installatiekopie van het deze op basis van de sjabloon, verwijdert u vervolgens de extra resources in de IT_<DestinationResourceGroup>_<TemplateName> resourcegroep die is gemaakt voor het proces.


## <a name="permissions"></a>Machtigingen

Als u wilt toestaan dat Azure VM Image Builder voor het distribueren van afbeeldingen aan de beheerde installatiekopieën of aan de galerie met installatiekopieën van een gedeelde, moet u 'Inzender machtigingen voor de service 'Azure VM Image Builder' opgeven (app-ID: cf32a0cc-373c-47c9-9156-0db11f6a6dfc ) op de resourcegroepen. 

Als u een bestaande aangepaste beheerde installatiekopie of installatiekopieversie gebruikt, moet de Azure-Image Builder minimaal 'Lezer' toegang tot deze resourcegroepen.

U kunt toegang met behulp van de Azure CLI toewijzen:

```azurecli-interactive
az role assignment create \
    --assignee cf32a0cc-373c-47c9-9156-0db11f6a6dfc \
    --role Contributor \
    --scope /subscriptions/$subscriptionID/resourceGroups/<distributeResoureGroupName>
```

Als het serviceaccount niet wordt gevonden, kan dat betekenen dat het abonnement waarin het toevoegen van de roltoewijzing is nog niet geregistreerd voor de resourceprovider.


## <a name="costs"></a>Kosten
Er worden sommige compute, netwerk- en opslagkosten bij het maken, bouwen en opslaan van installatiekopieën met Azure Image Builder gebracht. Deze kosten zijn vergelijkbaar met de kosten in rekening gebracht bij het handmatig maken van aangepaste installatiekopieën. Voor de resources, wordt u gefactureerd volgens de tarieven voor Azure. 

Tijdens het proces installatiekopie maken van bestanden worden gedownload en opgeslagen in de `IT_<DestinationResourceGroup>_<TemplateName>` resourcegroep, die wordt een klein opslagkosten in rekening worden gebracht. Als u niet wilt blijven, verwijder de sjabloon installatiekopie na de image build.
 
Image Builder maakt een virtuele machine met behulp van een D1v2 VM-grootte en de opslag en netwerken nodig voor de virtuele machine. Deze resources duurt voor de duur van het bouwproces en worden verwijderd nadat Image Builder is voltooid, de installatiekopie is gemaakt. 
 
Azure Image Builder wordt de installatiekopie distribueren naar uw gekozen regio's, wat zich van kosten voor netwerk-uitgaand verkeer brengen kan.
 
## <a name="next-steps"></a>Volgende stappen 
 
Als u wilt uitproberen de Azure-functie voor installatiekopieën te maken, Zie de artikelen voor het bouwen [Linux](../articles/virtual-machines/linux/image-builder.md) of [Windows](../articles/virtual-machines/windows/image-builder.md) afbeeldingen.
 
 
