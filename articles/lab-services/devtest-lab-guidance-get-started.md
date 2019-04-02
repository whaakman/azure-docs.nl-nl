---
title: Aan de slag met Azure DevTest Labs
description: Dit artikel bevat de primaire scenario's voor het gebruik van Azure DevTest Labs en twee algemene paden om te beginnen met de service in uw organisatie.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 219697451174811fab7880ba0d5930ba85edf8fb
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58803796"
---
# <a name="get-started-with-using-azure-devtest-labs"></a>Aan de slag met Azure DevTest Labs
Afhankelijk van de behoeften van een onderneming, kan de DevTest Labs worden geconfigureerd om te voldoen aan verschillende vereisten.  Dit artikel worden de gangbare scenario's. Elk scenario bevat informatie over voordelen gebracht met behulp van DevTest Labs en resources te gebruiken voor het implementeren van deze scenario's.  

- Developer bureaubladen
- Testomgevingen
- Trainingssessies, praktijksessies en hackathons
- Sandbox onderzoeken
- Leslokaallabs

## <a name="developer-desktops"></a>Developer bureaubladen
Ontwikkelaars hebben vaak verschillende vereisten voor de ontwikkeling van machines voor verschillende projecten. Met DevTest Labs kunnen ontwikkelaars hebben altijd toegang tot on-demand virtuele machines die aan de behoeften van hun meest voorkomende scenario's zijn geconfigureerd. DevTest Labs biedt de volgende voordelen:

- Organisaties kunnen bieden algemene ontwikkeling machines ervoor te zorgen dat de consistentie tussen teams.
- Ontwikkelaars kunnen snel inrichten voor hun ontwikkeling-machines op aanvraag of [claimen van een bestaande, vooraf geconfigureerde machine](devtest-lab-add-claimable-vm.md).
- Ontwikkelaars kunnen resources inrichten op een manier selfservice zonder machtigingen op abonnementsniveau.
- IT of beheerders kunt [vooraf definiëren de netwerktopologie](devtest-lab-configure-vnet.md) en ontwikkelaars kunnen rechtstreeks worden gebruikt in een eenvoudige en intuïtieve manier zonder speciale toegang.
- Ontwikkelaars kunnen eenvoudig [aanpassen](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) hun ontwikkeling machines zo nodig.
- Beheerders kunnen kosten beheren door ervoor te zorgen dat:
    - Ontwikkelaars [meer virtuele machines kan niet worden opgehaald](devtest-lab-set-lab-policy.md#set-virtual-machines-per-user) dan ze nodig hebt voor ontwikkeling
    - [VM's zijn afgesloten](devtest-lab-set-lab-policy.md#set-auto-shutdown) wanneer deze niet in gebruik
    - Alleen [waarmee een subset van de VM-exemplaargrootten](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes) voor de specifieke labs
    - [Beheren van kosten doelen en meldingen](devtest-lab-configure-cost-management.md) voor elk lab.

Zie voor meer informatie, [met Azure DevTest Labs voor ontwikkelaars](devtest-lab-developer-lab.md). 

## <a name="test-environments"></a>Testomgevingen
Het maken en beheren van testomgevingen binnen een onderneming kunnen een aanzienlijke inspanning vereisen. Met DevTest Labs, kunnen testomgevingen worden eenvoudig gemaakt, bijgewerkt of gedupliceerd. Hiermee kunnen teams voor toegang tot een volledig geconfigureerde omgeving als dat nodig. In dit scenario biedt DevTest Labs de volgende voordelen:

- Organisaties kunnen bieden algemene testomgevingen ervoor te zorgen dat de consistentie tussen teams.
- Testers kunnen de nieuwste versie van de toepassing testen door snel Windows en Linux-omgevingen inrichten met behulp van herbruikbare sjablonen.
- Beheerders verbinding kunnen maken van de testomgeving met Azure DevOps, DevOps-scenario's inschakelen
- Lab-eigenaar kunnen u kosten beheren door ervoor te zorgen dat:
    - [VM's in een omgeving zijn afgesloten](devtest-lab-set-lab-policy.md#set-auto-shutdown) wanneer deze niet in gebruik
    - Alleen [waarmee een subset van de VM-exemplaargrootten voor](devtest-lab-set-lab-policy.md#set-allowed-virtual-machine-sizes) de specifieke labs
    - [Beheren van kosten doelen en meldingen](devtest-lab-configure-cost-management.md) voor elk lab.

Zie voor meer informatie, [testomgevingen met Azure DevTest Labs voor virtuele machine en PaaS](devtest-lab-test-env.md).

## <a name="sandboxed-investigations"></a>Sandbox onderzoeken
Ontwikkelaars onderzoeken vaak verschillende technologieën of het ontwerp van infrastructuur. Standaard worden alle omgevingen die zijn gemaakt met DevTest Labs worden gemaakt in hun eigen resourcegroep bevinden. De DevTest Labs-gebruiker ontvangt alleen lezen-toegang tot deze bronnen. Echter, voor ontwikkelaars die meer controle nodig hebt, een lab-instelling kan worden bijgewerkt zodat [Inzender-rechten](https://azure.microsoft.com/updates/azure-devtest-labs-view-and-set-access-rights-to-an-environment-rg/) naar de oorspronkelijke DevTest Labs-gebruiker voor elke omgeving die ze maken.  Met DevTest Labs, kunnen de machtiging Inzender automatisch doorvoert in omgevingen waarin ze in het lab maken op ontwikkelaars worden opgegeven.  In dit scenario kan ontwikkelaars toevoegen en/of Azure-resources te wijzigen als ze nodig hebben voor hun ontwikkeling- of testomgevingen. De [kosten per resource](devtest-lab-configure-cost-management.md#view-cost-by-resource) op de pagina kunt Lab-eigenaren voor het bijhouden van de kosten van elke omgeving moet worden gebruikt voor onderzoek.

Zie voor meer informatie, [toegangsrechten instellen op een resourcegroep omgeving](https://aka.ms/dtl-sandbox).

## <a name="trainings-hands-on-labs-and-hackathons"></a>U kunt, praktijksessies en hackathons 
Een lab in Azure DevTest Labs fungeert als een geweldige container voor tijdelijke activiteiten zoals workshops, praktijksessies, u kunt of hackathons.  De service kunt u het maken van een lab waarin u aangepaste sjablonen die elke leerling gebruiken kunt voor het maken van identieke en geïsoleerde omgevingen voor training kunt opgeven. In dit scenario biedt DevTest Labs de volgende voordelen:

- [Beleid](devtest-lab-set-lab-policy.md) zorgen stagiairs krijgen alleen het aantal resources, zoals virtuele machines, die ze nodig hebben.
- Vooraf geconfigureerde en gemaakte machines zijn [geclaimd](devtest-lab-add-claimable-vm.md) met één actie van leerling.
- Labs met stagiairs worden gedeeld door het openen van [URL voor de testomgeving](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab).
- [Vervaldatums](devtest-lab-add-vm.md#steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs) op virtuele machines ervoor te zorgen dat machines worden verwijderd nadat ze zijn niet meer nodig hebt.
- Het is eenvoudig te [verwijderen van een lab](devtest-lab-delete-lab-vm.md#delete-a-lab) en alle [gerelateerde resources](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) wanneer de training ligt boven de.

Zie voor meer informatie, [Azure DevTest Labs gebruiken voor training](devtest-lab-training-lab.md).  

## <a name="proof-of-concept-vs-scaled-deployment"></a>Testen van concept versus uitgebreide implementatie
Als u DevTest Labs verkennen besluit, zijn er twee algemene paden doorsturen: Bewijs van Concept vs schaal implementatie.  

Een **uitgebreide implementatie** bestaat uit weken/maanden van controleren en planning met de opzet van de implementatie van DevTest Labs voor de hele onderneming met honderden of duizenden ontwikkelaars.

Een **concepttest** implementatie is gericht op een geconcentreerd inspanningen van één team tot stand brengen van organisatie-waarde. Terwijl het verleidelijk zijn kan om na te denken van een uitgebreide implementatie, doorgaans de aanpak kan niet vaker dan het bewijs van concept-optie. Daarom raden wij aan dat u klein beginnen, leer van de eerste team, herhaalt u de dezelfde benadering met twee of drie extra teams en vervolgens te plannen voor een uitgebreide implementatie op basis van de kennis opgedaan uit. Voor een geslaagde bewijs van concept, wordt aangeraden dat u een of twee teams kiest, en identificeren van de scenario's (dev-omgeving vs testomgevingen), hun huidige gebruiksvoorbeelden document, en DevTest Labs implementeert.

## <a name="next-steps"></a>Volgende stappen
Lees de volgende artikelen:

- [DevTest Labs-concepten](devtest-lab-concepts.md)
- [Veelgestelde vragen DevTest Labs](devtest-lab-faq.md)

