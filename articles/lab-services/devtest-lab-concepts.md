---
title: DevTest Labs-concepten | Microsoft Docs
description: Leer de basisconcepten van DevTest Labs en hoe deze kan gemakkelijk te maken, beheren en controleren van virtuele machines van Azure
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 1e35513d5a5a799b1f5e45cf9a5aa97c083e2087
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725198"
---
# <a name="devtest-labs-concepts"></a>DevTest Labs-concepten
## <a name="overview"></a>Overzicht
De volgende lijst bevat belangrijke DevTest Labs-concepten en definities:

## <a name="labs"></a>Labs
Een lab is de infrastructuur die een groep resources omvat, zoals virtuele Machines (VM's), waarmee u betere die bronnen beheren door limieten en quota's op te geven.

## <a name="virtual-machine"></a>Virtuele machine
Een Azure-VM is een van verschillende typen [on-demand, schaalbare computerbronnen](https://docs.microsoft.com/azure/app-service/overview-compare) die Azure biedt. Azure-VM's bieden u de flexibiliteit van virtualisatie zonder te hoeven kopen en te onderhouden van de fysieke hardware die wordt uitgevoerd, maar u wel onderhouden van de virtuele machine moet door het uitvoeren van bepaalde taken, zoals het configureren, patchen en installeren van de software die wordt uitgevoerd op het is.

[Overzicht van Windows-machines in Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) biedt u informatie over wat u moet overwegen voordat u het maken van een virtuele machine, hoe u deze maakt en hoe u deze beheert.

## <a name="claimable-vm"></a>Claimbare virtuele machine
Een Azure-Claimbare virtuele machine is een virtuele machine die is beschikbaar voor gebruik door een lab-gebruiker met machtigingen. Een lab-beheerder kunt virtuele machines voorbereiden met specifieke basisinstallatiekopieën en artefacten en deze opslaan op een gedeelde groep. Een lab-gebruiker kan vervolgens een werkende virtuele machine uit de pool claim wanneer ze een met die specifieke configuratie nodig.

Een virtuele machine die is claimbare in eerste instantie niet is toegewezen aan een bepaalde gebruiker, maar worden weergegeven in de lijst van elke gebruiker onder 'Claimbare virtuele machines'. Nadat een virtuele machine wordt aangevraagd door een gebruiker, wordt het verplaatst tot hun gebied 'Mijn virtuele machines' en is niet langer claimbare door een andere gebruiker.

## <a name="environment"></a>Omgeving
In DevTest Labs verwijst een omgeving naar een verzameling van Azure-resources in een testomgeving. [Dit blogbericht](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) wordt beschreven hoe u multi-VM-omgevingen maken vanuit de Azure Resource Manager-sjablonen.

## <a name="base-images"></a>Basisinstallatiekopieën
Basisinstallatiekopieën zijn VM-installatiekopieën met de hulpprogramma's en instellingen vooraf geïnstalleerd en geconfigureerd voor het snel een virtuele machine te maken. U kunt een virtuele machine inrichten door een bestaande base verzamelen en het toevoegen van een artefact als uw test-agent wilt installeren. U kunt vervolgens de ingerichte virtuele machine opslaan als basis, zodat de base kan worden gebruikt zonder te hoeven de test-agent opnieuw installeren voor de inrichting van de virtuele machine.

## <a name="artifacts"></a>Artefacten
Artefacten worden gebruikt om te implementeren en configureren van uw toepassing nadat een virtuele machine is ingericht. Artefacten kunnen het volgende zijn:

* Hulpprogramma's die u wilt installeren op de VM - zoals agents, Fiddler en Visual Studio.
* Acties die u uitvoeren op de VM wilt-zoals een opslagplaats te klonen.
* Programma's die u wilt testen.

Artefacten zijn [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) JSON-bestanden met instructies voor het uitvoeren van de implementatie en configuratie toe te passen.

## <a name="artifact-repositories"></a>Artefact opslagplaatsen
Artefact opslagplaatsen zijn git-opslagplaatsen waar de artefacten worden ingecheckt. Artefact opslagplaatsen kunnen worden toegevoegd aan meerdere labs in uw organisatie delen en opnieuw inschakelen.

## <a name="formulas"></a>Formules
Formules, naast de basisinstallatiekopieën, bieden een mechanisme voor snelle inrichting van virtuele machine. Een formule in DevTest Labs is een lijst van de eigenschap standaardwaarden gebruikt voor het maken van een lab-VM.
Met virtuele machines met dezelfde set van eigenschappen - zoals basisinstallatiekopie, VM-grootte, virtueel netwerk en artefacten - formules kunnen worden gemaakt zonder dat u hoeft op te geven deze eigenschappen telkens. Bij het maken van een virtuele machine van een formule, de standaardwaarden kunnen worden gebruikt als-is of gewijzigd.

## <a name="policies"></a>Beleidsregels
Beleid voor hulp bij het beheer over de kosten in uw testomgeving. U kunt bijvoorbeeld een beleid om virtuele machines op basis van een gedefinieerde planning automatisch af te maken.

## <a name="caps"></a>Hoofdletters
Caps is een mechanisme voor het minimale verspilling in uw testomgeving. U kunt bijvoorbeeld een limiet om te beperken van het aantal virtuele machines dat kan worden gemaakt per gebruiker of in een testomgeving instellen.

## <a name="security-levels"></a>Beveiligingsniveaus
Toegang wordt bepaald door op rollen gebaseerd toegangsbeheer (RBAC). Voor meer informatie over de werking van toegang, is het handig om te weten wat de verschillen tussen een machtiging, een rol en een bereik, zoals gedefinieerd door RBAC.

* Machtiging - een machtiging is een gedefinieerde toegang tot een specifieke actie (bijvoorbeeld lezen-toegang tot alle virtuele machines).
* Rol: een rol is een reeks machtigingen die kunnen worden gegroepeerd en toegewezen aan een gebruiker. Bijvoorbeeld, de *abonnementhouder* rol heeft toegang tot alle resources binnen een abonnement.
* Bereik - een scope is een niveau binnen de hiërarchie van een Azure-resource, zoals een resourcegroep, een enkel lab of het hele abonnement.

Binnen het bereik van DevTest Labs, er zijn twee soorten rollen om gebruikersmachtigingen te definiëren: lab-eigenaar en lab-gebruiker.

* Lab-eigenaar - lab-eigenaar heeft toegang tot alle resources in het lab. Daarom kan lab-eigenaar beleid wijzigen, lezen en schrijven van virtuele machines, wijzigen van het virtuele netwerk, enzovoort.
* Lab-gebruiker - een lab-gebruiker alle labresources, zoals virtuele machines, beleidsregels en virtuele netwerken kunt weergeven, maar kan beleidsregels niet wijzigen of alle virtuele machines die zijn gemaakt door andere gebruikers.

Raadpleeg het artikel voor meer informatie over aangepaste rollen maken in DevTest Labs, [gebruikersmachtigingen verlenen voor specifieke lab beleid](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Omdat bereiken hiërarchische, zijn wanneer een gebruiker machtigingen met een bepaald bereik heeft, krijgen ze automatisch deze machtigingen op elke op lager niveau scope die zijn opgenomen. Bijvoorbeeld, als een gebruiker is toegewezen aan de rol van de eigenaar van het abonnement, hebben klikt u vervolgens ze toegang tot alle bronnen in een abonnement, waaronder alle virtuele machines, alle virtuele netwerken en alle labs. De eigenaar van een abonnement neemt daarom automatisch de rol van de eigenaar van het lab. Het tegenovergestelde is echter niet het geval. Een lab-eigenaar heeft toegang tot een lab een lagere omvang dan het abonnementsniveau is. Daarom is lab-eigenaar niet mogelijk om te zien van virtuele machines of virtuele netwerken of alle resources die zich buiten het lab.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
Alle van de concepten die worden beschreven in dit artikel kan worden geconfigureerd met behulp van Azure Resource Manager-sjablonen, zodat u de infrastructuur/configuratie van uw Azure-oplossing definiëren en implementeren van het herhaaldelijk een consistente status hebben.

[Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) beschrijft de structuur van een Azure Resource Manager-sjabloon en de eigenschappen die beschikbaar in de verschillende secties van een sjabloon zijn.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
[Een lab maken in DevTest Labs](devtest-lab-create-lab.md)
