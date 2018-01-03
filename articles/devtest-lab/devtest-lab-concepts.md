---
title: Concepten van DevTest Labs | Microsoft Docs
description: Leer de basisconcepten van DevTest Labs en hoe deze kunt u gemakkelijk kunt maken, beheren en bewaken van virtuele machines in Azure
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: v-craic
ms.openlocfilehash: 46271c1122df852b37d4117f9d4008fd74f43d95
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="devtest-labs-concepts"></a>DevTest Labs-concepten
## <a name="overview"></a>Overzicht
De volgende lijst bevat DevTest Labs hoofdconcepten en definities:

## <a name="labs"></a>Labs
Een lab is de infrastructuur die een groep resources omvat, zoals virtuele Machines (VM's), waarmee u betere die bronnen beheren door te geven limieten en quota's.

## <a name="virtual-machine"></a>Virtuele machine
Een Azure VM is een van de verschillende soorten [op aanvraag, schaalbare computerbronnen](https://docs.microsoft.com/azure/app-service/choose-web-site-cloud-service-vm) die Azure biedt. Virtuele machines in Azure biedt u de flexibiliteit van virtualisatie zonder te kopen en beheren van de fysieke hardware die wordt uitgevoerd, hoewel u nog onderhouden van de virtuele machine moet door bepaalde taken uitvoeren, zoals het configureren, patchen en installeren van de software die wordt uitgevoerd op het is.

[Overzicht van Windows virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) biedt u informatie over wat u moet overwegen voordat u maakt een virtuele machine, hoe u het maken en beheren.

## <a name="claimable-vm"></a>Claimable VM
Een Azure Claimable VM is een virtuele machine die beschikbaar is voor gebruik door een lab-gebruiker met machtigingen. Een beheerder lab kan virtuele machines voorbereiden met specifieke basisinstallatiekopieën en artefacten en deze opslaan op een gedeelde groep. Een lab-gebruiker kan vervolgens een actieve virtuele machine uit de groep claim wanneer ze met die specifieke configuratie nodig.

Een virtuele machine die is claimable in eerste instantie niet is toegewezen aan een bepaalde gebruiker, maar wordt weergegeven in de lijst van elke gebruiker onder 'Claimable virtuele machines'. Nadat een virtuele machine wordt opgeëist door een gebruiker, wordt het verplaatst tot hun gebied 'Mijn virtuele machines' en niet langer claimable door een andere gebruiker.

## <a name="environment"></a>Omgeving
In DevTest Labs verwijst een omgeving naar een verzameling Azure-resources in een testomgeving. [Dit blogbericht](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) wordt beschreven hoe u meerdere VM-omgevingen van uw Azure Resource Manager-sjablonen maken.

## <a name="base-images"></a>Basisinstallatiekopieën
Basisinstallatiekopieën zijn installatiekopieën van virtuele machine met de hulpprogramma's en instellingen vooraf is geïnstalleerd en geconfigureerd om snel een virtuele machine. U kunt een virtuele machine inrichten door een bestaande base verzamelen en het toevoegen van een artefact om uw test-agent te installeren. U kunt de ingerichte virtuele machine opslaan als een base zodat de base kan worden gebruikt zonder de test-agent voor elke inrichting van de virtuele machine opnieuw te installeren.

## <a name="artifacts"></a>Artefacten
Artefacten worden gebruikt voor het implementeren en configureren van uw toepassing nadat een virtuele machine is ingericht. Artefacten kunnen het volgende zijn:

* Hulpprogramma's die u wilt installeren op de virtuele machine - zoals agents, Fiddler en Visual Studio.
* Acties die u uitvoeren op de VM wilt, zoals het klonen van een opslagplaats.
* Programma's die u wilt testen.

Artefacten zijn [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) JSON-bestanden met instructies voor het uitvoeren van implementatie en configuratie toepassen.

## <a name="artifact-repositories"></a>Artefact-opslagplaatsen
Artefact-opslagplaatsen zijn git-opslagplaatsen waar de artefacten in worden gecontroleerd. Artefact-opslagplaatsen kunnen worden toegevoegd aan meerdere labs in uw organisatie voor het inschakelen van hergebruik en delen.

## <a name="formulas"></a>Formules
Formules naast basisinstallatiekopieën, bieden een mechanisme voor snelle VM-inrichting. Een formule in DevTest Labs is een lijst van de eigenschap standaardwaarden gebruikt voor het maken van een testomgeving VM.
Met formules, virtuele machines met dezelfde set van eigenschappen - zoals basisinstallatiekopie, VM-grootte, virtueel netwerk en artefacten - kunnen worden gemaakt zonder deze eigenschappen steeds opgeven. Wanneer u een virtuele machine maakt van een formule, de standaardwaarden kunnen worden gebruikt als-is of gewijzigd.

## <a name="policies"></a>Beleidsregels
Beleidsregels helpen bij het beheren van kosten in uw testomgeving. U kunt bijvoorbeeld een beleid voor het automatisch afgesloten VM's op basis van een ingesteld schema maken.

## <a name="caps"></a>Hoofdletters
Caps is een mechanisme om te beperken indien in uw testomgeving. U kunt bijvoorbeeld instellen dat een limiet om te beperken het aantal virtuele machines die per gebruiker of in een testomgeving kunnen worden gemaakt.

## <a name="security-levels"></a>Beveiligingsniveaus
Toegang wordt bepaald door gebaseerd toegangsbeheer (RBAC). Om te begrijpen hoe toegang werkt, is het nuttig om het verschil tussen een machtiging voor een rol en een bereik, zoals gedefinieerd door RBAC.

* Machtiging - een machtiging is een gedefinieerde toegang tot een specifieke actie (bijvoorbeeld leestoegang tot alle virtuele machines).
* Functie - een rol is een reeks machtigingen die kunnen worden gegroepeerd en toegewezen aan een gebruiker. Bijvoorbeeld, de *eigenaar abonnement* rol toegang heeft tot alle bronnen binnen een abonnement.
* Scope - een scope is een niveau binnen de hiërarchie van een Azure-resource, zoals een resourcegroep, een lab dat één of het hele abonnement.

Binnen het bereik van DevTest Labs, er zijn twee typen van rollen om gebruikersmachtigingen te definiëren: labeigenaar en lab-gebruiker.

* Labeigenaar: een labeigenaar heeft toegang tot bronnen in het lab. Daarom kunt de eigenaar van een testomgeving beleid wijzigen, lezen en schrijven van alle virtuele machines, wijzigen van het virtuele netwerk, enzovoort.
* Lab-gebruiker - lab-gebruiker kunt weergeven alle lab-resources, zoals virtuele machines, beleid en virtuele netwerken, maar niet wijzigen beleid of alle virtuele machines die zijn gemaakt door andere gebruikers.

Als u wilt zien hoe u aangepaste rollen maken in DevTest Labs, raadpleegt u het artikel [gebruikersmachtigingen toewijzen aan specifieke labbeleidsregels](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Aangezien scopes hiërarchische, wanneer een gebruiker machtigingen op een bepaalde scope heeft, krijgen ze automatisch deze machtigingen voor elke laag niveau scope die zijn opgenomen. Bijvoorbeeld, als een gebruiker is toegewezen aan de rol van eigenaar abonnement, hebben vervolgens ze toegang tot alle bronnen in een abonnement, inclusief alle virtuele machines, alle virtuele netwerken en alle labs. De eigenaar van een abonnement neemt daarom automatisch de rol van de labeigenaar van het. Het tegenovergestelde is echter niet het geval. Een labeigenaar heeft toegang tot een lab een bereik lager is dan het abonnementsniveau is. De eigenaar van een testomgeving wordt daarom niet worden kunnen virtuele machines of virtuele netwerken of alle bronnen die zich buiten het lab te zien.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
Alle van de concepten beschreven in dit artikel kan worden geconfigureerd met behulp van Azure Resource Manager-sjablonen waarmee u de infrastructuur/configuratie van uw Azure-oplossing bepalen en herhaaldelijk implementeren in een consistente status.

[Overzicht van de structuur en de syntaxis van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) beschrijft de structuur van een Azure Resource Manager-sjabloon en de eigenschappen die beschikbaar in de verschillende secties van een sjabloon zijn.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
[Een lab maken in DevTest Labs](devtest-lab-create-lab.md)
