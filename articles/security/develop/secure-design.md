---
title: Ontwerpen van veilige toepassingen op Microsoft Azure
description: In dit artikel wordt beschreven aanbevolen procedures overwegen tijdens de fasen van de vereiste en het ontwerp van uw web application-project.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/11/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 12b9793cabb261368c437bd2ae2dbb39cf078bef
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653282"
---
# <a name="design-secure-applications-on-azure"></a>Beveiligde toepassingen ontwerpen in Azure
In dit artikel geven we beveiligingsactiviteiten en besturingselementen om te overwegen bij het ontwerpen van toepassingen voor de cloud. Trainingsresources samen met vragen over de beveiliging en concepten kunt u tijdens de vereisten en ontwerpen van de fasen van de Microsoft [Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) vallen. Het doel is voor het definiëren van activiteiten en Azure-services die u gebruiken kunt om een beter beveiligde toepassing te ontwerpen.

De volgende fasen van de SDL worden in dit artikel besproken:

- Training
- Vereisten
- Ontwerp

## <a name="training"></a>Training
Voordat u begint met het ontwikkelen van uw cloudtoepassing, even duren om te begrijpen van de beveiliging en privacy in Azure. Door deze stap duurt, kunt u het aantal en de ernst van kwetsbaarheden exploiteerbaar beperken in uw toepassing. U zult beter voorbereid om te reageren op de juiste wijze aan de voortdurend veranderende bedreigingen.

Gebruik de volgende bronnen in de fase training om vertrouwd te raken met de Azure-services die beschikbaar voor ontwikkelaars zijn en aanbevolen procedures voor beveiliging op Azure:

  - [Ontwikkelaarshandleiding voor Azure](https://azure.microsoft.com/campaigns/developer-guide/) ziet u hoe u aan de slag met Azure. De handleiding wordt beschreven welke services kunt u uw toepassingen uitvoeren, opslaan van uw gegevens, intelligence opnemen, IoT-apps ontwikkelen en implementeren van uw oplossingen op een meer efficiënte en veilige manier.

  - [Introductiehandleiding voor Azure-ontwikkelaars](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide) biedt essentiële informatie voor ontwikkelaars die op zoek bent naar aan de slag met het Azure-platform voor hun ontwikkeling.

  - [SDK's en hulpprogramma's](https://docs.microsoft.com/azure/index#pivot=sdkstools) beschrijft de hulpprogramma's die beschikbaar op Azure zijn.

  - [Azure DevOps-Services](https://docs.microsoft.com/azure/devops/) ontwikkeling biedt hulpprogramma's voor samenwerking. De hulpprogramma's omvatten krachtige pijplijnen, gratis Git-opslagplaatsen, configureerbare kanbanborden en uitgebreide geautomatiseerde en cloud-gebaseerde belastingtests uitvoeren.
    De [DevOps Resource Center](https://docs.microsoft.com/azure/devops/learn/) combineert onze bronnen voor het leren van DevOps procedures, Git versiebeheer, flexibele methoden, hoe we met DevOps bij Microsoft werken en hoe u de voortgang van uw eigen DevOps kunt beoordelen.

  - [Top 5 security rekening mee moet houden voordat u naar productie pusht](https://docs.microsoft.com/learn/modules/top-5-security-items-to-consider/index?WT.mc_id=Learn-Blog-tajanca) ziet u hoe u uw webtoepassingen op Azure beveiligen en beschermen van uw apps het meest voorkomende en gevaarlijke toepassing tegen aanvallen via Internet.

  - [Secure DevOps Kit voor Azure](https://azsk.azurewebsites.net/index.html) is een verzameling van scripts, hulpprogramma's, extensies en automatische die caters naar de uitgebreide Azure-abonnement en resourcegroep beveiligingsbehoeften van DevOps-teams die gebruikmaken van uitgebreide automation. De Secure DevOps Kit voor Azure kunt laten zien hoe u beveiliging naadloos integreren in uw eigen DevOps-werkstromen. De kit adressen hulpprogramma's zoals security verificatietests uitvoeren (SVTs), waarmee ontwikkelaars kunnen veilige code schrijven en testen van de veilige configuratie van hun cloud-toepassingen in de fasen van de ontwikkeling van code en vroege.

  - [Aanbevolen beveiligingsprocedures voor Azure-oplossingen](https://azure.microsoft.com/resources/security-best-practices-for-azure-solutions) biedt een verzameling van security best practices te gebruiken als u ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.

## <a name="requirements"></a>Vereisten
De fase van de definitie van vereisten is een belangrijke stap bij het definiëren van wat er in uw toepassing is en wat er gebeurt wanneer dit wordt uitgebracht. De fase van de vereisten is ook een tijd om na te denken over de besturingselementen voor de beveiliging die u in uw toepassing bouwt. Tijdens deze fase moet u ook de stappen die u gedurende de SDL onderneemt om ervoor te zorgen dat u de release en beveiligde toepassingen implementeren begint.

### <a name="consider-security-and-privacy-issues"></a>Houd rekening met beveiliging en privacy
Deze fase is het beste moment om te overwegen fundamentele beveiliging en privacy-problemen. Aanvaardbaar niveau van beveiliging en privacy aan het begin van een project te definiëren, kunt een team:

- Krijg inzicht in risico's die zijn gekoppeld aan beveiligingsproblemen met zich mee.
- Opsporen en oplossen van fouten die beveiliging tijdens de ontwikkeling.
- Van toepassing tot stand gebrachte niveaus van beveiliging en privacy in de gehele project.

Bij het schrijven van de vereisten voor uw toepassing, moet u rekening houden met beveiligingsmaatregelen waarmee uw toepassing en gegevens veilig worden bewaard.

### <a name="ask-security-questions"></a>Stel vragen over de beveiliging
Stel vragen over de beveiliging, zoals:

  - Bevat mijn toepassing gevoelige gegevens?

  - Wordt mijn toepassing verzamelen of opslaan van gegevens die is vereist om te voldoen aan de industrienormen en nalevingsprogramma's, zoals de [federale financiële instelling onderzoek Raad (FFIEC)](https://docs.microsoft.com/azure/security/blueprints/ffiec-analytics-overview) of de [Payment Card Industry Data Security Standards (PCI DSS)](https://docs.microsoft.com/azure/security/blueprints/pcidss-analytics-overview)?

  - Mijn toepassing verzamelen of gevoelige gegevens voor persoonlijke of klant die kan worden gebruikt, op een eigen of met andere informatie te identificeren, neem contact op met, of één persoon vinden?

  - Mijn toepassing verzamelen of gegevens die kunnen worden gebruikt voor toegang tot een persoon medisch, onderwijs, financiële of dienst informatie bevatten? Identificeren van de vertrouwelijkheid van uw gegevens tijdens de fase van de vereisten, kunt u uw gegevens classificeren en de methode voor gegevensbeveiliging die u voor uw toepassing gebruiken wilt te identificeren.

  - Waar en hoe worden mijn gegevens dan opgeslagen? Houd rekening met hoe u de storage-services die uw toepassing wordt gebruikt voor eventuele onverwachte wijzigingen (zoals tragere responstijden) worden gecontroleerd. Wordt u mogelijk logboekregistratie naar meer gedetailleerde gegevens verzamelen en analyseren van een probleem in de diepte van invloed zijn op?

  - Worden mijn toepassing beschikbaar is voor de openbare (op het internet) zijn of intern alleen? Als uw toepassing openbaar beschikbaar is, hoe beveiligt u de gegevens die worden gebruikt in de verkeerde manier kunnen worden verzameld? Als uw toepassing beschikbaar intern alleen, kunt u die in uw organisatie toegang tot de toepassing moet hebben en hoe lang ze toegang moeten hebben.

  - Begrijpt u het identiteitsmodel van uw voordat u begint met het ontwerpen van uw toepassing? Hoe bepaalt u dat gebruikers zijn wie ze beweren te zijn en wat een gebruiker is gemachtigd om u te doen?

  - Mijn toepassing gevoelige of belangrijke taken (zoals het overbrengen van geld, deuren ontgrendelen of leveren van medicijnen) uitvoeren?
    Houd rekening met hoe u dat de gebruiker een gevoelige taak die is gemachtigd om uit te voeren van de taak wordt gevalideerd en hoe u verifieert dat de persoon is wie ze beweren te zijn. Autorisatie (AuthZ) is de handeling van het verlenen van een geverifieerde principal beveiligingsmachtiging om iets te doen. Verificatie (AuthN) is de handeling van een partij voor legitieme referenties lastig.

  - Voert geen riskante software-activiteiten, zoals het toestaan van gebruikers om te uploaden of downloaden van bestanden of andere gegevens uit door mijn toepassing? Als uw toepassing riskante activiteiten uitvoeren, kunt u overwegen hoe uw toepassing gebruikers zullen beschermen tegen schadelijke bestanden of gegevens verwerken.

### <a name="review-owasp-top-10"></a>Beoordeling OWASP top 10
Houd rekening met het controleren van de [ <span class="underline">OWASP Top 10 toepassing beveiligingsrisico's</span>](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).
De OWASP Top 10 adressen kritieke beveiligingsrisico's voor web-apps.
Kennis van deze beveiligingsrisico's kunt u de vereiste en ontwerp beslissingen te nemen die deze risico's in uw toepassing te minimaliseren.

Nadenken over beveiligingsmaatregelen om te voorkomen dat schendingen is belangrijk.
Echter, wilt u ook [wordt ervan uitgegaan dat er inbreuk wordt gepleegd](https://docs.microsoft.com/azure/devops/learn/devops-at-microsoft/security-in-devops) wordt uitgevoerd. Ervan uitgaande dat er inbreuk wordt gepleegd kunt beantwoorden van enkele belangrijke vragen over de beveiliging van tevoren verkrijgt, zodat ze niet hoeven te worden beantwoord in geval van nood:

  - Hoe wordt ik een aanval detecteren?

  - Wat wordt ik doen als er een aanval of inbreuk op?

  - Hoe ga ik om te herstellen van de aanval, zoals gegevens lekken en onrechtmatig gebruik?

## <a name="design"></a>Ontwerp

De ontwerpfase is essentieel voor het tot stand brengen van aanbevolen procedures voor het ontwerpen en functionele specificaties. Het is ook essentieel voor het uitvoeren van een risicoanalyse die verhelpen van problemen met beveiliging en privacy in een project.

Wanneer u beschikken over vereisten voor beveiliging en veilig ontwerpconcepten gebruiken, kunt u voorkomen of minimaliseren mogelijkheden voor een beveiligingsfout. Een beveiligingsfout is een toezicht in het ontwerp van de toepassing die mogelijk een gebruiker kan schadelijke of onverwachte acties uitvoeren nadat u uw toepassing wordt vrijgegeven.

Tijdens de ontwerpfase ook nadenken over hoe u beveiliging in lagen; kunt toepassen Eén niveau van bescherming niet noodzakelijkerwijs genoeg. Wat gebeurt er als een aanvaller het verleden web application firewall (WAF)? Wilt u een andere beveiligingscontrole om te zetten voor bescherming tegen deze aanvallen.

Met dat gegeven in gedachte bespreken we de volgende concepten voor veilig ontwerp en de beveiligingsbesturingselementen die u rekening moet houden bij het ontwerpen van veilige toepassingen:

- Gebruik een bibliotheek met veilige codering en een softwareframework.
- De scan voor kwetsbare onderdelen.
- Gebruik threat modeling tijdens het ontwerpen van de toepassing.
- Verminder de kwetsbaarheid voor aanvallen.
- Een beleid van de identiteit als de primaire beveiligingsperimeter nemen.
- Hernieuwde verificatie vereisen voor belangrijke transacties.
- Een oplossing voor sleutelbeheer gebruiken voor het beveiligen van sleutels, referenties en andere geheimen.
- Gevoelige gegevens beveiligen.
- Implementeer failsafe metingen.
- Profiteer van de fout en de afhandeling van uitzonderingen.
- Gebruik logboekregistratie en waarschuwingen.

### <a name="use-a-secure-coding-library-and-a-software-framework"></a>Een bibliotheek met veilige codering en een softwareframework gebruiken

Voor ontwikkeling, een bibliotheek met veilige codering en een softwareframework dat is ingesloten security te gebruiken. Ontwikkelaars kunnen gebruiken bestaande functies (versleuteling, riolering invoer, uitvoercodering, sleutels of verbindingsreeksen en zaken die kan worden gezien als een beveiligingscontrole) een bewezen oplossing in plaats van het ontwikkelen van beveiligingsmaatregelen helemaal. Dit zorgt voor de bescherming tegen beveiligingsgerelateerde ontwerp en implementatie-fouten.

Zorg ervoor dat u de meest recente versie van uw framework en de beveiligingsfuncties die beschikbaar in het kader zijn. Microsoft biedt een uitgebreide [set ontwikkeltools](https://azure.microsoft.com/product-categories/developer-tools/) voor alle ontwikkelaars, op elk platform of taal, voor het leveren van cloud-Apps. U kunt de code met de taal van uw keuze door te kiezen uit verschillende [SDK's](https://azure.microsoft.com/downloads/).
U kunt profiteren van de complete geïntegreerde ontwikkelingsomgevingen (IDE's) en editors dat zijn geavanceerde mogelijkheden voor foutopsporing en geïntegreerde ondersteuning van Azure.

Microsoft biedt diverse [programmeertalen, frameworks en hulpprogramma's](https://docs.microsoft.com/azure/index#pivot=sdkstools&panel=sdkstools-all) waarmee u kunt het ontwikkelen van toepassingen op Azure. Een voorbeeld is [Azure voor .NET en .NET Core-ontwikkelaars](https://docs.microsoft.com/dotnet/azure/). Voor elke taal en het framework dat we bieden, vindt u snelstartgidsen, zelfstudies en API-verwijzingen om u snel aan de slag te helpen.

Azure biedt tal van services die kunt u websites en webtoepassingen om toepassingen te hosten. Deze services kunt u ontwikkelen in uw favoriete taal, ongeacht of het nu .NET, .NET Core, Java, Ruby, Node.js, PHP of Python.
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview) (Web-Apps) is een van deze services.

De kracht van Microsoft Azure Web Apps toegevoegd aan uw toepassing. Het bevat beveiliging, taakverdeling, automatisch schalen en geautomatiseerd beheer. U kunt ook profiteren van DevOps-mogelijkheden in Web-Apps, zoals Pakketbeheer, staging-omgevingen, aangepaste domeinen, SSL/TLS-certificaten en continue implementatie vanuit Azure DevOps, GitHub, Docker Hub en andere bronnen.

Azure biedt andere services die u websites en webtoepassingen om toepassingen te hosten kunt. Voor de meeste scenario's is Web Apps de beste keuze. Voor een servicearchitectuur micro-, kunt u overwegen [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric).
Als u meer controle wilt over de virtuele machines waarop uw code wordt uitgevoerd, kunt u [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/) overwegen.
Zie voor meer informatie over hoe u kiest tussen deze Azure-services, een [vergelijking van Azure App Service, virtuele Machines, Service Fabric en Cloud Services](https://docs.microsoft.com/azure/app-service/choose-web-site-cloud-service-vm).

### <a name="apply-updates-to-components"></a>Updates toepassen op onderdelen

Om te voorkomen dat zwakke plekken, moet u zowel de client en server-side onderdelen (bijvoorbeeld, frameworks en bibliotheken) en hun afhankelijkheden voor updates continu inventariseren. Beveiligingsproblemen van de nieuwe en bijgewerkte softwareversies worden continu vrijgegeven. Zorg ervoor dat u een voortdurend plan bewaken hebt, sorteren en toepassen van updates of configuratiewijzigingen naar de bibliotheken en -onderdelen die u wilt gebruiken.

Zie de [Open Web Application Security Project (OWASP)](https://www.owasp.org/index.php/Main_Page) op pagina [componenten gebruiken met bekende beveiligingsproblemen](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities) voor hulpprogramma suggesties. U kunt ook abonneren op e-mailwaarschuwingen voor beveiligingsproblemen die betrekking hebben op de onderdelen die u wilt gebruiken.

### <a name="use-threat-modeling-during-application-design"></a>Threat modeling tijdens het ontwerpen van de toepassing gebruiken

Risicomodel is het proces van potentiële beveiligingsrisico's op uw bedrijf en de toepassing te identificeren en vervolgens hebt gecontroleerd of de juiste oplossingen voldaan is. De SDL geeft aan dat teams threat modeling tijdens de ontwerpfase, wanneer het oplossen van mogelijke problemen relatief eenvoudig en kosteneffectief is moeten bezighouden. Met behulp van threat modellen in de ontwerpfase total cost of ontwikkeling drastisch kan verlagen.

We ontworpen om te vergemakkelijken de threat modeling proces, de [SDL Threat Modeling Tool](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) met niet-beveiligingsexperts in gedachten. Dit hulpprogramma eenvoudiger risicomodel voor alle ontwikkelaars door op te geven informatie over het maken en bedreigingsmodellen analyseren.

Ontwerp van de toepassing modelleren en inventariseren van [STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy) bedreigingen-Spoofing, Tampering, Repudiation, openbaarmaking van informatie, denial of Service en Elevation of Privilege — in alle vertrouwen de grenzen van een effectieve manier heeft bewezen om af te vangen ontwerpfouten vroegtijdig op. De volgende tabel geeft een lijst van de STRIDE-bedreigingen en biedt enkele voorbeeld-oplossingen die gebruikmaken van functies die worden geleverd door Azure. Deze oplossingen werkt in elk geval niet.

| Bedreiging | De beveiligingseigenschap | Mogelijke Azure-platform risicobeperking |
| ---------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Adresvervalsing (spoofing)               | Authentication        | [HTTPS-verbindingen vereisen](https://docs.microsoft.com/aspnet/core/security/enforcing-ssl?view=aspnetcore-2.1&tabs=visual-studio). |
| Manipulatie              | Gegevensintegriteit             | Valideren van SSL/TLS-certificaten. Toepassingen die gebruikmaken van SSL/TLS moeten de X.509-certificaten van de entiteiten die ze verbinding met maken volledig controleren. Azure Key Vault-certificaten te gebruiken [beheren van uw x509 certificaten](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-certificates). |
| Afwijzing            | Niet-afwijzing       | Inschakelen van Azure [controle en diagnose](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).|
| Openbaarmaking van informatie | Vertrouwelijkheid       | Versleutelen van gevoelige gegevens [at-rest](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest) en [in-transit](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices#protect-data-in-transit). |
| Denial of service (DoS)      | Beschikbaarheid          | Prestatiegegevens voor mogelijke denial of service-voorwaarden controleren. Verbindingsfilters implementeren. [Azure DDoS protection](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview#next-steps), gecombineerd met aanbevolen procedures voor ontwerp van toepassingen, biedt beveiliging tegen DDoS-aanvallen.|
| Verhoging van bevoegdheden | Authorization         | Gebruik van Azure Active Directory <span class="underline"> </span> [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure).|

### <a name="reduce-your-attack-surface"></a>Uw kwetsbaarheid voor aanvallen verminderen

Een kwetsbaarheid voor aanvallen is de totale som van waar mogelijke beveiligingsproblemen optreden. In dit artikel, we ons richten op de kwetsbaarheid voor aanvallen van een toepassing.
De focus zich op een toepassing te beveiligen tegen aanvallen. Een snel en eenvoudig manier om te minimaliseren uw kwetsbaarheid voor aanvallen is het niet-gebruikte resources en code van uw toepassing te verwijderen. De kleinste uw toepassing, de kleinere uw kwetsbaarheid voor aanvallen. Bijvoorbeeld verwijderen:

- Code voor de functies die u nog niet hebt vrijgegeven.
- Ondersteuningscode voor foutoplossing.
- Netwerkinterfaces en protocollen die niet worden gebruikt of die zijn afgeschaft.
- Virtuele machines en andere bronnen die u niet gebruikt.

Uitvoeren van reguliere opschonen van uw resources en ervoor te zorgen dat u niet-gebruikte code verwijderen zijn goede manieren om te zorgen dat er minder mogelijkheden voor kwaadwillende actoren voor aanvallen.

Er is een meer gedetailleerde en uitgebreide manier om uw kwetsbaarheid voor aanvallen verminderen om uit te voeren van de analyse van een aanval. Analyse van een aanval kunt u de onderdelen van een systeem dat moet worden gecontroleerd en getest voor beveiligingsproblemen worden toegewezen.

Het doel van een analyse van aanval is om te begrijpen van de gebieden van de risico's in een toepassing, zodat ontwikkelaars en beveiligingsspecialisten zich bewust zijn van welke onderdelen van de toepassing worden geopend om aan te vallen. Vervolgens vindt u manieren om te minimaliseren van deze mogelijke, bijhouden hoe en wanneer de kwetsbaarheid voor aanvallen verandert en wat dit betekent dat vanuit het oogpunt van de risico's.

Analyse van een aanval kunt u identificeren:

- Functies en onderdelen van het systeem moet u om te controleren en te testen op beveiligingsproblemen.
- Met een hoog risico gebieden van code die moeten ingrijpende beveiliging (delen van het systeem die u nodig hebt om te beschermen).
- Wanneer u de kwetsbaarheid voor aanvallen alter en moet vernieuwen van een beoordeling van bedreigingen.

Mogelijkheden voor aanvallers een potentiële zwakke plek of een beveiligingsprobleem verminderen, moet u zorgvuldig analyseren van uw toepassing algehele kwetsbaarheid voor aanvallen. Dit omvat ook het uitschakelen of beperken van toegang tot systeemservices, toepassen van het principe van minimale bevoegdheden, en die gebruikmaakt van een gelaagde bescherming mogelijk.

We bespreken [uitvoeren van een aanval surface beoordeling](secure-develop.md#conduct-attack-surface-review) tijdens de verificatie-fase van de SDL.

> [!NOTE]
> **Wat is het verschil tussen risicomodel en analyse van aanval?**
Risicomodel is het proces van het identificeren van mogelijke bedreigingen voor uw toepassing en ervoor te zorgen dat de juiste oplossingen tegen de bedreigingen in plaats zijn. Analyse van aanval wordt aangeduid met een hoog risico gebieden van code die zijn geopend om aan te vallen. Dit omvat het zoeken naar manieren om u te beschermen met een hoog risico gebieden van uw toepassing en controleren en deze gebieden van de code te testen voordat u de toepassing implementeert.

### <a name="adopt-a-policy-of-identity-as-the-primary-security-perimeter"></a>Een beleid van de identiteit als de primaire beveiligingsperimeter nemen

Bij het ontwerpen van cloud-toepassingen, is het belangrijk om uit te breiden focus van de perimeter van een netwerk gebaseerde aanpak een identiteit gerichte aanpak. In het verleden was het beveiligingsperimeter primair on-premises netwerk van een organisatie. De meeste on-premises beveiliging ontwerpen gebruiken het netwerk als de primaire beveiliging pivot. Voor cloud-toepassingen, u betere dienstverlening door de identiteit als de primaire beveiligingsperimeter rekening te houden.

Wat u kunt doen voor het ontwikkelen van een identiteit gebaseerde benadering voor het ontwikkelen van webtoepassingen:

- Multi-factor authentication voor gebruikers afdwingen.
- Sterke verificatie en autorisatie platforms te gebruiken.
- Het principe van minimale bevoegdheden van toepassing.
- Just-in-time-toegang implementeren.

#### <a name="enforce-multi-factor-authentication-for-users"></a>Multi-factor authentication voor gebruikers afdwingen

Gebruik verificatie met twee factoren. Verificatie met twee factoren is de huidige standaard voor verificatie en autorisatie omdat wordt voorkomen dat de beveiliging zwakke punten die inherent aan de gebruikersnaam en wachtwoord typen verificatie zijn. Toegang tot de Azure management-interfaces (Azure portal/externe PowerShell) en klantgerichte services moet worden ontworpen en geconfigureerd voor het gebruik van [Azure multi-factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication).

#### <a name="use-strong-authentication-and-authorization-platforms"></a>Sterke verificatie en autorisatie platforms te gebruiken

Platform opgegeven verificatie en autorisatie-mechanismen gebruiken in plaats van aangepaste code. Dit komt doordat het ontwikkelen van aangepaste verificatiecode op te geven kunt foutgevoelig zijn. Commerciële code (bijvoorbeeld van Microsoft) vaak wordt uitgebreid beoordeeld voor beveiliging. [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) de Azure-oplossing voor identiteits-en toegang. Deze Azure AD-hulpprogramma's en services kunt u helpen met veilige ontwikkeling:

- [Azure AD identity-platform (Azure AD voor ontwikkelaars)](https://docs.microsoft.com/azure/active-directory/develop/about-microsoft-identity-platform) is een identiteitsservice in de cloud waarmee ontwikkelaars apps maken waarmee gebruikers veilig aanmelden. Azure AD biedt ondersteuning voor ontwikkelaars die één tenant, line-of-business (LOB)-apps bouwen en ontwikkelaars die behoefte hebben aan het ontwikkelen van apps met meerdere tenants. Naast eenvoudige aanmelding, worden de apps die zijn gebouwd met behulp van Azure AD kunnen aanroepen Microsoft APIs en aangepaste API's die zijn gebouwd op de Azure AD-platform. Het Azure AD identity-platform biedt ondersteuning voor standaardprotocollen zoals OAuth 2.0 en OpenID Connect.

- [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/) is een identity management-service die u kunt aanpassen en beheren hoe klanten zich registreren, aanmelden en hun profielen beheren wanneer ze uw toepassingen. Dit omvat toepassingen die zijn ontwikkeld voor iOS, Android en .NET, onder andere. Azure AD B2C kunt deze acties tijdens het beveiligen van de klantidentiteiten.

#### <a name="apply-the-principle-of-least-privilege"></a>Het principe van minimale bevoegdheden toepassen

Het concept van [minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) betekent dat gebruikers de precieze niveau van toegang tot en beheer ze moeten gaan doen van hun taken en niets meer te geven.

Een software-ontwikkelaar zou hebt domeinbeheerdersrechten nodig? Moet een administratief medewerker toegang tot beheerfuncties op hun pc? Evaluatie van toegang tot software is niet anders. Als u [op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) zodat gebruikers verschillende mogelijkheden en -instantie in uw toepassing u iedereen wouldn't geeft toegang tot alles. Door het beperken van toegang tot wat vereist voor elke rol is, beperkt u het risico van een security probleem optreedt.

Zorg ervoor dat uw toepassing dwingt [minimale bevoegdheden](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models#in-applications) gedurende de patronen voor databasetoegang.

> [!NOTE]
>  De regels van minimale bevoegdheden nodig om toe te passen op de software en de personen die het maken van de software. Software-ontwikkelaars kunnen een grote risico's voor IT-beveiliging worden als ze zijn te veel toegang krijgen. De gevolgen zijn ernstige als een ontwikkelaar kwade bedoelingen heeft of te veel toegang wordt verleend. U wordt aangeraden dat de regels van minimale bevoegdheden worden toegepast op ontwikkelaars gedurende de ontwikkelingscyclus.

#### <a name="implement-just-in-time-access"></a>Just-in-time-toegang implementeren

Implementeer *just-in-time* (JIT) toegang tot de blootstellingstijd van bevoegdheden verder te verlagen. Gebruik [Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure#stage-3-build-visibility-and-take-full-control-of-admin-activity) aan:

- Geef gebruikers de machtigingen die ze nodig hebben alleen JIT.
- Rollen toewijzen voor een kortere duur met vertrouwen dat de bevoegdheden automatisch worden ingetrokken.

### <a name="require-re-authentication-for-important-transactions"></a>Hernieuwde verificatie vereist is voor belangrijke transacties

[Aanvraag voor cross-site kunnen worden vervalst](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery?view=aspnetcore-2.1) (ook wel bekend als *XSRF* of *CSRF*) is een aanval tegen waarin een schadelijke web-app is van invloed op de interactie tussen de browser van een client en een web-gehoste web-apps de App dat die browser vertrouwt. Aanvraag voor cross-site kunnen worden vervalst aanvallen zijn mogelijk omdat webbrowsers bepaalde typen verificatietokens automatisch met elke aanvraag naar een website verzenden.
Deze vorm van misbruik wordt ook wel een *aanval met één muisklik* of *sessie hing* omdat de aanval gebruik van maakt de eerder sessie is geverifieerd voor de gebruiker.

De beste manier om te zetten voor bescherming tegen dit soort aanval is te vragen de gebruiker iets dat alleen de gebruiker voor elk belangrijk transactie, zoals een aankoop, account deactiveren of een wachtwoordwijziging bieden kan. U mogelijk vraagt de gebruiker het wachtwoord opnieuw invoeren of een captcha volledig of dien een token voor geheim die alleen de gebruiker kan hebben. De meest voorkomende aanpak is het token voor geheim.

### <a name="use-a-key-management-solution-to-secure-keys-credentials-and-other-secrets"></a>Een oplossing voor sleutelbeheer gebruiken voor het beveiligen van sleutels, referenties en andere geheimen

Verlies van sleutels en referenties is een veelvoorkomend probleem. Het enige wat slechter dan een onbevoegde partij verlies van uw sleutels en referenties heeft er toegang toe hebben. Aanvallers kunnen profiteren van automatische en handmatige technieken om sleutels en geheimen die zijn opgeslagen in broncodeopslagruimte zoals GitHub te vinden. Geen sleutels en geheimen plaats in deze openbare opslagplaatsen voor codes of op een andere server.

Plaats uw sleutels, certificaten, geheimen en verbindingsreeksen altijd in een oplossing voor sleutelbeheer. U kunt een gecentraliseerde oplossing waarin sleutels en geheimen worden opgeslagen in hardware security modules (HSM's) gebruiken. Azure voorziet u van een HSM in de cloud met [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis).

Key Vault is een *geheime store*: Er is een gecentraliseerde cloudservice voor het opslaan van toepassingsgeheimen. Key Vault uw vertrouwelijke gegevens houdt veilig door toepassingsgeheimen bewaren in één centrale locatie en beveiligde toegang, machtigingenbeheer en -logboekregistratie te leveren.

Geheimen worden opgeslagen in afzonderlijke *kluizen*. Elke kluis heeft een eigen configuratie- en beveiligingsbeleid om toegang te beheren. U krijgt tot uw gegevens via een REST-API of een client-SDK die beschikbaar is voor de meeste moderne programmeertalen.

> [!IMPORTANT]
> Azure Key Vault is ontworpen voor het opslaan van configuratie-geheimen voor servertoepassingen. Het niet bedoeld voor het opslaan van gegevens die deel uitmaakt van app-gebruikers. Dit wordt weergegeven in de prestatiekenmerken, API en kostenmodel.
>
> Gebruikersgegevens moeten elders, worden opgeslagen in een Azure SQL Database-exemplaar waarvoor transparante gegevensversleuteling (TDE) of in een opslagaccount die gebruikmaakt van Azure Storage-Serviceversleuteling. Geheimen die door uw toepassing worden gebruikt voor toegang tot deze gegevensarchieven kunnen worden bewaard in Azure Key Vault.

### <a name="protect-sensitive-data"></a>Gevoelige gegevens beveiligen

Beveiligen van gegevens is een essentieel onderdeel van uw beveiligingsstrategie.
Classificatie van uw gegevens en het identificeren van de beveiliging van uw gegevens moet helpt u bij het ontwerpen van uw app met beveiliging van gegevens in gedachten. Gegevens classificeren (categoriseren) worden opgeslagen door gevoeligheid en impact op bedrijf helpt ontwikkelaars bepalen van de risico's die gekoppeld aan gegevens zijn.

Alle toepasselijke gegevens labelen als gevoelig bij het ontwerpen van de opmaak van uw gegevens. Zorg ervoor dat de toepassing de gegevens van toepassing als gevoelig beschouwd. Deze procedures kunt u uw gevoelige gegevens te beschermen:

- Versleuteling gebruiken.
- Vermijd codeert geheimen zoals sleutels en wachtwoorden.
- Zorg ervoor dat besturingselementen voor toegang en controle voldaan is.

#### <a name="use-encryption"></a>Versleuteling gebruiken

Beveiligen van gegevens, moet een essentieel onderdeel van uw beveiligingsstrategie.
Als uw gegevens worden opgeslagen in een database of als heen en weer tussen locaties verplaatst, gebruikt u versleuteling van [data-at-rest](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest) (Hoewel in de database) en versleuteling van [gegevens die onderweg zijn](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices#protect-data-in-transit) (onderweg naar en van de gebruiker de database, een API of service-eindpunt). U wordt aangeraden dat u altijd SSL/TLS-protocollen gebruikt voor het uitwisselen van gegevens. Zorg dat u de nieuwste versie van TLS voor versleuteling gebruikt (op dit moment is dit versie 1.2).

#### <a name="avoid-hard-coding"></a>Codeert voorkomen

Wat u mag nooit hardgecodeerd in de software. Enkele voorbeelden zijn hostnamen of IP-adressen, URL's, e-mailadressen, gebruikersnamen, wachtwoorden, sleutels voor het opslagaccount en andere cryptografische sleutels. Overweeg de implementatie van eisen op aan wat kan of kunnen niet worden vastgelegd in uw code, inclusief in de secties Opmerking van uw code.

Als u opmerkingen in uw code plaatsen, zorg ervoor dat u gevoelige informatie niet opslaan. Dit omvat de e-mailadres, wachtwoorden, verbindingsreeksen, informatie over uw toepassing zou alleen bekend zijn door iemand in uw organisatie en iets anders dat een aanvaller een voordeel in uw toepassing of de organisatie is een aanval mogelijk geven .

In principe wordt ervan uitgegaan dat alles in uw project voor de ontwikkeling openbare kennis wordt worden als deze is geïmplementeerd. Vermijd het gebruik van gevoelige gegevens van welke aard dan ook in het project.

Eerder besproken [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). U kunt Key Vault gebruiken voor het opslaan van geheimen zoals sleutels en wachtwoorden in plaats van hard-coding ze. Wanneer u Key Vault in combinatie met beheerde identiteiten voor Azure-resources gebruiken, uw Azure-web-app toegang tot geheime configuratiewaarden eenvoudig en veilig zonder dat er geheimen opslaan in uw broncodebeheer of configuratie. Zie voor meer informatie, [geheimen in uw server-apps met Azure Key Vault beheren](https://docs.microsoft.com/learn/modules/manage-secrets-with-azure-key-vault/).

### <a name="implement-fail-safe-measures"></a>Failsafe maatregelen

Uw toepassing moet overweg kan met [fouten](https://docs.microsoft.com/dotnet/standard/exceptions/) die optreden tijdens het uitvoeren op een consistente manier. De toepassing moet catch alle fouten en een failover veilige of gesloten.

U moet er ook voor zorgen dat er fouten worden vastgelegd met voldoende context van de gebruiker voor het identificeren van verdachte of schadelijke activiteiten. Logboeken worden bewaard voor een voldoende tijd voor het toestaan van vertraagde forensische analyse. Logboeken moeten zich in een indeling die eenvoudig kan worden gebruikt door een oplossing logboekbeheer. Zorg ervoor dat waarschuwingen voor fouten die betrekking op security hebben worden geactiveerd. Onvoldoende logboekregistratie en bewaking kan aanvallers verder aanvallen op systemen en onderhouden van persistentie.

### <a name="take-advantage-of-error-and-exception-handling"></a>Profiteer van de fout en de afhandeling van uitzonderingen

Implementatie van de fout en [afhandeling van uitzonderingen](https://docs.microsoft.com/dotnet/standard/exceptions/best-practices-for-exceptions) is een belangrijk onderdeel van het coderen van verdediging. Fout- en afhandeling van uitzonderingen zijn essentieel voor het maken van een systeem, betrouwbaar en veilig. Fouten in foutafhandeling kunnen leiden tot verschillende soorten beveiligingsproblemen, zoals het lekken van gegevens voor aanvallen en helpt meer informatie over het platform en het ontwerp aanvallers.

Zorg ervoor dat:

- Voor het verwerken van de uitzonderingen in een gecentraliseerde manier om te voorkomen dat dubbele [try/catch blokken](https://docs.microsoft.com/dotnet/standard/exceptions/how-to-use-the-try-catch-block-to-catch-exceptions) in de code.

- Alle onverwachte gebeurtenissen worden verwerkt in de toepassing.

- Berichten die worden weergegeven aan gebruikers geen kritieke gegevens lekken, maar beschikken over voldoende informatie voor een uitleg van het probleem.

- Uitzonderingen worden vastgelegd en die ze leveren voldoende informatie voor forensische of incident response teams om te onderzoeken.

[Met Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) biedt een eersteklas ervaring voor [afhandeling van fouten en uitzonderingen](https://docs.microsoft.com/azure/logic-apps/logic-apps-exception-handling) die worden veroorzaakt door afhankelijke systemen. Logic Apps kunt u werkstromen maken om te automatiseren van taken en processen die kunnen worden geïntegreerd apps, gegevens, systemen en services voor bedrijven en organisaties.

### <a name="use-logging-and-alerting"></a>Gebruik logboekregistratie en waarschuwingen

[Logboek](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1) uw beveiligingsproblemen voor onderzoek, beveiliging en trigger waarschuwingen over problemen om ervoor te zorgen dat gebruikers weten over problemen tijdig. Inschakelen van controle en logboekregistratie op alle onderdelen. De logboeken voor controle moeten gebruikerscontext vastleggen en identificeren van alle belangrijke gebeurtenissen.

Controleer of u geen gevoelige gegevens die is ingediend door een gebruiker zich bij uw site. Voorbeelden van gevoelige gegevens zijn:

- De referenties van gebruiker
- Sociaal-fiscale nummers bevatten of andere identificerende gegevens
- Creditcardnummers of andere financiële gegevens
- Gegevens over de servicestatus
- Persoonlijke sleutels of andere gegevens die kunnen worden gebruikt om versleutelde gegevens te ontsleutelen
- Systeem- of gegevens die kan worden gebruikt voor de toepassing effectiever aanvallen

Zorg ervoor dat de toepassing gebruiker management gebeurtenissen, zoals geslaagde en mislukte aanmeldingen, wachtwoord opnieuw instellen van wachtwoorden, wijzigen van wachtwoorden, accountvergrendeling en gebruikersregistratie bewaakt. Logboekregistratie voor deze gebeurtenissen kunt u detecteren en reageren op mogelijk verdacht gedrag. U kunt er ook voor het verzamelen van operationele gegevens, zoals wie de toepassing.

## <a name="next-steps"></a>Volgende stappen
In de volgende artikelen, is het raadzaam beveiligingsmaatregelen en activiteiten waarmee u kunnen ontwikkelen en implementeren van veilige toepassingen.

- [Beveiligde toepassingen ontwikkelen](secure-develop.md)
- [Beveiligde toepassingen implementeren](secure-deploy.md)
