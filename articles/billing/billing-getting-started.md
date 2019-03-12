---
title: Onverwachte kosten voorkomen, facturering in Azure beheren | Microsoft Docs
description: Informatie over het voorkomen van onverwachte kosten op uw Azure-factuur. Kosten bij te houden en het beheer-functies voor een Microsoft Azure-abonnement gebruiken.
services: ''
documentationcenter: ''
author: bandersmsft
manager: alherz
editor: ''
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2018
ms.author: banders
ms.openlocfilehash: 56d392a7df2a659cf457631ec5693766fd674f91
ms.sourcegitcommit: 235cd1c4f003a7f8459b9761a623f000dd9e50ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57727192"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Voorkomen van onverwachte kosten met Azure-facturering en -kostenbeheer

Wanneer u zich registreert voor Azure, zijn er verschillende dingen die u doen kunt om een beter idee van uw uitgaven te krijgen. De [prijscalculator](https://azure.microsoft.com/pricing/calculator/) een schatting van de kosten kunt opgeven voordat u een Azure-resource maakt. De [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) biedt u de huidige verdeling van de kosten en prognose voor uw abonnement. Als u wilt groeperen en te begrijpen van de kosten voor verschillende projecten en teams, bekijkt u [resource tagging](../azure-resource-manager/resource-group-using-tags.md). Als uw organisatie een systeem voor rapportage die u wilt gebruiken heeft, bekijk dan de [facturering API's](billing-usage-rate-card-overview.md).

- Als uw abonnement is een Enterprise Agreement (EA), is de openbare preview-versie voor het bekijken van uw kosten in de Azure-portal beschikbaar. Als uw abonnement via een Cloud Solution Provider (CSP) of Azure Sponsorship, kunnen sommige van de volgende functies niet geldt voor u. Zie [aanvullende bronnen voor EA, CSP en Sponsorship](#other-offers) voor meer informatie.

- Als uw abonnement een gratis proefversie is, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure in Open (AIO) of BizSpark, uw abonnement wordt automatisch uitgeschakeld wanneer alle uw tegoed is gebruikt. Meer informatie over [bestedingslimieten](#spending-limit) om te voorkomen dat uw abonnement is onverwacht wordt uitgeschakeld.

- Als u zich hebt aangemeld voor [gratis Azure-account](https://azure.microsoft.com/free/), [kunt u enkele van de meest populaire Azure-services voor 12 maanden gratis](billing-create-free-services-included-free-account.md). Samen met de onderstaande aanbevelingen, Zie [voorkomen oninbaar gratis account](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>De geschatte kosten voor Azure-services toe te voegen

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Schat de kosten online met behulp van de prijscalculator

Bekijk de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) een geschatte maandelijkse kosten van de service die u wilt ophalen. U kunt een eerste partij Azure-resource om de kosten van een schatting kunt toevoegen.

![Schermopname van de prijzen Rekenmachine-menu](./media/billing-getting-started/pricing-calc.png)

Een A1 Windows Virtual Machine (VM) wordt bijvoorbeeld geschat op $66.96 USD/maand in de rekenuren kosten als u het uitvoeren van de hele tijd laat:

![Schermafbeelding van de prijscalculator weergegeven dat een Windows-VM van A1 schatting kosten $66.96 USD per maand](./media/billing-getting-started/pricing-calcVM.png)

Zie voor meer informatie over prijzen [Veelgestelde vragen over](https://azure.microsoft.com/pricing/faq/). Of als u wilt om te communiceren met een Azure verkoper, neem dan contact op met de 1-800-867-1389.

### <a name="review-the-estimated-cost-in-the-azure-portal"></a>Bekijk de geschatte kosten in de Azure-portal

Doorgaans wanneer u een service in Azure portal toevoegt, is er een weergave waarin u een vergelijkbare geschatte kosten per maand. Bijvoorbeeld, als u ervoor de grootte van uw Windows-VM kiest, ziet u de geschatte maandelijkse kosten voor de rekenuren:

![Voorbeeld: een A1-Windows-VM wordt geschat op kosten $66.96 USD per maand](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="spending-limit"></a> Controleer of u een bestedingslimiet hebt op

Als u een abonnement dat gebruikmaakt van tegoed hebt, is klikt u vervolgens de bestedingslimiet voor u standaard ingeschakeld. Op deze manier wanneer u uw tegoed, besteden aan uw creditcard niet in rekening gebracht. Zie de [volledig overzicht van Azure-aanbiedingen en de beschikbaarheid van de bestedingslimiet](https://azure.microsoft.com/support/legal/offer-details/).

Echter, als u uw bestedingslimiet hebt bereikt, krijgen uw services uitgeschakeld. Dit betekent dat uw VM's ongedaan wordt gemaakt. Om te voorkomen dat een service-uitvaltijd, moet u de bestedingslimiet uitschakelen. Een eventuele overschrijding wordt in rekening gebracht op uw creditcard in het bestand. 

Als u wilt zien als u hebt gekregen bestedingslimiet, gaat u naar de [abonnementen weergeven in het Accountcentrum](https://account.windowsazure.com/Subscriptions). Als u uw bestedingslimiet hebt ingeschakeld, wordt een banner weergegeven:

![Schermafbeelding van een waarschuwing over uitgaven limiet wordt op in het Accountcentrum](./media/billing-getting-started/spending-limit-banner.PNG)

Klik op de banner en volg de aanwijzingen voor de bestedingslimiet verwijderen. Als u niet hebt creditcardgegevens invoert wanneer u zich hebt geregistreerd, moet u om de bestedingslimiet verwijderen. Zie voor meer informatie, [bestedingslimiet voor Azure – hoe het werkt en hoe u kunt inschakelen of verwijderen van deze](https://azure.microsoft.com/pricing/spending-limits/).

U kunt de [Cloudyn](https://www.cloudyn.com/) service voor het maken van waarschuwingen die automatisch op de hoogte belanghebbenden stellen van uitgaven van afwijkingen en risico's budgetoverschrijding. U kunt waarschuwingen maken met behulp van rapporten die waarschuwingen op basis van budget en kostendrempels ondersteunen. Zie voor meer informatie over het gebruik van Cloudyn [zelfstudie: Gebruik en kosten bekijken](../cost-management/tutorial-review-usage.md).

In dit voorbeeld wordt het rapport **Actual Cost Over Time** (Werkelijke kosten in de loop van de tijd) gebruikt om u een waarschuwing te sturen wanneer uw uitgaven voor een Azure-VM het totale budget naderen. In dit scenario hebt u een totaalbudget van $ 20.000. U wilt een waarschuwing ontvangen wanneer bij $ 9000 de kosten de helft van het budget beginnen te naderen en een extra waarschuwing wanneer het bedrag van $ 10.000 wordt bereikt.

1. Selecteer in het menu boven aan Cloudyn-portal de optie **Costs** > **Cost Analysis** > **Actual Cost Over Time**. 
2. Stel **Groups** in op **Service** en stel **Filter on the service** in op **Azure/VM**. 
3. Selecteer rechtsboven in het rapport de optie **Actions** en selecteer **Schedule report**.
4. Als u zelf op vaste tijden een e-mail met het rapport wilt ontvangen, selecteert u het tabblad **Scheduling** in het dialoogvenster **Save or Schedule this**. Selecteer **Send via email**. Alle labels, groepen en filters die u gebruikt, zijn opgenomen het rapport dat u per e-mail wordt toegezonden. 
5. Selecteer het tabblad **Threshold** en selecteer **Actual Cost vs. Threshold**. 
   1. In het vak voor de drempelwaarde **Red alert** voert u 10000 in. 
   2. In het vak voor de drempelwaarde **Yellow alert** voert u 9000 in. 
   3. In het vak **Number of consecutive alerts** voert u het aantal opeenvolgende te ontvangen waarschuwingen in. Wanneer u het totaal aantal waarschuwingen hebt ontvangen dat u hebt opgegeven, worden er geen extra waarschuwingen meer verzonden. 
6. Selecteer **Opslaan**.

    ![Voorbeeld met rode en gele waarschuwingen op basis van uitgavedrempelwaarden](./media/billing-getting-started/schedule-alert01.png)

U kunt ook de drempeloptie **Cost Percentage vs. Budget** kiezen om waarschuwingen te maken. Hiermee kunt u de drempels opgeven als percentage van uw budget in plaats van bedragen.

## <a name="ways-to-monitor-your-costs-when-using-azure-services"></a>Manieren om te controleren van uw kosten bij het gebruik van Azure-services

### <a name="tags"></a> Labels toevoegen aan uw resources voor het groeperen van uw factureringsgegevens

U kunt tags facturering om gegevens te groeperen voor ondersteunde services gebruiken. Bijvoorbeeld, als u meerdere virtuele machines voor verschillende teams uitvoert, kunt klikt u vervolgens u labels voor het categoriseren van kosten op kostenplaats (HR, marketing, financiën) of de omgeving (Pre-productie-test, productie,). 

![Schermafbeelding van het instellen van tags in de portal](./media/billing-getting-started/tags.PNG)

De labels weergegeven in de gehele verschillende kosten voor het melden van weergaven. Bijvoorbeeld, ze zichtbaar zijn in uw [analyseweergave kosten](#costs) direct en details van CSV-gebruiksbestand na uw eerste factureringsperiode.

Zie voor meer informatie, [met tags voor het ordenen van uw Azure-resources](../azure-resource-manager/resource-group-using-tags.md).

### <a name="costs"></a> Regelmatig controleren op de portal voor verdeling van de kosten en burn rate

Nadat u uw services die worden uitgevoerd, moet u regelmatig controleren hoeveel ze bent u onnodig op kosten. U kunt zien van de huidige uitgaven en burn rate in Azure portal.

1. Ga naar de [abonnementen in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en een abonnement te selecteren.

2. Als dit wordt ondersteund voor uw abonnement, kunt u zien van de verdeling van de kosten en burn rate.

    ![Schermopname van branden snelheid en een uitsplitsing van in Azure portal](./media/billing-getting-started/burn-rate.PNG)

3. Klik op **Cost analysis** in de lijst aan de linkerkant om te zien van de verdeling van de kosten per resource. Wacht 24 uur na het toevoegen van een service voor de gegevens om in te vullen.

    ![Schermafbeelding van de weergave van de kosten voor analyse in Azure portal](./media/billing-getting-started/cost-analysis.PNG)

4. U kunt filteren op verschillende eigenschappen, zoals [tags](#tags), resourcetype, resourcegroep en duur. Klik op **toepassen** om te bevestigen van de filters en **downloaden** als u wilt de weergave exporteren naar een bestand Comma-Separated waarden (.csv).

5. Bovendien kunt u een resource om te zien van uw dagelijkse geschiedenis en hoeveel de resourcekosten per dag.

    ![Schermafbeelding van de geschiedenis-weergave van de uitgaven in Azure portal](./media/billing-getting-started/costhistory.PNG)

Het is raadzaam om eerst te controleren van de kosten die u met de schattingen die u hebt gezien ziet wanneer u de services hebt geselecteerd. Als de kosten sterk verschillen van de schattingen, Controleer de prijsstelling die u hebt geselecteerd voor uw resources.

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Houd rekening met kostenbesparende functies zoals automatisch afsluiten inschakelen voor virtuele machines

Afhankelijk van uw scenario kunt u automatisch afsluiten configureren voor uw virtuele machines in Azure portal. Zie voor meer informatie, [Auto-shutdown voor virtuele machines met Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Schermafbeelding van de optie voor automatisch afsluiten in de portal](./media/billing-getting-started/auto-shutdown.PNG)

Automatisch afsluiten is niet hetzelfde als wanneer u binnen de virtuele machine met opties voor energiebeheer afgesloten. Automatisch afsluiten gestopt en wordt de toewijzing van uw virtuele machines om te stoppen extra gebruikskosten ingetrokken. Voor meer informatie, Zie de veelgestelde vragen over prijzen [virtuele Linux-machines](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) en [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) over VM-statussen.

Bekijk voor meer kostenbesparende-functies voor uw ontwikkel- en testomgevingen, [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Inschakelen en bekijk Azure Advisor-aanbevelingen

[Azure Advisor](../advisor/advisor-overview.md) is een functie die u helpt kosten verlagen door resources te identificeren met lage gebruik. Ga naar Advisor in Azure portal:

![Schermopname van Azure Advisor-knop in Azure portal](./media/billing-getting-started/advisor-button.PNG)

Vervolgens kunt u uitvoerbare aanbevelingen krijgen in de **kosten** tabblad in de Advisor-dashboard:

![Schermafbeelding van Advisor kosten aanbeveling voorbeeld](./media/billing-getting-started/advisor-action.PNG)

Zie voor meer informatie, [kosten van de Advisor-aanbevelingen](../advisor/advisor-cost-recommendations.md).

## <a name="reviewing-costs-at-the-end-of-your-billing-cycle"></a>Kosten controleren aan het einde van de factureringscyclus

Aan het einde van de factureringscyclus wordt uw factuur beschikbaar gemaakt. U kunt ook [downloaden oude facturen en gebruiksgegevens details](billing-download-azure-invoice-daily-usage-date.md) om ervoor te zorgen dat u goed in zijn gebracht. Zie voor meer informatie over het vergelijken van uw dagelijkse gebruik met uw factuur [meer informatie over uw factuur voor Microsoft Azure](billing-understand-your-bill.md).

### <a name="billing-api"></a>Billing-API

De facturering API gebruiken om op te halen via een programma gegevens over het gebruik. Gebruik de RateCard API en de API-gebruik samen om uw gefactureerd gebruik. Zie voor meer informatie, [inzicht in het gebruik van de Microsoft Azure-resources](billing-usage-rate-card-overview.md).

## <a name="other-offers"></a> Aanvullende bronnen en speciale aanvragen

### <a name="ea-csp-and-sponsorship-customers"></a>EA, CSP en Sponsorship-klanten
Neem contact op met uw accountmanager of de Azure-partner om te beginnen.

| Aanbieding | Resources |
|-------------------------------|-----------------------------------------------------------------------------------|
| Enterprise Agreement (EA) | [EA-portal](https://ea.azure.com/), [help docs](https://ea.azure.com/helpdocs), en [Power BI-rapport](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (CSP) | Neem contact op met uw provider |
| Azure-sponsoring | [Sponsorship-portal](https://www.microsoftazuresponsorships.com/) |

Als u beheert IT voor een grote organisatie, raden wij aan lezen [Azure enterprise-platform](/azure/architecture/cloud-adoption-guide/subscription-governance) en de [enterprise IT-whitepaper](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (PDF-download, alleen in het Engels).

#### <a name="EA"></a> Voorbeeld van Enterprise Agreement kosten weergaven in Azure portal 

Enterprise kosten weergaven zijn momenteel in openbare Preview. Items om te weten:

- Abonnementskosten zijn gebaseerd op het gebruik en zijn exclusief vooruitbetaalde bedragen, overschotten, inbegrepen hoeveelheden, aanpassingen en belastingen. Werkelijke kosten worden berekend op het inschrijvingsniveau.
- Bedragen in Azure portal is mogelijk anders dan wat is er in de Enterprise portal. Updates in de Enterprise portal duurt een paar minuten voordat de wijzigingen worden weergegeven in de Azure-portal.
- Als u de kosten worden niet ziet, is het mogelijk om een van de volgende redenen:
    - U hebt geen machtigingen op het abonnementsniveau. Als u wilt zien enterprise kosten weergaven, moet u een factureren voor lezer, lezer, Inzender of eigenaar op het abonnementsniveau.
    - U bent eigenaar van een Account en de Inschrijvingsbeheerder heeft de 'door de AO kosten weergeven' instelling uitgeschakeld.  Neem contact op met uw beheerder voor inschrijving voor toegang tot de kosten. 
    - U bent een beheerder van een afdeling en de Inschrijvingsbeheerder heeft het "DA kosten weergeven' instellen uitgeschakeld.  Neem contact op met de beheerder van uw registratie om toegang te krijgen.
    - U hebt gekocht Azure via een kanaalpartner en de partner informatie over de prijzen niet vrijgeven.  
- Als u met betrekking tot de kosten van de toegang in de Enterprise portal-instellingen bijwerken, is er een vertraging van enkele minuten duren voordat de wijzigingen worden weergegeven in de Azure-portal.
- Bestedingslimiet en factuur richtlijnen gelden niet voor EA-abonnementen.

### <a name="check-your-subscription-and-access"></a>Controleer uw abonnement en toegang

Als u wilt kosten weergeven, moet u hebben [abonnementen toegang tot factureringsgegevens](billing-manage-access.md). Alleen de beheerder van het Account toegang heeft tot de [Accountcentrum](https://account.azure.com/Subscriptions), wijzigen van informatie over facturering en abonnementen beheren. De accountbeheerder is de persoon die het registratieproces hebben doorlopen. Zie voor meer informatie, [toevoegen of wijzigen Azure-beheerdersrollen waarmee het abonnement of de services](billing-add-change-azure-subscription-administrator.md).

Als u wilt zien als u de accountbeheerder bent, gaat u naar [abonnementen in Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Bekijk de lijst met abonnementen die u toegang tot hebt. Kijk onder **mijn rol**. De status *accountbeheerder*, dan is dit ok. Iets anders, zoals de status *eigenaar*, hebt u geen volledige bevoegdheden.

![Schermafbeelding van uw rol in de weergave abonnementen in Azure portal](./media/billing-getting-started/sub-blade-view.PNG)

Als u niet de accountbeheerder bent, wordt dat iemand waarschijnlijk heeft gestuurd gedeeltelijk toegang met behulp van [toegangsbeheer op basis van Azure Active Directory-rol](../role-based-access-control/role-assignments-portal.md) (RBAC). Voor het beheren van abonnementen en facturering informatie wijzigen [vinden van de accountbeheerder](billing-subscription-transfer.md#whoisaa). Vraag de accountbeheerder om het te doen of [het abonnement aan u overdragen](billing-subscription-transfer.md).

Als uw beheerder van het Account niet meer met uw organisatie is en u moet voor het beheren van facturering, [contact met ons opnemen](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).


### <a name="how-to-request-a-service-level-agreement-credit-for-a-service-incident"></a>Het aanvragen van een Service Level Agreement tegoed voor een service-incident

In de SLA (Service Level Agreement) worden de toezeggingen van Microsoft voor bedrijfstijd en connectiviteit beschreven. Een service-incident wordt gerapporteerd bij het Azure-services ondervindt een probleem dat gevolgen voor actieve tijdsduur of connectiviteit, vaak aangeduid als een 'storing." Als we niet bereiken en onderhouden van de serviceniveaus voor elke Service, zoals beschreven in de SLA, klikt u mogelijk in aanmerking voor een gedeelte van uw maandelijkse servicetarief tegoed.

Om aan te vragen een tegoed:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/). Als u meerdere accounts hebt, zorg ervoor dat u de naam die werd beïnvloed door Azure downtime. Dit helpt ondersteuning automatisch verzamelen van de benodigde achtergrondinformatie en sneller kunt oplossen door het geval is.
2. Maak een nieuwe ondersteuningsaanvraag.
3. Onder **type probleem**, selecteer **facturering**.
4. Onder **probleemtype**, selecteer **aanvraag restitutie**.
5. Toevoegen van informatie om te geven dat vraagt u voor een SLA-credits, vermeld de datum/tijd/tijdzone-, evenals de betrokken services (virtuele machines, websites, enz.)
6. Controleer of uw contactgegevens en selecteer de **maken** knop om uw aanvraag te verzenden.

SLA drempels afhankelijk van de service. Bijvoorbeeld, de weblaag SQL beschikt over een SLA van 99,9%, virtuele machines hebben een SLA van 99,95% en SQL Standard-laag beschikt over een SLA van 99,99%.

Voor sommige services zijn er vereisten voor de SLA te kunnen toepassen. Virtuele Machines moet bijvoorbeeld twee of meer instanties zijn geïmplementeerd in dezelfde Beschikbaarheidsset hebben.

Zie voor meer informatie de [Service Level Agreements](https://azure.microsoft.com/en-us/support/legal/sla/) documentatie en de [samenvatting van SLA voor Azure-services](https://azure.microsoft.com/en-us/support/legal/sla/summary/) documentatie.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
