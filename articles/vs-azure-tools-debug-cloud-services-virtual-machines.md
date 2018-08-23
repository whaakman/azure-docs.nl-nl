---
title: Foutopsporing in een Azure-cloudservice of virtuele machine in Visual Studio | Microsoft Docs
description: Foutopsporing in een Cloudservice of virtuele Machine in Visual Studio
services: visual-studio-online
documentationcenter: na
author: mikejo
manager: douge
editor: ''
ms.assetid: 945e06e0-2100-41af-b218-72347367ddab
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: af961533648ce3a939c25fca32173684255a52a8
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444687"
---
# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Foutopsporing in een Azure-cloudservice of virtuele machine in Visual Studio

Visual Studio biedt verschillende opties voor het opsporen van fouten in Azure cloudservices en virtuele machines.

## <a name="debug-your-cloud-service-on-your-local-computer"></a>Fouten opsporen in uw cloudservice op de lokale computer

U bespaart tijd en geld met behulp van de Azure-rekenemulator fouten opsporen in uw cloudservice op een lokale computer. U kunt door een service-lokaal foutopsporing voordat u deze implementeert, betrouwbaarheid en prestaties verbeteren zonder te hoeven betalen voor de rekentijd. Echter enkele fouten optreden alleen wanneer u een cloudservice uitvoert in Azure zelf. U kunt deze fouten kan fouten opsporen, als u foutopsporing op afstand inschakelen wanneer u uw service publiceren en vervolgens het foutopsporingsprogramma aan een rolinstantie koppelt.

De emulator simuleert de Azure Compute-service en wordt uitgevoerd in uw lokale omgeving zodat u kunt testen en fouten opsporen in uw cloudservice voordat u deze implementeert. De emulator verwerkt de levenscyclus van uw rolinstanties en biedt toegang tot gesimuleerde resources, zoals lokale opslag. Wanneer u fouten opspoort of uw service vanuit Visual Studio uitvoeren, start u de emulator automatisch als een achtergrond-toepassing en klikt u vervolgens implementeert uw service in de emulator. U kunt de emulator gebruiken om weer te geven van uw service wanneer deze wordt uitgevoerd in de lokale omgeving. U kunt de volledige versie of de express-versie van de emulator uitvoeren. (Te beginnen met Azure 2.3, de express-versie van de emulator is de standaardinstelling.) Zie [Emulator Express gebruiken voor het uitvoeren en fouten opsporen in een Cloudservice lokaal](vs-azure-tools-emulator-express-debug-run.md).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Fouten opsporen in uw cloudservice op de lokale computer

1. Kies op de menubalk **Debug**, **Start Debugging** om uit te voeren van uw Azure-cloud service-project. Als alternatief kunt u druk op F5. U ziet een bericht dat de Compute-Emulator wordt gestart. Wanneer de emulator wordt gestart, wordt het door het pictogram in systeemvak voor systeem bevestigt.

    ![Azure-emulator in het systeemvak](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

2. De gebruikersinterface voor de rekenemulator weergeven door het openen van het snelmenu voor het Azure-pictogram in het systeemvak te plaatsen en selecteer vervolgens **weergeven gebruikersinterface van de Rekenemulator**.

    Het linkerdeelvenster van de gebruikersinterface ziet u de services die momenteel zijn geïmplementeerd op de rekenemulator en de rolinstanties die elke service wordt uitgevoerd. U kunt de service of -rollen om weer te geven van de levenscyclus, logboekregistratie en diagnostische gegevens in het rechter deelvenster. Als u de focus op de bovenkant van een venster met een inbegrepen opslaat, wordt deze uitgebreid om te vullen in het rechterdeelvenster.

3. Doorlopen van de toepassing met de opdrachten op de **Debug** menu en onderbrekingspunten instellen in uw code. Tijdens het doorlopen van de toepassing in het foutopsporingsprogramma, worden de deelvensters bijgewerkt met de huidige status van de toepassing. Wanneer u stopt met het opsporen van fouten, wordt implementatie van de toepassing verwijderd. Als uw toepassing een Webrol bevat en u de eigenschap van de actie gestart om te beginnen de webbrowser hebt ingesteld, begint Visual Studio uw web-App in de browser. Als u het aantal exemplaren van een rol in de serviceconfiguratie wijzigt, moet u uw cloudservice stoppen en opnieuw opstarten zodat u kunt deze nieuwe instanties van de rol van fouten opsporen van fouten.

    **Opmerking:** wanneer u actief of foutopsporing van uw service stopt, worden de lokale compute-emulator en de opslagemulator worden niet gestopt. U moet ze expliciet stoppen vanuit het systeemvak.

## <a name="debug-a-cloud-service-in-azure"></a>Fouten opsporen in een cloudservice in Azure

Als u wilt fouten opsporen in een cloudservice vanaf een externe computer, moet u inschakelen dat de functionaliteit expliciet wanneer u uw cloudservice implementeert, zodat de vereiste services (bijvoorbeeld msvsmon.exe) zijn geïnstalleerd op de virtuele machines met uw rolinstanties. Als u niet hebt ingeschakeld om externe foutopsporing wanneer u de service hebt gepubliceerd, hebt u de service publiceren met de externe foutopsporing is ingeschakeld.

Als u foutopsporing op afstand voor een cloudservice hebt ingeschakeld, kan deze niet slechtere prestaties levert of extra kosten in rekening gebracht. Gebruik geen externe foutopsporing op een productieservice, omdat clients die gebruikmaken van de service kunnen nadelig worden beïnvloed.

> [!NOTE]
> Wanneer u een service in de cloud vanuit Visual Studio publiceert, kunt u inschakelen **IntelliTrace** voor alle functies in die service die gericht zijn op het .NET Framework 4 of .NET Framework 4.5. Met behulp van **IntelliTrace**, kunt u gebeurtenissen die hebben plaatsgevonden in een rolinstantie in het verleden onderzoeken en de context van die tijd reproduceren. Zie [opsporen van fouten in een gepubliceerde cloudservice met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) en [met behulp van IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Foutopsporing op afstand voor een cloudservice inschakelen

1. Open het snelmenu voor het Azure-project en selecteer vervolgens **publiceren**.

2. Selecteer de **fasering** omgeving en de **Debug** configuratie.

    Dit is slechts een richtlijn. U kunt kiezen voor het uitvoeren van uw testomgevingen in een productieomgeving. Echter, u mogelijk nadelige invloed heeft op gebruikers als u externe foutopsporing op de productie-omgeving inschakelen. U kunt de configuratie van de Release, maar de configuratie van de foutopsporing is foutopsporing gemakkelijker.

    ![Kiest u de configuratie voor foutopsporing](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

3. De gebruikelijke stappen maar selecteer de **externe foutopsporing inschakelen voor alle rollen** selectievakje op de **geavanceerde instellingen** tabblad.

    ![Fouten opsporen in configuratie](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Het foutopsporingsprogramma koppelen aan een cloudservice in Azure

1. Vouw in Server Explorer het knooppunt voor je cloudservice.

2. Open het snelmenu voor de functie of de rolinstantie die u wilt koppelen, en selecteer vervolgens **foutopsporingsprogramma koppelen**.

    Als u fouten opsporen in een rol, wordt de foutopsporingsfunctie gekoppeld aan elk exemplaar van die rol. Het foutopsporingsprogramma wordt op een onderbrekingspunt voor de eerste rolinstantie die wordt uitgevoerd die regel code en voldoet aan alle voorwaarden van deze onderbrekingspunt verbroken. Als u een exemplaar, de foutopsporing wordt alleen voor die instantie en regeleinden zijn opgesplitst op een onderbrekingspunt alleen wanneer dat specifieke exemplaar dat line-of-code wordt uitgevoerd en voldoet aan de voorwaarden van het onderbrekingspunt fouten opsporen.

    ![Debugger koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

3. Nadat het foutopsporingsprogramma aan een exemplaar koppelt, fouten opsporen in normale wijze te gebruiken. Het foutopsporingsprogramma worden automatisch gekoppeld aan het proces van de meest geschikte host voor uw rol. Afhankelijk van wat de rol is, koppelt u het foutopsporingsprogramma aan w3wp.exe, WaWorkerHost.exe of WaIISHost.exe. Om te controleren of het proces waarmee het foutopsporingsprogramma is gekoppeld, vouw het knooppunt van het exemplaar in Server Explorer. Zie [Azure Role Architecture](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) voor meer informatie over Azure-processen.

    ![Dialoogvenster voor code-type selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

4. Open het dialoogvenster processen door op de menubalk, foutopsporing, Windows, processen te kiezen voor het identificeren van de processen die het foutopsporingsprogramma is gekoppeld. (Toetsenbord: Ctrl + Alt + Z) Als u wilt loskoppelen van een specifiek proces, opent u de snelkoppeling menu en selecteer vervolgens **loskoppelen proces**. Of, zoek de exemplaar-knooppunt in Server Explorer, zoek het proces, het snelmenu te openen en selecteer vervolgens **loskoppelen proces**.

    ![Fouten opsporen in processen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

> [!WARNING]
> Te voorkomen dat lange wordt gestopt bij onderbrekingspunten wanneer externe foutopsporing. Azure wordt een proces dat gestopt voor langer dan een paar minuten als niet-reagerende en stopt het verzenden van verkeer naar dat exemplaar behandeld. Als u voor te lang msvsmon.exe losgekoppeld van het proces stopt.

Als u wilt loskoppelen van het foutopsporingsprogramma van alle processen in uw exemplaar of -rol, open het snelmenu voor de functie of het exemplaar dat u fouten opspoort in en selecteer vervolgens **loskoppelen foutopsporingsprogramma**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Beperkingen van de externe foutopsporing in Azure

Van de Azure SDK 2.3 heeft foutopsporing op afstand de volgende beperkingen:

* Met de externe foutopsporing ingeschakeld, kunt u een cloudservice waarin een rol meer dan 25 instanties heeft niet publiceren.
* Het foutopsporingsprogramma poorten 30400-30424, 31400-31424 en 32400-32424 gebruikt. Als u een van deze poorten gebruiken probeert, kunt u zich niet voor het publiceren van uw service en wordt een van de volgende foutberichten weergegeven in het activiteitenlogboek voor Azure:

  * Fout bij het valideren van het bestand cscfg tegen het .csdef-bestand.
    Het gereserveerde bereik 'bereik' voor het eindpunt Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector van rol 'rol' overlapt met een al gedefinieerde poort of -bereik.
  * Toewijzing is mislukt. Probeer het later opnieuw, Verminder de VM-grootte of het aantal rolinstanties, of probeer te implementeren naar een andere regio.

## <a name="debugging-azure-virtual-machines"></a>Foutopsporing van virtuele machines van Azure

U kunt fouten opsporen in programma's die worden uitgevoerd op Azure virtual machines via Server Explorer in Visual Studio. Wanneer u externe foutopsporing op een virtuele Azure-machine hebt ingeschakeld, wordt de extensie voor externe foutopsporing in Azure geïnstalleerd op de virtuele machine. U kunt vervolgens koppelen aan processen op de virtuele machine en fouten opsporen in zoals u gewend bent.

> [!NOTE]
> Virtuele machines die zijn gemaakt via de Azure resource manager-stack kan op afstand opsporen met behulp van Cloud Explorer in Visual Studio 2015. Zie voor meer informatie, [Azure-Resources beheren met Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Fouten opsporen in een virtuele machine van Azure

1. In Server Explorer, vouw het knooppunt voor virtuele Machines uit en selecteer het knooppunt van de virtuele machine die u wilt om op te sporen.

2. Open het contextmenu en selecteer **foutopsporing inschakelen**. Wanneer u wordt gevraagd of u zeker weet of u wilt inschakelen foutopsporing op de virtuele machine, selecteer **Ja**.

    Azure installeert de extensie voor externe foutopsporing op de virtuele machine om foutopsporing te schakelen.

    ![Virtuele machine foutopsporing-opdracht inschakelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Activiteitenlogboek van Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

3. Nadat de externe foutopsporing extensie is voltooid installeren, open het contextmenu van de virtuele machine en selecteer **foutopsporingsprogramma koppelen...**

    Azure haalt u een lijst van de processen op de virtuele machine en wordt deze weergegeven in het koppelen aan het dialoogvenster proces.

    ![Opdracht voor het foutopsporingsprogramma koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

4. In de **koppelen aan proces** in het dialoogvenster, selecteer **Selecteer** om te beperken van de lijst met resultaten om weer te geven alleen de typen van code die u wilt om op te sporen. U kunt fouten opsporen op 32-bits of 64-bits beheerde code, systeemeigen code of beide.

    ![Dialoogvenster voor code-type selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

5. Selecteer de processen die u wilt fouten opsporen op de virtuele machine en selecteer vervolgens **Attach**. U kunt bijvoorbeeld de w3wp.exe-proces selecteren als u wilt fouten opsporen in een web-app op de virtuele machine. Zie [fouten opsporen in een of meer processen in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) en [Azure Role Architecture](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) voor meer informatie.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Maken van een webproject en een virtuele machine voor het opsporen van fouten

Voordat u publiceert uw Azure-project, het wellicht handig zijn om deze te testen in een ingesloten omgeving die ondersteuning biedt voor foutopsporing en testen van scenario's en waar u testen en controleren of programma's kunt installeren. Er is één manier om uit te voeren van deze tests op afstand fouten opsporen in uw app op een virtuele machine.

Visual Studio-ASP.NET-projecten bieden een optie voor het maken van een handige virtuele machine die u voor het testen van de app gebruiken kunt. De virtuele machine bevat meestal benodigde eindpunten, zoals PowerShell, extern bureaublad en Web Deploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Maken van een webproject en een virtuele machine voor het opsporen van fouten

1. Maak een nieuwe ASP.NET-toepassing in Visual Studio.

2. Kies in het dialoogvenster Nieuw ASP.NET-Project in de sectie Azure **virtuele Machine** in de vervolgkeuzelijst. Laat de **maken van externe bronnen** selectievakje is ingeschakeld. Selecteer **OK** om door te gaan.

    De **virtuele machine maken op Azure** in het dialoogvenster wordt weergegeven.

    ![Dialoogvenster voor ASP.NET web-project maken](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Opmerking:** u wordt gevraagd om aan te melden bij uw Azure-account als u nog niet bent aangemeld.

3. Selecteer de verschillende instellingen voor de virtuele machine en selecteer vervolgens **OK**. Zie [virtuele Machines](http://go.microsoft.com/fwlink/?LinkId=623033) voor meer informatie.

    De naam die u voor DNS-naam invoert is de naam van de virtuele machine.

    ![Virtuele machine maken in het dialoogvenster Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure maakt de virtuele machine en bepalingen, en configureert u de eindpunten, zoals Extern bureaublad en Web Deploy

4. Nadat de virtuele machine volledig is geconfigureerd, selecteer u de virtuele machine-knooppunt in Server Explorer.

5. Open het contextmenu en selecteer **foutopsporing inschakelen**. Wanneer u wordt gevraagd of u zeker weet of u wilt inschakelen foutopsporing op de virtuele machine, selecteer **Ja**.

    Azure installeert de extensie voor externe foutopsporing voor de virtuele machine om de foutopsporing inschakelen.

    ![Virtuele machine foutopsporing-opdracht inschakelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Activiteitenlogboek van Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

6. Publiceren van uw project, zoals wordt beschreven in [hoe: een Web-Project met één muisklik publiceren in Visual Studio implementeren](https://msdn.microsoft.com/library/dd465337.aspx). Omdat u fouten opsporen op de virtuele machine wilt op de **instellingen** pagina van de **webpublicatie** wizard de optie **Debug** als de configuratie. Dit zorgt ervoor dat code symbolen beschikbaar tijdens het opsporen van fouten zijn.

    ![Publicatie-instellingen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

7. In de **opties voor het publiceren van bestand**, selecteer **aanvullende bestanden op de bestemming verwijderen** als het project is al geïmplementeerd op een eerder tijdstip.

8. Nadat het project wordt gepubliceerd, wordt in het contextmenu van de virtuele machine in Server Explorer selecteert **foutopsporingsprogramma koppelen...**

    Azure haalt u een lijst van de processen op de virtuele machine en wordt deze weergegeven in het koppelen aan het dialoogvenster proces.

    ![Opdracht voor het foutopsporingsprogramma koppelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

9. In de **koppelen aan proces** in het dialoogvenster, selecteer **Selecteer** om te beperken van de lijst met resultaten om weer te geven alleen de typen van code die u wilt om op te sporen. U kunt fouten opsporen op 32-bits of 64-bits beheerde code, systeemeigen code of beide.

    ![Dialoogvenster voor code-type selecteren](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

10. Selecteer de processen die u wilt fouten opsporen op de virtuele machine en selecteer vervolgens **Attach**. U kunt bijvoorbeeld de w3wp.exe-proces selecteren als u wilt fouten opsporen in een web-app op de virtuele machine. Zie [fouten opsporen in een of meer processen in Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* Gebruik **Intellitrace** voor het verzamelen van een logboek van aanroepen en gebeurtenissen van een release-server. Zie [opsporen van fouten in een gepubliceerde Cloudservice met IntelliTrace en Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016).

* Gebruik **Azure Diagnostics** om aan te melden gedetailleerde informatie van code die wordt uitgevoerd in rollen, of de rollen die worden uitgevoerd in de ontwikkelomgeving, of in Azure. Zie [gegevens voor logboekregistratie verzamelen met behulp van Azure Diagnostics](http://go.microsoft.com/fwlink/p/?LinkId=400450).
