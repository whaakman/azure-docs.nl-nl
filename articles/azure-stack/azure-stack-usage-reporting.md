---
title: Azure-Stack gebruiksgegevens rapporteren aan Azure | Microsoft Docs
description: Informatie over het instellen van rapportage in Azure Stack gebruiksgegevens.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 2A397C83-1A6C-4290-B0EF-FDC34EE290CD
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: mabrigg;AlfredoPizzirani
ms.openlocfilehash: 451e8c604e48d9cfc42284f2413fa5ed4fea03bc
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="report-azure-stack-usage-data-to-azure"></a>Azure Stack gebruiksgegevens rapporteren naar Azure 

Gegevens over het gebruik, ook wel verbruiksgegevens, vertegenwoordigt de hoeveelheid resources die worden gebruikt. 

Azure Stack met meerdere knooppunten systemen die gebruik maken op basis factureringsmodel verbruik moeten rapporteren gebruiksgegevens Azure voor facturering doel.  Azure Stack-operators moeten hun Azure Stack instantie geconfigureerd met gebruiksgegevens rapport naar Azure.

> [!NOTE]
> Gegevens gebruiksrapportage is vereist voor de Azure-Stack met meerdere knooppunten-gebruikers die een licentie onder het Pay-as-u-use-model. Dit is optioneel voor klanten die een licentie onder het model capaciteit (Zie de [aanschaffen pagina](https://azure.microsoft.com/overview/azure-stack/how-to-buy/ to learn more about pricing in Azure Stack)). Voor gebruikers van Azure Stack Development Kit Azure Stack operators gebruiksgegevens rapporteren en testen van de functie. Echter gebruikers wordt niet in rekening gebracht voor enig gebruik die ze rekening worden gebracht. 


![Facturering stroom](media/azure-stack-usage-reporting/billing-flow.png)

Gegevens over het gebruik van Azure-Stack verzonden naar Azure via de Azure-brug. In Azure, wordt het systeem commerce verwerkt de gebruiksgegevens en genereert de factuur. Nadat de factuur is gegenereerd, de eigenaar van de Azure-abonnement kunt weergeven en downloaden via de [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions). Raadpleeg voor meer informatie over hoe Azure-Stack is in licentie gegeven aan de [Azure Stack verpakken en prijzen document](https://go.microsoft.com/fwlink/?LinkId=842847&clcid=0x409).

## <a name="set-up-usage-data-reporting"></a>Gegevens gebruiksrapportage instellen

Als u gegevens gebruiksrapportage instelt, moet u [registreren van uw Azure-Stack-exemplaar met Azure](azure-stack-register.md). Als onderdeel van het registratieproces, is het onderdeel Azure Bridge van Azure-Stack die Azure-Stack verbindt met Azure en de gebruiksgegevens verzendt, geconfigureerd. De volgende gegevens over het gebruik wordt door Azure Stack verzonden naar Azure:

- **ID meten** – unieke ID voor de resource die is verbruikt.
- **Aantal** – hoeveelheid Resourcegebruik.
- **Locatie** – locatie waar de huidige Azure-Stack-resource is geïmplementeerd.
- **Resource-URI** – volledig gekwalificeerde URI van de resource waarvoor gebruik wordt gerapporteerd.
- **Abonnements-ID** – abonnements-ID van de Azure-Stack-gebruiker. Dit is een abonnement op de lokale (Azure-Stack).
- **Tijd** – begin- en tijd van de gebruiksgegevens. Er is enige vertraging tussen het moment wanneer deze bronnen worden verbruikt in Azure-Stack en wanneer de gebruiksgegevens naar commerce wordt gemeld. Duurt enkele uren in een andere Azure Stack statistische gegevens over het gebruik voor elke 24 uur reporting gegevens en gebruiksgegevens om commerce pijplijn in Azure. Dus gebruik deze gebeurtenis treedt op vóór middernacht kort mogelijk weergegeven in Azure de volgende dag.

## <a name="generate-usage-data-reporting"></a>Rapportage over het gebruik gegevens genereren

1. Als u wilt testen gebruiksgegevens reporting, een paar resources te maken in Azure-Stack. Bijvoorbeeld, kunt u een [opslagaccount](azure-stack-provision-storage-account.md), [Windows Server-VM](azure-stack-provision-vm.md) en een Linux-VM met Basic en Standard-SKU's om te zien hoe core gebruik wordt gemeld. De gebruiksgegevens voor verschillende soorten resources zijn onder andere meters gerapporteerd.

2. Laat uw resources voor uitvoering voor enkele uren. Gebruiksgegevens worden verzameld ongeveer één keer per uur. Na het verzamelen, worden deze gegevens verzonden naar Azure en verwerkt in de Azure commerce-systeem. Dit proces kan enkele uren duren.

## <a name="view-usage---csp-subscriptions"></a>Gebruik in de weergave - CSP-abonnementen

Als u uw Azure-Stack met behulp van een CSP-abonnement hebt geregistreerd, kunt u uw gebruik en de kosten kunt weergeven op dezelfde manier waarop u Azure-verbruik weergeven. Azure Stack-gebruik worden opgenomen in uw factuur is vermeld en in het bestand afstemming via [Partnercentrum](https://partnercenter.microsoft.com/partner/home). Het bestand afstemming wordt maandelijks bijgewerkt. Als u nodig hebt voor toegang tot gebruiksgegevens van recente Azure Stack, kunt u de Partner Center API's.

   ![partnercentrum](media/azure-stack-usage-reporting/partner-center.png)


## <a name="view-usage--enterprise-agreement-subscriptions"></a>Gebruik in de weergave-Enterprise Agreement abonnementen

Als u uw Azure-Stack met behulp van een Enterprise Agreement-abonnement hebt geregistreerd, vindt u de informatie over het gebruik en de kosten in de [EA Portal](https://ea.azure.com/). Azure Stack-gebruik worden opgenomen in de geavanceerde downloads samen met Azure gebruik onder de sectie rapporten in de portal EA. 

## <a name="view-usage--other-subscriptions"></a>Gebruik in de weergave-andere abonnementen

Als u uw Azure-Stack met behulp van een ander abonnementstype, bijvoorbeeld een abonnement omslagstelsel geregistreerd kunt u informatie over het gebruik en kosten weergeven in het Azure-accountcentrum. Aanmelden bij de [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions) beheerder als de Azure-account in en selecteer de Azure-abonnement dat u gebruikt voor het registreren van de Azure-Stack. U kunt de Azure-Stack-gebruiksgegevens het bedrag in rekening gebracht voor elk van de gebruikte resources, zoals wordt weergegeven in de volgende afbeelding kunt bekijken:

   ![Facturering stroom](media/azure-stack-usage-reporting/pricing-details.png)

Voor de Azure-Stack Development Kit Stack Azure-resources zijn niet in rekening gebracht zodat de prijs wordt weergegeven als $0,00. Wanneer Azure Stack met meerdere knooppunten in het algemeen beschikbaar komt, kunt u de werkelijke kosten zien voor elk van deze resources.

## <a name="which-azure-stack-deployments-are-charged"></a>Welke Azure-Stack-implementaties worden in rekening gebracht?

Resourcegebruik is gratis voor Azure Stack Development Kit. Terwijl voor systemen met meerdere knooppunten Azure Stack, werkbelasting van virtuele machines, Storage-services en App-Services in rekening gebracht worden.

## <a name="are-users-charged-for-the-infrastructure-vms"></a>Worden gebruikers in rekening gebracht voor de infrastructuur van virtuele machines?

Nee. Gebruiksgegevens voor sommige Azure Stack-resourceprovider virtuele machines in Azure wordt gerapporteerd, maar er zijn geen kosten voor deze virtuele machines, noch voor de virtuele machines gemaakt tijdens de implementatie zodat de Azure-Stack-infrastructuur.  

Gebruikers worden alleen kosten in rekening gebracht voor virtuele machines die worden uitgevoerd onder de tenant-abonnementen. Alle werkbelastingen moeten worden geïmplementeerd onder tenant abonnementen om te voldoen aan de licentievoorwaarden van Azure-Stack.

## <a name="i-have-a-windows-server-license-i-want-to-use-on-azure-stack-how-do-i-do-it"></a>Ik heb een Windows Server-licentie die ik wil op Azure-Stack gebruiken, hoe kan ik dat doen?

Met behulp van de bestaande licenties voorkomt gebruik meters genereren. Bestaande Windows Server-licenties kunnen worden gebruikt in Azure-Stack, zoals beschreven in de sectie 'Met behulp van bestaande software met Azure Stack' van [de Azure-handleiding voor het licentieverlening van Stack](https://go.microsoft.com/fwlink/?LinkId=851536&clcid=0x409). Klanten willen hun virtuele machines van Windows Server implementeren, zoals beschreven in de [hybride-voordeel voor Windows Server-licentie](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) onderwerp om te kunnen gebruiken hun bestaande licenties.

## <a name="which-subscription-is-charged-for-the-resources-consumed"></a>Welke abonnement wordt in rekening gebracht voor de resources die worden gebruikt?
Het abonnement dat is geleverd wanneer [Azure Stack registreren bij Azure](azure-stack-register.md) wordt in rekening gebracht.

## <a name="what-types-of-subscriptions-are-supported-for-usage-data-reporting"></a>Welke typen abonnementen worden ondersteund voor het gebruik van gegevens melden?

Voor Azure-Stack-multinode worden Enterprise Agreement (EA) en CSP-abonnementen ondersteund. Voor de Azure-Stack Development Kit ondersteuning voor Enterprise Agreement (EA), betalen naar gebruik, CSP en MSDN-abonnementen gebruiksrapportage van gegevens.

## <a name="does-usage-data-reporting-work-in-sovereign-clouds"></a>Werk in soevereine clouds reporting gebruiksgegevens ondersteunt?

In de Azure-Stack Development Kit, gegevens gebruiksrapportage abonnementen die zijn gemaakt in het globale Azure systeem vereist. Abonnementen die zijn gemaakt in een van de soevereine clouds (Azure Government, Duitse Azure en Azure voor China clouds) kunnen niet worden geregistreerd met Azure, zodat ze niet gebruiksrapportage van gegevens ondersteunen.

## <a name="how-can-users-identify-azure-stack-usage-data-in-the-azure-billing-portal"></a>Hoe kunnen gebruikers Azure Stack gebruiksgegevens in de Azure portal facturering identificeren?

Gebruikers kunnen zien van de Azure-Stack-gebruiksgegevens in het gegevensbestand van het gebruik. Als u wilt weten over het ophalen van het gegevensbestand van het gebruik, verwijzen naar de [gebruik bestand downloaden van het Azure-Accountcentrum artikel](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-from-the-account-center-csv). Het gebruik van details-bestand bevat de Azure-Stack meters die Azure-Stack opslag en virtuele machines te identificeren. Alle resources die worden gebruikt in Azure-Stack worden gerapporteerd in het gebied met de naam 'Azure Stack'.

## <a name="why-doesnt-the-usage-reported-in-azure-stack-match-the-report-generated-from-azure-account-center"></a>Waarom niet het-gebruik gerapporteerd in de Azure-Stack overeenkomt met het rapport is gegenereerd op basis van de Azure-Accountcentrum?

Er is altijd een delaybetween de gebruiksgegevens die zijn gerapporteerd door het Azure-Stack gebruik API's en de gebruiksgegevens die zijn gerapporteerd door het Azure-Accountcentrum... Dit uitstel is de tijd die nodig zijn voor het uploaden van gebruiksgegevens van Azure-Stack naar Azure commerce. Als gevolg van deze vertraging gebruik binnenkort voor middernacht die mogelijk weergegeven in Azure de volgende dag. Als u de [API's voor informatie over het gebruik van Azure-Stack](azure-stack-provider-resource-api.md), en de resultaten met het gebruik gerapporteerd in de Azure-facturering portal vergelijken, ziet u een verschil.

## <a name="next-steps"></a>Volgende stappen

* [API voor providergebruik](azure-stack-provider-resource-api.md)  
* [API voor tenantgebruik](azure-stack-tenant-resource-usage-api.md)
* [Veelgestelde vragen over gebruik](azure-stack-usage-related-faq.md)
