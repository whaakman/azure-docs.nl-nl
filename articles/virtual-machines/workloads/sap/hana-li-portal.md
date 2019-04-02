---
title: Azure HANA grote instanties beheren via Azure portal | Microsoft Docs
description: Wordt beschreven hoe u de manier waarop u kunt identificeren en communiceren met Azure HANA grote instanties via portal
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/01/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b186aa2692033a774d1d8f294315fcc0f5e874d5
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58763192"
---
# <a name="azure-hana-large-instances-control-through-azure-portal"></a>Azure HANA grote instanties beheer via Azure portal
In dit document bevat informatie over de manier hoe [HANA grote instanties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) worden voorgesteld in [Azure-portal](https://portal.azure.com) en welke activiteiten kunnen worden uitgevoerd via Azure portal met HANA grote instantie eenheden die zijn geïmplementeerd. De zichtbaarheid van HANA grote instanties in Azure portal wordt geboden via een Azure-resourceprovider voor HANA grote instanties, die momenteel in openbare preview

## <a name="display-of-hana-large-instance-units-in-the-azure-portal"></a>Weergave van HANA grote instantie eenheden in Azure portal
Bij het indienen van een aanvraag voor HANA grote instantie, kunt u wordt gevraagd om op te geven van de Azure-abonnement dat u verbinding met de HANA grote instanties ook maakt. Het verdient aanbeveling gebruik van hetzelfde abonnement die u gebruikt voor het implementeren van de SAP-toepassingslaag die op basis van de eenheden HANA grote instantie werkt.
Als uw eerste HANA grote instanties zijn ophalen geïmplementeerd, een nieuwe [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) wordt gemaakt in de Azure-abonnement u hebt ingediend in de implementatieaanvraag voor uw HANA grote instanties.  De nieuwe resourcegroep wordt een lijst al uw HANA grote instantie eenheden die u hebt geïmplementeerd in het specifieke abonnement.

Als u wilt zoeken in de nieuwe Azure-resourcegroep, u een lijst met de resourcegroep waarin uw abonnement door te navigeren door het navigatiedeelvenster links in Azure portal

![Navigatiedeelvenster in Azure portal](./media/hana-li-portal/portal-resource-group.png)

In de lijst met resourcegroepen, u worden gesteld, moet u mogelijk om te filteren op het abonnement dat u hebt gebruikt om HANA grote instanties zijn geïmplementeerd

![Resourcegroepen in Azure portal filteren](./media/hana-li-portal/portal-filtering-subscription.png)

Na het filteren op het juiste abonnement, u mogelijk nog een lange lijst met resourcegroepen. Zoek naar een met een na correctie van **- Txxx** waarbij "xxx" drie cijfers, zoals **-T050**. 

Als u de resourcegroep hebt gevonden, geef de details van deze. De lijst die u hebt ontvangen kan er als volgt uitzien:

![Lijst met HLI in Azure portal](./media/hana-li-portal/portal-hli-units-list.png)

Alle eenheden die vermeld zijn die één HANA grote instantie eenheid die is geïmplementeerd in uw abonnement. In dit geval kijken u acht verschillende HANA grote instantie eenheden, die zijn geïmplementeerd in uw abonnement.


## <a name="look-at-attributes-of-single-hli-unit"></a>Kenmerken van één HLI eenheid bekijken
U kunt in de lijst van de eenheden HANA grote instantie, klikt u op één eenheid en gaan naar de details van de eenheid van HANA grote instantie. 

U ontvangt een presentatie van de eenheid, waarmee ziet als eruit in het overzichtsscherm:

![Overzicht van een eenheid HLI weergeven](./media/hana-li-portal/portal-show-overview.png)

Kijken naar de verschillende kenmerken die worden weergegeven, zien deze kenmerken nauwelijks anders dan de kenmerken van de virtuele machine van Azure. Op de kop van de zijde links toont het de resourcegroep, Azure-regio, naam van abonnement en -ID, evenals enkele codes die u hebt toegevoegd. De eenheden HANA grote instantie hebben standaard geen code die is toegewezen. Aan de rechterkant van de koptekst, wordt de naam van de eenheid vermeld als toegewezen wanneer de implementatie is uitgevoerd. Het besturingssysteem wordt weergegeven en het IP-adres. Als met virtuele machines typt u de HANA grote instantie-eenheid met het aantal CPU wordt-threads en het geheugen weergegeven en. Meer informatie over de verschillende HANA grote instantie-eenheden, worden hier weergegeven:

- [Beschikbare SKU's voor HLI](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-available-skus)
- [Architectuur voor SAP HANA (grote instanties)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-storage-architecture) 

Een extra veld in de rechterkolom van de koptekst informeert over de energiestatus van de HANA grote instantie-eenheid.

> [!NOTE]
> De energiestatus beschrijft of de hardware-eenheid is ingeschakeld in- of uitschakelen. Deze geeft geen informatie over het besturingssysteem wordt omhoog en uitgevoerd. Als u een HANA grote instantie eenheid opnieuw start, wordt er een korte periode waarin de status van de eenheid is gewijzigd in **vanaf** verplaatsen naar de status van **gestart**. In de status van **gestart** betekent dat het besturingssysteem wordt gestart of dat het besturingssysteem volledig is is opgestart. Als gevolg hiervan, na het opnieuw opstarten van de eenheid, kan niet verwachte onmiddellijk Meld u aan bij de eenheid als de status verandert in een **gestart**.
> 


## <a name="check-activities-of-a-single-hana-large-instance-unit"></a>Activiteiten van één HANA grote instantie eenheid controleren 
Na het geeft een overzicht van de eenheden HANA grote instantie, kunt u activiteiten van de specifieke unit controleren. Een activiteitenlogboek kan er als volgt uitzien:

![Navigatiedeelvenster in Azure portal](./media/hana-li-portal/portal-activity-list.png)

Een van de belangrijkste activiteiten vastgelegd opnieuw opstarten van een eenheid zijn. De gegevens die worden vermeld, waaronder de status van de activiteit, de tijdstempel die de activiteit is geactiveerd, de abonnements-ID van die de activiteit is geactiveerd en de Azure-gebruiker die de activiteit geactiveerd. 

Een andere activiteit die wordt ophalen geregistreerd zijn wijzigingen aangebracht in de eenheid in de Azure-metagegevens. Naast het opnieuw opstarten geïnitieerd, ziet u de activiteit van **schrijven HANAInstances**. Dit type activiteit wordt uitgevoerd zonder wijzigingen voor de eenheid HANA grote instantie zelf, maar is waarin u wijzigingen in de metagegevens van de eenheid in Azure. In het geval is vermeld, we hebben toegevoegd en verwijderd van een tag (Zie volgende sectie).

## <a name="add-and-delete-an-azure-tag-to-a-hana-large-instance-unit"></a>Toevoegen en verwijderen van een Azure-tag bevindt op een eenheid van HANA grote instantie
Een andere mogelijkheid die u hebt, is het toevoegen van een [tag](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) aan een eenheid HANA grote instantie. De manier waarop tags zijn ophalen van toegewezen verschilt niet van tags toewijzen aan virtuele machines. Als met de labels aanwezig zijn in Azure metagegevens en voor HANA grote instanties, virtuele machines hebben dezelfde beperkingen als labels voor virtuele machines.

Verwijderen van tags werkt op dezelfde manier net als bij virtuele machines. Beide activiteiten, toe te passen en verwijderen van een label wordt weergegeven in het activiteitenlogboek van de specifieke HANA grote instantie-eenheid.

## <a name="check-properties-of-a-hana-large-instance-unit"></a>Controleer de eigenschappen van een eenheid HANA grote instantie
De sectie **eigenschappen** bevat belangrijke informatie die u ontvangt wanneer de exemplaren worden afgegeven aan u. Het is een sectie waarin u alle informatie die u nodig in de ondersteuning aanvragen of die u nodig hebt bij het instellen van de configuratie van opslag voor momentopnamen. In deze sectie wordt als zodanig een verzameling van gegevens over uw exemplaar, de connectiviteit van het exemplaar naar Azure en de opslag back-end. De bovenkant van de sectie ziet eruit zoals:


![bovenste deel van de eigenschappen HLI in Azure portal](./media/hana-li-portal/portal-properties-top.png)

De eerste paar gegevensitems, hebt u gezien in het overzichtsscherm al. Maar een belangrijk onderdeel van de gegevens is de ExpressRoute-Circuit-ID, die wordt geleverd wanneer u de eerste geïmplementeerde eenheden afgegeven. In sommige gevallen ondersteuning kan u ophalen gevraagd om die gegevens. Een vermelding van belangrijke gegevens wordt aan de onderkant van de schermafbeelding weergegeven. De gegevens die worden weergegeven, is het IP-adres van de NFS-opslag-kop die worden geïsoleerd van uw opslagruimte te uw **tenant** in de stack HANA grote instantie. Dit IP-adres is bovendien vereist wanneer u bewerkt de [configuratiebestand voor de opslag momentopname maken van back-ups](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore#set-up-storage-snapshots). 

Als u naar beneden in het deelvenster met de eigenschap schuift, dat u aanvullende gegevens, zoals een unieke resource-ID voor uw HANA grote instantie-eenheid of de abonnements-ID die is toegewezen aan de implementatie.

## <a name="restart-a-hana-large-instance-unit-through-azure-portal"></a>Opnieuw opstarten van een eenheid HANA grote instantie via Azure portal
Opnieuw opstarten van het Linux-besturingssysteem wordt gestart, zijn verschillende situaties waarin het besturingssysteem kan niet worden voltooid opnieuw worden opgestart. U nodig om af te dwingen een opnieuw opstarten, opent u een verzoek om Microsoft-bewerkingen uitvoeren van kracht opnieuw opstarten van de eenheid HANA grote instantie. De functionaliteit van kracht opnieuw opstarten van een eenheid HANA grote instantie is nu geïntegreerd in Azure portal. Terwijl u in het overzicht van de eenheid HANA grote instantie bent, ziet u de knop voor het opnieuw opstarten boven op het gedeelte met gegevens

![Stap 1 # starten in Azure portal](./media/hana-li-portal/portal-restart-first-step.png)

Als u bent u op de knop opnieuw opstarten, kunt u wordt gevraagd of u echt wilt starten van de eenheid. Als u bevestigt dat door 'Ja' op de knop te drukken, kunnen de eenheid wordt opnieuw opgestart.

> [!NOTE]
> In de computer opnieuw wordt gestart, wordt er een korte periode waarin de status van de eenheid is gewijzigd in **vanaf** verplaatsen naar de status van **gestart**. In de status van **gestart** betekent dat het besturingssysteem wordt gestart of dat het besturingssysteem volledig is is opgestart. Als gevolg hiervan, na het opnieuw opstarten van de eenheid, kan niet verwachte onmiddellijk Meld u aan bij de eenheid als de status verandert in een **gestart**.


## <a name="open-a-support-request-for-hana-large-instances"></a>Open een ondersteuningsaanvraag voor HANA grote instanties
Buiten de Azure portal weergave van HANA grote instantie eenheden, kunt u ondersteuningsaanvragen specifiek voor een HANA grote instantie eenheid ook maken. Als u de koppeling volgen **nieuwe ondersteuningsaanvraag** 

![starten van service-aanvraag stap #1 in Azure portal](./media/hana-li-portal/portal-initiate-support-request.png)

Om de service van SAP HANA grote instanties die worden vermeld in het volgende scherm, moet u mogelijk selecteren ' alle Services ' zoals hieronder wordt weergegeven

![Selecteer alle services in Azure portal](./media/hana-li-portal/portal-create-service-request.png)

In de lijst met services, vindt u de service **SAP HANA grote instantie**. Als u ervoor die service kiest, kunt u specifieke typen kunt selecteren, zoals wordt weergegeven:


![Probleem-klasse selecteren in Azure portal](./media/hana-li-portal/portal-select-problem-class.png)

Bij elk van de verschillende typen krijgt u een selectie van het probleem subtypen die u moet selecteren om aan te geven van het probleem verder. Na het selecteren van het subtype, kunt u nu het onderwerp van de naam. Wanneer u klaar bent met het proces voor het selecteren, kunt u verplaatsen naar volgende stap van het maken. In de **oplossingen** sectie u worden verwezen naar documentatie over HANA grote instanties, die een verwijzing naar een oplossing van uw probleem mogelijk geven. Als u een oplossing voor uw probleem in de documentatie van voorgestelde vinden kan, gaat u naar de volgende stap. In de volgende stap gaat u gevraagd of het probleem met VM's of met HANA grote instantie eenheden is. Dit helpt om de ondersteuningsaanvraag aan de juiste specialisten te regelen. 

![Details van de ondersteuningsaanvraag in Azure portal](./media/hana-li-portal/portal-support-request-details.png)

Beantwoord de vragen en aanvullende informatie verstrekt, kunt u de volgende stap gaan om te controleren van de ondersteuningsaanvraag en het indienen deze.

## <a name="next-steps"></a>Volgende stappen

- [Het bewaken van SAP HANA (grote instanties) op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)
- [Bewaken en problemen oplossen vanaf de HANA-zijde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-monitor-troubleshoot)

