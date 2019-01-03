---
title: Overzicht - rendering Azure Batch
description: Introductie van het gebruik van Azure voor rendering en een overzicht van Azure Batch rendering-mogelijkheden
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: bc8c96345aeb1886696326edd230666ac8b6c41d
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542192"
---
# <a name="rendering-using-azure"></a>Weergeven met Azure

Rendering is het proces van 3D-modellen maken en deze omzetten in 2D-afbeeldingen. 3D-scènebestanden zijn ontworpen en in toepassingen als Autodesk 3ds Max, Autodesk Maya en Blender.  Rendering-toepassingen als Autodesk Maya, Autodesk Arnold, Chaos Group V-Ray en Blender cycli produceren 2D-afbeeldingen.  Soms enkele afbeeldingen zijn gemaakt op basis van de scènebestanden. Het is echter gebruikelijk modelleren en meerdere afbeeldingen weergeven en ze vervolgens combineren in een animatie.

De renderingswerklast wordt intensief gebruikt voor speciale effecten (VFX) in de Media en Entertainment. Rendering wordt ook gebruikt in veel andere industires zoals reclame, detailhandel, olie en gas- en productie.

Het proces van rendering is rekenintensief zijn; kunnen er veel frames/images voor het produceren en elke afbeelding kunnen vele uren om weer te geven.  Rendering is daarom een perfecte batch verwerkt-werkbelasting die kan gebruikmaken van Azure en Azure Batch om u te veel renders parallel worden uitgevoerd.

## <a name="why-use-azure-for-rendering"></a>Waarom Azure voor rendering gebruiken?

Rendering is om verschillende redenen een werkbelasting is perfect geschikt zijn voor Azure en Azure Batch:

* Rendering van taken kan worden gesplitst in veel onderdelen die parallel met behulp van meerdere virtuele machines kunnen worden uitgevoerd:
  * Animaties bestaan uit veel frames en elk frame parallel kan worden weergegeven.  De meer virtuele machines beschikbaar om te verwerken die elk frame, hoe sneller alle frames en de animatie kunnen worden geproduceerd.
  * Sommige software rendering kan één frames moeten worden opgesplitst in meerdere onderdelen, zoals tegels of segmenten.  Elk kan afzonderlijk worden weergegeven en vervolgens gecombineerd tot de uiteindelijke installatiekopie wanneer u klaar bent met alle onderdelen.  De meer virtuele machines die beschikbaar zijn, des te sneller een frame kan worden gerenderd.
* Rendering van projecten, kan enorme schaal vereisen:
  * Afzonderlijke frames kunnen complex zijn en vele uren om weer te geven, zelfs op geavanceerde hardware; vereisen animaties kunnen bestaan uit honderden of duizenden frames.  Een enorme hoeveelheid rekenkracht is vereist voor het renderen van animaties van hoge kwaliteit in een redelijk tijdsbestek.  In sommige gevallen kan zijn meer dan 100.000 kernen gebruikt om duizenden frames parallel weer te geven.
* Rendering-projecten zijn op basis van een project en vereisen verschillende hoeveelheden compute:
  * Toewijzen van reken- en opslagcapaciteit indien nodig, het omhoog of omlaag schalen op basis van laden tijdens een project en verwijderen wanneer een project is voltooid.
  * Betaal voor capaciteit toewijzen, maar Betaal niet voor deze wanneer er geen belasting, zoals tussen projecten.
  * Geschikt zijn voor pieken vanwege onverwachte wijzigingen. schaal hoger als er onverwachte wijzigingen laat in een project en de wijzigingen zijn moeten worden verwerkt op een strakke planning.
* Kies uit een breed aanbod van hardware op basis van de toepassing, werkbelasting en de tijdsduur:
  * Er is een ruime keuze aan hardware beschikbaar zijn in Azure die kan worden toegewezen en beheerd met Batch.
  * Afhankelijk van het project mogelijk de vereiste voor de beste prijs-prestatieverhouding of de beste prestaties.  Verschillende schermen en/of renderingtoepassing hebben verschillende geheugenvereisten.  Een renderingtoepassing kan gebruikmaken van GPU's voor de beste prestaties of bepaalde functies. 
* Virtuele machines met lage prioriteit verlagen:
  * Virtuele machines met lage prioriteit zijn beschikbaar voor een grote kortingen in vergelijking met VM's op aanvraag en zijn geschikt voor bepaalde taaktypen.
  * Azure Batch met Batch flexibiliteit op hoe ze worden gebruikt die geschikt zijn voor een breed scala aan vereisten kunnen VM's met lage prioriteit worden toegewezen.  Batch-pools kunnen bestaan uit zowel toegewezen als lage prioriteit VM's, met erop vertrouwen dat u kunt de combinatie van VM-typen op elk gewenst moment wijzigen.

## <a name="options-for-rendering-on-azure"></a>Opties voor rendering in Azure

Er zijn een verscheidenheid aan door Azure-mogelijkheden die kunnen worden gebruikt voor rendering-workloads.  Welke mogelijkheden te gebruiken, is afhankelijk van een bestaande omgeving en vereisten.

### <a name="existing-on-premises-rendering-environment-using-a-render-management-application"></a>Bestaande on-premises rendering-omgeving met behulp van een render management-toepassing

De meeste gevallen is voor als er weer een bestaande on-premises farm die worden beheerd door een render-management-toepassing zoals PipelineFX Qube, Royal weergeven of Thinkbox Deadline.  De vereiste is om uit te breiden de on-premises render farm capaciteit die gebruikmaakt van Azure-VM's.

De render-beheersoftware heeft ingebouwde ondersteuning voor Azure of maken we beschikbare invoegtoepassingen die Azure-ondersteuning toevoegt. Voor meer informatie over de ondersteunde renderen managers en functionaliteit ingeschakeld, raadpleegt u het artikel [renderen met behulp van managers](https://docs.microsoft.com/azure/batch/batch-rendering-render-managers).

### <a name="custom-rendering-workflow"></a>Aangepaste rendering-werkstroom

De vereiste is voor virtuele machines om uit te breiden van een bestaande renderfarm.  Azure Batch-pools kunnen grote aantallen virtuele machines toewijzen, zodat VM's met lage prioriteit worden gebruikt en dynamisch automatisch geschaald met de volledige prijs virtuele machines en betalen voor gebruik licentieverlening voor veelgebruikte renderingtoepassing bieden.

### <a name="no-existing-render-farm"></a>Er zijn geen bestaande renderfarm

Clientwerkstations rendering, kunnen uitvoeren, maar het verhogen van de renderingswerklast en duurt te lang is uitsluitend gebruik van capaciteit werkstation.  Azure Batch kan worden gebruikt om zowel render farm Computing op aanvraag toewijzen als de weergavetaken naar de Azure renderfarm plannen.

## <a name="azure-batch-rendering-capabilities"></a>Mogelijkheden van Azure Batch rendering

Azure Batch kunt parallelle workloads kunnen worden uitgevoerd in Azure.  Hiermee kunt het maken en beheren van grote aantallen virtuele machines waarop toepassingen worden geïnstalleerd en uitgevoerd.  Het biedt ook uitgebreide taakplanning mogelijkheden voor het uitvoeren van exemplaren van deze toepassingen, bieden de toewijzing van taken aan virtuele machines, queuing toepassingsbewaking, enzovoort.

Azure Batch is gebruikt voor veel werkbelastingen, maar de volgende mogelijkheden zijn beschikbaar voor een specifiek u gemakkelijker en sneller om uit te voeren renderingswerklasten.

* VM-installatiekopieën met vooraf geïnstalleerde grafische toepassingen en renderingtoepassingen:
  * Azure Marketplace-VM-installatiekopieën beschikbaar zijn die populaire grafische toepassingen en renderingtoepassingen, voorkomen van de noodzaak om te installeren van de toepassingen zelf of uw eigen aangepaste installatiekopieën maken met de geïnstalleerde toepassingen bevatten. 
* Betalen per gebruik-licentieverlening voor rendering-toepassingen:
  * U kunt kiezen om te betalen voor de toepassingen per minuut, naast de facturatie voor de Reken-VM's, die voorkomt dat u hoeft te kopen van licenties en mogelijk voor de toepassingen een licentieserver configureren.  Betalen voor gebruik betekent ook dat het is mogelijk die geschikt zijn voor verschillende en onverwachte laden omdat er geen een vast aantal licenties.
  * Het is ook mogelijk om de vooraf geïnstalleerde toepassingen gebruiken met uw eigen licenties en gebruik niet de licentieverlening voor betalen per gebruik. U doet dit door u installeert gewoonlijk een on-premises of op basis van Azure licentieserver en een Azure-netwerk om de rendering van toepassingen verbinding met de licentieserver te gebruiken.
* Plug-ins voor het ontwerpen van de client en ontwerptoepassingen:
  * Invoegtoepassingen toestaan eindgebruikers gebruikmaken van Azure Batch rechtstreeks vanuit de clienttoepassing, zoals Autodesk Maya, waardoor ze maken van pools,-taken verzenden en maken gebruik van meer rekencapaciteit om uit te voeren sneller uitgevoerd.
* Integratie van manager weergegeven:
  * Azure Batch is geïntegreerd met toepassingen voor rendering of invoegtoepassingen zijn beschikbaar om de Azure Batch-integratie te bieden.

Er zijn verschillende manieren waarop u met Azure Batch, die allemaal ook van toepassing op Azure Batch rendering.

* API's:
  * Schrijf code met behulp van de [REST](https://docs.microsoft.com/rest/api/batchservice), [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/batch), [Python](https://docs.microsoft.com/python/api/overview/azure/batch), [Java](https://docs.microsoft.com/java/api/overview/azure/batch), of andere API's ondersteund.  Ontwikkelaars kunnen Azure Batch-mogelijkheden integreren in hun bestaande toepassingen of de werkstroom, of cloud of op basis van on-premises.  Bijvoorbeeld, de [Autodesk Maya invoegtoepassing](https://github.com/Azure/azure-batch-maya) maakt gebruik van de Batch Python-API voor het aanroepen van Batch, maken en beheren van pools, jobs en taken indienen en bewaken van status.
* Opdrachtregelprogramma's:
  * De [Azure vanaf de opdrachtregel](https://docs.microsoft.com/cli/azure/) of [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) kan worden gebruikt voor het gebruik van de Batch script.
  * In het bijzonder gemakkelijker de ondersteuning van de sjabloon Batch CLI veel te maken van pools en taken verzendt.
* UI:
  * [Batch Explorer](https://github.com/Azure/BatchExplorer) is een platformoverschrijdende clienthulpprogramma waarmee ook Batch-accounts worden beheerd en bewaakt, maar sommige uitgebreidere mogelijkheden ten opzichte van de gebruikersinterface van Azure portal biedt.  Een aantal toepassingen en -taak zijn opgegeven die speciaal geschikt zijn voor elke ondersteunde toepassing en kan worden gebruikt om eenvoudig te maken van toepassingen en voor het verzenden van taken.
  * De Azure-portal kan worden gebruikt voor het beheren en controleren van Azure Batch.
* Client-toepassing plug-in de:
  * Invoegtoepassingen zijn beschikbaar die Batch rendering worden gebruikt als rechtstreeks in het ontwerp van de client en ontwerptoepassingen toestaan. De plug-ins aanroepen voornamelijk van de Batch Explorer-toepassing met contextuele informatie over de huidige 3D-model.
  * De volgende invoegtoepassingen zijn beschikbaar:
    * [Azure Batch voor Maya](https://github.com/Azure/azure-batch-maya)
    * [3DS Max.](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
    * [Blender](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)

## <a name="getting-started-with-azure-batch-rendering"></a>Aan de slag met Azure Batch rendering

Zie de volgende inleidende zelfstudie om te proberen van Azure Batch rendering:

* [Batch Explorer gebruiken voor een Blender scène renderen](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)
* [De Batch-Opdrachtregelinterface gebruiken om een Autodesk 3ds Max-scène weer te geven](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)

## <a name="next-steps"></a>Volgende stappen

Bepalen van de lijst met rendering-toepassingen en versies die zijn opgenomen in de Azure Marketplace VM-installatiekopieën in [in dit artikel](https://docs.microsoft.com/azure/batch/batch-rendering-applications).