---
title: Instellen van uw factureringsrekening voor een Microsoft-KLANTOVEREENKOMST - Azure | Microsoft Docs
description: Meer informatie over het instellen van uw factureringsrekening voor een Microsoft-KLANTOVEREENKOMST.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 0bb5b118b3d1bc7b89354bb23305eed15a96985d
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57249014"
---
# <a name="set-up-your-billing-account-for-a-microsoft-customer-agreement"></a>Instellen van uw factureringsrekening voor een Microsoft-KLANTOVEREENKOMST

Als uw organisatie heeft een Microsoft-KLANTOVEREENKOMST vernieuwen van uw Enterprise Agreement-inschrijving is ondertekend, moet u uw factureringsrekening instellen. Uw nieuwe factureringsrekening biedt verbeterde facturerings- en beheermogelijkheden voor kosten via een nieuwe gestroomlijnde, gebundeld beheer:  

- De Azure-services en facturering, met inbegrip van facturen, in de Azure-portal van uw organisatie beheren

- De kosten op uw factuur op basis van uw behoeften voor eenvoudig bijhouden en kostentoewijzing organiseren

- Controleren en analyseren van uw factuur per maand, digitale in de facturering pagina + Azure Cost Management

Bij het instellen van het factureringsaccount, verandert u de facturering van uw bestaande Azure-abonnementen aan het nieuwe account. De manier waarop u zult beheren de facturering voor uw abonnementen in het nieuwe account verschilt van hoe u in uw Enterprise Agreement-inschrijving beheerd. U wordt aangeraden dat u het volgende doen voordat u de installatie begint:

- **Informatie over wijzigingen in uw hiërarchie van facturering**
    - U nieuwe facturering account is anders dan uw Enterprise Agreement-inschrijving ingedeeld. Zie voor meer informatie, [informatie over wijzigingen in uw hiërarchie facturering](#understand-changes-to-your-billing-hierarchy).
- **Informatie over wijzigingen in uw facturering beheerders toegang**
    - Beheerders van uw Enterprise Agreement-inschrijving krijgen toegang tot de facturering bereiken in het nieuwe account. Zie voor meer informatie, [informatie over wijzigingen in uw facturering beheerders toegang](#understand-changes-to-your-billing-administrators-access).
- **Bekijk Enterprise Agreement-functies die zijn vervangen door het nieuwe account**
    - Bekijk de functies van Enterprise Agreement-inschrijving die zijn vervangen door bijgewerkte functies in het nieuwe account. [Bekijk functies vervangen door de nieuwe factureringsaccount](#review-features-replaced-by-the-new-billing-account).
- **Weergave-antwoorden op veelgestelde vragen**
    - Aanvullende informatie voor meer informatie over de installatie weergeven. Ga naar [aanvullende informatie](#additional-information).

## <a name="set-up-your-billing-account-in-the-azure-portal"></a>Instellen van uw factureringsrekening in Azure portal

U voltooit de installatie, moet u toegang tot zowel de nieuwe factureringsaccount als de Enterprise Agreement-inschrijving. Zie voor meer informatie, [toegang vereist voor het voltooien van de set up van uw factureringsrekening](#access-required-to-complete-the-setup).

1. Aanmelden bij Azure portal via de koppeling in het e-mailadres dat naar u verzonden is wanneer u de overeenkomst van de klant Microsoft ondertekend.

2. Als iemand anders in uw organisatie de overeenkomst ondertekend of als u het e-mailbericht niet hebt, kunt u zich aanmelden met behulp van de volgende koppeling. Vervang **enrollmentNumber** met het inschrijvingsnummer van uw Enterprise Agreement die is vernieuwd.

   `https://portal.azure.com/#blade/Microsoft_Azure_Billing/EATransitionToMCA/enrollmentId/enrollmentNumber`

3. Selecteer **Start overgang** in de laatste stap van de installatie. Nadat u start overgang selecteren:

    ![Schermafbeelding van de wizard setup](./media/billing-mca-setup-account/ea-mca-set-up-wizard.png)

    - Een facturering hiërarchie die overeenkomt met uw Enterprise Agreement-hiërarchie wordt gemaakt in het nieuwe account voor facturering. Zie voor meer informatie, [informatie over wijzigingen in uw hiërarchie facturering](#understand-changes-to-your-billing-hierarchy).
    - Beheerders van uw Enterprise Agreement-inschrijving toegang krijgen tot de nieuwe factureringsaccount zodat ze blijven beheren de facturering voor uw organisatie.
    - De facturering van uw Azure-abonnementen is overgezet naar het nieuwe account. **Er niet dit van invloed is op uw Azure-services tijdens deze overgang. Ze zult blijven uitvoeren zonder enige onderbreking**.
    - Als u Azure reserveringen hebt, moeten ze worden verplaatst naar uw nieuwe factureringsrekening met dezelfde korting en termijn. De reserveringskorting blijven worden toegepast tijdens de overgang.

4. U kunt de status van de overgang controleren op de **overgang status** pagina.

   ![Schermafbeelding van de status van de overgang](./media/billing-mca-setup-account/ea-mca-set-up-status.png)

## <a name="access-required-to-complete-the-setup"></a>Toegang is vereist om de installatie te voltooien

U voltooit de installatie, moet u de volgende toegang:

- Eigenaar van de facturering-profiel dat is gemaakt tijdens de Microsoft-overeenkomst voor de klant is ondertekend. Zie voor meer informatie over facturering-profielen, [inzicht in facturering profielen](billing-mca-overview.md#understand-billing-profiles).

- Enterprise-beheerder voor de inschrijving die wordt verlengd.

### <a name="if-youre-not-an-enterprise-administrator-on-the-enrollment"></a>Als u niet een enterprise-beheerder voor de inschrijving

U kunt de enterprise-beheerders van de inschrijving om de instellingen van uw factureringsrekening aanvragen.

1. Aanmelden bij Azure portal via de koppeling in het e-mailadres dat naar u verzonden is wanneer u de overeenkomst van de klant Microsoft ondertekend.

2. Als iemand anders in uw organisatie de overeenkomst ondertekend of als u het e-mailbericht niet hebt, kunt u zich aanmelden met behulp van de volgende koppeling. Vervang **enrollmentNumber** met het inschrijvingsnummer van uw enterprise agreement die is vernieuwd.

   (https://portal.azure.com/#blade/Microsoft_Azure_Billing/EATransitionToMCA/enrollmentId/enrollmentNumber)

3. Selecteer de enterprise-beheerders die u wilt dat de aanvraag te verzenden.

   ![Schermafbeelding waarin de administrators van de onderneming uitnodigen](./media/billing-mca-setup-account/ea-mca-invite-admins.png)

4. Selecteer **Verzendaanvraag**.

   De beheerders ontvangt een e-mailbericht met instructies om de installatie te voltooien.

### <a name="if-youre-not-an-owner-of-the-billing-profile"></a>Als je niet eigenaar van het profiel voor facturering

De gebruiker in uw organisatie, die de klant Microsoft overeenkomst ondertekend wordt toegevoegd als de eigenaar van het profiel voor facturering. Vraag de gebruiker toe te voegen als een eigenaar zodat u de installatie kunt uitvoeren.  <!-- Todo Are there any next steps -->

## <a name="understand-changes-to-your-billing-hierarchy"></a>Inzicht in wijzigingen in uw factureringshiërarchie

Uw nieuwe factureringsrekening vereenvoudigt de facturering voor uw organisatie terwijl u verbeterde facturering en beheermogelijkheden voor kosten. Het volgende diagram wordt uitgelegd hoe de facturering wordt georganiseerd in het nieuwe account voor facturering.

![Afbeelding van ea-mca-post-overgang-hiërarchie](./media/billing-mca-setup-account/mca-post-transition-hierarchy.svg)

1. U de factureringsaccount gebruiken voor het beheren van facturering voor uw Microsoft-overeenkomst voor de klant. Zie voor meer informatie over factureringsaccount [begrijpen factureringsaccount](billing-mca-overview.md#understand-billing-account).
2. U kunt het facturering profiel gebruiken voor het beheren van facturering voor uw organisatie, die vergelijkbaar is met uw Enterprise Agreement-inschrijving. Enterprise-beheerders kunnen worden eigenaren van het profiel voor facturering. Zie voor meer informatie over facturering-profielen, [inzicht in facturering profielen](billing-mca-overview.md#understand-billing-profiles).
3. Een gedeelte van de factuur kunt u uw kosten op basis van uw behoeften, vergelijkbaar met afdelingen in uw Enterprise Agreement-inschrijving organiseren. Afdeling wordt factuur secties en beheerders van de afdeling worden eigenaars van de secties met respectieve factuur. Zie voor meer informatie over de secties van de factuur, [begrijpen factuur secties](billing-mca-overview.md#understand-invoice-sections).
4. De accounts die zijn gemaakt in uw Enterprise Agreement worden niet ondersteund in het nieuwe account voor facturering. De account van de abonnementen deel uitmaken van het gedeelte van de respectieve factuur voor hun afdeling. Eigenaars kunnen maken en beheren van abonnementen voor een sectie factuur.

## <a name="understand-changes-to-your-billing-administrators-access"></a>Informatie over wijzigingen in uw facturering beheerders toegang

Afhankelijk van hun toegang krijgen factureringsbeheerders op uw Enterprise Agreement-inschrijving toegang tot de facturering scopes op het nieuwe account. Het volgende diagram wordt de wijziging in de toegang tijdens de overgang uitgelegd:

![Afbeelding van ea-mca-post-overgang-toegang](./media/billing-mca-setup-account/mca-post-transition-access.png)

| Bestaande rol | Post overgang rol  |
|---------|---------|
|Ondernemingsbeheerder (alleen-lezen = Nee)     |-Eigenaar van het profiel van de facturering voor de inschrijving </br> -Eigenaar van alle secties van de factuur |
|Ondernemingsadministrator | -Lezer op het profiel van de facturering voor de inschrijving </br> -Lezer op alle secties van de factuur |
|De beheerder van de afdeling (lezen alleen = Nee)    |-Eigenaar van het gedeelte van de factuur voor hun respectieve afdeling is gemaakt      |
|Afdelingsbeheerder     | -Lezer in het gedeelte van de factuur voor hun respectieve afdeling is gemaakt      |
|Eigenaar van account     | -Maker azure-abonnement op de factuur sectie gemaakt voor hun respectieve afdeling |

Een Azure Active Directory-tenant is geselecteerd voor de nieuwe factureringsaccount tijdens het ondertekenen van de Microsoft-overeenkomst voor de klant. Als een tenant voor uw organisatie niet bestaat, wordt een nieuwe tenant wordt gemaakt. De tenant aansluit bij uw organisatie in Azure Active Directory. De tenant wordt globale tenantbeheerders in uw organisatie gebruiken voor het beheren van toegang tot toepassingen en gegevens in uw organisatie.

Uw nieuwe account biedt alleen ondersteuning voor gebruikers van de tenant die is geselecteerd tijdens het ondertekenen van de Microsoft-overeenkomst voor de klant. Als gebruikers met beheerdersrechten van uw Enterprise overeenkomst deel van de tenant uitmaken, krijgt zij toegang tot de nieuwe factureringsaccount tijdens de overgang. Als ze geen deel uit van de tenant bent, zich ze niet voor toegang tot de nieuwe factureringsaccount, tenzij u ze kunt uitnodigen.

Wanneer u de gebruikers uitnodigen, ze als gastgebruikers ook kunnen worden toegevoegd aan de tenant en krijg toegang tot de rekening. Als u wilt uitnodigen de gebruikers, moet u de toegang voor gasten ingeschakeld voor de tenant. Zie voor meer informatie, [beheren van toegang voor gasten in Azure Active Directory](https://docs.microsoft.com/en-us/microsoftteams/teams-dependencies#control-guest-access-in-azure-active-directory). Als de toegang voor gasten is uitgeschakeld, moet u contact op met de globale beheerders van uw tenant deze in te schakelen. <!-- Todo - How can they find their global administrator -->

## <a name="review-features-replaced-by-the-new-billing-account"></a>Bekijk functies vervangen door de nieuwe factureringsaccount

De volgende Enterprise Agreement van functies worden vervangen door nieuwe functies in de factureringsaccount voor een Microsoft-KLANTOVEREENKOMST.

### <a name="enterprise-agreement-accounts"></a>Enterprise Agreement-accounts

De accounts die zijn gemaakt in uw Enterprise Agreement-inschrijving worden niet in de nieuwe factureringsaccount ondersteund. De account van de abonnementen deel uitmaken van het gedeelte van de factuur voor hun respectieve afdeling gemaakt. Eigenaars kunnen worden makers van Azure-abonnement en maken en beheren van abonnementen voor een sectie factuur.

### <a name="notification-contacts"></a>Melding contactpersonen

Melding-contactpersonen worden verzonden e-mailberichten over de Azure Enterprise-overeenkomst. Ze worden niet ondersteund in het nieuwe account voor facturering. E-mailberichten over Azure-tegoed en facturen worden verzonden naar gebruikers die toegang tot facturering-profielen in uw factureringsrekening hebben.

### <a name="spending-quotas"></a>Quota voor de uitgavelimiet

De bestedingslimiet quota's die zijn ingesteld voor afdelingen in uw Enterprise Agreement-inschrijving zijn vervangen door budgetten in het nieuwe account voor de facturering. Een budget wordt voor elke bestedingsquotum instellen voor afdelingen in uw inschrijving gemaakt. Zie voor meer informatie over budgetten [maken en beheren van Azure budgetten](../cost-management/manage-budgets.md).

### <a name="cost-centers"></a>Kostenplaatsen

Kostenplaats die zijn ingesteld voor de Azure-abonnementen in uw Enterprise Agreement-inschrijving in de nieuwe factureringsaccount worden overgedragen. Echter, kostenplaatsen voor afdelingen en Enterprise Agreement-accounts worden niet ondersteund.

## <a name="additional-information"></a>Aanvullende informatie

De volgende secties bevatten aanvullende informatie over het instellen van uw factureringsrekening.

### <a name="no-service-downtime"></a>Er is geen service-uitvaltijd

Azure-services in uw abonnement blijven zonder enige onderbreking uitvoeren. We alleen de facturering regelen voor uw Azure-abonnementen worden overgezet. Er is niet een invloed op bestaande resources, resourcegroepen of beheergroepen.

### <a name="user-access-to-azure-resources"></a>Gebruikerstoegang tot Azure-resources

Toegang tot Azure-resources die was ingesteld met behulp van Azure RBAC (op rollen gebaseerd toegangsbeheer) wordt niet beïnvloed tijdens de overgang.

### <a name="azure-reservations"></a>Azure-reserveringen

Een Azure-reserveringen in uw Enterprise Agreement-inschrijving wordt verplaatst naar uw nieuwe account voor de facturering. Tijdens de overgang niet eventuele wijzigingen in de reservering kortingen die worden toegepast op uw abonnementen.

### <a name="azure-marketplace-products"></a>Azure Marketplace-producten

Alle Azure Marketplace-producten in uw Enterprise agreement-inschrijving worden, samen met de abonnementen verplaatst. Er zijn niet alle wijzigingen in de Servicetoegang van de Marketplace-producten tijdens de overgang.

### <a name="support-plan"></a>Ondersteuningsplan

Regelingen voor ondersteuning worden niet overgedragen als onderdeel van de overgang. Een nieuw ondersteuningsabonnement voor ophalen van de voordelen voor Azure-abonnementen in uw nieuwe account voor de facturering koopt.

### <a name="past-charges-and-balance"></a>Kosten in rekening gebracht en saldo uit het verleden

Kosten in rekening gebracht en verantwoording saldo vóór overgang kan worden weergegeven in uw Enterprise Agreement-inschrijving via Azure portal. <!--Todo - Add a link for this-->

### <a name="when-should-the-setup-be-completed"></a>Wanneer de installatie moet worden uitgevoerd?

Voltooi de installatie van uw factureringsrekening voordat uw Enterprise Agreement-inschrijving is verlopen. Als uw registratie is verlopen, services in uw Azure-abonnementen wordt nog steeds houden op worden uitgevoerd zonder onderbreking. Echter, u in rekening gebracht de tarieven voor de services.

### <a name="changes-to-the-enterprise-agreement-enrollment-after-the-setup"></a>Wijzigingen in de Enterprise Agreement-inschrijving na de installatie

Azure-abonnementen die zijn gemaakt voor de Enterprise Agreement-inschrijving na de overgang kan handmatig worden verplaatst naar het nieuwe account voor facturering. Zie voor meer informatie, [ophalen facturatie eigendom van Azure-abonnementen van andere gebruikers](billing-mca-request-billing-ownership.md). Azure-reserveringen die zijn gekocht na de overgang verplaatsen [Neem contact op met ondersteuning voor Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). U kunt ook gebruikers toegang tot de factureringsaccount opgeven na de overgang. Zie voor meer informatie, [facturering rollen in Azure portal beheren](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal)

### <a name="revert-the-transition"></a>De overgang herstellen

De overgang kan niet worden teruggedraaid. Zodra de facturering van uw Azure-abonnementen is overgezet naar de nieuwe factureringsaccount, kunt u deze niet terugkeren terug naar uw Enterprise Agreement-inschrijving.

### <a name="closing-your-browser-during-setup"></a>Uw browser wordt gesloten tijdens de installatie

Voordat u klikt op **Start overgang**, kunt u de browser sluiten. U kunt terugkeren naar de installatie met behulp van de koppeling die u hebt verkregen in het e-mailbericht en start de overgang. Als u de browser sluit nadat de overgang is gestart, wordt de overgang op blijven uitvoeren. Ga terug naar de pagina van de overgang status voor het bewaken van de meest recente status van de overgang. U ontvangt een e-mail wanneer de overgang is voltooid.

## <a name="validate-the-billing-account-is-set-up-properly"></a>Valideren van de facturering account juist is ingesteld

 Controleer het volgende om te controleren of dat uw nieuwe factureringsrekening juist is ingesteld:

### <a name="azure-subscriptions"></a>Azure-abonnementen

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in Azure portal](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Selecteer het profiel van de facturering. Afhankelijk van uw toegang moet u mogelijk facturering-account selecteren. Selecteer in het factureringsaccount **facturering profielen** en vervolgens het profiel voor facturering.

4. Selecteer **Azure-abonnementen** aan de linkerkant.

   ![Schermafbeelding van lijst met abonnementen](./media/billing-mca-setup-account/billing-mca-subscriptions-post-transition.png)

Alle Azure-abonnementen die aan de nieuwe factureringsrekening zijn overgegaan van uw Enterprise Agreement-inschrijving worden weergegeven op de pagina Azure-abonnementen. Als u denkt dat er ontbreekt een abonnement dat, verandert de facturering van het abonnement handmatig in Azure portal. Zie voor meer informatie, [ophalen facturatie eigendom van Azure-abonnementen van andere gebruikers](billing-mca-request-billing-ownership.md)

### <a name="azure-reservations"></a>Azure-reserveringen

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in Azure portal](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Selecteer een gedeelte van de factuur. Afhankelijk van uw toegang moet u mogelijk factureringsaccount of een profiel voor facturering selecteren.  Selecteer uit de factureringsaccount of factureringsprofiel **factuur secties** en vervolgens een gedeelte van de factuur.

    ![Schermafbeelding van lijst met factuur sectie post overgang](./media/billing-mca-setup-account/billing-mca-invoice-sections-post-transition.png)

4. Selecteer **alle producten** aan de linkerkant.

5. Zoeken op **gereserveerde**.

    ![Schermafbeelding van lijst met abonnementen post overgang](./media/billing-mca-setup-account/billing-mca-azure-reservations-post-transition.png)

Alle Azure-reserveringen die van uw Enterprise Agreement-inschrijving worden verplaatst naar de nieuwe factureringsaccount worden weergegeven op de pagina met alle producten. Herhaal de stappen voor alle secties van een factuur om te verifiëren dat alle Azure-reserveringen zijn verplaatst van uw Enterprise Agreement-inschrijving. Als u denkt dat elke Azure-reservering ontbreekt, [Neem contact op met ondersteuning van Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) de reservering verplaatsen naar het nieuwe account voor facturering.

### <a name="access-of-enterprise-administrators-on-the-billing-profile"></a>Toegang van enterprise-beheerders op het profiel voor facturering

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in Azure portal](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Selecteer het facturering profiel gemaakt voor uw inschrijving. Afhankelijk van uw toegang moet u mogelijk facturering-account selecteren.  Selecteer in het factureringsaccount **facturering profielen** en vervolgens het profiel voor facturering.

4. Selecteer **toegangsbeheer (IAM)** aan de linkerkant.

   ![Schermafbeelding van de toegang van enterprise-beheerders post overgang](./media/billing-mca-setup-account/billing-mca-ea-admins-access-post-transition.png)

Enterprise-beheerders kunnen worden weergegeven als het profiel eigenaren bij de enterprise-beheerders met alleen-lezen machtigingen worden vermeld als profiel lezers facturering facturering. Als de toegang voor alle enterprise-beheerders kunnen ontbreekt, kunt u hen toegang geven in de Azure-portal. Zie voor meer informatie, [facturering rollen in Azure portal beheren](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="access-of-enterprise-administrators-department-administrators-and-account-owners-on-invoice-sections"></a>Toegang van enterprise-beheerders kunnen beheerders van de afdeling en eigenaars op factuur secties

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

2. Zoeken op **kosten Management en facturering**.

   ![Schermafbeelding van zoeken in Azure portal](./media/billing-mca-setup-account/billing-search-cost-management-billing.png).

3. Selecteer een gedeelte van de factuur. Factuur secties hebben dezelfde naam als de desbetreffende afdelingen in EA-inschrijvingen. Afhankelijk van uw toegang moet u mogelijk een factureringsprofiel of factureringsaccount selecteren. Selecteer in het profiel voor facturering of het factureringsaccount **factuur secties** en selecteer vervolgens een gedeelte van de factuur.

   ![Schermafbeelding van lijst met factuur sectie post overgang](./media/billing-mca-setup-account/billing-mca-invoice-sections-post-transition.png)

4. Selecteer **toegangsbeheer (IAM)** aan de linkerkant.

    ![Schermafbeelding van de toegang van de afdeling en beheerders toegang tot post overgang-account](./media/billing-mca-setup-account/billing-mca-department-account-admins-access-post-transition.png)

Enterprise-beheerders en beheerders van de afdeling worden weergegeven als eigenaren van de sectie factuur of factuur sectie lezers terwijl eigenaars van de afdeling worden weergegeven als de makers van Azure-abonnement. Herhaal de stap voor alle factuur secties om te controleren of de toegang voor alle afdelingen in uw Enterprise Agreement-inschrijving. Eigenaars die deel uitmaken van een afdeling niet krijgen toestemming voor de gedeelte van een factuur met de naam **factuur standaardsectie**. Als de toegang voor alle beheerders ontbreekt, kunt u hen toegang geven in de Azure-portal. Zie voor meer informatie, [facturering rollen in Azure portal beheren](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag met uw nieuwe account voor facturering](billing-mca-overview.md)

- [Leer hoe u algemene EA-taken uitvoert in uw nieuwe account voor facturering](billing-mca-enterprise-operations.md)
