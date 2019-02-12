---
title: Beheer van Azure DevTest Labs-infrastructuur
description: Dit artikel bevat richtlijnen voor het beheer van Azure DevTest Labs-infrastructuur.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: christianreddington
manager: femila
editor: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: chredd
ms.reviewer: anthdela,juselph
ms.openlocfilehash: f4459d43452aa2066f4a7f841bb7d92914e7a073
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55998309"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---company-policy-and-compliance"></a>Beheer van infrastructuur voor Azure DevTest Labs - bedrijfsbeleid en naleving
In dit artikel bevat richtlijnen voor het betreffende bedrijfsbeleid en naleving voor Azure DevTest Labs-infrastructuur. 

## <a name="public-vs-private-artifact-repository"></a>Openbare en persoonlijke artefactopslagplaats

### <a name="question"></a>Vraag
Wanneer moet een organisatie een openbare artefactenopslag gebruiken versus persoonlijke artefactopslagplaats in DevTest Labs?

### <a name="answer"></a>Antwoord
De [openbare artefactenopslag](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) biedt een eerste set met software-updatepakketten die meestal worden gebruikt. Dit helpt bij de snelle implementatie zonder te hoeven investeren tijd om algemene hulpprogramma's voor ontwikkelaars en invoegtoepassingen te reproduceren. U kunt implementeren hun eigen privé-opslagplaats. U kunt een openbare en een privé-opslagplaats gelijktijdig gebruiken. U kunt er ook voor kiezen om uit te schakelen van de openbare opslagplaats. De criteria voor het implementeren van een privé-opslagplaats moeten worden aangestuurd door de volgende vragen en overwegingen:

- Beschikt over de organisatie een vereiste zakelijke gelicentieerde software hebben als onderdeel van hun aanbieding DevTest Labs? Als het antwoord Ja is, moet een privé-opslagplaats worden gemaakt.
- De organisatie aangepaste software waarmee een bepaalde bewerking, die vereist als onderdeel van het algehele proces van inrichting is ontwikkelen? Als het antwoord Ja is, moet een privé-opslagplaats worden geïmplementeerd.
- Als organisatie governance beleid vereist dat isolatie en externe opslagplaatsen niet onder beheer van de directe configuratie door de organisatie vallen, moet een persoonlijke artefactopslagplaats worden geïmplementeerd. Als onderdeel van dit proces kan een initiële kopie van de openbare opslagplaats worden gekopieerd en geïntegreerd met de privé-opslagplaats. De openbare opslagplaats kan vervolgens worden uitgeschakeld zodat niemand binnen de organisatie toegang heeft tot deze niet meer. Deze aanpak zorgt ervoor dat alle gebruikers binnen de organisatie hebben slechts één enkele opslagruimte die door de organisatie is goedgekeurd en afwijkende configuraties te minimaliseren.

### <a name="single-repository-or-multiple-repositories"></a>Één opslagplaats of meerdere opslagplaatsen 

### <a name="question"></a>Vraag
Moet een organisatie van plan bent voor één enkele opslagruimte of meerdere opslagplaatsen toe te staan?

### <a name="answer"></a>Antwoord
Als onderdeel van de algehele governance van uw organisatie en beheer configuratiestrategie, raden wij aan dat u een centrale opslagplaats. Wanneer u meerdere opslagplaatsen, kunnen ze silo's van niet-beheerde software worden na verloop van de tijd. Met een centrale opslagplaats, kunnen meerdere teams artefacten uit deze opslagplaats voor hun projecten gebruiken. Het wordt afgedwongen standaardisatie, beveiliging, eenvoudig beheer en elimineert de duplicatie van inspanningen. Als onderdeel van de centralisatie, worden de volgende acties aanbevolen procedures voor het beheer van op de lange termijn en duurzaamheid:

- De Azure-opslagplaatsen koppelen aan dezelfde Azure Active Directory-tenant die het Azure-abonnement wordt gebruikt voor verificatie en autorisatie.
- Maak een groep met de naam **alle DevTest Labs ontwikkelaars** in Azure Active Directory die centraal wordt beheerd. Elke ontwikkelaar die draagt bij aan de ontwikkeling van artefacten moet worden geplaatst in deze groep.
- De dezelfde Azure Active Directory-groep kan worden gebruikt voor toegang tot de opslagplaats van de Azure-opslagplaatsen en de testomgeving.
- In de Azure-opslagplaatsen, moet vertakkingen of splits worden gebruikt naar een afzonderlijke een in-ontwikkeling-opslagplaats van de primaire productie-opslagplaats. Inhoud is alleen toegevoegd aan de master-vertakking met een pull-aanvraag na een overzicht van de juiste code. Zodra de revisor van de code wordt de wijziging goedgekeurd, worden de bijgewerkte code door een ontwikkelaar lead, die verantwoordelijk voor onderhoud van de master-vertakking is, samengevoegd. 

## <a name="corporate-security-policies"></a>Zakelijke beveiligingsbeleid

### <a name="question"></a>Vraag
Hoe kan een organisatie dat zakelijke beveiligingsbeleid wordt voldaan

### <a name="answer"></a>Antwoord
Een organisatie kan doen dit door de volgende acties uitvoeren:

1. Ontwikkelen en publiceren van een uitgebreide beveiligingsbeleid. Het beleid van de regels van aanvaardbaar gebruik die zijn gekoppeld aan de via articulates software, cloudassets. Het definieert ook wat duidelijk is in strijd met het beleid. 
2. Ontwikkel een aangepaste installatiekopie, aangepaste artefacten en een proces waarmee voor het indelen van binnen de realm van de beveiliging die is gedefinieerd met active directory. Deze benadering dwingt af van het bedrijf bevindt en Hiermee stelt u een gemeenschappelijke set controles van de omgeving. Deze besturingselementen voor de omgeving een ontwikkelaar zou kan ontwikkelen en volgt u de levensduur van een veilige ontwikkeling als onderdeel van het algehele proces. Het doel is ook voor een omgeving die geen te streng die mei een belemmering vormen voor ontwikkeling, maar een redelijke set besturingselementen. Het groepsbeleid op de organisatie-eenheid (OE) met lab-virtuele machines mogelijk een subset van de totale groepsbeleidsregels die zijn gevonden in de productieomgeving. Ze kunnen ook worden een extra set goed een geïdentificeerde risico's te beperken.

## <a name="data-integrity"></a>Integriteit van gegevens

### <a name="question"></a>Vraag
Hoe zorgt een organisatie de integriteit van gegevens om ervoor te zorgen dat ontwikkelaars van externe toegang kunnen code verwijderen of kwaadaardige software of niet-goedgekeurde software introduceren?

### <a name="answer"></a>Antwoord
Er zijn verschillende lagen van besturingselement in op het oplossen van de bedreiging van externe adviseurs, aannemers of werknemers die zijn van externe toegang in kunnen samenwerken in DevTest Labs. 

Zoals eerder vermeld, moet de eerste stap een gebruiksregels opgesteld en gedefinieerd die duidelijk de gevolgen geeft een overzicht van wanneer iemand in strijd is met het beleid hebben. 

De eerste laag van besturingselementen voor externe toegang is het toepassen van een beleid voor externe toegang via een VPN-verbinding die niet direct is verbonden met het lab. 

De tweede laag van besturingselementen is het toepassen van een set van groepsbeleidsobjecten die voorkomen dat kopiëren en plakken via Extern bureaublad. Een beleid voor netwerken kan worden geïmplementeerd om toe te staan geen uitgaande services uit de omgeving, zoals FTP en RDP-services uit de omgeving. Alle netwerkverkeer Azure naar on-premises gebruiker gedefinieerde routering kan afdwingen, maar de routering kan geen rekening gehouden met alle URL's die mogelijk uploaden van gegevens, tenzij beheerd via een proxy voor het scannen van inhoud en sessies. Openbare IP-adressen kan worden beperkt in het virtuele netwerk ondersteunen DevTest Labs van een extern netwerk-resource-bridging niet zijn toegestaan.

Uiteindelijk moet hetzelfde type van de beperkingen worden toegepast in de hele organisatie, die zelf zou moeten ook rekening voor alle mogelijke methoden van verwisselbare media of externe URL's die een bericht van de inhoud kunnen worden geaccepteerd. Neem contact op met uw professionele om te controleren en implementeren van een beveiligingsbeleid voor beveiliging. Zie voor meer aanbevelingen [Microsoft Cyber Security](https://www.microsoft.com/security/default.aspx?&WT.srch=1&wt.mc_id=AID623240_SEM_sNYnsZDs).


## <a name="next-steps"></a>Volgende stappen
Zie [toepassingen migreren en integratie](devtest-lab-guidance-governance-application-migration-integration.md).
