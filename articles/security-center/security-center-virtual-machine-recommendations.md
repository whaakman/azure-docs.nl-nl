---
title: Aanbevelingen voor virtuele machines in Azure Security Center | Microsoft Docs
description: Dit document wordt uitgelegd dat de Azure Security Center aanbevelingen voor het beveiligen van uw virtuele machines en computers en uw web-apps en App Service-omgevingen.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: f5ce7f62-7b12-4bc0-b418-4a2f9ec49ca1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2018
ms.author: rkarlin
ms.openlocfilehash: 7e73d6236f76c58307bb552aeee03bafe66addcc
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43132938"
---
# <a name="understand-azure-security-center-resource-recommendations"></a>Aanbevelingen voor Azure Security Center resource begrijpen


## <a name="recommendations"></a>Aanbevelingen
Gebruik de onderstaande tabellen als referentie om te begrijpen van de beschikbare Compute en App services aanbevelingen en wat elke doet als u deze toepast.

### <a name="computers"></a>Computers
| Aanbeveling | Beschrijving |
| --- | --- |
| [Gegevensverzameling voor abonnementen inschakelen](security-center-enable-data-collection.md) |Hiermee wordt aanbevolen om gegevensverzameling in te schakelen in het beveiligingsbeleid voor elk van de abonnementen en voor alle VM's (virtuele machines) in uw abonnementen. |
| [Versleuteling inschakelen voor Azure Storage-Account](security-center-enable-encryption-for-storage-account.md) | Hiermee wordt aanbevolen dat u Azure Storage Service Encryption voor data-at-rest inschakelen. Storage Service Encryption (SSE) werkt door het versleutelen van de gegevens wanneer deze worden geschreven naar Azure storage en voordat ophalen ontsleutelt. SSE is momenteel alleen beschikbaar voor de Azure Blob-service en kan worden gebruikt voor blok-blobs, pagina-blobs en toevoeg-blobs. Zie voor meer informatie, [Storage Service Encryption voor data-at-rest](../storage/common/storage-service-encryption.md).</br>SSE wordt alleen ondersteund op Resource Manager-opslagaccounts. Klassieke opslagaccounts worden momenteel niet ondersteund. Zie voor meer informatie over de klassieke en Resource Manager-implementatiemodel, [Azure-implementatiemodellen](../azure-classic-rm.md). |
| [Beveiligingsconfiguraties herstellen](security-center-remediate-os-vulnerabilities.md) |Aanbevolen configuraties voor het besturingssysteem met de aanbevolen configuratie van beveiligingsregels, uit te lijnen bijvoorbeeld toestaan niet dat wachtwoorden worden opgeslagen. |
| [Systeemupdates toepassen](security-center-apply-system-updates.md) |Aanbeveling voor het implementeren van ontbrekende updates voor systeembeveiliging en essentiële updates op VM's. |
| [Toepassen van een Just-In-Time netwerktoegangsbeheer](security-center-just-in-time.md) | Hiermee wordt aanbevolen dat u van toepassing just-in-time-VM-toegang zijn. Het gewoon in-time-functie beschikbaar als preview is en beschikbaar zijn op de prijscategorie Standard van Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center. |
| [Opnieuw opstarten na systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Hiermee wordt aanbevolen om een VM opnieuw op te starten om het proces van het toepassen van systeemupdates te voltooien. |
| [Eindpuntbeveiliging installeren](security-center-install-endpoint-protection.md) |Hiermee wordt aanbevolen om antimalwareprogramma's op VM's te installeren. (Alleen voor Windows-VM's.) |
| [VM-agent inschakelen](security-center-enable-vm-agent.md) |Hiermee kunt u zien voor welke VM's de VM-agent is vereist. De VM-agent moet zijn geïnstalleerd op VM's om patches en basislijnen te scannen en antimalwareprogramma's uit te voeren. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Het artikel [VM Agent and Extensions – Part 2)](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (VM-agent en -extensies – deel 2) bevat informatie over het installeren van de VM-agent. |
| [Schijfversleuteling toepassen](security-center-apply-disk-encryption.md) |Hiermee wordt aanbevolen om de VM-schijven te versleutelen met behulp van Azure Disk Encryption. (Voor VM's van Windows en Linux.) Versleuteling wordt aanbevolen voor het besturingssysteem en voor de gegevensvolumes op de VM. |
| [Besturingssysteemversie bijwerken](security-center-update-os-version.md) |Hiermee wordt aanbevolen dat u de versie van het besturingssysteem (OS) voor je Cloud Service naar de meest recente versie beschikbaar voor uw type besturingssysteem bijwerken.  Zie voor meer informatie over Cloud Services, de [overzicht van Cloud Services](../cloud-services/cloud-services-choose-me.md). |
| [Evaluatie van beveiligingsproblemen is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| [Beveiligingsproblemen herstellen](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Hiermee ziet u beveiligingsproblemen in het systeem en in de toepassing die zijn gedetecteerd met de oplossing voor beveiligingslekken die op de VM is geïnstalleerd. |

### <a name="app-services"></a>App-services
| Aanbeveling | Beschrijving |
| --- | --- |
| App Service mag alleen toegankelijk zijn via HTTPS | Hiermee wordt aanbevolen dat u de toegang van App Service via HTTPS alleen beperken. |
| Websockets moeten worden uitgeschakeld voor Web-App| Hiermee wordt aanbevolen dat u zorgvuldig door het gebruik van Websockets in webtoepassingen.  Het websocketsprotocol is kwetsbaar voor verschillende soorten beveiligingsrisico's. |
| Aangepaste domeinen gebruiken voor uw Web-App | Hiermee wordt aanbevolen dat u aangepaste domeinen gebruiken voor een webtoepassing beveiligen tegen algemene aanvallen zoals phishing en andere aanvallen met betrekking tot DNS. |
| IP-beperkingen voor Web-App configureren | Hiermee wordt aanbevolen dat u een lijst met IP-adressen die zijn toegestaan voor toegang tot uw toepassing definiëren.  Gebruik van IP-beperkingen beschermt u webtoepassingen tegen algemene aanvallen. |
| Niet alle toestaan ('* ') resources toegang tot uw toepassing | Hiermee wordt aanbevolen dat u de parameter WEBSITE_LOAD_CERTIFICATES niet ingesteld op '*'. Als u de parameter naar '*' betekent dat alle certificaten in de persoonlijke certificaatarchief van uw web-toepassingen worden geladen.  Dit kan leiden tot misbruik van het principe van minimale bevoegdheden als is het onwaarschijnlijk dat de site toegang hebben tot alle certificaten die tijdens runtime moet. |
| CORS kunt u voorkomen dat elke resource toegang tot uw toepassing | Hiermee wordt aanbevolen dat u alleen vereiste domeinen om te communiceren met uw webtoepassing toestaan. Cross-origin resource sharing (CORS) kunt u voorkomen dat alle domeinen toegang tot uw webtoepassing. |
| Gebruik de nieuwste ondersteunde .NET Framework voor webtoepassingen | Hiermee wordt aanbevolen dat u de meest recente versie van .NET Framework voor de nieuwste beveiligingsklassen gebruiken. Met behulp van oudere klassen en typen kan uw toepassing kwetsbaar maken. |
| Gebruik de nieuwste ondersteunde Java-versie voor Web-App | Hiermee wordt aanbevolen dat u de meest recente versie van Java voor de nieuwste beveiligingsklassen gebruiken. Met behulp van oudere klassen en typen kan uw toepassing kwetsbaar maken. |
| Gebruik de nieuwste ondersteunde PHP-versie voor Web-App | Hiermee wordt aanbevolen dat u de nieuwste PHP-versie voor de nieuwste beveiligingsklassen gebruiken. Met behulp van oudere klassen en typen kan uw toepassing kwetsbaar maken. |
| [Een firewall voor webtoepassingen toevoegen](security-center-add-web-application-firewall.md) |Hiermee wordt aanbevolen dat u een web application firewall (WAF) voor web-eindpunten te implementeren. Een WAF-aanbeveling wordt voor een openbaar IP-adres (Instance Level IP of Load Balanced IP) met een gekoppelde netwerkbeveiligingsgroep met open binnenkomende webpoorten (80,443) weergegeven.</br></br>Security Center raadt aan dat u een WAF om te helpen beschermen tegen aanvallen die gericht is op uw webtoepassingen op virtuele machines en op App Service-omgeving inrichten. Een App Service Environment (ASE) is een [Premium](https://azure.microsoft.com/pricing/details/app-service/) service-plan-optie van Azure App Service waarmee u een volledig geïsoleerde en toegewezen omgeving voor Azure App Service-apps veilig kunnen worden uitgevoerd. Zie voor meer informatie over het as-omgeving, de [documentatie over App Service-omgeving](../app-service/environment/intro.md).</br></br>U kunt meerdere webtoepassingen in Security Center beveiligen door toe te voegen deze toepassingen aan uw bestaande WAF-implementaties. |
| [Toepassingsbeveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection) |Voor het voltooien van de configuratie van een WAF, moet verkeer worden omgeleid naar de WAF-apparaat. Deze aanbeveling te volgen, wordt de wijzigingen in de vereiste installatie voltooid. |
| Gebruik de nieuwste ondersteunde Node.js versie voor Web-App | Hiermee wordt aanbevolen dat u de meest recente versie van Node.js voor de nieuwste beveiligingsklassen gebruiken. Met behulp van oudere klassen en typen kan uw toepassing kwetsbaar maken. |
| CORS kunt u voorkomen dat elke resource toegang tot uw functie-App | Hiermee wordt aanbevolen dat u alleen vereiste domeinen om te communiceren met uw webtoepassing toestaan. Cross-origin resource sharing (CORS) kunt u voorkomen dat alle domeinen toegang tot uw functie-toepassing. |
| Aangepaste domeinen voor functie-App gebruiken | Hiermee wordt aanbevolen dat u aangepaste domeinen gebruiken voor een functie-app beveiligen tegen algemene aanvallen zoals phishing en andere aanvallen met betrekking tot DNS. |
| IP-beperkingen voor functie-App configureren | Hiermee wordt aanbevolen dat u een lijst met IP-adressen die zijn toegestaan voor toegang tot uw toepassing definiëren. Een functie-app beschermt gebruik van IP-beperkingen tegen algemene aanvallen. |
| Functie-App moet alleen toegankelijk zijn via HTTPS | Hiermee wordt aanbevolen dat u de toegang van de functie-apps via HTTPS alleen beperken. |
| Foutopsporing op afstand moet worden uitgeschakeld voor functie-App | Hiermee wordt aanbevolen dat u als u niet meer nodig hebt om deze te gebruiken voor functie-App-foutopsporing uitschakelen. Externe foutopsporing moeten inkomende poorten op een functie-App moeten worden geopend. |
| Websockets moeten worden uitgeschakeld voor functie-App | Hiermee wordt aanbevolen dat u zorgvuldig door het gebruik van Websockets in functie-Apps. Het websocketsprotocol is kwetsbaar voor verschillende soorten beveiligingsrisico's. |


## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:

* [Identiteit en toegang bewaken in Azure Security Center](security-center-identity-access.md)
* [Protecting your network in Azure Security Center](security-center-network-recommendations.md) (Uw netwerk beveiligen in Azure Security Center)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligen van uw computers en toepassingen in Azure Security Center](security-center-virtual-machine-protection.md)
* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
