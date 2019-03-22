---
title: Begrijpen en gebruiken met een bereik van Azure Cost Management | Microsoft Docs
description: In dit artikel krijgt u inzicht in facturering en resource management bereiken die beschikbaar zijn in Azure en het gebruik van de bereiken in Cost Management en API's.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/13/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 4e7956e8873b552fcd73c51a51f51d99f21af324
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58002931"
---
# <a name="understand-and-work-with-scopes"></a>Bereiken begrijpen en gebruiken

In dit artikel krijgt u inzicht in facturering en resource management bereiken die beschikbaar zijn in Azure en het gebruik van de bereiken in Cost Management en API's.

## <a name="scopes"></a>Bereiken

Een _bereik_ is een knooppunt in de hiërarchie van de Azure-resource waar Azure AD-gebruikers toegang tot en beheer van services. De meeste Azure-resources worden gemaakt en geïmplementeerd in resourcegroepen, die deel van abonnementen uitmaken. Microsoft biedt ook twee hiërarchieën boven Azure-abonnementen die specifieke functies voor het beheren van factureringsgegevens hebt:
- Factureringsgegevens, zoals betalingen en facturen
- Cloudservices, zoals kosten en beleid voor beheer

Bereiken zijn waar u factureringsgegevens beheren, hebt van functies die specifiek zijn voor betalingen, facturen weergeven en uitvoeren van algemene accountbeheer. Factuur- en -functies afzonderlijk worden beheerd die worden gebruikt voor het resourcebeheer van de waarmee [Azure RBAC](../role-based-access-control/overview.md). Als u wilt duidelijk onderscheid maken de bedoeling van de afzonderlijke bereiken, met inbegrip van de access control verschillen, deze worden aangeduid als _scopes facturering_ en _RBAC scopes_, respectievelijk.

## <a name="how-cost-management-uses-scopes"></a>Hoe Cost Management maakt gebruik van bereiken

Cost Management werkt scopes in alle bovenstaande resources waarmee organisaties voor het beheren van kosten op het niveau waarop ze toegang hebben of die de hele factureringsaccount of één resourcegroep bestaan. Hoewel facturering scopes verschillen op basis van uw Microsoft-overeenkomst (abonnementstype), niet de RBAC-bereiken.

## <a name="azure-rbac-scopes"></a>Azure RBAC-bereiken

Azure ondersteunt drie bereiken voor resourcebeheer. Elk bereik biedt ondersteuning voor beheren van toegang en beheer, met inbegrip van maar niet beperkt tot, kostenbeheer.

- [**Beheergroepen** ](../governance/management-groups/index.md) -hiërarchische containers, maximaal acht niveaus, voor het ordenen van Azure-abonnementen.

    Resourcetype: [Microsoft.Management/managementGroups](/rest/api/resources/managementgroups)

- **Abonnementen** -primaire containers voor Azure-resources.

    Resourcetype: [Microsoft.Resources/subscriptions](/rest/api/resources/subscriptions)

- [**Resourcegroepen** ](../azure-resource-manager/resource-group-overview.md#resource-groups) -logische groeperingen van gerelateerde resources voor een Azure-oplossing die de dezelfde levenscyclus delen. Bijvoorbeeld resources die zijn geïmplementeerd en verwijderd samen.

    Resourcetype: [Microsoft.Resources/subscriptions/resourceGroups](/rest/api/resources/resourcegroups)

Van beheergroepen kunnen u abonnementen in een hiërarchie te organiseren. U kunt bijvoorbeeld de hiërarchie van een logische organisatie met behulp van beheergroepen maken. Geef teams abonnementen voor productie en werkbelastingen voor ontwikkelen/testen. En maak vervolgens de resourcegroepen in de abonnementen voor het beheren van elk subsysteem of het onderdeel.

Het maken van dat een organisatie-hiërarchie kan kosten en naleving van het beleid totaliseren aan. Vervolgens kunt elke leider weergeven en analyseren van hun huidige kosten. En vervolgens deze budgetten perken ongeldige bestedingslimiet patronen en kosten optimaliseert met aanbevelingen van Advisor op het laagste niveau kunnen maken.

Verlenen van toegang tot kosten weergeven en beheren (optioneel) kosten-configuratie, zoals budgetten en uitvoer, wordt uitgevoerd op governance bereiken met behulp van Azure RBAC. U Azure RBAC gebruiken om te verlenen van Azure AD-gebruikers en groepen toegang om uit te voeren van een vooraf gedefinieerde set acties die zijn gedefinieerd in een rol op een bepaald bereik en lager. Een rol die is toegewezen aan een bereik van de groep management verleent bijvoorbeeld ook dezelfde machtigingen voor geneste abonnementen en resourcegroepen.

Kostenbeheer ondersteunt de volgende ingebouwde rollen voor elk van de volgende bereiken:

- [**De eigenaar van** ](../role-based-access-control/built-in-roles.md#owner) : kunt u kosten wilt weergeven en alles beheren, inclusief configuratie van de kosten.
- [**Inzender** ](../role-based-access-control/built-in-roles.md#contributor) : kunt u kosten wilt weergeven en alles beheren, inclusief configuratie van de kosten, maar met uitzondering van toegangsbeheer.
- [**Lezer** ](../role-based-access-control/built-in-roles.md#reader) : alles, inclusief de voor kostengegevens en configuratie, kunt bekijken, maar u kunt geen wijzigingen aanbrengen.
- [**Inzender voor versiebeheer voor kosten** ](../role-based-access-control/built-in-roles.md#cost-management-contributor) : kunt u kosten wilt weergeven en configuratie van de kosten beheren.
- [**Kosten Management lezer** ](../role-based-access-control/built-in-roles.md#cost-management-reader) : gegevens van cost en configuratie kunt bekijken.

Cost Management inzenders is de aanbevolen minimale bevoegdheden-rol. Deze personen toegang maken en beheren van budgetten en de uitvoer naar meer effectief bewaken en rapporteren van kosten. Management kostenposten mogelijk ook aanvullende functies voor de ondersteuning van scenario's voor het beheer van kosten voor end-to-end. Houd rekening met de volgende scenario's:

- **Handelen wanneer budgetten worden overschreden** – kostenposten Management moet ook toegang tot het maken en/of beheren van actiegroepen om automatisch reageren op overschrijdingen. Houd rekening met het verlenen van [bewaking Inzender](../role-based-access-control/built-in-roles.md#monitoring-contributor) naar een resourcegroep met de actiegroep voor het gebruik van wanneer budgetdrempelwaarden worden overschreden. Specifieke acties automatiseren vereist aanvullende functies voor de specifieke services gebruikt, zoals Automation en Azure Functions.
- **Schema exporteren van gegevens kosten** – kostenposten Management moet ook toegang tot het beheer van opslagaccounts voor het plannen van een export gegevens te kopiëren naar een opslagaccount. Houd rekening met het verlenen van [Inzender voor Opslagaccounts](../role-based-access-control/built-in-roles.md#storage-account-contributor) aan een resourcegroep met de opslag account waar kostengegevens wordt geëxporteerd.
- **Kostenbesparend aanbevelingen weergeven** – Cost Management lezers en medewerkers hebt geen toegang tot aanbevelingen standaard. Toegang tot aanbevelingen moet leestoegang tot afzonderlijke resources. Houd rekening met het verlenen van [lezer](../role-based-access-control/built-in-roles.md#reader) of een [servicespecifieke rollen](../role-based-access-control/built-in-roles.md#built-in-role-descriptions).

## <a name="enterprise-agreement-scopes"></a>Enterprise Agreement-bereiken

Enterprise Agreement (EA)-facturering accounts, ook wel genoemd inschrijvingen, hebben de volgende bereiken:

- [**Factureringsaccount** ](../billing/billing-view-all-accounts.md) -Hiermee geeft u een EA-inschrijving. Facturen worden gegenereerd op dit bereik. Aankopen die geen gebruik zijn gebaseerd, zoals Marketplace en reserveringen, zijn alleen beschikbaar op dit bereik. Ze worden niet weergegeven in de afdelingen of inschrijvingsaccounts.

    Resourcetype: `Microsoft.Billing/billingAccounts (accountType = Enrollment)`
- **Afdeling** - optioneel groepering van inschrijvingsaccounts.

    Resourcetype: `Billing/billingAccounts/departments`

- **Inschrijvingsaccount** -Hiermee geeft u de eigenaar van een één-account. Biedt geen ondersteuning voor toegang verlenen tot meerdere personen.

    Resourcetype: `Microsoft.Billing/billingAccounts/enrollmentAccounts`

Hoewel governance bereiken zijn gebonden aan één map, niet EA facturering bereiken. Een EA-factureringsaccount mogelijk abonnementen naar een willekeurig aantal Azure AD-mappen.

De volgende rollen bieden ondersteuning voor facturering EA-scopes:

- **Ondernemingsadministrator** : facturering accountinstellingen en toegang kunt beheren, kunt u alle kosten weergeven en configuratie van de kosten kunt beheren. Bijvoorbeeld, budgetten en exporteert. In de functie, de EA facturering bereik is gelijk aan [Cost Management Inzender voor Azure RBAC-rol](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Enterprise alleen-lezen-gebruiker** – facturering accountinstellingen, gegevens van cost en configuratie van de kosten kunt weergeven. Bijvoorbeeld, budgetten en exporteert. In de functie, de EA facturering bereik is gelijk aan de [Cost Management lezer Azure RBAC-rol](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **Beheerder van de afdeling** : kunt afdeling instellingen, zoals kostenplaats, beheren en kan toegang tot alle kosten weergeven en configuratie van de kosten beheren. Bijvoorbeeld, budgetten en exporteert.  De **DA weergave kosten** facturering van de instellingen van een account voor afdeling beheerders en alleen-lezen gebruikers moet zijn ingeschakeld om te zien van kosten. Als **DA weergave kosten** is uitgeschakeld, afdeling gebruikers niet zien kosten op elk niveau, zelfs als ze de eigenaar van een account of -abonnement.
- **Afdeling alleen-lezengebruiker** – afdeling instellingen, kosten en de configuratie van de kosten kunt bekijken. Bijvoorbeeld, budgetten en exporteert. Als **DA weergave kosten** is uitgeschakeld, afdeling gebruikers niet zien kosten op elk niveau, zelfs als ze de eigenaar van een account of -abonnement.
- **De eigenaar van account** : kan account inschrijvingsinstellingen (zoals kostenplaats) beheren, alle kosten weergeven en beheren van configuratie van de kosten (zoals budgetten en uitvoer) voor het inschrijvingsaccount. De **kosten weergeven die door de AO** facturering van de instellingen van een account voor eigenaars en gebruikers van RBAC moet zijn ingeschakeld om te zien van kosten.

EA facturering accountgebruikers hebben geen directe toegang tot facturen. Facturen zijn verkrijgbaar via een externe volume licensing-systeem.

Azure-abonnementen worden genest onder inschrijvingsaccounts. Facturering-gebruikers hebben toegang tot gegevens van cost voor de abonnementen en resourcegroepen die onder hun respectievelijke bereiken. Ze geen toegang om te bekijken of beheren van resources in Azure portal. Facturering-gebruikers kunnen kosten weergeven door te navigeren naar **kostenbeheer en facturering** in de Azure portal lijst met services. Vervolgens kunnen ze kosten voor de specifieke abonnementen en resourcegroepen die ze nodig hebben om te rapporteren over filteren.

Facturering-gebruikers hebben geen toegang tot van beheergroepen omdat ze expliciet niet onder een specifieke factureringsaccount vallen. Toegang moet expliciet worden verleend aan beheergroepen. -Beheergroepen totaliseren kosten van alle geneste abonnementen. Echter, zijn ze alleen aankopen op basis van gebruik. Ze omvatten geen aankopen zoals reserveringen en van derden Marketplace-aanbiedingen. Als u deze kosten, facturering EA-account te gebruiken.

## <a name="individual-agreement-pay-as-you-go-scopes"></a>Afzonderlijke overeenkomst (betalen naar gebruik) bereiken

Abonnementen voor betalen per gebruik (betalen per gebruik), inclusief het gerelateerde typen, zoals gratis of proeflaag en dev/test-aanbiedingen, hoeft een expliciete facturering accountbereik. Elk abonnement heeft in plaats daarvan een accounteigenaar of beheerder van het account, net als de eigenaar van het EA-account.

- [**Factureringsaccount** ](../billing/billing-view-all-accounts.md) -Hiermee geeft u de accounteigenaar van een enkel voor een of meer Azure-abonnementen. Het ondersteunt momenteel geen toegang verlenen tot meerdere personen of toegang tot samengevoegde kosten weergaven.

    Resourcetype: Niet van toepassing

Betalen per gebruik-account abonnementsbeheerders kunt weergeven en beheren van facturering gegevens, zoals facturen en betalingen van de [Azure-Accountcentrum](https://account.azure.com/subscriptions). Ze kunnen echter kostengegevens weergeven of beheren van resources in Azure portal. Om toegang tot de accountbeheerder, gebruikt u de Cost Management-functies die eerder is vermeld.

In tegenstelling tot EA ziet de betalen per gebruik-account abonnementsbeheerders hun facturen in Azure portal. Houd er rekening mee dat Cost Management lezer en Inzender voor versiebeheer voor kosten rollen niet worden gebruikt voor het bieden van toegang tot facturen. Zie voor meer informatie, [over het verkrijgen van toegang tot PAYG facturen](../billing/billing-manage-access.md#give-access-to-billing).

## <a name="customer-agreement-scopes"></a>Klant overeenkomst bereiken

Facturering KLANTOVEREENKOMST van Microsoft-accounts hebben de volgende bereiken:

- **Factureringsaccount** -Hiermee geeft u een KLANTOVEREENKOMST voor meerdere Microsoft-producten en services. Facturering klantaccounts overeenkomst zijn niet functioneel gelijk zijn aan de EA-inschrijvingen. EA-inschrijvingen zijn beter aansluiten op facturering profielen uitgelijnd.

    Resourcetype: `Microsoft.Billing/billingAccounts (accountType = Organization)`

- **Facturering profiel** -definieert de abonnementen die zijn opgenomen in een factuur. Facturering-profielen zijn het functioneel equivalent van een EA-inschrijving, omdat dat het bereik die facturen worden gegenereerd op. Op deze manier-aankopen die niet op gebruik gebaseerde (zoals Marketplace en reserveringen) zijn alleen beschikbaar op dit bereik. Ze worden niet opgenomen in de secties van de factuur.

    Resourcetype: `Microsoft.Billing/billingAccounts/billingProfiles`

- **Sectie factuur** -vertegenwoordigt een groep van abonnementen in een factuur of een profiel voor facturering. Factuur secties zijn, zoals afdelingen: meerdere mensen toegang hebt tot een gedeelte van de factuur.

    Resourcetype: `Microsoft.Billing/billingAccounts/invoiceSections`

In tegenstelling tot EA facturering scopes, KLANTOVEREENKOMST facturering accounts _zijn_ gekoppeld aan één map en geen abonnementen over meerdere Azure AD-mappen.

Klant overeenkomst facturering bereiken ondersteunen de volgende rollen:

- **De eigenaar van** : kan instellingen voor facturering en toegang beheren, alle kosten weergeven en beheren van configuratie van de kosten. Bijvoorbeeld, budgetten en exporteert. In de functie, deze KLANTOVEREENKOMST facturering bereik is hetzelfde als de [Cost Management Inzender voor Azure RBAC-rol](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Inzender** – kunt facturering beheren behalve toegang, alle kosten weergeven en beheren van configuratie van de kosten. Bijvoorbeeld, budgetten en exporteert. In de functie, deze KLANTOVEREENKOMST facturering bereik is hetzelfde als de [Cost Management Inzender voor Azure RBAC-rol](../role-based-access-control/built-in-roles.md#cost-management-contributor).
- **Lezer** – instellingen voor facturering, gegevens van cost en configuratie van de kosten kunt weergeven. Bijvoorbeeld, budgetten en exporteert. In de functie, deze KLANTOVEREENKOMST facturering bereik is hetzelfde als de [Cost Management lezer Azure RBAC-rol](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **Factuurbeheer** : kunt weergeven en facturen en kunnen gegevens en configuratie weergeven. Bijvoorbeeld, budgetten en exporteert. In de functie, deze KLANTOVEREENKOMST facturering bereik is hetzelfde als de [Cost Management lezer Azure RBAC-rol](../role-based-access-control/built-in-roles.md#cost-management-reader).
- **De maker van de Azure-abonnement** – kunt maken van Azure-abonnementen, kosten weergeven en beheren van kosten configuratie. Bijvoorbeeld, budgetten en exporteert. In de functie is dit KLANTOVEREENKOMST facturering bereik gelijk zijn aan de eigenaarsrol van EA-inschrijving-account.

Azure-abonnementen worden genest onder factuur secties, zoals hoe ze onder EA-inschrijvingsaccounts zijn. Facturering-gebruikers hebben toegang tot gegevens van cost voor de abonnementen en resourcegroepen die onder hun respectievelijke bereiken. Ze hebben echter geen toegang om te bekijken of beheren van resources in Azure portal. Facturering-gebruikers kunnen kosten weergeven door te navigeren naar **kostenbeheer en facturering** in de Azure portal lijst met services. Kosten voor de specifieke abonnementen en resourcegroepen die ze nodig hebben om te rapporteren over vervolgens filteren.

Facturering-gebruikers hebben geen toegang tot van beheergroepen omdat ze expliciet niet onder het account van de facturering vallen. Echter wanneer beheergroepen zijn ingeschakeld voor de organisatie, abonnementskosten voor alle worden samengevouwen aan de factureringsrekening en aan de beheergroep hoofdmap omdat ze beide tot één map beperkt zijn. Beheergroepen zijn alleen van aankopen op basis van gebruik. Aankopen, zoals reserveringen en van derden Marketplace-aankopen worden niet opgenomen in beheergroepen. De facturering-account en de hoofdmap beheergroep kan dus verschillende totalen melden. Als u deze kosten, de factureringsaccount of de respectieve factureringsprofiel te gebruiken.

## <a name="cloud-solution-provider-csp-scopes"></a>Cloud Solution Provider (CSP) bereiken

Cloud Solution Provider (CSP) partners worden niet ondersteund in Cost Management vandaag nog. In plaats daarvan kunt u [Partner Center](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview).

## <a name="switch-between-scopes-in-cost-management"></a>Schakelen tussen bereiken in Cost Management

Alle weergaven van Cost Management in Azure portal zijn onder meer een **bereik** pill in de linkerbovenhoek van de weergave. Gebruik dit voor het snel bereik wijzigen. Klik op de **bereik** pill openen de bereikskiezer voor. U ziet facturering accounts, de root management-groep en abonnementen die niet worden genest onder het root-beheergroep. Als u wilt een bereik selecteert, klikt u op de achtergrond Markeer deze en klik vervolgens op **Selecteer** aan de onderkant. Als u wilt inzoomen in op de geneste bereiken, zoals resourcegroepen in een abonnement, klikt u op de koppeling van de naam van bereik. Klik om te selecteren in het bovenliggende bereik op het niveau van alle geneste, **Selecteer deze optie &lt;bereik&gt;**  aan de bovenkant van de bereikskiezer voor.

## <a name="identify-the-resource-id-for-a-scope"></a>Identificeren van de resource-ID voor een bereik

Als u werkt met Cost Management-API's, weet dat het bereik is van essentieel belang. Gebruik de volgende informatie om het juiste bereik URI voor Cost Management-API's.

### <a name="billing-accounts"></a>Factureringsaccounts

1. Open de Azure-portal en navigeer vervolgens naar **kostenbeheer en facturering** in de lijst met services.
2. Selecteer **eigenschappen** in de facturering menu van het account.
3. Kopieer de facturering account-ID.
4. Uw bereik is: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}"`

### <a name="billing-profiles"></a>Factureringsprofielen

1. Open de Azure-portal en navigeer vervolgens naar **kostenbeheer en facturering** in de lijst met services.
2. Selecteer **facturering profielen** in de facturering menu van het account.
3. Klik op de naam van het gewenste profiel voor facturering.
4. Selecteer **eigenschappen** in het menu met facturering profiel.
5. Kopieer de factureringsaccount en facturering profiel-id.
6. Uw bereik is: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}"`

### <a name="invoice-sections"></a>Factuursecties

1. Open de Azure-portal en navigeer vervolgens naar **kostenbeheer en facturering** in de lijst met services.
2. Selecteer **factuur secties** in de facturering menu van het account.
3. Klik op de naam van de gewenste factuur-sectie.
4. Selecteer **eigenschappen** in het menu van de sectie factuur.
5. Kopieer de factureringsaccount en sectie id's factureren.
6. Uw bereik is: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/invoiceSections/{invoiceSectionId}"`

### <a name="ea-departments"></a>EA-afdelingen

1. Open de Azure-portal en navigeer vervolgens naar **kostenbeheer en facturering** in de lijst met services.
2. Selecteer **afdelingen** in de facturering menu van het account.
3. Klik op de naam van de gewenste afdeling.
4. Selecteer **eigenschappen** in het menu van de afdeling.
5. Kopieer de facturering-ID-account en afdeling.
6. Uw bereik is: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/departments/{departmentId}"`

### <a name="ea-enrollment-account"></a>Account voor EA-inschrijving

1. Open de Azure-portal en Ga naar **kostenbeheer en facturering** in de lijst met services.
2. Selecteer **inschrijvingsaccounts** in de facturering menu van het account.
3. Klik op de naam van de gewenste inschrijvingsaccount.
4. Selecteer **eigenschappen** in het menu voor het account van inschrijving.
5. Kopieer het factureringsaccount en id's voor apparaatregistratie-account.
6. Uw bereik is: `"/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/enrollmentAccounts/{enrollmentAccountId}"`

### <a name="management-group"></a>Beheergroep

1. Open de Azure-portal en Ga naar **beheergroepen** in de lijst met services.
2. Navigeer naar de gewenste beheergroep.
3. Kopieer de beheergroep-ID van de tabel.
4. Uw bereik is: `"/providers/Microsoft.Management/managementGroups/{id}"`

### <a name="subscription"></a>Abonnement

1. Open de Azure-portal en Ga naar **abonnementen** in de lijst met services.
2. Kopieer de abonnements-ID van de tabel.
3. Uw bereik is: `"/subscriptions/{id}"`

### <a name="resource-groups"></a>Resourcegroepen

1. Open de Azure-portal en Ga naar **resourcegroepen** in de lijst met services.
2. Klik op de naam van de gewenste resourcegroep.
3. Selecteer **eigenschappen** in het menu resourcegroep.
4. Kopieer de waarde van het resource-ID-veld.
5. Uw bereik is: `"/subscriptions/{id}/resourceGroups/{name}"`

Cost Management is momenteel niet ondersteund in [Azure Global](https://management.azure.com) en [Azure Government](https://management.usgovcloudapi.net). Zie voor meer informatie over Azure Government [Azure Global en overheid API-eindpunten](../azure-government/documentation-government-developer-guide.md#endpoint-mapping)_._

## <a name="next-steps"></a>Volgende stappen

- Als u dit nog niet hebt al de eerste snelstartgids voltooid voor kostenbeheer, lezen via [kosten analyseren](quick-acm-cost-analysis.md).
