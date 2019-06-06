---
title: Controlelijst voor het maken van - bieden commerciële Marketplace naar Azure
description: De details die u kunt opgeven in een proces voor het maken van de aanbieding. -Commerciële Marketplace naar Azure
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 904058c2c98c8ded2ea9c91e8aa7ec595aa49b05
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481442"
---
# <a name="offer-creation-checklist"></a>Controlelijst voor aanbieding maken

Het proces voor het maken van aanbieding gaat u door meerdere pagina's. Hier volgen de details die u op elke pagina, met koppelingen opgeven kunt voor meer informatie over elk item.

U bent verplicht om te geven of geef items worden hieronder vermeld. Bepaalde gebieden zijn optioneel of hebt standaardwaarden opgegeven, kunt u naar wens wijzigen. U hoeft niet te werken in deze secties in de volgorde die hier worden vermeld.

| **Item**    | **Doel**  |
| :---------- | :-------------------|
| [**Nieuwe aanbieding modale**](#new-offer-modal) | Verzamelt bieden gegevens van identiteit.  |
| [Bieden van Setup](#offer-setup-page) | Hiermee kunt u meldt zich aan voor gebruik van belangrijke functies en kies het verkopen van uw aanbieding met behulp van Microsoft.  |
| [Eigenschappenpagina](#properties-page) | De categorieën en branches gebruikt voor het groeperen van uw aanbieding op de marktplaatsen, de ondersteuning van uw aanbieding en de appversie van uw contracten definiëren. |
| [Pagina met de vermelding bieden](#offer-listing-page) | Definieer de details van de aanbieding moet worden weergegeven in de marketplace, met inbegrip van de beschrijvingen van uw aanbieding en marketing activa. |
| [Preview-pagina](#preview-page) | Een beperkte Preview-doelgroep voor het vrijgeven van uw aanbod publiceren van uw aanbieding live naar de bredere marketplace audience(s) definiëren. |
| [Technische configuratiepagina bieden](#technical-configuration-page)  | Is alleen beschikbaar als u kiest voor het verkopen van de aanbieding tot en met Microsoft. Definieer de technische details (URL-pad, webhook, tenant-ID en app-ID) gebruikt voor verbinding met uw aanbieding. |
| [**Nieuw Plan modale**](#plan-identity-modal) | Verzamelt plannen gegevens van identiteit.  |
| [Pagina met de vermelding plannen](#plan-listing-page)  | Is alleen beschikbaar als u kiest voor het verkopen van de aanbieding tot en met Microsoft. Definieer de details die wordt gebruikt om het abonnement in de marketplace weer te geven.  |
| [Planprijzen & pagina beschikbaarheid](#plan-pricing--availability-page)  | Is alleen beschikbaar als u kiest voor het verkopen van de aanbieding tot en met Microsoft.  Verzamelt de zakelijke kenmerken (prijsmodel), de beschikbaarheid van de doelgroep en het marktaandeel voor elk abonnement (versie) van uw aanbieding.  |
| [Overzicht van de Testritpagina](#test-drive-listing-page)  | Is alleen beschikbaar als u kiest voor het bieden van een test uit voor uw aanbieding. Definieer de details die aan de uitproberen in de marketplace-lijst wordt gebruikt.  |
| Technische configuratie van de Testritpagina  | Is alleen beschikbaar als u kiest voor het bieden van een test uit voor uw aanbieding. Definieer de technische details voor de demonstratie (of 'test drive') die klanten de mogelijkheid om te proberen uw aanbieding biedt voordat het doorvoeren van kopen.  |
| [Controleer en pagina publiceren](#review-and-publish-page)  | Selecteer de wijzigingen die u wilt publiceren, ziet de status van elke pagina en notities toe aan het team voor certificering.  |


## <a name="new-offer-modal"></a>Nieuwe aanbieding modaal 

De eerste stukjes informatie die u wordt gevraagd om te bieden, zijn een naam en een ID voor uw aanbieding. 

| **Veldnaam**    | **Opmerkingen**   |  
| :---------------- | :-----------| 
| Aanbiedings-ID  | Vereist, kan niet worden gewijzigd nadat de is gemaakt. Maximaal 50 tekens bevatten en moet bestaan alleen uit kleine letters, alfanumerieke tekens, streepjes of onderstrepingstekens bevatten. |
| Naam van aanbieding  | Vereist. |

## <a name="offer-setup-page"></a>Bieden van Setup

De installatiepagina van de aanbieding is waar u kunt kiezen voor verschillende kanalen en Verkoopmanager bewegingen, evenals declareren leidt het gebruik van de belangrijkste functies, zoals uitproberen en klanten. 

| **Veldnaam**    | **Opmerkingen**   | 
| :---------------- | :-----------|  
| Wilt u verkopen via Microsoft?  | Vereist. Standaard: Ja |
| Hoe wilt u potentiële klanten om te communiceren met de aanbieding lijsten? (Aanzetten tot actie)  | Vereist als het niet verkopen van door Microsoft. Standaard: Gratis proefversie, opties: 'Nu ophalen', 'Gratis', "Contact met mij opnemen." |
| Trial URL  | Vereist als 'Gratis' is ingeschakeld, zoals de manier waarop klanten met de vermelding van de aanbieding communiceren moeten. |
| URL van de aanbieding  | Vereist als 'Ophalen nu' is ingeschakeld, zoals de manier waarop klanten met de vermelding van de aanbieding communiceren moeten |
| Kanalen  | Optioneel. Standaard: Niet deelneemt aan het kanaal CSP (reseller).  |
| Test Drive | Optioneel. Standaard: Er is geen test drive is ingeschakeld.  |
| Type Test Drive | Vereist als een test uit ingeschakeld. Standaard: Niets geselecteerd. Opties: Azure Resource Manager, Dynamics 365 voor bedrijven-centraal, Dynamics 365 voor Customer Engagement, Dynamics 365 voor bewerkingen, logische app, Power BI.  |
| Leads beheren: verbinding maken met een CRM-systeem | Vereist als verkopen via Microsoft, of als aanbieding biedt als "Contact met mij opnemen." Standaard: geen CRM-systeem die zijn verbonden. CRM-opties: Azure-tabel, Azure blob-, Dynamics CRM online, de HTTPs-eindpunt, Marketo, Salesforce  |

## <a name="properties-page"></a>De pagina Eigenschappen

De eigenschappenpagina is waar u de categorieën en branches gebruikt voor het groeperen van uw aanbieding op de marktplaatsen, de ondersteuning van uw aanbieding en de appversie van uw contracten definiëren. Zorg ervoor dat bieden volledige en nauwkeurige gegevens over uw aanbieding op deze pagina wordt weergegeven op de juiste wijze en aangeboden aan de juiste set met klanten. 

| **Veldnaam**    | **Opmerkingen**   | 
| :---------------- | :-----------|  
| Categorie en subcategorie | Vereiste 1 en maximaal 3. Standaard: Niets geselecteerd. |
| Industrieën en subindustries | Optioneel. maximaal 2 L1 branches en maximaal 2 subindustries binnen elke sector L1, standaard: Geen geselecteerd |
| App-versie  | Optioneel. Standaard: Geen. |
| Gebruik standaard Contract  | Optioneel. Standaard: niet geselecteerd.  | |
| Gebruiksvoorwaarden  | Vereist als de standaardcontract niet is geselecteerd.  |

## <a name="offer-listing-page"></a>Pagina met de vermelding bieden

De pagina met de vermelding is waar u de tekst en afbeeldingen die klanten zien bij het weergeven van uw aanbieding aanbieding op marketplace bieden. 

| **Veldnaam**    | **Opmerkingen**   |
| :---------------- | :-----------| 
| Name  | Vereist, maximaal 50 tekens. |
| Samenvatting  | Vereist, maximaal 100 tekens. | 
| Description  | Vereist, maximaal 3000 tekens. |
| Aan de slag instructies  | Vereist, maximaal 3000 tekens. |
| Aan de slag instructies  | Vereist, maximaal 3000 tekens. |
| Zoektrefwoorden  | Optioneel; de aanbevolen, maximaal 3 trefwoorden. |
| URL privacybeleid  | Vereist. |
| CSP Program Marketing Materials URL  | Optioneel. |
| Nuttige koppelingen titel + URL  | Optioneel. |
| Bewijsstukken titel + -bestand  | Vereist, 1 min en max. 3. Moet u PDF-indeling. |
| Schermopnamen  | Vereist, schermafbeelding 1 min en max 5; vier of meer aanbevolen. Moet 1280 X 720 in PNG-indeling. |
| Store-logo's (klein, middelgroot, groot, Wide, Hero)  | Klein (48 X 48) en groot (216 X 216) vereist. andere grootten optioneel maar aanbevolen: Normaal (90 x 90), Wide (255 x 115), Hero (815 x 290). Moet de PNG-indeling. |
| De naam van de video's, URL + miniatuur  | Optioneel; de aanbevolen, maximaal 4 video's. Miniatuur moet 1280 x 720 PNG-indeling hebben. Video moet worden gehost in YouTube of Vimeo. |
| Contactpersonen (CSP-programma, techniek, ondersteuning)  | Engineering en neem contact op met ondersteuning vereist (naam, e-mailadres en telefoonnummer); De contactpersoon van de CSP-programma is optioneel maar aanbevolen. |
| URL voor ondersteuning  | Vereist. |

## <a name="preview-page"></a>Preview-pagina

De preview-pagina is waarin u de doelgroep toegang hebben tot uw aanbieding-voorbeeld, om te controleren of de aanbieding voldoet aan alle vereisten van uw voordat deze live meteen opgeven. 

| **Veldnaam**    | **Opmerkingen**   | 
| :---------------- | :-----------| 
| AAD/MSA-e-mail + beschrijving | Vereist, min 1 en maximaal 10 als handmatig of tot maximaal 20 ingevoerd als een CSV-bestand uploaden. |

## <a name="technical-configuration-page"></a>Pagina voor technische configuratie 

De configuratiepagina van technische is waarin u de technische details door Microsoft gebruikt om te verbinden met uw aanbieding opgeven. Deze pagina is niet zichtbaar voor u, als u had besloten niet te verkopen via Microsoft.

| **Veldnaam**    | **Opmerkingen**   |  
| :---------------- | :-----------| 
| URL landingspagina | Vereist als het verkopen van door Microsoft. |
| Verbinding webhook | Vereist als het verkopen van door Microsoft. |
| Azure AD-tenant-ID | Vereist als het verkopen van door Microsoft. |
| Azure AD-app-ID | Vereist als het verkopen van door Microsoft. |

## <a name="plan-identity-modal"></a>Identiteit modale plannen

De eerste stukjes informatie die u wordt gevraagd om u te bieden, zijn een naam en een ID voor uw abonnement. Deze pagina is niet zichtbaar voor u, als u hebt besloten niet te verkopen via Microsoft.

| **Veldnaam**    | **Opmerkingen**   |  
| :---------------- | :-----------| 
| Plan-ID  | Vereist als het verkopen van door Microsoft. Het kan niet worden gewijzigd nadat de is gemaakt. Maximaal 50 tekens bevatten en moet bestaan alleen uit kleine letters, alfanumerieke tekens, streepjes of onderstrepingstekens bevatten. |
| Naam van het plan  | Vereist als het verkopen van door Microsoft. Moet uniek zijn in alle abonnementen in de aanbieding. Maximaal 50 tekens. |

## <a name="plan-listing-page"></a>Pagina met de vermelding plannen

Het abonnement die pagina is waarin u de tekst voor klanten om te zien wanneer ze het plan bekijken in de marketplace opgeven. Deze pagina is niet zichtbaar voor u, als u had besloten niet te verkopen via Microsoft.

| **Veldnaam**    | **Opmerkingen**   |  
| :---------------- | :-----------| 
| Beschrijving van plan bent   | Vereist als het verkopen van door Microsoft. Max. 500 tekens. | |

## <a name="plan-pricing--availability-page"></a>Pagina prijzen en beschikbaarheid plannen

De pagina voor prijzen en beschikbaarheid van plan is waar het definiëren van het bedrijf kenmerken, doelgroep en beschikbaarheid van de markt voor elk abonnement (versie) van uw aanbieding. Deze pagina is niet zichtbaar voor u, als u had besloten niet te verkopen via Microsoft.

| **Veldnaam**    | **Opmerkingen**   | 
| :---------------- | :-----------| 
| Beschikbaarheid van de markt  | Vereiste 1 min en max 141. |
| Prijsmodel  | Vereist. Standaard: Vast bedrag. Opties: Vaste prijs, per gebruiker. |
| Minimum en maximum aantal seats  | Optioneel, alleen beschikbaar als seat gebaseerde prijsmodel geselecteerd. |
| Facturering-Term  | Vereist. Standaard: Per maand. Opties: Maandelijks, jaarlijks. |
| Prijs  | Vereiste USD per maand, als maandelijks factureren term geselecteerd. of USD per jaar als jaarlijkse facturering term geselecteerd. |
| Doelgroep plannen  | Optioneel. Standaard: Openbare plan. Opties: Openbare, particuliere door tenant-ID |
| Beperkte plannen-doelgroep (tenant-ID + beschrijving)  | Vereist als persoonlijke plan geselecteerd. 1 min en max 10 tenant-id's als handmatig hebt ingevoerd. Max 20000 als CSV-bestand importeren. |

## <a name="test-drive-listing-page"></a>Overzicht van de testritpagina

Is alleen beschikbaar als u kiest voor het bieden van een test uit voor uw aanbieding. Definieer de details die aan de uitproberen in de marketplace-lijst wordt gebruikt.

| **Veldnaam**    | **Opmerkingen**   | 
| :---------------- | :-----------| 
| Description  | Vereist. |
| Handmatige gebruikersnaam + -bestand  | Vereist, maximaal 1 doc-bestand. Moet u PDF-indeling. |
| Naam van de video, URL + miniatuur  | Optioneel, aanbevolen. Miniatuur moet 533 x 324 in JPGP of PNG-indeling. Video moet worden gehost in YouTube of Vimeo. |

## <a name="review-and-publish-page"></a>Controleer en pagina publiceren

| **Veldnaam**    | **Opmerkingen**   | 
| :---------------- | :-----------| 
| Notities voor certificering  | Optioneel. |

## <a name="next-steps"></a>Volgende stappen

- [Een nieuwe SaaS-aanbieding maken](./create-new-saas-offer.md)
