---
title: Azure Reserved VM Instances korting begrijpen | Microsoft Docs
description: Meer informatie over hoe de korting voor gereserveerde VM-exemplaren van Azure wordt toegepast op actieve virtuele machines.
author: yashesvi
manager: yashar
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2019
ms.author: banders
ms.openlocfilehash: 191160035f516d818d5537c5c47f9604998c46f7
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849995"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Hoe de korting voor Azure-reserve ringen wordt toegepast op virtuele machines

Nadat u een gereserveerde instantie van een virtuele machine van Azure hebt gekocht, wordt de reserverings korting automatisch toegepast op virtuele machines die overeenkomen met de kenmerken en hoeveelheid van de reserve ring. Een reserve ring omvat de reken kosten van uw virtuele machines.

Een reserverings korting is van toepassing op de basis-Vm's die u aanschaft vanuit Azure Marketplace.

Zie meer informatie over gereserveerde [instanties van Azure](billing-understand-reservation-charges.md)voor SQL database gereserveerde capaciteit.

In de volgende tabel ziet u de kosten voor uw virtuele machine nadat u een gereserveerde VM-instantie hebt aangeschaft. In alle gevallen worden er kosten in rekening gebracht voor opslag en netwerken tegen de normale tarieven.

| Type virtuele machine  | Kosten met gereserveerde VM-instantie |
|-----------------------|--------------------------------------------|
|Linux-Vm's zonder extra software | De reserve ring geldt voor de kosten van uw VM-infra structuur.|
|Linux-Vm's met software kosten (bijvoorbeeld Red Hat) | De reserve ring heeft betrekking op de kosten van de infra structuur. Er worden kosten in rekening gebracht voor aanvullende software.|
|Windows-Vm's zonder aanvullende software |De reserve ring heeft betrekking op de kosten van de infra structuur. Er worden kosten in rekening gebracht voor Windows-software.|
|Windows-Vm's met aanvullende software (bijvoorbeeld SQL Server) | De reserve ring heeft betrekking op de kosten van de infra structuur. Er worden kosten in rekening gebracht voor Windows-software en voor aanvullende software.|
|Windows-Vm's met [Azure Hybrid Benefit](../virtual-machines/windows/hybrid-use-benefit-licensing.md) | De reserve ring heeft betrekking op de kosten van de infra structuur. De Windows-software kosten vallen onder de Azure Hybrid Benefit. Eventuele extra software wordt afzonderlijk in rekening gebracht.|

## <a name="how-reservation-discount-is-applied"></a>De manier waarop de reserverings korting wordt toegepast

Een reserverings korting is '*use-it-or-kwijtraakt-* '. Als u geen overeenkomende resources voor elk uur hebt, verliest u een reserverings hoeveelheid voor dat uur. U kunt niet-gebruikte gereserveerde uren niet door sturen.

Wanneer u een resource afsluit, wordt de reserverings korting automatisch toegepast op een andere overeenkomende resource in het opgegeven bereik. Als er geen overeenkomende resources worden gevonden in het opgegeven bereik, gaan de gereserveerde uren *verloren*.

## <a name="reservation-discount-for-non-windows-vms"></a>Reserverings korting voor niet-Windows Vm's

 De korting voor Azure-reserve ringen wordt op elk uur toegepast op actieve VM-exemplaren. De reserve ringen die u hebt aangeschaft, worden vergeleken met het gebruik dat door de actieve Vm's wordt gegenereerd om de reserverings korting toe te passen. Voor virtuele machines die het volledige uur niet uitvoeren, wordt de reserve ring opgevuld van andere Vm's die geen reserve ring gebruiken, inclusief gelijktijdig uitgevoerde Vm's. Aan het einde van het uur is de reserverings toepassing voor Vm's in het uur vergrendeld. In het geval dat een virtuele machine niet wordt uitgevoerd gedurende een uur of gelijktijdige Vm's binnen het uur, wordt het uur van de reserve ring niet gevuld, wordt de reserve ring voor dat uur ondergebruikt. In de volgende grafiek ziet u de toepassing van een reserve ring voor het factureer bare VM-gebruik. De afbeelding is gebaseerd op één reserverings aankoop en twee overeenkomende VM-exemplaren.

![Scherm opname van één toegepaste reserve ring en twee overeenkomende VM-exemplaren](./media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Het gebruik dat zich boven de reserverings regel bevindt, wordt in rekening gebracht op basis van de normale tarieven voor betalen naar gebruik. U betaalt geen gebruik onder de regel reserve ringen, omdat deze al is betaald als onderdeel van de aankoop van de reserve ring.
2. In uur 1 wordt instance 1 gedurende 0,75 uur uitgevoerd en wordt exemplaar 2 gedurende 0,5 uur uitgevoerd. Totaal gebruik voor uur 1 is 1,25 uur. U betaalt de betalen naar gebruik-tarieven voor de resterende 0,25 uur.
3. Voor uur 2 en 3 uur, hebben beide exemplaren elk één uur elke keer uitgevoerd. Een exemplaar wordt gedekt door de reserve ring en de andere wordt in rekening gebracht tegen betalen naar gebruik-tarieven.
4. Voor uur 4 wordt instance 1 gedurende 0,5 uur uitgevoerd en wordt exemplaar 2 gedurende 1 uur uitgevoerd. Exemplaar 1 wordt volledig gedekt door de reserve ring en 0,5 uur van instantie 2 wordt gedekt. Het tarief voor betalen per gebruik wordt in rekening gebracht voor de resterende 0,5 uur.

Zie inzicht in het [gebruik van reserve ringen](billing-understand-reserved-instance-usage-ea.md)voor informatie over en het weer geven van de toepassing van uw Azure Reservations in rapporten over facturerings gebruik.

## <a name="reservation-discount-for-windows-vms"></a>Reserverings korting voor Windows-Vm's

Wanneer u Windows VM-exemplaren uitvoert, wordt de reserve ring toegepast voor de kosten van de infra structuur. De toepassing van de reserve ring voor de VM-infrastructuur kosten voor Windows-Vm's is hetzelfde als voor niet-Windows-Vm's. U betaalt afzonderlijk voor Windows-software op basis van een per vCPU. Zie [Windows-software kosten met reserve ringen](billing-reserved-Instance-windows-software-costs.md). U kunt uw Windows-licentie kosten afdekken met [Azure Hybrid Benefit voor Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

## <a name="discount-can-apply-to-different-sizes"></a>De korting kan worden toegepast op verschillende grootten

Wanneer u een gereserveerde VM-instantie koopt en u hebt **geoptimaliseerd voor**: de flexibiliteit van de **instantie grootte**, is de kortings dekking afhankelijk van de grootte van de virtuele machine die u selecteert. De reserve ring kan van toepassing zijn op de grootte van virtuele machines (Vm's) in dezelfde grootte reeks groep. Zie [flexibiliteit van virtuele machines met gereserveerde VM-instanties](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md)voor meer informatie.

## <a name="discount-applies-to-matching-servicetype-only"></a>Korting geldt alleen voor overeenkomende Service type

Een reserverings korting geldt alleen voor het VM- `ServiceType` gebruik waarbij `AdditionalInfo` de waarde in overeenkomt met de aangeschafte reserve ring. De toepassing reserverings korting negeert de meter die wordt gebruikt voor `ServiceType`vm's en evalueert deze alleen. Weet welk service type u de virtuele machine hebt aangeschaft voor. U kunt een reserve ring van een niet-Premium-opslag-VM uitwisselen voor een reserve ring van een Premium-opslag of op een andere manier.

## <a name="services-that-get-vm-reservation-discounts"></a>Services die VM-reserverings kortingen ophalen

Uw VM-reserve ringen kunnen van toepassing zijn op het VM-gebruik dat wordt verzonden vanuit meerdere services, niet alleen voor uw VM-implementaties. Resources die reserverings kortingen ophalen, zijn afhankelijk van de instelling voor de flexibiliteit van de instantie grootte.

### <a name="instance-size-flexibility-setting"></a>Instelling voor de flexibiliteit van instantie grootte

De instelling voor de flexibiliteit van de instantie grootte bepaalt welke services de gereserveerde exemplaar kortingen ophalen.

Of de instelling is ingeschakeld of uitgeschakeld, de reserverings kortingen zijn automatisch van toepassing op alle  overeenkomende VM `Microsoft.Compute`-gebruik wanneer de ConsumedService is. Controleer daarom de gebruiks gegevens voor de waarde *ConsumedService* . Voorbeelden zijn:

- Virtuele machines
- Virtuele-machineschaalsets
- Container service
- Azure Batch implementaties (in de modus gebruikers abonnementen)
- Azure Kubernetes Service (AKS)
- Service Fabric

Als de instelling is ingeschakeld, worden de reserverings kortingen automatisch toegepast op het overeenkomende VM-gebruik wanneer de *ConsumedService* een van de volgende items is:

- Microsoft.Compute
- Microsoft.ClassicCompute
- Microsoft.Batch
- Microsoft.MachineLearningServices
- Microsoft.Kusto

Controleer de waarde *ConsumedService* in uw gebruiks gegevens om te bepalen of het gebruik in aanmerking komt voor reserverings kortingen.

Zie flexibiliteit van de [grootte van virtuele machines met gereserveerde VM-instanties](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md)voor meer informatie over de flexibiliteit van de grootte van het exemplaar.


## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, kunt u [een ondersteunings aanvraag maken](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over Azure Reservations:

- [Wat zijn reserve ringen voor Azure?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Reserve ringen voor Azure beheren](billing-manage-reserved-vm-instance.md)
- [Het gebruik van de reserve ring begrijpen voor uw abonnement voor betalen naar gebruik](billing-understand-reserved-instance-usage.md)
- [Het gebruik van de reserve ring begrijpen voor uw Enter prise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Meer informatie over reserverings gebruik voor CSP-abonnementen](/partner-center/azure-reservations)
- [Windows-software kosten niet inbegrepen bij reserve ringen](billing-reserved-instance-windows-software-costs.md)
