---
title: 'Azure AD Connect: Ondersteunde topologieën | Microsoft Docs'
description: In dit onderwerp wordt uitgelegd ondersteunde en niet-ondersteunde topologieën voor Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: article
ms.date: 02/27/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: bab18c5c36cf4ffbbe6268910bf9121bc351fdca
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46311995"
---
# <a name="topologies-for-azure-ad-connect"></a>Topologieën voor Azure AD Connect
Dit artikel beschrijft de verschillende on-premises en Azure Active Directory (Azure AD)-topologieën die gebruikmaken van Azure AD Connect-synchronisatie als de integratieoplossing key. Dit artikel bevat de ondersteunde en niet-ondersteunde configuraties.

Hier volgt de legenda voor afbeeldingen in het artikel:

| Beschrijving | Symbool |
| --- | --- |
| On-premises Active Directory-forest |![On-premises Active Directory-forest](./media/plan-connect-topologies/LegendAD1.png) |
| On-premises Active Directory met gefilterde importeren |![Active Directory met gefilterde importeren](./media/plan-connect-topologies/LegendAD2.png) |
| Azure AD Connect-synchronisatieserver |![Azure AD Connect-synchronisatieserver](./media/plan-connect-topologies/LegendSync1.png) |
| Azure AD Connect-synchronisatieserver "faseringsmodus" |![Azure AD Connect-synchronisatieserver "faseringsmodus"](./media/plan-connect-topologies/LegendSync2.png) |
| GALSync met Forefront Identity Manager (FIM) 2010 of Microsoft Identity Manager (MIM) 2016 |![GALSync met FIM 2010 of MIM 2016](./media/plan-connect-topologies/LegendSync3.png) |
| Azure AD Connect-synchronisatieserver, gedetailleerde |![Azure AD Connect-synchronisatieserver, gedetailleerde](./media/plan-connect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/plan-connect-topologies/LegendAAD.png) |
| Niet-ondersteund scenario |![Niet-ondersteund scenario](./media/plan-connect-topologies/LegendUnsupported.png) |


> [!IMPORTANT]
> Microsoft biedt geen ondersteuning voor wijzigen of uitvoeren van Azure AD Connect-synchronisatie buiten de configuraties of acties die officieel is gedocumenteerd. Een van deze configuraties of acties kan leiden tot een inconsistente of niet-ondersteunde status van Azure AD Connect-synchronisatie. Daarom biedt Microsoft geen technische ondersteuning voor dergelijke implementaties.


## <a name="single-forest-single-azure-ad-tenant"></a>Eén forest, één Azure AD-tenant
![Topologie voor één forest en één tenant](./media/plan-connect-topologies/SingleForestSingleDirectory.png)

De meest voorkomende topologie is één on-premises forest, met een of meerdere domeinen en één Azure AD-tenant. Voor Azure AD-verificatie, wordt wachtwoord-hashsynchronisatie gebruikt. De snelle installatie van Azure AD Connect ondersteunt alleen deze topologie.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Eén forest, meerdere synchronisatieservers in één Azure AD-tenant
![Niet-ondersteunde, gefilterde topologie voor één forest](./media/plan-connect-topologies/SingleForestFilteredUnsupported.png)

Met meerdere Azure AD Connect-synchronisatieservers is verbonden met dezelfde Azure AD-tenant wordt niet ondersteund, met uitzondering van een [testserver](#staging-server). Het bevat niet-ondersteunde, zelfs als deze servers zijn geconfigureerd om te synchroniseren met een set objecten die sluiten elkaar wederzijds uit. U kunt hebben beschouwd als deze topologie als u alle domeinen in het forest van één server niet bereiken, of als u wilt verdelen over meerdere servers.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Meerdere forests, één Azure AD-tenant
![Topologie voor meerdere forests en één tenant](./media/plan-connect-topologies/MultiForestSingleDirectory.png)

Veel organisaties beschikken over omgevingen met meerdere on-premises Active Directory-forests. Er zijn verschillende redenen hebben van meer dan één on-premises Active Directory-forest. Typische voorbeelden zijn mogelijk te maken met account-bron-forests en het resultaat van een fusie of overname.

Wanneer u hebt meerdere forests, alle forests bereikbaar is via een enkel moet Azure AD Connect-synchronisatieserver. U hebt geen de-server toevoegen aan een domein. Indien nodig alle forests bereiken, kunt u de server plaatsen in een perimeternetwerk (ook wel DMZ, gedemilitariseerde zone en gescreend subnet genoemd).

De Azure AD Connect-installatiewizard biedt verschillende opties om te consolideren van gebruikers die worden weergegeven in meerdere forests. Het doel is dat een gebruiker slechts één keer in Azure AD wordt gerepresenteerd. Er zijn enkele algemene topologieën die u in het pad aangepaste installatie in de installatiewizard configureren kunt. Op de **uw gebruikers identificeren** pagina, selecteert u de bijbehorende optie waarmee uw topologie. De samenvoeging wordt alleen voor gebruikers geconfigureerd. Dubbele groepen worden niet samengevoegd met de standaardconfiguratie.

Algemene topologieën worden beschreven in de secties over [afzonderlijke topologieën](#multiple-forests-separate-topologies), [volledige mesh](#multiple-forests-full-mesh-with-optional-galsync), en [de account-resource-topologie](#multiple-forests-account-resource-forest).

De standaardconfiguratie in Azure AD Connect-synchronisatie wordt ervan uitgegaan dat:

* Elke gebruiker heeft slechts één ingeschakeld account en het forest waar dit account zich bevindt, wordt gebruikt voor verificatie van de gebruiker. Deze aanname is voor synchronisatie van wachtwoordhashes, pass-through-verificatie en Federatie. UserPrincipalName en sourceAnchor/immutableID zijn afkomstig van dit forest.
* Elke gebruiker heeft slechts één postvak.
* Het forest dat als host fungeert voor het postvak van een gebruiker heeft de beste kwaliteit van de gegevens voor kenmerken die zichtbaar zijn in de Exchange globale adreslijst (GAL). Als er geen postvak voor de gebruiker, kan elk forest kan worden gebruikt om bij te dragen deze kenmerkwaarden.
* Hebt u een gekoppeld postvak, is er ook een account in een ander forest dat is gebruikt voor aanmelding bij.

Als uw omgeving komt niet overeen met deze veronderstellingen, gebeuren de volgende dingen:

* Als u meer dan één actieve account of meer dan één postvak hebt, wordt de synchronisatie-engine kiest een en de andere worden genegeerd.
* Een gekoppeld postvak zonder actieve rekening is niet geëxporteerd naar Azure AD. Het gebruikersaccount wordt niet weergegeven als een lid van een groep. Een gekoppeld postvak in DirSync wordt altijd weergegeven als een normale postvak. Deze wijziging is opzettelijk een verschillend gedrag voor betere ondersteuning van scenario's voor meerdere forests.

U vindt meer informatie in [inzicht in de standaardconfiguratie](concept-azure-ad-connect-sync-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Meerdere forests, meerdere synchronisatieservers in één Azure AD-tenant
![Niet-ondersteunde topologie voor meerdere forests en meerdere synchronisatieservers](./media/plan-connect-topologies/MultiForestMultiSyncUnsupported.png)

Met meer dan één Azure AD Connect-synchronisatieserver die is verbonden met één Azure AD-tenant wordt niet ondersteund. De uitzondering hierop is het gebruik van een [testserver](#staging-server).

### <a name="multiple-forests-separate-topologies"></a>Meerdere forests, afzonderlijke topologieën
![Optie voor de vertegenwoordiging van gebruikers slechts één keer op alle mappen](./media/plan-connect-topologies/MultiForestUsersOnce.png)

![Weergave van meerdere forests en afzonderlijke topologieën](./media/plan-connect-topologies/MultiForestSeperateTopologies.png)

In deze omgeving, worden alle on-premises forests behandeld als afzonderlijke entiteiten. Er is geen gebruiker is aanwezig in een andere forest. Elke forest heeft een eigen Exchange-organisatie, en er is geen GALSync tussen de forests. Deze topologie mogelijk de situatie na een fusie/verwerving van of in een organisatie waar elk bedrijfsonderdeel werkt onafhankelijk van elkaar. Deze forests zijn in dezelfde organisatie in Azure AD en worden weergegeven met een uniforme GAL. In de voorgaande afbeelding wordt elk object in elk forest één keer weergegeven in de metaverse en samengevoegd in de doel-Azure AD-tenant.

### <a name="multiple-forests-match-users"></a>Meerdere forests: overeenkomst met gebruikers
Voor deze scenario's is dat distributie en beveiligingsgroepen kunnen een combinatie van gebruikers, contactpersonen en afwijkende beveiligings-Principals (FSP's) bevatten. FSP's zijn voor leden van andere forests in een beveiligingsgroep in Active Directory Domain Services (AD DS) gebruikt. Alle FSP's worden omgezet in het werkelijke object in Azure AD.

### <a name="multiple-forests-full-mesh-with-optional-galsync"></a>Meerdere forests: volledige mesh met optionele GALSync
![Optie voor het gebruik van het e-mailkenmerk voor overeenkomende als er gebruikers-id's in meerdere mappen bestaan](./media/plan-connect-topologies/MultiForestUsersMail.png)

![Full mesh-topologie voor meerdere forests](./media/plan-connect-topologies/MultiForestFullMesh.png)

Een full mesh-topologie kan gebruikers en resources zich bevinden in een forest. Er zijn doorgaans vertrouwensrelaties in twee richtingen tussen de forests.

Als Exchange in meer dan één forest aanwezig is, kan er (optioneel) een on-premises GALSync-oplossing. Elke gebruiker wordt vervolgens weergegeven als een contactpersoon in alle andere forests. GALSync wordt gewoonlijk geïmplementeerd via de FIM 2010 of MIM 2016. Azure AD Connect kan niet worden gebruikt voor on-premises GALSync.

In dit scenario zijn identiteitsobjecten gekoppeld via het e-mailkenmerk. Een gebruiker met een postvak in één forest wordt samengevoegd met de contactpersonen in de andere forests.

### <a name="multiple-forests-account-resource-forest"></a>Meerdere forests: account-bron-forest
![Optie voor het gebruik van de ObjectSID- en msExchMasterAccountSID kenmerken voor het afstemmen van wanneer identiteiten in meerdere mappen bestaan](./media/plan-connect-topologies/MultiForestUsersObjectSID.png)

![Account-resource foresttopologie voor meerdere forests](./media/plan-connect-topologies/MultiForestAccountResource.png)

In een topologie met account-resource forest, hebt u een of meer *account* forests met actieve gebruikersaccounts. U hebt ook een of meer *resource* forests met uitgeschakelde accounts.

In dit scenario vertrouwt een (of meer)-bron-forest alle accountforests. De bron-forest heeft doorgaans een uitgebreid Active Directory-schema met Exchange en Lync. Alle Exchange en Lync-services, samen met andere gedeelde services bevinden zich in het forest. Gebruikers hebben een uitgeschakelde gebruikersaccount in het forest en het postvak wordt gekoppeld aan het forest.

## <a name="office-365-and-topology-considerations"></a>Office 365 en aandachtspunten voor topologie
Sommige Office 365-werkbelastingen hebben bepaalde beperkingen op ondersteunde topologieën:

| Workload | Beperkingen |
| --------- | --------- |
| Exchange Online | Zie voor meer informatie over hybride topologieën ondersteund door Exchange Online, [hybride implementaties met meerdere Active Directory-forests](https://technet.microsoft.com/library/jj873754.aspx). |
| Skype voor Bedrijven | Als u meerdere on-premises forests, worden alleen de topologie van de account-resource forest wordt ondersteund. Zie voor meer informatie, [omgevingsvereisten voor Skype for Business Server 2015](https://technet.microsoft.com/library/dn933910.aspx). |

Als u een grotere organisatie bent, dan kunt u overwegen om te gebruiken de [Office 365 PreferredDataLocation](how-to-connect-sync-feature-preferreddatalocation.md) functie. Hiermee kunt u bepalen in welke datacentrumregio van de gebruiker resources zich bevinden.

## <a name="staging-server"></a>Staging-server
![Staging-server in een topologie](./media/plan-connect-topologies/MultiForestStaging.png)

Azure AD Connect ondersteunt het installeren van een tweede server in *faseringsmodus*. Een server in deze modus leest gegevens uit alle gekoppelde mappen, maar niet schrijven naar verbonden mappen. Het maakt gebruik van de normale synchronisatiecyclus en daarom heeft een bijgewerkte kopie van de identiteitsgegevens.

In een noodsituatie waar de primaire server mislukt, kunt u een failover naar de staging-server. Dit doet u in de Azure AD Connect-wizard. Deze tweede server kan zich bevinden in een ander datacenter, omdat er geen infrastructuur wordt gedeeld met de primaire server. U moet handmatig een configuratiewijziging gemaakt op de primaire server naar de tweede server kopiëren.

U kunt een staging-server gebruiken om een nieuwe aangepaste configuratie en het effect dat er op uw gegevens te testen. U kunt de wijzigingen bekijken en aanpassen van de configuratie. Wanneer u tevreden met de nieuwe configuratie bent, kunt u de actieve server van de staging-server maken en de oude active server ingesteld op de faseringsmodus.

U kunt deze methode ook gebruiken om te vervangen de ActiveSync-server. De nieuwe server voorbereiden en stel deze in op de faseringsmodus. Zorgt ervoor dat deze in goede staat verkeren, schakel de faseringsmodus is (waardoor dit active), en de momenteel actieve server afsluiten.

Het is mogelijk dat meer dan één server met tijdelijke bestanden wanneer u meerdere back-ups hebben in verschillende datacenters.

## <a name="multiple-azure-ad-tenants"></a>Meerdere Azure AD-tenants
We raden u aan één tenant in Azure AD voor een organisatie.
Voordat u van plan bent te gebruiken van meerdere Azure AD-tenants, Zie het artikel [administratieve eenheden beheren in Azure AD](../users-groups-roles/directory-administrative-units.md). Hierin worden algemene scenario's waarbij u één tenant kunt gebruiken.

![Topologie voor meerdere forests en meerdere tenants](./media/plan-connect-topologies/MultiForestMultiDirectory.png)

Er is een 1:1-relatie tussen een Azure AD Connect-synchronisatieserver en een Azure AD-tenant. Voor elke Azure AD-tenant moet u een Azure AD Connect sync-server-installatie. De exemplaren van de Azure AD-tenant zijn geïsoleerd standaard. Dat wil zeggen, gebruikers in één tenant kunnen gebruikers in de andere tenant niet zien. Als u wilt dat deze scheiding, is dit een ondersteunde configuratie. Anders moet u de één model van Azure AD-tenant.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Elk object slechts één keer in een Azure AD-tenant
![Gefilterde topologie voor één forest](./media/plan-connect-topologies/SingleForestFiltered.png)

In deze topologie wordt is één Azure AD Connect-synchronisatieserver verbonden met elke Azure AD-tenant. De Azure AD Connect-synchronisatieservers moeten worden geconfigureerd voor het filteren, zodat beide over een sluiten elkaar wederzijds uit set objecten die moeten worden uitgevoerd op. U kunt bijvoorbeeld het bereik van elke server aan een bepaald domein of organisatie-eenheid.

Een DNS-domein kan worden geregistreerd in slechts één Azure AD-tenant. De UPN's van de gebruikers in de on-premises Active Directory-exemplaar moeten ook afzonderlijke naamruimten gebruiken. Bijvoorbeeld drie afzonderlijke UPN-achtervoegsels in de voorgaande afbeelding zijn geregistreerd in de on-premises Active Directory-exemplaar: contoso.com en fabrikam.com wingtiptoys.com. De gebruikers in elke on-premises Active Directory-domein gebruiken een andere naamruimte.

>[!NOTE]
>Globale adres lijst synchronisatie (GalSync) wordt niet automatisch uitgevoerd in deze topologie en vereist een aanvullende aangepaste MIM-implementatie om ervoor te zorgen dat elke tenant heeft een volledige globale adreslijst (GAL) in Exchange Online en Skype voor bedrijven Online.


Deze topologie heeft de volgende beperkingen met betrekking tot-ondersteund scenario's:

* Slechts één van de Azure AD-tenants kan een Exchange-hybride oplossing met de on-premises Active Directory-exemplaar inschakelen.
* Windows 10-apparaten kunnen worden gekoppeld aan slechts één Azure AD-tenant.
* De eenmalige aanmelding (SSO) optie voor wachtwoord-hash-synchronisatie en Pass through-verificatie kan worden gebruikt met slechts één Azure AD-tenant.

De vereiste voor een sluiten elkaar wederzijds uit set van objecten geldt ook voor terugschrijven van wachtwoorden. Sommige Write-back-functies worden niet ondersteund met deze topologie omdat ze gaan ervan uit een configuratie met één on-premises. Deze functies zijn onder andere:

* Groep terugschrijven van wachtwoorden met standaardconfiguratie.
* Write-back van apparaat.

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Elk object meerdere keren in een Azure AD-tenant
![Niet-ondersteunde topologie voor één forest en meerdere tenants](./media/plan-connect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Niet-ondersteunde topologie voor één forest en meerdere connectors](./media/plan-connect-topologies/SingleForestMultiConnectorsUnsupported.png)

Deze taken worden niet ondersteund:

* Synchroniseren van de gebruiker aan meerdere Azure AD-tenants.
* Een configuratiewijziging hebt doorgevoerd zodat gebruikers in een Azure AD-tenant worden weergegeven als contactpersonen in een andere Azure AD-tenant maken.
* Azure AD Connect-synchronisatie verbinding maken met meerdere Azure AD-tenants wijzigen.

### <a name="galsync-by-using-writeback"></a>GALSync met behulp van Write-back
![Niet-ondersteunde topologie voor meerdere forests en meerdere mappen, met GALSync gericht op Azure AD](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Niet-ondersteunde topologie voor meerdere forests en meerdere mappen, met GALSync te focussen op on-premises Active Directory](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD-tenants, zijn geïsoleerd standaard. Deze taken worden niet ondersteund:

* Wijzig de configuratie van Azure AD Connect-synchronisatie gegevens lezen uit een andere Azure AD-tenant.
* Gebruikers exporteren als contactpersonen aan een andere on-premises Active Directory-exemplaar met behulp van Azure AD Connect-synchronisatie.

### <a name="galsync-with-on-premises-sync-server"></a>GALSync met on-premises-synchronisatieserver
![GALSync in een topologie voor meerdere forests en meerdere mappen](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync.png)

U kunt FIM 2010 of MIM 2016 on-premises gebruiken om te synchroniseren van gebruikers (via GALSync) tussen twee Exchange-organisaties. De gebruikers in een organisatie weergegeven als externe gebruikers/contactpersonen in de andere organisatie. Deze verschillende on-premises Active Directory exemplaren kunnen vervolgens worden gesynchroniseerd met hun eigen Azure AD-tenants.

## <a name="next-steps"></a>Volgende stappen
Zie voor informatie over het installeren van Azure AD Connect voor deze scenario's, [aangepaste installatie van Azure AD Connect](how-to-connect-install-custom.md).

Meer informatie over de [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md) configuratie.

Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).
