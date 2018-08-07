---
title: Aanbevelingen voor beveiliging in Azure Security Center beheren | Microsoft Docs
description: Dit document helpt u bij hoe aanbevelingen in Azure Security Center helpen u bij het beveiligen van uw Azure-resources en blijven in overeenstemming met beveiligingsbeleid.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/05/2018
ms.author: rkarlin
ms.openlocfilehash: 0581ddc4289f604533bae3650618fada15e02bef
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522234"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Aanbevelingen voor beveiliging in Azure Security Center beheren
Dit document helpt u bij het gebruik van de aanbevelingen in Azure Security Center kunt u uw Azure-resources beveiligen.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is geen stapsgewijze handleiding.
>
>

## <a name="what-are-security-recommendations"></a>Wat zijn aanbevelingen voor beveiliging?
De beveiligingsstatus van uw Azure-bronnen worden regelmatig door Security Center gecontroleerd. Wanneer met Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen gedaan. Deze aanbevelingen begeleiden u bij het configureren van de benodigde besturingselementen.

## <a name="implementing-security-recommendations"></a>Beveiligingsaanbevelingen implementeren
### <a name="set-recommendations"></a>Set-aanbevelingen
In [beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md), leert u het:

* Beveiligingsbeleid configureren.
* Schakel het verzamelen van gegevens.
* Kies welke aanbevelingen om te zien als onderdeel van uw beveiligingsbeleid.

Aanbevelingen van huidige beleidscentrum om de systeemupdates, basislijnregels, anti-malware-programma's, [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) op subnetten en netwerkinterfaces, controleren voor SQL database, transparent data encryption voor SQL database, Apps en web application firewalls.  [Beveiligingsbeleid instellen](security-center-policies.md) bevat een beschrijving van elke optie aanbeveling.

### <a name="monitor-recommendations"></a>Monitor voor aanbevelingen
Nadat er een beveiligingsbeleid is ingesteld, wordt met Security Center de beveiligingsstatus van de Azure-resources geanalyseerd om potentiële beveiligingsproblemen op te sporen. De **aanbevelingen** tegel onder **overzicht** kunt u het totale aantal aanbevelingen geïdentificeerd door Security Center weten.

![Aanbevelingen tegel][1]

De details van elke aanbeveling, selecteer de **aanbevelingen tegel** onder **overzicht**. **Aanbevelingen** wordt geopend.

![Aanbevelingen voor filteren][2]

U kunt filteren, aanbevelingen. Als u wilt de aanbevelingen filteren, selecteert u **Filter** op de **aanbevelingen** blade. De **Filter** blade wordt geopend en selecteert u de ernst en status waarden die u wilt zien.

De aanbevelingen worden weergegeven in tabelindeling, waarbij elke regel één bepaalde aanbeveling vertegenwoordigt. De kolommen van deze tabel zijn:

* **BESCHRIJVING**: de aanbeveling en wat er moet gebeuren om deze op te lossen.
* **RESOURCE**: geeft een lijst van de resources die deze aanbeveling van toepassing is.
* **STATUS**: Beschrijving van de huidige status van de aanbeveling:
  * **Open**: de aanbeveling nog niet is opgelost.
  * **Bezig**: de aanbeveling wordt momenteel toegepast op de resources en er is geen actie te ondernemen.
  * **Opgelost**: de aanbeveling is al voltooid (in dit geval wordt de regel is lichter gekleurd).
* **ERNST**: hiermee wordt de ernst van deze bepaalde aanbeveling beschreven:
  * **Hoge**: een beveiligingsprobleem bestaat een belangrijke resource (zoals een toepassing, een virtuele machine of een netwerkbeveiligingsgroep) en dit probleem vereist uw aandacht.
  * **Gemiddeld**: een beveiligingsprobleem bestaat en niet-kritieke of extra stappen zijn vereist om dit te dichten of om een proces te voltooien.
  * **Lage**: Er bestaat een beveiligingsprobleem die moet worden opgelost, maar geen onmiddellijke aandacht vereist. (Standaard lage aanbevelingen worden niet weergegeven, maar u kunt filteren op lage aanbevelingen als u wilt zien.)

Gebruik de onderstaande tabel als referentie om te begrijpen van de beschikbare aanbevelingen en wat elke doet als u deze toepast.

> [!NOTE]
> Wilt u inzicht in de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md) voor Azure-resources.
>
>

| Aanbeveling | Beschrijving |
| --- | --- |
| [Gegevensverzameling voor abonnementen inschakelen](security-center-enable-data-collection.md) |Hiermee wordt aanbevolen dat u gegevensverzameling in het beveiligingsbeleid voor elk van uw abonnementen en alle virtuele Azure-machines (VM's) en niet-Azure-computers inschakelen. |
| [Beveiligingsconfiguraties herstellen](security-center-remediate-os-vulnerabilities.md) |Hiermee wordt aanbevolen dat u de configuraties voor het besturingssysteem met de aanbevolen configuratie van beveiligingsregels, bijvoorbeeld uitlijnen, niet toestaan dat wachtwoorden worden opgeslagen. |
| [Systeemupdates toepassen](security-center-apply-system-updates.md) |Hiermee wordt aanbevolen dat u ontbrekende systeembeveiliging en essentiële updates op uw Windows en Linux-VM's en computers implementeren. |
| [Toepassen van een Just-In-Time netwerktoegangsbeheer](security-center-just-in-time.md) | Hiermee wordt aanbevolen dat u van toepassing just-in-time-VM-toegang zijn. De just in time functie is beschikbaar op de prijscategorie Standard van Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center. |
| [Opnieuw opstarten na systeemupdates](security-center-apply-system-updates.md#reboot-after-system-updates) |Hiermee wordt aanbevolen om een VM opnieuw op te starten om het proces van het toepassen van systeemupdates te voltooien. |
| [Een firewall voor webtoepassingen toevoegen](security-center-add-web-application-firewall.md) |Hiermee wordt aanbevolen dat u een web application firewall (WAF) voor web-eindpunten te implementeren. Een WAF-aanbeveling wordt voor een openbaar IP-adres (Instance Level IP of Load Balanced IP) met een gekoppelde netwerkbeveiligingsgroep met open binnenkomende webpoorten (80,443) weergegeven. </br>Security Center raadt aan dat u een WAF om te helpen beschermen tegen aanvallen die gericht is op uw webtoepassingen op virtuele machines en op App Service-omgeving inrichten. Een App Service Environment (ASE) is een [Premium](https://azure.microsoft.com/pricing/details/app-service/) service-plan-optie van Azure App Service waarmee u een volledig geïsoleerde en toegewezen omgeving voor Azure App Service-apps veilig kunnen worden uitgevoerd. Zie voor meer informatie over het as-omgeving, de [documentatie over App Service-omgeving](../app-service/environment/intro.md).</br>U kunt meerdere webtoepassingen in Security Center beveiligen door toe te voegen deze toepassingen aan uw bestaande WAF-implementaties. |
| [Toepassingsbeveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection) |Voor het voltooien van de configuratie van een WAF, moet verkeer worden omgeleid naar de WAF-apparaat. Deze aanbeveling te volgen, wordt de wijzigingen in de vereiste installatie voltooid. |
| [Een firewall van de volgende generatie toevoegen](security-center-add-next-generation-firewall.md) |Hiermee wordt aanbevolen dat u een volgende generatie Firewall (NGFW) toevoegen van een Microsoft-partner uw beveiligingen verhogen. |
| [Verkeer alleen via NGFW sturen](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Hiermee wordt aanbevolen dat u configureert (NSG) regels voor netwerkbeveiligingsgroepen die binnenkomend verkeer op uw virtuele machine via de NGFW afgedwongen. |
| [Eindpuntbeveiliging installeren](security-center-install-endpoint-protection.md) |Hiermee wordt aanbevolen om antimalwareprogramma's op VM's te installeren. (Alleen voor Windows-VM's.) |
| [Netwerkbeveiligingsgroepen inschakelen op subnetten of virtuele machines](security-center-enable-network-security-groups.md) |Hiermee wordt aanbevolen dat u nsg's op subnetten of virtuele machines inschakelen. |
| [Toegang via een Internetgericht eindpunt beperken](security-center-restrict-access-through-internet-facing-endpoints.md) |Hiermee wordt aanbevolen dat u regels voor binnenkomend verkeer voor nsg's configureren. |
| [Controle en detectie van bedreigingen op SQL-servers inschakelen](security-center-enable-auditing-on-sql-servers.md) |Hiermee wordt aanbevolen dat u controle en detectie van bedreigingen voor Azure SQL-servers inschakelen. (Alleen voor azure SQL-service. Bevat geen SQL die worden uitgevoerd op uw virtuele machines.) |
| [Controle en detectie van bedreigingen op SQL-databases inschakelen](security-center-enable-auditing-on-sql-databases.md) |Hiermee wordt aanbevolen dat u controle en detectie van bedreigingen voor Azure SQL-databases inschakelen. (Alleen voor azure SQL-service. Bevat geen SQL die worden uitgevoerd op uw virtuele machines.) |
| [Transparent Data Encryption inschakelen voor SQL-databases](security-center-enable-transparent-data-encryption.md) |Hiermee wordt aanbevolen dat u versleuteling voor SQL-databases inschakelen. (Alleen azure SQL-service). |
| [VM-agent inschakelen](security-center-enable-vm-agent.md) |Hiermee kunt u zien voor welke VM's de VM-agent is vereist. De VM-Agent moet worden geïnstalleerd op VM's voor het patchen van inrichten scannen, basislijnen te scannen en antimalwareprogramma's. De VM-agent wordt standaard geïnstalleerd op VM's die zijn geïmplementeerd vanuit Azure Marketplace. Het artikel [VM Agent and Extensions – Part 2)](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (VM-agent en -extensies – deel 2) bevat informatie over het installeren van de VM-agent. |
| [Schijfversleuteling toepassen](security-center-apply-disk-encryption.md) |Hiermee wordt aanbevolen om de VM-schijven te versleutelen met behulp van Azure Disk Encryption. (Voor VM's van Windows en Linux.) Versleuteling wordt aanbevolen voor het besturingssysteem en voor de gegevensvolumes op de VM. |
| [Contactgegevens voor beveiliging verstrekken](security-center-provide-security-contact-details.md) |Gebruiker wordt aanbevolen dat u beveiliging bieden contact opnemen met informatie voor elk van uw abonnementen. Contactgegevens is een e-mailadres en telefoonnummer getal. De informatie die wordt gebruikt om contact met u als beveiligingsteam vindt dat uw resources zijn aangetast. |
| [Besturingssysteemversie bijwerken](security-center-update-os-version.md) |Hiermee wordt aanbevolen dat u de versie van het besturingssysteem (OS) voor je Cloud Service naar de meest recente versie beschikbaar voor uw type besturingssysteem bijwerken.  Zie voor meer informatie over Cloud Services, de [overzicht van Cloud Services](../cloud-services/cloud-services-choose-me.md). |
| [Evaluatie van beveiligingsproblemen is niet geïnstalleerd](security-center-vulnerability-assessment-recommendations.md) |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| [Beveiligingsproblemen herstellen](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Hiermee ziet u beveiligingsproblemen in het systeem en in de toepassing die zijn gedetecteerd met de oplossing voor beveiligingslekken die op de VM is geïnstalleerd. |
| [Versleuteling inschakelen voor Azure Storage-Account](security-center-enable-encryption-for-storage-account.md) | Hiermee wordt aanbevolen dat u Azure Storage Service Encryption voor data-at-rest inschakelen. Storage Service Encryption (SSE) werkt door het versleutelen van de gegevens wanneer deze worden geschreven naar Azure storage en voordat ophalen ontsleutelt. SSE is momenteel alleen beschikbaar voor de Azure Blob-service en kan worden gebruikt voor blok-blobs, pagina-blobs en toevoeg-blobs. Zie voor meer informatie, [Storage Service Encryption voor data-at-rest](../storage/common/storage-service-encryption.md).</br>SSE wordt alleen ondersteund op Resource Manager-opslagaccounts. |
| [Besturingselementen voor adaptieve toepassingen inschakelen](security-center-adaptive-application.md) | Hiermee wordt aanbevolen dat u besturingselementen voor adaptieve toepassingen op uw Windows-VM's van toepassing zijn. Deze functie is beschikbaar op de prijscategorie Standard van Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center. |
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
| Meer dan één eigenaar van uw abonnement opgeven | Hiermee wordt aanbevolen dat u meer dan één abonnementseigenaar aanwijzen om de beheerder toegang redundantie. |
| Maximaal 3 eigenaren voor uw abonnement opgeven | Hiermee wordt aanbevolen dat u minder dan 3 abonnementseigenaren aanwijzen om te reduceren van de mogelijkheden voor inbreuk op een gecomprimeerde eigenaar. |
| MFA inschakelen voor accounts met eigenaarsmachtigingen voor uw abonnement | Hiermee wordt aanbevolen dat u multi-factor Authentication (MFA) voor alle abonnementsaccounts met administrator-bevoegdheden inschakelen om te voorkomen dat er inbreuk wordt gepleegd accounts of resources. |
| MFA inschakelen voor accounts met schrijfmachtigingen voor uw abonnement | Hiermee wordt aanbevolen dat u multi-factor Authentication (MFA) voor alle abonnementsaccounts met schrijfmachtigingen inschakelen om te voorkomen dat er inbreuk wordt gepleegd accounts of resources. |
| MFA inschakelen voor accounts met leesmachtigingen voor uw abonnement | Hiermee wordt aanbevolen dat u multi-factor Authentication (MFA) voor alle abonnementsaccounts met leesmachtigingen inschakelen om te voorkomen dat er inbreuk wordt gepleegd accounts of resources. |
| Externe accounts met leesmachtigingen verwijderen van uw abonnement | Hiermee wordt aanbevolen dat u externe accounts met leesmachtigingen van uw abonnement verwijderen om onbewaakte toegang te voorkomen. |
| Externe accounts met schrijfmachtigingen verwijderen van uw abonnement | Hiermee wordt aanbevolen dat u externe accounts met schrijfmachtigingen van uw abonnement verwijderen om onbewaakte toegang te voorkomen. |
| Externe accounts met eigenaarsmachtigingen van uw abonnement verwijderen | Hiermee wordt aanbevolen dat u externe accounts met eigenaarsmachtigingen uit uw abonnement verwijderen om onbewaakte toegang te voorkomen. |
| Afgeschafte accounts verwijderen van abonnement | Raadt u afgeschafte accounts van uw abonnementen. |
| Afgeschafte accounts met eigenaarsmachtigingen verwijderen uit abonnement | Raadt u afgeschafte accounts met eigenaarsmachtigingen van uw abonnementen. |

### <a name="apply-recommendations"></a>Toepassen van aanbevelingen
Bekijk alle aanbevelingen en besluiten die u moet eerst worden toegepast. U wordt aangeraden de ernstclassificatie die de te gebruiken aangezien de belangrijkste parameter om te evalueren welke aanbevelingen voor moet eerst worden toegepast.

Selecteer een aanbeveling in de tabel van de bovenstaande aanbevelingen, en doorlopen als een voorbeeld van hoe u een aanbeveling toepast.

## <a name="next-steps"></a>Volgende stappen
In dit document, kunt u kennisgemaakt met aanbevelingen voor beveiliging in Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) : informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
