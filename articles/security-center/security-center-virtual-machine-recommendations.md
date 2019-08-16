---
title: Aanbevelingen voor virtuele machines in Azure Security Center | Microsoft Docs
description: In dit document worden de Azure Security Center aanbevelingen beschreven voor het beveiligen van uw virtuele machines en computers en uw web-apps en App Service omgevingen.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f5ce7f62-7b12-4bc0-b418-4a2f9ec49ca1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2018
ms.author: rkarlin
ms.openlocfilehash: 798e027ca611905766b1fb8bcdb89cba4aeaf9b2
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69531336"
---
# <a name="understand-azure-security-center-resource-recommendations"></a>Aanbevelingen voor Azure Security Center resource begrijpen


## <a name="recommendations"></a>Aanbevelingen
Gebruik de onderstaande tabellen als referentie om inzicht te krijgen in de beschik bare aanbevelingen voor berekenings-en app-Services en wat er gebeurt als u deze toepast.

### <a name="computers"></a>Computers
| Aanbeveling | Description |
| --- | --- |
| Gegevensverzameling voor abonnementen inschakelen|Hiermee wordt aanbevolen om gegevensverzameling in te schakelen in het beveiligingsbeleid voor elk van de abonnementen en voor alle VM's (virtuele machines) in uw abonnementen. |
| Versleuteling inschakelen voor het Azure Storage-account| Hiermee wordt aanbevolen om Azure Storage service versleuteling in te scha kelen voor gegevens in rust. Storage Service Encryption (SSE) werkt door de gegevens te versleutelen wanneer deze naar Azure Storage worden geschreven en ontsleuteld voordat ze worden opgehaald. SSE is momenteel alleen beschikbaar voor Azure Blob service en kan worden gebruikt voor blok-blobs, pagina-blobs en toevoeg-blobs. Zie [Storage service Encryption voor Data-at-rest voor](../storage/common/storage-service-encryption.md)meer informatie.</br>SSE wordt alleen ondersteund voor Resource Manager-opslag accounts. Klassieke opslag accounts worden momenteel niet ondersteund. Zie Azure-implementatie modellen voor meer informatie over het klassieke en Resource Manager-implementatie model. |
| Beveiligingsconfiguraties herstellen|Hiermee wordt aanbevolen dat u de configuraties van het besturings systeem uitlijnt met de aanbevolen beveiligings configuratie regels, bijvoorbeeld niet toestaan dat wacht woorden worden opgeslagen. |
| Systeemupdates toepassen |Aanbeveling voor het implementeren van ontbrekende updates voor systeembeveiliging en essentiële updates op VM's. |
| Just-In-Time-netwerktoegangsbeheer toepassen| U wordt aangeraden just-in-time-VM-toegang uit te voeren. De just-in-time-functie is beschikbaar in de preview-versie van Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center. |
| Opnieuw opstarten na systeemupdates|Hiermee wordt aanbevolen om een VM opnieuw op te starten om het proces van het toepassen van systeemupdates te voltooien. |
| Eindpuntbeveiliging installeren|Hiermee wordt aanbevolen om antimalwareprogramma's op VM's te installeren. (Alleen voor Windows-VM's.) |
| VM-agent inschakelen |Hiermee kunt u zien voor welke VM's de VM-agent is vereist. De VM-agent moet zijn geïnstalleerd op VM's om patches en basislijnen te scannen en antimalwareprogramma's uit te voeren. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Het artikel [VM Agent and Extensions – Part 2)](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (VM-agent en -extensies – deel 2) bevat informatie over het installeren van de VM-agent. |
| Schijfversleuteling toepassen|Hiermee wordt aanbevolen om de VM-schijven te versleutelen met behulp van Azure Disk Encryption. (Voor VM's van Windows en Linux.) Versleuteling wordt aanbevolen voor het besturingssysteem en voor de gegevensvolumes op de VM. |
| Besturingssysteemversie bijwerken|Hiermee wordt aanbevolen dat u de versie van het besturings systeem (OS) voor uw Cloud service bijwerkt naar de meest recente versie die beschikbaar is voor uw besturingssysteem familie.  Zie Cloud Services Overview voor meer informatie over Cloud Services. |
| Beoordeling van beveiligingslekken is niet geïnstalleerd|Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| Beveiligings problemen oplossen) |Hiermee ziet u beveiligingsproblemen in het systeem en in de toepassing die zijn gedetecteerd met de oplossing voor beveiligingslekken die op de VM is geïnstalleerd. |

### App-Services<a name="app-services"></a>
| Aanbeveling | Description |
| --- | --- |
| App Service mag alleen toegankelijk zijn via HTTPS | Hiermee wordt aanbevolen dat u de toegang van App Service alleen via HTTPS beperkt. |
| Websockets moeten worden uitgeschakeld voor Web-App| U wordt aangeraden om het gebruik van websockets in webtoepassingen zorgvuldig te controleren.  Het websocketsprotocol is kwetsbaar voor verschillende soorten beveiligingsrisico's. |
| Aangepaste domeinen gebruiken voor uw Web-App | U wordt aangeraden aangepaste domeinen te gebruiken om een webtoepassing te beveiligen tegen algemene aanvallen zoals phishing en andere DNS-gerelateerde aanvallen. |
| IP-beperkingen voor Web-App configureren | Hiermee wordt aanbevolen een lijst te definiëren met IP-adressen die toegang hebben tot uw toepassing.  Gebruik van IP-beperkingen beschermt u webtoepassingen tegen algemene aanvallen. |
| Niet alle toestaan ('* ') resources toegang tot uw toepassing | Hiermee wordt aanbevolen dat u de para meter WEBSITE_LOAD_CERTIFICATES niet*instelt op. Als u de para meter*instelt op ' ', worden alle certificaten geladen in het persoonlijke certificaat archief van uw webtoepassingen.  Dit kan leiden tot misbruik van het principe van minimale bevoegdheden als is het onwaarschijnlijk dat de site toegang hebben tot alle certificaten die tijdens runtime moet. |
| CORS mag niet alle bronnen toestaan om toegang te krijgen tot uw toepassing | Hiermee wordt u aangeraden alleen vereiste domeinen te laten communiceren met uw webtoepassing. Cross-origin resource sharing (CORS) kunt u voorkomen dat alle domeinen toegang tot uw webtoepassing. |
| Gebruik de nieuwste ondersteunde .NET Framework voor webtoepassingen | Hiermee wordt aanbevolen dat u de nieuwste versie van .NET Framework gebruikt voor de nieuwste beveiligings klassen. Met behulp van oudere klassen en typen kan uw toepassing kwetsbaar maken. |
| Gebruik de nieuwste ondersteunde Java-versie voor Web-App | Hiermee wordt aanbevolen dat u de meest recente Java-versie gebruikt voor de nieuwste beveiligings klassen. Met behulp van oudere klassen en typen kan uw toepassing kwetsbaar maken. |
| Gebruik de nieuwste ondersteunde PHP-versie voor Web-App | Hiermee wordt aanbevolen dat u de meest recente PHP-versie gebruikt voor de nieuwste beveiligings klassen. Met behulp van oudere klassen en typen kan uw toepassing kwetsbaar maken. |
| [Een firewall voor webtoepassingen toevoegen](security-center-add-web-application-firewall.md) |Hiermee wordt aanbevolen dat u een Web Application Firewall (WAF) implementeert voor web-eind punten. Er wordt een WAF-aanbeveling weer gegeven voor een openbaar gericht IP-adres (IP op exemplaar niveau of IP-verkeer met gelijke taak verdeling) met een netwerk beveiligings groep die is gekoppeld aan open inkomende webpoorten (80.443).</br></br>Security Center raadt u aan om een WAF in te richten, zodat u zich kunt beschermen tegen aanvallen die gericht zijn op uw webtoepassingen op virtuele machines en op App Service Environment. Een App Service Environment (ASE) is een [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan-optie van Azure app service die een volledig geïsoleerde en toegewezen omgeving biedt voor het veilig uitvoeren van Azure app service-apps. Zie de [app service Environment-documentatie](../app-service/environment/intro.md)voor meer informatie over ASE.</br></br>U kunt meerdere webtoepassingen in Security Center beveiligen door deze toepassingen toe te voegen aan uw bestaande WAF-implementaties. |
| [Toepassingsbeveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection) |Voor het volt ooien van de configuratie van een WAF moet het verkeer worden omgeleid naar het WAF-apparaat. Als u deze aanbeveling volgt, worden de benodigde configuratie wijzigingen voltooid. |
| Gebruik de nieuwste ondersteunde Node.js versie voor Web-App | Hiermee wordt aanbevolen dat u de meest recente versie van node. js gebruikt voor de nieuwste beveiligings klassen. Met behulp van oudere klassen en typen kan uw toepassing kwetsbaar maken. |
| CORS kunt u voorkomen dat elke resource toegang tot uw functie-App | Hiermee wordt u aangeraden alleen vereiste domeinen te laten communiceren met uw webtoepassing. Cross-origin resource sharing (CORS) kunt u voorkomen dat alle domeinen toegang tot uw functie-toepassing. |
| Aangepaste domeinen voor functie-App gebruiken | U wordt aangeraden aangepaste domeinen te gebruiken om een functie-app te beschermen tegen algemene aanvallen zoals phishing en andere DNS-gerelateerde aanvallen. |
| IP-beperkingen voor functie-App configureren | Hiermee wordt aanbevolen een lijst te definiëren met IP-adressen die toegang hebben tot uw toepassing. Een functie-app beschermt gebruik van IP-beperkingen tegen algemene aanvallen. |
| Functie-App moet alleen toegankelijk zijn via HTTPS | Hiermee wordt aanbevolen dat u de toegang tot functie-apps alleen via HTTPS beperkt. |
| Externe foutopsporing moet worden uitgeschakeld voor functie-app | U wordt aangeraden de fout opsporing voor functie-app uit te scha kelen als u deze niet meer nodig hebt. Externe foutopsporing moeten inkomende poorten op een functie-App moeten worden geopend. |
| Websockets moeten worden uitgeschakeld voor functie-App | U wordt aangeraden om het gebruik van websockets in functie-apps zorgvuldig te bekijken. Het websocketsprotocol is kwetsbaar voor verschillende soorten beveiligingsrisico's. |


## <a name="next-steps"></a>Volgende stappen
Zie de volgende onderwerpen voor meer informatie over aanbevelingen die betrekking hebben op andere typen Azure-resources:

* [Identiteit en toegang bewaken in Azure Security Center](security-center-identity-access.md)
* [Protecting your network in Azure Security Center](security-center-network-recommendations.md) (Uw netwerk beveiligen in Azure Security Center)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Uw machines en toepassingen beveiligen in Azure Security Center](security-center-virtual-machine-protection.md)
* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
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
