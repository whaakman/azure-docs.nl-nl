---
title: Met Visual Studio op een virtuele Machine in Azure | Microsoft Docs
description: Met behulp van Visual Studio op een virtuele Machine van Azure.
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
ms.openlocfilehash: 599a890be4d014d22bae899be4cf6e281c4109d4
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2018
---
# <a id="top"></a> Visual Studio-installatiekopieën in Azure
Met Visual Studio in een vooraf geconfigureerde Azure virtuele machine (VM), is de snelste en gemakkelijkste manier verder niets voor een ontwikkelingsomgeving omhoog en uitgevoerd.  Installatiekopieën met verschillende configuraties van Visual Studio zijn beschikbaar in de [Azure Marketplace](https://portal.azure.com/). Gewoon een virtuele machine opstarten en uitschakelen u gaat.

Bent u nog niet bekend met Azure? [Maak een gratis Azure-account](https://azure.microsoft.com/free).

## <a name="what-configurations-and-versions-are-available"></a>Welke configuraties en -versies zijn beschikbaar?
In Azure Marketplace, u installatiekopieën vinden voor de meest recente primaire versies: Visual Studio 2017 en Visual Studio 2015.  Voor elke primaire versie, ziet u de officiële (aka ' RTW') versie en de 'nieuwste' bijgewerkte versies.  Voor elk van deze verschillende versies vindt u de Visual Studio Enterprise en Visual Studio Community-edities.  Werken wij deze installatiekopieën ten minste elke maand zodanig dat de meest recente updates voor Visual Studio en Windows.  Terwijl de namen van de installatiekopieën hetzelfde blijft, wordt elke installatiekopie desription bevat de versie van het geïnstalleerde product en 'vanaf datum van de afbeelding.

|               Release-versie              |          Edities            |     Versie van het product     |
|:------------------------------------------:|:----------------------------:|:-----------------------:|
| Visual Studio 2017 - nieuwste (versie 15,5) |    Enterprise, Community     |      Versie 15.5.3     |
|         Visual Studio 2017 - RTW           |    Enterprise, Community     |      Versie 15.0.7     |
|   Visual Studio 2015 - nieuwste (Update 3)   |    Enterprise, Community     |  Versie 14.0.25431.01  |
|         Visual Studio 2015 - RTW           |              Geen            | (Verlopen voor onderhoud) |

> [!NOTE]
> In overeenstemming met Microsoft-beleid, de officiële onderhoud (aka ' RTW') versie van Visual Studio 2015 is verlopen voor onderhoud.  Visual Studio 2015 Update 3 is daarom de enige resterende versie geboden voor de regel voor het product van Visual Studio 2015.

Zie voor meer informatie de [Visual Studio onderhoud beleid](https://www.visualstudio.com/en-us/productinfo/vs-servicing-vs).

## <a name="what-features-are-installed"></a>Welke functies zijn geïnstalleerd?
Elke installatiekopie bevat de aanbevolen functieset voor deze versie van Visual Studio.  In het algemeen omvat de-installatie:

* Alle beschikbare werkbelastingen, met inbegrip van de aanbevolen optionele onderdelen die de werkbelasting
* .NET 4.6.2 en .NET 4.7 SDK's, doelitems Packs en hulpprogramma's voor ontwikkelaars
* Visual F#
* GitHub-extensie voor Visual Studio
* LINQ to SQL-hulpprogramma 's

Dit is de opdrachtregel die we gebruiken voor het installeren van Visual Studio bij het maken van installatiekopieën van het:

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

Als de afbeeldingen geen een Visual Studio-functie die u nodig hebt, feedback geven die via het hulpprogramma feedback (rechterbovenhoek van de pagina).

## <a name="what-size-vm-should-i-choose"></a>Welke VM-grootte moet ik kiezen?
Inrichting van een nieuwe virtuele machine is eenvoudig en Azure biedt een breed scala aan de grootte van virtuele machines.  Net als bij de aanschaf van alle hardware die u wilt prestaties in vergelijking met kosten worden verdeeld.  Aangezien Visual Studio een krachtige toepassing met meerdere threads is, wilt u een VM-grootte die ten minste 2 processors en 7 GB geheugen bevat.  Dit zijn de aanbevolen VM-grootten voor de installatiekopieën van het Visual Studio:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
Zie voor meer informatie over de grootte van de meest recente machines [grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Met Azure, bent u niet gebonden aan uw eerste pick – u kunt uw eerste keuze opnieuw door het formaat van de virtuele machine te verdelen.  Kunt u een nieuwe virtuele machine met een grootte geschiktere inrichten, of u kunt de grootte van uw bestaande virtuele machine naar andere onderliggende hardware.  Zie voor meer informatie [vergroten of verkleinen van een virtuele machine van Windows](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/resize-vm).

## <a name="after-i-get-the-vm-running-then-what"></a>Als ik de virtuele machine wordt uitgevoerd, klikt u vervolgens wat krijgen?
Visual Studio volgt het model 'bring u eigen licentie' in Azure.  Dus is ook aan een installatie op eigen hardware, een van de eerste stappen licentieverlening voor uw installatie van Visual Studio.  U kunt Visual Studio ontgrendelen door beide aanmelden met een Microsoft-account die is gekoppeld aan een Visual Studio-abonnement of ontgrendelt u Visual Studio met de productcode bij de aankoop van uw eerste.  Zie voor meer informatie [Signing in to Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/signing-in-to-visual-studio) en [het ontgrendelen van Visual Studio](https://docs.microsoft.com/en-us/visualstudio/ide/how-to-unlock-visual-studio).

## <a name="after-i-build-out-the-dev-vm-how-do-i-save-capture-it-for-future-or-team-use"></a>Als build van de dev VM, hoe kan ik opslaan (vastleggen) voor toekomstige of het team gebruik?

Het spectrum van ontwikkelomgevingen grote, en er echte kosten in verband met het opzetten van de complexere omgevingen.  Ongeacht de configuratie van uw omgeving, Azure heeft echter behouden die investering eenvoudig door opslaan/vastleggen uw perfect geconfigureerde virtuele machine als een 'basisinstallatiekopie' voor toekomstig gebruik – voor uzelf en/of andere leden van uw team.  Klik, wanneer een nieuwe virtuele machine wordt opgestart, inrichten van de basisinstallatiekopie in plaats van de Marketplace-installatiekopie.

Als een snel overzicht, moet u sysprep en afsluiten de actieve virtuele machine vervolgens *vastleggen (afbeelding 1)* de virtuele machine als een afbeelding via de Azure portal-gebruikersinterface.  Azure slaat de `.vhd` -bestand met de installatiekopie in de storage-account van uw keuze.  Vervolgens de nieuwe installatiekopie wordt weergegeven als een Afbeeldingsbron in de lijst met resources van uw abonnement.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal’s UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Zie afbeelding 1) Een installatiekopie via de Azure portal-gebruikersinterface.*</center>

Zie voor meer informatie [vastleggen van een virtuele machine op een installatiekopie](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/capture-image-resource).

  **Herinnering:** niet vergeten om sysprep de virtuele machine.  Als u deze stap hebt gemist, kan een virtuele machine van de installatiekopie van het Azure niet inrichten.

> [!NOTE]
> U nog steeds kosten in rekening gebracht sommige voor opslag van de installatiekopieën, maar die incrementele kosten is waarschijnlijk gering vergeleken met de personeelskosten opnieuw opbouwen van de virtuele machine vanaf het begin-voor elke persoon in uw team die behoeften van een virtuele machine.  Het kost bijvoorbeeld enkele bedragen maken en opslaan van een installatiekopie 127 GB gedurende een maand die opnieuw kan worden gebruikt door alle leden van uw team.  Deze kosten zijn echter gering vergeleken met elke werknemer investeert voor het bouwen van en valideren van een correct geconfigureerde dev-vak voor het gebruik ervan afzonderlijke uur.

Bovendien uw taken of technologieën mogelijk meer scale – zoals varianten van ontwikkeling configuraties en configuraties met meerdere machines.  U kunt Azure DevTest Labs maken _recepten_ die de opbouw van de 'gouden installatiekopie' automatiseren en beheren van beleidsregels voor uw team de VM's worden uitgevoerd.  [Met behulp van Azure DevTest Labs voor ontwikkelaars](https://docs.microsoft.com/en-us/azure/devtest-lab/devtest-lab-developer-lab) is de beste bron voor meer informatie over DevTest Labs.

## <a name="next-steps"></a>Volgende stappen
Nu u weet over de vooraf geconfigureerde installatiekopieën van de Visual Studio, de volgende stap is een nieuwe virtuele machine maken:

* [Een virtuele machine via de Azure portal maken](quick-create-portal.md)
* [Overzicht van virtuele Machines van Windows](overview.md)
