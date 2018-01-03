---
title: Azure DevTest Labs gebruiken voor training | Microsoft Docs
description: Informatie over het gebruik van Azure DevTest Labs voor training scenario's.
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: v-craic
ms.openlocfilehash: 96f7ed2084fbfc14c6bac09819ce3724b4168995
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="use-azure-devtest-labs-for-training"></a>Azure DevTest Labs gebruiken voor training
Azure DevTest Labs kan worden gebruikt voor het implementeren van veel van belangrijke scenario's naast ontwikkelen en testen. Een van de scenario's is voor het instellen van een testomgeving voor training. Azure DevTest Labs kunt u voor het maken van een testomgeving waarin u aangepaste sjablonen die elke leerling gebruiken kunt voor het maken van identieke en geïsoleerde omgevingen voor training kunt opgeven. U kunt beleid om ervoor te zorgen dat training omgevingen beschikbaar voor elke leerling zijn alleen wanneer ze deze nodig en voldoende resources - zoals virtuele machines - vereist voor de training bevatten toepassen. Ten slotte kunt u de testomgeving eenvoudig delen met stagiairs die ze in één klik kunnen openen.

![DevTest Labs gebruiken voor training](./media/devtest-lab-training-lab/devtest-lab-training.png)

Azure DevTest Labs voldoet aan de volgende vereisten die nodig zijn voor het trainen in een virtuele omgeving: 

* Stagiairs zien virtuele machines die zijn gemaakt door andere stagiairs niet
* Elke machine training moet identiek zijn
* Stagiairs kunnen hun omgeving training snel inrichten
* Kosten beheren door ervoor te zorgen dat stagiairs kunnen niet worden gebruikt voor het ophalen van meer virtuele machines dan ze nodig hebben om de trainings- en ook virtuele machines afsluiten wanneer ze niet gebruiken
* De testomgeving training eenvoudig delen met elke leerling
* De testomgeving training opnieuw gebruiken

In dit artikel leert u verschillende functies van Azure DevTest Labs die kunnen worden gebruikt om te voldoen aan de vereisten van de eerder beschreven training en de gedetailleerde stappen die u volgen kunt om een testomgeving voor training instellen.  

## <a name="implementing-training-with-azure-devtest-labs"></a>Training met Azure DevTest Labs implementeren
1. **De testomgeving maken** 
   
    Labs vormen het beginpunt in Azure DevTest Labs. Wanneer u een testomgeving maakt, kunt u taken uitvoeren zoals als gebruikers (stagiairs) toevoegen aan het lab, beleid instellen voor beheerkosten, het definiëren van de VM-installatiekopieën die u kunnen snel maken en meer.   
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een lab maken in Azure DevTest Labs](devtest-lab-create-lab.md) |Informatie over het maken van een testomgeving in Azure DevTest Labs in de Azure portal. |
2. **Training virtuele machines in minuten met behulp van vooraf gedefinieerde marketplace-installatiekopieën en aangepaste installatiekopieën maken** 
   
    U kunt vooraf gedefinieerde installatiekopieën van een groot aantal afbeeldingen kiezen in Azure Marketplace en beschikbaar maken voor de deelnemers in de testomgeving. Als de kant-en-afbeeldingen niet aan uw vereisten voldoet, kunt u een aangepaste installatiekopie maken door het maken van een testomgeving VM die gebruikmaakt van een installatiekopie van een vooraf gedefinieerde uit Azure Marketplace, installatie van de software die u nodig hebt om de training en opslaan van de virtuele machine als aangepaste installatiekopie in de testomgeving. 
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Azure Marketplace-installatiekopieën configureren](devtest-lab-configure-marketplace-images.md) |Meer informatie over hoe u kunt de Azure Marketplace-installatiekopieën geaccepteerde; het beschikbaar maken voor de selectie van alleen de installatiekopieën die u wilt voor de training. |
   | [Een aangepaste installatiekopie maken](devtest-lab-create-template.md) |Een aangepaste installatiekopie maken door de software die u nodig hebt voor de training zodat stagiairs kunnen snel een virtuele machine maken met de aangepaste installatiekopie vooraf te installeren. |
3. **Herbruikbare sjablonen maken voor training machines** 
   
    Een formule in Azure DevTest Labs is een lijst van de eigenschap standaardwaarden gebruikt voor het maken van een virtuele machine. U kunt een formule in de testomgeving door het verzamelen van een installatiekopie van een VM-grootte (een combinatie van CPU en RAM) en een virtueel netwerk maken. Elke leerling kan de formule zien in de testomgeving en deze gebruiken voor het maken van een virtuele machine. 
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [DevTest Labs formules voor het maken van virtuele machines beheren](devtest-lab-manage-formulas.md) |Meer informatie over hoe u een formule kunt maken met een installatiekopie van een VM-grootte (combinatie van CPU en RAM) en een virtueel netwerk ophalen. |
4. **Beheer van kosten**
   
    Azure DevTest Labs kunt u een beleid ingesteld in een testomgeving om het maximum aantal VM's die kunnen worden gemaakt door in de testomgeving toetsen opgeven. 
   
    Als u bent bezig met Meerdaags training en wilt stoppen van de virtuele machines op een bepaald tijdstip van de dag en vervolgens automatisch opnieuw opgestart ze de volgende dag, kunt u eenvoudig bereiken die door in te stellen automatisch afsluiten en automatisch starten van beleid in de testomgeving. 
   
    Ten slotte is het zo dat als training voltooid is u de virtuele machines in één keer kunt verwijderen door het uitvoeren van een enkel PowerShell-script. 
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Beleid voor lab maken](devtest-lab-set-lab-policy.md) |Kosten beheren door het instellen van beleidsregels in een testomgeving. |
   | [Verwijder alle lab virtuele machines met een PowerShell-script](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |Verwijder alle labs in één bewerking wanneer de training voltooid is. |
5. **De testomgeving met elke leerling delen**
   
    Labs zijn rechtstreeks toegankelijk via een koppeling die u met uw stagiairs deelt. Uw stagiairs hoeft te hebben van een Azure-account, zolang ze beschikken over een [Microsoft-account](devtest-lab-faq.md#what-is-a-microsoft-account). Stagiairs zichtbaar niet voor virtuele machines die zijn gemaakt door andere stagiairs.  
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Toetsen toevoegen aan een lab in Azure DevTest Labs](devtest-lab-add-devtest-user.md) |Gebruik de Azure-portal stagiairs toevoegen aan uw testomgeving training. |
   | [Stagiairs toevoegen aan de testomgeving met een PowerShell-script](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |PowerShell gebruiken voor het automatiseren van toe te voegen stagiairs aan uw lab training. |
   | [Een koppeling naar het lab](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |Meer informatie over hoe een lab rechtstreeks toegankelijk zijn via een hyperlink. |
6. **De testomgeving opnieuw gebruiken** 
   
    U kunt maken van de testomgeving, met inbegrip van aangepaste instellingen door Resource Manager-sjabloon maken en deze opnieuw maken van identieke labs automatiseren. 
   
    Meer leren door te klikken op de koppelingen in de volgende tabel:
   
   | Taak | Wat u leert |
   | --- | --- |
   | [Een lab met een Resource Manager-sjabloon maken](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Labs in Azure DevTest Labs met Resource Manager-sjablonen maken. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

