---
title: Ontwikkelen van veilige toepassingen op Microsoft Azure
description: In dit artikel wordt beschreven aanbevolen procedures overwegen tijdens de fasen van de implementatie en controle van uw web application-project.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/12/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: bcd66d1a8077b4cc87c184f34b43cc5846a83f2f
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144423"
---
# <a name="develop-secure-applications-on-azure"></a>Ontwikkelen van veilige toepassingen op Azure
In dit artikel geven we beveiligingsactiviteiten en besturingselementen om te overwegen bij het ontwikkelen van toepassingen voor de cloud. Beveiligingsvragen en -concepten te houden tijdens de fasen van de implementatie en controle van de Microsoft [Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) vallen. Het doel is voor het definiëren van activiteiten en Azure-services die u gebruiken kunt voor het ontwikkelen van een beter beveiligde toepassing.

De volgende fasen van de SDL worden in dit artikel besproken:

- Implementatie
- Verificatie

## <a name="implementation"></a>Implementatie
De focus van de implementatiefase is tot stand brengen van aanbevolen procedures voor vroege voorkomen en detecteren en verwijderen van beveiligingsproblemen op het gebied van de code.
Wordt ervan uitgegaan dat uw toepassing wordt gebruikt op een manier die u niet van plan bent om te worden gebruikt. Zo kunt u bescherming tegen per ongeluk of opzettelijk misbruik van uw toepassing.

### <a name="perform-code-reviews"></a>Codebeoordelingen uitvoeren

Voordat u code incheckt, voeren [code, revisies](https://docs.microsoft.com/azure/devops/learn/devops-at-microsoft/code-reviews-not-primarily-finding-bugs) voor betere algehele kwaliteit van code en vermindert het risico van het maken van bugs. U kunt [Visual Studio](https://docs.microsoft.com/azure/devops/repos/tfvc/get-code-reviewed-vs?view=vsts) voor het beheren van het proces voor de code.

### <a name="perform-static-code-analysis"></a>De statische codeanalyse analyses uitvoeren

[Analyse van de statische codeanalyse](https://www.owasp.org/index.php/Static_Code_Analysis) (ook wel bekend als *source code analysis*) wordt gewoonlijk uitgevoerd als onderdeel van een beoordeling code. Analyse van de statische codeanalyse vaak verwijst naar het uitvoeren van de statische codeanalyse analyseprogramma's om mogelijke beveiligingsproblemen in de code niet worden uitgevoerd met behulp van technieken zoals [taint controleren](https://en.wikipedia.org/wiki/Taint_checking) en [gegevensstroom analysis](https://en.wikipedia.org/wiki/Data-flow_analysis).

Azure Marketplace-aanbiedingen [hulpprogramma's voor ontwikkelaars](https://azuremarketplace.microsoft.com/marketplace/apps/category/developer-tools?page=1&search=code%20review) die statische code analyses uitvoeren en hulp met codebeoordelingen.

### <a name="validate-and-sanitize-every-input-for-your-application"></a>Valideren en opschonen van elke invoer voor uw toepassing

Alle invoer behandelen als niet-vertrouwde ter bescherming van uw toepassing van de meest voorkomende internetkwetsbaarheden van toepassing. Niet-vertrouwde gegevens is een voertuig voor injectieaanvallen. Invoer voor uw toepassing met parameters die in de URL invoeren van de gebruiker gegevens uit de database of uit een API, en alles dat wordt doorgegeven in die een gebruiker mogelijk kan bewerken. Een toepassing moet [valideren](https://www.owasp.org/index.php/OWASP_Proactive_Controls_2016#4:_Validate_All_Inputs) dat gegevens syntactisch en semantisch geldig is voordat de toepassing gebruikmaakt van de gegevens op een manier (met inbegrip van deze terug naar de gebruiker wordt weergegeven).

Valideren van invoer vroeg in de gegevensstroom om ervoor te zorgen dat alleen juist opgemaakte gegevens de werkstroom voert. U wilt niet dat onjuist gevormde gegevens persistent maken in uw database of activeren van een storing in een downstream-onderdeel.

Zijn twee algemene methoden voor het valideren van de syntaxis van de invoer blacklisting en opname in de whitelist:

  - Witte probeert om te controleren dat de invoer van een bepaalde gebruiker geen 'bekende schadelijke worden' inhoud bevat.

  - Opname in de Whitelist probeert om te controleren of de invoer van een bepaalde gebruiker overeenkomt met een set 'bekende goede' invoer. Op basis van een teken in een whitelist opnemen is een vorm van opname in de whitelist waarin een toepassing wordt gecontroleerd dat invoer van de gebruiker alleen 'bekende goede' tekens bevat of dat invoer komt overeen met een bekende indeling.
    Bijvoorbeeld, kan dit inhouden controleren dat een gebruikersnaam alleen alfanumerieke tekens bevat of dat deze precies twee getallen bevat.

Opname in de Whitelist is de aanpak van voorkeur voor het bouwen van veilige software.
Witte is foutgevoelig, omdat het is niet mogelijk om na te denken van een volledige lijst met mogelijk ongeldige invoer.

Dit werk op de server, niet op de client (of op de server en op de client).

### <a name="verify-your-applications-outputs"></a>Controleer of de uitvoer van uw toepassing

Geen uitvoer die u visueel of in een document moet altijd worden gecodeerd en escape. [Aanhalingstekens](https://www.owasp.org/index.php/Injection_Theory#Escaping_.28aka_Output_Encoding.29), ook wel bekend als *uitvoercodering*, om ervoor te zorgen dat niet-vertrouwde gegevens een voertuig voor een injectie-aanval niet wordt gebruikt. Aanhalingstekens, gecombineerd met gegevensvalidatie, biedt gelaagde beveiliging voor betere beveiliging van het systeem als geheel.

Aanhalingstekens is ervoor dat alles weergegeven als *uitvoer.* Aanhalingstekens, kunt ook de interpreter weet dat de gegevens is niet bedoeld om te worden uitgevoerd en zo worden aanvallen werken voorkomen. Dit is een andere veelvoorkomende aanval techniek met de naam *cross-site scripting* (XSS).

Als u een webframework van een derde partij gebruikt, kunt u controleren of uw opties voor het coderen van uitvoer op websites met behulp van de [OWASP XSS preventie overzichtskaart](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md).

### <a name="use-parameterized-queries-when-you-contact-the-database"></a>Geparameteriseerde query's gebruiken wanneer u contact opnemen met de database

Maak nooit een inline-databasequery 'op elk gewenst moment"in uw code en verzend het rechtstreeks naar de database. Schadelijke code ingevoegd in uw toepassing zou kunnen veroorzaken voor uw database worden gestolen, gewist of gewijzigd. Uw toepassing kan ook worden gebruikt om schadelijke besturingssysteem-opdrachten uitvoeren op het besturingssysteem dat als host fungeert voor uw database.

Gebruik in plaats daarvan geparameteriseerde query's of opgeslagen procedures. Wanneer u geparameteriseerde query's gebruikt, kunt u de procedure veilig aanroepen vanuit uw code en geven deze een tekenreeks zonder u zorgen te maken dat dit wordt beschouwd als onderdeel van de query-instructie.

### <a name="remove-standard-server-headers"></a>Standard van SQL server-headers verwijderen

Kopteksten, zoals Server X-aangedreven-By, en X-AspNet-Version geven informatie over de server en de onderliggende technologie. Het is raadzaam dat u deze headers om te voorkomen dat de toepassing fingerprinting onderdrukken.
Zie [headers van de standaard-server op Azure websites verwijderen](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

### <a name="segregate-your-production-data"></a>Uw productiegegevens scheiden

Uw productiegegevens of 'echte' gegevens moeten niet worden gebruikt voor ontwikkeling, tests of andere doeleinden dan wat het bedrijf bestemd. Een gemaskeerd ([geanonimiseerd](https://en.wikipedia.org/wiki/Data_anonymization)) gegevensset moet worden gebruikt voor alle ontwikkeling en testen.

Dit betekent dat minder personen hebben toegang tot uw echte gegevens waardoor uw kwetsbaarheid voor aanvallen. Het betekent ook dat minder werknemers Zie persoonlijke gegevens, waarmee wordt voorkomen dat een mogelijk beveiligingsprobleem in vertrouwen te behandelen.

### <a name="implement-a-strong-password-policy"></a>Een sterk wachtwoordbeleid implementeren

Om te zetten voor bescherming tegen brute-force en op basis van een woordenlijst raden, moet u een sterk wachtwoordbeleid om ervoor te zorgen dat gebruikers een complex wachtwoord (bijvoorbeeld de minimale lengte 12 tekens en alfanumerieke tekens en speciale tekens vereisen) maken implementeren.

Een identiteit framework kunt u maken en wachtwoordbeleid afdwingen. Azure AD B2C helpt u bij het Wachtwoordbeheer door te geven [ingebouwde beleidsregels](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy), [Self-service voor wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-sspr), en nog veel meer.

Als u wilt beschermen tegen aanvallen op de standaard-accounts, Controleer of alle sleutels en wachtwoorden replaceable en ze zijn gegenereerd of vervangen na de installatie van resources.

Als de toepassing automatisch wachtwoorden genereren moet, zorgt u ervoor dat de gegenereerde wachtwoorden willekeurige zijn en of ze hoge entropie hebben.

### <a name="validate-file-uploads"></a>Valideren van het uploaden van bestanden

Als uw toepassing kunt [file uploads](https://www.owasp.org/index.php/Unrestricted_File_Upload), houd rekening met voorzorgsmaatregelen die u voor deze riskante activiteit kunt uitvoeren. De eerste stap in veel aanvallen is toegang te krijgen van bepaalde schadelijke code tot een systeem dat wordt aangevallen. Met behulp van een bestand is geüpload, kunt de aanvaller hiervoor. OWASP biedt oplossingen voor het valideren van een bestand om ervoor te zorgen dat het bestand dat u uploadt veilig is.

Bescherming tegen malware helpt te identificeren en verwijderen van virussen, spyware en andere schadelijke software. U kunt installeren [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) of oplossing voor eindpuntbeveiliging is een Microsoft-partner ([Trend Micro](https://www.trendmicro.com/azure/), [Symantec](https://www.symantec.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10), en [System Center Endpoint Protection](https://docs.microsoft.com/sccm/protect/deploy-use/endpoint-protection)).

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) bevat functies zoals realtime-beveiliging, geplande scannen, oplossen van malware, handtekeningupdates, engine-updates, rapportage-voorbeelden en uitsluitingsverzameling gebeurtenis. U kunt oplossingen van Microsoft Antimalware en partners met integreren [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) voor een eenvoudige implementatie en ingebouwde detecties (waarschuwingen en incidenten).

### <a name="dont-cache-sensitive-content"></a>Geen gevoelige inhoud in cache

Geen cache gevoelige inhoud in de browser. Browsers kunnen opslaan voor caching en geschiedenis. Bestanden in de cache worden opgeslagen in een map, zoals de map Tijdelijke internetbestanden in het geval van Internet Explorer. Wanneer deze pagina's worden aangeduid, de browser, wordt de pagina's uit de cache weergegeven. Als gevoelige gegevens (adres, creditcardgegevens, burgerservicenummer, gebruikersnaam) worden weergegeven aan de gebruiker, de informatie kan worden opgeslagen in de cache van de browser en meer worden opgehaald door te controleren van de cache van de browser of door op de browser tedrukken **Terug** knop.

## <a name="verification"></a>Verificatie
De verificatie-fase omvat een uitgebreide inspanning om ervoor te zorgen dat de code voldoet aan de basisprincipes voor beveiliging en privacy die zijn vastgelegd in de vorige fasen.

### <a name="find-and-fix-vulnerabilities-in-your-application-dependencies"></a>Zoek en corrigeer kwetsbaarheden in uw toepassingsafhankelijkheden

U scannen uw toepassing en de afhankelijke bibliotheken voor het identificeren van alle bekende kwetsbare onderdelen. Producten die beschikbaar zijn voor het uitvoeren van deze scan [OWASP-afhankelijkheden controleren](https://www.owasp.org/index.php/OWASP_Dependency_Check),[Snyk](https://snyk.io/), en [zwart Duck](https://www.blackducksoftware.com/).

Scannen op beveiligingsproblemen aangedreven door [Tinfoil Security](https://www.tinfoilsecurity.com/) is beschikbaar voor Azure App Service Web Apps. [Tinfoil Security scanning via App Service](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) biedt ontwikkelaars en beheerders snel, geïntegreerde en betaalbare middel om te detecteren en aanpakken van beveiligingsproblemen voordat een schadelijke actor van deze profiteren kan.

> [!NOTE]
> U kunt ook [Tinfoil Security integreren met Azure AD](https://docs.microsoft.com/azure/active-directory/saas-apps/tinfoil-security-tutorial). Tinfoil Security integreren met Azure AD biedt u de volgende voordelen:
>  - In Azure AD, kunt u bepalen wie toegang heeft tot Tinfoil Security.
>  - Uw gebruikers kunnen worden automatisch aangemeld bij Tinfoil Security (eenmalige aanmelding) met behulp van hun Azure AD-accounts.
>  - U kunt uw accounts op een centrale locatie, de Azure-portal beheren.

### <a name="test-your-application-in-an-operating-state"></a>Uw toepassing testen in een operationele status

Dynamische toepassing beveiligingstesten (DAST) is een proces van het testen van een toepassing in een operationele status voor het vinden van beveiligingsproblemen. Hulpprogramma's voor DAST analyseren programma's, terwijl ze worden uitgevoerd om te vinden van beveiligingsproblemen, zoals beschadiging van het geheugen, onveilig serverconfiguratie, cross-site scripting, gebruiker bevoegdheden problemen, SQL-injectie en andere kritieke beveiligingsproblemen.

DAST wijkt af van statische Toepassingsbeveiliging testen (SAST). Hulpprogramma's voor SAST broncode analyseren of versies van de code wordt gecompileerd wanneer de code wordt uitgevoerd om te zoeken beveiligingsfouten.

DAST, bij voorkeur uitgevoerd met de hulp van een medewerker van de beveiliging (een [indringingstests tester](https://docs.microsoft.com/azure/security/azure-security-pen-testing) of door een beveiligingslek assessor). Als een medewerker van de beveiliging niet beschikbaar is, kunt u uitvoeren DAST uzelf met een web proxy-scanner en training. Sluit een scanner DAST vroeg om ervoor te zorgen dat u voor de hand liggende beveiligingsproblemen geen in uw code introduceren. Zie de [OWASP](https://www.owasp.org/index.php/Category:Vulnerability_Scanning_Tools) site voor een lijst met scanners van beveiligingsproblemen voor web-toepassing.

### <a name="perform-fuzz-testing"></a>Fuzzy testen uit te voeren

In [fuzzy testen](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/), u programma fout veroorzaken door de introductie van opzettelijk onjuist gevormd of willekeurige gegevens naar een toepassing. Programma fout veroorzaken, kunt mogelijke beveiligingsproblemen te onthullen voordat de toepassing wordt uitgebracht.

[Detectie van beveiligingsrisico](https://docs.microsoft.com/security-risk-detection/) is de unieke fuzzy testen van de service voor het vinden van beveiligings-kritieke fouten in de software van Microsoft.

### <a name="conduct-attack-surface-review"></a>Aanval surface evaluatie uitvoeren

Controleren van de kwetsbaarheid voor aanvallen na codevoltooiing ervoor zorgt dat elk ontwerp of implementatie van in een toepassing wijzigingen of systeem heeft genomen. Dit zorgt ervoor dat alle nieuwe aanvalsvectoren die zijn gemaakt als gevolg van de wijzigingen, met inbegrip van bedreigingsmodellen, zijn bekeken en verholpen.

U kunt een afbeelding van de kwetsbaarheid voor aanvallen maken met het scannen van de toepassing. Microsoft biedt een aanval analysehulpprogramma met de naam [aanval Surface Analyzer](https://www.microsoft.com/download/details.aspx?id=24487). U kunt kiezen uit veel commerciële dynamische testen en hulpprogramma's of services, met inbegrip van scannen van beveiligingsproblemen [OWASP schrijving aanval Proxy Project](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project), [Arachni](http://arachni-scanner.com/), [Skipfish](http://code.google.com/p/skipfish/), en [w3af](http://w3af.sourceforge.net/). Deze hulpprogramma's voor scannen verkennen van uw app en toewijzen van de onderdelen van de toepassing die toegankelijk via het web zijn. U kunt ook zoeken naar de Azure Marketplace voor vergelijkbare [hulpprogramma's voor ontwikkelaars](https://azuremarketplace.microsoft.com/marketplace/apps/category/developer-tools?page=1).

### <a name="perform-security-penetration-testing"></a>Indringingstests beveiliging uitvoeren

Ervoor te zorgen dat uw toepassing beveiligd is is net zo belangrijk als alle andere functionaliteit testen. Controleer [indringingstests](https://docs.microsoft.com/azure/security/azure-security-pen-testing) standaard deel uitmaken van de build- en implementatieproces. Plannen van regelmatige tests en scannen op geïmplementeerde toepassingen van beveiligingsproblemen en controleren voor open poorten, eindpunten en aanvallen.

### <a name="run-security-verification-tests"></a>Voer tests uit security-verificatie

[Secure DevOps Kit voor Azure](https://azsk.azurewebsites.net/index.html) (AzSK) bevat SVTs voor meerdere services van het Azure-platform. U uitvoeren deze SVTs periodiek om ervoor te zorgen dat uw Azure-abonnement en de verschillende bronnen die deel uitmaken van uw toepassing zich in een beveiligde status. U kunt ook deze tests automatiseren met behulp van de continue integratie/continue implementatie (CI/CD)-extensies functie van AzSK, waardoor SVTs beschikbaar als een uitbreiding van Visual Studio.

## <a name="next-steps"></a>Volgende stappen
In de volgende artikelen, is het raadzaam beveiligingsmaatregelen en activiteiten waarmee u kunnen ontwerpen en implementeren van veilige toepassingen.

- [Beveiligde toepassingen ontwerpen](secure-design.md)
- [Beveiligde toepassingen implementeren](secure-deploy.md)
