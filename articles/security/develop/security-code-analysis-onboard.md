---
title: Hand leiding voor analyse van beveiligings code Microsoft Azure
description: Dit artikel bevat informatie over het installeren van de uitbrei ding voor beveiligings code analyse
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 8a8eca73205d4f8f33d4d069fda72638af61d355
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718299"
---
# <a name="how-to-onboarding-and-installing"></a>Procedure: Onboarding en installatie

Vereisten om aan de slag te gaan met de analyse van micro soft-beveiligings code
  - In aanmerking komende Microsoft Unified Support Services-aanbieding (details hieronder)
  - Een Azure DevOps-organisatie
  - Machtigingen voor het installeren van extensies voor de Azure DevOps-organisatie
  - Bron code die kan worden gesynchroniseerd met een door de Cloud gehoste Azure DevOps-pijp lijn


## <a name="onboarding-microsoft-security-code-analysis-extension"></a>Uitbrei ding van de analyse van micro soft-beveiligings code

- Als u al een van de volgende ondersteunings aanbiedingen hebt, neemt u contact op met uw Technical Account Manager en purchase\swap u de bestaande uren om toegang te krijgen tot de uitbrei ding.
   - Uniforme ondersteuning: geavanceerde en prestatie niveau, Premier Support voor ontwikkel aars, Premier Support voor partners of Premier Support voor bedrijven.
- Als u een van de volgende ondersteunings services hebt of geen ondersteunings plan hebt met micro soft, moet u een upgrade uitvoeren naar een in aanmerking komende ondersteunings aanbieding:
   - Azure-ondersteuning voor partners, Azure Basic, Azure Developer, Azure Standard, Azure Professional Direct, of Unified support – core-laag
- Ga naar de [Start pagina](https://www.microsoft.com/enterprise/services/support) van de ondersteunings services als u een in aanmerking komend ondersteunings aanbieding wilt kopen
- Zodra een ondersteunings contract is ingevoerd, neemt u contact op met uw technische account beheerder die u kan helpen aan de slag te gaan en alle vereiste gegevens te verzamelen.
 
>[!NOTE]
> Alleen geregistreerde partners in het netwerk van micro soft-partners komen in aanmerking voor de aanschaf van Premier Support voor partners, anders kopen we een van de hiervoor genoemde ondersteunings aanbiedingen.

## <a name="installing-microsoft-security-code-analysis-extension"></a>Uitbrei ding voor de analyse van micro soft-beveiligings code installeren

1. Zodra de extensie is gedeeld met uw Azure DevOps-organisatie, gaat u naar de pagina Azure DevOps-organisatie (bijvoorbeeld http://dev.azure.com/contoso)
2. Klik op het pictogram winkel Bag in de rechter bovenhoek naast uw naam en klik vervolgens op uitbrei dingen beheren. 
3. Klik op de uitbrei ding analyse van micro soft-beveiligings code en start de wizard Azure DevOps-gebruikers interface om de installatie te starten.
4. Kies de organisatie van Azure DevOps om de extensie te installeren in de vervolg keuzelijst
5. Klik op installeren. Zodra het proces is voltooid, kunt u door gaan met het gebruik van de extensie

>[!NOTE]
> Zelfs als u geen toegang hebt, gaat u door met de installatie stappen en kunt u tijdens het proces toegang aanvragen bij de beheerder van uw Azure DevOps-organisatie.
>
Zodra de uitbrei ding is geïnstalleerd, zijn de taken voor het bouwen van beveiligde ontwikkel aars zichtbaar en beschikbaar voor toevoegen aan uw Azure-pijp lijnen.

## <a name="adding-specific-build-tasks-to-your-devops-pipeline"></a>Specifieke build-taken toevoegen aan uw DevOps-pijp lijn

1. Open uw team project vanuit uw Azure DevOps-organisatie.
2. Navigeer naar het tabblad builds onder **pijp lijnen** 
3. Selecteer de pijp lijn waaraan u de extensie voor het maken van extensies wilt toevoegen. 
   - Nieuw: Klik op **Nieuw** en volg de stappen voor het maken van een nieuwe pijp lijn.
   - Bewerk de pijp lijn en klik op **bewerken** om te beginnen met het bewerken van een bestaande pijp lijn.
4. Klik op + om naar het deel venster **taken toevoegen** te gaan.
5. Zoek de taak die u wilt toevoegen in de lijst of gebruik het zoekvak en klik vervolgens op **toevoegen**. 
6. U kunt nu door gaan met het opgeven van de para meters die nodig zijn voor de taak.
>[!NOTE]
>Paden voor bestanden of mappen zijn relatief ten opzichte van de hoofdmap van de bron opslagplaats en para meters die de uitvoermap/bestanden opgeven, worden vervangen door de algemene locatie die we hebben gedefinieerd in de build-agent.

7. Een nieuwe build in de wachtrij plaatsen.
> [!TIP]
>  - Als u de analyse na uw build wilt uitvoeren, plaatst u de taken voor het analyseren van micro soft-beveiligings code na de stap build bouwen artefacten van uw build. Op die manier kan uw build de resultaten volt ooien en plaatsen voordat statische analyse Programma's worden uitgevoerd.
>  - Controleer altijd de optie **door gaan bij fout** van het ontwikkelen van beveiligde ontwikkel taken. Zelfs als een van de hulp middelen mislukt, kunnen de andere functies worden uitgevoerd. Er zijn geen onderlinge afhankelijkheden.
>  - Micro soft-beveiligings code analyse taken mislukken alleen als het hulp programma niet kan worden uitgevoerd, maar ze zullen niet mislukken als en wanneer het hulp programma problemen in de code identificeert. U kunt uw build configureren om te kraken wanneer een hulp programma problemen in de code identificeert met behulp van de taak bouwen **na de analyse** .
>  - Sommige Azure DevOps-build-taken worden niet ondersteund wanneer ze worden uitgevoerd via een ' release '-pijp lijn. Dit is een beperking van Azure DevOps. Ze ondersteunen geen taken die artefacten publiceren vanuit een release pijplijn.
>  - Zie [Azure DevOps build Varia bles](https://docs.microsoft.com/azure/devops/pipelines/build/variables?tabs=batch&view=vsts) (Engelstalig) voor een lijst met vooraf gedefinieerde variabelen in azure DevOps-team builds.

## <a name="next-steps"></a>Volgende stappen

Zie onze [configuratie handleiding](security-code-analysis-customize.md) voor meer informatie over het configureren van de build-taken

[Raadpleeg onze pagina Veelgestelde vragen](security-code-analysis-faq.md) als u meer vragen hebt over de uitbrei ding en de hulpprogram ma's die worden aangeboden.


