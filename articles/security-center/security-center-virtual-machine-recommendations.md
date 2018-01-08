---
title: Beveiligen van uw virtuele machines in Azure Security Center | Microsoft Docs
description: Dit document gaat in Azure Security Center aanbevelingen die u helpen beveiligen van uw virtuele machines en blijven in overeenstemming met het beveiligingsbeleid.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: terrylan
ms.openlocfilehash: 54375f6f98b4989a7af8bcde649d967f77c6c862
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Beveiligen van uw virtuele machines in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer het Beveiligingscentrum identificeert mogelijke beveiligingsproblemen, maakt deze aanbevelingen die u bij het proces begeleiden van het configureren van benodigde besturingselementen.  Aanbevelingen hebben betrekking op Azure brontypen: virtuele machines (VM's), netwerken, SQL en toepassingen.

In dit artikel biedt de aanbevelingen die betrekking hebben op virtuele machines.  VM aanbevelingen center rond gegevensverzameling, systeemupdates, inrichting van antimalware, toepassen versleutelen van uw VM-schijven en meer.  Gebruik de onderstaande tabel als referentie om te begrijpen van de beschikbare VM-aanbevelingen en elk criterium doen als u deze toe te passen.

## <a name="available-vm-recommendations"></a>Beschikbare VM aanbevelingen
| Aanbeveling | Beschrijving |
| --- | --- |
| [Gegevensverzameling voor abonnementen inschakelen](security-center-enable-data-collection.md) |Hiermee wordt aanbevolen om gegevensverzameling in te schakelen in het beveiligingsbeleid voor elk van de abonnementen en voor alle VM's (virtuele machines) in uw abonnementen. |
| [Schakel versleuteling voor Azure Storage-Account](security-center-enable-encryption-for-storage-account.md) | Raadt aan dat u Azure Storage-Service: versleuteling voor gegevens in rust inschakelen. Versleuteling voor opslag-Service (SSE) werkt door de gegevens te versleutelen wanneer deze wordt geschreven naar Azure-opslag en voordat ophalen ontsleutelt. SSE is momenteel alleen beschikbaar voor de Azure Blob-service en kan worden gebruikt voor blok-blobs, pagina-blobs en toevoeg-blobs. Zie voor meer informatie, [Service versleuteling van opslag voor gegevens in rust](../storage/common/storage-service-encryption.md).</br>SSE wordt alleen ondersteund op Resource Manager storage-accounts. Klassieke opslagaccounts worden momenteel niet ondersteund. Zie inzicht in het klassieke en het Resource Manager-implementatiemodel [Azure-implementatiemodellen](../azure-classic-rm.md). |
| [Beveiligingsconfiguraties herstellen](security-center-remediate-os-vulnerabilities.md) |Lijn uw besturingssysteemconfiguraties met de aanbevolen configuratie beveiligingsregels, raadt bijvoorbeeld toestaan niet dat wachtwoorden worden opgeslagen. |
| [Systeemupdates toepassen](security-center-apply-system-updates.md) |Aanbeveling voor het implementeren van ontbrekende updates voor systeembeveiliging en essentiële updates op VM's. |
| [Toepassen van een Just-In-Time netwerk toegangsbeheer](security-center-just-in-time.md) | Adviseert toe te passen alleen bij het VM-time-toegang. De just-in tijd functie in preview is en beschikbaar zijn op de prijscategorie Standard van Security Center. Zie [prijzen](security-center-pricing.md) voor meer informatie over Security Center de prijscategorie. |
| [Opnieuw opstarten na systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Hiermee wordt aanbevolen om een VM opnieuw op te starten om het proces van het toepassen van systeemupdates te voltooien. |
| [Eindpuntbeveiliging installeren](security-center-install-endpoint-protection.md) |Hiermee wordt aanbevolen om antimalwareprogramma's op VM's te installeren. (Alleen voor Windows-VM's.) |
| [VM-agent inschakelen](security-center-enable-vm-agent.md) |Hiermee kunt u zien voor welke VM's de VM-agent is vereist. De VM-agent moet zijn geïnstalleerd op VM's om patches en basislijnen te scannen en antimalwareprogramma's uit te voeren. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Het artikel [VM Agent and Extensions – Part 2)](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (VM-agent en -extensies – deel 2) bevat informatie over het installeren van de VM-agent. |
| [Schijfversleuteling toepassen](security-center-apply-disk-encryption.md) |Hiermee wordt aanbevolen om de VM-schijven te versleutelen met behulp van Azure Disk Encryption. (Voor VM's van Windows en Linux.) Versleuteling wordt aanbevolen voor het besturingssysteem en voor de gegevensvolumes op de VM. |
| [Besturingssysteemversie bijwerken](security-center-update-os-version.md) |Raadt aan dat u de versie van het besturingssysteem (OS) voor uw Cloud-Service naar de meest recente versie beschikbaar voor uw OS-familie bijwerken.  Zie voor meer informatie over Cloudservices, de [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md). |
| [Evaluatie van beveiligingsproblemen is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| [Beveiligingsproblemen herstellen](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Hiermee ziet u beveiligingsproblemen in het systeem en in de toepassing die zijn gedetecteerd met de oplossing voor beveiligingslekken die op de VM is geïnstalleerd. |

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over de aanbevelingen die betrekking hebben op andere Azure-resource-typen:

* [Beveiligen van uw toepassingen in Azure Security Center](security-center-application-recommendations.md)
* [Beveiligen van uw netwerk in Azure Security Center](security-center-network-recommendations.md)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
