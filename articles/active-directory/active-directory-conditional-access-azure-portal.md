---
title: Voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: "Gebruik voorwaardelijk toegangsbeheer in Azure Active Directory om te controleren op bepaalde voorwaarden bij het verifiëren voor toegang tot toepassingen."
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 3ca9157823a52bd0e4e8dd3b2656fc2724c8b4b6
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Voorwaardelijke toegang in Azure Active Directory

In een wereld mobiel eerste, cloud eerste kunnen Azure Active Directory eenmalige aanmelding aan apparaten, apps en services vanaf elke locatie. Werken met de komst van apparaten (inclusief BYOD), zakelijke netwerken en SaaS-apps 3e partij, IT-professionals worden momenteel geconfronteerd met twee tegengestelde doelstellingen:

- Zorgen dat eindgebruikers om productief te zijn overal en wanneer
- Beveiligen van zakelijke activa op elk gewenst moment

Ter verbetering van de productiviteit, biedt Azure Active Directory uw gebruikers met een groot aantal opties voor toegang tot uw zakelijke activa. Met Toepassingsbeheer toegang, Azure Active Directory kunt u om ervoor te zorgen dat alleen *de juiste mensen* toegang tot uw toepassingen. Wat gebeurt er als u wilt meer controle over hoe de juiste mensen toegang hebben tot uw resources onder bepaalde omstandigheden? Wat gebeurt er als u ook hebt voorwaarden waaronder die u wilt blokkeren van toegang tot bepaalde apps zelfs voor de *rechtermuisknop mensen*? Bijvoorbeeld, mogelijk OK voor u wanneer de juiste mensen toegang hebben tot bepaalde apps van een vertrouwd netwerk. echter raadzaam niet ze toegang krijgen tot deze apps vanaf een netwerk dat u niet vertrouwt. U kunt deze vragen met behulp van voorwaardelijke toegang oplossen.

Voorwaardelijke toegang is een functie van Azure Active Directory waarmee u kunt het afdwingen van besturingselementen op de toegang tot apps in uw omgeving op basis van bepaalde voorwaarden. U kunt ofwel verbinden, aanvullende vereisten voor de toegang met besturingselementen, of kunt u blokkeren. De implementatie van voorwaardelijke toegang is op basis van beleid. Een benadering op basis van beleid vereenvoudigt uw ervaring configuratie omdat de manier waarop die u over uw toegangsvereisten nadenken volgt.  

Doorgaans kunt u uw toegangsvereisten using-instructies die zijn gebaseerd op het volgende patroon definiëren:

![Besturingselement](./media/active-directory-conditional-access-azure-portal/10.png)

Wanneer u de twee exemplaren van vervangen '*dit*' met echte informatie hebt u een voorbeeld van een beleidsverklaring die waarschijnlijk eruit u vertrouwd voorkomen ziet:

*Als u aannemers wilt vanaf netwerken die geen vertrouwde toegang tot onze cloud-apps, klikt u vervolgens toegang te blokkeren.*

De bovenstaande beleidsverklaring licht de kracht van voorwaardelijke toegang. Terwijl u aannemers in feite toegang krijgen tot uw cloud-apps kunt inschakelen (**die**), met voorwaardelijke toegang, kunt u ook omstandigheden waarin de toegang mogelijk is definiëren (**hoe**).

In de context van voorwaardelijke toegang van Azure Active Directory

- "**Wanneer dit gebeurt**' heet **voorwaarde-instructie**
- "**Voert u deze**' heet **besturingselementen**

![Besturingselement](./media/active-directory-conditional-access-azure-portal/11.png)

De combinatie van een voorwaardeninstructie met de besturingselementen vertegenwoordigt een beleid voor voorwaardelijke toegang.

![Besturingselement](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Besturingselementen

In een beleid voor voorwaardelijke toegang definiëren besturingselementen wat is dat er moet gebeuren wanneer een instructie voor een voorwaarde is voldaan.  
Met besturingselementen, moet u de toegang blokkeren of toestaan van toegang met aanvullende vereisten.
Wanneer u een beleid waarmee toegang configureert, moet u ten minste één vereiste selecteren.  

Er zijn twee typen besturingselementen: 

- **Hiermee bepaalt u GRANT** -Grant besturingselementen bepalen of er een gebruiker kan authenticatie voltooien en de resource die u probeert aan te melden om te bereiken. Als u meerdere besturingselementen geselecteerd hebt, kunt u configureren of allemaal vereist zijn tijdens het verwerken van uw beleid.
De huidige implementatie van Azure Active Directory kunt u de volgende vereisten voor grant-besturingselement te configureren:

    ![Besturingselement](./media/active-directory-conditional-access-azure-portal/73.png)

- **Sessie-besturingselementen** -sessie bepaalt inschakelen ervaring in een cloud-app beperken. De sessie-besturingselementen worden afgedwongen door de cloud-apps en zijn gebaseerd op aanvullende informatie verstrekt door Azure AD naar de app over de sessie.

    ![Besturingselement](./media/active-directory-conditional-access-azure-portal/31.png)


Zie voor meer informatie [besturingselementen in Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access-controls.md).


## <a name="condition-statement"></a>Voorwaardeninstructie

De vorige sectie heeft geïntroduceerd op ondersteunde opties om te blokkeren of beperken van toegang tot uw resources in de vorm van besturingselementen. In een beleid voor voorwaardelijke toegang definieert u de criteria die worden voldaan om uw besturingselementen moeten moet worden toegepast in de vorm van een voorwaardeninstructie.  

U kunt de volgende toewijzingen opnemen in de voorwaardeninstructie:

![Besturingselement](./media/active-directory-conditional-access-azure-portal/07.png)


### <a name="who"></a>Wie?

Wanneer u een beleid voor voorwaardelijke toegang configureert, moet u de gebruikers of groepen die het beleid van toepassing op selecteren. In veel gevallen wilt u de besturingselementen op een specifieke groep gebruikers moet worden toegepast. U kunt deze set definiëren door het selecteren van de vereiste gebruikers en groepen die het beleid van toepassing op in een voorwaardeninstructie. Indien nodig, kunt u ook expliciet een set gebruikers van uw beleid uitsluiten door ze vrijstellen.  

![Besturingselement](./media/active-directory-conditional-access-azure-portal/08.png)



### <a name="what"></a>Wat?

Wanneer u een beleid voor voorwaardelijke toegang configureert, moet u het beleid van toepassing op de cloud-apps selecteren.
Er zijn meestal bepaalde apps in uw omgeving, vanuit het oogpunt van beveiliging van meer dat aandacht vereist dan andere. Dit geldt bijvoorbeeld apps die toegang tot gevoelige gegevens hebben.
Als u cloud-apps selecteert, moet u het bereik van uw beleid geldt voor cloud-apps definiëren. Indien nodig, kunt u ook expliciet een set apps uitsluiten van uw beleid.

![Besturingselement](./media/active-directory-conditional-access-azure-portal/09.png)

Zie voor een volledige lijst van de cloud-apps kunt u in uw beleid voor voorwaardelijke toegang, de [technische documentatie voor Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).

### <a name="how"></a>Hoe?

Zolang de toegang tot uw apps wordt uitgevoerd onder de voorwaarden die u kunt beheren, is er mogelijk niet nodig om extra controles op hoe uw cloud-apps worden gebruikt door uw gebruikers om op te leggen. Echter dingen er mogelijk anders als toegang tot uw cloud-apps wordt uitgevoerd, bijvoorbeeld van netwerken die geen vertrouwde of apparaten die niet compatibel. U kunt bepaalde voorwaarden voor toegang tot die aanvullende vereisten gelden voor hoe de toegang tot uw apps wordt uitgevoerd definiëren in een voorwaardeninstructie.

![Voorwaarden](./media/active-directory-conditional-access-azure-portal/01.png)


## <a name="conditions"></a>Voorwaarden

In de huidige implementatie van Azure Active Directory, kunt u definiëren voorwaarden voor de volgende gebieden:

- Aanmeldingsrisico
- Apparaatplatformen
- Locaties
- Client-apps


![Voorwaarden](./media/active-directory-conditional-access-azure-portal/01.png)

### <a name="sign-in-risk"></a>Aanmeldingsrisico

Een risico aanmelden is een object dat wordt gebruikt door Azure Active Directory voor het bijhouden van de kans dat een aanmeldingspagina proberen is niet uitgevoerd door de legitieme eigenaar van een gebruikersaccount. In dit object, de kans op (hoog, Gemiddeld of laag) wordt opgeslagen in de vorm van een kenmerk met de naam [aanmelden risiconiveau](active-directory-reporting-risk-events.md#risk-level). Dit object wordt gegenereerd tijdens een aanmeldingspagina van een gebruiker als aanmelden risico's zijn gedetecteerd door Azure Active Directory. Zie [Riskante aanmeldingen](active-directory-identityprotection.md#risky-sign-ins) voor meer informatie.  
U kunt het risiconiveau voor berekende aanmelden als voorwaarde in een beleid voor voorwaardelijke toegang gebruiken. 

![Voorwaarden](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Apparaatplatformen

Platform van het apparaat wordt gekenmerkt door het besturingssysteem dat wordt uitgevoerd op uw apparaat: U kunt de apparaatplatforms die opgenomen zijn, evenals de apparaatplatforms die zijn uitgesloten van een beleid definiëren.  
Als u apparaatplatforms in het beleid, moet u eerst de Schakelknoppen configureren om te wijzigen **Ja**, en selecteer vervolgens alle of afzonderlijke apparaatplatforms het beleid van toepassing. Als u afzonderlijke apparaatplatforms selecteert, wordt in het beleid alleen gevolgen heeft voor deze platforms. Aanmeldingen voor andere ondersteunde platforms zijn in dit geval wordt niet beïnvloed door het beleid.


![Voorwaarden](./media/active-directory-conditional-access-azure-portal/02.png)

Zie voor een volledige lijst met de ondersteunde apparaatplatforms [apparaat platform voorwaarde](active-directory-conditional-access-technical-reference.md#device-platform-condition).


### <a name="locations"></a>Locaties

Met locaties hebt u de optie voor het definiëren van de voorwaarden die zijn gebaseerd op waar een verbindingspoging van is gestart. De vermeldingen in de lijst met locaties bevinden zich **locaties met de naam** of **MFA goedgekeurde IP-adressen**.  

**Met de naam locaties** is een functie van Azure Active Directory waarmee u voor het definiëren van labels voor de locaties is geprobeerd verbinding uit. Als u een locatie definieert, kunt u ofwel een IP-adres configureren-adresbereiken of Selecteer een land / regio.  

![Voorwaarden](./media/active-directory-conditional-access-azure-portal/42.png)

Bovendien kunt u een benoemde locatie als vertrouwde locatie markeren. Voor een beleid voor voorwaardelijke toegang, is de vertrouwde locatie een andere filteroptie die u kunt kiezen *alle vertrouwde locaties* in de voorwaarde locaties.
Benoemde locaties zijn ook belangrijk is in de context van de detectie van [bestaat de kans dat gebeurtenissen](active-directory-reporting-risk-events.md) om het aantal ONWAAR-positieven voor de onmogelijke reis naar ongewone locaties risicogebeurtenis te verminderen. 

Het aantal benoemde locaties die u kunt configureren, wordt beperkt door de grootte van het gerelateerde object in Azure AD. U kunt configureren:
 
 - Een locatie met maximaal 500 IP-adresbereiken met de naam
 - Maximaal 60 benoemde locaties (preview) met één IP-bereik dat is toegewezen aan elk van deze 

Zie voor meer informatie [locaties in Azure Active Directory met de naam](active-directory-named-locations.md).


**MFA goedgekeurde IP-adressen** is een functie van multi-factor authentication-server waarmee u vertrouwde IP-adresbereiken die lokaal intranet van uw organisatie definiëren. Wanneer u een voorwaarde locatie configureert, kunt goedgekeurde IP-adressen u onderscheid maken tussen verbindingen vanaf het netwerk van uw organisatie en alle andere locaties. Zie voor meer informatie [goedgekeurde IP-adressen](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  

U kunt in uw beleid voor voorwaardelijke toegang:

- Opnemen
    - Elke locatie
    - Alle vertrouwde locaties
    - Geselecteerde locaties
- Uitsluiten
    - Alle vertrouwde locaties
    - Geselecteerde locaties
     
![Voorwaarden](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-apps"></a>Client-apps

De client-app kan op een algemeen beveiligingsniveau de app (webbrowser, mobiele app bureaubladclient) die u hebt gebruikt om verbinding maken met Azure Active Directory of kunt u Exchange Active Sync specifiek selecteren.  
Verouderde verificatie verwijst naar clients met behulp van basisverificatie zoals oudere Office-clients die gebruikmaken van moderne verificatie niet. Voorwaardelijke toegang wordt momenteel niet ondersteund met verouderde verificatie.

![Voorwaarden](./media/active-directory-conditional-access-azure-portal/04.png)


Zie voor een volledige lijst van de ClientApps kunt u in uw beleid voor voorwaardelijke toegang, de [technische documentatie voor Azure Active Directory voorwaardelijke toegang](active-directory-conditional-access-technical-reference.md#client-apps-condition).




## <a name="common-scenarios"></a>Algemene scenario's

### <a name="requiring-multi-factor-authentication-for-apps"></a>Meervoudige verificatie vereisen voor apps

Veel omgevingen hebben apps waarvoor een hoger niveau van beveiliging dan de andere.
Dit is bijvoorbeeld het geval voor apps die toegang tot gevoelige gegevens hebben.
Als u een andere beschermingslaag toevoegen aan deze apps wilt, kunt u een beleid voor voorwaardelijke toegang waarvoor multi-factor authentication-server is vereist wanneer gebruikers toegang hebben tot deze apps kunt configureren.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Meervoudige verificatie vereisen voor toegang via netwerken die geen vertrouwde

Dit scenario is vergelijkbaar met het vorige scenario, omdat een vereiste voor multi-factor authentication wordt toegevoegd.
Het belangrijkste verschil is echter de voorwaarde voor deze vereiste.  
Tijdens de focus van het vorige scenario op apps met toegang tot gevoelige gegevens, is de focus van dit scenario op vertrouwde locaties.  
U wellicht een vereiste voor multi-factor authentication met andere woorden, als een app wordt geopend door een gebruiker vanaf een netwerk dat u niet vertrouwt.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Alleen vertrouwde apparaten hebben toegang tot Office 365-services

Als u van Intune in uw omgeving gebruikmaakt, kunt u onmiddellijk starten via de interface van het beleid voor voorwaardelijke toegang in de Azure-console.

Veel klanten van Intune voorwaardelijke toegang gebruiken om ervoor te zorgen dat alleen vertrouwde apparaten toegang krijgen Office 365-services tot. Dit betekent dat mobiele apparaten zijn ingeschreven bij Intune en voldoen aan nalevingsvereisten van beleid en dat Windows-pc's lid zijn van een lokaal domein. Een verbetering van de belangrijkste is dat er geen hetzelfde beleid instellen voor elk van de Office 365-services.  Wanneer u een nieuw beleid maakt, moet u de Cloud-apps zodat elk van de O365-apps die u wilt beveiligen met voorwaardelijke toegang configureren.

### <a name="switching-a-device-from-corporate-owned-to-bring-your-own-device-byod"></a>Overschakelen van een apparaat in Bedrijfseigendom voor Bring Your Own Device (BYOD)
Als u blokkeren, ingeschreven apparaten wilt door het wijzigen van de apparaten die eigendom van zakelijke naar persoonlijke, kunt u dit doen met behulp van voorwaardelijke toegang van Azure Active Directory (AAD). U moet eerst een beleid voor voorwaardelijke toegang maken waar **toegang blokkeren** is geselecteerd in de toegangscontrole **Grant** blade. Maak vervolgens een **dynamische apparaat** groep door in te stellen de **deviceOwnership** eigenschap **persoonlijke**. Vervolgens de bovenstaande beleid naar de nieuwe groep als doel.

## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
