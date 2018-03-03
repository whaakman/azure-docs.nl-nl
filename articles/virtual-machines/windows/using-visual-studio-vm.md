---
title: Met Visual Studio op een virtuele machine in Azure | Microsoft Docs
description: Met behulp van Visual Studio op een virtuele machine van Azure.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: PhilLee-MSFT
manager: sacalla
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.prod: vs-devops-alm
ms.date: 01/30/2018
ms.author: phillee
keywords: visualstudio
ms.openlocfilehash: a77fa83ba4fe3f6d7e7ab1ea35929ae610566129
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="visual-studio-images-on-azure"></a>Visual Studio-installatiekopieën in Azure
Met Visual Studio in een vooraf geconfigureerde Azure virtuele machine (VM) is een snelle en eenvoudige manier om naar een up uitgevoerd development environment van nul. Installatiekopieën met verschillende configuraties van Visual Studio zijn beschikbaar in de [Azure Marketplace](https://portal.azure.com/).

Bent u nog niet bekend met Azure? [Maak een gratis Azure-account](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Welke configuraties en -versies zijn beschikbaar?
Afbeeldingen voor de meest recente primaire versies, Visual Studio 2017 en Visual Studio 2015, vindt u in Azure Marketplace. Voor elke primaire versie ziet u de oorspronkelijke versie (RTW) en de meest recente bijgewerkte versies. Elk van deze versies biedt de Visual Studio Enterprise en de Visual Studio Community-edities. Deze installatiekopieën zijn bijgewerkt ten minste elke maand met de meest recente updates voor Visual Studio en Windows. Terwijl de namen van de installatiekopieën hetzelfde blijft, wordt de beschrijving van elke installatiekopie bevat de versie van het geïnstalleerde product en 'op' datum van de afbeelding.

| Release-versie              | Edities            | Productversie     |
|:------------------------------------------:|:----------------------------:|:-----------------------:|
| Visual Studio 2017: Laatste (versie 15,5) |    Enterprise, Community     |      Versie 15.5.3     |
|         Visual Studio 2017: RTW           |    Enterprise, Community     |      Versie 15.0.7     |
|   Visual Studio 2015: Laatste (Update 3)   |    Enterprise, Community     |  Versie 14.0.25431.01  |
|         Visual Studio 2015: RTW           |              Geen            | (Verlopen voor onderhoud) |

> [!NOTE]
> De officiële (RTW) versie van Visual Studio 2015 is in overeenstemming met Microsoft beleid onderhoud, verlopen voor onderhoud. Visual Studio 2015 Update 3 is de enige resterende versie geboden voor de regel voor het product van Visual Studio 2015.

Zie voor meer informatie de [Visual Studio onderhoud beleid](https://www.visualstudio.com/en-us/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Welke functies zijn geïnstalleerd?
Elke installatiekopie bevat de aanbevolen functieset voor deze versie van Visual Studio. In het algemeen omvat de-installatie:

* Alle beschikbare werklasten, met inbegrip van elke werkbelasting aanbevolen optionele onderdelen
* .NET 4.6.2 en .NET 4.7 SDK's, doelitems Packs en hulpprogramma's voor ontwikkelaars
* Visual F#
* GitHub-extensie voor Visual Studio
* LINQ to SQL-hulpprogramma 's

De opdrachtregel die wordt gebruikt voor het installeren van Visual Studio tijdens het bouwen van de afbeeldingen is als volgt:

```
    vs_enterprise.exe --allWorkloads --includeRecommended --passive ^
       add Microsoft.Net.Component.4.7.SDK ^
       add Microsoft.Net.Component.4.7.TargetingPack ^ 
       add Microsoft.Net.Component.4.6.2.SDK ^
       add Microsoft.Net.Component.4.6.2.TargetingPack ^
       add Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
       add Microsoft.VisualStudio.Component.FSharp ^
       add Component.GitHub.VisualStudio ^
       add Microsoft.VisualStudio.Component.LinqToSql
```

Als de afbeeldingen geen een Visual Studio-functie die u nodig hebt, feedback geven via het hulpprogramma feedback in de rechterbovenhoek van de pagina.

## <a name="what-size-vm-should-i-choose"></a>Welke VM-grootte moet ik kiezen?
Azure biedt een breed scala aan de grootte van virtuele machines. Omdat Visual Studio een krachtige toepassing met meerdere threads is, wilt u een VM-grootte die ten minste twee processors en 7 GB geheugen bevat. U wordt aangeraden de volgende VM-grootten voor de installatiekopieën van het Visual Studio:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
Zie voor meer informatie over de grootte van de meest recente machines [grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Met Azure, kunt u uw eerste keuze opnieuw verdelen door het formaat van de virtuele machine te. U kunt inrichten van een nieuwe virtuele machine met een geschiktere grootte of het formaat van uw bestaande virtuele machine naar andere onderliggende hardware. Zie voor meer informatie [vergroten of verkleinen van een virtuele machine van Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm).

## <a name="after-the-vm-is-running-whats-next"></a>Nadat de virtuele machine wordt uitgevoerd, is hoe nu verder?
Visual Studio volgt het model 'bring your own license' in Azure. Net als bij een installatie op eigen hardware, is een van de eerste stappen licentieverlening voor uw installatie van Visual Studio. Voor het ontgrendelen van de Visual Studio:
- Meld u aan met een Microsoft-account die is gekoppeld aan een Visual Studio-abonnement 
- Visual Studio met de productcode die bij uw eerste aankoop ontgrendelen

Zie voor meer informatie [aanmelden bij Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/signing-in-to-visual-studio) en [het ontgrendelen van Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="how-do-i-save-the-development-vm-for-future-or-team-use"></a>Hoe gebruik ik opslaan van de ontwikkeling VM voor toekomstige of het team?

Het spectrum van ontwikkelomgevingen grote, en er echte kosten in verband met het opzetten van de complexere omgevingen. Ongeacht de configuratie van uw omgeving, kunt u opslaan of vastleggen, uw geconfigureerde virtuele machine als een 'basisinstallatiekopie' voor toekomstig gebruik of voor andere leden van uw team. Klik, wanneer een nieuwe virtuele machine wordt opgestart, u ingericht bij de basisinstallatiekopie in plaats van de Azure Marketplace-installatiekopie.

Een kort overzicht: hulpprogramma voor systeemvoorbereiding (Sysprep) gebruiken en de actieve virtuele machine afgesloten en vervolgens vast te leggen *(afbeelding 1)* de virtuele machine als een afbeelding via de gebruikersinterface in de Azure portal. Azure slaat de `.vhd` -bestand met de installatiekopie in de storage-account van uw keuze. De nieuwe installatiekopie vervolgens wordt weergegeven als een Afbeeldingsbron in de lijst met resources van uw abonnement.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Zie afbeelding 1) Een installatiekopie via de gebruikersinterface van de Azure portal.*</center>

Zie voor meer informatie [een begeleide afbeelding van een gegeneraliseerde virtuele machine maken in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).

> [!IMPORTANT]
> Vergeet niet het gebruik van Sysprep voor het voorbereiden van de virtuele machine. Als u deze stap hebt gemist, kan een virtuele machine van de installatiekopie van het Azure niet inrichten.

> [!NOTE]
> U nog steeds kosten in rekening gebracht sommige voor opslag van de installatiekopieën, maar dat incrementele kosten verwaarlozen kan worden vergeleken met de overheadkosten opnieuw opbouwen van de virtuele machine maken voor elk teamlid dat er één nodig heeft. Het kost bijvoorbeeld enkele bedragen maken en opslaan van een installatiekopie 127 GB gedurende een maand die opnieuw kan worden gebruikt door het hele team. Deze kosten zijn echter gering vergeleken met elke werknemer investeert voor het bouwen van en valideren van een correct geconfigureerde dev-vak voor het gebruik ervan afzonderlijke uur.

Bovendien uw taken of technologieën mogelijk meer schaal aan, zoals de soorten ontwikkeling configuraties en configuraties met meerdere machines. U kunt Azure DevTest Labs maken _recepten_ die automatiseren constructie van de 'gouden installatiekopie'. U kunt ook DevTest Labs gebruiken voor het beheren van beleid voor actieve virtuele machines van uw team. [Met behulp van Azure DevTest Labs voor ontwikkelaars](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-developer-lab) is de beste bron voor meer informatie over DevTest Labs.

## <a name="next-steps"></a>Volgende stappen
Nu u weet over de vooraf geconfigureerde installatiekopieën van de Visual Studio, de volgende stap is een nieuwe virtuele machine maken:

* [Een virtuele machine via de Azure portal maken](quick-create-portal.md)
* [Overzicht van virtuele Machines van Windows](overview.md)
