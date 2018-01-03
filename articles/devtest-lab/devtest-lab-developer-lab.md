---
title: Gebruik Azure DevTest Labs voor ontwikkelaars | Microsoft Docs
description: Informatie over het gebruik van Azure DevTest Labs voor scenario's voor ontwikkelaars.
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 22e070e5-3d1a-49fe-9d4c-5e07cb0b7fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: v-craic
ms.openlocfilehash: f99776a718c03e8af36d45d7006be8ce29208615
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="use-azure-devtest-labs-for-developers"></a>Gebruik Azure DevTest Labs voor ontwikkelaars
Azure DevTest Labs kunnen worden gebruikt voor het implementeren van veel van belangrijke scenario's, maar een van de primaire scenario's wordt DevTest Labs voor ontwikkeling hostmachines voor ontwikkelaars. In dit scenario biedt DevTest Labs de volgende voordelen:

- Ontwikkelaars kunnen hun computers ontwikkeling op aanvraag snel inrichten.
- Ontwikkelaars kunnen hun computers ontwikkeling als dat nodig is gemakkelijk aanpassen.
- Beheerders kunnen kosten beheren door ervoor te zorgen dat:
  - Ontwikkelaars kunnen niet meer virtuele machines dan ze nodig hebben voor de ontwikkeling van ophalen.
  - Virtuele machines worden afgesloten wanneer deze niet in gebruik. 

![DevTest Labs gebruiken voor training](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

In dit artikel leert u over verschillende Azure DevTest Labs-functies die kunnen worden gebruikt om te voldoen aan vereisten voor ontwikkelaars en de gedetailleerde stappen die u volgen kunt om een testomgeving instellen.

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a>Implementatie van ontwikkelaars omgevingen met Azure DevTest Labs
1. **De testomgeving maken** 
   
    Labs vormen het beginpunt in Azure DevTest Labs. Wanneer u een testomgeving maakt, kunt u taken uitvoert zoals gebruikers (ontwikkelaars) toevoegen aan het lab, beleid instellen voor het beheren van kosten, het definiëren van de VM-installatiekopieën die snel kunnen maken en meer.  
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een lab maken in Azure DevTest Labs](devtest-lab-create-lab.md) |Informatie over het maken van een testomgeving in Azure DevTest Labs in de Azure portal. |
2. **Virtuele machines in minuten met behulp van vooraf gedefinieerde marketplace-installatiekopieën en aangepaste installatiekopieën maken** 
   
    U kunt vooraf gedefinieerde installatiekopieën van een groot aantal afbeeldingen kiezen in Azure Marketplace en zodat ze beschikbaar zijn in de testomgeving. Als de kant-en-afbeeldingen niet aan uw vereisten voldoet, kunt u een aangepaste installatiekopie maken door het maken van een testomgeving VM die gebruikmaakt van een installatiekopie van een vooraf gedefinieerde uit Azure Marketplace, installatie van de software die u nodig hebt, en het opslaan van de virtuele machine als een aangepaste installatiekopie in de testomgeving.

    Als u aangepaste installatiekopieën gebruikt, kunt u overwegen de fabrieksinstellingen van een installatiekopie te maken en distribueren van uw afbeeldingen. De fabrieksinstellingen van een installatiekopie is een configuratie als code-oplossing die regelmatig bouwt en uw geconfigureerde installatiekopieën automatisch distribueert. Hierdoor wordt de tijd die nodig zijn om het systeem handmatig configureren nadat een virtuele machine is gemaakt met het Basisbesturingssysteem opgeslagen.
  
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Azure Marketplace-installatiekopieën configureren](devtest-lab-configure-marketplace-images.md) |Meer informatie over hoe u kunt geaccepteerde Azure Marketplace-installatiekopieën, beschikbaar voor selectie alleen de installatiekopieën die u voor de ontwikkelaars wilt maken.|
   | [Een aangepaste installatiekopie maken](devtest-lab-create-template.md) |Een aangepaste installatiekopie maken door het vooraf installeren van de software die u nodig hebt, zodat ontwikkelaars snel een virtuele machine met behulp van de aangepaste installatiekopie kunnen maken.|
   | [Meer informatie over de installatiekopie-factory](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |Bekijk een video waarin wordt beschreven hoe instellen en gebruiken van een installatiekopie-factory.|

3. **Herbruikbare sjablonen maken voor ontwikkelaars machines** 
   
    Een formule in Azure DevTest Labs is een lijst van de eigenschap standaardwaarden gebruikt voor het maken van een virtuele machine. U kunt een formule in de testomgeving door het verzamelen van een installatiekopie van een VM-grootte (een combinatie van CPU en RAM) en een virtueel netwerk maken. Elke ontwikkelaar kan de formule zien in de testomgeving en deze gebruiken voor het maken van een virtuele machine. 
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [DevTest Labs formules voor het maken van virtuele machines beheren](devtest-lab-manage-formulas.md) |Meer informatie over hoe u een formule kunt maken met een installatiekopie van een VM-grootte (combinatie van CPU en RAM) en een virtueel netwerk ophalen.|

4. **Artefacten zodat flexibele aanpassing van de virtuele machine maken**

   Artefacten worden gebruikt voor het implementeren en configureren van uw toepassing nadat een virtuele machine is ingericht. Artefacten kunnen het volgende zijn:

   - Hulpprogramma's die u wilt installeren op de virtuele machine - zoals agents, Fiddler en Visual Studio.
   - Acties die u uitvoeren op de VM wilt, zoals het klonen van een opslagplaats.
   - Programma's die u wilt testen.

   Veel artefacten zijn al beschikbaar out-of-the-box. U kunt uw eigen aangepaste artefacten maken als u wilt dat meer aanpassen voor uw specifieke behoeften.

   Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Aangepaste artefacten maken voor uw DevTest Labs VM](devtest-lab-artifact-author.md) |Maak uw eigen aangepaste artefacten voor de virtuele machines in uw testomgeving.|
   | [Voeg een Git-opslagplaats voor het opslaan van aangepaste artefacten en Azure Resource Manager-sjablonen voor gebruik in Azure DevTest Labs](devtest-lab-add-artifact-repo.md) |Informatie over het opslaan van uw aangepaste artefacten in uw eigen persoonlijke Git-opslagplaats.|

5. **Beheer van kosten**
   
    Azure DevTest Labs kunt u een beleid in een testomgeving om Geef het maximum aantal VM's die kunnen worden gemaakt door een ontwikkelaar in de testomgeving instellen. 
   
    Als uw developer-team heeft een set werken planning en u wilt stoppen van de virtuele machines op een bepaald tijdstip van de dag en vervolgens automatisch opnieuw opgestart ze de volgende dag, kunt u die eenvoudig uitvoeren door het instellen van beleidsregels voor automatisch afsluiten en automatisch starten in de testomgeving. 
   
    Ten slotte, als ontwikkeling van apps voltooid is, kunt u verwijderen de virtuele machines in één keer een één PowerShell-script uit te voeren. 
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Beleid voor lab maken](devtest-lab-set-lab-policy.md) |Kosten beheren door het instellen van beleidsregels in een testomgeving. |
   | [Verwijder alle lab virtuele machines met een PowerShell-script](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Verwijder alle labs in één bewerking als ontwikkeling voltooid is.|

1. **Een virtueel netwerk toevoegen aan een virtuele machine** 
   
    DevTest Labs maakt een nieuw virtueel netwerk (VNET) als een lab is gemaakt. Als u uw eigen VNET hebt geconfigureerd: bijvoorbeeld via ExpressRoute of site-naar-site VPN-kunt u dit VNET toevoegen aan de instellingen van het virtuele netwerk van uw lab zodat deze beschikbaar is bij het maken van virtuele machines.

    Daarnaast is er een beschikbaar die wordt een virtuele machine toevoegen aan een domein, wanneer de virtuele machine wordt gemaakt van Azure Active Directory domain join artefact. 
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een virtueel netwerk configureren in Azure DevTest Labs](devtest-lab-configure-vnet.md) |Informatie over het configureren van een virtueel netwerk in Azure DevTest Labs met de Azure portal.|

6. **De testomgeving delen met elke ontwikkelaar**
   
    Labs zijn rechtstreeks toegankelijk via een koppeling die u met uw ontwikkelaars deelt. Ze hoeft te hebben van een Azure-account, zolang ze beschikken over een [Microsoft-account](devtest-lab-faq.md#what-is-a-microsoft-account). Ontwikkelaars zichtbaar niet voor virtuele machines die zijn gemaakt door andere ontwikkelaars.  
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een ontwikkelaar toevoegen aan een lab in Azure DevTest Labs](devtest-lab-add-devtest-user.md) |De Azure portal gebruiken voor ontwikkelaars toevoegen aan uw testomgeving.|
   | [Ontwikkelaars toevoegen aan de testomgeving met een PowerShell-script](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |PowerShell gebruiken voor het automatiseren van ontwikkelaars van toe te voegen aan uw testomgeving. |
   | [Een koppeling naar het lab](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Meer informatie over hoe ontwikkelaars rechtstreeks toegang tot een lab via een hyperlink.|

7. **Lab maken voor meer teams automatiseren** 
   
    U kunt maken van de testomgeving, met inbegrip van aangepaste instellingen door Resource Manager-sjabloon maken en deze opnieuw maken van identieke labs automatiseren. 
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een lab met een Resource Manager-sjabloon maken](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Labs in Azure DevTest Labs met Resource Manager-sjablonen maken. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

