---
title: Geautomatiseerde SaaS app gebruikers inrichten in Azure AD | Microsoft Docs
description: Een inleiding tot hoe u Azure AD kunt gebruiken voor het automatisch inrichten, inrichting ongedaan maken en continu bijwerken gebruikersaccounts voor meerdere externe SaaS-toepassingen.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 68c235f32d003adcddecb98c20f30c517c723617
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723943"
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Gebruiker-inrichting en ongedaan maken van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren

Azure Active Directory (Azure AD) kunt u het maken, onderhoud en verwijderen van gebruikers-id's in de cloud automatiseren ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) toepassingen, zoals Dropbox, Salesforce, ServiceNow en meer. Dit staat bekend als geautomatiseerde gebruikersinrichting voor SaaS apps.

> [!VIDEO https://www.youtube.com/embed/_ZjARPpI6NI]

Deze functie kunt u:

- Automatisch nieuwe accounts maken in de juiste systemen voor nieuwe personen die lid worden van uw team of organisatie.
- Deactiveren accounts automatisch in de juiste systemen wanneer mensen uw team of organisatie verlaten.
- Zorg ervoor dat de identiteiten in uw apps en systemen up-to-date op basis van wijzigingen in de directory of uw HR-systeem gehouden worden.
- Niet-gebruikers objecten, zoals groepen, toepassingen die ondersteuning bieden voor deze inrichten.

Geautomatiseerde gebruikersinrichting bevat ook deze functionaliteit:

- De mogelijkheid om bestaande identiteiten tussen bron en doel-systemen.
- Aanpasbare kenmerktoewijzingen die welke gegevens bepalen moeten stromen van het bronsysteem op het doelsysteem.
- Optioneel e-mailwaarschuwingen voor fouten bij het inrichten.
- Rapportage en activiteit Logboeken om te helpen bij het bewaken en problemen oplossen.

## <a name="why-use-automated-provisioning"></a>Waarom een geautomatiseerde inrichting gebruiken?

Enkele veelvoorkomende redenen voor het gebruik van deze functie zijn onder andere:

- De kosten, inefficiënt en menselijke fouten die zijn gekoppeld aan handmatige inrichting processen voorkomen.
- De kosten die zijn gekoppeld aan die als host fungeert en onderhouden van aangepaste oplossingen voor inrichting en scripts vermijden.
- De beveiliging van uw organisatie door direct identiteit van gebruikers verwijderen uit sleutel SaaS-apps wanneer ze de organisatie verlaten.
- Eenvoudig importeren van een groot aantal gebruikers in een bepaalde SaaS-toepassing of het systeem.
- Een enkele set beleidsregels om te bepalen die is ingericht en die kunnen zich aanmelden bij een app hebben.

## <a name="how-does-automatic-provisioning-work"></a>Hoe werkt automatische inrichting samen?

De **Azure AD-inrichtingsservice** gebruikers inricht voor SaaS-apps en andere systemen door verbinding te maken met de eindpunten van gebruiker beheer-API is geleverd door de leverancier van elke toepassing. Deze gebruiker management API-eindpunten kunnen Azure AD via een programma maken, bijwerken en verwijderen van gebruikers. Voor de geselecteerde toepassingen kan de provisioning-service ook maken, bijwerken en aanvullende identiteit gerelateerde objecten, zoals groepen en rollen verwijderen.

![Azure AD-inrichtingsservice](./media/user-provisioning/provisioning0.PNG)
*afbeelding 1: De Provisioning-Service van Azure AD*

![Uitgaande gebruikersinrichting werkstroom](./media/user-provisioning/provisioning1.PNG)
*afbeelding 2: "Uitgaande" gebruiker inrichtingswerkstroom van Azure AD met populaire SaaS-toepassingen*

![Inkomende gebruikersinrichting werkstroom](./media/user-provisioning/provisioning2.PNG)
*afbeelding 3: "Binnenkomende" inrichten werkstroom van de gebruiker van populaire Human Capital Management (HCM)-toepassingen naar Azure Active Directory en Windows Server Active Directory*

## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Welke toepassingen en systemen kan ik gebruiken met Azure AD automatisch gebruikers inrichten?

Azure AD-functies vooraf geïntegreerde ondersteuning voor veel populaire SaaS-apps en human resources-systemen en algemene ondersteuning voor apps die specifieke onderdelen van de standaard SCIM 2.0 implementeren.

### <a name="pre-integrated-applications"></a>Vooraf geïntegreerde toepassingen

Zie voor een lijst van alle toepassingen waarvoor Azure AD biedt ondersteuning voor een vooraf geïntegreerde inrichting connector, de [lijst met zelfstudies voor toepassingen voor het inrichten van gebruikers](../saas-apps/tutorial-list.md).

Neem contact op met de Azure AD een bericht via engineeringteam om aan te vragen van inrichting ondersteuning voor aanvullende toepassingen indienen de [forum met feedback van Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/filters/new?category_id=172035).

> [!NOTE]
> In de order voor een toepassing voor de ondersteuning van geautomatiseerde gebruikersinrichting, moet deze eerst de benodigde Gebruikersbeheer API's waarmee externe programma's voor het automatiseren van het maken, onderhoud en verwijdering van gebruikers opgeven. Er zijn daarom niet alle SaaS-apps compatibel zijn met deze functie. Voor apps die ondersteuning voor gebruikersbeheer API's bieden, het technische team van Azure AD kan vervolgens een inrichting connector naar deze apps maken en dit werk met prioriteit wordt toegepast door de behoeften van de huidige en potentiële klanten.

### <a name="connecting-applications-that-support-scim-20"></a>Het verbinden van toepassingen die ondersteuning bieden voor SCIM 2.0

Voor meer informatie over het algemeen verbinding maken met toepassingen die SCIM implementeren 2.0 - op basis van gebruiker-management-API's, Zie [SCIM gebruiken voor het automatisch inrichten van gebruikers en groepen uit Azure Active Directory met toepassingen](use-scim-to-provision-users-and-groups.md).

## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Hoe stel ik automatische inrichting tot een toepassing?

> [!VIDEO https://www.youtube.com/embed/pKzyts6kfrw]

Gebruikt de Azure Active Directory-portal voor het configureren van de Azure AD-inrichtingsservice voor een geselecteerde toepassing.

1. Open de  **[Azure Active Directory-portal](https://aad.portal.azure.com)** .
1. Selecteer **bedrijfstoepassingen** in het linkerdeelvenster. Een lijst met alle geconfigureerde apps wordt weergegeven.
1. Kies **+ nieuwe toepassing** een toepassing toevoegen. Voeg een van de volgende, afhankelijk van uw scenario:

   - De **toevoegen aan uw eigen app** optie biedt ondersteuning voor aangepaste SCIM integraties.
   - Alle toepassingen in de **toevoegen vanuit de galerie** > **aanbevolen toepassingen** sectie ondersteuning voor automatische inrichting. Zie de [lijst met zelfstudies voor toepassingen voor het inrichten van gebruikers](../saas-apps/tutorial-list.md) voor nieuwe zijn in.

1. Geef details op en selecteer **toevoegen**. De nieuwe app wordt toegevoegd aan de lijst van zakelijke toepassingen en wordt geopend op de application management-scherm.
1. Selecteer **Provisioning** voor het beheren van instellingen voor de app voor het inrichten gebruikersaccount.

   ![Geeft het scherm van de instellingen voor inrichten](./media/user-provisioning/provisioning_settings0.PNG)

1. Selecteer de optie automatisch voor de **inrichting modus** instellingen opgeven voor de Administrator-referenties, toewijzingen, starten en stoppen, en synchronisatie.

   - Vouw **beheerdersreferenties** in te voeren van de referenties die nodig zijn voor Azure AD verbinding maken met de API voor gebruikersbeheer van de toepassing. Deze sectie kunt u e-mailmeldingen inschakelen als de referenties mislukt of de inrichtingstaak krijgt ook [quarantaine](#quarantine).
   - Vouw **toewijzingen** bekijken en bewerken van de kenmerken van de gebruiker die tussen Azure AD stromen en de doeltoepassing wanneer gebruikersaccounts worden ingericht of bijgewerkt. Als de doeltoepassing wordt ondersteund, wordt in deze sectie kunt u eventueel configureren van groepen en accounts voor gebruikers wordt ingericht. Selecteer een toewijzing in de tabel om te openen van de editor voor kolomtoewijzing aan de rechterkant, waarin u kunt bekijken en aanpassen van de kenmerken van gebruiker.

     **Bereikfilters** de inrichtingsservice zien welke gebruikers en groepen in het bronsysteem moeten worden ingericht of de inrichting ongedaan gemaakt op het doelsysteem. In de **kenmerktoewijzing** venster **bereik van het bronobject** om te filteren op specifieke kenmerkwaarden. U kunt bijvoorbeeld opgeven dat alleen gebruikers waarvoor het kenmerk Afdeling is ingesteld op Verkoop moeten worden ingericht. Zie [Using scoping filters](define-conditional-rules-for-provisioning-user-accounts.md) (Bereikfilters gebruiken) voor meer informatie.

     Zie voor meer informatie, [kenmerktoewijzingen aanpassen](customize-application-attributes.md).

   - **Instellingen voor** bepalen de werking van de provisioning-service voor een toepassing, waaronder of de implementatie wordt uitgevoerd. De **bereik** menu kunt u opgeven of alleen toegewezen gebruikers en groepen moet binnen het bereik voor het inrichten of dat alle gebruikers in de Azure AD-directory moeten worden ingericht. Zie [Assign a user or group to an enterprise app in Azure Active Directory](assign-user-or-group-access-portal.md) (Een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory) voor informatie over het 'toewijzen' van gebruikers en groepen.

Selecteer in het scherm app management **auditlogboeken** om weer te geven records van elke bewerking uitgevoerd door de Azure AD-service inricht. Zie voor meer informatie de [inrichting handleiding over rapportering](check-status-user-account-provisioning.md).

![Voorbeeld: auditlogboeken beschikbaar zijn voor een app](./media/user-provisioning/audit_logs.PNG)

> [!NOTE]
> De inrichtingsservice Azure AD-gebruiker kan ook worden geconfigureerd en beheerd met behulp van de [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview).

## <a name="what-happens-during-provisioning"></a>Wat gebeurt er tijdens het inrichten?

Wanneer Azure AD het bronsysteem is, de provisioning-service gebruikt de [differentiële Query-functie van de Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query) om gebruikers en groepen te bewaken. De inrichtingsservice wordt een eerste synchronisatie uitgevoerd tegen het bronsysteem en doelsysteem, gevolgd door periodieke incrementele synchronisaties.

### <a name="initial-sync"></a>Initiële synchronisatie

Wanneer de provisioning-service wordt gestart, wordt de eerste synchronisatie ooit uitvoeren:

1. Query uitvoeren op alle gebruikers en groepen uit het bronsysteem bij het ophalen van alle kenmerken die zijn gedefinieerd de [kenmerktoewijzingen](customize-application-attributes.md).
1. Filteren van de gebruikers en groepen die zijn geretourneerd, met behulp van een geconfigureerd [toewijzingen](assign-user-or-group-access-portal.md) of [op kenmerken gebaseerde bereikfilters](define-conditional-rules-for-provisioning-user-accounts.md).
1. Wanneer een gebruiker is toegewezen of binnen het bereik voor het inrichten van de service zoekopdrachten uit op het doelsysteem voor een overeenkomende gebruiker met behulp van de opgegeven [die overeenkomt met de kenmerken](customize-application-attributes.md#understanding-attribute-mapping-properties). Voorbeeld: Als de naam van de userPrincipal in het bronsysteem het overeenkomende kenmerk is en toegewezen aan een gebruikersnaam in het doelsysteem en vervolgens de provisioning-service het doelsysteem voor gebruikersnamen die overeenkomen met de naam userPrincipal waarden in het bronsysteem query uitgevoerd.
1. Als er een overeenkomende gebruiker is niet gevonden in het doelsysteem, wordt deze gemaakt met behulp van de kenmerken die door het bronsysteem geretourneerd. Nadat het gebruikersaccount dat is gemaakt, wordt de provisioning-service detecteert en het doelsysteem-ID voor de nieuwe gebruiker, die wordt gebruikt voor het uitvoeren van alle toekomstige bewerkingen voor die gebruiker in de cache opslaat.
1. Als een overeenkomende gebruiker wordt gevonden, wordt deze bijgewerkt met behulp van de kenmerken die is geleverd door het bronsysteem. Nadat het gebruikersaccount dat is gekoppeld, wordt de provisioning-service detecteert en het doelsysteem-ID voor de nieuwe gebruiker, die wordt gebruikt voor het uitvoeren van alle toekomstige bewerkingen voor die gebruiker in de cache opslaat.
1. Als de kenmerktoewijzingen 'verwijzing' kenmerken bevatten, wordt de service extra updates op het doelsysteem te maken en koppelen van de objecten waarnaar wordt verwezen. Een gebruiker kan bijvoorbeeld een 'Manager'-kenmerk hebben in het doelsysteem, die is gekoppeld aan een andere gebruiker hebt gemaakt in het doelsysteem.
1. Een watermerk aan het einde van de initiële synchronisatie, waarmee het beginpunt voor de later incrementele synchronisaties behouden.

Sommige toepassingen, zoals ServiceNow, G Suite en Box-ondersteuning niet alleen inrichten van gebruikers, maar ook groepen en leden wordt ingericht. In deze gevallen, als groep inrichten is ingeschakeld de [toewijzingen](customize-application-attributes.md), wordt de inrichtingsservice synchroniseert de gebruikers en groepen en vervolgens het groepslidmaatschap later opnieuw te synchroniseren.

### <a name="incremental-syncs"></a>Incrementele synchronisaties

Na de initiële synchronisatie zullen alle andere wordt gesynchroniseerd:

1. Query uitvoeren op het bronsysteem voor alle gebruikers en groepen die zijn bijgewerkt sinds de laatste watermerk is opgeslagen.
1. Filteren van de gebruikers en groepen die zijn geretourneerd, met behulp van een geconfigureerd [toewijzingen](assign-user-or-group-access-portal.md) of [op kenmerken gebaseerde bereikfilters](define-conditional-rules-for-provisioning-user-accounts.md).
1. Wanneer een gebruiker is toegewezen of binnen het bereik voor het inrichten van de service zoekopdrachten uit op het doelsysteem voor een overeenkomende gebruiker met behulp van de opgegeven [die overeenkomt met de kenmerken](customize-application-attributes.md#understanding-attribute-mapping-properties).
1. Als er een overeenkomende gebruiker is niet gevonden in het doelsysteem, wordt deze gemaakt met behulp van de kenmerken die door het bronsysteem geretourneerd. Nadat het gebruikersaccount dat is gemaakt, wordt de provisioning-service detecteert en het doelsysteem-ID voor de nieuwe gebruiker, die wordt gebruikt voor het uitvoeren van alle toekomstige bewerkingen voor die gebruiker in de cache opslaat.
1. Als een overeenkomende gebruiker wordt gevonden, wordt deze bijgewerkt met behulp van de kenmerken die is geleverd door het bronsysteem. Als het een recent toegewezen account dat is gekoppeld, wordt de provisioning-service detecteert en het doelsysteem-ID voor de nieuwe gebruiker, die wordt gebruikt voor het uitvoeren van alle toekomstige bewerkingen voor die gebruiker in de cache opslaat.
1. Als de kenmerktoewijzingen 'verwijzing' kenmerken bevatten, wordt de service extra updates op het doelsysteem te maken en koppelen van de objecten waarnaar wordt verwezen. Een gebruiker kan bijvoorbeeld een 'Manager'-kenmerk hebben in het doelsysteem, die is gekoppeld aan een andere gebruiker hebt gemaakt in het doelsysteem.
1. Wanneer een gebruiker die eerder in het bereik vallen voor inrichting van bereik (inclusief wordt niet-toegewezen) wordt verwijderd, wordt de gebruiker uitgeschakeld in het doelsysteem via een update van de service.
1. Als een gebruiker die eerder in het bereik vallen voor inrichting is uitgeschakeld of voorlopig in het bronsysteem verwijderde, wordt de gebruiker uitgeschakeld in het doelsysteem via een update van de service.
1. Als een gebruiker die eerder in het bereik vallen voor inrichting verwijderd in het bronsysteem is, wordt de gebruiker door de service verwijderd in het doelsysteem. Gebruikers zijn in Azure AD-verwijderd 30 dagen nadat ze voorlopig verwijderde.
1. Een nieuw watermerk aan het einde van de incrementele synchronisatie, waarmee het beginpunt voor de later incrementele synchronisaties behouden.

> [!NOTE]
> U kunt desgewenst uitschakelen de **maken**, **Update**, of **verwijderen** bewerkingen met behulp van de **Objectacties als doel** selectievakjes in de [Toewijzingen](customize-application-attributes.md) sectie. De logica voor het uitschakelen van een gebruiker tijdens het bijwerken wordt beheerd via een kenmerk wordt toegewezen uit een veld, zoals 'accountEnabled'.

De inrichtingsservice wordt back to back incrementele synchronisaties voor onbepaalde tijd, uitgevoerd met intervallen gedefinieerd in de [zelfstudie die specifiek zijn voor elke toepassing](../saas-apps/tutorial-list.md), totdat een van de volgende gebeurtenissen zich voordoet:

- De service is handmatig gestopt met behulp van de Azure-portal of met behulp van de juiste Graph API-opdracht 
- Een nieuwe initiële synchronisatie wordt geactiveerd met behulp van de **status wissen en opnieuw starten** optie in Azure portal of met behulp van de juiste Graph API-opdracht. Met deze actie worden alle opgeslagen watermerk gewist en zorgt ervoor dat alle bronobjecten moet opnieuw worden geëvalueerd.
- Een nieuwe initiële synchronisatie wordt geactiveerd vanwege een wijziging in de kenmerktoewijzingen of bereikfilters toevoegen. Met deze actie wordt ook een opgeslagen watermerk wissen en zorgt ervoor dat alle bronobjecten moet opnieuw worden geëvalueerd.
- Het inrichtingsproces gaat in quarantaine (Zie hieronder) vanwege een hoog aantal fouten bij, en in quarantaine voor meer dan vier weken blijft. In dat geval kan wordt de service automatisch uitgeschakeld.

### <a name="errors-and-retries"></a>Fouten en nieuwe pogingen

Als een afzonderlijke gebruiker kan niet worden toegevoegd, bijgewerkt of verwijderd in het doelsysteem vanwege een fout in het doelsysteem, klikt u vervolgens de bewerking wordt opnieuw uitgevoerd in de volgende synchronisatiecyclus. Als de gebruiker mislukken blijft, begint de nieuwe pogingen worden uitgevoerd met een lagere frequentie, geleidelijk terug naar slechts één poging per dag te schalen. Los de fout op door beheerders moeten controleren de [auditlogboeken](check-status-user-account-provisioning.md) voor 'proces-escrow' gebeurtenissen om te bepalen van de hoofdmap veroorzaken en de juiste actie ondernemen. Veelvoorkomende fouten kunnen opnemen:

- Gebruikers niet met een kenmerk is ingevuld in het bronsysteem die is in het doelsysteem vereist
- Gebruikers met een waarde van het kenmerk in het bronsysteem waarvoor er een unieke beperking in het doelsysteem is en dezelfde waarde is aanwezig in de record van een andere gebruiker

Deze fouten kunnen worden opgelost door aan te passen van de kenmerkwaarden voor de betrokken gebruiker in het bronsysteem, of door de kenmerktoewijzingen als u wilt niet leiden tot conflicten aan te passen.

### <a name="quarantine"></a>In quarantaine plaatsen

Als de meeste of alle van de oproepen die aan het doelsysteem consistent mislukt vanwege een fout (zoals ongeldig beheerdersreferenties), klikt u vervolgens gaat de inrichtingstaak in een status 'in quarantaine plaatsen'. Deze status wordt aangegeven in de [inrichting overzichtsrapport](check-status-user-account-provisioning.md) en via e-mail als e-mailmeldingen zijn geconfigureerd in Azure portal.

Wanneer het in quarantaine, de frequentie van incrementele synchronisaties geleidelijk teruggebracht tot één keer per dag.

De inrichtingstaak worden uit quarantaine verwijderd nadat alle strijdige fouten zijn opgelost en de volgende synchronisatiecyclus start. Als de taak in quarantaine meer dan vier weken blijft, wordt de taak is uitgeschakeld.

## <a name="how-long-will-it-take-to-provision-users"></a>Hoe lang duurt het inrichten van gebruikers?

Prestaties, is afhankelijk van of uw provisioning-taak wordt uitgevoerd een cyclus van een eerste inrichting of een cyclus van een incrementele. Zie voor meer informatie over hoe lang de inrichting duurt en controleren van de status van de inrichtingsservice [Controleer de status van het inrichten van gebruikers](application-provisioning-when-will-provisioning-finish-specific-user.md).

## <a name="how-can-i-tell-if-users-are-being-provisioned-properly"></a>Hoe kan ik zien als gebruikers juist worden ingericht?

Alle bewerkingen uitgevoerd door de gebruiker inrichtingsservice worden vastgelegd in de Azure AD auditlogboeken. Dit omvat alle lees- en schrijfbewerkingen die zijn aangebracht in de bron en doel-systemen en de gebruikersgegevens die is gelezen of geschreven bij elke bewerking.

Zie voor meer informatie over het lezen van de controlelogboeken in Azure portal, de [inrichting handleiding over rapportering](check-status-user-account-provisioning.md).

## <a name="how-do-i-troubleshoot-issues-with-user-provisioning"></a>Hoe los ik problemen met het inrichten van gebruikers?

Zie voor instructies over het automatisch inrichten van gebruikers oplossen scenario's gebaseerde [problemen bij het configureren en inrichten van gebruikers voor een toepassing](application-provisioning-config-problem.md).

## <a name="what-are-the-best-practices-for-rolling-out-automatic-user-provisioning"></a>Wat zijn de aanbevolen procedures voor het implementeren van automatisch inrichten van gebruikers?

> [!VIDEO https://www.youtube.com/embed/MAy8s5WSe3A]

Zie voor een voorbeeld van stapsgewijze implementatie-plan voor het inrichten van uitgaande gebruikers naar een toepassing, de [identiteit Deployment Guide voor het inrichten van gebruikers](https://aka.ms/userprovisioningdeploymentplan).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-b2b-users-in-azure-ad"></a>Wordt er automatisch gebruikers inrichten voor SaaS-apps werken met B2B-gebruikers in Azure AD?

Ja, is het mogelijk om te gebruiken van de Azure AD-gebruiker inrichten van gebruikers van B2B inrichten (of Gast)-service in Azure AD SaaS-toepassingen.

De SaaS-toepassing moet echter voor B2B-gebruikers zich aanmeldt bij de SaaS-toepassing met behulp van Azure AD, hebben de SAML gebaseerde eenmalige aanmeldings-mogelijkheid geconfigureerd op een specifieke manier. Zie voor meer informatie over het configureren van SaaS-toepassingen voor de ondersteuning van aanmeldingen van gebruikers van B2B [configureren SaaS-apps voor B2B-samenwerking]( https://docs.microsoft.com/azure/active-directory/b2b/configure-saas-apps).

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-dynamic-groups-in-azure-ad"></a>Wordt er automatisch gebruikers inrichten voor SaaS-apps werken met dynamische groepen in Azure AD?

Ja. Wanneer geconfigureerd voor 'sync alleen toegewezen gebruikers en groepen', de Azure AD-gebruiker inrichtingsservice kunt inrichten of ongedaan maken inrichting gebruikers in een SaaS-toepassing is afhankelijk van of ze lid zijn van een [dynamische groep](../users-groups-roles/groups-create-rule.md). Dynamische groepen werken ook met de optie 'alle gebruikers en groepen synchroniseren'.

Gebruik van dynamische groepen kan echter van invloed op de algehele prestaties van end-to-end gebruikersinrichting van de Azure AD met SaaS-toepassingen. Wanneer u dynamische groepen, Let op deze aanvullende opmerkingen en aanbevelingen in gedachten:

- Hoe snel een gebruiker in een dynamische groep is ingericht of de inrichting ongedaan gemaakt in een SaaS-toepassing afhankelijk van hoe snel de dynamische groep wijzigingen in het lidmaatschap kunt evalueren. Zie voor meer informatie over het controleren van de verwerkingsstatus van een dynamische groep [Controleer de verwerkingsstatus voor een lidmaatschapsregel](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule).

- Wanneer u dynamische groepen, moeten de voorschriften zorgvuldig worden nagedacht met gebruiker inrichting en ongedaan maken inrichting in gedachten, als een verlies van resultaten voor lidmaatschap in een opheffen van inrichtingen gebeurtenis.

### <a name="does-automatic-user-provisioning-to-saas-apps-work-with-nested-groups-in-azure-ad"></a>Wordt er automatisch gebruikers inrichten voor SaaS-apps werken met geneste groepen in Azure AD?

Nee. Wanneer geconfigureerd voor 'sync alleen toegewezen gebruikers en groepen', is de Azure AD-gebruiker inrichtingsservice niet kunnen lezen of het inrichten van gebruikers die zich in de geneste groepen. Het is alleen kan lezen en inrichten van gebruikers die directe leden van de expliciet toegewezen groep.

Dit is een beperking van '-groep op basis van toewijzingen voor toepassingen', die ook van invloed is op eenmalige aanmelding en wordt beschreven in [met behulp van een groep voor het beheren van toegang tot SaaS-toepassingen](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-saasapps ).

Als tijdelijke oplossing, moet u expliciet toewijzen (of anderszins [in het bereik van](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)) de groepen met de gebruikers die moeten worden ingericht.

### <a name="is-provisioning-between-azure-ad-and-a-target-application-using-an-encrypted-channel"></a>Tussen Azure AD wordt ingericht en een doeltoepassing met behulp van een versleuteld kanaal?

Ja. We gebruiken HTTPS SSL-versleuteling voor de doel-server.

## <a name="related-articles"></a>Verwante artikelen:

- [Lijst met zelfstudies over het integreren van SaaS-apps](../saas-apps/tutorial-list.md)
- [Kenmerktoewijzingen voor het inrichten van gebruikers aan te passen](customize-application-attributes.md)
- [Expressies schrijven voor kenmerktoewijzingen](functions-for-customizing-application-data.md)
- [Bereikfilters toevoegen voor het inrichten van gebruikers](define-conditional-rules-for-provisioning-user-accounts.md)
- [Using SCIM voor automatische inrichting van gebruikers en groepen uit Azure AD met toepassingen inschakelen](use-scim-to-provision-users-and-groups.md)
- [Overzicht van Azure AD-synchronisatie API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)
