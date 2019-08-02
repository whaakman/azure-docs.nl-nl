---
title: Onverwachte kosten voor komen en facturering in azure beheren
description: Meer informatie over hoe u onverwachte kosten voor uw Azure-factuur kunt voor komen. Gebruik functies voor het bijhouden en beheren van kosten voor een Azure-abonnement.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: b64e84c3fff27675029ff35f27972a4aca014ec3
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68612077"
---
# <a name="prevent-unexpected-charges-with-azure-billing-and-cost-management"></a>Onverwachte kosten voor komen met facturering en kosten beheer van Azure

Wanneer u zich aanmeldt voor Azure, zijn er verschillende dingen die u kunt doen om een beter beeld te krijgen van uw uitgaven:

- De [prijs calculator](https://azure.microsoft.com/pricing/calculator/) kan een schatting van de kosten bieden voordat u een Azure-resource maakt. 

- De [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) biedt u de huidige kosten analyse en prognose voor uw abonnement. 

- Als u de kosten voor verschillende projecten of teams wilt groeperen en begrijpen, bekijkt u de [bron code ring](../azure-resource-manager/resource-group-using-tags.md). Als uw organisatie een rapportage systeem heeft dat u wilt gebruiken, Bekijk dan de facturerings- [api's](billing-usage-rate-card-overview.md).

- Als uw abonnement is gemaakt op basis van een Enterprise Agreement (EA), kunt u uw kosten bekijken in de Azure Portal. Als uw abonnement is via een Cloud Solution Provider (CSP) of Azure Sponsorship, zijn sommige van de volgende functies mogelijk niet op u van toepassing. Zie [aanvullende bronnen voor EA, CSP en sponsor](#other-offers)voor meer informatie.

- Als uw abonnement een gratis proef versie, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure in open (AIO) of BizSpark is, wordt uw abonnement automatisch uitgeschakeld wanneer al uw tegoeden worden gebruikt. Meer informatie [](#spending-limit) over bestedings limieten om te voor komen dat uw abonnement onverwacht wordt uitgeschakeld.

- Als u zich hebt geregistreerd voor een [gratis Azure-account](https://azure.microsoft.com/free/), [kunt u een aantal van de populairste Azure-Services gedurende 12 maanden gratis gebruiken](billing-create-free-services-included-free-account.md). Bekijk, samen met de hieronder vermelde aanbevelingen, voor [komen dat u in rekening wordt gebracht voor gratis account](billing-avoid-charges-free-account.md).

## <a name="get-estimated-costs-before-adding-azure-services"></a>Geschatte kosten ophalen voordat u Azure-Services toevoegt

Hier vindt u meer informatie over het schatten van de kosten met de volgende hulpprogram ma's:
- Azure-prijscalculator
- Azure Portal
- Bestedingslimiet

In de afbeeldingen in de volgende secties worden de voorbeeld prijzen in Amerikaanse dollars weer gegeven.

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Kosten online schatten met de prijs calculator

Bekijk de [prijs calculator](https://azure.microsoft.com/pricing/calculator/) om een schatting te krijgen van de maandelijkse kosten van de service waarop u bent geïnteresseerd. U kunt een Azure-resource voor de eerste partij toevoegen om een geschatte kosten te krijgen. In de prijs calculator kunt u het valuta type wijzigen.

![Scherm opname van het menu met prijs calculator](./media/billing-getting-started/pricing-calc.png)

In de prijs calculator wordt een a1 virtuele machine (VM) van Windows bijvoorbeeld geschat om een bepaalde hoeveelheid/maand in reken uren te berekenen als u de hele tijd uitvoert:

![Scherm afbeelding van de prijs calculator met een a1 Windows VM-geschatte kosten per maand](./media/billing-getting-started/pricing-calcvm.png)

Zie [Veelgestelde vragen over prijzen](https://azure.microsoft.com/pricing/faq/)voor meer informatie over prijzen. Als u met een Azure-verkoper wilt praten, belt u het telefoon nummer dat boven aan de pagina met veelgestelde vragen wordt weer gegeven.

### <a name="review-estimated-costs-in-the-azure-portal"></a>Bekijk de geschatte kosten in de Azure Portal

Wanneer u een service toevoegt in de Azure Portal, is er meestal een weer gave waarin u een geschatte prijs per maand in uw gefactureerde valuta kunt zien. Als u bijvoorbeeld de grootte van uw Windows-VM kiest, ziet u de geschatte maandelijkse kosten voor de reken uren:

![Voor beeld: een a1 Windows-VM met geschatte kosten per maand](./media/billing-getting-started/vm-size-cost.png)

### <a name="spending-limit"></a>Controleer of u een bestedings limiet hebt voor

Als u een abonnement hebt dat tegoed gebruikt, is de bestedings limiet standaard ingeschakeld. Op deze manier, wanneer u al uw tegoed aan het best Eden, worden er geen kosten in rekening gebracht voor uw credit card. Bekijk de [volledige lijst met Azure-aanbiedingen en de beschik baarheid van bestedings limieten](https://azure.microsoft.com/support/legal/offer-details/).

Wanneer u echter uw bestedings limiet bereikt, worden uw services uitgeschakeld. Dit betekent dat de toewijzing van uw Vm's ongedaan wordt gemaakt. Als u downtime van services wilt voor komen, moet u de bestedings limiet uitschakelen. Alle overschrijding worden in rekening gebracht op uw credit card in het bestand.

Als u wilt controleren of u een bestedings limiet hebt, gaat u naar de [weer gave abonnementen in het account centrum](https://account.windowsazure.com/Subscriptions). Er wordt een banner weer gegeven als uw bestedings limiet is ingeschakeld, vergelijkbaar met het volgende:

![Scherm afbeelding met een waarschuwing over de bestedings limiet die is ingesteld in het account centrum](./media/billing-getting-started/spending-limit-banner.png)

Klik op de banner en volg de aanwijzingen om de bestedings limiet te verwijderen. Als u geen creditcard gegevens hebt ingevoerd toen u zich registreerde, moet u deze invoeren om de bestedings limiet te verwijderen. Zie voor meer informatie [Azure bestedings limiet – hoe deze werkt en hoe u deze kunt in-of uitschakelen](https://azure.microsoft.com/pricing/spending-limits/).

## <a name="use-budgets-and-cost-alerts"></a>Budgetten en kosten waarschuwingen gebruiken

U kunt [budgetten](../cost-management/tutorial-acm-create-budgets.md) maken om kosten te beheren en [waarschuwingen](../cost-management/cost-mgt-alerts-monitor-usage-spending.md) te maken waarmee de betrokkenen automatisch worden geïnformeerd over uitgave afwijkingen en het overeden van Risico's. Waarschuwingen zijn gebaseerd op uitgaven vergeleken met budget en kosten drempels.

## <a name="monitor-costs-when-using-azure-services"></a>Kosten bewaken bij het gebruik van Azure-Services
U kunt de kosten controleren met de volgende hulp middelen:

- Labels
- Kosten analyse en brand frequentie
- Kostenanalyse

### <a name="tags"></a>Tags toevoegen aan resources om facturerings gegevens te groeperen

U kunt tags gebruiken om facturerings gegevens te groeperen voor ondersteunde services. Als u bijvoorbeeld verschillende Vm's voor verschillende teams uitvoert, kunt u Tags gebruiken om de kosten te categoriseren per kosten plaats (bijvoorbeeld: HR, marketing, financiën, enz.) of omgeving (bijvoorbeeld: productie, voor bereiding, test).

![Scherm afbeelding van het instellen van tags in de portal](./media/billing-getting-started/tags.png)

De labels worden weer gegeven in verschillende weer gaven van de kosten rapportage. Ze zijn bijvoorbeeld zichtbaar in de [weer gave kosten analyse](#costs) direct en in het uitgebreide gebruik CSV-bestand na de eerste facturerings periode.

Zie [Tags gebruiken om uw Azure-resources te organiseren](../azure-resource-manager/resource-group-using-tags.md)voor meer informatie.

### <a name="costs"></a>Kosten analyse en brand frequentie bewaken

Nadat u uw Azure-Services hebt uitgevoerd, controleert u de kosten regel matig. U kunt de huidige uitgaven-en brand frequentie bekijken in de Azure Portal.

1. Ga naar de [abonnementen in de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en selecteer een abonnement.

2. Als de app wordt ondersteund voor uw abonnement, ziet u de kosten analyse en de brand frequentie.

    ![Scherm opname van de brand frequentie en uitsplitsing in het Azure Portal](./media/billing-getting-started/burn-rate.PNG)

3. Klik op [kosten analyse](../cost-management/quick-acm-cost-analysis.md) in de lijst een van de linkerkant om de kosten specificatie per resource te bekijken. Nadat u een service hebt toegevoegd, moet u 24 uur wachten totdat de gegevens worden weer gegeven.

    ![Scherm opname van de weer gave kosten analyse in Azure Portal](./media/billing-getting-started/cost-analysis.png)

4. U kunt filteren op verschillende eigenschappen, zoals [Tags](#tags), resource type, resource groep en time span. Klik op **Toep assen** om de filters te bevestigen en te **downloaden** als u de weer gave wilt exporteren naar een bestand met door komma's gescheiden waarden (. CSV).

5. Daarnaast kunt u klikken op een resource om uw dagelijkse uitgaven geschiedenis te bekijken en hoeveel resource kosten elke dag.

    ![Scherm opname van de weer gave uitgaven geschiedenis in Azure Portal](./media/billing-getting-started/costhistory.png)

Vergelijk de kosten die u ziet met de schattingen die u hebt gezien tijdens het selecteren van de services. Als de kosten aanzienlijk afwijken van de schattingen, controleert u het prijs plan dat u hebt geselecteerd voor uw resources.

## <a name="optimize-and-reduce-costs"></a>Kosten optimaliseren en verlagen
Als u niet bekend bent met de principes van Cost Management, lees dan [hoe u uw Cloud investering optimaliseert met Azure Cost Management](../cost-management/cost-mgt-best-practices.md).

In de Azure Portal kunt u Azure-kosten ook optimaliseren en verlagen met automatisch afsluiten voor Vm's en Advisor-aanbevelingen.

### <a name="consider-cost-cutting-features-like-auto-shutdown-for-vms"></a>Kosten besparingen overwegen, zoals automatisch afsluiten voor Vm's

Afhankelijk van uw scenario kunt u automatisch afsluiten configureren voor uw virtuele machines in de Azure Portal. Zie [automatisch afsluiten voor vm's met Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)voor meer informatie.

![Scherm afbeelding van de optie automatisch afsluiten in de portal](./media/billing-getting-started/auto-shutdown.png)

Automatisch afsluiten is niet hetzelfde als wanneer u op de virtuele machine afsluit met energie beheer. Automatisch afsluiten wordt gestopt en de toewijzing van uw Vm's wordt ongedaan om extra gebruiks kosten te stoppen. Zie Veelgestelde vragen over prijzen voor [Linux vm's](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) en Windows- [vm's](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) over VM-statussen voor meer informatie.

Bekijk [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/)voor meer kosten voor het knippen van uw ontwikkel-en test omgevingen.

### <a name="turn-on-and-review-azure-advisor-recommendations"></a>Azure Advisor aanbevelingen inschakelen en controleren

[Azure Advisor](../advisor/advisor-overview.md) helpt u kosten te verlagen door resources te identificeren met weinig gebruik. Ga naar Advisor in de Azure Portal:

![Scherm afbeelding van de knop Azure Advisor in Azure Portal](./media/billing-getting-started/advisor-button.png)

U kunt aanbevelingen voor acties op het tabblad **kosten** op het dash board van Advisor ophalen:

![Scherm opname van voor beeld van Advisor-kosten aanbeveling](./media/billing-getting-started/advisor-action.png)

Raadpleeg de zelf studie [kosten optimaliseren van aanbevelingen](../cost-management/tutorial-acm-opt-recommendations.md) voor een begeleide zelf studie over aanbevelingen voor kosten besparingen.

## <a name="review-costs-against-your-latest-invoice"></a>Bekijk de kosten voor uw laatste factuur

Aan het einde van de facturerings cyclus is uw laatste factuur beschikbaar. U kunt ook [facturen en gedetailleerde gebruiks bestanden downloaden](billing-download-azure-invoice-daily-usage-date.md) om er zeker van te zijn dat u goed in rekening wordt gebracht. Zie [inzicht in uw factuur voor Microsoft Azure](billing-understand-your-bill.md)voor meer informatie over het vergelijken van uw dagelijkse gebruik met uw factuur.

### <a name="billing-api"></a>Billing-API

Gebruik de API voor Azure-facturering om gebruiks gegevens programmatisch te verkrijgen. Gebruik de RateCard-API en de gebruiks-API samen om uw gefactureerd gebruik te verkrijgen. Zie [inzicht verkrijgen in uw Microsoft Azure Resource verbruik](billing-usage-rate-card-overview.md)voor meer informatie.

## <a name="other-offers"></a>Aanvullende bronnen en speciale gevallen

### <a name="ea-csp-and-sponsorship-customers"></a>Klanten van EA, CSP en sponsoring
Neem contact op met uw account manager of Azure-partner om aan de slag te gaan.

| Aanbieding | Resources |
|-------------------------------|-----------------------------------------------------------------------------------|
| Enterprise Agreement (EA) | [EA-Portal](https://ea.azure.com/), [help docs](https://ea.azure.com/helpdocs)en [Power bi rapport](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-enterprise/) |
| Cloud Solution Provider (CSP) | Neem contact op met uw provider |
| Azure Sponsorship | [Sponsor Portal](https://www.microsoftazuresponsorships.com/) |

Als u het beheert voor een grote organisatie, raden we u aan om [Azure Enter prise](/azure/architecture/cloud-adoption-guide/subscription-governance) -steigers en de IT-afdeling van het [bedrijf](https://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) te lezen (PDF-down load, alleen Engels).

#### <a name="EA"></a>Enterprise Agreement kosten weergaven in het Azure Portal

De Enter prise-kosten weergaven zijn momenteel beschikbaar als open bare preview. Items die moeten worden genoteerd:

- Abonnements kosten zijn gebaseerd op gebruik en bevatten geen vooruitbetaalde bedragen, overschrijdingen, opgenomen hoeveel heden, aanpassingen en belastingen. De werkelijke kosten worden berekend op het inschrijvings niveau.
- De bedragen die in de Azure Portal worden weer gegeven, kunnen afwijken van de in de Enter prise Portal. Updates in de Enter prise portal kunnen enkele minuten duren voordat de wijzigingen worden weer gegeven in de Azure Portal.
- Als u geen kosten ziet, kan dit een van de volgende oorzaken hebben:
    - U hebt geen machtigingen op het abonnements niveau. Als u kosten weergaven voor ondernemingen wilt bekijken, moet u een facturerings lezer, lezer, Inzender of eigenaar zijn op het abonnements niveau.
    - U bent een eigenaar van het account en de registratie beheerder heeft de instelling ' AO-weergave kosten ' uitgeschakeld.  Neem contact op met de registratie beheerder om toegang te krijgen tot de kosten.
    - U bent een afdelings beheerder en de registratie beheerder heeft de instelling **voor de da-weer gave-kosten** uitgeschakeld.  Neem contact op met de registratie beheerder om toegang te krijgen.
    - U hebt Azure gekocht via een kanaal partner en de partner heeft geen prijs informatie vrijgegeven.  
- Als u de instellingen met betrekking tot de kosten toegang in de Enter prise Portal bijwerkt, is er een vertraging van enkele minuten voordat de wijzigingen worden weer gegeven in de Azure Portal.
- De bestedings limiet en de factuur richtlijnen zijn niet van toepassing op EA-abonnementen.

### <a name="check-your-subscription-and-access"></a>Controleer uw abonnement en toegang

Als u de kosten wilt weer geven, moet u [toegang tot facturerings gegevens op abonnementen niveau](billing-manage-access.md)hebben. Alleen de account beheerder kan toegang krijgen tot het [account centrum](https://account.azure.com/Subscriptions), facturerings gegevens wijzigen en abonnementen beheren. De account beheerder is de persoon die het registratie proces heeft door lopen. Zie voor meer informatie [Azure-beheerders rollen toevoegen of wijzigen waarmee het abonnement of de services worden beheerd](billing-add-change-azure-subscription-administrator.md).

Als u wilt weten of u de account beheerder bent, gaat u naar [abonnementen in de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Bekijk de lijst met abonnementen en zoek **mijn rol**. Als *account beheerder* de rol is, hebt u volledige bevoegdheden. Als er iets anders is, zoals de *eigenaar*, hebt u geen volledige bevoegdheden.

![Scherm afbeelding van uw rol in de weer gave abonnementen in de Azure Portal](./media/billing-getting-started/sub-blade-view.PNG)

Als u abonnementen wilt beheren en de facturerings gegevens wilt wijzigen, gaat u [naar de account beheerder](billing-subscription-transfer.md#whoisaa). Vraag de account beheerder om de taken te volt ooien of [het abonnement naar u over te dragen](billing-subscription-transfer.md).

Als uw account beheerder niet langer deel uitmaakt van uw organisatie en u uw facturering wilt beheren, [neemt u contact met ons op](https://go.microsoft.com/fwlink/?linkid=2083458).


### <a name="request-a-service-level-agreement-credit-for-a-service-incident"></a>Een Service Level Agreement tegoed aanvragen voor een service-incident

In de SLA (Service Level Agreement) worden de toezeggingen van Microsoft voor bedrijfstijd en connectiviteit beschreven. Er wordt een service-incident gerapporteerd wanneer Azure Services een probleem ondervindt dat gevolgen heeft voor de uptime of de verbinding, ook wel een *storing*genoemd. Als we de service niveaus voor elke service niet bereiken en onderhouden, zoals beschreven in de SLA, kunt u in aanmerking komen voor een tegoed van een deel van uw maandelijkse service kosten.

Een tegoed aanvragen:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/). Als u meerdere accounts hebt, moet u ervoor zorgen dat u deze gebruikt die werd beïnvloed door Azure downtime. 
2. Een nieuwe ondersteunings aanvraag maken.
3. Selecteer onder **type probleem** **facturering**.
4. Selecteer **restitutie aanvraag**onder **probleem type**.
5. Voeg details toe om op te geven dat u een SLA-tegoed vraagt, vermeld de datum/tijd/tijd zone en de betrokken Services (Vm's, websites, enzovoort).
6. Controleer uw contact gegevens en selecteer **maken** om uw aanvraag in te dienen.

SLA-drempels variëren per service. De SQL-weblaag heeft bijvoorbeeld een SLA van 99,9%, Vm's hebben een SLA van 99,95% en de SQL Standard-laag heeft een SLA van 99,99%.

Voor sommige services zijn er vereisten voor het Toep assen van de SLA. Voor virtuele machines moeten bijvoorbeeld twee of meer exemplaren zijn geïmplementeerd in dezelfde Beschikbaarheidsset.

Zie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/) en de [Sla-samen vatting van Azure-Services](https://azure.microsoft.com/support/legal/sla/summary/) -documentatie voor meer informatie.

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, kunt u [een ondersteunings aanvraag maken](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [](billing-spending-limit.md) het gebruik van bestedings limieten om te voor komen dat u overbesteedt.
- Begin met [het analyseren van uw Azure-kosten](../cost-management/quick-acm-cost-analysis.md).
