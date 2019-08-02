---
title: Veelgestelde vragen over de analyse van beveiligings code-documentatie Microsoft Azure
description: Dit artikel bevat veelgestelde vragen over de uitbrei ding voor de analyse van beveiligings code
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 1fc5e83c2c46a7da2a4b56879a2d596405fd8bee
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718312"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen
Vragen? Bekijk de veelgestelde vragen hieronder voor meer informatie.

## <a name="general-faqs"></a>Algemene veelgestelde vragen

### <a name="can-i-install-the-extension-on-my-tfs-not-azure-devops-server"></a>Kan ik de uitbrei ding installeren op mijn TFS-server (niet Azure DevOps)? 

Nee, de uitbrei ding is niet beschikbaar voor downloaden en installeren voor TFS.

### <a name="do-i-have-to-run-microsoft-security-code-analysis-with-my-build"></a>Moet ik de analyse van micro soft-beveiligings code uitvoeren met mijn build? 

Ja en Nee. Afhankelijk van het type analyse programma, is de bron code zelf mogelijk het enige wat vereist is, of is de uitvoer van de build mogelijk vereist. Omdat de referentie scanner bijvoorbeeld bestanden in de mappen structuur van de code opslagplaats analyseert, kunt u de referentie scanner uitvoeren en logboeken voor beveiligings analyse publiceren om de resultaten op te halen in een zelfstandige build.
Voor andere hulpprogram ma's die post build-artefacten analyseren, zoals BinSkim, is de build eerst vereist.

### <a name="can-i-break-my-build-when-results-are-found"></a>Kan ik mijn build opdelen wanneer er resultaten worden gevonden? 
Ja, u kunt een build-kraken introduceren wanneer een hulp programma een probleem, een zoek actie, in het logboek bestand meldt. U hoeft alleen de taak maken na analyse toe te voegen en het selectie vakje in te vinken voor elk hulp programma waarvan u wilt dat de build wordt verbroken. U kunt ervoor kiezen om de build te verstoren wanneer er fouten, waarschuwingen en fouten worden gerapporteerd in de gebruikers interface van de taak na de analyse.

### <a name="how-are-the-command-line-arguments-different-in-azure-devops-than-they-are-in-the-standalone-desktop-tools"></a>Hoe verschillen de opdracht regel argumenten in azure DevOps dan in de zelfstandige bureau blad-hulpprogram ma's? 

De Azure DevOps-build-taken zijn in het algemeen direct wrappers rond de opdracht regel argumenten van de beveiligings hulpprogramma's. Alles wat u normaal gesp roken doorgeeft aan het hulp programma op de opdracht regel vanaf uw bureau blad, kunt u door geven aan de argumenten invoer van de taak bouwen.
Hier volgt een lijst met merken bare verschillen:
 - Het hulp programma wordt uitgevoerd vanuit de bronmap van de agent $ (build. SourcesDirectory) of% BUILD_SOURCESDIRECTORY%. Voorbeeld: C:\agent\_work\1\s 
 - Paden in de argumenten kunnen relatief zijn ten opzichte van de hoofdmap van de bronmap hierboven of absoluut door een on-premises agent uit te voeren met bekende implementatie locaties van lokale bronnen of met Azure DevOps build-variabelen.
 - De hulpprogram ma's geven automatisch een pad naar het uitvoer bestand of de map als er een uitvoerpad wordt verstrekt. het pad wordt verwijderd en vervangen door een pad naar onze bekende logboek locatie op de build-agent
 - Sommige extra opdracht regel parameters worden opgeschoond en verwijderd in sommige hulpprogram ma's, zoals het toevoegen of verwijderen van opties om ervoor te zorgen dat er geen GUI wordt gestart.

### <a name="can-i-run-a-build-task-for-example-credential-scanner-across-multiple-repositories-in-an-azure-devops-build"></a>Kan ik een build-taak (bijvoorbeeld referentie scanner) uitvoeren in meerdere opslag plaatsen in een Azure DevOps-build? 

Nee, het uitvoeren van de hulpprogram ma's voor beveiligde ontwikkel aars op meerdere opslag plaatsen in één pijp lijn wordt momenteel niet ondersteund.

###  <a name="the-output-file-i-specified-is-not-being-created--i-cant-find-the-output-file-i-specified"></a>Het opgegeven uitvoer bestand wordt niet gemaakt/ik kan het opgegeven uitvoer bestand niet vinden 

De build-taken maken momenteel gebruik van gebruikers invoer en werken de locatie bij van het uitvoer bestand dat is gegenereerd op een algemene locatie op de build-agent. Zie de volgende vragen voor meer informatie over deze locatie.

### <a name="where-are-the-output-files-generated-by-the-tools-saved"></a>Waar worden de uitvoer bestanden gegenereerd door de hulpprogram ma's die worden opgeslagen? 

De build-taken voegen automatisch uitvoer paden toe aan de volgende bekende locatie op de build-agent $ (agent.\_BuildDirectory) sdt\logs. Door standaard op deze locatie te worden gebruikt, kunnen we garanderen dat andere teams die logboeken voor code analyse genereren of deze gebruiken toegang hebben.

### <a name="can-i-queue-a-build-to-run-these-tasks-on-a-hosted-build-agent"></a>Kan ik een build in de wachtrij plaatsen om deze taken uit te voeren op een gehoste build-agent? 

Ja, alle taken en hulpprogram ma's in de uitbrei ding kunnen worden uitgevoerd op een gehoste build-agent.

>[!NOTE]
> De anti-malware-build-taak vereist een build-agent waarop Windows Defender is ingeschakeld. Dit is waar op ' gehoste VS2017 ' of later bouw agenten. (Deze wordt niet uitgevoerd op de verouderde VS2015-agent.) Hand tekeningen kunnen niet worden bijgewerkt voor deze agents, maar de hand tekening moet altijd relatief actueel zijn en moet korter zijn dan 3 uur oud.
>

### <a name="can-i-run-these-build-tasks-as-part-of-a-release-pipeline-as-opposed-to-a-build-pipeline"></a>Kan ik deze bouw taken uitvoeren als onderdeel van een release pijplijn (in plaats van een build-pijp lijn)? 
In de meeste gevallen moet u Ja. Taken die artefacten publiceren, worden echter niet ondersteund door de Azure-DevOps die moeten worden uitgevoerd vanuit de release pijplijnen: "De enige categorie taken die niet worden gebruikt voor het werken met de release, is de taak die artefacten publiceert. Dit komt omdat we vanaf nu geen ondersteuning voor het publiceren van artefacten in release hebben.
Hiermee wordt voor komen dat de taak ' beveiligings analyse logboeken publiceren ' wordt uitgevoerd vanuit een release pijplijn. Dit kan mislukken, met een beschrijving van het fout bericht.

### <a name="from-where-do-the-build-tasks-download-the-tools"></a>Van waar worden de hulpprogram ma's gedownload met de build-taken? 
Met de build tasks a) kunt u NuGet-pakketten downloaden voor de hulpprogram ma's van de volgende [Azure DevOps Package Management-feed](https://securitytools.pkgs.visualstudio.com/_packaging/SecureDevelopmentTools/nuget/v3/index.json) of met behulp van het knooppunt pakket beheer, dat vooraf moet worden geïnstalleerd in de build-agent (bijvoorbeeld: "NPM installeren tslint").

### <a name="what-effect-will-installing-the-extension-have-on-my-azure-devops-organization"></a>Welk effect heeft de installatie van de uitbrei ding op mijn Azure DevOps-organisatie? 

Bij de installatie worden de door de uitbrei ding geboden taken voor het maken van beveiliging beschikbaar voor gebruik door alle gebruikers in uw organisatie. Wanneer u een Azure-pijp lijn maakt of bewerkt, zijn deze taken beschikbaar om toe te voegen aan de verzamelings lijst voor het maken van taken. Anders is het installeren van de uitbrei ding in uw Azure DevOps-organisatie niet van invloed. Er worden geen account-of project instellingen of-pijp lijnen gewijzigd.

### <a name="will-installing-the-extension-modify-my-existing-azure-pipelines"></a>Wordt de uitbrei ding geïnstalleerd mijn bestaande Azure-pijp lijnen wijzigen? 

Nee. Door de uitbrei ding te installeren, worden de taken voor het maken van beveiliging beschikbaar om toe te voegen aan uw Azure-pijp lijnen. Gebruikers zijn nog steeds verplicht om Build-definities toe te voegen of bij te werken om de hulpprogram ma's in uw bouw proces te integreren.

## <a name="task-specific-faqs"></a>Specifieke veelgestelde vragen over taken

Veelgestelde vragen over het maken van taken worden vermeld in deze sectie.

### <a name="credential-scanner-faqs"></a>Veelgestelde vragen over referentie scanner

#### <a name="what-are-common-suppressions-scenarios-and-examples"></a>Wat zijn veelvoorkomende onderdrukkingen scenario's en voor beelden? 
Twee van de meest voorkomende waarschuwingen voor onderdrukking worden hieronder beschreven:
##### <a name="suppress-all-occurrences-of-a-given-secret-within-the-specified-path"></a>Alle instanties van een gegeven geheim in het opgegeven pad onderdrukken 
De hash-sleutel van het geheim van het uitvoer bestand van de referentie scanner is vereist, zoals wordt weer gegeven in het onderstaande voor beeld
   
        {
            "tool": "Credential Scanner",
            "suppressions": [
            {
                "hash": "CLgYxl2FcQE8XZgha9/UbKLTkJkUh3Vakkxh2CAdhtY=",
                "_justification": "Secret used by MSDN sample, it is fake."
            }
          ]
        }

>[!WARNING]
> De hash-sleutel wordt gegenereerd door een deel van de overeenkomende waarde of bestands inhoud. Elke bron code revisie kan de hash-sleutel wijzigen en de onderdrukkings regel uitschakelen. 

##### <a name="to-suppress-all-secrets-in-a-specified-file-or-to-suppress-the-secrets-file-itself"></a>Om alle geheimen in een opgegeven bestand te onderdrukken (of om het geheimen-bestand zelf te onderdrukken) 
De bestands expressie kan een bestands naam of een achtervoegsel gedeelte van het volledige bestandspad/naam zijn. Jokertekens worden niet ondersteund. 

**Voorbeeld** 

Te onderdrukken bestand: [InputPath] \src\JS\lib\angular.js 

Geldige onderdrukkings regels: 
- [InputPath] \src\JS\lib\angular.js--het bestand in het opgegeven pad onderdrukken
- \src\JS\lib\angular.js
- \JS\lib\angular.js
- \lib\angular.js
- hoek. js--alle bestanden met dezelfde naam onderdrukken

        {
            "tool": "Credential Scanner",
            "suppressions": [
            {
                "file": "\\files\\AdditonalSearcher.xml", 
                "_justification": "Additional CredScan searcher specific to my team"
            },
            {
                "file": "\\files\\unittest.pfx", 
                "_justification": "Legitimate UT certificate file with private key"
            }
          ]
        }      

>[!WARNING] 
> Alle toekomstige geheimen die worden toegevoegd aan het bestand worden ook automatisch onderdrukt. 

#### <a name="what-are-recommended-secrets-management-guidelines"></a>Wat zijn aanbevolen geheimen beheer richtlijnen? 
Hoewel het mogelijk is om op tijd vastgelegde geheime geheimen te detecteren en de Risico's te verhelpen, is het zelfs nog beter als u een geheimen zou kunnen voor komen dat een geheim wordt ingecheckt. In dit opzicht heeft micro soft CredScan-code analyse uitgebracht als onderdeel van de [micro soft DevLabse-extensie](https://marketplace.visualstudio.com/items?itemName=VSIDEDevOpsMSFT.ContinuousDeliveryToolsforVisualStudio) voor Visual Studio. In de vroege preview biedt ontwikkel aars een inline-ervaring voor het detecteren van potentiële geheimen in hun code, waardoor ze de mogelijkheid hebben om deze problemen in realtime op te lossen. Raadpleeg [deze](https://devblogs.microsoft.com/visualstudio/managing-secrets-securely-in-the-cloud/) blog voor meer informatie over het veilig beheren van geheimen in de Cloud. Hieronder vindt u enkele extra bronnen om u te helpen bij het beheren van geheimen en op een veilige manier toegang tot gevoelige gegevens in uw toepassingen: 
 - [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
 - [Azure Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication)
 - [Azure AD-Managed Service Identity](https://azure.microsoft.com/blog/keep-credentials-out-of-code-introducing-azure-ad-managed-service-identity/)
 - [Managed Service Identity (MSI) voor Azure-resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
 - [Azure Managed Service Identity](https://docs.microsoft.com/azure/app-service/overview-managed-identity)
 - [AppAuthentication-bibliotheek](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication)

#### <a name="can-i-write-my-own-custom-searchers"></a>Kan ik mijn eigen aangepaste zoekers schrijven?

Referentie scanner is afhankelijk van een set inhouds zoekacties die meestal zijn gedefinieerd in het bestand **buildsearchers. XML** . Het bestand bevat een matrix met XML-geserialiseerde objecten die een ContentSearcher-object vertegenwoordigen. Het programma wordt gedistribueerd met een set zoek functies die goed zijn getest, maar waarmee u ook uw eigen aangepaste zoek functies kunt implementeren. 

Een inhouds zoekmachine wordt als volgt gedefinieerd: 

- **Naam** : de beschrijvende naam van de zoek functie die moet worden gebruikt in het uitvoer bestand van de referentie scanner. Het is raadzaam om Camel Case-naamgevings regels te gebruiken voor namen van zoekers. 
- **RuleId** : de stabiele dekkende id van de zoek functie. 
    - Standaard zoekmachines voor referentie scanners worden toegewezen met RuleIds zoals CSCAN0010, CSCAN0020, CSCAN0030, enzovoort. Het laatste cijfer is gereserveerd voor mogelijke zoek functie regex groep samen voegen of delen.
    - RuleId voor aangepaste zoek functies moeten een eigen naam ruimte hebben met de volgende indeling: CSCAN-{namespace} 0010, CSCAN-{namespace} 0020, CSCAN-{namespace} 0030, enzovoort.
    - De volledig gekwalificeerde naam van de zoek functie is de combi natie van de RuleId en de naam van de zoek functie. Voor beeld van CSCAN0010. KeyStoreFiles, CSCAN0020. Base64EncodedCertificate, etc.
- **ResourceMatchPattern** : de regex van bestands extensies die moeten worden gecontroleerd met de zoek functie
- **ContentSearchPatterns** : een matrix van teken reeksen met regex-instructies die moeten overeenkomen. Als er geen Zoek patronen zijn gedefinieerd, worden alle bestanden geretourneerd die overeenkomen met het resource match-patroon.
- **ContentSearchFilters** : matrix van teken reeksen die regex-instructies bevatten om zoek functie-specifieke fout-positieven te filteren.
- **MatchDetails** : een beschrijvend bericht en/of beperkings instructies die moeten worden toegevoegd voor elk treffer van de zoek functie.
- **Aanbeveling** : voorziet in de inhoud van het veld suggesties voor een overeenkomst met de PREfast Report-indeling.
- **Ernst** : een geheel getal dat de ernst van het probleem weerspiegelt (hoogste = 1).
![Referentie scanner instellen](./media/security-tools/6-credscan-customsearchers.png)

### <a name="roslyn-analyzers-faqs"></a>Veelgestelde vragen over Roslyn-analyse

#### <a name="what-are-the-most-common-errors-when-using-the-roslyn-analyzers-task"></a>Wat zijn de meest voorkomende fouten bij het gebruik van de Roslyn-analyse taken?

**Fout: Het project is hersteld met behulp van micro soft. NetCore. app versie x. x. x, maar met de huidige instellingen versie y. y. y wordt in plaats daarvan gebruikt. Om dit probleem op te lossen, moet u ervoor zorgen dat dezelfde instellingen worden gebruikt voor herstel en voor volgende bewerkingen, zoals bouwen of publiceren. Dit probleem kan zich doorgaans voordoen als de eigenschap RuntimeIdentifier is ingesteld tijdens het maken of publiceren, maar niet tijdens het herstellen:**

Roslyn-analyse functies worden uitgevoerd als onderdeel van compilatie. Daarom moet de bron structuur op de build-machine een samenstelbaar status hebben. Een stap (waarschijnlijk ' dotnet. exe Publish ') tussen uw belangrijkste build-en Roslyn-analyse functies heeft mogelijk de bron structuur in een niet-samenstel bare staat geplaatst. Als u de stap die een Nuget herstelt, net vóór de Roslyn Analyseers stap dupliceert, plaatst de bron structuur mogelijk terug in een samenstelbaar stadium.

**CSC. exe is afgesloten met fout code 1--een exemplaar van Analyzer AAAA kan niet worden gemaakt op basis van C:\BBBB.dll: Kan bestand of Assembly ' micro soft. CodeAnalysis, version = X. X. X. X, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' of een van de bijbehorende afhankelijkheden niet laden. Het systeem kan het opgegeven bestand niet vinden.**

Zorg ervoor dat uw compiler Roslyn-analyse functies ondersteunt. ' CSC. exe/version ' moet ten minste v 2.6. x rapporteren. In sommige gevallen kunnen afzonderlijke. csproj-bestanden de installatie van de Visual Studio voor het bouwen van de machine overschrijven door te verwijzen naar een pakket van Microsoft.Net. compilers. Als het gebruik van een specifieke versie van de compiler onbedoeld was, verwijdert u verwijzingen naar Microsoft.Net. compilers. Zorg er anders voor dat het pakket waarnaar wordt verwezen ook ten minste v 2.6. x is. Probeer het fouten logboek op te halen, dat u kunt vinden in de para meter/errorlog: van de opdracht regel CSC. exe (gevonden in het logboek van de Roslyn-build). Dit kan er ongeveer als volgt uitzien:/errorlog\_: f:\ts-Services-123 work\456\s\Some\Project\Code\Code.csproj.Sarif

**De C# compiler is niet recent genoeg (moet > = 2,6)**

De nieuwste versies van de C# compiler worden hier uitgebracht: https://www.nuget.org/packages/Microsoft.Net.Compilers. Als u de geïnstalleerde versie wilt downloaden, gebruikt `C:\>csc.exe /version` u vanaf de opdracht prompt. Zorg ervoor dat u geen verwijzing hebt naar een Microsoft.Net. compilers NuGet-pakket dat < v 2.6 is.

**MSBuild/VSBuild-logboeken niet gevonden**

Vanwege de werking van de taak moet deze taak een query uitvoeren op Azure DevOps voor het MSBuild-logboek van de MSBuild build-taak. Als deze taak onmiddellijk na de MSBuild-build-taak wordt uitgevoerd, is het logboek nog niet beschikbaar. Plaats andere build-taken, waaronder SecDevTools-build-taken, zoals Binskim, antimalware scan en andere), tussen de MSBuild-build-taak en de taak analyse functies van Roslyn. 

## <a name="next-steps"></a>Volgende stappen

Als u aanvullende hulp nodig hebt, is ondersteuning van micro soft-beveiligings code-analyse beschikbaar vanaf 9:00 uur 's nachts (standaard tijd 5:00)

  - Onboarding: Neem contact op met uw technische account beheerders om aan de slag te gaan. 
  >[!NOTE] 
  >Als u nog geen betaalde ondersteunings relatie met micro soft hebt of als u een ondersteunings aanbieding hebt waarmee u geen services kunt kopen vanuit de Phoenix-catalogus, gaat u naar de [Start pagina](https://www.microsoft.com/enterprise/services/support) van de ondersteunings services voor meer informatie.

  - Ondersteuning: Stuur een E-mail naar ons team bij de [micro soft security code Analysis-ondersteuning](mailto:mscahelp@microsoft.com?Subject=Microsoft%20Security%20Code%20Analysis%20Support%20Request)