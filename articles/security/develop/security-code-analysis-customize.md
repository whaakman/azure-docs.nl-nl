---
title: Aanpassings handleiding voor de analyse van beveiligings code Microsoft Azure
description: Dit artikel bevat informatie over het aanpassen van de taken in de uitbrei ding voor de analyse van beveiligings code
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
ms.openlocfilehash: ab219b71eb8cd6f6172b7d02a639301c67811b49
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718351"
---
# <a name="how-to-configure--customize-the-build-tasks"></a>Procedure: De bouw taken configureren & aanpassen

Op deze pagina worden de configuratie opties beschreven die beschikbaar zijn in elk van de build-taken, te beginnen met de taken voor hulpprogram ma's voor het analyseren van beveiligings code, gevolgd door de verwerkings taken

## <a name="anti-malware-scanner-task"></a>Anti-Malware-Scanner taak

> [!NOTE]
> De anti-malware-build-taak vereist een build-agent waarop Windows Defender is ingeschakeld. Dit is waar op ' gehoste VS2017 ' en latere agents. (Deze wordt niet uitgevoerd op de verouderde VS2015-agent) Hand tekeningen kunnen niet worden bijgewerkt voor deze agents, maar de hand tekening moet altijd relatief actueel zijn en moet korter zijn dan 3 uur oud.

Scherm afbeelding en Details over het configureren hieronder.

![De taak voor het maken van een anti-malware scanner aanpassen](./media/security-tools/5-add-anti-malware-task600.png) 

- Instellingen voor type = **basis**
- Met type = **Custom**kunnen opdracht regel argumenten worden gegeven om de scan aan te passen.

Windows Defender gebruikt de Windows Update-client om hand tekeningen te downloaden en te installeren. Als de handtekening update mislukt in uw build-agent, komt de HRESULT-fout code waarschijnlijk van Windows Update. Ga voor meer informatie over Windows Update fouten en-beperking naar [Deze pagina](https://docs.microsoft.com/windows/deployment/update/windows-update-error-reference) en deze [TechNet-pagina](https://social.technet.microsoft.com/wiki/contents/articles/15260.windows-update-agent-error-codes.aspx)

## <a name="binskim-task"></a>BinSkim-taak

> [!NOTE]
> Als een vereiste om de BinSkim-taak uit te voeren, moet uw build voldoen aan een van de onderstaande voor waarden.
>    - Uw build produceert binaire artefacten van beheerde code.
>    - U hebt binaire artefacten die u wilt analyseren met BinSkim.

Scherm afbeelding en Details over het configureren hieronder. 

![BinSkim-installatie](./media/security-tools/7-binskim-task-details.png)  
1. Stel de configuratie voor het compileren in op debug voor het opsporen van bestanden. Ze worden door BinSkim gebruikt voor het toewijzen van problemen die zijn gevonden in de binaire uitvoer naar de bron code. 
2. Kies type = **Basic** & Function = **analyseren** om te voor komen dat u uw eigen opdracht regel opnieuw zoekt en maakt. 
3. **Doel** -een of meer specificaties naar een bestand, map of filter patroon dat wordt omgezet in een of meer binaire bestanden die moeten worden geanalyseerd. 
    - Meerdere doelen moeten worden gescheiden door een **punt komma (;)** . 
    - Dit kan één bestand zijn of joker tekens bevatten.
    - Directory's moeten altijd eindigen op\*
    - Voorbeelden:

           *.dll;*.exe
           $(BUILD_STAGINGDIRECTORY)\*
           $(BUILD_STAGINGDIRECTORY)\*.dll;$(BUILD_STAGINGDIRECTORY)\*.exe;
4. Als u type = **opdracht regel**selecteert, 
     - Zorg ervoor dat het eerste argument voor **BinSkim. exe** de term **analyse** met volledige paden of paden ten opzichte van de bronmap is.
     - Voor de invoer van de **opdracht regel** moeten meerdere doelen worden gescheiden door een spatie.
     - U kunt de file-para meter **/o** of **/output** weglaten. deze wordt toegevoegd aan u of vervangen. 
     - **Standaard opdracht regel configuratie** 

           analyze $(Build.StagingDirectory)\* --recurse --verbose
           analyze *.dll *.exe --recurse --verbose
          > [!NOTE]
          > Het navolgen \* is zeer belang rijk bij het opgeven van een directory of directory's voor het doel. 

Voor meer informatie over BinSkim over opdracht regel argumenten, regels op ID of afsluit codes gaat u naar de [Gebruikers handleiding voor BinSkim](https://github.com/Microsoft/binskim/blob/master/docs/UserGuide.md)

## <a name="credential-scanner-task"></a>Taak referentie scanner
Scherm afbeelding en Details over het configureren hieronder.
 
![Aanpassing van referentie scanner](./media/security-tools/3-taskdetails.png)

Beschik bare opties zijn 
  - **Uitvoer indeling** – TSV/CSV/SARIF/PREfast
  - **Versie van hulp programma** Aanbevelingen Jongste
  - **Map scannen** : de map in uw opslag plaats die u wilt scannen
  - **Zoek functies bestands type** : opties voor het zoeken naar het Zoek bestand dat wordt gebruikt voor het scannen.
  - **Onderdrukkings bestand** : een [JSON](https://json.org/) -bestand kan worden gebruikt voor het onderdrukken van problemen in het uitvoer logboek (meer informatie in de sectie Resources). 
  - **Uitgebreide uitvoer** -automatische uitleg 
  - **Batch grootte** : het aantal gelijktijdige threads dat wordt gebruikt om referentie scanners parallel uit te voeren. De standaard instelling is 20 (de waarde moet tussen 1 en 2147483647 liggen).
  - **Time-out** : de hoeveelheid tijd die nodig is om een zoek opdracht uit te voeren voordat de controle wordt afgebroken. 
  - **Lees buffer grootte van bestands scan** bij het lezen van inhoud in bytes. (De standaard instelling is 524288) 
  - **Maximale bestands scan gelezen bytes** -maximum aantal bytes dat moet worden gelezen uit een bepaald bestand tijdens inhouds analyse. (De standaard instelling is 104857600) 
  - **Deze taak uitvoeren** (onder **controle opties**): Hiermee geeft u op wanneer de taak moet worden uitgevoerd. Kies aangepaste voor waarden om complexere voor waarden op te geven. 
  - **Versie** : taak versie samen stellen in azure DevOps. Niet vaak gebruikt. 

## <a name="microsoft-security-risk-detection-task"></a>Micro soft-taak risico detectie
> [!NOTE]
> U moet een account maken en configureren met de service voor risico detectie als vereiste om deze taak te kunnen gebruiken. Deze service vereist een afzonderlijk voorbereidings proces. het is niet ' Plug-and-Play ' als de meeste andere taken in deze uitbrei ding. Raadpleeg de [micro soft-beveiligings Risico's](https://aka.ms/msrddocs) en [detectie van micro soft-beveiligings Risico's: ](https://docs.microsoft.com/security-risk-detection/how-to/) Instructies.

Details over het configureren hieronder.

Voer de vereiste gegevens in. elke optie heeft Help voor de aanwijs tekst.
   - **Azure DevOps service-eindpunt naam voor MSRD**: Als u een Gene riek type Azure DevOps service-eind punt hebt gemaakt voor het opslaan van de URL van het MSRD-exemplaar (u hebt voor bereid op) en het toegangs token REST API, kunt u dat service-eind punt kiezen. Als dat niet het geval is, klikt u op de koppeling beheren om een nieuw service-eind punt voor deze MSRD-taak te maken en te configureren. 
   - **Account-id**: Het is een GUID die kan worden opgehaald uit de URL van het MSRD-account.
   - **Url's naar binaire bestanden**: Een door punt komma's gescheiden lijst met openbaar beschik bare Url's (voor het downloaden van de binaire bestanden door de fuzzy machine).
   - **Url's van de Seed-bestanden**: Een door punt komma's gescheiden lijst met openbaar beschik bare Url's (die worden gebruikt door de fuzzy machine om de zaden te downloaden). Dit veld is optioneel als de Seed-bestanden worden gedownload met de binaire bestanden.
   - **Type besturings systeem**: Het type besturings systeem (Windows of Linux) van machines voor het uitvoeren van de taak voor benadering op.
   - **Windows-editie/Linux-editie**: De versie van het besturings systeem van de computers waarop de taak voor het uitvoeren van de benadering moet worden uitgevoerd. U kunt de standaard waarde overschrijven als uw computers een andere versie van het besturings systeem hebben.
   - **Pakket installatie script**: Geef het script op dat moet worden uitgevoerd op een test computer om het test doel programma en de afhankelijkheden ervan te installeren voordat u de taak die het onduidelijk maakt, inzendt.
   - **Para meters voor taak verzending**:
       - **Seed-map**: Het pad naar de map op de computer met het zaden van de fuzzy.
       - **Seed-extensie**: De bestands extensie van de zaden
       - **Uitvoerbaar stuur programma testen**: Het pad naar het uitvoer bare doel bestand op de computer met fuzzy.
       - **Uitvoer bare architectuur van stuur programma testen**: De architectuur van het uitvoer bare bestand (x86 of amd64).
       - **Argumenten voor testen van Stuur Programma's**: De opdracht regel argumenten die zijn door gegeven aan het uitvoer bare test doel bestand. Houd er rekening mee dat het symbool "% testbestand", inclusief de dubbele aanhalings tekens, automatisch wordt vervangen door het volledige pad naar het doel bestand. het test stuur programma wordt naar verwachting geparseerd en is vereist.
       - Het testen van het **stuur programma wordt afgesloten wanneer de test is voltooid**: Controleren of het test stuur programma na voltooiing wordt beëindigd. Schakel dit selectie vakje uit als het test stuur programma geforceerd moet worden gesloten.
       - **Maximum duur (in seconden)** : Geef een schatting op van de langste redelijkerwijs verwachte tijd die is vereist voor het parseren van een invoer bestand door het doel programma. Hoe nauw keuriger deze schatting, hoe efficiënter de benadering van het uitvoeren van de informatie.
       - Het **test stuur programma kan herhaaldelijk worden uitgevoerd**: Controleer of het test stuur programma herhaaldelijk kan worden uitgevoerd zonder dat dit afhankelijk is van een persistente/gedeelde globale status.
       - De naam van het **test stuur programma kan worden gewijzigd**: Controleer of het uitvoer bare bestand van het test stuur programma kan worden hernoemd en goed werkt.
       - **De toepassing die u wilt uitvoeren, wordt uitgevoerd als één besturingssysteem proces**: Controleren of het test stuur programma wordt uitgevoerd onder één besturingssysteem proces; Controleer of het test stuur programma extra processen heeft gestart.


## <a name="roslyn-analyzers-task"></a>Roslyn-analyse taak
> [!NOTE]
> Als een vereiste om de Roslyn Analyzer-taak uit te voeren, moet uw build voldoen aan de volgende voor waarden.
>  - De build-definitie bevat de ingebouwde MSBuild-of VSBuild-build-taak C# voor het compileren van (of VB)-code. Deze taak is afhankelijk van de invoer en uitvoer van die specifieke build-taak voor het opnieuw uitvoeren van de MSBuild-compilatie met Roslyn-analyse functies ingeschakeld.
>  - De build-agent die deze build-taak uitvoert, heeft Visual Studio 2017 v 15.5 of later geïnstalleerd (compiler versie 2.6. x).
>

Details over het configureren hieronder.

Beschik bare opties zijn 
- **RuleSet** -sdl vereist, SDL wordt aanbevolen of u kunt een aangepaste ruleset van uw eigen toepassing gebruiken.
- **Versie van analyse** functies Aanbevelingen Jongste
- **Waarschuwingen van het compiler bestand** -een tekst bestand met een lijst met waarschuwings-id's die moeten worden onderdrukt. 
- **Deze taak uitvoeren** (onder **controle opties**): Hiermee geeft u op wanneer de taak moet worden uitgevoerd. Kies**aangepaste voor waarden**om complexere voor waarden op te geven. 

> [!NOTE]
> - Roslyn-analyse functies zijn door het compileren geïntegreerd en kunnen alleen worden uitgevoerd als onderdeel van de CSC. exe-compilatie. Deze taak vereist daarom opnieuw afspelen/opnieuw uitvoeren van de compiler opdracht die eerder in de build is uitgevoerd. Dit wordt gedaan door een query uit te voeren op VSTS voor de MSBuild build-taak Logboeken (er is geen andere manier om de opdracht regel voor MSBuild-compilatie op een betrouw bare wijze op te halen uit de build-definitie; er is een tekst vorm toegevoegd om gebruikers toe te staan hun opdracht regels op te geven , maar het is lastig om deze up-to-date te houden en te synchroniseren met de belangrijkste build. Aangepaste builds vereisen het opnieuw afspelen van de hele set opdrachten, niet alleen voor Compileer opdrachten en zijn niet trivial/reliable om in deze gevallen Roslyn-analyse functies in te scha kelen. 
> - Roslyn-analyse functies zijn geïntegreerd met de compiler. hiervoor moet de compilatie worden aangeroepen. Deze build-taak wordt geïmplementeerd door C# projecten die al zijn gemaakt met behulp van de functie MSBuild/VSBuild bouwen, te hercompileren in dezelfde Build/Build-definitie, maar in dit geval met de analyse functies ingeschakeld. Als deze build-taak wordt uitgevoerd op dezelfde agent als de oorspronkelijke build-taak, wordt de uitvoer van de oorspronkelijke MSBuild/VSBuild-build-taak overschreven in de map Sources door de uitvoer van deze build-taak. De build-uitvoer is hetzelfde, maar u wordt aangeraden MSBuild uit te voeren, uitvoer naar de map voor het maken van artefacten te kopiëren en vervolgens Roslyn uit te voeren.
>

Raadpleeg voor aanvullende bronnen voor Roslyn-analyse taken [Roslyn-analyseën op Microsoft docs](https://docs.microsoft.com/dotnet/standard/analyzers/)

Het analyse pakket dat door deze build-taak wordt geïnstalleerd en gebruikt, vindt u [hier](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers) 

## <a name="tslint-task"></a>TSLint-taak

Ga voor meer informatie over TSLint naar [TSLint github opslag plaats](https://github.com/palantir/tslint)
>[!NOTE] 
>Zoals u mogelijk weet, is TSLint enige tijd in 2019 (Bron: [TSLint github opslag plaats](https://github.com/palantir/tslint)) Het team onderzoekt momenteel [ESLint](https://github.com/eslint/eslint) als een alternatief en het maken van een nieuwe taak voor ESLint is in het schema.

## <a name="publish-security-analysis-logs-task"></a>Taak beveiligings analyse logboeken publiceren
Scherm afbeelding en Details over het configureren hieronder.

![Beveiligings analyse voor publicatie aanpassen](./media/security-tools/9-publish-security-analsis-logs600.png)  

- **Artefact naam** -kan een wille keurige teken reeks-id zijn
- **Type artefact** : u kunt Logboeken publiceren naar de Azure-DevOps-server of naar een bestands share die toegankelijk is voor de build-agent. 
- **Hulpprogram ma's** : u kunt ervoor kiezen om logboeken voor afzonderlijke/specifieke hulpprogram ma's te bewaren of **alle hulpprogram ma's** te selecteren om alle logboeken te behouden. 

## <a name="security-report-task"></a>Beveiligings rapport taak
Scherm afbeelding en Details over het configureren hieronder.  
![Setup na de analyse](./media/security-tools/4-createsecurityanalysisreport600.png) 
- **Rapporten** -Kies de te maken rapport bestanden; Er wordt één gemaakt in elke indelings **console**, **TSV**en/of **HTML** 
- **Hulp middelen** : Selecteer de hulpprogram ma's in de build-definitie waarvoor u een overzicht van de gedetecteerde problemen wilt zien. Voor elk geselecteerd hulp programma kunt u ook selecteren of u alleen fouten of zowel fouten als waarschuwingen in het rapport wilt weer geven. 
- **Geavanceerde opties** : u kunt ervoor kiezen om een waarschuwing of een fout te registreren (en de taak te laten mislukken) als er geen logboeken zijn voor een van de geselecteerde hulpprogram ma's.
U kunt de map basis logboeken aanpassen waarin de logboeken worden gevonden, maar dit is geen typisch scenario. 

## <a name="post-analysis-task"></a>Taak na de analyse
Scherm afbeelding en Details over het configureren hieronder.

![Post-analyse aanpassen](./media/security-tools/a-post-analysis600.png) 
- **Hulp middelen** : Selecteer de hulpprogram ma's in de build-definitie waarvoor u een build-afbreek wilt injecteren op basis van de bevindingen. Voor elk geselecteerd hulp programma kunt u ook selecteren of u alleen fouten wilt opsporen of zowel fouten als waarschuwingen. 
- **Rapport** : u kunt desgewenst de gevonden resultaten schrijven en de build onderbreken in het Azure DevOps-console venster en het logboek bestand. 
- **Geavanceerde opties** : u kunt ervoor kiezen om een waarschuwing of een fout te registreren (en de taak te laten mislukken) als er geen logboeken zijn voor een van de geselecteerde hulpprogram ma's.

## <a name="next-steps"></a>Volgende stappen

[Raadpleeg onze pagina Veelgestelde vragen](security-code-analysis-faq.md) als u meer vragen hebt over de uitbrei ding en de hulpprogram ma's die worden aangeboden.


