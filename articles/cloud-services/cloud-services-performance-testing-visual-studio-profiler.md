---
title: Profileren van een Cloudservice lokaal in de Rekenemulator | Microsoft Docs
services: cloud-services
description: Het onderzoeken van prestatieproblemen in cloudservices met de profiler Visual Studio
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
tags: 
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 51c8192d8312dc5cf546b4c357aeecf6f19d56b8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>De prestaties van een Cloudservice lokaal te testen in de Azure-Rekenemulator met behulp van de Profiler Visual Studio
Tal van hulpprogramma's en technieken zijn beschikbaar voor het testen van de prestaties van cloudservices.
Wanneer u een cloudservice naar Azure publiceert, kunt u Visual Studio profileringsgegevens verzamelen en deze vervolgens te analyseren hebt lokaal, zoals beschreven in [profileren van een Azure-toepassing][1].
U kunt ook diagnostische gegevens gebruiken voor het bijhouden van tal van prestatiemeteritems, zoals beschreven in [met behulp van de prestatiemeteritems in Azure][2].
U kunt ook het profiel van uw toepassing lokaal in de rekenemulator voordat u deze implementeert in de cloud.

Dit artikel behandelt de profileringsmethode voor CPU-sampling, die lokaal kan worden uitgevoerd in de emulator. CPU-steekproeven is een methode voor het samenstellen van profiel die niet zeer hoog. De profiler maakt op een aangewezen steekproefinterval een momentopname van de aanroepstack. De gegevens worden verzameld over een tijdsperiode, en in een rapport weergegeven. Deze methode van profilering doorgaans om aan te geven waar in een toepassing rekenkracht meeste CPU werk wordt uitgevoerd.  Hierdoor kunt u zich kunt richten op het 'hot 'pad waar de toepassing de meeste tijd besteedt.

## <a name="1-configure-visual-studio-for-profiling"></a>1: Visual Studio voor profilering configureren
Er zijn eerst enkele Visual Studio-configuratieopties die nuttig zijn kunnen bij het samenstellen van profiel. Om het zinvol zijn van de profilering rapporten, moet u symbolen (.pdb-bestanden) voor uw toepassing en ook symbolen voor systeembibliotheken. Moet u ervoor zorgen dat u verwijst naar de beschikbare symbool-servers. Om dit te doen, op de **extra** in het menu in Visual Studio **opties**, en kies vervolgens **foutopsporing**, klikt u vervolgens **symbolen**. Zorg ervoor dat Microsoft symbool Servers wordt vermeld onder **symbool bestandslocaties (.pdb)**.  U kunt ook verwijzen naar http://referencesource.microsoft.com/symbols deze heeft mogelijk extra symboolbestanden.

![Symboolopties][4]

Indien gewenst, kunt u de rapporten die de profiler wordt gegenereerd door in te stellen alleen mijn Code vereenvoudigen. Functie-aanroep stacks zijn met alleen mijn Code is ingeschakeld, vereenvoudigd zodat aanroepen volledig interne bibliotheken en .NET Framework zijn verborgen voor de rapporten. Op de **extra** menu kiezen **opties**. Vouw de **prestatiehulpprogramma's** knooppunt, en kies **algemene**. Schakel het selectievakje voor **inschakelen alleen mijn Code voor de profiler rapporten**.

![Alleen mijn Code opties][17]

U kunt deze instructies met een bestaand project of met een nieuw project.  Als u een nieuw project om te proberen de hieronder beschreven technieken maakt, kiest u een C# **Azure Cloud Service** project en selecteer een **Webrol** en een **Werkrol**.

![Azure Cloud Service-Projectrollen][5]

Bijvoorbeeld toevoegen toepassing, code aan uw project die ervoor zorgt dat veel tijd ziet u een prestatieprobleem voor de hand liggende. Bijvoorbeeld, voeg de volgende code naar een werkrolproject:

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

Deze code aanroepen vanuit de RunAsync-methode in de werkrol RoleEntryPoint afgeleide klasse. (Negeer de waarschuwing over de methode synchroon uitgevoerd.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Bouwen en uitvoeren van uw cloudservice lokaal zonder foutopsporing (Ctrl + F5) met de oplossingsconfiguratie ingesteld op **Release**. Dit zorgt ervoor dat alle bestanden en mappen worden gemaakt voor het uitvoeren van de toepassing lokaal en zorgt ervoor dat alle emulators worden gestart. De gebruikersinterface van de Emulator Compute starten vanaf de taakbalk om te controleren of uw werkrol wordt uitgevoerd.

## <a name="2-attach-to-a-process"></a>2: koppelen aan een proces
In plaats van de toepassing vanaf de Visual Studio 2010 IDE profilering, moet u de profiler koppelen aan een proces dat wordt uitgevoerd. 

De profiler koppelen aan een proces op de **analyseren** menu kiezen **Profiler** en **Attach/Detach**.

![Optie profiel koppelen][6]

Zoek het WaWorkerHost.exe-proces voor een werkrol.

![WaWorkerHost proces][7]

Als de projectmap zich op een netwerkstation bevindt, vraagt de profiler u een andere locatie om op te slaan de profilering rapporten bieden.

 U kunt ook koppelen aan een Webrol door aan WaIISHost.exe te koppelen.
Als er meerdere werkprocessen functie in uw toepassing zijn, moet u de proces-id gebruiken om ze te onderscheiden. U kunt de processID programmatisch opvragen door het openen van het procesobject. Als u deze code aan de methode Run van de klasse die is afgeleid van RoleEntryPoint in een rol toevoegen, kunt u kunt bijvoorbeeld voor zoeken in het logbestand in de gebruikersinterface van de Emulator Compute weten welk proces verbinding maken met.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Als u wilt weergeven in het logboek, start de gebruikersinterface van de Emulator Compute.

![Start de gebruikersinterface van de Rekenemulator][8]

Open het consolevenster worker-rol logboek in de gebruikersinterface van de Emulator berekenen door te klikken op de titelbalk van het consolevenster. U ziet de proces-ID in het logboek.

![Proces-ID weergeven][9]

Een u hebt gekoppeld, voer de stappen in de gebruikersinterface van uw toepassing (indien nodig) om het scenario.

Als u stoppen profilering wilt, kiest u de **stoppen profilering** koppeling.

![Profileren van de optie stoppen][10]

## <a name="3-view-performance-reports"></a>3: prestatierapporten weergeven
Het prestatierapport voor uw toepassing wordt weergegeven.

Op dit moment de profiler stopt met het uitvoeren van gegevens worden opgeslagen in een bestand .vsp en een rapport een analyse van deze gegevens bevat worden weergegeven.

![Profiler rapport][11]

Als u String.wstrcpy in de Hot pad ziet, klik op alleen mijn Code voor het wijzigen van de weer te geven alleen gebruikerscode.  Als u String.Concat ziet, drukt u op de knop alle Code weergeven.

U ziet de samenvoegen methode en String.Concat inneemt een groot deel van de uitvoeringstijd.

![Analyse van rapport][12]

Als u de code van de samengevoegde tekenreeks in dit artikel hebt toegevoegd, ziet u een waarschuwing in de taaklijst voor deze. U ziet ook een waarschuwing dat er is een uitzonderlijk groot aantal garbagecollection die wordt veroorzaakt door het aantal tekenreeksen die zijn gemaakt en verwijderd.

![Waarschuwingen voor prestaties][14]

## <a name="4-make-changes-and-compare-performance"></a>4: wijzigingen aanbrengen en vergelijkt u prestaties
U kunt ook de prestaties voor en na een codewijziging vergelijken.  Stoppen van het proces dat wordt uitgevoerd en de code ter vervanging van de bewerking van de samengevoegde tekenreeks met het gebruik van StringBuilder bewerken:

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

Uitvoeren van een andere prestaties doen, en vervolgens vergelijken de prestaties. De Explorer prestaties als het wordt uitgevoerd in dezelfde sessie, u kunt alleen Selecteer in beide rapporten, open het snelmenu, en kies **prestatierapporten vergelijken**. Als u vergelijken met een wordt uitgevoerd in een andere prestatiesessie wilt, opent u de **analyseren** menu en kies **prestatierapporten vergelijken**. Geef beide bestanden in het dialoogvenster dat wordt weergegeven.

![Optie voor prestaties van rapporten vergelijken][15]

De rapporten Markeer verschillen tussen de twee wordt uitgevoerd.

![Van vergelijkingsrapport][16]

Gefeliciteerd. U slag bent gegaan met de profiler.

## <a name="troubleshooting"></a>Problemen oplossen
* Controleer of u een Release-build zijn profilering en starten zonder foutopsporing.
* Als de optie Attach/Detach is niet ingeschakeld in het menu Profiler, voert u de Wizard prestaties.
* De Emulator Compute gebruikersinterface gebruiken om de status van uw toepassing weer te geven. 
* Als u problemen ondervindt bij het starten van toepassingen in de emulator of koppelen van de profiler, sluit de rekenemulator omlaag en start deze opnieuw. Als dit het probleem niet oplost, opnieuw op te starten. Dit probleem kan optreden als u de Compute-Emulator gebruiken om te onderbreken en verwijderen van actieve implementaties.
* Als u een van de profilering opdrachten vanaf de opdrachtregel, met name de algemene instellingen hebt gebruikt moet VSPerfClrEnv /globaloff is aangeroepen en dat VsPerfMon.exe is afgesloten.
* Als een steekproef nemen, ziet u het bericht ' PRF0025: is er geen gegevens verzameld, "Controleer of het proces dat u hebt gekoppeld aan CPU-activiteit is. Toepassingen die niet alle rekenwerk doen produceert mogelijk niet alle gegevens steekproeven.  Het is ook mogelijk dat het proces is afgesloten voordat alle steekproeven werd uitgevoerd. Controleer dat de Run-methode voor een functie die u zijn profilering niet beëindigt.

## <a name="next-steps"></a>Volgende stappen
Azure binaire bestanden in de emulator instrumenteren wordt niet ondersteund in de Visual Studio-profiler, maar als u testen geheugentoewijzing wilt, kunt u die optie kiezen bij het samenstellen van profiel. U kunt ook gelijktijdigheid profilering, waarmee u bepalen of threads zijn verspilling tijd concurreren voor vergrendelingen of laag interactie profilering, waarmee u prestatieproblemen sporen zijn wanneer de interactie tussen lagen van een toepassing, meest vaak tussen de gegevenslaag en een werkrol.  U kunt bekijken van de database-query's die uw app wordt gegenereerd en de profileringsgegevens gebruiken voor het verbeteren van uw gebruik van de database. Zie voor informatie over laag interactie profilering, het blogbericht [Walkthrough: met behulp van de laag interactie Profiler in Visual Studio Team System 2010][3].

[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
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
