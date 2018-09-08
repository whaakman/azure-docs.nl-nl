---
title: Inzicht in het gebruik van de virtuele machine van Azure | Microsoft Docs
description: Informatie over het gebruik virtuele machines begrijpen
services: virtual-machines
documentationcenter: ''
author: mmccrory
manager: jeconnoc
editor: ''
tags: azure-virtual-machine
ms.assetid: ''
ms.service: ''
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 12/04/2017
ms.author: memccror
ms.openlocfilehash: d9df71f6fa2f64544cbefc7d9aca8c153c86ac98
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094252"
---
# <a name="understanding-azure-virtual-machine-usage"></a>Inzicht in het gebruik van de virtuele machine van Azure
Uw gebruik van Azure door gegevens te analyseren, krachtige verbruik inzichten kunnen worden verkregen – inzichten die mogelijk kosten beter beheer en de toewijzing binnen uw organisatie. Dit document bevat een dieper ingaan op de details van uw Azure Compute-gebruik. Voor meer informatie over algemene Azure-gebruik, gaat u naar [informatie over uw factuur](https://docs.microsoft.com/azure/billing/billing-understand-your-bill).

## <a name="download-your-usage-details"></a>Uw gebruiksgegevens downloaden
Om te beginnen, [downloaden van de informatie over het gebruik](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-from-the-account-center-csv). De onderstaande tabel bevat de definitie en voorbeeld van de waarden van het gebruik van voor virtuele Machines die zijn geïmplementeerd via de Azure Resource Manager. Dit document bevat geen gedetailleerde informatie over virtuele machines die via het klassieke model zijn geïmplementeerd.


| Velden             | Betekenis                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Voorbeelden van waarden                                                                                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Gebruiksdatum         | De datum waarop de resource is gebruikt.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |  “11/23/2017”                                                                                                                                                                                                                                                                                                                                                     |
| Id van de meter           | Identificeert de op het hoogste niveau service waarvoor dit gebruik behoort.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | 'Virtuele Machines'                                                                                                                                                                                                                                                                                                                                               |
| De subcategorie van de meter | De id van de gefactureerde meter. <ul><li>Voor Rekenuren verbruik, er is een meter voor elke VM-grootte en het besturingssysteem (Windows, niet-Windows) + regio.</li><li>Voor gebruik van Premium-software is er een meter voor elk softwaretype. De meeste software premium-installatiekopieën hebben andere manier en voor elke grootte core. Voor meer informatie gaat u naar de [Compute-pagina met prijzen.](https://azure.microsoft.com/pricing/details/virtual-machines/)</li></ul>                                                                                                                                                                                                                                                                                                                                         | "2005544f-659d-49c9-9094-8e0aea1be3a5"                                                                                                                                                                                                                                                                                                                           |
| De naam van de meter         | Dit is specifiek voor elke service in Azure. Voor de rekensnelheid is het altijd 'Rekenuren'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "Rekenuren"                                                                                                                                                                                                                                                                                                                                                  |
| De regio van de meter       | De datacenterlocatie voor bepaalde services waarbij de prijs is gebaseerd op de datacenterlocatie.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |  'Japan-Oost'                                                                                                                                                                                                                                                                                                                                                       |
| Eenheid               | De eenheid waarmee de service wordt in rekening gebracht in. COMPUTE-resources worden gefactureerd per uur.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 'Hours'                                                                                                                                                                                                                                                                                                                                                          |
| Verbruikt           | De hoeveelheid van de resource die is verbruikt voor die dag. Voor de rekensnelheid factureren we voor elke minuut dat de virtuele machine is uitgevoerd voor een bepaalde tijd (maximaal 6 decimalen van nauwkeurigheid).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |    “1”, “0.5”                                                                                                                                                                                                                                                                                                                                                    |
| Resourcelocatie  | Hiermee wordt het datacenter geïdentificeerd waarop de resource wordt uitgevoerd.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | 'Japan-Oost'                                                                                                                                                                                                                                                                                                                                                        |
| Verbruikte service   | De Azure-platform-service die u gebruikt.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | "Microsoft.Compute"                                                                                                                                                                                                                                                                                                                                              |
| Resourcegroep     | De resourcegroep waarin de geïmplementeerde resource wordt uitgevoerd. Zie voor meer informatie, [overzicht van Azure Resource Manager.](https://docs.microsoft.com/azure/virtual-machines/linux/vm-usage)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |    'MyRG'                                                                                                                                                                                                                                                                                                                                                        |
| Exemplaar-id        | De id voor de resource. De id bevat de naam die u voor de resource opgeeft wanneer deze wordt gemaakt. Voor virtuele machines, de exemplaar-ID bevat de SubscriptionId, ResourceGroupName en VMName (of de naam voor het gebruik van scale set-schaalset).                                                                                                                                                                                                                                                                                                                                                                                                                    | "/ abonnementen/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx / resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachines/MyVM1"<br><br>of<br><br>"/ abonnementen/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx / resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachineScaleSets/MyVMSS1"                                                                                           |
| Tags               | Code die u aan de resource toewijst. Gebruik tags facturering om records te groeperen. Meer informatie over het [taggen van uw virtuele Machines.](tag.md) Dit is alleen beschikbaar voor virtuele machines Resource Manager.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | ' {"myDepartment": "RD", 'myUser': 'MijnNaam'} '                                                                                                                                                                                                                                                                                                                        |
| Aanvullende informatie    | Servicespecifieke metagegevens. Voor virtuele machines vult u de volgende gegevens in het veld voor aanvullende informatie: <ul><li>Afbeelding typespecifieke afbeelding die u hebt uitgevoerd. Zoek de volledige lijst met ondersteunde tekenreeksen hieronder onder afbeeldingstypen.</li><li>Servicetype: de grootte die u hebt geïmplementeerd.</li><li>VMName: naam van uw virtuele machine. Dit veld wordt alleen ingevuld voor virtuele machines met schaalsets. Als u de naam van uw virtuele machine voor schaal ingesteld VM's, die kunt u vinden in de bovenstaande exemplaar-ID-tekenreeks.</li><li>UsageType: Dit geeft het type van dit verwijst naar gebruik.<ul><li>ComputeHR is het gebruik van de Rekenuren voor de onderliggende virtuele machine, zoals Standard_D1_v2.</li><li>ComputeHR_SW is de premium-software kosten in rekening gebracht als de virtuele machine met behulp van premium-software, zoals Microsoft R Server.</li></ul></li></ul>    | Virtuele Machines {"ImageType": "Canonieke", 'ServiceType': 'Standard_DS1_v2', 'VMName': ' ","UsageType":" ComputeHR"}<br><br>Virtual Machine Scale Sets {"ImageType": "Canonieke", 'ServiceType': 'Standard_DS1_v2', 'VMName': 'myVM1","UsageType":"ComputeHR"}<br><br>Premium-Software {"ImageType": "","ServiceType ': 'Standard_DS1_v2', 'VMName': '", "UsageType": "ComputeHR_SW"} |

## <a name="image-type"></a>Type installatiekopie
Voor bepaalde installatiekopieën in de Azure-galerie, wordt het afbeeldingstype ingevuld in het veld voor aanvullende informatie. Hierdoor kunnen gebruikers te begrijpen en te volgen wat ze op hun virtuele Machine hebt geïmplementeerd. De volgende waarden worden ingevuld. in dit veld op basis van de installatiekopie die u hebt geïmplementeerd:
  - BitRock 
  - Canonical 
  - FreeBSD 
  - Open logica 
  - Oracle 
  - SLES voor SAP 
  - SQL Server 14 Preview-versie van Windows Server 2012 R2 Preview 
  - SUSE
  - SUSE Premium
  - StorSimple Cloud Appliance 
  - Red Hat
  - Red Hat voor SAP Business Applications     
  - Red Hat voor SAP HANA 
  - Windows-Client BYOL 
  - WindowsServer BYOL 
  - Windows Server-Preview 

## <a name="service-type"></a>Servicetype
Het veld van het type service in het veld voor aanvullende informatie komt overeen met de exacte VM-grootte die u hebt geïmplementeerd. Premium storage-VM's (SSD-gebaseerde) en niet-premium-opslag (HDD-indeling) virtuele machines gelden dezelfde zijn. Als u een grootte op basis van SSD, zoals standaard implementeert\_DS2\_v2, ziet u de grootte van de niet-SSD ('Standard\_D2\_v2 VM') in de kolom Metersubcategorie en de SSD-grootte ('Standard\_DS2\_ v2') in het veld voor aanvullende informatie.

## <a name="region-names"></a>Regionamen
De naam van de regio die is ingevuld in het veld locatie van de Resource van de informatie over het gebruik verschilt van de naam van de regio die wordt gebruikt in de Azure Resource Manager. Hier volgt een toewijzing tussen de waarden van de regio:

|    **Naam van de Resource Manager-gebied**       |    **Resourcelocatie in informatie over het gebruik**    |
|--------------------------|------------------------------------------|
|    australiaeast         |    AU - oost                               |
|    australiasoutheast    |    AU - zuidoost                          |
|    brazilsouth           |    BR - zuid                              |
|    CanadaCentral         |    CA - centraal                            |
|    CanadaEast            |    CA - oost                               |
|    CentralIndia          |    IN - centraal                            |
|    centralus             |    US - centraal                            |
|    chinaeast             |    China East                            |
|    chinanorth            |    China - noord                           |
|    eastasia              |    Azië - oost                             |
|    eastus                |    US - oost                               |
|    eastus2               |    US - oost 2                             |
|    GermanyCentral        |    DE, midden                            |
|    GermanyNortheast      |    DE, noordoosten                          |
|    japaneast             |    JA - oost                               |
|    japanwest             |    JA - west                               |
|    KoreaCentral          |    KR - centraal                            |
|    KoreaSouth            |    KR - zuid                              |
|    northcentralus        |    US - noord-centraal                      |
|    northeurope           |    Europa - noord                          |
|    southcentralus        |    US - zuid-centraal                      |
|    southeastasia         |    Azië - zuidoost                        |
|    SouthIndia            |    IN - zuid                              |
|    UKNorth               |    Verenigde Staten-Noord                              |
|    uksouth               |    Verenigd Koninkrijk Zuid                              |
|    UKSouth2              |    VK, zuid 2                            |
|    ukwest                |    Verenigd Koninkrijk West                               |
|    USDoDCentral          |    US DoD - centraal                        |
|    USDoDEast             |    US DoD - oost                           |
|    USGovArizona          |    USGov Arizona                         |
|    usgoviowa             |    USGov Iowa                            |
|    USGovTexas            |    USGov Texas                           |
|    usgovvirginia         |    USGov Virginia                        |
|    westcentralus         |    US - west-centraal                       |
|    westeurope            |    Europa -west                           |
|    WestIndia             |    West-India                               |
|    westus                |    US - west                               |
|    westus2               |    US - west 2                             |


## <a name="virtual-machine-usage-faq"></a>Veelgestelde vragen over virtuele machine gebruik
### <a name="what-resources-are-charged-when-deploying-a-vm"></a>Welke bronnen worden in rekening gebracht bij het implementeren van een virtuele machine?    
VM's verkrijgen kosten voor de virtuele machine zelf, een premium-software die wordt uitgevoerd op de virtuele machine, de opslagschijf account\managed die zijn gekoppeld aan de virtuele machine, en de bandbreedte van het netwerk worden overgedragen van de virtuele machine.
### <a name="how-can-i-tell-if-a-vm-is-using-azure-hybrid-benefit-in-the-usage-csv"></a>Hoe kan ik zien als een virtuele machine Azure Hybrid Benefit wordt gebruikt in de CSV gebruik?
Als u met behulp van implementeert de [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/), u het niet-Windows-VM-tarief wordt gefactureerd omdat u uw eigen licentie naar de cloud brengen. Op uw factuur, kunt u onderscheiden welke virtuele machines Resource Manager Azure Hybrid Benefit worden uitgevoerd, omdat ze een ' Windows\_Server BYOL ' of ' Windows\_Client BYOL ' in de kolom ImageType.
### <a name="how-are-basic-vs-standard-vm-types-differentiated-in-the-usage-csv"></a>Hoe worden Basic vs. Standaard VM-typen onderscheiden in het CSV-gebruik?
Zowel Basic en Standard A-serie VM's worden aangeboden. Als u een eenvoudige virtuele machine, in de subcategorie Meter implementeren is de tekenreeks 'Basic'. Als u een Standard A-serie VM implementeert, klikt u vervolgens de VM-grootte wordt weergegeven als 'A1-VM' omdat Standard de standaardinstelling is. Zie voor meer informatie over de verschillen tussen Basic en Standard, de [pagina met prijzen van](https://azure.microsoft.com/pricing/details/virtual-machines/).
### <a name="what-are-extrasmall-small-medium-large-and-extralarge-sizes"></a>Wat zijn ExtraSmall, klein, middelgroot, groot en ExtraLarge grootten?
ExtraSmall - zijn, worden de oude namen voor Standard\_A0 – Standard\_A4. In de klassieke VM gebruiksrecords ziet u mogelijk deze afspraak gebruikt als u deze grootten hebt geïmplementeerd.
### <a name="what-is-the-difference-between-meter-region-and-resource-location"></a>Wat is het verschil tussen Meterregio en Resourcelocatie?
De Meter-regio is gekoppeld aan de meter. Voor sommige Azure-services die gebruikmaken van één prijs voor alle regio's, kan het veld Meterregio niet leeg zijn. Echter, omdat virtuele machines zijn speciaal bestemd voor prijzen per regio voor virtuele Machines, is dit veld ingevuld. Op deze manier is de locatie van de Resource voor virtuele Machines de locatie waar de virtuele machine is geïmplementeerd. De Azure-regio's in beide velden zijn hetzelfde, hoewel ze mogelijk een andere tekenreeks-overeenkomst voor de regionaam van de hebben.
### <a name="why-is-the-imagetype-value-blank-in-the-additional-info-field"></a>Waarom is de waarde ImageType leeg in het veld voor aanvullende informatie?
Het veld ImageType wordt alleen ingevuld voor een subset van afbeeldingen. Als u een van de bovenstaande installatiekopieën niet hebt geïmplementeerd, is de ImageType is leeg.
### <a name="why-is-the-vmname-blank-in-the-additional-info"></a>Waarom is de VMName leeg in de aanvullende informatie?
Het VMName wordt alleen ingevuld in het veld voor aanvullende informatie voor virtuele machines in een schaalset. Het veld InstanceID bevat de naam van de virtuele machine voor niet--VM's schaalset.
### <a name="what-does-computehr-mean-in-the-usagetype-field-in-the-additional-info"></a>Wat betekent ComputeHR in het veld UsageType in de aanvullende informatie?
ComputeHR staat voor Rekenuur waarmee de gebruiksgebeurtenis worden gerapporteerd voor de onderliggende infrastructuur hoeft te investeren. Als de UsageType ComputeHR\_SW, de gebeurtenis gebruik vertegenwoordigt de premium-software kosten in rekening gebracht voor de virtuele machine.
### <a name="how-do-i-know-if-i-am-charged-for-premium-software"></a>Hoe weet ik als ik kosten in rekening voor premium-software gebracht ben?
Bij het verkennen van welke VM-installatiekopie best aansluit bij uw behoeften, lees dan zeker worden de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute). De afbeelding is de snelheid van software-abonnement. Als u 'Gratis' voor de snelheid, is er geen extra kosten voor de software. 
### <a name="what-is-the-difference-between-microsoftclassiccompute-and-microsoftcompute-in-the-consumed-service"></a>Wat is het verschil tussen Microsoft.ClassicCompute en Microsoft.Compute in de service verbruikt?
Microsoft.ClassicCompute vertegenwoordigt klassieke resources die zijn geïmplementeerd via de Azure Service Manager. Als u via het Resource Manager implementeert, wordt in de verbruikte service Microsoft.Compute gevuld. Meer informatie over de [Azure-implementatiemodellen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).
### <a name="why-is-the-instanceid-field-blank-for-my-virtual-machine-usage"></a>Waarom is het InstanceID veld leeg voor mijn virtuele Machine gebruik?
Als u via het klassieke implementatiemodel implementeert, is de InstanceID-tekenreeks niet beschikbaar.
### <a name="why-are-the-tags-for-my-vms-not-flowing-to-the-usage-details"></a>Waarom zijn de labels voor mijn virtuele machines niet doorgestuurd naar de informatie over het gebruik?
Labels alleen overgebracht naar u het gebruik van CSV voor Resource Manager-virtuele machines alleen. Klassieke resource-tags zijn niet beschikbaar in de informatie over het gebruik.
### <a name="how-can-the-consumed-quantity-be-more-than-24-hours-one-day"></a>Hoe kan de verbruikte hoeveelheid meer dan 24 uur één dag zijn?
Facturering voor de resources worden in het klassieke model samengevoegd op het niveau van de Service in de Cloud. Als u meer dan één virtuele machine in een Cloudservice die gebruikmaakt van de dezelfde factureringsmeter hebt, wordt uw gebruik samen samengevoegd. Via Resource Manager geïmplementeerde virtuele machines worden gefactureerd op het niveau van de virtuele machine, zodat deze aggregatie is niet van toepassing.
### <a name="why-is-pricing-not-available-for-dsfsgsls-sizes-on-the-pricing-page"></a>Waarom is prijzen niet beschikbaar voor de DS/FS/GS/LS-grootten op de pagina met prijzen?
Premium-opslag kunnen virtuele machines worden gefactureerd tegen hetzelfde tarief als niet-premium-opslag kunnen virtuele machines. Alleen de opslagkosten van uw afwijken. Ga naar de [pagina met prijzen voor storage](https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de informatie over het gebruik, [meer informatie over uw factuur voor Microsoft Azure.](https://docs.microsoft.com/azure/billing/billing-understand-your-bill
)

