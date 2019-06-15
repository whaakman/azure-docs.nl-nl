---
title: Werkstroom van Windows Azure-VM-architectuur | Microsoft Docs
description: Dit artikel bevat een overzicht van de werkstroomprocessen worden uitgevoerd wanneer u een service implementeert.
services: cloud-services
documentationcenter: ''
author: genlin
manager: Willchen
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/08/2019
ms.author: kwill
ms.openlocfilehash: 7c8459a6694663a49203b6ec21a760d3e6bd60c3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60480741"
---
#    <a name="workflow-of-windows-azure-classic-vm-architecture"></a>Werkstroom van Windows Azure classic VM-architectuur 
Dit artikel bevat een overzicht van de werkstroomprocessen die optreden wanneer u implementeert of bijwerken van een Azure-resource, zoals een virtuele machine. 

> [!NOTE]
>Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: Resource Manager en klassiek. Dit artikel gaat over het gebruik van het klassieke implementatiemodel.

Het volgende diagram toont de architectuur van Azure-resources.

![Azure-werkstroom](./media/cloud-services-workflow-process/workflow.jpg)

## <a name="workflow-basics"></a>Grondbeginselen van de werkstroom
   
**A**. RDFE / FFE is het communicatiepad van de gebruiker aan de infrastructuur. RDFE (RedDog Front-End) is de publiekelijk blootgestelde API die de front-end aan het beheerportal en de servicebeheer-API, zoals Visual Studio, Azure MMC, enzovoort.  Alle aanvragen van de gebruiker zijn doorlopen van RDFE. FFE (Fabric-Front-End) is de laag die aanvragen van RDFE in fabric-opdrachten vertaalt. Alle aanvragen van RDFE is doorlopen van de FFE bereiken van de infrastructuur domeincontrollers.

**B**. De infrastructuurcontroller is verantwoordelijk voor het onderhoud en bewaking van alle resources in het datacenter. Deze communiceert met fabric host agents op de OS het verzenden van informatie, zoals de Guest OS-versie, servicepakket, configuratie van de service en de status van service-infrastructuur.

**C**. De Host-Agent wordt toegevoegd aan de Host OSsystem en is verantwoordelijk voor het instellen van het Gastbesturingssysteem en communiceren met gast-Agent (WindowsAzureGuestAgent) om te kunnen bijwerken van de rol op een doelstatus van het beoogde en voer heartbeat gecontroleerd met de gastagent. Als de Host-Agent geen heartbeat antwoord ontvangt voor 10 minuten, Host-Agent opnieuw is opgestart het Gastbesturingssysteem.

**C2**. WaAppAgent is verantwoordelijk voor het installeren, configureren en bijwerken van WindowsAzureGuestAgent.exe.

**D**.  WindowsAzureGuestAgent is verantwoordelijk voor het volgende:

1. Configureren van het Gastbesturingssysteem waaronder firewall, ACL's, LocalStorage resources, service-pakket en configuratie van en certificaten. Instellen van de beveiligings-id voor het gebruikersaccount dat de rol wordt uitgevoerd onder.
2. De Rolstatus van de communiceert aan de infrastructuur.
3. WaHostBootstrapper begin- en toezicht om ervoor te zorgen dat de rol is in doel staat.

**E**. WaHostBootstrapper is verantwoordelijk voor:

1. Lezen van de configuratie van de functie en de juiste taken en processen voor het configureren en uitvoeren van de rol te starten.
2. Bewaking van alle onderliggende processen.
3. Het verhogen van de gebeurtenis StatusCheck op het hostproces van de rol.

**F**. IISConfigurator wordt uitgevoerd als de rol is geconfigureerd als een volledige IIS-Webrol (kan niet worden uitgevoerd voor de SDK 1.2 HWC). Het is verantwoordelijk voor:

1. De standaard IIS-services starten
2. Configureren van de module voor het herschrijven in de webconfiguratie
3. Instellen van de toepassingsgroep voor de geconfigureerde rol in het servicemodel
4. IIS-logboekregistratie instellen om te verwijzen naar de map DiagnosticStore LocalStorage
5. Configureren van machtigingen en ACL 's
6. De website zich bevindt in % roleroot%:\sitesroot\0 en de apppool verwijst naar deze locatie IIS uit te voeren. 

**G**. Opstarttaken zijn gedefinieerd door het model van de functie en door WaHostBootstrapper gestart. Opstarttaken kunnen worden geconfigureerd voor het asynchroon worden uitgevoerd op de achtergrond en de host bootstrapper wordt start de taak starten en ga vervolgens door u aan bij de andere taken gestart. Opstarttaken kunnen ook worden geconfigureerd om te worden uitgevoerd in de modus voor eenvoudig (standaard) waarin de host bootstrapper tot de opstarttaak wacht moet worden uitgevoerd en een afsluitcode geslaagd (0) geretourneerd voordat u doorgaat naar de volgende opstarttaak.

**H**. Deze taken maken deel uit van de SDK en zijn gedefinieerd als invoegtoepassingen in het definitie van de service van de rol (.csdef). Als uitgebreid naar opstarttaken, de **DiagnosticsAgent** en **RemoteAccessAgent** uniek zijn in die ze elke twee opstarttaken, een normale en met definiëren voor een **/blockStartup** parameter. De taak van de normale opstartprocedure wordt gedefinieerd als een achtergrondtaak opstarten zodat het kan op de achtergrond worden uitgevoerd terwijl de functie zelf wordt uitgevoerd. De **/blockStartup** opstarttaak wordt gedefinieerd als een eenvoudige opstarttaak zodat WaHostBootstrapper wacht tot het om af te sluiten voordat u doorgaat. De **/blockStartup** taak wacht totdat de reguliere taak te voltooien tijdens de initialisatie van, en vervolgens deze wordt afgesloten en toestaan dat de host bootstrapper om door te gaan. Dit wordt gedaan zodat diagnostische gegevens en RDP-toegang kunnen worden geconfigureerd voordat de rol processen gestart (dit wordt gedaan door de taak /blockStartup). Zo kan diagnostische gegevens en RDP-toegang te blijven uitvoeren nadat de host bootstrapper de opstarttaken (dit wordt gedaan door de normale taak) is voltooid.

**I**. WaWorkerHost is het standaard host-proces voor normale werkrollen. Dit hostproces als host fungeert voor de dll-bestanden en de vermelding punt code, zoals OnStart en uitvoeren van de rol.

**J**. WaWebHost is het standaard host-proces voor webrollen als ze zijn geconfigureerd voor het gebruik van de SDK 1.2-compatibele Hostable Web Core HWC (). Rollen kunnen de HWC-modus inschakelen door het verwijderen van het element uit de definitie van de service (.csdef). In deze modus wordt uitvoeren de code en .dll-bestanden van de service vanaf het WaWebHost-proces. IIS (w3wp) wordt niet gebruikt en er zijn geen AppPools geconfigureerd in IIS-beheer, omdat IIS wordt gehost binnen WaWebHost.exe.

**K**. WaIISHost is het hostproces voor de rol van vermelding punt code voor webrollen die volledige IIS gebruiken. Dit proces wordt geladen voor het eerste dll-bestand dat is gevonden die gebruikmaakt van de **RoleEntryPoint** klasse en de code van deze klasse (OnStart, uitvoering, OnStop) wordt uitgevoerd. Alle **RoleEnvironment** gebeurtenissen (zoals StatusCheck en gewijzigd) die zijn gemaakt in de klasse RoleEntryPoint in dit proces worden gegenereerd.

**L**. W3wp is de standaard IIS-werkproces dat wordt gebruikt als de rol is geconfigureerd voor het volledige IIS gebruiken. Hiermee wordt de AppPool die is geconfigureerd uit IISConfigurator uitgevoerd. Alle RoleEnvironment-gebeurtenissen (zoals StatusCheck en gewijzigd), die hier worden gemaakt worden in dit proces gegenereerd. Houd er rekening mee dat RoleEnvironment gebeurtenissen worden als u zich op gebeurtenissen in beide processen abonneert op beide locaties (WaIISHost en w3wp.exe) wordt gestart.

## <a name="workflow-processes"></a>Werkstroomprocessen worden uitgevoerd

1. Een gebruiker maakt een aanvraag, zoals .cspkg en .cscfg-bestanden uploaden, waarin een resource om te stoppen of maken van een wijziging in de configuratie, enzovoort. Dit kan worden gedaan via de Azure portal of een hulpprogramma dat gebruikmaakt van de servicebeheer-API, zoals de functie voor het publiceren van Visual Studio. Deze aanvraag wordt gerouteerd naar RDFE doen alle de abonnement-gerelateerde werken en vervolgens de aanvraag voor het FFE communiceren. De overige werkstroomstappen zijn het implementeren van een nieuw pakket en start de service.
2. FFE zoekt naar de juiste Machinegroep (op basis van de klant invoer, als de affiniteitsgroep of geografische locatie en invoer van de infrastructuur, zoals de beschikbaarheid van computers) en communiceert met de master-infrastructuurcontroller in de opslaggroep die machine.
3. De infrastructuurcontroller vindt u een host die de beschikbare CPU-kernen (of fabriceert-up maken van een nieuwe host). De service-pakket en de configuratie wordt gekopieerd naar de host en de infrastructuurcontroller communiceert met de host-agent op de host OS aan het pakket implementeren (Spanningsdips, poorten en gastbesturingssysteem configureren).
4. De host-agent begint het Gastbesturingssysteem en communiceert met de gastagent (WindowsAzureGuestAgent). De host verzendt heartbeats naar de Gast om ervoor te zorgen dat de rol wordt gewerkt aan de doelstatus.
5. WindowsAzureGuestAgent stelt u het gastbesturingssysteem te installeren (firewall, ACL's, LocalStorage, enzovoort), een nieuwe XML-configuratiebestand worden gekopieerd naar c:\Config en start het proces WaHostBootstrapper.
6. Voor volledige IIS-webrollen, WaHostBootstrapper IISConfigurator wordt gestart en wordt het verwijderen van alle bestaande AppPools niet plannen voor de Webrol in IIS.
7. WaHostBootstrapper leest de **opstarten** taken van E:\RoleModel.xml en opstarttaken actief. WaHostBootstrapper moet wachten tot alle eenvoudige opstarttaken hebt voltooid en een bericht "geslaagd" geretourneerd.
8. Voor volledige IIS-webrollen, WaHostBootstrapper IISConfigurator het configureren van de IIS-toepassingsgroep staat en wijst u de site `E:\Sitesroot\<index>`, waarbij `<index>` is een op basis van 0-index in het aantal <Sites> elementen die zijn gedefinieerd voor de service.
9. WaHostBootstrapper wordt het hostproces, afhankelijk van het Roltype starten:
    1. **Werkrol**: WaWorkerHost.exe is gestart. WaHostBootstrapper wordt de methode OnStart() uitgevoerd. Nadat het resultaat, WaHostBootstrapper begint de methode Run() uit te voeren en vervolgens tegelijkertijd markeert de rol als gereed en plaatst deze in de load balancer-rotatie (als InputEndpoints zijn gedefinieerd). WaHostBootsrapper wordt vervolgens in een lus van de status van de gebruikersrol controleren.
    1. **Web SDK 1.2 HWC-rol**: WaWebHost is gestart. WaHostBootstrapper wordt de methode OnStart() uitgevoerd. Nadat het resultaat, WaHostBootstrapper wordt gestart voor het uitvoeren van de methode Run() en tegelijkertijd markeert vervolgens de rol als gereed en de load balancer-rotatie geplaatst. WaWebHost Hiermee wordt een aanvraag voor opwarming (GET-/do.rd_runtime_init). Alle webservice-aanvragen worden verzonden naar WaWebHost.exe. WaHostBootsrapper wordt vervolgens in een lus van de status van de gebruikersrol controleren.
    1. **IIS-Webrol volledige**: aIISHost is gestart. WaHostBootstrapper wordt de methode OnStart() uitgevoerd. Nadat het resultaat, begint de methode Run() uit te voeren en tegelijkertijd markeert vervolgens de rol als gereed en de load balancer-rotatie geplaatst. WaHostBootsrapper wordt vervolgens in een lus van de status van de gebruikersrol controleren.
10. Binnenkomende webaanvragen naar een volledige IIS web rol triggers IIS het W3WP-proces starten en de aanvraag, hetzelfde als in een on-premises IIS-omgeving.

## <a name="log-file-locations"></a>Locaties logboekbestand

**WindowsAzureGuestAgent**

- C:\Logs\AppAgentRuntime.log.  
Dit logboek bevat wijzigingen in de service wordt gestart, gestopt en nieuwe configuraties. Als de service niet wordt gewijzigd, kunt u verwachten grote lacunes in de tijd in dit logboekbestand.
- C:\Logs\WaAppAgent.Log.  
Dit logboek bevat statusupdates en heartbeat meldingen en elke 2-3 seconden wordt bijgewerkt.  Dit logboek bevat een historisch overzicht van de status van het exemplaar en laat u weten wanneer het exemplaar is niet de status gereed heeft.
 
**WaHostBootstrapper**

C:\Resources\Directory\<deploymentID>.<role>.DiagnosticStore\WaHostBootstrapper.log
 
**WaWebHost**

C:\Resources\Directory\<guid>.<role>\WaWebHost.log
 
**WaIISHost**

C:\Resources\Directory\<deploymentID>.<role>\WaIISHost.log
 
**IISConfigurator**

C:\Resources\Directory\<deploymentID>.<role>\IISConfigurator.log
 
**IIS-logboeken**

C:\Resources\Directory\<guid>.<role>.DiagnosticStore\LogFiles\W3SVC1
 
**Windows-gebeurtenislogboeken**

D:\Windows\System32\Winevt\Logs
 



