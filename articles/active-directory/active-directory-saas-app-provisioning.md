---
title: Automatische SaaS app gebruikers inrichten in Azure AD | Microsoft Docs
description: Een inleiding tot hoe u Azure AD kunt gebruiken voor het automatisch inrichten, inrichten ongedaan, en werk continu gebruikersaccounts voor meerdere derde SaaS-toepassingen.
services: active-directory
documentationcenter: ''
author: asmalser-msft
manager: mtillman
editor: ''
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2017
ms.author: asmalser
ms.openlocfilehash: 72f796f0a4522b66feb55b827b02a83dcfdd3a01
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Gebruiker inrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Wat is geautomatiseerde gebruikersinrichting voor SaaS-apps?
Azure Active Directory (Azure AD) kunt u het maken, het onderhoud en het verwijderen van gebruikers-id's in de cloud automatiseren ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) toepassingen zoals Dropbox, Salesforce en ServiceNow.

**Hieronder volgen enkele voorbeelden van wat deze functie u kunt doen:**

* Automatisch nieuwe accounts maken in de juiste systemen voor nieuwe gebruikers wanneer ze bij uw team of organisatie.
* Automatisch accounts in de juiste systemen deactiveren wanneer mensen uw team of organisatie verlaat.
* Zorg ervoor dat de identiteiten in uw apps en systemen zijn bijgewerkt op basis van wijzigingen in de map of het systeem human resources.
* Voorzien in een niet-gebruikers objecten, zoals groepen van toepassingen die deze ondersteunen.

**Geautomatiseerde gebruikersinrichting bevat ook de volgende functionaliteit:**

* De mogelijkheid om overeenkomen met bestaande identiteiten tussen bron- en doelsystemen.
* Aanpasbare kenmerktoewijzingen die welke gebruikersgegevens definieert moeten stromen van het bronsysteem naar het doelsysteem.
* Optionele e-mailwaarschuwingen voor het inrichten van fouten
* Rapportage- en activiteit Logboeken om te helpen bij de bewaking en probleemoplossing.

## <a name="why-use-automated-provisioning"></a>Waarom een geautomatiseerde inrichting gebruiken?
Sommige algemene motivaties voor het gebruik van deze functie zijn onder andere:

* De kosten, inefficiënte en menselijke fouten die zijn gekoppeld aan de handmatige inrichting processen voorkomen.
* De kosten in verband met de host en onderhouden van ontwikkelde aangepaste oplossingen voor inrichting en scripts te vermijden
* Beveiligen van uw organisatie met de identiteit van gebruikers direct verwijderen uit sleutel SaaS-apps wanneer ze de organisatie verlaat.
* Eenvoudig importeren een groot aantal gebruikers in een bepaalde SaaS-toepassing of het systeem.
* Om te profiteren van een enkele set beleidsregels om te bepalen die is ingericht en die zich aanmelden bij een app hebben.


## <a name="how-does-automatic-provisioning-work"></a>Hoe automatische inrichting werkt?
    
De **Azure AD-inrichtingsservice** gebruikers SaaS-apps en andere systemen, bepalingen door verbinding te maken met beheer-API-eindpunten gebruiker geleverd door de leverancier van elke toepassing. Deze gebruiker management API-eindpunten kunnen Azure AD via een programma maken, bijwerken en verwijderen van gebruikers. Voor de geselecteerde toepassingen die kunt ook de inrichting service maken, bijwerken en verwijderen van aanvullende identity-gerelateerde objecten, zoals rollen en groepen. 

![Inrichting](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*afbeelding 1: de Azure AD-Service inricht*

![Uitgaande inrichten](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*afbeelding 2: 'Uitgaande' gebruikers inrichten van de werkstroom van Azure AD voor populaire SaaS-toepassingen*

![Inkomende inrichten](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*afbeelding 3: 'Inkomend' gebruiker inrichten werkstroom van populaire menselijke kapitaal Management (HCM)-toepassingen voor Azure Active Directory en Windows Server Active Directory*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Welke toepassingen en systemen kan ik met Azure AD automatisch gebruikers inrichten gebruiken?

Azure AD-functies geïntegreerde vooraf ondersteuning voor een groot aantal populaire SaaS-apps en human resources-systemen, evenals algemene ondersteuning voor apps die specifieke onderdelen van de standaard SCIM 2.0 implementeren.

Zie voor een lijst van alle toepassingen waarvoor Azure AD een vooraf geïntegreerde inrichting connector ondersteunt, de [lijst met zelfstudies bij de toepassing voor gebruikers inrichten](active-directory-saas-tutorial-list.md).

Zie voor meer informatie over het toevoegen van ondersteuning voor Azure AD-gebruikers inrichten tot een toepassing [SCIM gebruiken voor het automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen](active-directory-scim-provisioning.md).

Neem contact op met de Azure AD technisch team om aan te vragen van de inrichting ondersteuning voor aanvullende toepassingen indienen om een bericht via de [forum met feedback van Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).    

> [!NOTE]
> In de volgorde voor een toepassing voor de ondersteuning van geautomatiseerde gebruikersinrichting, moet deze eerst de benodigde Gebruikersbeheer API's voor het externe programma's voor het automatiseren van het maken, het onderhoud en de verwijdering van gebruikers opgeven. Er zijn daarom niet alle SaaS-apps compatibel met deze functie. Voor apps die ondersteuning voor gebruikersbeheer API's bieden, het technische team van Azure AD zijn vervolgens een inrichting connector voor deze apps bouwen en deze taak is geplaatst door de vereisten van de huidige en potentiële klanten. 
    
    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Hoe stel ik automatische inrichting tot een toepassing?

Configuratie van de Azure AD-service inricht voor een geselecteerde toepassing wordt gestart in de  **[Azure-portal](https://portal.azure.com)**. In de **Azure Active Directory > bedrijfstoepassingen** sectie **toevoegen**, klikt u vervolgens **alle**, en voeg vervolgens een van de volgende, afhankelijk van uw scenario:

* Alle toepassingen in de **toepassingen aanbevolen** sectie ondersteuning automatische inrichting. Zie de [lijst met zelfstudies bij de toepassing voor gebruikers inrichten](active-directory-saas-tutorial-list.md) voor andere toevoegen.

* De optie 'niet galerie application' gebruiken voor aangepaste ontwikkelde SCIM integraties

![Gallery](./media/active-directory-saas-app-provisioning/gallery.png)

Klik in het scherm application management-inrichting is geconfigureerd in de **inrichten** tabblad.

![Instellingen](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* **Beheerdersreferenties** moet worden opgegeven om de Azure AD-service waarmee deze verbinding maken met de API door de toepassing geleverd voor het Gebruikersbeheer inricht. In deze sectie kunt u e-mailmeldingen inschakelen als de referenties mislukken of inrichtingstaak van de probeert het [quarantaine](#quarantine).

* **Kenmerktoewijzingen** kunnen worden geconfigureerd die opgeven welke velden in het bronsysteem (voorbeeld: Azure AD) wordt de inhoud ervan met hebben gesynchroniseerd welke velden in het doelsysteem (voorbeeld: ServiceNow). Wanneer de doeltoepassing wordt ondersteund kunt in deze sectie u eventueel configureren inrichting van groepen naast gebruikersaccounts. 'Overeenkomende eigenschappen' kunt u selecteren welke velden worden gebruikt voor accounts tussen de systemen overeenkomen. "[Expressies](active-directory-saas-writing-expressions-for-attribute-mappings.md)' kunt u wijzigen en transformatie van de waarden die zijn opgehaald uit het bronsysteem voordat ze op het doelsysteem worden geschreven. Zie voor meer informatie [kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md).

![Instellingen](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Bereikfilters** de inrichting service zien welke gebruikers en groep in het bronsysteem moet worden ingericht en/of gemaakt met het doelsysteem. Er zijn twee aspecten bereikfilters die gezamenlijk worden geëvalueerd om te bepalen wie in het bereik voor het inrichten is:

    * **Filteren op kenmerkwaarden** -het menu 'Objectbereik bron' in de kenmerktoewijzingen Hiermee kunt u filteren op specifieke kenmerkwaarden. U kunt bijvoorbeeld opgeven dat alleen gebruikers met een kenmerk 'Afdeling' van 'Verkoop' binnen het bereik van de inrichting moet. Zie voor meer informatie [met bereik filters](active-directory-saas-scoping-filters.md).

    * **Filter op toewijzingen** -het menu 'Bereik' in de inrichting > sectie van de instellingen van de portal kunt u opgeven of alleen 'toegewezen' gebruikers en groepen moeten zich binnen het bereik van de inrichting, of als alle gebruikers in de Azure AD-directory moet ingericht. Zie voor informatie over ' ' gebruikers en groepen toewijzen, [een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md).
    
* **Instellingen** bepalen de werking van de inrichting-service voor een toepassing, met inbegrip van of deze nu wordt uitgevoerd of niet.

* **Controlelogboeken** bieden records van elke bewerking uitgevoerd door de Azure AD-service inricht. Zie voor meer informatie de [inrichting rapportagegids](active-directory-saas-provisioning-reporting.md).

![Instellingen](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

> [!NOTE]
> De inrichting van de service Azure AD-gebruiker kan ook worden geconfigureerd en beheerd via de [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).


## <a name="what-happens-during-provisioning"></a>Wat gebeurt er tijdens het inrichten?

Wanneer u Azure AD is het bronsysteem, de inrichting service gebruikt de [differentiële Query-functie van de Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) om gebruikers en groepen te bewaken. De inrichting-service wordt uitgevoerd voor een eerste synchronisatie tegen het bronsysteem en doelsysteem, gevolgd door periodieke incrementele synchronisaties. 

### <a name="initial-sync"></a>Initiële synchronisatie
Wanneer de inrichting-service wordt gestart, wordt de eerste synchronisatie ooit uitgevoerd:

1. Query uitvoeren op alle gebruikers en groepen uit het bronsysteem bij het ophalen van alle kenmerken zijn gedefinieerd in de [kenmerktoewijzingen](active-directory-saas-customizing-attribute-mappings.md).
2. Filteren van de gebruikers en groepen die zijn geretourneerd, met behulp van een geconfigureerd [toewijzingen](active-directory-coreapps-assign-user-azure-portal.md) of [op kenmerken gebaseerde bereikfilters](active-directory-saas-scoping-filters.md).
3. Wanneer een gebruiker wordt gevonden, worden toegewezen of in het bereik voor het inrichten van de service vraagt het doelsysteem voor een overeenkomende gebruiker met behulp van de aangewezen [overeenkomende kenmerken](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-properties). Voorbeeld: Als de naam van de userPrincipal in het bronsysteem het overeenkomende kenmerk is en toegewezen aan gebruikersnaam in het doelsysteem en vervolgens de inrichting service query uitgevoerd op het doelsysteem voor gebruikersnamen die overeenkomen met de naam userPrincipal waarden in het bronsysteem.
4. Als een overeenkomende gebruiker niet in het doelsysteem gevonden is, wordt deze gemaakt met behulp van de kenmerken van het bronsysteem geretourneerd.
5. Als een overeenkomende gebruiker wordt gevonden, wordt deze bijgewerkt met behulp van de kenmerken die wordt geleverd door het bronsysteem.
6. Als de kenmerktoewijzingen ' verwijzingskenmerken ' bevatten, worden aanvullende updates in de service uitgevoerd op het doelsysteem maken en koppelen van de objecten waarnaar wordt verwezen. Een gebruiker kan bijvoorbeeld een 'Manager'-kenmerk hebben in het doelsysteem die is gekoppeld aan een andere gebruiker gemaakt in het doelsysteem.
7. Een watermerk aan het einde van de eerste synchronisatie, wat zorgt voor het beginpunt voor de volgende incrementele synchronisaties bewaard.

Sommige servertoepassingen, zoals ondersteuning voor ServiceNow, Google Apps en het selectievakje niet alleen gebruikers inrichten, maar ook inrichting groepen en hun leden. In deze gevallen, als groep inrichting is ingeschakeld in de [toewijzingen](active-directory-saas-customizing-attribute-mappings.md), wordt de service inricht synchroniseert de gebruikers en groepen en vervolgens de groepslidmaatschappen vervolgens synchroniseert. 

### <a name="incremental-syncs"></a>Incrementele synchronisaties
Na de eerste synchronisatie worden alle volgende synchronisaties:

1. Query uitvoeren op het bronsysteem voor alle gebruikers en groepen die zijn bijgewerkt sinds de laatste watermerk is opgeslagen.
2. Filteren van de gebruikers en groepen die zijn geretourneerd, met behulp van een geconfigureerd [toewijzingen](active-directory-coreapps-assign-user-azure-portal.md) of [op kenmerken gebaseerde bereikfilters](active-directory-saas-scoping-filters.md).
3. Wanneer een gebruiker wordt gevonden, worden toegewezen of in het bereik voor het inrichten van de service vraagt het doelsysteem voor een overeenkomende gebruiker met behulp van de aangewezen [overeenkomende kenmerken](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-properties).
4. Als een overeenkomende gebruiker niet in het doelsysteem gevonden is, wordt deze gemaakt met behulp van de kenmerken van het bronsysteem geretourneerd.
5. Als een overeenkomende gebruiker wordt gevonden, wordt deze bijgewerkt met behulp van de kenmerken die wordt geleverd door het bronsysteem.
6. Als de kenmerktoewijzingen ' verwijzingskenmerken ' bevatten, worden aanvullende updates in de service uitgevoerd op het doelsysteem maken en koppelen van de objecten waarnaar wordt verwezen. Een gebruiker kan bijvoorbeeld een 'Manager'-kenmerk hebben in het doelsysteem die is gekoppeld aan een andere gebruiker gemaakt in het doelsysteem.
7. Als een gebruiker die eerder binnen het bereik van de inrichting van bereik (inclusief wordt niet-toegewezen) wordt verwijderd, wordt de gebruiker uitgeschakeld in het doelsysteem via een update van de service.
8. Als een gebruiker die eerder in het bereik voor het inrichten is uitgeschakeld of voorlopig verwijderd in het bronsysteem, wordt de gebruiker uitgeschakeld in het doelsysteem via een update van de service.
9. Als een gebruiker die eerder in het bereik voor het inrichten verwijderd in het bronsysteem is, de service worden verwijderd van de gebruiker in het doelsysteem. Gebruikers hebben in Azure AD verwijderd 30 dagen nadat ze voorlopig verwijderd.
10. Een nieuw watermerk aan het einde van de incrementele synchronisatie, wat zorgt voor het beginpunt voor de volgende incrementele synchronisaties bewaard.

>[!NOTE]
> U kunt eventueel de create, update of delete-bewerkingen uitschakelen met behulp van de **Objectacties gericht** selectievakjes in de [kenmerktoewijzingen](active-directory-saas-customizing-attribute-mappings.md) sectie. De logica voor het uitschakelen van een gebruiker tijdens een update is tevens worden beheerd via een kenmerk wordt toegewezen uit een veld zoals 'accountEnabled'.

De inrichting service blijft actief back to back incrementele synchronisaties voor onbepaalde tijd intervallen gedefinieerd in de [zelfstudie die specifiek zijn voor elke toepassing](active-directory-saas-tutorial-list.md), totdat een van de volgende gebeurtenissen zich voordoet:

* De service is handmatig gestopt met behulp van de Azure-portal of met de juiste Graph API-opdracht 
* Een nieuwe initiële synchronisatie is geactiveerd met behulp van de **status wissen en opnieuw opstarten** optie in de Azure portal of met de juiste Graph API-opdracht. Hierdoor worden gewist van elke opgeslagen watermerk en wordt alle bronobjecten moet opnieuw worden geëvalueerd.
* Een nieuwe initiële synchronisatie wordt geactiveerd na een wijziging in kenmerktoewijzingen of bereik filters. Dit ook een opgeslagen watermerk worden gewist en zorgt ervoor dat alle bronobjecten moet opnieuw worden geëvalueerd.
* Tijdens het inrichtingsproces gaat in quarantaine (Zie hieronder) vanwege een hoge Foutfrequentie en in quarantaine voor meer dan vier weken blijft. In dat geval kan wordt de service automatisch uitgeschakeld.

### <a name="errors-and-retries"></a>Fouten en nieuwe pogingen 
Als een afzonderlijke gebruiker kan niet worden toegevoegd, bijgewerkt of verwijderd in het doelsysteem vanwege een fout in het doelsysteem, klikt u vervolgens de bewerking uit te voeren in de volgende synchronisatiecyclus. Als de gebruiker mislukken blijft, begint de pogingen om te worden uitgevoerd met een lagere frequentie, geleidelijk schalen terug naar slechts één poging per dag. Los de fout door beheerders moet controleren de [controlelogboeken](active-directory-saas-provisioning-reporting.md) voor 'proces escrow' gebeurtenissen om te bepalen van de hoofdmap veroorzaken en de juiste actie ondernemen. Veelvoorkomende fouten kunnen opnemen:

* Gebruikers niet met een kenmerk ingevuld in het bronsysteem die is in het doelsysteem vereist
* De waarde van een kenmerk in het bronsysteem waarvoor er een unique-beperking in het doelsysteem is en dezelfde waarde hebben gebruikers is aanwezig in de record van een andere gebruiker

Deze fouten kunnen worden opgelost door de kenmerkwaarden voor de betrokken gebruiker in het bronsysteem aan te passen of door de kenmerktoewijzingen als u geen conflicten aan te passen.   

### <a name="quarantine"></a>Quarantaine
Als de meeste of alle van de oproepen opzichte van het doelsysteem consistent ten mislukt vanwege een fout (zoals in het geval van ongeldige beheerdersreferenties), en de inrichtingstaak gaat dan in een status 'in quarantaine plaatsen'. Hiermee wordt aangegeven in de [inrichting overzichtsrapport](active-directory-saas-provisioning-reporting.md), en via e-mail als e-mailmeldingen zijn geconfigureerd in de Azure portal. 

Wanneer het in quarantaine, is de frequentie van incrementele synchronisaties geleidelijk wordt verlaagd één keer per dag. 

Inrichtingstaak van de wordt verwijderd uit quarantaine nadat u alle van de strijdige fouten worden opgelost en de volgende synchronisatiecyclus wordt gestart. Als de taak in quarantaine gedurende meer dan vier weken blijft, wordt de taak is uitgeschakeld.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Hoe lang duurt het inrichten van mijn gebruikers?**

Prestaties zijn afwijken, afhankelijk van of uw inrichtingstaak van een initiële synchronisatie of een incrementele synchronisatie wordt uitgevoerd.

Voor de eerste wordt gesynchroniseerd worden de tijd die nodig zijn om te voltooien rechtstreeks afhankelijk van hoeveel gebruikers, groepen en groepsleden aanwezig in het bronsysteem zijn. Initiële synchronisatie binnen een paar minuten kunnen uitvoeren die zeer kleine bronsystemen honderden objecten. Bronsystemen met honderden of duizenden of miljoenen gecombineerde objecten duurt langer.

Voor incrementele synchronisaties is de tijd die nodig is afhankelijk van de wijzigingen in synchronisatiecyclus gedetecteerd. Als er minder dan 5000 gebruikers- of wijzigingen in groepslidmaatschappen gedetecteerd, kunnen deze vaak in een cyclus 40 minuten worden gesynchroniseerd. 

Houd er rekening mee dat algehele prestaties afhankelijk van de bron-en doelsystemen is. Sommige doelsystemen implementeren aanvraag frequentielimieten en beperking dat kunt impact prestaties bij grote synchronisatiebewerkingen en de vooraf gemaakte Azure AD-connectors voor deze systemen inrichting rekening.

Is ook trager als er veel fouten (opgenomen in de [controlelogboeken](active-directory-saas-provisioning-reporting.md)) en de inrichting-service is geworden in een status 'quarantaine'.

**Hoe kan ik de prestaties van synchronisatie verbeteren?**

De meeste prestatieproblemen optreden tijdens de initiële synchronisatie van systemen die een groot aantal groepen en leden van de beveiligingsgroep hebt.

Als de synchronisatie van groepen of groepslidmaatschappen is niet vereist, kan de synchronisatie-prestaties aanzienlijk worden verbeterd door:

1. Instellen van de **inrichten > Instellingen > bereik** menu **Alles synchroniseren**, in plaats van toegewezen gebruikers en groepen worden gesynchroniseerd.
2. Gebruik [bereikfilters](active-directory-saas-scoping-filters.md) in plaats van de toewijzingen voor het filteren van de lijst met gebruikers die zijn ingericht.

> [!NOTE]
> Voor toepassingen die ondersteuning bieden voor inrichting van namen en eigenschappen van de groep (zoals ServiceNow en Google Apps), minder uitschakelen van dit ook tijd die nodig is voor een initiële synchronisatie te voltooien. Als u niet inrichten namen van groepen en groepslidmaatschappen voor uw toepassing wilt, kunt u uitschakelen in de [kenmerktoewijzingen](active-directory-saas-customizing-attribute-mappings.md) van de inrichting van de configuratie.

**Hoe kan ik de voortgang van de inrichtingstaak van de huidige bijhouden**

Zie de [inrichting rapportagegids](active-directory-saas-provisioning-reporting.md).

**Hoe weet ik als gebruikers geen goed ophalen ingericht?**

Alle fouten worden vastgelegd in de Azure AD controlelogboeken. Zie voor meer informatie de [inrichting rapportagegids](active-directory-saas-provisioning-reporting.md).

**Hoe kan ik een toepassing die geschikt is voor de inrichting service maken?**

Zie [SCIM gebruiken voor het automatisch inrichten van gebruikers en groepen van Azure Active Directory naar toepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning).

**Hoe kan ik feedback aan het technische team?**

Contact met ons opnemen via de [forum met feedback van Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).


## <a name="related-articles"></a>Verwante artikelen:
* [Lijst met zelfstudies over het integreren van SaaS-Apps](active-directory-saas-tutorial-list.md)
* [Kenmerktoewijzingen voor gebruikers inrichten aanpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Expressies voor kenmerktoewijzingen schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereikfilters voor gebruikers inrichten](active-directory-saas-scoping-filters.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](active-directory-scim-provisioning.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Overzicht van Azure AD-synchronisatie API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
* [Plan voor uitgaande gebruikers inrichten van een toepassing de stapsgewijze implementatie](https://aka.ms/userprovisioningdeploymentplan)

