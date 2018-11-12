---
title: PaaS-implementaties beveiligen | Microsoft Docs
description: " Informatie over de beveiligingsvoordelen van PaaS ten opzichte van andere service-modellen in de cloud en informatie over aanbevolen procedures voor het beveiligen van uw Azure PaaS-implementatie. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: techlake
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: terrylan
ms.openlocfilehash: 98eee29dc6810d35ee1792c601e6d2f147602cae
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262179"
---
# <a name="securing-paas-deployments"></a>PaaS-implementaties beveiligen

Dit artikel bevat informatie die u helpt:

- Informatie over de beveiligingsvoordelen van hosting van toepassingen in de cloud
- Evalueren van de beveiligingsvoordelen van het platform als een service (PaaS) en een andere cloud service-modellen
- De focus van de beveiliging niet wijzigen van een centraal netwerk in een aanpak die perimeter-identiteit gebaseerde beveiliging
- Algemene PaaS best practices voor aanbevelingen voor beveiliging implementeren

## <a name="cloud-security-advantages"></a>Voordelen van cloud-beveiliging
Er zijn beveiligingsvoordelen wordt in de cloud. In een on-premises-omgeving, kunnen organisaties hebben nog niet vervulde verantwoordelijkheden en beperkte resources beschikbaar te investeren in de beveiliging, die zorgt voor een omgeving waarin aanvallers kunnen gebruikmaken van beveiligingsproblemen op alle lagen.

![Beveiligingsvoordelen van het cloudtijdperk][1]

Organisaties kunnen de detectie van bedreigingen en de reactietijden verbeteren met behulp van een provider cloud-gebaseerde beveiligingsmogelijkheden en intelligentie van de cloud.  Doordat de verantwoordelijkheden van de cloudprovider krijgt organisaties meer beveiliging dekking, zodat ze voor het toewijzen van resources van de beveiliging en het budget aan andere prioriteiten in uw bedrijf.

## <a name="division-of-responsibility"></a>Deling van verantwoordelijkheid
Het is belangrijk om te begrijpen van de verdeling van de verantwoordelijkheid tussen u en Microsoft. On-premises eigenaar van de hele stack, maar wanneer u naar de cloud bepaalde verantwoordelijkheden overbrengen naar Microsoft. Verantwoordelijkheid in de volgende matrix ziet u de gebieden van de stack in een SaaS, PaaS en IaaS-implementatie waarbij u bent verantwoordelijk voor en Microsoft is verantwoordelijk voor.

![Verantwoordelijkheid zones][2]

Voor alle cloud-implementatietypen, u bent eigenaar van uw gegevens en identiteiten. U bent verantwoordelijk voor het beveiligen van de beveiliging van uw gegevens en identiteiten, on-premises bronnen en de cloudonderdelen u controle (dit is afhankelijk van het servicetype).

Taken die altijd door u, ongeacht het type van de implementatie behouden, zijn:

- Gegevens
- Eindpunten
- Account
- Toegangsbeheer

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>Beveiligingsvoordelen van een PaaS cloud service-model
Met behulp van dezelfde verantwoordelijkheid matrix, de beveiligingsvoordelen van een Azure PaaS-implementatie plaats van on-premises laten we kijken.

![Beveiligingsvoordelen van PaaS][3]

Aan de onderkant van de stack, de fysieke infrastructuur, vanaf vermindert Microsoft algemene risico's en -verantwoordelijkheden. Omdat de Microsoft-cloud voortdurend door Microsoft bewaakt wordt, is het moeilijk zijn om aan te vallen. Het zinvol niet zijn voor een aanvaller na te streven van Microsoft cloud als een doel. Als de aanvaller veel geld en bronnen, de aanvaller heeft is het waarschijnlijk om door te gaan naar een ander doel.  

Er is geen verschil tussen een PaaS-implementatie en on-premises in het midden van de stack. Op het niveau van de toepassing en een beheerlaag voor de account- en toegangsbeheer hebt u soortgelijke risico's. In de volgende sectie voor stappen van dit artikel wordt helpen we u aan de aanbevolen procedures voor het verwijderen van of het minimaliseren van deze risico's.

Aan de bovenkant van de stack, gegevensbeheer en rights management uitvoeren u op een risico's die kan worden opgelost door Sleutelbeheer. (Key management wordt beschreven in de aanbevolen procedures.) Sleutelbeheer is een extra verantwoordelijkheid, hebt u gebieden in een PaaS-implementatie die u niet langer hoeft te beheren zodat u resources naar Sleutelbeheer verplaatsen kunt.

Het Azure-platform biedt u ook sterk DDoS-beveiliging met behulp van verschillende technologieën op het netwerk. Alle typen van DDoS protection-methoden op het netwerk hebben echter hun beperkingen op basis van per-koppeling en per datacenter. Om te voorkomen van de impact van grote DDoS-aanvallen, kunt u profiteren van Azure core cloud mogelijkheden van het inschakelen van u om snel en automatisch de schaal vergroten tot verdediging tegen DDoS-aanvallen. Gaan we meer informatie over hoe u dit in de aanbevolen procedures-artikelen doen kunt.

## <a name="modernizing-the-defenders-mindset"></a>Modernisering van de Verdediger assume ')
Met PaaS komen implementaties een verschuiving van de algehele gang voor beveiliging. U schuiven hoeft voor het beheren van alles zelf verantwoordelijkheid delen met Microsoft.

Een ander belangrijk verschil tussen PaaS en traditionele on-premises implementaties, is een nieuwe weergave van de primaire beveiligingsperimeter bepaald. In het verleden was de beveiligingsperimeter primair on-premises is uw netwerk en de meeste on-premises beveiliging ontwerpen gebruiken het netwerk als de primaire beveiliging pivot. Voor PaaS-implementaties, u betere dienstverlening door de identiteit van de primaire beveiligingsperimeter overweegt.

## <a name="adopt-a-policy-of-identity-as-the-primary-security-perimeter"></a>Een beleid van de identiteit als de primaire beveiligingsperimeter nemen
Een van de vijf belangrijke kenmerken van cloudcomputing is ruime netwerktoegang, waardoor de netwerk-georiënteerde minder relevante overweegt. Het doel van veel van cloudcomputing om toe te staan gebruikers toegang krijgen tot resources, ongeacht de locatie. Voor de meeste gebruikers gaat de locatie zich ergens op het Internet.

De volgende afbeelding ziet u hoe de beveiligingsperimeter zich heeft ontwikkeld van een perimeter van het netwerk een perimeter-identiteit. Beveiliging wordt minder over het verdedigen van uw netwerk en meer informatie over uw gegevens beschermen, evenals de beveiliging van uw apps en gebruikers beheren. Het belangrijkste verschil is dat u pushen dichter bij de beveiliging wilt op wat belangrijk is voor uw bedrijf is.

![De identiteit als nieuwe beveiligingsperimeter][4]

In eerste instantie wordt Azure PaaS-services (bijvoorbeeld webrollen en Azure SQL) opgegeven weinig of geen traditionele netwerk perimeter-beveiliging. Het is duidelijk dat van het element doel is om te worden blootgesteld aan Internet (Webrol) en dat de verificatie de nieuwe omheining (bijvoorbeeld BLOB of Azure SQL biedt).

Moderne beveiligingsprocedures wordt ervan uitgegaan dat de kwaadwillende persoon de netwerkperimeter heeft geschonden. Moderne defense procedures zijn daarom verplaatst naar de identiteit. Organisaties moeten een perimeternetwerk identiteit gebaseerde beveiliging met sterke verificatie en autorisatie hygiëne (aanbevolen procedures) maken.

Beginselen en patronen voor de netwerkverbinding zijn beschikbaar voor tientallen jaren. Daarentegen is de branche relatief minder ervaring met het gebruik van identiteit als de primaire beveiligingsperimeter. Met die al zei, hebben we voldoende ervaring voor enkele algemene aanbevelingen die in het veld hebben bewezen en van toepassing op bijna alle PaaS-services verzameld.

Hier volgen de aanbevolen procedures voor het beheren van de perimeter identiteit.

**Beste**: beveiligen van uw sleutels en referenties voor het beveiligen van uw PaaS-implementatie.   
**Details**: verlies van sleutels en referenties is een veelvoorkomend probleem. U kunt een gecentraliseerde oplossing waarbij de sleutels en geheimen kunnen worden opgeslagen in hardware security modules gebruiken. Azure biedt u een HSM in de cloud met [Azure Key Vault](../key-vault/key-vault-whatis.md).

**Beste**: plaats geen referenties en andere geheimen in de broncode of GitHub.   
**Details**: het enige wat slechter dan een onbevoegde partij verlies van uw sleutels en referenties heeft er toegang toe hebben. Aanvallers kunnen profiteren van bot technologieën om sleutels en geheimen in broncodeopslagruimte zoals GitHub te vinden. Plaats niet sleutel en geheimen in deze openbare opslagplaatsen voor codes.

**Beste**: beveiligen van uw VM-beheerinterfaces van hybride PaaS en IaaS-services met behulp van een beheerinterface waarmee u op afstand beheren van deze virtuele machines rechtstreeks.   
**Details**: extern beheer protocollen zoals [SSH](https://en.wikipedia.org/wiki/Secure_Shell), [RDP](https://support.microsoft.com/kb/186607), en [PowerShell remoting](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) kan worden gebruikt. In het algemeen is het raadzaam dat u niet rechtstreekse externe toegang tot VM's vanaf het internet inschakelt.

Gebruik indien mogelijk alternatieve methoden, zoals het gebruik van virtuele particuliere netwerken in een Azure-netwerk. Als geen alternatieve methoden beschikbaar zijn, zorgt u ervoor dat u complexe wachtwoordzinnen en verificatie met twee factoren gebruiken (zoals [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md)).

**Beste**: sterke verificatie en autorisatie platforms te gebruiken.   
**Details**: federatieve identiteiten in Azure AD in plaats van aangepaste gebruikersopslag gebruiken. Wanneer u federatieve identiteiten, u profiteren van een benadering op basis van het platform en u het beheer van bevoegde identiteiten delegeren aan uw partners. Een federatieve identiteit aanpak is vooral belangrijk wanneer werknemers worden beëindigd en die gegevens moeten worden weerspiegeld in meerdere systemen voor identiteit en autorisatie.

Platform opgegeven verificatie en autorisatie-mechanismen gebruiken in plaats van aangepaste code. De reden is dat het ontwikkelen van aangepaste verificatiecode op te geven foutgevoelig kan zijn. De meeste van uw ontwikkelaars zijn niet beveiligingsexperts en waarschijnlijk niet op de hoogte van de finesses en de meest recente ontwikkelingen in de verificatie en autorisatie. Commerciële code (bijvoorbeeld van Microsoft) is vaak veel security gecontroleerd.

Gebruik verificatie met twee factoren. Verificatie met twee factoren is de huidige standaard voor verificatie en autorisatie omdat wordt voorkomen de gebruikersnaam en wachtwoord typen verificatie intrinsieke security zwakke punten dat. Toegang tot de Azure-beheer (portal/externe PowerShell)-interfaces en de klantgerichte services moet worden ontworpen en geconfigureerd voor het gebruik van [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md).

Gebruik standaard verificatieprotocollen, zoals OAuth2 en Kerberos. Deze protocollen zijn uitgebreid peer gecontroleerd en worden waarschijnlijk geïmplementeerd als onderdeel van uw platform-bibliotheken voor verificatie en autorisatie.

## <a name="use-threat-modeling-during-application-design"></a>Threat modeling tijdens het ontwerpen van de toepassing gebruiken
De Microsoft [Security Development Lifecycle](https://www.microsoft.com/en-us/sdl) geeft aan dat teams moeten deelnemen aan een proces genaamd threat modeling tijdens de ontwerpfase. Dit proces te vereenvoudigen, zodat Microsoft heeft gemaakt de [SDL Threat Modeling Tool](../security/azure-security-threat-modeling-tool.md). Ontwerp van de toepassing modelleren en inventariseren van [STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy) bedreigingen in alle vertrouwen grenzen kunnen catch ontwerpen fouten vroegtijdig op.

De volgende tabel geeft een lijst van de STRIDE-bedreigingen en enkele voorbeeld-oplossingen die gebruikmaken van Azure-functies biedt. Deze oplossingen werkt in elk geval niet.

| Bedreiging | De beveiligingseigenschap | Mogelijke Azure-platform-migratie |
| --- | --- | --- |
| Adresvervalsing (spoofing) | Verificatie | HTTPS-verbindingen vereisen. |
| Knoeien | Gegevensintegriteit | Valideer de SSL-certificaten. |
| Afwijzing | Geen weerlegbaarheid | Inschakelen van Azure [controle en diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). |
| Openbaarmaking van informatie | Vertrouwelijkheid | Gevoelige gegevens in rust versleutelen met behulp van [service-certificaten](https://docs.microsoft.com/rest/api/appservice/certificates). |
| Denial of service | Beschikbaarheid | Prestatiegegevens mogelijke situaties waarbij een denial-of-service controleren. Verbindingsfilters implementeren. |
| Misbruik van bevoegdheden | Autorisatie | Gebruik [Privileged Identity Management](../active-directory/privileged-identity-management/subscription-requirements.md). |

## <a name="develop-on-azure-app-service"></a>Ontwikkelen op Azure App Service
[Azure App Service](../app-service/app-service-web-overview.md) is een product met PaaS kunt u web- en mobiele apps voor elk platform of apparaat maken en verbinding maken met gegevens waar dan ook, in de cloud of on-premises. App Service bevat de web- en mobiele mogelijkheden die zijn eerder afzonderlijk verkrijgbaar waren in Azure Websites en Azure Mobile Services. Deze service bevat ook nieuwe mogelijkheden voor het automatiseren van bedrijfsprocessen en het hosten van cloud-API's. Als een geïntegreerde service heeft App Service een uitgebreide set mogelijkheden voor het web, mobiele en integratiescenario's.

Hieronder vindt u aanbevolen procedures voor het gebruik van App Service.

**Beste**: [verifiëren via Azure Active Directory](../app-service/app-service-authentication-overview.md).   
**Details**: App Service biedt een OAuth 2.0-service voor uw id-provider. OAuth 2.0 is gericht op client-ontwikkelaar eenvoudig te houden terwijl er specifieke autorisatiestromen voor webtoepassingen, bureaubladtoepassingen en mobiele telefoons. Azure AD maakt gebruik van OAuth 2.0 waarmee u kunt toestaan dat de toegang tot mobiele en webtoepassingen.

**Beste**: beperken van toegang op basis van de noodzaak om te weten en minimale bevoegdheden beveiligingsprincipes.   
**Details**: beperken van de toegang is van cruciaal belang voor organisaties die willen beveiligingsbeleid voor toegang tot gegevens afdwingen. RBAC kunt u machtigingen toewijzen aan gebruikers, groepen en toepassingen met een bepaald bereik. Zie voor meer informatie over het verlenen van toegang tot toepassingen voor gebruikers, [aan de slag met toegangsbeheer](../role-based-access-control/overview.md).

**Beste**: beveiligen van uw sleutels.   
**Details**: Azure Key Vault helpt bescherming van de cryptografische sleutels en geheimen die toepassingen en services in de cloud gebruiken. Met Key Vault, kunt u sleutels en geheimen (zoals verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels, versleutelen. PFX-bestanden en wachtwoorden) met behulp van sleutels die worden beveiligd door hardware security modules (HSM's). Voor extra zekerheid kunt u de sleutels importeren of genereren in HSM's. Zie [Azure Key Vault](../key-vault/key-vault-whatis.md) voor meer informatie. U kunt ook de Key Vault gebruiken voor het beheren van de TLS-certificaten met automatische verlenging.

**Beste**: beperken van binnenkomende bron-IP-adressen.   
**Details**: [App Service-omgeving](../app-service/environment/intro.md) heeft een virtueel netwerk-integratie-functie waarmee u beperkingen instellen voor binnenkomende bron-IP-adressen via netwerkbeveiligingsgroepen. Virtuele netwerken kunnen u Azure-resources in een internet-, routeerbaar netwerk dat u toegang tot plaatsen. Zie voor meer informatie, [uw app integreren met een Azure virtual network](../app-service/web-sites-integrate-with-vnet.md).

**Beste**: de beveiligingsstatus van uw App Service-omgevingen bewaken.   
**Details**: Azure Security Center gebruiken voor het bewaken van uw App Service-omgevingen. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, maakt het [aanbevelingen](../security-center/security-center-virtual-machine-recommendations.md) die u begeleiden bij het configureren van de benodigde besturingselementen.

> [!NOTE]
> Bewaking van App Service is in preview en beschikbaar is alleen op de [Standard-laag](../security-center/security-center-pricing.md) van Security Center.
>
>

## <a name="install-a-web-application-firewall"></a>Installeren van een web application firewall
Webtoepassingen zijn in toenemende mate het doel van aanvallen die gebruikmaken van veelvoorkomende bekende beveiligingsproblemen. Veelvoorkomende aanvallen zijn hierbij onder andere aanvallen met SQL-injecties en aanvallen via scripting op meerdere sites. Het kan een hele uitdaging zijn om dergelijke aanvallen in toepassingscode te voorkomen en dit kan tevens veel onderhoud, patching en controle vereisen op meerdere lagen van de toepassingstopologie. Een gecentraliseerde firewall voor webtoepassingen maakt het beveiligingsbeheer veel eenvoudiger en biedt toepassingsbeheerders meer veiligheid tegen bedreigingen of aanvallen. Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

[De Web application firewall (WAF)](../application-gateway/waf-overview.md) is een functie van Application Gateway die gecentraliseerde beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. WAF is gebaseerd op regels uit de [Open Web Application Security Project (OWASP) core rule set](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.0 of 2.2.9.

## <a name="monitor-the-performance-of-your-applications"></a>De prestaties van uw toepassingen controleren
Bewaken is het verzamelen en analyseren van gegevens om te bepalen van de prestaties, status en beschikbaarheid van uw toepassing. Een effectieve bewakingsstrategie helpt u de gedetailleerde werking van de onderdelen van uw toepassing te begrijpen. Hiermee kunt u uw bedrijfstijd op de hoogte van kritieke problemen zodat u deze oplossen kunt voordat ze problemen worden vergroot. Hiermee kunt u ook afwijkingen die mogelijk die betrekking hebben.

Gebruik [Azure Application Insights](https://azure.microsoft.com/documentation/services/application-insights) voor het bewaken van beschikbaarheid, prestaties en gebruik van uw toepassing, of deze in de cloud of on-premises wordt gehost. Met behulp van Application Insights, kunt u snel identificeren en fouten opsporen in uw toepassing zonder te wachten op een gebruiker ze heeft gerapporteerd. Met de informatie die u verzamelt, kunt u weloverwogen keuzes maken over het onderhoud en de verbeteringen van uw toepassing.

Application Insights heeft uitgebreide hulpmiddelen voor interactie met de gegevens die worden verzameld. Application Insights slaat zijn gegevens op in een gemeenschappelijke opslagplaats. Het kan profiteren van gedeelde functionaliteit zoals waarschuwingen, dashboards en diepgaande analyse met de Log Analytics-querytaal.



## <a name="next-steps"></a>Volgende stappen
In dit artikel richten we ons op beveiligingsvoordelen van een Azure PaaS-implementatie en aanbevolen procedures voor beveiliging voor cloud-toepassingen. Hierna leert u aanbevolen procedures voor het beveiligen van uw PaaS-web- en mobiele oplossingen met behulp van specifieke Azure-services. We beginnen met Azure App Service, Azure SQL Database en Azure SQL Data Warehouse en Azure Storage. Artikelen over aanbevolen procedures voor andere Azure-services beschikbaar komen, worden koppelingen worden opgegeven in de volgende lijst:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Azure SQL Database en Azure SQL datawarehouse](security-paas-applications-using-sql.md)
- [Azure Storage](security-paas-applications-using-storage.md)
- Azure REDIS-Cache
- Azure Service Bus
- Web Application Firewalls

Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.

De volgende resources zijn beschikbaar, voor meer algemene informatie over Azure-beveiliging en gerelateerde Microsoft-services:
* [Azure-Team Beveiligingsblog](https://blogs.msdn.microsoft.com/azuresecurity/) - voor up-to-date informatie over het laatste nieuws over Azure-beveiliging
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - hier Microsoft beveiligingsproblemen, met inbegrip van problemen met Azure, kunnen u ook via e-mail secure@microsoft.com

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
