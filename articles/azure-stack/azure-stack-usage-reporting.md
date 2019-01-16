---
title: Azure Stack-gebruiksgegevens naar Azure rapport | Microsoft Docs
description: Meer informatie over het instellen van gebruiksgegevens rapportage in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: 147242e7d5d62204198360f580848cd997850d09
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332520"
---
# <a name="report-azure-stack-usage-data-to-azure"></a>Rapport van Azure Stack-gebruiksgegevens naar Azure

Gegevens over gebruik, ook wel genoemd verbruiksgegevens, vertegenwoordigt de hoeveelheid resources die worden gebruikt.

Azure Stack met meerdere knooppunten-systemen die gebruikmaken van het factureringsmodel op basis van gebruik moeten gegevens over gebruik rapporteren naar Azure voor factureringsdoeleinden. Azure Stack-operators moeten hun Azure Stack-exemplaar om rapport gebruiksgegevens naar Azure te configureren.

> [!IMPORTANT]
> Alle werkbelastingen [moet worden geïmplementeerd onder de tenant-abonnementen](#are-users-charged-for-the-infrastructure-vms) om te voldoen aan de licentievoorwaarden van Azure Stack.

Rapportage van gebruik van gegevens is vereist voor de gebruikers van de Azure Stack-meerdere knooppunten met een licentie onder het model betalen als u-gebruik. Het is optioneel voor klanten die een licentie onder het model capaciteit (Zie de [over het aanschaffen van pagina](https://azure.microsoft.com/overview/azure-stack/how-to-buy/). Voor gebruikers van Azure Stack Development Kit, Azure Stack-operators rapporteren van gegevens over gebruik en de functie testen. Echter gebruikers niet gefactureerd voor het gebruik die ze in rekening worden gebracht.

![Facturering stroom](media/azure-stack-usage-reporting/billing-flow.png)

Gegevens over het gebruik wordt naar Azure via de Azure-brug verzonden vanaf Azure Stack. In Azure, de commerce-systeem verwerkt de gegevens over gebruik en de factuur wordt gegenereerd. Nadat de factuur is gegenereerd, de eigenaar van de Azure-abonnement kunt weergeven en downloaden via de [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions). Zie voor meer informatie over hoe Azure Stack is een licentie heeft, de [Azure Stack verpakking en prijzen document](https://go.microsoft.com/fwlink/?LinkId=842847).

## <a name="set-up-usage-data-reporting"></a>Gebruiksrapportage voor gegevens instellen

Als u gegevens gebruiksrapportage instelt, moet u [registreren van uw Azure Stack-exemplaar met Azure](azure-stack-register.md). Als onderdeel van het registratieproces, is het Azure-Bridge-onderdeel van Azure Stack, die verbinding maakt Azure Stack met Azure en verzendt gegevens over gebruik, geconfigureerd. De volgende gegevens over gebruik wordt verzonden vanaf de Azure Stack naar Azure:

- **ID meten** – unieke ID voor de resource die is verbruikt.
- **Aantal** – hoeveelheid van het gebruik van bronnen.
- **Locatie** – de locatie waar de huidige Azure Stack-resource is geïmplementeerd.
- **Resource-URI** – volledig gekwalificeerde URI van de resource waarvoor gebruik wordt gerapporteerd.
- **Abonnements-ID** – abonnements-ID van de Azure Stack-gebruiker die de lokale (Azure Stack)-abonnement.
- **Tijd** : begin- en -tijd van de gebruiksgegevens. Er is enige vertraging tussen het moment wanneer deze resources worden gebruikt in Azure Stack, waarbij de gebruiksgegevens wordt gerapporteerd aan commerce. Duurt enkele uren in een andere Azure Stack statistische functies gebruiksgegevens voor elke 24 uur en rapportage gebruiksgegevens aan de commerce-pijplijn in Azure. Dus gebruik die wordt weergegeven over enkele ogenblikken vóór middernacht mogelijk weergegeven in Azure de volgende dag.

## <a name="generate-usage-data-reporting"></a>Rapportage van gebruik gegevens genereren

- Als u wilt testen rapportage over gebruiksgegevens, enkele resources te maken in Azure Stack. Bijvoorbeeld, kunt u een [opslagaccount](azure-stack-provision-storage-account.md), [Windows Server-VM](azure-stack-provision-vm.md) en een Linux-VM met Basic en Standard-SKU's om te zien hoe core-gebruik wordt gerapporteerd. Gegevens over gebruik voor verschillende soorten resources worden onder een andere manier gerapporteerd.

- Laat uw resources voor een paar uur uitgevoerd. Informatie over het gebruik is ongeveer één keer per uur worden verzameld. Na het verzamelen, is deze gegevens naar Azure verzonden en verwerkt in het Azure commerce-systeem. Dit proces kan enkele uren duren.

## <a name="view-usage---csp-subscriptions"></a>Gebruik van weergeven - CSP-abonnementen

Als u uw Azure-Stack met behulp van een CSP-abonnement hebt geregistreerd, kunt u uw gebruik en kosten bekijken op dezelfde manier als waarop u Azure-verbruik bekijken. Gebruik van Azure Stack worden opgenomen in uw factuur en in het afstemmingsbestand, beschikbaar via de [Partner Center](https://partnercenter.microsoft.com/partner/home). Het afstemmingsbestand wordt maandelijks bijgewerkt. Als u nodig hebt voor toegang tot gebruiksgegevens van recente Azure Stack, kunt u de Partner Center API's gebruiken.

![partnercentrum](media/azure-stack-usage-reporting/partner-center.png)

## <a name="view-usage--enterprise-agreement-subscriptions"></a>Gebruik van weergave: Enterprise Agreement-abonnementen

Als u uw Azure-Stack met behulp van een Enterprise Agreement-abonnement hebt geregistreerd, kunt u bekijken uw gebruik en de kosten in de [EA-portal](https://ea.azure.com/). Gebruik van Azure Stack is opgenomen in de geavanceerde downloads samen met gebruik van Azure onder de sectie rapporten in deze portal. 

## <a name="view-usage--other-subscriptions"></a>Gebruik van weergave: andere abonnementen

Als u uw Azure Stack geregistreerd met behulp van een ander abonnement dan invoert. u kunt bijvoorbeeld een abonnement met betalen per gebruik en de kosten weergeven in het Azure-accountcentrum. Aanmelden bij de [Azure-Accountcentrum](https://account.windowsazure.com/Subscriptions) als de Azure account beheerder en selecteert u de Azure-abonnement dat u gebruikt voor het registreren van de Azure Stack. U vindt de gebruiksgegevens van Azure Stack, het bedrag in rekening gebracht voor elk van de gebruikte resources, zoals wordt weergegeven in de volgende afbeelding:

![Facturering stroom](media/azure-stack-usage-reporting/pricing-details.png)

Voor de Azure Stack Development Kit, worden Azure Stack-resources niet gefactureerd, zodat de prijs wordt weergegeven als $0,00.

## <a name="which-azure-stack-deployments-are-charged"></a>Welke Azure Stack-implementaties worden in rekening gebracht?

Resourcegebruik is gratis voor de Azure Stack Development Kit. Azure Stack-systemen voor meerdere knooppunten, werkbelasting van virtuele machines, Storage-services en App-Services, worden in rekening gebracht.

## <a name="are-users-charged-for-the-infrastructure-vms"></a>Zijn gebruikers in rekening gebracht voor de infrastructuur-VM's?

Nee. Gegevens over gebruik voor sommige Azure Stack-resourceprovider VM's wordt gerapporteerd aan Azure, maar er zijn geen kosten voor deze virtuele machines, noch voor de virtuele machines die zijn gemaakt tijdens de implementatie om in te schakelen van de Azure Stack-infrastructuur.  

Gebruikers betalen alleen voor virtuele machines die worden uitgevoerd onder de tenant-abonnementen. Alle werkbelastingen moeten worden geïmplementeerd onder de tenant-abonnementen om te voldoen aan de licentievoorwaarden van Azure Stack.

## <a name="i-have-a-windows-server-license-i-want-to-use-on-azure-stack-how-do-i-do-it"></a>Ik heb een Windows Server-licentie die ik wil gebruiken in Azure Stack, hoe kan ik dat doen?

Met behulp van de bestaande licenties voorkomt het genereren van meters in gebruik. Bestaande Windows Server-licenties kunnen worden gebruikt in Azure Stack, zoals beschreven in de sectie 'Met behulp van bestaande software met Azure Stack' van de [Azure Stack-licentiehandleiding](https://go.microsoft.com/fwlink/?LinkId=851536). Klanten moeten hun virtuele machines van Windows Server implementeren, zoals beschreven in de [Hybrid benefit voor Windows Server-licentie](../virtual-machines/windows/hybrid-use-benefit-licensing.md) artikel om te kunnen hun bestaande licenties te gebruiken.

## <a name="which-subscription-is-charged-for-the-resources-consumed"></a>Welk abonnement wordt in rekening gebracht voor de resources die nodig zijn?

Het abonnement dat is opgegeven bij [Azure Stack registreren bij Azure](azure-stack-register.md) wordt in rekening gebracht.

## <a name="what-types-of-subscriptions-are-supported-for-usage-data-reporting"></a>Welke typen abonnementen worden ondersteund voor het gebruik van gegevens melden?

Voor Azure Stack met meerdere knooppunten, worden Enterprise Agreement (EA) en CSP-abonnementen ondersteund. Voor de Azure Stack Development Kit ondersteuning voor Enterprise Agreement (EA), betalen per gebruik, CSP en MSDN-abonnementen gegevens rapportage over het gebruik.

## <a name="does-usage-data-reporting-work-in-sovereign-clouds"></a>Werk in onafhankelijke clouds rapportage over gebruiksgegevens gebruikt?

In de Azure Stack Development Kit vereist gebruiksrapportage voor gegevens abonnementen die zijn gemaakt in de globale Azure-systeem. Abonnementen die zijn gemaakt in een van de soevereine clouds (Azure Government, Azure Duitsland en Azure China clouds) kunnen niet worden geregistreerd met Azure, zodat ze bieden geen ondersteuning voor rapportage van gegevens over het gebruik.

## <a name="why-doesnt-the-usage-reported-in-azure-stack-match-the-report-generated-from-azure-account-center"></a>Waarom niet het gebruik in Azure Stack gerapporteerd overeenkomt met het rapport gegenereerd op basis van Azure-Accountcentrum?

Er is altijd een vertraging tussen de gebruiksgegevens die zijn gerapporteerd door het gebruik van Azure Stack API's en het van gebruiksgegevens die zijn gerapporteerd door het Azure-Accountcentrum. Deze vertraging is de tijd die nodig is voor het uploaden van gebruiksgegevens van Azure Stack op Azure commerce. Vanwege deze vertraging gebruik die wordt weergegeven over enkele ogenblikken vóór middernacht mogelijk weergegeven in Azure de volgende dag. Als u de [Azure Stack-gebruik API's](azure-stack-provider-resource-api.md), en vergelijk de resultaten het gebruik van gerapporteerd in de Azure-factureringsportal, ziet u een verschil.

## <a name="next-steps"></a>Volgende stappen

* [API voor providergebruik](azure-stack-provider-resource-api.md)  
* [API voor tenantgebruik](azure-stack-tenant-resource-usage-api.md)
* [Veelgestelde vragen over gebruik](azure-stack-usage-related-faq.md)
* [Beheren van gebruik en facturering als een Cloudserviceprovider](azure-stack-add-manage-billing-as-a-csp.md)
