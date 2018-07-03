---
title: Profilering van een Cloudservice lokaal in de Rekenemulator | Microsoft Docs
services: cloud-services
description: Het onderzoeken van prestatieproblemen in cloudservices met de Visual Studio profiler
documentationcenter: ''
author: mikejo
manager: douge
editor: ''
tags: ''
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/18/2016
ms.author: mikejo
ms.openlocfilehash: ea46039583681bd89e254d153997e3a300041d4e
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341351"
---
# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>De prestaties van een Cloudservice lokaal te testen in de Azure-Rekenemulator met behulp van de Visual Studio Profiler
Diverse hulpmiddelen en technieken zijn beschikbaar voor het testen van de prestaties van cloudservices.
Wanneer u een cloudservice naar Azure publiceert, kunt u Visual Studio gegevens verzamelen en deze vervolgens te analyseren is lokaal op, zoals beschreven in [profilering van een Azure-toepassing][1].
U kunt ook diagnostische gegevens gebruiken voor het bijhouden van tal van prestatiemeteritems, zoals beschreven in [met behulp van prestatiemeteritems in Azure][2].
U kunt ook het profiel van uw toepassing lokaal in de rekenemulator voorafgaand aan implementatie naar de cloud.

Dit artikel behandelt de profileringsmethode voor CPU-sampling, die lokaal kan worden uitgevoerd in de emulator. CPU-sampling is een methode van de profilering die is niet erg firewallverbinding. Op een aangewezen steekproefinterval momentopname de profiler een van de aanroepstack. De gegevens is verzameld gedurende een bepaalde periode en wordt weergegeven in een rapport. Deze profileringsmethode om aan te geven waar in een rekenintensieve toepassing de meeste CPU werk wordt verricht.  Dit biedt u de mogelijkheid om zich te richten op het 'hot 'pad waar uw toepassing de meeste tijd doorbrengt.

## <a name="1-configure-visual-studio-for-profiling"></a>1: Visual Studio configureren voor het profileren
Eerste, zijn er enkele Visual Studio-configuratieopties die mogelijk nuttig zijn wanneer de profilering. Voor het zinvol zijn van de profilering rapporten, moet u voor uw toepassing en ook symbolen voor systeembibliotheken symbolen (.pdb-bestanden). Moet u om ervoor te zorgen dat u verwijst naar de servers beschikbaar symbool. Om dit te doen, op de **extra** in het menu in Visual Studio **opties**, en kies vervolgens **foutopsporing**, klikt u vervolgens **symbolen**. Zorg ervoor dat Microsoft symbool Servers wordt vermeld onder **bestandslocaties (.pdb) symbool**.  U kunt ook verwijzen naar http://referencesource.microsoft.com/symbols, die mogelijk aanvullende symboolbestanden.

![Symboolopties voor][4]

Indien gewenst, kunt u de rapporten die de profiler wordt gegenereerd door in te stellen alleen mijn Code vereenvoudigen. Functie-aanroep stacks zijn met alleen mijn Code is ingeschakeld, vereenvoudigd zodat aanroepen volledig interne bibliotheken en .NET Framework zijn verborgen voor de rapporten. Op de **extra** menu, kiest u **opties**. Vouw vervolgens de **prestatiehulpprogramma's** knooppunt, en kies **algemene**. Schakel het selectievakje voor **inschakelen alleen mijn Code voor profiler rapporten**.

![Alleen mijn Code opties][17]

Volg deze instructies kunt u met een bestaand project of een nieuw project.  Als u een nieuw project om te proberen de hieronder beschreven technieken maakt, kiest u een C# **Azure Cloud Service** project en selecteer een **Webrol** en een **Werkrol**.

![Azure Cloud Service-project-rollen][5]

Bijvoorbeeld toevoegen doeleinden, code aan uw project die neemt veel tijd en ziet u een prestatieprobleem voor de hand liggende. Bijvoorbeeld, de volgende code toevoegen aan een werkrolproject:

```csharp
public class Concatenator
{
    public static string Concatenate(int number)
    {
        int count;
        string s = "";
        for (count = 0; count < number; count++)
        {
            s += "\n" + count.ToString();
        }
        return s;
    }
}
```

Deze code aanroepen via de methode RunAsync in de werkrol RoleEntryPoint afgeleide klasse. (Negeer de waarschuwing over de methode synchroon uitgevoerd.)

```csharp
private async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following with your own logic.
    while (!cancellationToken.IsCancellationRequested)
    {
        Trace.TraceInformation("Working");
        Concatenator.Concatenate(10000);
    }
}
```

Bouwen en uitvoeren van uw cloudservice lokaal zonder foutopsporing (Ctrl + F5), met de oplossingsconfiguratie van de ingesteld op **Release**. Dit zorgt ervoor dat alle bestanden en mappen worden gemaakt voor het uitvoeren van de toepassing lokaal en zorgt ervoor dat alle emulators worden gestart. De gebruikersinterface van de Rekenemulator starten vanaf de taakbalk om te controleren of de werkrol wordt uitgevoerd.

## <a name="2-attach-to-a-process"></a>2: koppelen aan een proces
In plaats van de toepassing door deze te starten vanuit de Visual Studio 2010 IDE-profilering, moet u de profiler koppelen aan een proces dat wordt uitgevoerd. 

De profiler koppelen aan een proces op de **analyseren** menu, kiest u **Profiler** en **Attach/Detach**.

![Optie profiel koppelen][6]

Zoek het WaWorkerHost.exe-proces voor een werkrol.

![WaWorkerHost proces][7]

Als de projectmap zich op een netwerkstation bevindt, de profiler wordt u gevraagd een andere locatie om op te slaan van de profilering rapporten op te geven.

 U kunt ook koppelen aan een Webrol door te koppelen aan WaIISHost.exe.
Als er meerdere werkprocessen rol in uw toepassing, moet u de proces-id gebruiken om ze te onderscheiden. U kunt de proces-id programmatisch opvragen door het openen van het procesobject. Als u deze code aan de Run-methode van de klasse die is afgeleid van RoleEntryPoint in een rol toevoegt, kunt u kunt bijvoorbeeld voor zoeken op het logboek in de gebruikersinterface van de Rekenemulator om te weten welk proces verbinden.

```csharp
var process = System.Diagnostics.Process.GetCurrentProcess();
var message = String.Format("Process ID: {0}", process.Id);
Trace.WriteLine(message, "Information");
```

Als u wilt het logboek weergeeft, start u de gebruikersinterface van de Rekenemulator.

![Start de Rekenemulator UI][8]

Open in het consolevenster van worker-rol log in de gebruikersinterface van de Rekenemulator door te klikken op de titelbalk van het consolevenster. Hier ziet u de proces-ID in het logboek.

![Proces-ID weergeven][9]

Een u hebt gekoppeld, volg de stappen in de gebruikersinterface van uw toepassing (indien nodig) voor het reproduceren van het scenario.

Als u stoppen wilt, kiest de **profilering stoppen** koppeling.

![Optie-profilering stoppen][10]

## <a name="3-view-performance-reports"></a>3: prestatierapporten weergeven
Het prestatierapport voor uw toepassing wordt weergegeven.

Op dit moment de profiler stopt met het uitvoeren van, gegevens worden opgeslagen in een bestand .vsp en een rapport met een analyse van deze gegevens worden weergegeven.

![Profiler-rapport][11]

Als u String.wstrcpy in het dynamische pad ziet, klikt u op alleen mijn Code om de weergave om weer te geven van gebruikerscode alleen te wijzigen.  Als u String.Concat ziet, drukt u op de knop alle Code weergeven.

U ziet de Concatenate methode en String.Concat een groot deel van de uitvoeringstijd in beslag.

![Analyse van rapport][12]

Als u de code van de samengevoegde tekenreeks in dit artikel hebt toegevoegd, ziet u een waarschuwing in de lijst met taken voor dit. U ziet ook een waarschuwing dat er is een overmatig groot aantal garbagecollection die wordt veroorzaakt door het aantal tekenreeksen die zijn gemaakt en verwijderd.

![Prestaties van waarschuwingen][14]

## <a name="4-make-changes-and-compare-performance"></a>4: wijzigingen aanbrengen en vergelijken van prestaties
U kunt de prestaties vóór en na een codewijziging ook vergelijken.  Stop het proces dat wordt uitgevoerd en de code bewerken om de bewerking van de samengevoegde tekenreeks vervangen door het gebruik van StringBuilder:

```csharp
public static string Concatenate(int number)
{
    int count;
    System.Text.StringBuilder builder = new System.Text.StringBuilder("");
    for (count = 0; count < number; count++)
    {
        builder.Append("\n" + count.ToString());
    }
    return builder.ToString();
}
```

Voer een andere prestaties uitvoeren en vergelijkt u de prestaties. In de Verkenner prestaties als de uitvoeringen in dezelfde sessie zijn, kunt u alleen selecteert u beide rapporten, open het snelmenu, en kies **prestatierapporten vergelijken**. Als u vergelijken met een uitvoering in een andere prestatiesessie wilt, opent u de **analyseren** in het menu en kies **prestatierapporten vergelijken**. Geef beide bestanden in het dialoogvenster dat wordt weergegeven.

![Optie voor prestaties rapporten vergelijken][15]

De rapporten markeren verschillen tussen de twee wordt uitgevoerd.

![Van vergelijkingsrapport][16]

Gefeliciteerd! U hebt verkregen gestart met de profiler.

## <a name="troubleshooting"></a>Problemen oplossen
* Zorg ervoor dat u een Release-build zijn profilering en starten zonder foutopsporing.
* Als de optie Attach/Detach is niet ingeschakeld in het menu Profiler, voert u de Wizard prestaties.
* Gebruik de gebruikersinterface van de Rekenemulator om de status van uw toepassing weer te geven. 
* Als u problemen ondervindt bij het starten van toepassingen in de emulator of koppelen van de profiler, sluit u de rekenemulator en start deze opnieuw. Als dat niet het probleem is opgelost, probeert u opnieuw wordt opgestart. Dit probleem kan optreden als u de Compute-Emulator gebruiken om te onderbreken en verwijderen van actieve implementaties.
* Als u hebt gebruikt een van de profilering opdrachten vanaf de opdrachtregel, met name de algemene instellingen, zorg dat VSPerfClrEnv /globaloff is aangeroepen en dat VsPerfMon.exe is afgesloten.
* Als meting, u het bericht ziet ' PRF0025: Er zijn geen gegevens zijn verzameld, "Controleer of het proces dat u hebt gekoppeld aan CPU-activiteit heeft. Toepassingen die niet worden rekenkundige werk doen mogelijk niet produceren van een steekproef nemen voor gegevens.  Het is ook mogelijk dat het proces is afgesloten voordat er een steekproef nemen is uitgevoerd. Controleer dat de Run-methode voor een rol die u profilering worden niet beëindigd.

## <a name="next-steps"></a>Volgende stappen
Instrumenteren Azure binaire bestanden in de emulator wordt niet ondersteund in de Visual Studio profiler, maar als u testen van de toewijzing van geheugen wilt, kunt u deze optie kiezen als profilering. U kunt ook gelijktijdigheid-profilering, die helpt u bepalen of threads zijn verspilling van tijd en dingen om de vergrendelingen of laag tussenkomst van de profilering, waarmee u problemen met prestaties sporen zijn wanneer de interactie tussen lagen van een toepassing, meest regelmatig tussen de gegevenslaag en een werkrol.  U kunt de database-query's die uw app genereert weergeven en gebruiken van de profileringsgegevens die voor het verbeteren van uw gebruik van de database. Zie voor informatie over laag tussenkomst van de profilering, het blogbericht [Walkthrough: met behulp van de laag interactie Profiler in Visual Studio Team System 2010][3].

[1]: https://docs.microsoft.com/azure/application-insights/app-insights-profiler
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
