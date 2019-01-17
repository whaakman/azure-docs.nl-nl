---
title: Oplossingen - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: Pagina met oplossingen voor Microsoft Threat Modeling Tool mogelijke oplossingen voor de meest blootgestelde markeren gegenereerd bedreigingen.
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 24aa49fd4ccccda372d2632ef4aee22bd5cb2bf6
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359133"
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Oplossingen van Microsoft Threat Modeling Tool

De Threat Modeling Tool is een core-element van de Microsoft Security Development Lifecycle (SDL). Hiermee kunt de software-architecten kunt herkennen en mogelijke beveiligingsproblemen vroeg, verminderen wanneer ze zijn relatief eenvoudig en kosteneffectief worden opgelost. Als gevolg hiervan vermindert aanzienlijk de totale kosten voor ontwikkeling. We ook ontworpen het hulpprogramma met experts van de niet-beveiligde Denk eraan dat u gemakkelijker risicomodel voor alle ontwikkelaars door het bieden van duidelijke richtlijnen over het maken en analyseren van bedreigingsmodellen.

Ga naar de **[Threat Modeling Tool](./azure-security-threat-modeling-tool.md)** om vandaag nog aan de slag!

## <a name="mitigation-categories"></a>Risicobeperking categorieën

De oplossingen Threat Modeling Tool zijn onderverdeeld op basis van het Web Application Security kader, die uit het volgende bestaat:

| Categorie | Description |
| -------- | ----------- |
| **[Controle en logboekregistratie](./azure-security-threat-modeling-tool-auditing-and-logging.md)** | Wie deed wat en? Controle en logboekregistratie verwijzen naar de manier waarop uw toepassing beveiligingsgerelateerde gebeurtenissen registreert |
| **[Verificatie](./azure-security-threat-modeling-tool-authentication.md)** | Wie bent u? Verificatie is het proces waarbij een entiteit aantoont de identiteit van een andere entiteit, doorgaans via referenties, zoals een gebruikersnaam en wachtwoord dat |
| **[Autorisatie](./azure-security-threat-modeling-tool-authorization.md)** | Wat kunt u doen? Autorisatie is hoe uw toepassing biedt toegangsbeheer voor resources en bewerkingen |
| **[Communicatiebeveiliging](./azure-security-threat-modeling-tool-communication-security.md)** | Wie bent u communicatie met? Communicatiebeveiliging zorgt ervoor dat alle communicatie gedaan zo veilig mogelijk is |
| **[Configuratiebeheer](./azure-security-threat-modeling-tool-configuration-management.md)** | Wie uw toepassing uitvoeren als? Welke databases deze verbinding maken? Hoe wordt de toepassing beheerd? Hoe worden deze instellingen beveiligd? Configuratiebeheer verwijst naar hoe uw toepassing omgaat met deze operationele problemen |
| **[Cryptografie](./azure-security-threat-modeling-tool-cryptography.md)** | Hoe u ervoor zorgen dat geheimen (vertrouwelijkheid)? Hoe gaat het onrechtmatige wijzigingen taalprogramma uw gegevens of -bibliotheken (integriteit)? Hoe biedt u seeds voor willekeurige waarden dat cryptografisch sterke worden moeten? Cryptografie verwijst naar hoe uw toepassing vertrouwelijkheid en integriteit dwingt |
| **[Uitzonderingenbeheer](./azure-security-threat-modeling-tool-exception-management.md)** | Wanneer een methodeaanroep in uw toepassing is mislukt, wat is uw toepassing? Hoeveel geven u? Kan u beschrijvende foutgegevens retourneren aan eindgebruikers? U waardevolle uitzonderingsinformatie terug naar de aanroeper doorgeven? Uw toepassing mislukt probleemloos? |
| **[Invoervalidatie](./azure-security-threat-modeling-tool-input-validation.md)** | Hoe weet u dat de invoer die uw toepassing ontvangt geldig en veilig is? Invoervalidatie verwijst naar hoe uw toepassing filters, struikvegetaties of invoer voorafgaand aan de extra verwerking worden geweigerd. Houd rekening met beperkingen invoer via toegangspunten en de uitvoer via punten afsluiten codering. Vertrouwt u gegevens uit bronnen, zoals databases en bestandsshares? |
| **[Gevoelige gegevens](./azure-security-threat-modeling-tool-sensitive-data.md)** | Hoe wordt gevoelige gegevens in uw toepassing verwerkt? Gevoelige gegevens verwijst naar hoe uw toepassing omgaat met alle gegevens die moeten worden beveiligd in het geheugen, via het netwerk of in de permanente winkels |
| **[Sessiebeheer](./azure-security-threat-modeling-tool-session-management.md)** | Hoe uw toepassing worden verwerkt en gebruikerssessies beveiligen? Een sessie verwijst naar een reeks van verwante interacties tussen een gebruiker en uw Web-App |

Hiermee kunt u identificeren:

* Waar zijn de meest voorkomende fouten gemaakt
* Waar zijn de meest bruikbare verbeteringen

Als gevolg hiervan u gebruikt deze categorieën zich kunnen concentreren en prioriteiten beveiliging, zodat als u weet dat de meest bekende beveiligingsproblemen optreden in de invoer valideren, verificatie en autorisatie-categorieën, kun u er. Ga voor meer informatie naar  **[deze octrooien koppeling](https://www.google.com/patents/US7818788)**

## <a name="next-steps"></a>Volgende stappen

Ga naar **[Threat Modeling Tool bedreigingen](./azure-security-threat-modeling-tool-threats.md)** voor meer informatie over de bedreiging categorieën maakt gebruik van het hulpprogramma voor het genereren van ontwerp van mogelijke bedreigingen.