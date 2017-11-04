---
title: Foutopsporing van een Azure-cloudservice of virtuele machine in Visual Studio | Microsoft Docs
description: Foutopsporing van een Cloudservice of virtuele Machine in Visual Studio
services: visual-studio-online
documentationcenter: na
author: mikejo
manager: ghogen
editor: 
ms.assetid: 945e06e0-2100-41af-b218-72347367ddab
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: a303e080bc847daf023eed2e9ba1ffc31e340160
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Foutopsporing van een Azure-cloudservice of virtuele machine in Visual Studio
Visual Studio biedt verschillende opties voor het opsporen van Azure-cloudservices en virtuele machines.

## <a name="debug-your-cloud-service-on-your-local-computer"></a>Fouten opsporen in uw cloudservice op de lokale computer
Bespaart u tijd en geld met behulp van de Azure-rekenemulator fouten opsporen in uw cloudservice op een lokale machine. U kunt door foutopsporing lokaal een service in voordat u deze implementeert, betrouwbaarheid en prestaties verbeteren zonder te betalen voor compute-tijd. Echter een aantal fouten optreden alleen wanneer u een cloudservice uitvoert in Azure zelf. U kunt deze fouten opsporen als u foutopsporing op afstand inschakelen wanneer u uw service publiceren en vervolgens het foutopsporingsprogramma aan een rolexemplaar koppelen.

De emulator simuleert van de Azure Compute-service en wordt uitgevoerd in uw lokale omgeving zodat u kunt testen en fouten opsporen in uw cloudservice voordat u deze implementeert. De emulator verwerkt de levenscyclus van uw rolinstanties en biedt toegang tot de gesimuleerde resources, zoals lokale opslag. Wanneer u foutopsporing of uitvoeren van uw service vanuit Visual Studio, start u de emulator automatisch als een achtergrondtoepassing en vervolgens uw service implementeert de emulator. U kunt de emulator gebruiken om weer te geven van uw service wanneer deze wordt uitgevoerd in de lokale omgeving. U kunt de volledige versie of de express-versie van de emulator uitvoeren. (Vanaf Azure 2.3 de express-versie van de emulator is de standaardwaarde.) Zie [met een Emulator snelle uitvoeren en fouten opsporen in een Cloudservice lokaal](vs-azure-tools-emulator-express-debug-run.md).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Fouten opsporen in uw cloudservice op de lokale computer
1. Kies op de menubalk **Debug**, **foutopsporing starten** uw Azure-cloud service-project wilt uitvoeren. Als alternatief kunt u op F5 drukken. Hier ziet u een bericht weergegeven waarin de Compute-Emulator wordt gestart. Wanneer de emulator wordt gestart, wordt het pictogram in het systeemvak system bevestigd.

    ![Azure-emulator in het systeemvak](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)
2. De gebruikersinterface voor de rekenemulator weergeven via het snelmenu voor het Azure-pictogram in het systeemvak en selecteer vervolgens **weergeven Compute-Emulator UI**.

    Het linkerdeelvenster van de gebruikersinterface geeft de services die momenteel zijn geïmplementeerd voor de rekenemulator en de rolinstanties die elke service wordt uitgevoerd. U kunt de service of functies levenscyclus, logboekregistratie en diagnostische gegevens weergeven in het rechterdeelvenster. Als u de focus op de bovenmarge van een venster van opgenomen plaatst, uitgebreid het vullen van het rechter deelvenster.
3. Stap door de toepassing met de opdrachten op de **Debug** menu's en onderbrekingspunten instellen in uw code. Tijdens het doorlopen van de toepassing in het foutopsporingsprogramma, worden de deelvensters bijgewerkt met de huidige status van de toepassing. Als u foutopsporing stopt, wordt de toepassingsimplementatie wordt verwijderd. Als uw toepassing een Webrol bevat en u de eigenschap opstarten action starten van de webbrowser hebt ingesteld, wordt uw webtoepassing in Visual Studio gestart in de browser. Als u het aantal exemplaren van een rol in de serviceconfiguratie wijzigt, moet u uw cloudservice stoppen en opnieuw starten zodat u fouten voor deze nieuwe exemplaren van de rol opsporen kunt foutopsporing.

    **Opmerking:** wanneer u wordt uitgevoerd of foutopsporing van uw service stopt, worden de lokale rekenemulator en opslagemulator niet worden gestopt. U moet ze expliciet stoppen vanuit het systeemvak.

## <a name="debug-a-cloud-service-in-azure"></a>Fouten opsporen in een cloudservice in Azure
Als u wilt opsporen in een cloudservice van een externe computer, moet u inschakelen dat de functionaliteit expliciet wanneer u uw cloudservice implementeert, zodat de vereiste services (bijvoorbeeld msvsmon.exe) zijn geïnstalleerd op de virtuele machines met uw rolinstanties. Als u niet foutopsporing op afstand inschakelen wanneer u de service hebt gepubliceerd, hebt u het publiceren van de service met foutopsporing op afstand is ingeschakeld.

Als u foutopsporing op afstand voor een cloudservice inschakelt, kan deze niet slechtere prestaties levert of extra kosten. U kunt foutopsporing op afstand voor een productieservice niet gebruikt omdat clients die gebruikmaken van de service mogelijk nadelig worden beïnvloed.

> [!NOTE]
> Wanneer u een cloudservice vanuit Visual Studio publiceert, kunt u inschakelen **IntelliTrace** voor alle functies in die service die gericht zijn op de .NET Framework 4 of .NET Framework 4.5. Met behulp van **IntelliTrace**, kunt u gebeurtenissen die hebben plaatsgevonden in een rolexemplaar van in het verleden onderzoeken en de context van die tijd reproduceren. Zie [foutopsporing van een gepubliceerde cloudservice met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) en [met behulp van IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).
>
>

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Foutopsporing op afstand voor een cloudservice inschakelen
1. Open het snelmenu voor het Azure-project en selecteer vervolgens **publiceren**.
2. Selecteer de **fasering** omgeving en de **Debug** configuratie.

    Dit is slechts een richtlijn. U kunt ervoor kiezen om uit te voeren van uw testomgeving in een productieomgeving. U kan echter gebruikers nadelig beïnvloeden als u foutopsporing op afstand op de productie-omgeving inschakelen. U kunt de configuratie van de Release, maar de configuratie van de foutopsporing maakt foutopsporing gemakkelijker.

    ![Kies de foutopsporing-configuratie](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)
3. De gebruikelijke stappen, maar selecteert de **externe foutopsporing inschakelen voor alle rollen** selectievakje op het **geavanceerde instellingen** tabblad.

    ![Fouten opsporen in configuratie](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Het foutopsporingsprogramma koppelen aan een cloudservice in Azure
1. Vouw het knooppunt voor de cloudservice in Server Explorer.
2. Open het snelmenu voor de functie of het rolexemplaar die u wilt toevoegen en selecteer vervolgens **foutopsporingsprogramma koppelen**.

    Als u fouten opsporen in een rol, wordt de foutopsporingsfunctie gekoppeld aan elk exemplaar van deze rol. Het foutopsporingsprogramma verbreekt op een onderbrekingspunt voor het eerste rolexemplaar die wordt uitgevoerd die regel en voldoet aan alle voorwaarden van deze onderbrekingspunt. Als u een exemplaar, het foutopsporingsprogramma wordt alleen dat exemplaar en de einden op een onderbrekingspunt alleen wanneer dat specifieke exemplaar wordt uitgevoerd die regel en voldoet aan de voorwaarden voor het onderbrekingspunt voor foutopsporing.

    ![Foutopsporingsprogramma koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)
3. Nadat de foutopsporing is gekoppeld aan een exemplaar, zoals gebruikelijk foutopsporing. Het foutopsporingsprogramma worden automatisch gekoppeld aan het proces geschikte host voor uw rol. Afhankelijk van wat de rol is, koppelt het foutopsporingsprogramma w3wp.exe, WaWorkerHost.exe of WaIISHost.exe. Om te controleren of het proces waarmee de foutopsporing is gekoppeld, vouw het knooppunt exemplaar in Server Explorer. Zie [Azure rol architectuur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) voor meer informatie over Azure-processen.

    ![Dialoogvenster voor code-type selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
4. Open het dialoogvenster processen door op de menubalk, foutopsporing, Windows, processen kiezen voor de processen die het foutopsporingsprogramma is gekoppeld. (Toetsenbord: Ctrl + Alt + Z) Als u wilt loskoppelen van een specifiek proces, opent u het snelmenu en selecteer vervolgens **loskoppelen proces**. Of, het knooppunt exemplaar niet vinden in Server Explorer, zoek het proces, opent u het snelmenu en selecteer vervolgens **loskoppelen proces**.

    ![Processen voor foutopsporing](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

> [!WARNING]
> Lange wordt gestopt bij onderbrekingspunten wanneer externe voorkomen foutopsporing. Azure beschouwt een proces dat langer dan een paar minuten als niet-reagerende wordt gestopt en wordt gestopt verkeer kunnen verzenden naar dit exemplaar. Als u voor het te lang msvsmon.exe losgekoppeld van het proces stopt.
>
>

Als u wilt loskoppelen van het foutopsporingsprogramma van alle processen in uw exemplaar of de rol, open het snelmenu voor de functie of het exemplaar dat u fouten opspoort in en selecteer vervolgens **loskoppelen foutopsporingsprogramma**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Beperkingen van foutopsporing op afstand in Azure
Van de Azure SDK 2.3 heeft foutopsporing op afstand de volgende beperkingen.

* Met foutopsporing op afstand is ingeschakeld, kunt u een cloudservice waarin een rol meer dan 25 exemplaren heeft niet publiceren.
* Het foutopsporingsprogramma poorten 30400-30424, 31400-31424 en 32400-32424 gebruikt. Als u een van deze poorten gebruiken probeert, wordt het niet mogelijk voor het publiceren van uw service en wordt een van de volgende foutberichten weergegeven in het gebeurtenissenlogboek voor Azure:

  * Fout bij het valideren van het bestand cscfg tegen het csdef-bestand.
    Het gereserveerde bereik 'poortbereik' voor het eindpunt Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector van de rol 'rol' overlapt met een reeds gedefinieerde poort of een bereik.
  * Toewijzing is mislukt. Probeer het later opnieuw, Verminder de VM-grootte of het aantal rolinstanties of probeer te implementeren naar een andere regio.

## <a name="debugging-azure-virtual-machines"></a>Foutopsporing van virtuele machines in Azure
U kunt fouten opsporen in programma's die worden uitgevoerd op virtuele machines in Azure met behulp van de Server Explorer in Visual Studio. Wanneer u foutopsporing op afstand op een virtuele machine van Azure inschakelt, installeert Azure de uitbreiding voor externe foutopsporing op de virtuele machine. U kunt vervolgens koppelen aan de processen op de virtuele machine en fouten opsporen in normaal.

> [!NOTE]
> Virtuele machines die zijn gemaakt via de Azure resource manager-stack kan op afstand fouten opgespoord via Cloud Explorer in Visual Studio 2015. Zie voor meer informatie [Azure-Resources beheren met Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031).
>
>

### <a name="to-debug-an-azure-virtual-machine"></a>Fouten opsporen in Azure een virtuele machine
1. Vouw het knooppunt van de virtuele Machines in Server Explorer en selecteer het knooppunt van de virtuele machine die u wilt opsporen.
2. Open het contextmenu en selecteer **inschakelen foutopsporing**. Wanneer u wordt gevraagd als u zeker weet of u foutopsporing inschakelt op de virtuele machine en selecteer de gewenste **Ja**.

    De externe foutopsporing extensie installeert Azure op de virtuele machine om foutopsporing te schakelen.

    ![Virtuele machine foutopsporing-opdracht inschakelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Activiteitenlogboek van Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
3. Nadat de externe foutopsporing extensie installatie heeft voltooid, opent u het contextmenu van de virtuele machine en selecteer **foutopsporingsprogramma koppelen...**

    Azure opgehaald van een lijst van de processen op de virtuele machine en wordt deze weergegeven in het koppelen aan het dialoogvenster proces.

    ![De opdracht foutopsporingsprogramma koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
4. In de **koppelen aan proces** dialoogvenster, **Selecteer** om te beperken van de lijst met resultaten om weer te geven van de soorten code u fouten wilt opsporen. U kunt fouten opsporen 32 - of 64-bits beheerde code, systeemeigen code of beide.

    ![Dialoogvenster voor code-type selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
5. Selecteer de processen die u wilt opsporen in op de virtuele machine en selecteer vervolgens **Attach**. U kunt het proces w3wp.exe bijvoorbeeld, als u fouten opsporen in een web-app op de virtuele machine wilt maken. Zie [fouten opsporen in een of meer processen in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) en [Azure rol architectuur](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) voor meer informatie.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Maak een webproject en een virtuele machine voor het opsporen van fouten
Voordat u uw Azure-project publiceert, het wellicht handig om te testen in een ingesloten omgeving die ondersteuning biedt voor foutopsporing en testen van scenario's en waar u testen en controleren of programma's kunt installeren. Een manier om dit te doen is op afstand fouten opsporen in uw app op een virtuele machine.

Visual Studio ASP.NET-projecten bieden een optie voor het maken van een handige virtuele machine die u voor het testen van de app gebruiken kunt. De virtuele machine bevat meestal nodig eindpunten zoals PowerShell, extern bureaublad en Web Deploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Maken van een webproject en een virtuele machine voor het opsporen van fouten
1. In Visual Studio maakt een nieuwe ASP.NET-webtoepassing.
2. Kies in het dialoogvenster Nieuw ASP.NET-Project in de sectie Azure **virtuele Machine** in de vervolgkeuzelijst. Laat de **maken van externe bronnen** selectievakje is ingeschakeld. Selecteer **OK** om door te gaan.

    De **virtuele machine in Azure maken** dialoogvenster wordt weergegeven.

    ![Dialoogvenster voor ASP.NET web project maken](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Opmerking:** wordt u gevraagd om aan te melden bij uw Azure-account als u nog niet bent aangemeld.

1. Selecteer de verschillende instellingen voor de virtuele machine en selecteer vervolgens **OK**. Zie [virtuele Machines](http://go.microsoft.com/fwlink/?LinkId=623033) voor meer informatie.

    De naam die u voor DNS-naam invoert is de naam van de virtuele machine.

    ![Virtuele machine op in het dialoogvenster Azure maken](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure maakt de virtuele machine en de bepalingen en configureert u de eindpunten, zoals Extern bureaublad- en Web Deploy
2. Nadat de virtuele machine is volledig geconfigureerd, moet u de virtuele machine knooppunt selecteren in Server Explorer.
3. Open het contextmenu en selecteer **inschakelen foutopsporing**. Wanneer u wordt gevraagd als u zeker weet of u foutopsporing inschakelt op de virtuele machine en selecteer de gewenste **Ja**.

    Azure installeert de externe foutopsporing extensie aan de virtuele machine om foutopsporing te schakelen.

    ![Virtuele machine foutopsporing-opdracht inschakelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Activiteitenlogboek van Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
4. Publiceren van uw project, zoals wordt beschreven in [procedure: een Web-Project met één klik publiceren in Visual Studio implementeren](https://msdn.microsoft.com/library/dd465337.aspx). Omdat u fouten opsporen op de virtuele machine wilt op de **instellingen** pagina van de **webpublicatie** wizard selecteert u **Debug** als de configuratie. Dit zorgt ervoor dat de symbolen code beschikbaar tijdens het opsporen van fouten zijn.

    ![Publicatie-instellingen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)
5. In de **opties voor het publiceren van bestand**, selecteer **aanvullende bestanden op de bestemming verwijderen** als u het project is al geïmplementeerd op een eerder tijdstip.
6. Nadat het project, in het contextmenu van de virtuele machine in Server Explorer publiceert, selecteert u **foutopsporingsprogramma koppelen...**

    Azure opgehaald van een lijst van de processen op de virtuele machine en wordt deze weergegeven in het koppelen aan het dialoogvenster proces.

    ![De opdracht foutopsporingsprogramma koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
7. In de **koppelen aan proces** dialoogvenster, **Selecteer** om te beperken van de lijst met resultaten om weer te geven van de soorten code u fouten wilt opsporen. U kunt fouten opsporen 32 - of 64-bits beheerde code, systeemeigen code of beide.

    ![Dialoogvenster voor code-type selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
8. Selecteer de processen die u wilt opsporen in op de virtuele machine en selecteer vervolgens **Attach**. U kunt het proces w3wp.exe bijvoorbeeld, als u fouten opsporen in een web-app op de virtuele machine wilt maken. Zie [fouten opsporen in een of meer processen in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
* Gebruik **Intellitrace** een logboek van aanroepen en gebeurtenissen verzamelen van een release-server. Zie [foutopsporing van een gepubliceerde Cloudservice met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).
* Gebruik **Azure Diagnostics** gedetailleerde om informatie te registreren in rollen, code wordt uitgevoerd of de functies worden uitgevoerd in de ontwikkelomgeving of in Azure. Zie [logboekgegevens verzamelen met behulp van Azure Diagnostics](http://go.microsoft.com/fwlink/p/?LinkId=400450).
