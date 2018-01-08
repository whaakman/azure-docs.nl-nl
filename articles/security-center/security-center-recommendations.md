---
title: Het beheren van aanbevelingen voor beveiliging in Azure Security Center | Microsoft Docs
description: Dit document begeleidt u bij hoe aanbevelingen in Azure Security Center helpen u bij het beveiligen van uw Azure-resources en blijven in overeenstemming met het beveiligingsbeleid.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2018
ms.author: terrylan
ms.openlocfilehash: 2cb4a1c944d6893ca7913eef4e93620059f2a839
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Aanbevelingen voor beveiliging in Azure Security Center beheren
Dit document begeleidt u bij het gebruik van de aanbevelingen in Azure Security Center voor hulp bij het beveiligen van uw Azure-resources.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is niet een stapsgewijze handleiding.
>
>

## <a name="what-are-security-recommendations"></a>Wat zijn aanbevelingen voor beveiliging?
De beveiligingsstatus van uw Azure-bronnen worden regelmatig door Security Center gecontroleerd. Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan. Deze aanbevelingen begeleiden u bij het configureren van de benodigde besturingselementen.

## <a name="implementing-security-recommendations"></a>Implementatie van aanbevelingen voor beveiliging
### <a name="set-recommendations"></a>Set-aanbevelingen
In [beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md), u informatie over het:

* Beveiligingsbeleid configureren.
* Schakel gegevensverzameling.
* Kies welke aanbevelingen om te zien als onderdeel van uw beveiligingsbeleid.

Huidige beleid aanbevelingen center rond systeemupdates, basislijnregels, antimalwareprogramma's [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) op subnetten en netwerkinterfaces, SQL database auditing transparent data encryption voor SQL-database en web application firewalls.  [Beveiligingsbeleid instellen](security-center-policies.md) bevat een beschrijving van elke optie aanbeveling.

### <a name="monitor-recommendations"></a>Monitor aanbevelingen
Nadat er een beveiligingsbeleid is ingesteld, wordt met Security Center de beveiligingsstatus van de Azure-resources geanalyseerd om potentiële beveiligingsproblemen op te sporen. De **aanbevelingen** tegel onder **overzicht** kunt u het totale aantal geïdentificeerd door Security Center aanbevelingen weten.

![Aanbevelingen tegel][1]

Overzicht van de details van elke aanbeveling, selecteer de **aanbevelingen tegel** onder **overzicht**. **Aanbevelingen** wordt geopend.

![Filter aanbevelingen][2]

De aanbevelingen worden weergegeven in tabelindeling, waarbij elke regel één bepaalde aanbeveling vertegenwoordigt. De kolommen van deze tabel zijn:

* **BESCHRIJVING**: verklaart de aanbeveling en wat er moet gebeuren om deze op te lossen.
* **RESOURCE**: geeft een lijst van de resources die deze aanbeveling is van toepassing.
* **STATUS**: Beschrijving van de huidige status van de aanbeveling:
  * **Open**: de aanbeveling nog niet is opgelost.
  * **Bezig**: de aanbeveling wordt momenteel toegepast op de bronnen en door u is geen actie vereist.
  * **Opgelost**: de aanbeveling is al voltooid (in dit geval wordt de regel is lichter gekleurd weergegeven).
* **ERNST**: hiermee wordt de ernst van deze bepaalde aanbeveling beschreven:
  * **Hoge**: een beveiligingsprobleem bestaat voor een belangrijke resource (zoals een toepassing, een virtuele machine of een netwerkbeveiligingsgroep) en aandacht vereist.
  * **Gemiddeld**: een beveiligingsprobleem bestaat en niet-kritieke of extra stappen zijn vereist om dit te dichten of om een proces te voltooien.
  * **Lage**: een beveiligingslek dat moet worden opgelost, maar geen onmiddellijke aandacht vereist. (Standaard lage aanbevelingen worden niet weergegeven, maar u kunt filteren op aanbevelingen als u wilt zien.)

Gebruik de onderstaande tabel als referentie om te begrijpen van de beschikbare aanbevelingen en elk biedt als u deze toe te passen.

> [!NOTE]
> Wilt u inzicht in de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md) voor Azure-resources.
>
>

| Aanbeveling | Beschrijving |
| --- | --- |
| [Gegevensverzameling voor abonnementen inschakelen](security-center-enable-data-collection.md) |Raadt aan dat u het verzamelen van gegevens in het beveiligingsbeleid voor elk van uw abonnementen en alle virtuele Azure-machines (VM's) en niet-Azure computers inschakelen. |
| [Beveiligingsconfiguraties herstellen](security-center-remediate-os-vulnerabilities.md) |Raadt aan dat u uw besturingssysteemconfiguraties met de aanbevolen configuratie beveiligingsregels, bijvoorbeeld uitlijnen, niet toestaan dat wachtwoorden worden opgeslagen. |
| [Systeemupdates toepassen](security-center-apply-system-updates.md) |Raadt aan dat u ontbrekende system beveiligingsupdates en essentiële updates op uw Windows- en Linux-machines en computers implementeren. |
| [Toepassen van een Just-In-Time netwerk toegangsbeheer](security-center-just-in-time.md) | Adviseert toe te passen alleen bij het VM-time-toegang. De just-in tijd functie in preview is en beschikbaar zijn op de prijscategorie Standard van Security Center. Zie [prijzen](security-center-pricing.md) voor meer informatie over Security Center de prijscategorie. |
| [Opnieuw opstarten na systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Hiermee wordt aanbevolen om een VM opnieuw op te starten om het proces van het toepassen van systeemupdates te voltooien. |
| [Een firewall voor webtoepassingen toevoegen](security-center-add-web-application-firewall.md) |Raadt aan dat u een web application firewall (WAF) voor web-eindpunten implementeert. Een WAF aanbeveling wordt voor een openbare internetgerichte IP (exemplaar niveau IP- of Load Balanced IP) met een gekoppelde netwerkbeveiligingsgroep met poorten voor inkomend web openen (80,443) weergegeven. </br>Security Center raadt aan dat u een WAF om te helpen beschermen tegen aanvallen die gericht is op uw webtoepassingen op virtuele machines en op App Service-omgeving inrichten. Een as-omgeving (App Service omgeving) is een [Premium](https://azure.microsoft.com/pricing/details/app-service/) service-plan optie van Azure App Service die een volledig geïsoleerde en toegewezen omgeving biedt voor het veilig uitvoeren van Azure App Service-apps. Zie voor meer informatie over het as-omgeving, de [documentatie van App Service-omgeving](../app-service/environment/intro.md).</br>U kunt meerdere webtoepassingen in Security Center beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties. |
| [Toepassingsbeveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection) |Voor het voltooien van de configuratie van een WAF moet verkeer worden gerouteerd naar het toestel WAF. Deze aanbeveling te volgen, voltooit de benodigde installatiebestanden wijzigingen. |
| [Een firewall van de volgende generatie toevoegen](security-center-add-next-generation-firewall.md) |Raadt aan dat u de volgende generatie Firewall (NGFW) uit een Microsoft-partner toevoegt te verhogen uw beveiligingsinstellingen. |
| [Verkeer alleen via NGFW sturen](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Raadt aan dat u configureert (NSG) netwerkbeveiligingsgroepen die binnenkomend verkeer naar uw virtuele machine via uw NGFW forceren. |
| [Eindpuntbeveiliging installeren](security-center-install-endpoint-protection.md) |Hiermee wordt aanbevolen om antimalwareprogramma's op VM's te installeren. (Alleen voor Windows-VM's.) |
| [Netwerkbeveiligingsgroepen op subnetten of virtuele machines inschakelen](security-center-enable-network-security-groups.md) |Raadt het nsg's op subnetten of VM's in te schakelen. |
| [Toegang tot en met een Internetgericht eindpunt beperken](security-center-restrict-access-through-internet-facing-endpoints.md) |Raadt aan dat u regels voor binnenkomend verkeer voor het nsg's configureren. |
| [Controle en detectie van bedreigingen op SQL-servers inschakelen](security-center-enable-auditing-on-sql-servers.md) |Raadt aan dat u de controle en detectie van bedreigingen voor Azure SQL-servers inschakelen. (Alleen voor azure SQL-service. Bevat geen SQL uitgevoerd op uw virtuele machines.) |
| [Controle en detectie van bedreigingen op SQL-databases inschakelen](security-center-enable-auditing-on-sql-databases.md) |Raadt aan dat u de controle en detectie van bedreigingen voor Azure SQL-databases inschakelen. (Alleen voor azure SQL-service. Bevat geen SQL uitgevoerd op uw virtuele machines.) |
| [Transparante gegevensversleuteling inschakelt op de SQL-databases](security-center-enable-transparent-data-encryption.md) |Raadt aan om versleuteling voor SQL-databases in te schakelen. (Alleen azure SQL-service). |
| [VM-agent inschakelen](security-center-enable-vm-agent.md) |Hiermee kunt u zien voor welke VM's de VM-agent is vereist. De VM-Agent moet worden geïnstalleerd op virtuele machines in inrichten patch scannen basislijn scannen en antimalwareprogramma's. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Het artikel [VM Agent and Extensions – Part 2)](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (VM-agent en -extensies – deel 2) bevat informatie over het installeren van de VM-agent. |
| [Schijfversleuteling toepassen](security-center-apply-disk-encryption.md) |Hiermee wordt aanbevolen om de VM-schijven te versleutelen met behulp van Azure Disk Encryption. (Voor VM's van Windows en Linux.) Versleuteling wordt aanbevolen voor het besturingssysteem en voor de gegevensvolumes op de VM. |
| [Contactgegevens voor beveiliging verstrekken](security-center-provide-security-contact-details.md) |Raadt u beveiliging bieden de contactgegevens voor elk van uw abonnementen. Contactgegevens is een e-mailadres en telefoonnummer getal. De informatie wordt gebruikt voor het contact met u opnemen als onze beveiligingsteam vindt dat uw resources worden getroffen. |
| [Besturingssysteemversie bijwerken](security-center-update-os-version.md) |Raadt aan dat u de versie van het besturingssysteem (OS) voor uw Cloud-Service naar de meest recente versie beschikbaar voor uw OS-familie bijwerken.  Zie voor meer informatie over Cloudservices, de [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md). |
| [Evaluatie van beveiligingsproblemen is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| [Beveiligingsproblemen herstellen](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Hiermee ziet u beveiligingsproblemen in het systeem en in de toepassing die zijn gedetecteerd met de oplossing voor beveiligingslekken die op de VM is geïnstalleerd. |
| [Schakel versleuteling voor Azure Storage-Account](security-center-enable-encryption-for-storage-account.md) | Raadt aan dat u Azure Storage-Service: versleuteling voor gegevens in rust inschakelen. Versleuteling voor opslag-Service (SSE) werkt door de gegevens te versleutelen wanneer deze wordt geschreven naar Azure-opslag en voordat ophalen ontsleutelt. SSE is momenteel alleen beschikbaar voor de Azure Blob-service en kan worden gebruikt voor blok-blobs, pagina-blobs en toevoeg-blobs. Zie voor meer informatie, [Service versleuteling van opslag voor gegevens in rust](../storage/common/storage-service-encryption.md).</br>SSE wordt alleen ondersteund op Resource Manager storage-accounts. |

U kunt filteren en aanbevelingen negeren.

1. Selecteer **Filter** op de **aanbevelingen** blade. De **Filter** blade wordt geopend en u selecteert de ernst en status waarden die u wilt zien.

2. Als u vaststelt dat een aanbeveling is niet van toepassing, kunt u de aanbeveling negeren en deze vervolgens uit uw weergave filteren. Er zijn twee manieren om te sluiten van een aanbeveling. Te klikken met de rechtermuisknop op een item en selecteer vervolgens **sluiten**. Beweeg de muisaanwijzer boven een item, klikt u op de drie punten die worden weergegeven aan de rechterkant en selecteer vervolgens het andere type is **sluiten**. U kunt ontslagen aanbevelingen weergeven door te klikken op **Filter**, en vervolgens te selecteren **genegeerd**.

    ![Aanbeveling negeren][3]

### <a name="apply-recommendations"></a>Toepassen van aanbevelingen
Bekijk bepalen alle aanbevelingen welke u moet eerst toepassen. Het is raadzaam dat u het prioriteitsniveau gebruiken, zoals de belangrijkste parameter om te beoordelen welke aanbevelingen eerst moet worden toegepast.

In de tabel van de bovenstaande aanbevelingen, selecteert u een aanbeveling en doorlopen als een voorbeeld van het toepassen van een aanbeveling.

## <a name="next-steps"></a>Volgende stappen
In dit document, kunt u kennisgemaakt met aanbevelingen voor beveiliging in Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) : informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Het beheren van en reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
