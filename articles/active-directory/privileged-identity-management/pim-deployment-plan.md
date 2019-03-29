---
title: Privileged Identity Management (PIM) - Azure Active Directory implementeren | Microsoft Docs
description: Beschrijft hoe u van plan bent de implementatie van Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/08/2019
ms.author: rolyon
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1755d627473b0ae47bbc4bc74a3f0d2210e5372b
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578190"
---
# <a name="deploy-azure-ad-privileged-identity-management-pim"></a>Azure AD Privileged Identity Management (PIM) implementeren

Deze stapsgewijze handleiding wordt beschreven hoe u de implementatie van Azure Active Directory (Azure AD) Privileged Identity Management (PIM) in uw organisatie van plan bent.

> [!TIP]
> In dit document ziet u de items die zijn gemarkeerd als:
> 
> :heavy_check_mark: **Microsoft raadt aan**
> 
> Dit zijn algemene aanbevelingen en moet u alleen implementeren als ze van toepassing op uw specifieke zakelijke behoeften zijn.

## <a name="step-1-learn-about-pim"></a>Step 1. Meer informatie over PIM

Azure AD Privileged Identity Management (PIM) helpt u bij het beheren van bevoegde beheerdersrollen in Azure AD, Azure-resources en andere Microsoft Online Services. In een wereld waar bevoegde identiteiten zijn toegewezen en bent vergeten, biedt PIM oplossingen zoals just-in-time-toegang, aanvraag goedkeuringswerkstromen en volledig geïntegreerde toegang beoordelingen, zodat u identificeren kunt, ontdekken en te voorkomen dat schadelijke activiteiten van bevoegdheden rollen in realtime. PIM voor het beheren van uw bevoorrechte rollen binnen uw organisatie implementeert, wordt risico aanzienlijk verlagen terwijl u waardevolle inzichten over de activiteiten van uw bevoorrechte rollen zichtbaar te maken.

### <a name="business-value-of-pim"></a>Bedrijfswaarde van PIM

**Risico's beheren** -uw organisatie beveiligen door af te dwingen van het principe van [minimale bevoegdheden toegang](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) en just-in-time-toegang. Door het minimaliseren van het aantal permanente toewijzingen van gebruikers aan bevoorrechte rollen en het afdwingen van goedkeuringen en MFA voor uitbreiding van bevoegdheden, kunt u beveiligingsrisico's met betrekking tot bevoorrechte toegang in uw organisatie aanzienlijk verminderen. Afdwingen van minimale bevoegdheden en just-in-time-toegang ook kunt u een overzicht van de toegang tot bevoorrechte rollen bekijken en te volgen op beveiligingsproblemen met zich mee wanneer deze zich voordoen.

**Naleving en governance** -PIM implementeren maakt u een omgeving voor continue identity governance. Just-in-time-uitbreiding van bevoorrechte identiteiten biedt een manier voor PIM voor het bijhouden van activiteiten van bevoegde toegang in uw organisatie. U wordt ook mogelijk om te bekijken en te ontvangen van meldingen voor alle toewijzingen van permanente en in aanmerking komende rollen binnen uw organisatie. Via toegangsbeoordeling, kunt u regelmatig controleren en verwijderen van onnodige bevoegde identiteiten en zorgt ervoor dat uw organisatie compatibel zijn met de meest strikte standaarden voor identiteit, toegang en beveiliging.

**Kosten verlagen** -kosten verlagen door het elimineren van inefficiëntie, menselijke fouten en beveiligingsproblemen met zich mee door PIM correct te implementeren. Het resultaat is een vermindering van cyber crimes die zijn gekoppeld aan bevoorrechte identiteiten die zijn duur en moeilijk te herstellen. PIM helpt ook uw organisatie verminderen de kosten die gepaard gaan met de controle van toegang tot informatie als het gaat om te voldoen aan regelgeving en standaarden.

Zie voor meer informatie, [wat is Azure AD Privileged Identity Management?](pim-configure.md).

### <a name="licensing-requirements"></a>Vereisten voor licentieverlening

Voor het gebruik van PIM, moet uw directory een van de volgende betaalde of proeflicenties hebben:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Zie voor meer informatie, [licentie-vereisten voor het gebruik van PIM](subscription-requirements.md).

### <a name="key-pim-terminology"></a>Sleutel PIM-terminologie

| Term of concept | Description |
| --- | --- |
| in aanmerking komend | Een roltoewijzing die vereist dat een gebruiker een of meer acties uitvoert om de rol te kunnen gebruiken. Als een gebruiker in aanmerking komt voor een rol, betekent dit dat de gebruiker de rol kan activeren wanneer deze nodig is om bevoegde taken uit te voeren. Er is geen verschil in de toegang voor iemand met een permanente roltoewijzing ten opzichte van iemand die in aanmerking komt voor een roltoewijzing. Het enige verschil is dat sommige gebruikers niet voortdurend toegang nodig hebben. |
| activeren | Het proces van het uitvoeren van een of meer acties om de rol te kunnen gebruiken waarvoor de gebruiker in aanmerking komt. Acties kunnen bijvoorbeeld een meervoudige verificatiecontrole, het opgeven van een zakelijke reden of het vragen om toestemming bij aangewezen fiatteurs zijn. |
| Just-in-time-toegang (JIT) | Een model waarbij gebruikers tijdelijke machtigingen ontvangen om bepaalde taken uit te mogen voeren, waardoor kwaadwillende of onbevoegde gebruikers geen toegang kunnen krijgen na het verlopen van deze machtigingen. Toegang wordt alleen verleend wanneer gebruikers deze nodig hebben. |
| Principe van toegang met minimale bevoegdheden | Een aanbevolen beveiligingsprocedure waarbij alle gebruikers enkel de minimale bevoegdheden krijgt toegewezen die nodig zijn om de taken uit te voeren waarvoor ze bevoegd zijn. Met deze procedure wordt het aantal globale beheerders tot het minimum beperkt en worden er specifieke beheerdersrollen gebruikt voor bepaalde scenario's. |

Zie voor meer informatie, [terminologie](pim-configure.md#terminology).

### <a name="high-level-overview-of-how-pim-works"></a>Overzicht op hoog niveau van de werking van PIM

1. PIM is ingesteld zodat gebruikers in aanmerking voor bevoorrechte rollen komen.
1. Wanneer een in aanmerking komende gebruiker moet het gebruik van hun bevoegde rol, activeren ze de rollen in PIM.
1. Afhankelijk van de PIM-instellingen voor de rol is geconfigureerd, moet de gebruiker bepaalde stappen (zoals meervoudige verificatie, ze goedkeuring, of een reden op te geven.) uitvoeren.
1. Zodra de gebruiker is de rol activeert, krijgt deze de rol voor een vooraf geconfigureerde periode.
1. Beheerders kunnen een geschiedenis van alle PIM-activiteiten bekijken in het auditlogboek. Ze kunnen ook hun tenants beveiligen en voldoen aan nalevingsvereisten met PIM-functies, zoals beoordelingen en waarschuwingen.

Zie voor meer informatie, [wat is Azure AD Privileged Identity Management?](pim-configure.md).

### <a name="roles-that-can-be-managed-by-pim"></a>Rollen die kunnen worden beheerd door PIM

**Azure AD-rollen** – deze rollen bevinden zich allemaal in Azure Active Directory (zoals globale beheerder, Exchange-beheerder en Beveiligingsbeheerder). U kunt meer lezen over de functies en hun functionaliteit in [rol beheerdersmachtigingen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md). Zie voor hulp bij het bepalen van welke rollen u wilt uw beheerders toewijst aan [minste bevoorrechte rollen door taak](../users-groups-roles/roles-delegate-by-task.md).

**Azure-resourcerollen** – deze rollen zijn gekoppeld aan een Azure-resource, resourcegroep, abonnement of beheergroep. PIM biedt just-in-time-toegang tot zowel ingebouwde rollen, zoals eigenaar, beheerder van gebruikerstoegang en Inzender, evenals [aangepaste rollen](../../role-based-access-control/custom-roles.md). Zie voor meer informatie over Azure-resourcerollen [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/overview.md).

Zie voor meer informatie, [rollen in PIM kunt u niet beheren](pim-roles.md).

## <a name="step-2-plan-your-deployment"></a>Stap 2. Uw implementatie plannen

Deze sectie richt zich op wat u doen moet voordat u PIM implementeert in uw organisatie. Het is essentieel voor de instructies en informatie over de concepten in deze sectie als ze leidt u om de beste plan die is ontworpen voor bevoorrechte identiteiten van uw organisatie te maken.

### <a name="identify-your-stakeholders"></a>Uw belanghebbenden identificeren

De volgende sectie helpt u identificeren van de betrokkenen die bij het project betrokken zijn en wilt afmelden, bekijken of blijf op de hoogte. Het bevat verschillende tabellen voor het implementeren van PIM voor Azure AD-rollen en PIM voor Azure-resource-rollen. Belanghebbenden toevoegen aan de volgende tabel als geschikt is voor uw organisatie.

- Meld u zo = uit op dit project
- R = Review dit project en gegevens opgeven
- Ik = geïnformeerde van dit project

#### <a name="stakeholders-pim-for-azure-ad-roles"></a>Belanghebbenden: PIM voor Azure AD-rollen

| Name | Rol | Bewerking |
| --- | --- | --- |
| Naam en e-mailadres | **Identiteit architect of globale beheerder van Azure**<br/>Een vertegenwoordiger van het identity management-team verantwoordelijk definiëren hoe deze wijziging wordt uitgelijnd met de kerninfrastructuur voor het beheer van identiteit in uw organisatie. | SO/R/I |
| Naam en e-mailadres | **Service-eigenaar / manager regel**<br/>Een vertegenwoordiger van de IT-eigenaren van een service of een groep services. Deze zijn sleutel in het nemen van beslissingen en helpt bij de implementatie van PIM voor hun team. | SO/R/I |
| Naam en e-mailadres | **De eigenaar van de beveiliging**<br/>Een vertegenwoordiger van het beveiligingsteam die het plan uit die kan zich voldoet aan de beveiligingsvereisten van uw organisatie. | SO/R |
| Naam en e-mailadres | **IT manager ondersteuning / Helpdesk**<br/>Een vertegenwoordiger van de IT-ondersteuning voor organisatie die gegevens over de ondersteuning van deze wijziging vanuit het oogpunt van de helpdesk kunt opgeven. | R/I |
| Naam en e-mailadres voor testgebruikers | **Gebruikers in bevoorrechte rollen**<br/>De groep gebruikers waarvoor privileged identity management is geïmplementeerd. Ze moeten wilt weten hoe hun rollen activeren nadat het PIM is geïmplementeerd. | I |

#### <a name="stakeholders-pim-for-azure-resource-roles"></a>Belanghebbenden: PIM voor Azure-resourcerollen

| Name | Rol | Bewerking |
| --- | --- | --- |
| Naam en e-mailadres | **Abonnement / Resource-eigenaar**<br/>Een vertegenwoordiger van de IT-eigenaren van elk abonnement en de resource die u wilt implementeren PIM voor | SO/R/I |
| Naam en e-mailadres | **De eigenaar van de beveiliging**<br/>Een vertegenwoordiger van het beveiligingsteam die het plan uit die kan zich voldoet aan de beveiligingsvereisten van uw organisatie. | SO/R |
| Naam en e-mailadres | **IT manager ondersteuning / Helpdesk**<br/>Een vertegenwoordiger van de IT-ondersteuning voor organisatie die gegevens over de ondersteuning van deze wijziging vanuit het oogpunt van de helpdesk kunt opgeven. | R/I |
| Naam en e-mailadres voor testgebruikers | **Gebruikers van RBAC-rol**<br/>De groep gebruikers waarvoor privileged identity management is geïmplementeerd. Ze moeten wilt weten hoe hun rollen activeren nadat het PIM is geïmplementeerd. | I |

### <a name="enable-pim"></a>PIM inschakelen

Als onderdeel van het planningsproces, moet u eerst toestemming geven en PIM inschakelen door onze [starten met PIM-document](pim-getting-started.md). PIM inschakelen, geeft u toegang tot sommige functies die speciaal zijn ontworpen om te helpen bij uw implementatie.

Als het doel is om te implementeren PIM voor Azure-resources, moet u onze [detecteren van Azure-resources te beheren in PIM-document](pim-resource-roles-discover-resources.md). Alleen eigenaars van elke resource, resourcegroep en abonnement zich voor het detecteren van deze in PIM. Als u een globale beheerder bent bij het PIM implementeren voor uw Azure-resources, kunt u [toegangsrechten voor het beheren van alle Azure-abonnementen](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) zelf toegang geven tot alle Azure-resources in de map voor detectie. Echter, we adviseren dat u goedkeuring van elk van de eigenaars van uw abonnementen voordat hun resources beheren met PIM ophalen.

### <a name="enforce-principle-of-least-privilege"></a>Principe van minimale bevoegdheden afdwingen

Het is belangrijk om ervoor te zorgen dat u het principe van minimale bevoegdheden hebben afgedwongen in uw organisatie voor uw Azure AD en uw Azure-resource-rollen.

#### <a name="azure-ad-roles"></a>Azure AD-rollen

Voor Azure AD-rollen is het gebruikelijk voor organisaties die de rol globale beheerder een groot aantal beheerders wanneer de meeste beheerders slechts één hoeft of twee specifieke beheerdersrollen toewijzen. Niet-bevoorrechte rol toewijzingen doorgaans ook worden beheerd.

> [!NOTE]
> Veelvoorkomende valkuilen in delegatie van de rol:
>
> - De beheerder die verantwoordelijk zijn voor Exchange wordt de rol Algemeen beheerder gegeven, ondanks dat ze hoeven alleen de rol Exchange-beheerder om uit te voeren hun dagelijkse werk.
> - De globale beheerdersrol is toegewezen aan een beheerder van Office omdat de beheerder moet het beveiligings- en SharePoint-beheerdersrollen en het is gemakkelijker om slechts één rol delegeren.
> - De beheerder een rol Beveiligingsbeheerder voor het uitvoeren van een taak lang geleden is toegewezen, maar is nooit verwijderd.

Volg deze stappen om af te dwingen van het principe van minimale bevoegdheden voor uw Azure AD-rollen.

1. Inzicht in de granulatie van de rollen door te lezen en inzicht krijgen in de [beschikbaar Azure AD-beheerdersrollen](../users-groups-roles/directory-assign-admin-roles.md#available-roles). U en uw team moeten ook verwijzen naar [door de taak identiteit in Azure AD-beheerdersrollen](../users-groups-roles/roles-delegate-by-task.md), waarin de minste bevoorrechte rol voor specifieke taken wordt uitgelegd.

1. De lijst die is bevoorrechte rollen in uw organisatie. U kunt de [PIM wizard](pim-security-wizard.md#run-the-wizard) naar een pagina als volgt te gaan.

    ![Bevoorrechte rollen detecteren](./media/pim-deployment-plan/discover-privileged-roles-users.png)

1. Voor alle globale beheerders in uw organisatie, ontdek waarom ze de rol nodig hebben. Op basis van de vorige documentatie lezen als de persoon die de taak kan worden uitgevoerd door een of meer gedetailleerde beheerdersrollen, moet u deze verwijderen uit de rol globale beheerder en dienovereenkomstig toewijzingen maken in Azure Active Directory (als uitgangspunt: Microsoft is momenteel alleen ongeveer 10 beheerders met de rol globale beheerder. Meer informatie over [hoe Microsoft PIM gebruikt](https://www.microsoft.com/itshowcase/Article/Content/887/Using-Azure-AD-Privileged-Identity-Management-for-elevated-access)).

1. Bekijk de lijst met toewijzingen, identificeren beheerders die de rol niet langer nodig hebt en verwijderen uit de toewijzingen voor alle andere Azure AD-rollen.

U kunt de functie voor het controleren van toegang tot in PIM gebruiken voor het automatiseren van stap 3 en 4. De stappen in [een toegangscontrole voor Azure AD-rollen in PIM starten](pim-how-to-start-security-review.md), kunt u een toegangscontrole voor elke Azure AD-rol die een of meer leden heeft ingesteld.

![Een toegangsbeoordeling maken](./media/pim-deployment-plan/create-access-review.png)

U moet de revisoren ingesteld op **leden (zelf)**. Hiermee wordt een e-mail verzonden op alle leden in de rol aan om ze om te bevestigen of ze de toegang nodig hebben. U moet ook inschakelen **reden vereisen bij goedkeuring** in de geavanceerde instellingen zodat gebruikers kunnen aangeven waarom ze moeten de rol. Op basis van deze informatie, kunt u zich gebruikers verwijderen van onnodige rollen en het delegeren van gedetailleerdere beheerdersrollen in het geval van globale beheerders.

Toegangsbeoordelingen zijn afhankelijk van e-mailberichten mensen om te controleren van hun toegang tot de rollen te laten weten. Als u accounts waarvoor geen e-mails die zijn gekoppeld uitgebreide, moet u voor het vullen van het veld secundaire e-mailadres op deze accounts. Zie voor meer informatie, [proxyAddresses-kenmerk in Azure AD](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad).

#### <a name="azure-resource-roles"></a>Azure-resourcerollen

Voor Azure-abonnementen en resources, kunt u een soortgelijk proces uit Access controle instellen om te controleren van de rollen in elk abonnement of resourcegroep. Het doel van dit proces is om te minimaliseren, eigenaar en beheerder van gebruikerstoegang toewijzingen die zijn gekoppeld aan elk abonnement en de bron evenals garantie voor verwijderen van onnodige toewijzingen. Echter, organisaties vaak dergelijke taken delegeren aan de eigenaar van elk abonnement of resourcegroep omdat ze hebben een beter begrip van de specifieke rollen (met name aangepaste rollen).

Als u een IT-beheerder met de rol globale beheerder bij het PIM voor Azure-resources in uw organisatie implementeert, kunt u [toegangsrechten voor het beheren van alle Azure-abonnementen](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) toegang te krijgen tot elk abonnement. U kunt vervolgens de eigenaar van elk abonnement vinden en werken met ze voor het verwijderen van onnodige toewijzingen en roltoewijzing eigenaar minimaliseren.

Gebruikers met de rol van eigenaar voor een Azure-abonnement kunnen ook gebruikmaken van [toegangsbeoordelingen voor Azure-resources](pim-resource-roles-start-access-review.md) controleren en verwijderen van onnodige roltoewijzingen die vergelijkbaar is met het proces dat eerder is beschreven voor Azure AD-rollen.

### <a name="decide-which-role-assignments-should-be-protected-by-pim"></a>Bepalen welke roltoewijzingen moeten worden beveiligd door PIM

Na het opschonen van bevoorrechte rollen zijn toegewezen in uw organisatie, moet u beslissen welke rollen u wilt beveiligen met PIM.

Als een rol wordt beveiligd door PIM, moeten in aanmerking komende gebruikers zijn toegewezen met verhoogde bevoegdheden om de bevoegdheden verleend door de rol te gebruiken. Het proces van uitbreiding van bevoegdheden mogelijk ook het verkrijgen van goedkeuring, uitvoeren van meervoudige verificatie en/of die een reden op waarom ze wilt activeren. PIM kunt ook hoogten via meldingen volgen en logboeken voor controle uitvoeren op de PIM- en Azure AD.

Kiezen welke rollen u wilt beveiligen met PIM kunnen lastig zijn en zijn verschillend voor elke organisatie. Deze sectie bevat onze beste adviezen voor Azure AD en Azure-resource-rollen.

#### <a name="azure-ad-roles"></a>Azure AD-rollen

Het is belangrijk om te prioriteren beveiligen van Azure AD-rollen die het hoogste aantal machtigingen hebben. Op basis van gebruikspatronen onder alle PIM-klanten, zijn de top 10 Azure AD-rollen die worden beheerd door PIM:

1. Globale beheerder
1. Beveiligingsbeheerder
1. Gebruikersbeheerder
1. Exchange-beheerder
1. SharePoint-beheerder
1. Intune-beheerder
1. Beveiligingslezer
1. Servicebeheerder
1. Factureringsbeheerder
1. Skype voor Bedrijven-beheerder

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u beheert alle globale beheerders en Beveiligingsbeheerders PIM gebruiken als eerste stap als ze degene die de meeste schade zijn als geknoeid kunnen doen.

Het is belangrijk te bedenken welke gegevens en de machtiging meest gevoelig zijn voor uw organisatie zijn. Een voorbeeld: sommige organisaties mogelijk wilt beveiligen hun Power BI-beheerdersrol of de beheerdersrol van Teams met PIM zoals ze de mogelijkheid hebben om toegang tot gegevens en/of core-werkstromen wijzigen.

Als er geen rollen met gastgebruikers die zijn toegewezen zijn, worden ze met name kwetsbaar voor aanvallen.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u alle rollen beheren met gastgebruikers met PIM te verkleinen van risico's die zijn gekoppeld aan de gebruikersaccounts waarmee is geknoeid Gast.

Rol van beveiligingslezer, zoals de lezer van de Directory, Message Center lezer en Beveiligingslezer zijn soms lijkt te zijn minder belangrijke in vergelijking met andere rollen, omdat ze geen schrijftoegang hebben. Soms echter enkele klanten ook deze rollen worden beveiligd omdat het is mogelijk dat kwaadwillende gebruikers die toegang tot deze accounts hebben opgedaan kunnen gevoelige gegevens, zoals persoonlijk identificeerbare informatie (PII) lezen. U moet deze in overweging nemen wanneer u beslist of rol van beveiligingslezer in uw organisatie moeten worden beheerd met PIM.

#### <a name="azure-resource-roles"></a>Azure-resourcerollen

Wanneer u bepaalt welke roltoewijzingen moeten worden beheerd met behulp van PIM voor Azure-resource, moet u eerst de abonnementen/resources die het meest essentieel voor uw organisatie zijn identificeren. Voorbeelden van dergelijke abonnementen/resources zijn:

- Resources die als host de meest gevoelige gegevens fungeren
- Bronnen die core, klantgerichte toepassingen afhankelijk zijn van

Als u een globale beheerder bent met problemen bij het bepalen van welke abonnementen/resources het belangrijkst zijn, moet u contact opnemen voor eigenaars van abonnementen in uw organisatie voor het verzamelen van een lijst met bronnen die worden beheerd door elk abonnement. Vervolgens moet u samenwerken met de abonnementseigenaren aan de groep van de resources op basis van ernst in het geval dat ze zijn aangetast (laag, Gemiddeld, hoog). U moet de prioriteit van resources worden beheerd met PIM op basis van dit ernstniveau te geven.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u werkt met eigenaren van kritieke services voor het instellen van de PIM-werkstroom voor alle rollen binnen gevoelige abonnementen/resources/resourcegroep.

PIM voor Azure-resources biedt ondersteuning voor tijdsgebonden service-accounts. U moet behandelen serviceaccounts exact dezelfde manier waarop u zou een normaal gebruikersaccount behandelen.

Voor bronnen of abonnementen die niet als kritiek, hoeft u PIM instellen voor alle rollen. U moet echter nog steeds de rollen eigenaar en beheerder van gebruikerstoegang met PIM beveiligen.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u de eigenaar van rollen en functies van de Administrator voor gebruikerstoegang van alle abonnementen/resources met PIM beheren.

### <a name="decide-which-role-assignments-should-be-permanent-or-eligible"></a>Bepalen welke roltoewijzingen moet permanente of in aanmerking komende

Nadat u hebt besloten de lijst met rollen worden beheerd door PIM, moet u bepalen welke gebruikers de in aanmerking komende rol ten opzichte van de rol permanent actief moeten krijgen. Permanent actief rollen zijn de normale rollen toegewezen via Azure Active Directory en Azure-resources, terwijl in aanmerking komende rollen kunnen alleen worden toegewezen in PIM.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** hebt u de toewijzingen van nul permanent actief voor zowel Azure AD-rollen en functies van Azure-resource dan de aanbevolen [twee break om toegang in noodgevallen geldt](../users-groups-roles/directory-emergency-access.md), die moet hebben de permanente rol globale beheerder.

Hoewel u wordt aangeraden geen permanente beheerder, is het soms moeilijk voor organisaties om dit te realiseren meteen. Hier volgen aandachtspunten bij deze beslissing:

- Frequentie van de uitbreiding van bevoegdheden – als de gebruiker slechts één keer, de bevoegde toewijzing hoeft hebben ze al dan niet mogen de permanente toewijzing. Anderzijds, als de gebruiker de rol voor hun dagelijkse werk moet en met behulp van PIM, zouden hun productiviteit aanzienlijk beperkt, ze kunnen worden beschouwd als voor de permanente rol.
- Aanvragen die specifiek zijn voor uw organisatie – als de persoon die wordt gegeven van de in aanmerking komende rol afkomstig is van een zeer verafgelegen team of een Topfunctionarissen executive op het moment dat communicatie en afdwingen van het proces van uitbreiding van bevoegdheden is het lastig om deze kunnen worden overwogen voor de permanente rol.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u het instellen van terugkerende toegang beoordeelt voor gebruikers met permanente roltoewijzingen (moet u een hebt). Meer informatie over terugkerende toegangsbeoordeling in de laatste sectie van dit plan voor de implementatie

### <a name="draft-your-pim-settings"></a>Ontwerp van uw instellingen voor PIM

Voordat u de PIM-oplossing implementeert, is het raadzaam om de PIM-instellingen voor elke bevoorrechte rol die uw organisatie gebruikmaakt van concept. Deze sectie bevat enkele voorbeelden van PIM-instellingen voor bepaalde rollen (ze zijn alleen ter referentie en kunnen afwijken voor uw organisatie). Elk van deze instellingen wordt uitvoerig besproken met de aanbevelingen van Microsoft na de tabellen.

#### <a name="pim-settings-for-azure-ad-roles"></a>PIM-instellingen voor Azure AD-rollen

| Rol | MFA vereisen | Melding | Incident ticket | Goedkeuring vereisen | Goedkeurder | Duur van rolactivering | Permanent beheerder |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Globale beheerder | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Andere globale beheerders | 1 uur | Accounts voor toegang in noodgevallen |
| Exchange-beheerder | :heavy_check_mark: | :heavy_check_mark: | : x:. | : x:. | Geen | 2 uur | Geen |
| Helpdeskbeheerder | : x:. | : x:. | :heavy_check_mark: | : x:. | Geen | 8 uur | Geen |

#### <a name="pim-settings-for-azure-resource-roles"></a>PIM-instellingen voor Azure-resourcerollen

| Rol | MFA vereisen | Melding | Goedkeuring vereisen | Goedkeurder | Duur van rolactivering | Actieve beheerder | Actieve verlopen | In aanmerking komende verlopen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Eigenaar van kritieke abonnementen | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Andere eigenaren van het abonnement | 1 uur | Geen | N.v.t. | drie maanden |
| Administrator voor gebruikerstoegang van minder belangrijke abonnementen | :heavy_check_mark: | :heavy_check_mark: | : x:. | Geen | 1 uur | Geen | N.v.t. | drie maanden |
| Inzender voor virtuele machines | : x:. | :heavy_check_mark: | : x:. | Geen | 3 uur | Geen | N.v.t. | 6 maanden |

De volgende tabel beschrijft elke van de instellingen.

| Instelling | Description |
| --- | --- |
| Rol | Naam van de rol die u definieert de instellingen voor. |
| MFA vereisen | De in aanmerking komende gebruiker moet of MFA uitvoeren voordat u de rol activeert.<br/><br/> :heavy_check_mark: **Microsoft raadt aan** u MFA afdwingen voor alle beheerdersrollen, met name als de rollen gastgebruikers ook kunnen hebben. |
| Melding | Indien ingesteld op true, globale beheerder, beheerder met bevoorrechte rol en Beveiligingsbeheerder in de organisatie een e-mailmelding ontvangt wanneer een in aanmerking komende gebruiker de rol activeert.<br/><br/>**Opmerking:** Sommige organisaties geen een e-mailadres dat is gekoppeld aan de administrator-accounts, naar deze e-mailmeldingen hebt, moet u gaat u een alternatieve e-mailadres instellen, zodat beheerders deze e-mailberichten ontvangen. |
| Incident ticket | De in aanmerking komende gebruiker moet of om vast te leggen van een incident Ticketnummer bij het activeren van hun rol. Deze instelling kan een organisatie die bij elke activering met een interne incident nummer om ongewenste activeringen identificeren.<br/><br/> :heavy_check_mark: **Microsoft raadt aan** profiteren van een incident ticket getallen PIM verbinden met uw interne systeem. Dit is vooral handig voor het goedkeurders die context voor de activering nodig hebben. |
| Goedkeuring vereisen | Of moet de in aanmerking komende gebruiker om goedkeuring om de rol te activeren.<br/><br/> :heavy_check_mark: **Microsoft raadt aan** u goedkeuring voor rollen met machtiging voor de meeste instellen. Op basis van patronen in het gebruik van alle PIM-klanten, zijn globale beheerder, beheerder van de gebruiker, Exchange-beheerder, Beveiligingsbeheerder en wachtwoordbeheerder de meest voorkomende rollen met de installatie van de goedkeuring. |
| Goedkeurder | Als goedkeuring vereist voor het activeren van de in aanmerking komende rol, lijst van de mensen die de aanvraag goedkeuren is moet. PIM wordt standaard de fiatteur naar alle gebruikers die een beheerder met bevoorrechte rol zowel permanente als in aanmerking komende worden ingesteld.<br/><br/>**Opmerking:** Als een gebruiker zowel in aanmerking voor een Azure AD-rol en een goedkeurder van de rol is, is ze niet mogelijk om goed te keuren zelf.<br/><br/> :heavy_check_mark: **Microsoft raadt aan** goedkeurders zijn personen die het meest bekend over de specifieke rol en de frequente gebruikers in plaats van een globale beheerder bent te kiezen. |
| Duur van rolactivering | De hoeveelheid tijd die een gebruiker worden geactiveerd in de rol voordat deze verloopt. |
| Permanent beheerder | Lijst met gebruikers die een permanente beheerder is voor de rol (nooit meer hoeft te activeren).<br/><br/> :heavy_check_mark: **Microsoft raadt aan** u hebt geen permanente beheerder voor alle rollen, met uitzondering van globale beheerders. Lees meer over in de die moeten worden aangebracht in aanmerking komende en die permanent actief sectie van dit plan moet zijn. |
| Actieve beheerder | Actieve beheerder is voor Azure-resources, de lijst met gebruikers die nooit meer hoeft wordt te activeren voor het gebruik van de rol. Dit wordt niet genoemd, zoals in Azure AD-rollen permanente beheerder omdat u een verlooptijd voor de gebruiker wanneer deze rol verliest kunt instellen. |
| Actieve verlopen | Een actieve roltoewijzing voor Azure-resourcerollen verloopt na deze periode geconfigureerd. U kunt kiezen uit 15 dagen, 1 maand, drie maanden, 6 maanden, 1 jaar of permanent actief. |
| In aanmerking komende verlopen | Een in aanmerking komende roltoewijzing voor Azure-resourcerollen verloopt na deze periode geconfigureerd. U kunt kiezen uit 15 dagen, 1 maand, drie maanden, 6 maanden, 1 jaar permanent in aanmerking komen. |

## <a name="step-3-implement-your-solution"></a>Stap 3. Uw oplossing implementeren

De basis van de juiste planning vormt de basis waarop u een toepassing is met Azure Active Directory kunt implementeren.  Het biedt intelligent beveiligings- en integratie van onboarding vereenvoudigt tijdens de tijd voor geslaagde implementaties.  Deze combinatie zorgt ervoor dat uw toepassing is geïntegreerd met gemak bij het oplossen van uitvaltijd voor uw eindgebruikers.

### <a name="identify-test-users"></a>Testgebruikers identificeren

Gebruik deze sectie voor het identificeren van een set van gebruikers en groepen van gebruikers voor het valideren van de implementatie. Op basis van de instellingen die u hebt geselecteerd in de sectie planning, identificatie van de gebruikers die u wilt testen voor elke rol.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u service-eigenaars van elke Azure AD-rol moet de testgebruikers zodat ze kunnen met het proces raken en worden van een interne advocator voor de implementatie.

In deze tabel is de testgebruikers controleren of de instellingen voor elke rol werkt te identificeren.

| Rolnaam | Gebruikers testen |
| --- | --- |
| &lt;Rolnaam&gt; | &lt;Gebruikers voor het testen van de rol&gt; |
| &lt;Rolnaam&gt; | &lt;Gebruikers voor het testen van de rol&gt; |

### <a name="test-implementation"></a>Test-implementatie

Nu dat u hebt vastgesteld dat de testgebruikers, gebruikt u deze stap PIM configureren voor uw testgebruikers. Als uw organisatie wil PIM-werkstroom opnemen in uw eigen interne toepassing in plaats van de PIM-gebruikersinterface in Azure portal, de bewerkingen in PIM worden ook ondersteund via onze [graph API](https://docs.microsoft.com/graph/api/resources/privilegedidentitymanagement-root).

#### <a name="configure-pim-for-azure-ad-roles"></a>PIM voor Azure AD-rollen configureren

1. [Configureer de instellingen van de rol van Azure AD](pim-how-to-change-default-settings.md) op basis van wat u van plan.

1. Navigeer naar **Azure AD-rollen**, klikt u op **rollen**, en selecteer vervolgens de rol die u zojuist hebt geconfigureerd.

1. Voor de groep testgebruikers, als ze al een permanente beheerder bent, kunt u ze in aanmerking komende door ze zoeken en deze te converteren van permanente in aanmerking komende door te klikken op de drie punten op de rij. Als ze niet beschikken over de roltoewijzingen hebt, u kunt [maken van een nieuwe in aanmerking komende toewijzing](pim-how-to-add-role-to-user.md#make-a-user-eligible-for-a-role).

1. Herhaal stappen 1-3 voor alle functies die u wilt testen.

1. Nadat u de testgebruikers hebt ingesteld, u moet verzend de koppeling voor informatie over het [hun Azure AD-rol activeren](pim-how-to-activate-role.md).

#### <a name="configure-pim-for-azure-resource-roles"></a>PIM configureren voor Azure-resourcerollen

1. [Configureer de instellingen van de rol van Azure-resource](pim-resource-roles-configure-role-settings.md) voor een rol in een abonnement of een resource die u wilt testen.

1. Navigeer naar **Azure-resources** voor dit abonnement en klik op **rollen**, selecteer de rol die u zojuist hebt geconfigureerd.

1. Voor de groep testgebruikers, als ze al een actieve beheerder bent, kunt u ze in aanmerking komende door te zoeken naar deze en [hun roltoewijzing bijwerken](pim-resource-roles-assign-roles.md#update-or-remove-an-existing-role-assignment). Als ze zich niet de rol hoeven hebt, u kunt [een nieuwe rol toewijzen](pim-resource-roles-assign-roles.md#assign-a-role).

1. Herhaal stappen 1-3 voor alle functies die u wilt testen.

1. Nadat u de testgebruikers hebt ingesteld, u moet verzend de koppeling voor informatie over het [hun Azure-resource-rol activeren](pim-resource-roles-activate-your-roles.md).

Deze fase moet u controleren dat of de configuratie die u heeft ingesteld voor de rollen die goed werkt. Gebruik de volgende tabel om te documenteren van uw tests uit. U moet deze fase ook gebruiken voor het optimaliseren van de communicatie met betrokken gebruikers.

| Rol | Normaal tijdens de activering | De werkelijke resultaten |
| --- | --- | --- |
| Globale beheerder | (1) MFA vereisen<br/>(2) goedkeuring vereisen<br/>(3) fiatteur melding ontvangt en kan goedkeuren<br/>(4) rol verloopt na vooraf ingestelde tijd |  |
| Eigenaar van het abonnement *X* | (1) MFA vereisen<br/>(2) in aanmerking komende toewijzing verloopt na de geconfigureerde periode |  |

### <a name="communicate-pim-to-affected-stakeholders"></a>PIM betrokken belanghebbenden communiceren

PIM implementeren vindt aanvullende stappen voor gebruikers van bevoorrechte rollen. Hoewel beveiligingsproblemen die zijn gekoppeld aan bevoorrechte identiteiten aanzienlijk wordt beperkt door PIM, moet de wijziging effectief worden gecommuniceerd voordat u de tenant-brede implementatie. Afhankelijk van het aantal betrokken beheerders kiezen organisaties vaak te maken van een intern document, een video of een e-mailbericht van de wijziging. Vaak is opgenomen in deze berichten opnemen:

- Wat is PIM
- Wat is het voordeel voor de organisatie
- Die worden beïnvloed
- Wanneer PIM worden bekendgemaakt
- Extra stappen zijn vereist voor gebruikers om hun rol te activeren
    - U moet de koppelingen naar onze documentatie verzenden:
    - [Azure AD-rollen activeren](pim-how-to-activate-role.md)
    - [Azure-resource-rollen activeren](pim-resource-roles-activate-your-roles.md)
- Contactgegevens of helpdeskkoppeling voor eventuele problemen die zijn gekoppeld aan PIM

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u tijd met uw helpdesk/ondersteuningsteam instellen om te zien hoe ze de PIM-werkstroom (als uw organisatie een interne IT-ondersteuning heeft team). Geef ze de juiste documentatie, evenals uw contactgegevens.

### <a name="move-to-production"></a>Verplaatsen naar productie

Zodra de test voltooid en geslaagd is, verplaatst u PIM naar productie door te herhalen van alle de stappen in de testfase voor alle gebruikers van elke rol die u hebt gedefinieerd in de PIM-configuratie. Voor PIM voor Azure AD-rollen, organisaties vaak test en implementeer PIM voor globale beheerders voor testen en PIM uitrollen voor andere rollen. In de tussentijd zorgen voor Azure-resource, organisaties normaal test en implementeer PIM één Azure-abonnement op een tijdstip.

### <a name="in-the-case-a-rollback-is-needed"></a>In het geval is een terugdraaiactie vereist

Als PIM is mislukt om te werken naar wens in de productieomgeving, kunnen de volgende stappen uit voor terugdraaien u helpen bij het terugkeren naar een bekende goede status voordat u PIM instelt:

#### <a name="azure-ad-roles"></a>Azure AD-rollen

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Open **Azure AD Privileged Identity Management**.
1. Klik op **Azure AD-rollen** en klik vervolgens op **rollen**.
1. Voor elke rol die u hebt geconfigureerd, klikt u op het weglatingsteken (**...** ) voor alle gebruikers met een in aanmerking komende toewijzing.
1. Klik op de **permanent maken** optie de roltoewijzing om permanent te maken.

#### <a name="azure-resource-roles"></a>Azure-resourcerollen

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Open **Azure AD Privileged Identity Management**.
1. Klik op **Azure-resources** en klik vervolgens op een abonnement of een resource die u wilt terugdraaien.
1. Klik op **rollen**.
1. Voor elke rol die u hebt geconfigureerd, klikt u op het weglatingsteken (**...** ) voor alle gebruikers met een in aanmerking komende toewijzing.
1. Klik op de **permanent maken** optie de roltoewijzing om permanent te maken.

## <a name="step-4-next-steps-after-deploying-pim"></a>Stap 4. Vervolgstappen na de implementatie van PIM

PIM is in productie implementeert, is een belangrijke stap voorwaarts wat betreft het beveiligen van uw organisatie de bevoegde identiteiten. Met de implementatie van PIM komt extra PIM-functies die u voor beveiliging en naleving gebruiken moet.

### <a name="use-pim-alerts-to-safeguard-your-privileged-access"></a>PIM-waarschuwingen gebruikt ter bescherming van uw privileged access

U moet de PIM ingebouwde waarschuwingen functionaliteit voor betere beveiliging gebruikmaken van uw tenant. Zie voor meer informatie, [beveiligingswaarschuwingen](pim-how-to-configure-security-alerts.md#security-alerts). Deze waarschuwingen bevatten: bevoorrechte rollen wordt niet gebruikt voor beheerders, rollen worden toegewezen buiten PIM, rollen te vaak worden geactiveerd en nog veel meer. Om volledig te beveiligen van uw organisatie, moet u regelmatig via uw lijst met waarschuwingen en los de problemen. U kunt weergeven en oplossen van uw waarschuwingen de volgende manier:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Open **Azure AD Privileged Identity Management**.
1. Klik op **Azure AD-rollen** en klik vervolgens op **waarschuwingen**.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u omgaat met alle waarschuwingen die zijn gemarkeerd met hoge urgentie onmiddellijk. Voor waarschuwingen met gemiddeld en laag urgentie, moet u op de hoogte blijven en wijzigingen aanbrengen, als u denkt dat er een beveiligingsrisico.

Als een van de specifieke waarschuwingen niet handig of niet voor uw organisatie geldt, kunt u altijd de waarschuwing op de pagina met waarschuwingen sluiten. Deze ontslag verderop in de pagina Azure AD-instellingen kunt u altijd terugkeren.

### <a name="set-up-recurring-access-reviews-to-regularly-audit-your-organizations-privileged-identities"></a>Terugkerende toegangsbeoordelingen instellen voor regelmatige controle van bevoorrechte identiteiten van uw organisatie

Toegangsbeoordelingen zijn de beste manier om aan te vragen van gebruikers die zijn toegewezen met bevoorrechte rollen of specifieke revisoren of elke gebruiker moet de privileged identity. Toegangsbeoordelingen zijn ideaal als u wilt verminderen van kwetsbaarheid voor aanvallen en dit blijft voldoen aan het beleid. Zie voor meer informatie over het starten van een toegangscontrole [toegangsbeoordelingen voor Azure AD-rollen](pim-how-to-start-security-review.md) en [toegangsbeoordelingen voor Azure-resourcerollen](pim-resource-roles-start-access-review.md). Voor sommige organisaties voeren periodieke toegangsbeoordeling is vereist om te blijven voldoen aan wetten en voorschriften terwijl voor andere, toegangsbeoordeling is de beste manier om af te dwingen de principal van minimale bevoegdheden binnen uw organisatie.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** instellen van elk kwartaal toegangsbeoordelingen voor uw Azure AD en Azure-resource-rollen.

In de meeste gevallen is de revisor voor Azure AD-rollen de gebruikers zelf terwijl de revisor voor Azure-resourcerollen de eigenaar van het abonnement, dat de rol is is in. Het is echter vaak het geval waarbij bedrijven hebben van beschermde accounts die niet zijn gekoppeld met een bepaalde persoon e-mailadres. In deze gevallen niemand leest en de toegang beoordeelt.

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u een secundaire e-mailadres voor alle accounts met toewijzingen van bevoorrechte rollen die niet zijn gekoppeld aan een regelmatig gemarkeerde e-mailadres toevoegen

### <a name="get-the-most-out-of-your-audit-log-to-improve-security-and-compliance"></a>Optimaal te profiteren van uw auditlogboek ophalen voor het verbeteren van beveiliging en naleving

Het controlelogboek is de plek waar u kunt op de hoogte blijven en moeten voldoen aan regelgeving. PIM wordt momenteel opgeslagen voor een 30-daagse geschiedenis van de geschiedenis van uw organisatie in de audit log inclusief:

- Activeren/deactiveren van in aanmerking komende rollen
- Rol toewijzing activiteiten binnen en buiten PIM
- Wijzigingen in de rolinstellingen
- Activiteiten voor rolactivering met Goedkeuringsinstellingen aanvraag/goedkeuren/weigeren
- Op waarschuwingen bijwerken

U hebt toegang tot deze auditlogboeken als u een globale beheerder of een beheerder met bevoorrechte rol. Zie voor meer informatie, [controlegeschiedenis voor Azure AD-rollen](pim-how-to-use-audit-log.md) en [controlegeschiedenis voor Azure-resourcerollen](azure-pim-resource-rbac.md).

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u hebt ten minste één beheerder lezen door alle gebeurtenissen wekelijks controleren en exporteren van uw controlegebeurtenissen op maandbasis.

Als u wilt voor het automatisch opslaan van de controlegebeurtenissen voor een langere periode, wordt de PIM-controlelogboek automatisch gesynchroniseerd naar de [Azure AD-auditlogboeken](../reports-monitoring/concept-audit-logs.md).

> [!TIP]
> :heavy_check_mark: **Microsoft raadt aan** u om in te stellen [Azure log bewaking](../reports-monitoring/concept-activity-logs-azure-monitor.md) voor het archiveren van gebeurtenissen voor beveiligingscontrole in Azure storage-account voor de behoeften van de beveiliging en naleving.
