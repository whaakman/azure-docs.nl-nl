---
title: Met behulp van Visual Studio op een Azure-machine | Microsoft Docs
description: Met behulp van Visual Studio op een Azure-machine.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: PhilLee-MSFT
manager: cathys
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.custom: vs-azure
ms.workload: azure-vs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.prod: vs-devops-alm
ms.date: 09/12/2018
ms.author: phillee
keywords: visualstudio
ms.openlocfilehash: 1582cc7e48adcf895dc7c07e7ab485790650ac14
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911908"
---
# <a name="visual-studio-images-on-azure"></a>Visual Studio-installatiekopieën in Azure
Met behulp van Visual Studio in een vooraf geconfigureerde Azure-machine (VM) is een snelle en eenvoudige manier naar een up-en-die wordt uitgevoerd development environment van nul. Installatiekopieën met verschillende configuraties van Visual Studio zijn beschikbaar in de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=%22visual%20studio%202017%22&page=1).

Bent u nog niet bekend met Azure? [Maak een gratis Azure-account](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Welke configuraties en -versies zijn beschikbaar?
Installatiekopieën voor de meest recente primaire versies, Visual Studio 2017 en Visual Studio 2015, vindt u in de Azure Marketplace.  We hebben onlangs ondersteuning voor Preview-versies van de verwachte primaire versie - Visual Studio 2019 toegevoegd.  Voor elke uitgebrachte primaire versie ziet u de oorspronkelijke versie (RTW) en de meest recente bijgewerkte versies.  Elk van deze versies biedt de Visual Studio Enterprise en de Visual Studio Community-edities.  Deze installatiekopieën zijn ten minste maandelijks bijgewerkt met de meest recente updates voor Visual Studio en Windows.  Terwijl de namen van de installatiekopieën hetzelfde zijn, wordt de beschrijving van elke afbeelding bevat de versie van het geïnstalleerde product en 'op' datum van de installatiekopie.

| Release-versie                                              | Edities                     |     Productversie      |
|:------------------------------------------------------------:|:----------------------------:|:------------------------:|
|    Visual Studio 2019: Preview-versie (Preview-versie 2)                   |           Enterprise         | Preview-versie 16.0.0 versie 2 |
| Visual Studio 2017: Meest recente (versie 15,9 inch)                    |    Enterprise, Community     |      Versie 15.9.4      |
|         Visual Studio 2017: RTW                              |    Enterprise, Community     |      Versie 15.0.20     |
|   Visual Studio 2015: Meest recente (Update 3)                      |    Enterprise, Community     |  Versie 14.0.25431.01   |
|         Visual Studio 2015: RTW                              |             Geen             | (Verlopen voor onderhoud)  |

> [!NOTE]
> De oorspronkelijk (RTW) versie van Visual Studio 2015 is in overeenstemming met de Microsoft servicebeleid, verlopen voor onderhoud. Visual Studio 2015 Update 3 is de enige resterende versie aangeboden voor de regel voor het product van Visual Studio 2015.

Zie voor meer informatie de [Visual Studio onderhoud beleid](https://www.visualstudio.com/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Welke functies zijn geïnstalleerd?
Elke afbeelding bevat de aanbevolen functieset voor deze versie van Visual Studio. Over het algemeen omvat de-installatie:

* Alle beschikbare werklasten, met inbegrip van de workload aanbevolen optionele onderdelen
* .NET 4.6.2 en .NET 4.7 SDK's, doelitems Packs en hulpprogramma's voor ontwikkelaars
* VisualF#
* GitHub-extensie voor Visual Studio
* LINQ to SQL-hulpprogramma 's

De opdrachtregel gebruikt voor het installeren van Visual Studio bij het bouwen van de afbeeldingen is als volgt:

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

Als de afbeeldingen niet over een Visual Studio-functie die u nodig hebt beschikken, feedback via het hulpprogramma feedback in de rechterbovenhoek van de pagina.

## <a name="what-size-vm-should-i-choose"></a>Hoe groot de virtuele machine moet ik kiezen?
Azure biedt een uitgebreid scala aan VM-groottes. Omdat Visual Studio een krachtige, dankzij de multi-threaded toepassing is, wilt u een VM-grootte die bestaat uit ten minste twee processoren en 7 GB geheugen. U wordt aangeraden de volgende VM-grootten voor de Visual Studio-installatiekopieën:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
Zie voor meer informatie over de meest recente machinegrootten [grootten voor Windows virtuele machines in Azure](/azure/virtual-machines/windows/sizes).

Met Azure, kunt u uw eerste keuze door het formaat van de virtuele machine te herverdelen. U kunt een nieuwe virtuele machine met een geschiktere grootte inrichten of grootte van uw bestaande virtuele machine naar andere onderliggende hardware. Zie voor meer informatie, [vergroten of verkleinen van een Windows-VM](/azure/virtual-machines/windows/resize-vm).

## <a name="after-the-vm-is-running-whats-next"></a>Nadat de virtuele machine wordt uitgevoerd, is hoe nu verder?
Visual Studio, volgt het model 'bring your own license' in Azure. Net als bij een installatie op de eigen hardware, is een van de eerste stappen licentieverlening voor uw installatie van Visual Studio. Voor het ontgrendelen van een Visual Studio:
- Meld u aan met een Microsoft-account dat is gekoppeld aan een abonnement op Visual Studio 
- Visual Studio met de productcode die is meegeleverd bij uw oorspronkelijke aankoop ontgrendelen

Zie voor meer informatie, [aanmelden bij Visual Studio](/visualstudio/ide/signing-in-to-visual-studio) en [hoe u voor het ontgrendelen van Visual Studio](/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="how-do-i-save-the-development-vm-for-future-or-team-use"></a>Hoe kan ik opslaan de toekomstige ontwikkeling VM of het team dat gebruiken?

Het spectrum van ontwikkelomgevingen is enorm er is echt kosten die gepaard gaan met het bouwen van de meer complexe omgevingen. U kunt ongeacht de configuratie van uw omgeving, opslaan of vastleggen, uw geconfigureerde virtuele machine als een 'basisinstallatiekopie' voor toekomstig gebruik of andere leden van uw team. Vervolgens, wanneer een nieuwe virtuele machine wordt opgestart, richt u deze uit de basisinstallatiekopie in plaats van de Azure Marketplace-installatiekopie.

Een snel overzicht: Gebruik het hulpprogramma voor systeemvoorbereiding (Sysprep) en de actieve virtuele machine af en vervolgens vast te leggen *(afbeelding 1)* de virtuele machine als een installatiekopie via de gebruikersinterface in Azure portal. Azure slaat de `.vhd` bestand met de installatiekopie in de storage-account van uw keuze. De nieuwe installatiekopie wordt vervolgens weergegeven als een installatiekopie-resource in de lijst met resources van uw abonnement.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Afbeelding 1) Een installatiekopie maken via de gebruikersinterface van Azure portal.*</center>

Zie voor meer informatie, [maken van een beheerde installatiekopie van een gegeneraliseerde VM in Azure](/azure/virtual-machines/windows/capture-image-resource).

> [!IMPORTANT]
> Vergeet niet het gebruik van Sysprep de virtuele machine voorbereiden. Als u deze stap hebt gemist, inrichten niet een VM op basis van de installatiekopie van het in Azure.

> [!NOTE]
> Worden er nog enige kosten voor opslag van de installatiekopieën, maar dat meerprijs extra kan worden vergeleken met de overheadkosten opnieuw opbouwen van de virtuele machine maken voor elk teamlid dat één nodig heeft. Het kost bijvoorbeeld een paar euro maken en opslaan van een installatiekopie 127 GB die opnieuw kan worden gebruikt door uw hele team een maand. Deze kosten zijn echter extra ten opzichte van elke werknemer investeert om te bouwen en valideren van een DEV-machine juist is geconfigureerd voor het gebruik ervan afzonderlijke uur.

Bovendien uw ontwikkelingstaken of technologieën mogelijk meer schaal, zoals varianten van de ontwikkeling van configuraties en configuraties met meerdere machines. U kunt Azure DevTest Labs maken _recepten_ die automatiseren constructie van de 'gouden installatiekopie'. U kunt ook DevTest Labs gebruiken voor het beheren van beleid voor actieve virtuele machines van uw team. [Met Azure DevTest Labs voor ontwikkelaars](/azure/devtest-lab/devtest-lab-developer-lab) is de beste bron voor meer informatie over DevTest Labs.

## <a name="next-steps"></a>Volgende stappen
Nu dat u over de vooraf geconfigureerde installatiekopieën voor Visual Studio weet, wordt de volgende stap is het maken van een nieuwe virtuele machine:

* [Een virtuele machine via de Azure-portal maken](quick-create-portal.md)
* [Overzicht van Windows-Machines](overview.md)
