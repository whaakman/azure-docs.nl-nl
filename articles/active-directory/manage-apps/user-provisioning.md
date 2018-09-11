---
title: Geautomatiseerde SaaS app gebruikers inrichten in Azure AD | Microsoft Docs
description: Een inleiding tot hoe u Azure AD kunt gebruiken voor het automatisch inrichten, inrichting ongedaan maken en continu bijwerken gebruikersaccounts voor meerdere externe SaaS-toepassingen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: 680cea983fb7435bf4492fc295e29f3a234a4323
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356812"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Gebruiker-inrichting en ongedaan maken van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Wat is geautomatiseerde gebruikersinrichting voor SaaS-apps?
Azure Active Directory (Azure AD) kunt u het maken, onderhoud en verwijderen van gebruikers-id's in de cloud automatiseren ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) toepassingen, zoals Dropbox, Salesforce, ServiceNow en meer.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

**Hieronder volgen enkele voorbeelden van deze functie kunt u doen:**

* Automatisch nieuwe accounts maken in de juiste systemen voor nieuwe personen die lid worden van uw team of organisatie.
* Deactiveren accounts automatisch in de juiste systemen wanneer mensen uw team of organisatie verlaten.
* Zorg ervoor dat de identiteiten in uw apps en systemen up-to-date op basis van wijzigingen in de directory of uw HR-systeem gehouden worden.
* Niet-gebruikers objecten, zoals groepen, toepassingen die ondersteuning bieden voor deze inrichten.

**Geautomatiseerde gebruikersinrichting bevat ook de volgende functionaliteit:**

* De mogelijkheid om bestaande identiteiten tussen bron en doel-systemen.
* Aanpasbare kenmerktoewijzingen die welke gegevens bepalen moeten stromen van het bronsysteem op het doelsysteem.
* Optioneel e-mailwaarschuwingen voor fouten bij het inrichten
* Rapportage en activiteit Logboeken om te helpen bij het bewaken en problemen oplossen.

## <a name="why-use-automated-provisioning"></a>Waarom een geautomatiseerde inrichting gebruiken?
Enkele veelvoorkomende redenen voor het gebruik van deze functie zijn onder andere:

* De kosten, inefficiënt en menselijke fouten die zijn gekoppeld aan handmatige inrichting processen voorkomen.
* De kosten die zijn gekoppeld aan die als host fungeert en onderhouden van aangepaste oplossingen voor inrichting en scripts te voorkomen
* Voor het beveiligen van uw organisatie door direct identiteit van gebruikers verwijderen uit sleutel SaaS-apps wanneer ze de organisatie verlaten.
* Eenvoudig een groot aantal gebruikers in een bepaalde SaaS-toepassing of het systeem importeren.
* Om te profiteren met één set met beleid om te bepalen die is ingericht en die kunnen zich aanmelden bij een app.

## <a name="how-does-automatic-provisioning-work"></a>Hoe werkt automatische inrichting samen?
    
De **Azure AD-inrichtingsservice** gebruikers inricht voor SaaS-apps en andere systemen, door verbinding te maken met de eindpunten van gebruiker beheer-API is geleverd door de leverancier van elke toepassing. Deze gebruiker management API-eindpunten kunnen Azure AD via een programma maken, bijwerken en verwijderen van gebruikers. Voor de geselecteerde toepassingen die kunt ook de provisioning-service maken, bijwerken en verwijderen van aanvullende identiteit gerelateerde objecten, zoals groepen en rollen. 

![Inrichting](./media/user-provisioning/provisioning0.PNG)
*afbeelding 1: de Azure AD-inrichtingsservice*

![Uitgaande Provisioning](./media/user-provisioning/provisioning1.PNG)
*afbeelding 2: "Uitgaande" gebruiker inrichtingswerkstroom van Azure AD met populaire SaaS-toepassingen*

![Inbound Provisioning](./media/user-provisioning/provisioning2.PNG)
*afbeelding 3: 'Inkomend' gebruiker inrichtingswerkstroom van populaire Human Capital Management (HCM)-toepassingen naar Azure Active Directory en Windows Server Active Directory*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Welke toepassingen en systemen kan ik gebruiken met Azure AD automatisch gebruikers inrichten?

Azure AD-functies geïntegreerde vooraf ondersteuning voor diverse populaire SaaS-apps en HR-systemen, evenals de algemene ondersteuning voor apps die specifieke onderdelen van de standaard SCIM 2.0 implementeren.

### <a name="pre-integrated-applications"></a>Vooraf geïntegreerde toepassingen
Zie voor een lijst van alle toepassingen waarvoor Azure AD biedt ondersteuning voor een vooraf geïntegreerde inrichting connector, de [lijst met zelfstudies voor toepassingen voor het inrichten van gebruikers](../saas-apps/tutorial-list.md).

Neem contact op met de Azure AD een bericht via engineeringteam om aan te vragen van inrichting ondersteuning voor aanvullende toepassingen indienen de [forum met feedback van Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> In de order voor een toepassing voor de ondersteuning van geautomatiseerde gebruikersinrichting, moet deze eerst de benodigde Gebruikersbeheer API's waarmee externe programma's voor het automatiseren van het maken, onderhoud en verwijdering van gebruikers opgeven. Er zijn daarom niet alle SaaS-apps compatibel zijn met deze functie. Het technische team van Azure AD worden vervolgens een inrichting connector naar deze apps bouwen voor apps die ondersteuning voor gebruikersbeheer API's bieden, en dit werk met prioriteit wordt toegepast door de behoeften van de huidige en potentiële klanten. 

### <a name="connecting-applications-that-support-scim-20"></a>Het verbinden van toepassingen die ondersteuning bieden voor SCIM 2.0
Voor meer informatie over het algemeen verbinding maken met toepassingen die SCIM implementeren 2.0 - op basis van gebruiker-management-API's, Zie [SCIM gebruiken voor het automatisch inrichten van gebruikers en groepen uit Azure Active Directory met toepassingen](use-scim-to-provision-users-and-groups.md).

    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Hoe stel ik automatische inrichting tot een toepassing?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Configuratie van de Azure AD-inrichtingsservice voor een geselecteerde toepassing wordt gestart de  **[Azure-portal](https://portal.azure.com)**. In de **Azure Active Directory > bedrijfstoepassingen** sectie, selecteer **toevoegen**, klikt u vervolgens **alle**, en voegt u een van de volgende, afhankelijk van uw scenario:

* Alle toepassingen in de **aanbevolen toepassingen** sectie ondersteuning voor automatische inrichting. Zie de [lijst met zelfstudies voor toepassingen voor het inrichten van gebruikers](../saas-apps/tutorial-list.md) voor nieuwe zijn in.

* Gebruik de optie 'buiten de galerie application' voor aangepaste SCIM integraties

![Galerie](./media/user-provisioning/gallery.png)

In het scherm voor het beheren van toepassing inrichting is geconfigureerd in de **Provisioning** tabblad.

![Instellingen](./media/user-provisioning/provisioning_settings0.PNG)


* **Beheerdersreferenties** moet worden opgegeven om de Azure AD-inrichtingsservice waarmee deze verbinding maken met de API geleverd door de toepassing voor het Gebruikersbeheer. Deze sectie kunt u het e-mailmeldingen inschakelen als de referenties mislukt of de inrichtingstaak krijgt ook [quarantaine](#quarantine).

* **Kenmerktoewijzingen** kan worden geconfigureerd die opgeven welke velden in het bronsysteem (voorbeeld: Azure AD), hebben hun inhoud gesynchroniseerd velden in het doelsysteem (voorbeeld: ServiceNow). Wanneer de doeltoepassing wordt ondersteund kunt in deze sectie u desgewenst configureren toewijzing van groepen naast gebruikersaccounts. 'Overeenkomende eigenschappen' kunt u selecteren welke velden worden gebruikt zodat deze overeenkomen met accounts tussen de systemen. "[Expressies](functions-for-customizing-application-data.md)' kunt u wijzigen en transformeren van de waarden die zijn opgehaald uit het bronsysteem voordat ze worden geschreven naar het doelsysteem. Zie voor meer informatie, [kenmerktoewijzingen aanpassen](customize-application-attributes.md).

![Instellingen](./media/user-provisioning/provisioning_settings1.PNG)

* **Bereikfilters** de inrichtingsservice zien welke gebruikers en groep in het bronsysteem moet worden ingericht en/of de inrichting ongedaan gemaakt op het doelsysteem. Er zijn twee aspecten bereikfilters die gezamenlijk worden geëvalueerd om te bepalen wie binnen het bereik voor het inrichten is:

    * **Filteren op kenmerkwaarden** -menu aan de 'Bereik van het bronobject' in de kenmerktoewijzingen Hiermee kunt u filteren op specifieke kenmerkwaarden. U kunt bijvoorbeeld opgeven dat alleen gebruikers met een kenmerk 'Afdeling' van 'Verkoop' binnen het bereik voor het inrichten moet. Zie voor meer informatie, [met behulp van bereikfilters](define-conditional-rules-for-provisioning-user-accounts.md).

    * **Filter voor toewijzingen** -menu aan de 'Bereik' in de inrichting > Instellingen-sectie van de portal kunt u opgeven of alleen 'toegewezen' gebruikers en groepen moet binnen het bereik voor het inrichten of als alle gebruikers in de Azure AD-directory moeten zijn ingericht. Zie voor meer informatie over het 'toewijzen' gebruikers en groepen [een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](assign-user-or-group-access-portal.md).
    
* **Instellingen voor** bepalen de werking van de provisioning-service voor een toepassing, met inbegrip van of deze nu wordt uitgevoerd of niet.

* **Auditlogboeken** staan records van elke bewerking die wordt uitgevoerd door de Azure AD-inrichtingsservice. Zie voor meer informatie de [inrichting handleiding over rapportering](check-status-user-account-provisioning.md).

![Instellingen](./media/user-provisioning/audit_logs.PNG)

> [!NOTE]
> De inrichtingsservice Azure AD-gebruiker kan ook worden geconfigureerd en beheerd met behulp van de [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).


## <a name="what-happens-during-provisioning"></a>Wat gebeurt er tijdens het inrichten?

Wanneer Azure AD het bronsysteem is, de provisioning-service gebruikt de [differentiële Query-functie van de Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) om gebruikers en groepen te bewaken. De inrichtingsservice wordt een eerste synchronisatie uitgevoerd tegen het bronsysteem en doelsysteem, gevolgd door periodieke incrementele synchronisaties. 

### <a name="initial-sync"></a>Initiële synchronisatie
Wanneer de provisioning-service wordt gestart, wordt de eerste synchronisatie heeft eerder een uitgevoerd:

1. Query uitvoeren op alle gebruikers en groepen uit het bronsysteem bij het ophalen van alle kenmerken die zijn gedefinieerd de [kenmerktoewijzingen](customize-application-attributes.md).
2. Filteren van de gebruikers en groepen die zijn geretourneerd, met behulp van een geconfigureerd [toewijzingen](assign-user-or-group-access-portal.md) of [op kenmerken gebaseerde bereikfilters](define-conditional-rules-for-provisioning-user-accounts.md).
3. Wanneer een gebruiker worden toegewezen is geconstateerd of binnen het bereik voor het inrichten van de service zoekopdrachten uit op het doelsysteem voor een overeenkomende gebruiker met behulp van de aangewezen [die overeenkomt met de kenmerken](customize-application-attributes.md#understanding-attribute-mapping-properties). Voorbeeld: Als de naam van de userPrincipal in het bronsysteem het overeenkomende kenmerk is en toegewezen aan vraagt gebruikersnaam in het doelsysteem en vervolgens de provisioning-service het doelsysteem voor gebruikersnamen die overeenkomen met de naam userPrincipal waarden in het bronsysteem.
4. Als een overeenkomende gebruiker niet in het doelsysteem gevonden is, wordt deze gemaakt met behulp van de kenmerken die door het bronsysteem geretourneerd.
5. Als een overeenkomende gebruiker wordt gevonden, wordt deze bijgewerkt met behulp van de kenmerken die is geleverd door het bronsysteem.
6. Als de kenmerktoewijzingen 'verwijzing' kenmerken bevatten, uitvoert de service extra updates op het doelsysteem te maken en koppelen van de objecten waarnaar wordt verwezen. Een gebruiker kan bijvoorbeeld een 'Manager'-kenmerk hebben in het doelsysteem, die is gekoppeld aan een andere gebruiker hebt gemaakt in het doelsysteem.
7. Een watermerk aan het einde van de initiële synchronisatie, waarmee het beginpunt voor de volgende incrementele synchronisaties behouden.

Sommige toepassingen, zoals ServiceNow, Google Apps en Box-ondersteuning niet alleen inrichten van gebruikers, maar ook groepen en leden wordt ingericht. In deze gevallen, als groep inrichten is ingeschakeld de [toewijzingen](customize-application-attributes.md), wordt de inrichtingsservice synchroniseert de gebruikers en groepen en vervolgens het groepslidmaatschap later te synchroniseren. 

### <a name="incremental-syncs"></a>Incrementele synchronisaties
Na de initiële synchronisatie zullen alle volgende synchronisaties:

1. Query uitvoeren op het bronsysteem voor alle gebruikers en groepen die zijn bijgewerkt sinds de laatste watermerk is opgeslagen.
2. Filteren van de gebruikers en groepen die zijn geretourneerd, met behulp van een geconfigureerd [toewijzingen](assign-user-or-group-access-portal.md) of [op kenmerken gebaseerde bereikfilters](define-conditional-rules-for-provisioning-user-accounts.md).
3. Wanneer een gebruiker worden toegewezen is geconstateerd of binnen het bereik voor het inrichten van de service zoekopdrachten uit op het doelsysteem voor een overeenkomende gebruiker met behulp van de aangewezen [die overeenkomt met de kenmerken](customize-application-attributes.md#understanding-attribute-mapping-properties).
4. Als een overeenkomende gebruiker niet in het doelsysteem gevonden is, wordt deze gemaakt met behulp van de kenmerken die door het bronsysteem geretourneerd.
5. Als een overeenkomende gebruiker wordt gevonden, wordt deze bijgewerkt met behulp van de kenmerken die is geleverd door het bronsysteem.
6. Als de kenmerktoewijzingen 'verwijzing' kenmerken bevatten, uitvoert de service extra updates op het doelsysteem te maken en koppelen van de objecten waarnaar wordt verwezen. Een gebruiker kan bijvoorbeeld een 'Manager'-kenmerk hebben in het doelsysteem, die is gekoppeld aan een andere gebruiker hebt gemaakt in het doelsysteem.
7. Wanneer een gebruiker die eerder in het bereik vallen voor inrichting van bereik (inclusief wordt niet-toegewezen) wordt verwijderd, wordt de gebruiker uitgeschakeld in het doelsysteem via een update van de service.
8. Als een gebruiker die eerder in het bereik vallen voor inrichting is uitgeschakeld of voorlopig in het bronsysteem verwijderde, wordt de gebruiker uitgeschakeld in het doelsysteem via een update van de service.
9. Als een gebruiker die eerder in het bereik vallen voor inrichting verwijderd in het bronsysteem is, wordt de gebruiker door de service verwijderd in het doelsysteem. Gebruikers zijn in Azure AD-verwijderd 30 dagen nadat ze voorlopig verwijderde zijn.
10. Een nieuw watermerk aan het einde van de incrementele synchronisatie, waarmee het beginpunt voor de volgende incrementele synchronisaties behouden.

>[!NOTE]
> U kunt eventueel de create, update of delete-bewerkingen uitschakelen met behulp van de **Objectacties als doel** selectievakjes in de [kenmerktoewijzingen](customize-application-attributes.md) sectie. De logica voor het uitschakelen van een gebruiker tijdens het bijwerken wordt beheerd via een kenmerk wordt toegewezen uit een veld, zoals 'accountEnabled'.

De provisioning-service blijft back to back incrementele synchronisaties voor onbepaalde tijd actief met intervallen gedefinieerd in de [zelfstudie die specifiek zijn voor elke toepassing](../saas-apps/tutorial-list.md), totdat een van de volgende gebeurtenissen zich voordoet:

* De service is handmatig gestopt met behulp van de Azure-portal of met behulp van de juiste Graph API-opdracht 
* Een nieuwe initiële synchronisatie wordt geactiveerd met behulp van de **status wissen en opnieuw starten** optie in Azure portal of met behulp van de juiste Graph API-opdracht. Hiermee wist u alle opgeslagen watermerk en zorgt ervoor dat alle bronobjecten moet opnieuw worden geëvalueerd.
* Een nieuwe initiële synchronisatie wordt geactiveerd vanwege een wijziging in de kenmerktoewijzingen of bereikfilters toevoegen. Dit ook wist u alle opgeslagen watermerk en zorgt ervoor dat alle bronobjecten moet opnieuw worden geëvalueerd.
* Het inrichtingsproces gaat in quarantaine (Zie hieronder) vanwege een hoog aantal fouten bij, en in quarantaine voor meer dan vier weken blijft. In dat geval kan wordt de service automatisch uitgeschakeld.

### <a name="errors-and-retries"></a>Fouten en nieuwe pogingen 
Als een afzonderlijke gebruiker kan niet worden toegevoegd, bijgewerkt of verwijderd in het doelsysteem vanwege een fout in het doelsysteem, klikt u vervolgens de bewerking wordt opnieuw geprobeerd in de volgende synchronisatiecyclus. Als de gebruiker mislukken blijft, begint de nieuwe pogingen worden uitgevoerd met een lagere frequentie, geleidelijk terug naar slechts één poging per dag te schalen. Los de fout op door beheerders moet om te controleren of de [auditlogboeken](check-status-user-account-provisioning.md) voor 'proces-escrow' gebeurtenissen om te bepalen van de hoofdmap veroorzaken en de juiste actie ondernemen. Veelvoorkomende fouten kunnen opnemen:

* Gebruikers niet met een kenmerk is ingevuld in het bronsysteem die is in het doelsysteem vereist
* Gebruikers met een waarde van het kenmerk in het bronsysteem waarvoor er een unieke beperking in het doelsysteem is en dezelfde waarde is aanwezig in de record van een andere gebruiker

Deze fouten kunnen worden opgelost door aan te passen van de kenmerkwaarden voor de betrokken gebruiker in het bronsysteem, of door de kenmerktoewijzingen als u wilt niet leiden tot conflicten aan te passen.   

### <a name="quarantine"></a>Quarantaine
Als de meeste of alle van de aanroepen opzichte van het doelsysteem consistent ten mislukt vanwege een fout (zoals in het geval van ongeldige beheerdersreferenties), en vervolgens de inrichtingstaak krijgt de status 'in quarantaine plaatsen'. Hiermee wordt aangegeven in de [inrichting overzichtsrapport](check-status-user-account-provisioning.md), en via e-mail als e-mailmeldingen zijn geconfigureerd in Azure portal. 

Wanneer het in quarantaine, de frequentie van incrementele synchronisaties geleidelijk teruggebracht tot één keer per dag. 

De taak wordt verwijderd uit quarantaine nadat u alle van de afwijkende fouten opgelost en de volgende synchronisatiecyclus start. Als de taak in quarantaine meer dan vier weken blijft, wordt de taak is uitgeschakeld.


## <a name="how-long-will-it-take-to-provision-users"></a>Hoe lang duurt het inrichten van gebruikers?

Prestaties is afhankelijk van of uw provisioning-taak wordt uitgevoerd een eerste synchronisatie of een incrementele synchronisatie, zoals beschreven in de vorige sectie.

Voor **wordt gesynchroniseerd voor de eerste**, de tijd voor de taak is afhankelijk van diverse factoren, waaronder het aantal gebruikers en groepen in het bereik vallen voor inrichting en het totale aantal gebruikers en groepen in het bronsysteem. Een uitgebreide lijst van de factoren die invloed hebben op prestaties van de initiële synchronisatie worden later samengevat in deze sectie.

Voor **incrementele synchronisaties**, de tijd voor de taak is afhankelijk van het aantal wijzigingen gedetecteerd in deze synchronisatiecyclus. Als er minder dan 5000 gebruiker of groepslidmaatschap wijzigingen, kan de taak binnen een synchronisatiecyclus één incrementele voltooien. 

De volgende tabel geeft een overzicht van synchronisatieschema de synchronisatietijden voor algemene inrichting scenario's. In deze scenario's, het bronsysteem is Azure AD en het doelsysteem is een SaaS-toepassing. De synchronisatietijden zijn afgeleid van een statistische analyse van synchronisatietaken voor de SaaS-toepassingen ServiceNow, werkplek, Salesforce en Google Apps.


| Scopeconfiguratie | Gebruikers, groepen en leden binnen het bereik | Tijd van de initiële synchronisatie | Tijd van incrementele synchronisatie |
| -------- | -------- | -------- | -------- |
| Toegewezen gebruikers en groepen alleen synchroniseren |  < 1000 |  < 30 minuten | < 30 minuten |
| Toegewezen gebruikers en groepen alleen synchroniseren |  1.000 - 10.000 | 142 - 708 minuten | < 30 minuten |
| Toegewezen gebruikers en groepen alleen synchroniseren |   10.000 - 100.000 | 1,170 - 2,340 minuten | < 30 minuten |
| Alle gebruikers en groepen in Azure AD worden gesynchroniseerd |  < 1000 | < 30 minuten  | < 30 minuten |
| Alle gebruikers en groepen in Azure AD worden gesynchroniseerd |  1.000 - 10.000 | < 30 tot 120 minuten | < 30 minuten |
| Alle gebruikers en groepen in Azure AD worden gesynchroniseerd |  10.000 - 100.000  | 713 - 1,425 minuten | < 30 minuten |
| Alle gebruikers in Azure AD worden gesynchroniseerd|  < 1000  | < 30 minuten | < 30 minuten |
| Alle gebruikers in Azure AD worden gesynchroniseerd | 1.000 - 10.000  | 43 - 86 minuten | < 30 minuten |


Voor de configuratie van **synchronisatie toegewezen gebruikers en groepen alleen**, kunt u de volgende formules om te bepalen van de geschatte minimale en maximale verwacht **voor de eerste synchronisatie** tijden:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Overzicht van de factoren die invloed hebben op de tijd die nodig zijn om uit te voeren een **voor de eerste synchronisatie**:

* Het totale aantal gebruikers en groepen in het bereik vallen voor inrichting

* Het totale aantal gebruikers, groepen en groepsleden aanwezig zijn in het bronsysteem (Azure AD)

* Of gebruikers in het bereik voor het inrichten van zijn afgestemd op bestaande gebruikers in de doeltoepassing of moeten worden gemaakt voor de eerste keer. Synchronisatietaken voor dat alle gebruikers worden gemaakt voor de eerste keer duurt circa *tweemaal zo lang* als taken voor die alle gebruikers zijn afgestemd op bestaande gebruikers worden gesynchroniseerd.

* Aantal fouten in de [auditlogboeken](check-status-user-account-provisioning.md). Is trager als er veel fouten en de provisioning-service is geworden in een status in quarantaine plaatsen 

* Aanvragen frequentielimieten en beperking geïmplementeerd door het doelsysteem. Sommige doelsystemen implementeren aanvraag frequentielimieten en beperkingen die de prestaties tijdens grote synchronisatiebewerkingen kunnen beïnvloeden. In deze omstandigheden kan een app die te veel aanvragen te snel ontvangt de respons vertragen of Verbreek de verbinding. Voor betere prestaties wordt de verbinding moet aanpassen door de app-aanvragen niet sneller dan ze kan worden verwerkt door de app verzendt. Inrichting connectors die zijn gemaakt door Microsoft u deze aanpassing. 

* Het aantal en de grootte van de toegewezen groepen. Toegewezen groepen synchroniseren duurt langer dan het synchroniseren van gebruikers. Zowel het aantal en de grootte van de toegewezen groepen invloed hebben op prestaties. Als een toepassing heeft [toewijzingen ingeschakeld voor synchronisatie van de groep object](customize-application-attributes.md#editing-group-attribute-mappings)en eigenschappen, zoals namen groeperen lidmaatschappen naast gebruikers worden gesynchroniseerd. Deze extra wordt gesynchroniseerd duurt langer dan alleen synchroniseren gebruikersobjecten.


##<a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>Hoe kan ik zien als gebruikers juist worden ingericht?

Alle bewerkingen die worden uitgevoerd door de gebruiker inrichtingsservice worden vastgelegd in de Azure AD auditlogboeken. Dit omvat alle lees- en schrijfbewerkingen die zijn aangebracht in de bron en doel-systemen, evenals de gegevens van de gebruiker is gelezen of geschreven tijdens elke bewerking.

Zie voor informatie over hoe de lees de auditlogboeken beschikbaar zijn in Azure portal, de [inrichting handleiding over rapportering](check-status-user-account-provisioning.md).


##<a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>Hoe los ik problemen met het inrichten van gebruikers?

Zie voor instructies over het automatisch inrichten van gebruikers oplossen scenario's gebaseerde [problemen bij het configureren en inrichten van gebruikers voor een toepassing](application-provisioning-config-problem.md).


##<a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>Wat zijn de aanbevolen procedures voor het implementeren van automatisch inrichten van gebruikers?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Zie voor een voorbeeld van stapsgewijze implementatie-plan voor het inrichten van uitgaande gebruikers naar een toepassing, de [identiteit Deployment Guide voor het inrichten van gebruikers](https://aka.ms/userprovisioningdeploymentplan).


## <a name="related-articles"></a>Verwante artikelen:
* [Lijst met zelfstudies over het integreren van SaaS-Apps](../saas-apps/tutorial-list.md)
* [Kenmerktoewijzingen voor het inrichten van gebruikers aan te passen](customize-application-attributes.md)
* [Expressies schrijven voor kenmerktoewijzingen](functions-for-customizing-application-data.md)
* [Bereikfilters toevoegen voor het inrichten van gebruikers](define-conditional-rules-for-provisioning-user-accounts.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](use-scim-to-provision-users-and-groups.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Overzicht van Azure AD-synchronisatie API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
