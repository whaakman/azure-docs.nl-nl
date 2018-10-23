---
title: Tabblad van de virtuele machine SKU's in de Cloud Partner-Portal voor Azure Marketplace | Microsoft Docs
description: Hierin wordt beschreven in het tabblad SKU's is gebruikt bij het maken van een virtuele machine-aanbieding in Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 4ecc259d40cdcba93a484f27e27191e967f10ff1
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639685"
---
# <a name="virtual-machine-skus-tab"></a>Tabblad voor virtuele machine-SKU 's

De **SKU's** tabblad van de **nieuwe aanbieding** pagina kunt u een of meer SKU's maken en deze koppelt aan uw nieuwe aanbieding.  Verschillende SKU's kunnen een onderscheid maken een oplossing door functiesets, VM-installatiekopie-typen, doorvoer of schaalbaarheid, factureringsmodellen of andere kenmerken.

## <a name="create-a-sku"></a>Een SKU maken

In eerste instantie wordt een nieuwe aanbieding heeft geen alle bijbehorende SKU's, zodat u door te klikken op **nieuwe Voorraadeenheid**.

![Nieuwe SKU-knop op het tabblad nieuwe aanbieding voor virtuele machines](./media/publishvm_005.png)

<br/>

De **nieuwe Voorraadeenheid** in het dialoogvenster wordt weergegeven.  Voer de id voor de nieuwe SKU en klik vervolgens op **OK**. (Zie hieronder voor id-naamconventies.)  De **SKU's** tabblad wordt nu beschikbaar voor het bewerken van velden worden weergegeven.    Een toegevoegde sterretje (*) op de veldnaam van het geeft aan dat dit vereist is.

<!-- TD: This tab has been updated, now has "Old Pricing" and "Simplified Currency Pricing" sections"! -->

![SKU-tabblad voor op het formulier nieuwe aanbieding voor virtuele machines](./media/publishvm_006.png)

De volgende tabel beschrijft het doel, inhoud en opmaak van deze velden.

<!-- TD: I took a new screenshot, and the fields differ somewhat from description in the VM Pub Guide.  Needs review. -->

|  **Veld**       |     **Beschrijving**                                                          |
|  ---------       |     ---------------                                                          |
|  *SKU-instellingen*   |  |
| **SKU-ID**       | De id voor deze SKU.  Deze naam is een maximum van 50 tekens, die bestaan uit kleine letters, alfanumerieke tekens en streepjes (-), maar mag niet eindigen met een streepje.  Worden kan niet gewijzigd nadat de aanbieding is gepubliceerd.  |
|  *SKU-Details*   |  |
| **Titel**        | Beschrijvende naam voor de aanbieding om weer te geven in de marketplace. Maximale lengte van maximaal 50 tekens. |
| **Samenvatting**      | Beknopte beschrijving van de aanbieding om weer te geven in de marketplace. Maximale lengte van 100 tekens. |
| **Beschrijving**  | Beschrijvende tekst waarmee een meer gedetailleerde uitleg van de aanbieding.  <!-- TD: max len/guidance? 3k characters -->  |
| **Deze SKU verbergen** | Geeft aan of de SKU weergegeven in de marketplace voor klanten worden moet.  U kunt de SKU verbergen als u wilt dat alleen het beschikbare alleen via oplossingssjablonen en niet voor aankoop afzonderlijk.  Het kan ook nuttig voor het eerste testen of voor tijdelijke of seizoensgebonden aanbiedingen zijn. |
| **Beschikbaarheid van de cloudservice** | Hiermee bepaalt u welke cloud het SKU beschikbaar moet zijn.  De standaardwaarde is de openbare versie van Azure.  Microsoft Azure Government is een overheidscommunitycloud met beperkte toegang voor de Amerikaanse federale overheid, status, lokale of Tribale overheden en hun gecertificeerde partners.  Zie voor meer informatie over overheidscloud, [Welkom bij Azure Government](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome). |
| **Is dit een privé-SKU?** | Geeft aan of de SKU privé of openbaar is. De standaardwaarde is **Nee** (openbare).  Zie voor meer informatie, [openbare en privé-SKU's](../../cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md). |
| **Beschikbaarheid van het land/regio** | Hiermee bepaalt u welke landen of wereldregio's uw SKU zijn beschikbaar voor aankoop. Selecteer ten minste één regio/land/regio. <!-- TD: Is this parameter an AMP visibility control or a contractual one, or both? --> |  
|  *Prijzen*   |  |
| **Licentiemodel**| Gestandaardiseerde facturering model te gebruiken.  Als u selecteert **maandelijks op basis van gebruik gefactureerd SKU**, een accordeon sectie wordt geopend zodat u kunt details van prijzen per kern opgeven en of u wilt bieden een gratis proefperiode is afgelopen.  In deze sectie kunt u ook om te exporteren en importeren van deze prijscategorie in Excel. Zie voor meer informatie, [opties voor facturering in Azure Marketplace](../../billing-options-azure-marketplace.md). | 
|  *VM-installatiekopieën*   |  |
| **Besturingssystemen** | Hiermee wordt aangegeven of de virtuele machine van de oplossing Windows of Linux gebaseerde. |
| **Besturingssysteemtype selecteren** | Specifieke leverancier of versie van het opgegeven besturingssysteem. |
| **Gebruiksvriendelijke naam** | De naam van het besturingssysteem moet worden weergegeven aan klanten.  |
| **Aanbevolen VM-grootten** | Selectie van maximaal zes aanbevolen VM-grootten in een gestandaardiseerde lijst kunt.  Hoewel deze aanbevelingen prominent op potentiële klanten weergegeven worden, kunnen worden om op te geven van elke VM-grootte die compatibel is met de installatiekopie van de oplossing. | 
| **Poorten openen**| De poorten te openen en ter ondersteuning van voor de SKU-protocol.  Deze configuraties moeten overeenkomen met het virtuele netwerk dat u hebt geconfigureerd voor het netwerk van de VM-oplossing. Deze instellingen gaan van kracht tijdens de implementatie van de virtuele machine. Instellingen van de poort kunnen echter worden gewijzigd nadat u een SKU publiceert. Zie voor meer informatie, [over het openen van poorten op een virtuele machine met de Azure-portal](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal). <br/>De volgende standaard-netwerkkoppelingen worden toegevoegd aan alle virtuele machines. &emsp; Windows: 3389 -> 3389 TCP, 5986 -> 5986 TCP; &emsp; Linux: 22 -> 22, TCP (SSH). |
| **Versie van schijf**  | Gekoppelde oplossing VM, opgegeven door het versienummer van de schijf en schijf-URL. De versie van de schijf moet zich in [semantische versie](http://semver.org/) indeling: `<major>.<minor>.<patch>`.  De URL is de shared access signature die URI voor het besturingssysteem VHD gemaakt.  Hoewel u maximaal acht versies van schijf per SKU toevoegt kunt, worden alleen de hoogste versie schijfnummer voor een SKU op Azure Marketplace weergegeven. De andere versies is alleen zichtbaar via API's.  <!--TD: Add more specific link to API --> <br/> De **nieuwe gegevensschijf** accordeon sectie kunt u maximaal 15 gegevensschijven koppelen aan uw virtuele machine.  Wanneer u een SKU met een bepaalde versie van de virtuele machine en de gekoppelde gegevensschijven publiceert, kan deze configuratie kan niet worden gewijzigd.  Als u meer versies van de virtuele machine toegevoegd aan de SKU, moeten ze ook hetzelfde aantal gegevensschijven ondersteunen. <br/> Als u uw Azure-gebaseerde VM-installatiekopieën niet hebt gemaakt, kunt u dit veld later bijwerken toevoegen.  Zie de sectie voor informatie over het maken van de bijbehorende VM-resource, [maken VM technische zaken](./cpp-create-technical-assets.md).  
|  |  |

<!-- TD: The CPP UX warning msg indicates that underscores are also supported in these SKU IDs. I suspect this might be true for other identifiers. --> 

<br/> Klik op **opslaan** uw voortgang opslaan. In het volgende tabblad u opgeeft als uw aanbieding ondersteunt [Test Drive](./cpp-test-drive-tab.md).


## <a name="additional-pricing-considerations"></a>Aanvullende overwegingen met betrekking tot prijzen

Het prijsmodel hierboven is een eenvoudige beschrijving.  Deze wijzigingen ondergaat en kan worden beïnvloed door lokale of regionale btw regelgeving en Microsoft prijsbeleid. 

### <a name="simplified-currency-pricing"></a>Vereenvoudigde valuta prijzen

Vanaf 1 September-2018, een nieuwe sectie met de naam **vereenvoudigd valuta prijzen** wordt toegevoegd aan de portal. Microsoft is de Azure Marketplace-bedrijven stroomlijnen door in te schakelen van meer voorspelbare prijzen en verzamelingen van uw klanten over de hele wereld. Deze stroomlijnen bevat het aantal valuta waarin wij factureren van uw klanten te verminderen.  Zie voor meer informatie, [Update een bestaande virtuele machine op Azure Marketplace bieden](./cpp-update-existing-offer.md).


### <a name="additional-information-on-taxes-and-prices"></a>Meer informatie over belastingen en prijzen

* Microsoft classificeert sommige landen, zoals *belasting overgeschreven landen*.  In dergelijke landen Microsoft belastingen voorheft van klanten en betaalt belastingen (mandaten) aan de overheid.  Partners zijn doorgaans verantwoordelijk voor het verzamelen van de belasting van hun klanten en belastingen betalen aan de overheid in andere landen. Als u ervoor kiest om te mogen verkopen in de laatste landen, moet u de mogelijkheid om te berekenen en lokale belastingen betalen hebben.  <!-- TD: Find a good reference on taxing policies. The best I found was in the UWP section: https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps -->
* Prijzen zijn niet mag worden gewijzigd zodra een aanbieding live meteen. U kunt echter nog steeds toevoegen of verwijderen van ondersteunde regio's. 
* Kosten van Microsoft de klant standard Virtuele machine van Azure-gebruikskosten naast uw geplande SKU-kosten.
* Prijzen zijn ingesteld voor alle regio's in de lokale valuta op wisselkoersen beschikbaar op het moment van prijzen.  <!-- TD: Meaning? - Offer created, published, other? -->
* Als u wilt de prijs van elke regio afzonderlijk instellen, kunt de prijsstelling spreadsheet exporteren, aangepaste prijzen van toepassing klikken en vervolgens importeren. 

<!-- TD: The detailed information in the table and supplemental notes should be centralized in another topic, maybe "Billing Options" in AMP tree. Need to include a common section on export/import pricing-->

