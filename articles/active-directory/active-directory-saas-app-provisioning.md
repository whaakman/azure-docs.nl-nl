---
title: Automatische SaaS app gebruikers inrichten in Azure AD | Microsoft Docs
description: Een inleiding tot hoe u Azure AD kunt gebruiken voor het automatisch inrichten, inrichten ongedaan, en werk continu gebruikersaccounts voor meerdere derde SaaS-toepassingen.
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: mtillman
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2017
ms.author: asmalser
ms.openlocfilehash: cd82ef109abbc5707db4c02c2f14f9d55dfb74e3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
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
* Aanpassingsopties Azure AD-Help aanpassen aan de huidige configuraties van de apps en systemen die momenteel wordt gebruikt door uw organisatie.
* Optionele e-mailwaarschuwingen voor fouten bij het inrichten.
* Rapportage- en activiteit Logboeken om te helpen bij de bewaking en probleemoplossing.

## <a name="why-use-automated-provisioning"></a>Waarom een geautomatiseerde inrichting gebruiken?
Sommige algemene motivaties voor het gebruik van deze functie zijn onder andere:

* Om te voorkomen dat de kosten, inefficiënte en menselijke fouten die zijn gekoppeld aan de handmatige Inrichtingsmethode processen.
* Om te voorkomen dat de kosten in verband met de host en onderhouden van ontwikkelde aangepaste oplossingen voor inrichting en scripts
* Beveiligen van uw organisatie met de identiteit van gebruikers direct verwijderen uit sleutel SaaS-apps wanneer ze de organisatie verlaat.
* Eenvoudig een bulksgewijs aantal gebruikers in een bepaalde SaaS-toepassing of het systeem importeren.
* Om te profiteren van het gemak van uw oplossing uitvoeren op de dezelfde app-beleidsregels die u hebt gedefinieerd voor Azure AD eenmalige aanmelding.


## <a name="how-does-automatic-provisioning-work"></a>Hoe automatische inrichting werkt?
    
De **Azure AD-inrichtingsservice** gebruikers SaaS-apps en andere systemen, bepalingen door verbinding te maken met beheer-API-eindpunten gebruiker geleverd door de leverancier van elke toepassing. Deze gebruiker management API-eindpunten kunnen Azure AD via een programma maken, bijwerken en verwijderen van gebruikers. Voor de geselecteerde toepassingen die kunt ook de inrichting service maken, bijwerken en verwijderen van aanvullende identity-gerelateerde objecten, zoals rollen en groepen. 

![Inrichting](./media/active-directory-saas-app-provisioning/provisioning0.PNG)
*afbeelding 1: de Azure AD-Service inricht*

![Uitgaande inrichten](./media/active-directory-saas-app-provisioning/provisioning1.PNG)
*afbeelding 2: 'Uitgaande' gebruikers inrichten van de werkstroom van Azure AD voor populaire SaaS-toepassingen*

![Inkomende inrichten](./media/active-directory-saas-app-provisioning/provisioning2.PNG)
*afbeelding 3: 'Inkomend' gebruiker inrichten werkstroom van populaire menselijke kapitaal Management (HCM)-toepassingen voor Azure Active Directory en Windows Server Active Directory*


## <a name="what-applications-and-systems-can-i-use-with-azure-ad-automatic-user-provisioning"></a>Welke toepassingen en systemen kan ik met Azure AD automatisch gebruikers inrichten gebruiken?

Azure AD-functies vooraf geïntegreerde ondersteuning voor een groot aantal populaire SaaS-apps en human resources-systemen, evenals algemene ondersteuning voor apps met specifieke onderdelen van de [SCIM 2.0 standaard](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning).

Alle apps 'Aanbevolen' in de Azure AD-toepassingsgalerie ondersteuning voor geautomatiseerde gebruikersinrichting. [De lijst met aanbevolen apps kan hier worden weergegeven.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

In de volgorde voor een toepassing voor de ondersteuning van geautomatiseerde gebruikers inrichten, moet deze eerst bieden de benodigde gebruiker eindpunten voor beheer voor het externe programma's voor het automatiseren van het maken, het onderhoud en de verwijdering van gebruikers. Er zijn daarom niet alle SaaS-apps compatibel met deze functie. Voor apps die ondersteuning voor gebruikersbeheer API's bieden, het technische team van Azure AD zijn vervolgens een inrichting connector voor deze apps bouwen en deze taak is geplaatst door de vereisten van de huidige en potentiële klanten. 

Neem contact op met de Azure AD technisch team om aan te vragen van de inrichting ondersteuning voor aanvullende toepassingen indienen om een bericht via de [forum met feedback van Azure Active Directory](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning). 
    
    
## <a name="how-do-i-set-up-automatic-provisioning-to-an-application"></a>Hoe stel ik automatische inrichting tot een toepassing?

Configuratie van de Azure AD-service inricht voor een geselecteerde toepassing wordt gestart in de  **[Azure-portal](https://potal.azure.com)**. In de **Azure Active Directory > bedrijfstoepassingen** sectie **toevoegen**, klikt u vervolgens **alle**, en voeg vervolgens een van de volgende, afhankelijk van uw scenario:

* Alle toepassingen in de **toepassingen aanbevolen** sectie ondersteuning automatische inrichting

* De optie 'niet galerie application' gebruiken voor aangepaste ontwikkelde SCIM integraties

![Gallery](./media/active-directory-saas-app-provisioning/gallery.png)

Klik in het scherm application management-inrichting is geconfigureerd in de **inrichten** tabblad.

![Instellingen](./media/active-directory-saas-app-provisioning/provisioning_settings0.PNG)


* **Beheerdersreferenties** moet worden opgegeven om de Azure AD-service waarmee deze verbinding maken met de API door de toepassing geleverd voor het Gebruikersbeheer inricht.

* **Kenmerktoewijzingen** kunnen worden geconfigureerd die opgeven welke velden in het bronsysteem (voorbeeld: Azure AD) wordt de inhoud ervan met hebben gesynchroniseerd welke velden in het doelsysteem (voorbeeld: ServiceNow). Wanneer de doeltoepassing wordt ondersteund kunt in deze sectie u eventueel configureren inrichting van groepen naast gebruikersaccounts. 'Overeenkomende eigenschappen' kunt u selecteren welke velden worden gebruikt voor accounts tussen de systemen overeenkomen. "[Expressies](active-directory-saas-writing-expressions-for-attribute-mappings.md)' kunt u wijzigen en transformatie van de waarden die zijn opgehaald uit het bronsysteem voordat ze op het doelsysteem worden geschreven. Zie voor meer informatie [kenmerktoewijzingen aanpassen](active-directory-saas-customizing-attribute-mappings.md).

![Instellingen](./media/active-directory-saas-app-provisioning/provisioning_settings1.PNG)

* **Bereikfilters** de inrichting service zien welke gebruikers en groep in het bronsysteem moet worden ingericht en/of gemaakt met het doelsysteem. Er zijn twee aspecten bereikfilters die gezamenlijk worden geëvalueerd om te bepalen wie in het bereik voor het inrichten is:

* **Filteren op kenmerkwaarden** -het menu 'Objectbereik bron' in de kenmerktoewijzingen Hiermee kunt u filteren op specifieke kenmerkwaarden. U kunt bijvoorbeeld opgeven dat alleen gebruikers met een kenmerk 'Afdeling' van 'Verkoop' binnen het bereik van de inrichting moet.

* **Filter op toewijzingen** -het menu 'Bereik' in de inrichting > sectie van de instellingen van de portal kunt u opgeven of alleen 'toegewezen' gebruikers en groepen moeten zich binnen het bereik van de inrichting, of als alle gebruikers in de Azure AD-directory moet ingericht. Zie voor informatie over ' ' gebruikers en groepen toewijzen, [een gebruiker of groep toewijzen aan een enterprise-app in Azure Active Directory](active-directory-coreapps-assign-user-azure-portal.md).
    
* **Instellingen** bepalen de werking van de inrichting-service voor een toepassing, met inbegrip van of deze nu wordt uitgevoerd of niet.

* **Controlelogboeken** bieden records van elke bewerking uitgevoerd door de Azure AD-service inricht. Zie voor meer informatie de [inrichting rapportagegids](active-directory-saas-provisioning-reporting.md).

![Instellingen](./media/active-directory-saas-app-provisioning/audit_logs.PNG)

## <a name="what-happens-during-provisioning"></a>Wat gebeurt er tijdens het inrichten?

1. Wanneer u inrichting voor een toepassing voor het eerst inschakelt, worden de volgende acties uitgevoerd:
   * Azure AD probeert te overeen met alle bestaande gebruikers in de SaaS-app met hun overeenkomstige identiteiten in de map. Wanneer een gebruiker is gekoppeld, zijn ze *niet* automatisch ingeschakeld voor eenmalige aanmelding. Om een gebruiker toegang heeft tot de toepassing, moeten ze worden expliciet toegewezen aan de app in Azure AD, rechtstreeks of via het lidmaatschap van.
   * Als u al hebt opgegeven welke gebruikers de toepassing moeten worden toegewezen, en als Azure AD niet vinden van bestaande accounts voor gebruikers, Azure AD wordt geen nieuwe accounts inrichten voor deze in de toepassing.
2. Nadat de initiële synchronisatie is voltooid zoals hierboven beschreven, wordt elke 20 minuten voor de volgende wijzigingen controleren in Azure AD:
   * Als u nieuwe gebruikers zijn toegewezen aan de toepassing (rechtstreeks of via een groepslidmaatschap), vervolgens deze zijn ingericht met een nieuwe account in de SaaS-app.
   * Als een gebruiker toegang is verwijderd, wordt hun account in de SaaS-app is gemarkeerd als uitgeschakeld (gebruikers nooit volledig verwijderd, die u beveiligt tegen gegevensverlies in geval van een onjuiste configuratie).
   * Als een gebruiker onlangs is toegewezen aan de toepassing en ze al een account in de SaaS-app, dat account is gemarkeerd als ingeschakeld en bepaalde gebruikerseigenschappen kunnen worden bijgewerkt als ze verouderd in vergelijking met de map zijn.
   * Als de informatie van een gebruiker (zoals telefoonnummer, kantoorlocatie) is gewijzigd in de directory, wordt er ook die informatie in de SaaS-toepassing worden bijgewerkt.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen
**Hoe vaak Azure AD directorywijzigingen schrijven naar de SaaS-app**

Nadat een initiële volledige synchronisatie is voltooid, de Azure AD-service inricht doorgaans wijzigingen gecontroleerd om de 20 minuten. 

Als de SaaS-app retourneert verschillende fouten (zoals in het geval van ongeldige beheerdersreferenties), klikt u vervolgens in Azure AD geleidelijk de frequentie maximaal eenmaal per dag totdat de fouten zijn opgelost vertragen. Als dit gebeurt, de inrichtingstaak van de Azure AD gaat in een status 'quarantaine', en wordt aangegeven dat dit in de [overzichtsrapport inrichting](active-directory-saas-provisioning-reporting.md).

**Hoe lang duurt het inrichten van mijn gebruikers?**

Nadat een initiële volledige synchronisatie is voltooid, gebeuren incrementele wijzigingen doorgaans binnen 20-40 minuten. Als u probeert de meeste van uw directory inrichten, klikt u vervolgens het afhankelijk is van het aantal gebruikers en groepen die u hebt. De prestaties zijn afhankelijk van de prestaties van de gebruiker management API's waarmee de inrichting services worden gegevens lezen uit het bronsysteem en de gegevens schrijven naar het doelsysteem. 

Is ook trager als er veel fouten (opgenomen in de [controlelogboeken](active-directory-saas-provisioning-reporting.md)) en de inrichting-service is geworden in een status 'quarantaine'.

**Wat is er een eerste volledige synchronisatie en waarom duurt langer dan de volgende synchronisaties?**

De eerste keer wordt uitgevoerd met de Azure AD-service inricht voor een bepaalde app deze een ' momentopname ' van de gebruikers (en desgewenst groepen) in de bronmap. Deze momentopname kan de inrichting service Verminder het aantal retouren naar de bron en doel-API's en volgende 'delta' synchronisaties zich gedragen efficiënter staat. 

De eerste volledige synchronisatie van gebruikers vaak in minuten voor zeer kleine mappen kan worden voltooid, maar het kan enkele uren voor grotere mappen duren. Mappen van de onderneming met honderden of duizenden gebruikers duurt vele uren voor de initiële synchronisatie te voltooien. Na de initiële synchronisatie daaropvolgende 'delta' synchronisaties gebeurt echter veel sneller.

> [!NOTE]
> Voor toepassingen die ondersteuning bieden voor inrichting van groepen en groepslidmaatschappen, het inschakelen van dit aanzienlijk verhoogt de tijd die nodig zijn voor een volledige synchronisatie te voltooien. Als u niet inrichten namen van groepen en groepslidmaatschappen voor uw toepassing wilt, kunt u uitschakelen in de [kenmerktoewijzingen](active-directory-saas-customizing-attribute-mappings.md) van de inrichting van de configuratie.

**Hoe kan ik de voortgang van de inrichtingstaak van de huidige bijhouden**

Zie de [inrichting rapportagegids](active-directory-saas-provisioning-reporting.md).

**Hoe weet ik als gebruikers geen goed ophalen ingericht?**

Alle fouten worden vastgelegd in de Azure AD controlelogboeken. Zie voor meer informatie de [inrichting rapportagegids](active-directory-saas-provisioning-reporting.md).

**Hoe kan ik feedback aan het technische team?**

Contact met ons opnemen via de [forum met feedback van Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).


## <a name="related-articles"></a>Verwante artikelen:
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Kenmerktoewijzingen voor gebruikers inrichten aanpassen](active-directory-saas-customizing-attribute-mappings.md)
* [Expressies voor kenmerktoewijzingen schrijven](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereikfilters voor gebruikers inrichten](active-directory-saas-scoping-filters.md)
* [Using SCIM to enable automatic provisioning of users and groups from Azure Active Directory to applications](active-directory-scim-provisioning.md) (SCIM gebruiken om in te stellen dat gebruikers en groepen van Azure Active Directory automatisch worden ingericht voor toepassingen)
* [Meldingen inrichten van een account](active-directory-saas-account-provisioning-notifications.md)
* [Lijst met zelfstudies over het integreren van SaaS-Apps](active-directory-saas-tutorial-list.md)

