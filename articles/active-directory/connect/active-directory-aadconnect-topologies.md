---
title: "Azure AD Connect: Ondersteunde topologieën | Microsoft Docs"
description: "In dit onderwerp beschrijft de ondersteunde en niet-ondersteunde topologieën voor Azure AD Connect"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 50cf58c7d2d9be4644ada4feae02d0d5219a3fd6
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="topologies-for-azure-ad-connect"></a>Topologieën voor Azure AD Connect
In dit artikel beschrijft de verschillende on-premises en Azure Active Directory (Azure AD)-topologieën die Azure AD Connect-synchronisatie als de sleutel integratieoplossing gebruiken. Dit artikel bevat de ondersteunde en niet-ondersteunde configuraties.

Hier volgt de legenda voor afbeeldingen in het artikel:

| Beschrijving | Symbool |
| --- | --- |
| On-premises Active Directory-forest |![On-premises Active Directory-forest](./media/active-directory-aadconnect-topologies/LegendAD1.png) |
| On-premises Active Directory met gefilterde importeren |![Active Directory met gefilterde importeren](./media/active-directory-aadconnect-topologies/LegendAD2.png) |
| Azure AD Connect sync-server |![Azure AD Connect sync-server](./media/active-directory-aadconnect-topologies/LegendSync1.png) |
| Azure AD Connect sync-server 'fasering modus' |![Azure AD Connect sync-server 'fasering modus'](./media/active-directory-aadconnect-topologies/LegendSync2.png) |
| GALSync met Forefront Identity Manager (FIM) 2010 of Microsoft Identity Manager (MIM) 2016 |![GALSync met FIM 2010 of MIM 2016](./media/active-directory-aadconnect-topologies/LegendSync3.png) |
| Azure AD Connect sync-server, gedetailleerde |![Azure AD Connect sync-server, gedetailleerde](./media/active-directory-aadconnect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/active-directory-aadconnect-topologies/LegendAAD.png) |
| Niet-ondersteund scenario |![Niet-ondersteund scenario](./media/active-directory-aadconnect-topologies/LegendUnsupported.png) |

## <a name="single-forest-single-azure-ad-tenant"></a>Eén forest, één Azure AD-tenant
![Topologie voor één forest en een enkele tenant](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

De meest voorkomende topologie is één lokale forest, met een of meerdere domeinen en een enkele Azure AD-tenant. Wachtwoordsynchronisatie is voor Azure AD-verificatie gebruikt. De snelle installatie van Azure AD Connect ondersteunt alleen deze topologie.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Eén forest, meerdere synchronisatieservers in een Azure AD-tenant
![Niet-ondersteunde, gefilterde topologie voor één forest](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Meerdere servers met Azure AD Connect-synchronisatie is verbonden met dezelfde Azure AD-tenant wordt niet ondersteund, met uitzondering van een [testserver](#staging-server). Het bevat niet-ondersteunde zelfs als deze servers zijn geconfigureerd om te synchroniseren met een sluiten elkaar wederzijds uit set van objecten. U kunt hebben beschouwd als deze topologie als u alle domeinen in het forest met een enkele server niet kan bereiken of als u wilt verdelen over meerdere servers.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Meerdere forests, enkel Azure AD-tenant
![Topologie voor meerdere forests en een enkele tenant](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Veel organisaties hebben omgevingen met meerdere lokale Active Directory-forests. Er zijn diverse redenen voor het hebben van meer dan een lokale Active Directory-forest. Typische voorbeelden zijn ontwerpen met account-bron-forests en het resultaat van een fusie of overname.

Wanneer u meerdere forests, alle forests hebt moet bereikbaar is op één server van Azure AD Connect-synchronisatie. U moet niet de server toevoegen aan een domein. Indien nodig alle forests bereiken, kunt u de server plaatsen in een perimeternetwerk (ook wel DMZ, gedemilitariseerde zone en gescreend subnet).

De Azure AD Connect-installatiewizard biedt verschillende opties om te consolideren van gebruikers die worden weergegeven in meerdere forests. Het doel is dat een gebruiker slechts één keer wordt weergegeven in Azure AD. Er zijn een aantal algemene topologieën die u in het pad van de aangepaste installatie in de installatiewizard configureren kunt. Op de **een unieke id van uw gebruikers** pagina, selecteert u de corresponderende optie waarmee uw topologie. De consolidatie is alleen geconfigureerd voor gebruikers. Dubbele groepen zijn niet samengevoegd met de standaardconfiguratie.

Algemene topologieën worden beschreven in de secties over [scheiden topologieën](#multiple-forests-separate-topologies), [full mesh](#multiple-forests-full-mesh-with-optional-galsync), en [de topologie van de account-resource](#multiple-forests-account-resource-forest).

De standaardconfiguratie in Azure AD Connect-synchronisatie wordt ervan uitgegaan dat:

* Elke gebruiker heeft slechts één ingeschakeld-account en het forest waar dit account zich bevindt, wordt gebruikt voor verificatie van de gebruiker. Deze aanname is voor zowel Wachtwoordsynchronisatie en Federatie. UserPrincipalName en sourceAnchor/onveranderbare id genoemd, is afkomstig van dit forest.
* Elke gebruiker heeft slechts één postvak.
* Het forest dat als host fungeert voor het postvak voor een gebruiker heeft de beste kwaliteit van de gegevens voor kenmerken zichtbaar in de Exchange globale adreslijst (GAL). Als er geen postvak voor de gebruiker, kan elk forest worden gebruikt om bij te dragen deze kenmerkwaarden.
* Als u een gekoppeld postvak hebt, is er ook een account in een ander forest dat is gebruikt voor aanmelden.

Als uw omgeving komt niet overeen met deze veronderstellingen, gebeuren de volgende dingen:

* Als u meer dan één actieve account of meer dan één postvak hebt, wordt de synchronisatie-engine haalt een en de andere worden genegeerd.
* Een gekoppeld postvak met geen andere actieve account is niet geëxporteerd naar Azure AD. Het gebruikersaccount wordt niet weergegeven als een lid in een groep. Een gekoppeld postvak in DirSync wordt altijd weergegeven als een normale postvak. Deze wijziging is opzettelijk een verschillend gedrag voor betere ondersteuning van meerdere forestscenario's.

U vindt meer informatie in [inzicht in de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Meerdere forests, meerdere synchronisatieservers in een Azure AD-tenant
![Niet-ondersteunde topologie voor meerdere forests en meerdere synchronisatieservers](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Er meer dan één Azure AD Connect sync-server is verbonden met één Azure AD-tenant wordt niet ondersteund. De uitzondering is het gebruik van een [testserver](#staging-server).

### <a name="multiple-forests-separate-topologies"></a>Afzonderlijke topologieën met meerdere forests
![Optie voor het voorstellen van gebruikers slechts één keer op alle mappen](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Weergave van meerdere forests en afzonderlijke topologieën](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

In deze omgeving worden alle on-premises-forests behandeld als afzonderlijke entiteiten. Er is geen gebruiker is aanwezig in een andere forest. Elk forest heeft een eigen Exchange-organisatie en er is geen GALSync tussen de forests. Deze topologie wordt mogelijk de situatie na een fusie/aanschaf of in een organisatie waar elk bedrijfsonderdeel onafhankelijk optreedt. Deze forests zijn binnen dezelfde organisatie in Azure AD en worden weergegeven met een geïntegreerde GAL. In de voorgaande afbeelding wordt elk object in elk forest één keer weergegeven in de metaverse en samengevoegd in de doel-Azure AD-tenant.

### <a name="multiple-forests-match-users"></a>Meerdere forests: overeenkomst met gebruikers
Voor deze scenario's is dat distributie en beveiligingsgroepen kunnen een combinatie van gebruikers, contactpersonen en afwijkende beveiligings-Principals (FSP's) bevatten. FSP's worden gebruikt in Active Directory Domain Services (AD DS) om leden uit andere forests in een beveiligingsgroep vertegenwoordigen. Alle FSP's worden omgezet in het juiste object in Azure AD.

### <a name="multiple-forests-full-mesh-with-optional-galsync"></a>Meerdere forests: full mesh met optionele GALSync
![Optie voor het gebruik van het e-mailkenmerk voor overeenkomende wanneer gebruikers-id's aanwezig zijn op meerdere mappen](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Full mesh-topologie voor meerdere forests](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Full mesh-topologie kan gebruikers en bronnen zich bevinden in een forest. Er zijn doorgaans vertrouwensrelaties in twee richtingen tussen de forests.

Als Exchange in meer dan één forest aanwezig is, kan er (optioneel) een GALSync on-premises oplossing. Elke gebruiker wordt vervolgens weergegeven als een contactpersoon in alle andere forests. GALSync wordt meestal geïmplementeerd via de FIM 2010 of MIM 2016. Azure AD Connect kan niet worden gebruikt voor lokale GALSync.

In dit scenario identiteitsobjecten die via het e-mailkenmerk lid zijn. Een gebruiker met een postvak in één forest wordt samengevoegd met de contactpersonen in de andere forests.

### <a name="multiple-forests-account-resource-forest"></a>Meerdere forests: account-bron-forest
![Optie voor het gebruik van de kenmerken ObjectSID en msExchMasterAccountSID voor overeenkomende wanneer bestaan-id's op meerdere mappen](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Account-resource foresttopologie voor meerdere forests](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

In een topologie met account-resource forest, hebt u een of meer *account* forests met actieve gebruikersaccounts. U hebt ook een of meer *resource* forests met uitgeschakelde accounts.

In dit scenario wordt forestvertrouwensrelaties een (of meer) resource alle accountforests. De bron-forest is doorgaans een uitgebreid Active Directory-schema in Exchange en Lync. Alle Exchange en Lync services, samen met andere gedeelde services, bevinden zich in dit forest. Gebruikers hebben een uitgeschakelde gebruikersaccount in dit forest en het postvak is gekoppeld aan het forest.

## <a name="office-365-and-topology-considerations"></a>Office 365 en aandachtspunten voor topologie
Sommige Office 365-werkbelastingen hebben bepaalde beperkingen op ondersteunde topologieën:

| Workload | Beperkingen |
--------- | ---------
| Exchange Online | Zie voor meer informatie over hybride topologieën ondersteund door Exchange Online, [hybride implementaties met meerdere Active Directory-forests](https://technet.microsoft.com/library/jj873754.aspx). |
| Skype voor Bedrijven | Wanneer u meerdere on-premises-forests, worden alleen de account-resource forest-topologie wordt ondersteund. Zie voor meer informatie [omgevingsvereisten voor Skype voor bedrijven Server 2015](https://technet.microsoft.com/library/dn933910.aspx). |


## <a name="staging-server"></a>Tijdelijke server
![Tijdelijke server in een topologie](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect ondersteunt het installeren van een tweede server in *faseringsmodus*. Een server in deze modus leest de gegevens uit alle gekoppelde mappen, maar niet schrijven naar verbonden mappen. De normale synchronisatiecyclus wordt gebruikt en daarom is een bijgewerkte kopie van de identiteitsgegevens.

In een ramp optreedt waarbij de primaire server mislukt, kunt u failover naar de staging-server. Dit doet u in de Azure AD Connect-wizard. Deze tweede server kan zich bevinden in een ander datacenter, omdat er geen infrastructuur wordt gedeeld met de primaire server. Elke configuratiewijziging in op de primaire server naar de tweede server, moet u handmatig kopiëren.

U kunt een testserver gebruiken om een nieuwe aangepaste configuratie en het effect dat er op uw gegevens te testen. U kunt de wijzigingen bekijken en aanpassen van de configuratie. Wanneer u tevreden met de nieuwe configuratie bent, kunt u de active server van de staging-server maken en de oude active server instellen op faseringsmodus.

U kunt deze methode ook gebruiken ter vervanging van de ActiveSync-server. De nieuwe server voorbereiden en stel deze in op de faseringsmodus. Zorg ervoor dat deze zich in goede staat verkeren, faseringsmodus (waardoor dit active), uitschakelen en de momenteel actieve server afgesloten.

Het is mogelijk meer dan één server met tijdelijke bestanden hebben wanneer u wilt meerdere back-ups in verschillende datacenters.

## <a name="multiple-azure-ad-tenants"></a>Meerdere Azure AD-tenants
Het is raadzaam dat een één tenant in Azure AD voor een organisatie.
Voordat u van plan bent te gebruiken van meerdere Azure AD-tenants, Zie het artikel [beheer van beheereenheden in Azure AD](../active-directory-administrative-units-management.md). Deze heeft algemene scenario's waarin u een één tenant kunt.

![Topologie voor meerdere forests en meerdere tenants](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Er is een 1:1-relatie tussen een Azure AD Connect sync-server en een Azure AD-tenant. Voor elke Azure AD-tenant moet u één serverinstallatie van de Azure AD Connect-synchronisatie. De exemplaren van de Azure AD-tenant zijn geïsoleerd standaard. Gebruikers in een tenant niet dat wil zeggen, de gebruikers in de andere tenant zien. Als u wilt dat deze scheiding, is dit een ondersteunde configuratie. Anders moet u de één model van Azure AD-tenant.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Elk object slechts eenmaal in een Azure AD-tenant
![Gefilterde topologie voor één forest](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

In deze topologie wordt is een Azure AD Connect-synchronisatie-server verbonden met elke Azure AD-tenant. De Azure AD Connect sync-servers moeten worden geconfigureerd voor het filteren zodat elk een sluiten elkaar wederzijds uit set van objecten heeft te bewerken. U kunt bijvoorbeeld het bereik van elke server aan een bepaald domein of organisatie-eenheid.

Een DNS-domein kan worden geregistreerd in slechts één Azure AD-tenant. De UPN van de gebruikers in de lokale Active Directory-exemplaar moeten ook afzonderlijke naamruimten gebruiken. Bijvoorbeeld drie afzonderlijke UPN-achtervoegsels in de voorgaande afbeelding zijn geregistreerd in de lokale Active Directory-exemplaar: contoso.com en fabrikam.com wingtiptoys.com. De gebruikers in elk lokale Active Directory-domein gebruiken een andere naamruimte.

Er is geen GALSync tussen de exemplaren van de Azure AD-tenant. Het adresboek in Exchange Online en Skype voor bedrijven toont alleen gebruikers binnen dezelfde tenant.

Deze topologie heeft de volgende beperkingen op andere wijze ondersteund scenario's:

* Slechts één van de Azure AD-tenants, kunt een hybride Exchange met de lokale Active Directory-exemplaar inschakelen.
* Windows 10-apparaten kunnen worden gekoppeld aan slechts één Azure AD-tenant.
* De eenmalige aanmelding (SSO) optie voor wachtwoord synchronisatie en Pass through-verificatie kan worden gebruikt met slechts één Azure AD-tenant.

De vereiste voor een sluiten elkaar wederzijds uit set van objecten geldt ook voor write-back van. Sommige Write-back-functies worden niet ondersteund met deze topologie omdat ze wordt ervan uitgegaan een configuratie met één on-premises dat. Deze functies:

* Groep terugschrijven met standaardconfiguratie.
* Write-back van apparaat.

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Elk object meerdere keren in een Azure AD-tenant
![Niet-ondersteunde topologie voor één forest en meerdere tenants](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Niet-ondersteunde topologie voor één forest en meerdere connectors](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

Deze taken worden niet ondersteund:

* Dezelfde gebruiker aan meerdere tenants van Azure AD synchroniseren.
* Maak een configuratie wijzigen, zodat gebruikers in een Azure AD-tenant worden weergegeven als contactpersonen in een andere Azure AD-tenant.
* Azure AD Connect-synchronisatie verbinding maken met meerdere Azure AD-tenants wijzigen.

### <a name="galsync-by-using-writeback"></a>GALSync via Write-back
![Niet-ondersteunde topologie voor meerdere forests en meerdere mappen, met GALSync gericht op Azure AD](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Niet-ondersteunde topologie voor meerdere forests en meerdere mappen, met GALSync te focussen op lokale Active Directory](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD-tenants worden geïsoleerd met opzet. Deze taken worden niet ondersteund:

* De configuratie van Azure AD Connect-synchronisatie gegevens lezen uit een andere Azure AD-tenant wijzigen.
* Gebruikers exporteren als contactpersonen naar een ander exemplaar van de lokale Active Directory via Azure AD Connect-synchronisatie.

### <a name="galsync-with-on-premises-sync-server"></a>GALSync met lokale synchronisatieserver
![GALSync in een topologie voor meerdere forests en meerdere mappen](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

FIM 2010 of MIM 2016 lokale kunt u gebruikers (via GALSync) synchroniseren tussen twee organisaties met Exchange. De gebruikers in een organisatie weergegeven als externe gebruikers/contactpersonen in de andere organisatie. Deze verschillende on-premises Active Directory exemplaren kunnen vervolgens worden gesynchroniseerd met hun eigen Azure AD-tenants.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het installeren van Azure AD Connect voor deze scenario's, [aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).
