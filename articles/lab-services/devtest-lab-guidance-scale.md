---
title: Schalen van uw infrastructuur voor Azure DevTest Labs
description: In dit artikel bevat richtlijnen voor het schalen van uw infrastructuur voor Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: spelluru
ms.openlocfilehash: ed6c21762fed331e2f11378ec9b6d91cba9bec11
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48251046"
---
# <a name="scale-up-your-azure-devtest-labs-infrastructure"></a>Schalen van uw infrastructuur voor Azure DevTest Labs
Vóór de implementatie van DevTest Labs op grote schaal, zijn er enkele belangrijke beslissingspunten. Informatie over deze certificaattoewijzing op hoog niveau, kunt een organisatie met ontwerpbeslissingen in de toekomst. Maar moeten deze punten niet bevatten terug vanaf een bewijs van concept van een organisatie. De drie belangrijkste gebieden voor het eerste omhoog plannen zijn:

- Netwerk- en beveiligingsteam
- Abonnement-topologie
- Rollen en verantwoordelijkheden

## <a name="networking-and-security"></a>Netwerk- en beveiligingsteam
Netwerk- en beveiligingsteam zijn fundament voor alle organisaties. Een bedrijfsbrede implementatie vereist een veel diepere analyse, maar er zijn een kleiner aantal vereisten om uit te voeren met succes een bewijs van concept. Enkele belangrijke gebieden van focus zijn onder andere:

- **Azure-abonnement** : implementatie van DevTest Labs, hebt u toegang tot een Azure-abonnement met de juiste rechten heeft om resources te maken. Er zijn verschillende manieren toegang krijgen tot Azure-abonnementen met inbegrip van een Enterprise Agreement en betalen. Zie voor meer informatie over het verkrijgen van toegang tot een Azure-abonnement, [licentieverlening voor Azure voor ondernemingen](https://azure.microsoft.com/pricing/enterprise-agreement/).
- **Toegang tot on-premises bronnen** : sommige organisaties moeten hun bronnen in DevTest Labs hebben toegang tot on-premises bronnen. Er is een beveiligde verbinding tussen uw on-premises-omgeving naar Azure nodig. Daarom is het belangrijk dat u een VPN of Express Route-verbinding voordat u aan de slag omhoog/configureren. Zie voor meer informatie, [virtuele netwerken-overzicht](../virtual-network/virtual-networks-overview.md).
- **Aanvullende beveiligingseisen** – andere beveiligingsvereisten, zoals machine beleid, toegang tot openbare IP-adressen, verbinding met internet zijn scenario's die worden gecontroleerd moeten mogelijk voordat u implementeert een bewijs van concept. 

## <a name="subscription-topology"></a>Abonnement-topologie
Abonnement topologie is een kritische ontwerpoverweging bij het implementeren van DevTest Labs in de onderneming. Het is echter niet vereist duidelijker alle beslissingen pas nadat het testen van het concept is voltooid. Bij het evalueren van het aantal abonnementen die zijn vereist voor een enterprise-implementatie, zijn er twee extreme: 

- Een abonnement voor de hele organisatie
- Abonnement per gebruiker

Vervolgens selecteren we de experts van elke methode.

### <a name="one-subscription"></a>Een abonnement
De aanpak van één abonnement is vaak niet kan worden beheerd in een grote onderneming. Echter, het aantal abonnementen te beperken, biedt de volgende voordelen:

- **Prognose** kosten voor enterprise.  Ook beter budgetteren wordt veel eenvoudiger in één abonnement, omdat alle resources in één groep. Deze aanpak kunt u eenvoudiger maken van beslissingen bij op wanneer kostenbeheer uitoefenen op een bepaald moment in een factureringsperiode meet.
- **Beheerbaarheid** van virtuele machines, artefacten, formules, netwerkconfiguratie, machtigingen, beleid, enzovoort is eenvoudiger omdat alle updates zijn alleen vereist in één abonnement in plaats van updates voor veel abonnementen maken.
- **Netwerken** inspanning sterk vereenvoudigd in één abonnement voor ondernemingen waarbij on-premises connectiviteit vereist is. Virtuele netwerken met elkaar verbinden in abonnementen (hub-spoke-model) is vereist als u meer abonnementen waarvoor aanvullende configuratie, beheer, IP-adresruimten, enzovoort.
- **Samen met collega** wordt eenvoudiger wanneer iedereen in hetzelfde abonnement werkt – bijvoorbeeld, is het gemakkelijker om te opnieuw toewijzen van een virtuele machine naar een collega, delen van teamresources, enzovoort.

### <a name="subscription-per-user"></a>Abonnement per gebruiker
Een afzonderlijk abonnement per gebruiker biedt mogelijkheden tot de alternatieve spectrum gelijk. De voordelen van het aantal abonnementen omvatten:

- **Azure schalen quota** niet meer kan worden aangepast acceptatie. Bijvoorbeeld: op dit moment die met Azure 200 storage-accounts per abonnement kunt. Er zijn operationele quota voor de meeste services in Azure (veel kunnen worden aangepast, kunnen sommige niet). In dit model van een abonnement per gebruiker is het zeer onwaarschijnlijk dat de meeste quota worden bereikt. Zie voor meer informatie over de huidige quota voor Azure vergroten/verkleinen, [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- **Toerekeningen** naar groepen of afzonderlijke ontwikkelaars geworden veel gemakkelijker zodat organisaties ter compensatie van kosten met behulp van hun huidige model.
- **Eigendom en machtigingen** van de DevTest Labs-omgevingen zijn eenvoudig. U bieden ontwikkelaars de toegang op abonnementsniveau en zijn 100% die verantwoordelijk is voor alles, met inbegrip van de configuratie van netwerken, lab-beleid en VM-beheer.

In de onderneming, is er mogelijk onvoldoende beperkingen met betrekking tot het uiterste van het spectrum. Daarom moet u mogelijk om in te stellen van abonnementen op een manier die in het midden van deze extreme valt. Als een best practice moet het doel van een organisatie u het minimale aantal abonnementen als mogelijk te houden rekening met de afdwingen functies die het totale aantal abonnementen verhogen. Nogmaals, abonnement topologie is essentieel is voor implementatie in een onderneming van DevTest Labs, maar moet een concepttest niet uitstellen. Er zijn aanvullende gegevens in de [Governance](devtest-lab-guidance-governance-policy-compliance.md) artikel over het bepalen van het abonnement en lab granulatie in de organisatie.

## <a name="roles-and-responsibilities"></a>Rollen en verantwoordelijkheden
Een DevTest Labs-concepttest heeft drie primaire rollen met gedefinieerde verantwoordelijkheden – abonnementhouder, DevTest Labs-eigenaar, DevTest Labs-gebruiker en eventueel een Inzender.

- **Abonnementhouder** – eigenaar van het abonnement heeft rechten voor het beheer van een Azure-abonnement toewijzen van gebruikers, beheren van beleid, het maken en beheren de netwerktopologie, inclusief aanvragen van quotaverhoging, enzovoort. Raadpleeg [dit artikel](../role-based-access-control/rbac-and-directory-admin-roles.md) voor meer informatie.
- **De eigenaar van DevTest Labs** – de DevTest Labs-eigenaar heeft volledige beheerderstoegang heeft tot het lab. Deze persoon is verantwoordelijk voor het toevoegen/verwijderen van gebruikers, instellingen voor kosten, algemene lab-instellingen en andere virtuele machine/artefact gebaseerde taken beheren. Een lab-eigenaar heeft ook de rechten van een DevTest Labs-gebruiker.
- **DevTest Labs-gebruiker** – de DevTest Labs-gebruiker kunt maken en gebruiken van de virtuele machines in het lab. Deze personen hebben bepaalde minimale administratieve functies op het maken van VM's (starten/stoppen/verwijderen/configureren hun virtuele machines). De gebruikers die geen VM's van andere gebruikers beheren.

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel in deze serie: [indelen van de implementatie van Azure DevTest Labs](devtest-lab-guidance-orchestrate-implementation.md)