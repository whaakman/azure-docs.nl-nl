---
title: Instellen van diagnostische gegevens voor Azure Cloud Services en virtuele machines | Microsoft Docs
description: Informatie over het instellen van diagnostische gegevens voor het opsporen van Azure cloude services en virtuele machines (VM's) in Visual Studio.
services: visual-studio-online
documentationcenter: na
author: mikejo
manager: ghogen
editor: 
ms.assetid: e70cd7b4-6298-43aa-adea-6fd618414c26
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: mikejo
ms.openlocfilehash: f00771d89749e7507d7f303f366fe63f537900ff
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="set-up-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Instellen van diagnostische gegevens voor Azure Cloud Services en virtuele machines
Wanneer u problemen met een Azure-cloudservice of virtuele machine, kunt u Visual Studio kunt gebruiken voor het eenvoudig instellen van Azure Diagnostics. Diagnostische gegevens worden vastgelegd system en logboekregistratie-gegevens op de virtuele machines en de virtuele machine-exemplaren die worden uitgevoerd van de cloudservice. Diagnostische gegevens worden overgedragen naar een opslagaccount die u kiest. Zie voor meer informatie over diagnostische gegevens van logboekregistratie in Azure, [logboekregistratie van diagnostische gegevens van Web-Apps in Azure App Service](app-service/web-sites-enable-diagnostic-log.md).

In dit artikel wordt beschreven hoe u Visual Studio gebruiken om te schakelen en het instellen van Azure Diagnostics, zowel voor als na de implementatie. Informatie over het instellen van diagnostische gegevens op virtuele machines in Azure, het selecteren van de soorten informatie voor het verzamelen van diagnostische gegevens en hoe de gegevens bekijken nadat deze zijn verzameld.

U kunt een van de volgende opties gebruiken voor het instellen van Azure Diagnostics:

* Wijzig de instellingen voor diagnostische gegevens in de **configuratie van diagnostische** dialoogvenster in Visual Studio. De instellingen worden opgeslagen in een bestand met de naam diagnostics.wadcfgx (in de Azure SDK 2.4 en eerder, het bestand heet diagnostics.wadcfg). U ook het configuratiebestand rechtstreeks kunnen worden gewijzigd. Als u handmatig het bestand bijwerkt, wordt de configuratie wijzigingen pas van kracht zodra u de cloud implementeren naar Azure-service of de service wordt uitgevoerd in de emulator.
* Cloud Explorer of Server Explorer in Visual Studio gebruiken om te wijzigen van de diagnostische instellingen voor een cloudservice of virtuele machine die wordt uitgevoerd.

## <a name="azure-sdk-26-diagnostics-changes"></a>Wijzigingen van Azure SDK 2.6 diagnostische gegevens
De volgende wijzigingen toepassen op de Azure SDK 2.6 en hoger projecten in Visual Studio:

* De lokale emulator ondersteunt nu de diagnostische gegevens. Dit betekent dat u kunt verzamelen van diagnostische gegevens en ervoor te zorgen dat de toepassing de juiste traceringen maakt tijdens het ontwikkelen en testen in Visual Studio. De verbindingsreeks `UseDevelopmentStorage=true` verzamelen van diagnostische gegevens ingeschakeld tijdens het uitvoeren van uw cloud service-project in Visual Studio met behulp van de Azure-opslagemulator. Alle diagnostische gegevens worden verzameld in de opslag van ontwikkeling opslagaccount.
* De diagnostische gegevens storage account connection string `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` wordt opgeslagen in het service-configuratiebestand (.cscfg). In de Azure SDK 2.5, is het opslagaccount voor diagnostische gegevens in het bestand diagnostics.wadcfgx opgegeven.

De verbindingsreeks werkt anders in enkele belangrijke manieren in de Azure SDK 2.6 en hoger ten opzichte van de Azure SDK 2.4 en eerdere versies:

* In Azure SDK 2.4 en eerdere versies, wordt de verbindingsreeks gebruikt als een runtime die door de invoegtoepassing diagnostische gegevens ophalen van de accountgegevens voor de opslag voor het overdragen van diagnostische logboeken.
* In Azure SDK 2.6 en hoger, Visual Studio maakt gebruik van de verbindingsreeks van diagnostische gegevens voor het instellen van de extensie voor diagnostische gegevens van Azure met de juiste opslag gegevens tijdens de publicatie. U kunt de verbindingsreeks gebruiken voor het definiëren van verschillende opslagaccounts voor verschillende configuraties die Visual Studio tijdens de publicatie gebruikt. Echter, omdat de invoegtoepassing diagnostische gegevens niet beschikbaar is nadat de Azure SDK 2.5 het cscfg-bestand zelf niet instellen extensie voor diagnostische gegevens. U moet de extensie apart instellen met behulp van hulpprogramma's zoals Visual Studio of PowerShell.
* Om te vereenvoudigen het proces voor het instellen van de extensie voor diagnostische gegevens met behulp van PowerShell, bevat de uitvoer van het pakket van Visual Studio de openbare configuratie-XML voor de extensie voor diagnostische gegevens voor elke rol. De verbindingsreeks diagnostics Visual Studio gebruikt voor het vullen van de accountgegevens voor de opslag in de openbare configuratie. De openbare configuratiebestanden worden gemaakt in de map extensies. De openbare configuratiebestanden gebruiken het naamgevingspatroon PaaSDiagnostics. &lt;rolnaam\>. PubConfig.xml. PowerShell-implementaties kunnen dit patroon gebruiken voor elke configuratie worden toegewezen aan een rol.
* De [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) maakt gebruik van de verbindingsreeks voor toegang tot de diagnostics-gegevens in het .cscfg-bestand. De gegevens worden weergegeven op de **bewaking** tabblad. De verbindingsreeks is vereist voor het instellen van de service naar de uitgebreide bewakingsgegevens weergeven in de portal.

## <a name="migrate-projects-to-azure-sdk-26-and-later"></a>Migreren van de projecten op Azure SDK 2.6 en hoger
Wanneer u van Azure SDK 2.5 naar Azure SDK 2.6 of hoger, migreert als u een opslagaccount voor diagnostische gegevens opgegeven in het bestand .wadcfgx had, blijft het storage-account in dat bestand. Als u wilt profiteren van de flexibiliteit van het gebruik van verschillende storage-accounts voor opslagconfiguraties verschillende, moet u handmatig de verbindingsreeks toevoegen aan uw project. Als u een project waarnaar u migreert van Azure SDK 2.4 of eerder in Azure SDK 2.6, blijven de verbindingsreeksen van diagnostische gegevens behouden. Let echter op de wijzigingen in hoe verbindingsreeksen worden behandeld in de Azure SDK 2.6, beschreven in de vorige sectie.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hoe de opslagaccount voor diagnostische gegevens in Visual Studio wordt bepaald
* Als een verbindingsreeks diagnostische gegevens in het .cscfg-bestand is opgegeven, wordt het in Visual Studio gebruikt voor het instellen van de extensie voor diagnostische gegevens tijdens de publicatie en bij het genereren van de openbare configuratie-XML-bestanden tijdens pakketten.
* Als u een verbindingsreeks diagnostics is niet opgegeven in het .cscfg-bestand, terugvalt Visual Studio op het gebruik van het opslagaccount dat opgegeven in het bestand .wadcfgx voor het instellen van de extensie voor diagnostische gegevens voor publicatie en voor het genereren van de openbare configuratie-XML bestanden tijdens pakketten.
* De verbindingsreeks voor diagnostische gegevens in het .cscfg-bestand heeft voorrang op de storage-account in het bestand .wadcfgx. Als een verbindingsreeks diagnostics is opgegeven in het .cscfg-bestand, Visual Studio maakt gebruik van deze verbindingsreeks en negeert het opslagaccount in .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-check-box-do"></a>Wat is het selectievakje 'Bijwerken ontwikkeling storage-verbindingsreeksen...'?
De **bijwerken van ontwikkeling storage-verbindingsreeksen voor diagnostische gegevens en opslaan in cache met Microsoft Azure storage-accountreferenties bij het publiceren van Microsoft Azure** selectievakje is een handige manier om bij te werken van alle opslag ontwikkeling account verbinding tekenreeksen met de Azure storage-account die u tijdens de publicatie opgeeft.

Als u dit selectievakje in en de verbindingsreeks diagnostics selecteert geeft bijvoorbeeld `UseDevelopmentStorage=true`, wanneer u het project naar Azure publiceren, Visual Studio automatisch de diagnostics-verbindingsreeks met het opslagaccount dat u hebt opgegeven in de wizard publiceren. Echter, als een echte storage-account is opgegeven als de verbindingsreeks van diagnostische gegevens, dat account wordt gebruikt in plaats daarvan.

## <a name="diagnostics-functionality-differences-in-azure-sdk-24-and-earlier-vs-azure-sdk-25-and-later"></a>Diagnostische functionaliteit verschillen in de Azure SDK 2.4 en eerdere vs. Azure SDK 2,5 en hoger
Als u een uw project van Azure SDK 2.4 upgrade uitvoert en eerder naar de Azure SDK 2.5 of hoger, houd rekening met de volgende verschillen van de diagnostics-functionaliteit:

* **Configuratie-API's zijn afgeschaft**. Programmatische configuratie van diagnostische gegevens is beschikbaar in de Azure SDK 2.4 en eerder, maar is afgeschaft in Azure SDK 2.5 en hoger. Als uw configuratie van diagnostische gegevens op dit moment is gedefinieerd in de code, moet u die instellingen vanaf het begin in het project gemigreerde voor diagnostische gegevens blijven werken opnieuw configureren. Het configuratiebestand van de diagnostische gegevens voor Azure SDK 2.4 is diagnostics.wadcfg. De diagnostics-configuratiebestand voor de Azure SDK 2.5 of hoger is diagnostics.wadcfgx.
* **Diagnostische gegevens voor cloud service-toepassingen kunnen worden geconfigureerd op het rolniveau van de**. In de Azure SDK 2.5 en hoger, instellen niet u diagnostische gegevens voor cloudtoepassingen service op instantieniveau.
* **Telkens wanneer u uw app implementeert, wordt de configuratie van diagnostische bijgewerkt**. Dit kan pariteit problemen veroorzaken als u de configuratie van de diagnostische gegevens van Visual Studio Server Explorer te wijzigen en vervolgens uw app te implementeren.
* **In de Azure SDK 2.5 en hoger crashdumps zijn geconfigureerd in het configuratiebestand van de diagnostische gegevens en niet in code**. Als u crashdumps geconfigureerd in de code hebt, moet u handmatig de configuratie van code overbrengen naar het configuratiebestand. Crashdumps worden niet tijdens de migratie naar Azure SDK 2.6 overgedragen.

## <a name="turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them"></a>Diagnostische gegevens in cloudserviceprojecten inschakelen voordat u ze implementeren
U kunt diagnostische gegevens voor rollen die worden uitgevoerd in Azure wanneer u de service in de emulator vóór de implementatie uitvoert verzamelen in Visual Studio. Alle wijzigingen in de diagnostische instellingen in Visual Studio zijn opgeslagen in het configuratiebestand diagnostics.wadcfgx. Deze instellingen bepalen het opslagaccount waar diagnostische gegevens worden opgeslagen wanneer u uw cloudservice implementeert.

[!INCLUDE [cloud-services-wad-warning](../includes/cloud-services-wad-warning.md)]

### <a name="to-turn-on-diagnostics-in-visual-studio-before-deployment"></a>Diagnostische gegevens in Visual Studio vóór de implementatie inschakelen

1. Selecteer in het snelmenu voor de rol **eigenschappen**. In de rol **eigenschappen** selecteert u de **configuratie** tabblad.
2. In de **Diagnostics** sectie, zorg ervoor dat de **diagnostische gegevens inschakelen** selectievakje is ingeschakeld.
   
    ![Toegang tot de optie diagnostische gegevens inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)
3. Als u het opslagaccount voor diagnostische gegevens, schakelt u de knop met het weglatingsteken (...).
   
    ![Geef het storage-account gebruiken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)
4. In de **Create Storage Connection String** dialoogvenster vak, opgeven of u verbinding maken wilt met behulp van de Azure-opslagemulator een Azure-abonnement of referenties handmatig worden ingevoerd.
   
    ![Dialoogvenster Storage-account](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)
   
   * Als u selecteert **Microsoft Azure-opslagemulator**, de verbindingsreeks is ingesteld op `UseDevelopmentStorage=true`.
   * Als u selecteert **uw abonnement**, selecteert u de Azure-abonnement dat u wilt gebruiken en voer een accountnaam op. Selecteer voor het beheren van uw Azure-abonnementen **Accounts beheren**.
   * Als u selecteert **referenties handmatig worden ingevoerd**, voer de naam en sleutel van het Azure-account die u wilt gebruiken.
5. Om weer te geven de **configuratie van diagnostische** dialoogvenster, **configureren**. Met uitzondering van **algemene** en **logboek mappen**, elk tabblad vertegenwoordigt een diagnostische gegevens van een gegevensbron die u kunt verzamelen. De standaardwaarde **algemene** tabblad biedt diagnostische gegevens over de volgende opties voor het verzamelen van gegevens: **alleen fouten**, **alle informatie**, en **aangepaste plan**. De standaardwaarde **alleen fouten** optie maakt gebruik van de minimale hoeveelheid opslag, omdat deze geen waarschuwingen of tracering berichten over te dragen. De **alle informatie** de meeste gegevens worden overgebracht, de meest opslag gebruikt en daarom is de meest dure optie.
   
    ![Azure diagnostics- en configuratie inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
6. Selecteer voor dit voorbeeld wordt de **aangepaste plan** optie, zodat u de verzamelde gegevens kunt aanpassen.
7. In de **schijfquotum in MB** vak kunt u instellen hoeveel ruimte toe te wijzen in uw opslagaccount voor diagnostische gegevens. U kunt wijzigen of accepteer de standaardwaarde.
8. Op elk tabblad van diagnostische gegevens die u wilt verzamelen, selecteer de **inschakelen overdragen van \<type logboek registreren\>**  selectievakje. Bijvoorbeeld, als u wenst te verzamelen van toepassingslogboeken, op de **toepassingslogboeken** tabblad de **inschakelen van de overdracht van toepassingslogboeken** selectievakje. Geef ook andere informatie die vereist voor elk gegevenstype diagnostische gegevens. Zie de sectie voor configuratie-informatie voor elk tabblad **diagnostics gegevensbronnen instellen** verderop in dit artikel. 
9. Nadat u de verzameling van alle diagnostics-gegevens die u hebt ingeschakeld, selecteert u **OK**.
10. Uw Azure-cloud service-project in Visual Studio gewoon uitgevoerd. Als u uw toepassing gebruikt, wordt de logboekgegevens die u hebt ingeschakeld opgeslagen in de Azure storage-account die u hebt opgegeven.

## <a name="turn-on-diagnostics-on-azure-virtual-machines"></a>Diagnostische gegevens op virtuele machines van Azure inschakelen
U kunt voor virtuele machines van Azure diagnostics-gegevens verzamelen in Visual Studio.

### <a name="to-turn-on-diagnostics-on-azure-virtual-machines"></a>Diagnostische gegevens op virtuele machines van Azure inschakelen

1. Selecteer het Azure-knooppunt in Server Explorer en maak verbinding met uw Azure-abonnement als u nog niet bent verbonden.
2. Vouw de **virtuele Machines** knooppunt. U kunt een nieuwe virtuele machine maken, of Selecteer een bestaand knooppunt.
3. Selecteer in het snelmenu voor de virtuele machine die u wilt dat **configureren**. Het dialoogvenster configuratie van virtuele machine wordt weergegeven.
   
    ![Een virtuele machine van Azure configureren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)
4. Als deze nog niet is geïnstalleerd, moet u de Microsoft Monitoring Agent Diagnostics-extensie toevoegen. U kunt met deze uitbreiding voor de virtuele machine van Azure diagnostics-gegevens verzamelen. Onder **extensies geïnstalleerd**, in de **selecteert u een beschikbare extensie** Selecteer vervolgkeuzelijst in **Microsoft Monitoring Agent Diagnostics**.
   
    ![De uitbreiding voor een virtuele machine van Azure installeren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)
   
    > [!NOTE]
   > Andere extensies diagnostische gegevens zijn beschikbaar voor uw virtuele machines. Zie voor meer informatie [uitbreidingen van de virtuele machine en functies voor Windows](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).
   > 
   > 
5. Toevoegen van de extensie en bekijk de **configuratie van diagnostische** dialoogvenster, **toevoegen**.
6. Als u wilt opgeven van een opslagaccount **configureren**, en selecteer vervolgens **OK**.
   
    Elk tabblad (met uitzondering van **algemene** en **logboek mappen**) vertegenwoordigt een diagnostische gegevens van een gegevensbron die u kunt verzamelen.
   
    ![Azure diagnostics- en configuratie inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
   
    Het standaardtabblad **algemene**, biedt de volgende opties van diagnostische gegevens verzameling: **alleen fouten**, **alle informatie**, en **aangepaste plan**. De standaardoptie **alleen fouten**, zo min mogelijk opslag vereist omdat deze geen waarschuwingen of tracering berichten over te dragen. De **alle informatie** optie de meeste gegevens worden overgebracht en is daarom de optie meest kostbaar in termen van opslag.
7. Selecteer voor dit voorbeeld wordt de **aangepaste plan** optie zodat u de gegevens kunt worden verzameld.
8. De **schijfquotum in MB** vak geeft aan hoeveel ruimte u in uw opslagaccount voor diagnostische gegevens wilt toewijzen. U kunt de standaardwaarde als u wilt wijzigen.
9. Op elk tabblad van diagnostische gegevens u wilt verzamelen, selecteer de **inschakelen overdragen van \<type logboek registreren\>**  selectievakje.
   
    Bijvoorbeeld, als u wenst te verzamelen van toepassingslogboeken, selecteert u de **inschakelen van de overdracht van toepassingslogboeken** selectievakje op het **toepassingslogboeken** tabblad. Geef ook andere informatie die zijn voor elk gegevenstype diagnostische gegevens vereist. Zie de sectie voor configuratie-informatie voor elk tabblad **diagnostics gegevensbronnen instellen** verderop in dit artikel.
10. Nadat u de verzameling van alle gegevens van diagnostische gegevens die u nodig hebt ingeschakeld, selecteert u **OK**.
11. Sla het bijgewerkte project.
    
    Een bericht in de **Microsoft Azure Activity Log** venster geeft aan dat de virtuele machine is bijgewerkt.

## <a name="set-up-diagnostics-data-sources"></a>Diagnostische gegevens gegevensbronnen instellen
Nadat u het verzamelen van diagnostische gegevens inschakelen, kunt u precies welke gegevensbronnen die u wenst te verzamelen en welke informatie wordt verzameld. De volgende secties beschrijven de tabbladen in de **configuratie van diagnostische** in het dialoogvenster en elke configuratieoptie betekent.

### <a name="application-logs"></a>Toepassingslogboeken
Toepassingslogboeken zijn diagnostische gegevens die wordt geproduceerd door een webtoepassing. Als u vastleggen toepassingslogboeken wilt, selecteert u de **inschakelen van de overdracht van toepassingslogboeken** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van toepassingslogboeken naar uw opslagaccount, wijzigen de **overdracht periode (min)** waarde. Ook kunt u de hoeveelheid gegevens die zijn vastgelegd in het logboek door in te stellen de **Meld niveau** waarde. Selecteer bijvoorbeeld **uitgebreid** meer informatie, of selecteer **Kritiek** om vast te leggen alleen kritieke fouten. Als u een specifieke diagnostics-provider die u toepassingslogboeken verzendt hebt, kunt u de logboeken vastleggen door toe te voegen van de provider de GUID in de **Provider GUID** vak.

  ![Toepassingslogboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

Zie voor meer informatie over toepassingslogboeken [logboekregistratie van diagnostische gegevens van Web-Apps in Azure App Service](app-service/web-sites-enable-diagnostic-log.md).

### <a name="windows-event-logs"></a>Windows-gebeurtenislogboeken
Voor het vastleggen van Windows-gebeurtenislogboeken, selecteer de **overdracht van het Windows-gebeurtenislogboeken inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van logboeken naar uw opslagaccount, wijzigen de **overdracht periode (min)** waarde. Schakel de selectievakjes voor de typen gebeurtenissen die u wilt traceren.

![Gebeurtenislogboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Als u Azure SDK 2.6 of hoger en u wilt een aangepaste gegevensbron opgeven, typt u dit in de  **\<gegevensbronnaam\>**  tekstvak in en selecteer vervolgens **toevoegen**. De gegevensbron is toegevoegd aan het bestand diagnostics.cfcfg.

Als u Azure SDK 2.5 en geef een aangepaste gegevensbron wilt, kunt u het toevoegen aan de `WindowsEventLog` sectie van de diagnostics.wadcfgx-bestand, zoals in het volgende voorbeeld:

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Prestatiemeteritems
Gegevens van prestatiemeteritems kunt u vinden knelpunten in het systeem en systeem- en -prestaties afstemmen. Zie voor meer informatie [prestatiemeteritems maken en gebruiken in een Azure-toepassing](https://msdn.microsoft.com/library/azure/hh411542.aspx). Voor het vastleggen van prestatiemeteritems, selecteer de **overdracht van prestatiemeteritems inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van logboeken naar uw opslagaccount, wijzigen de **overdracht periode (min)** waarde. Schakel de selectievakjes voor de prestatiemeteritems die u wilt traceren.

![Prestatiemeteritems](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Voer het prestatiemeteritem met behulp van de voorgestelde syntaxis voor het bijhouden van een prestatiemeteritem die niet is vermeld. en selecteer vervolgens **toevoegen**. Het besturingssysteem op de virtuele machine bepaalt welke prestatiemeteritems die u kunt volgen. Zie voor meer informatie over de syntaxis [Geef een pad van de teller](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Logboeken van de infrastructuur
Logboeken van de infrastructuur bevat gegevens over de diagnostische Azure-infrastructuur, de RemoteAccess-module en de RemoteForwarder-module. Voor het verzamelen van informatie over de logboeken van de infrastructuur, selecteer de **overdracht van infrastructuur Logboeken inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van infrastructuur logboeken naar uw opslagaccount, wijzigen de **overdracht periode (min)** waarde.

![Diagnostische gegevens infrastructuur Logboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

Zie voor meer informatie [logboekgegevens verzamelen met behulp van Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="log-directories"></a>Logboek mappen
Logboek mappen hebt gegevens verzameld van logboek-mappen voor Internet Information Services (IIS)-aanvragen, mislukte aanvragen of mappen die u kiest. Voor het vastleggen van logboek-mappen, selecteer de **overdracht van logboek mappen inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van logboeken naar uw opslagaccount, wijzigen de **overdracht periode (min)** waarde.

Schakel de selectievakjes van de logboeken die u verzamelen wilt, zoals **IIS-logboeken** en **kan aanvragen** Logboeken. Standaard opslag containernamen worden opgegeven, maar u kunt de namen wijzigen.

U kunt de logboeken van een willekeurige map vastleggen. Geef het pad in de **logboek van Directory Absolute** sectie en selecteer vervolgens **map toevoegen**. De logboeken worden vastgelegd in de opgegeven containers.

![Logboek mappen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW-Logboeken
Als u [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803\(v=vs.85\).aspx) (ETW) en wilt vastleggen ETW-Logboeken, selecteer de **overdracht van ETW-Logboeken inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van logboeken naar uw opslagaccount, wijzigen de **overdracht periode (min)** waarde.

De gebeurtenissen worden vastgelegd van bronnen van gebeurtenissen en het gebeurtenismanifesten die u opgeeft. Een gebeurtenisbron opgeven in de **gebeurtenisbronnen** sectie, voer een naam in en selecteer vervolgens **gebeurtenisbron toevoegen**. Op deze manier kunt u een manifest van de gebeurtenis in de **gebeurtenis manifesten** sectie en selecteer vervolgens **toevoegen gebeurtenis Manifest**.

![ETW-Logboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

Het ETW-framework in ASP.NET wordt ondersteund door de klassen in de [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) naamruimte. De Microsoft.WindowsAzure.Diagnostics naamruimte overneemt en breidt standaard [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) klassen, maakt het gebruik van [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) als een logboekregistratie Framework in de Azure-omgeving. Zie voor meer informatie [overnemen van logboekregistratie en tracering in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) en [diagnostische gegevens in Azure Cloud Services en virtuele machines inschakelen](cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Crashdumps
Voor het vastleggen van informatie over wanneer een rolinstantie is vastgelopen, selecteer de **inschakelen van de overdracht van Crash dumpen** selectievakje. (Omdat ASP.NET de meeste uitzonderingen verwerkt, dit is doorgaans alleen nuttig voor werkrollen.) Als u wilt vergroten of verkleinen van het percentage van de opslagruimte die aan de crashdumps besteed, wijzigen de **Directory quotum (%)** waarde. U kunt de opslagcontainer waarin de crashdumps worden opgeslagen en selecteer of u wilt vastleggen wijzigen een **volledige** of **Mini** dump.

De processen die momenteel worden bijgehouden, worden vermeld in de volgende schermafbeelding. Schakel de selectievakjes voor de processen die u wilt vastleggen. Een ander proces toevoegen aan de lijst, voer de procesnaam en selecteer vervolgens **proces toevoegen**.

![Crashdumps](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

Zie voor meer informatie [overnemen van logboekregistratie en tracering in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) en [Microsoft Azure Diagnostics deel 4: aangepaste logboekregistratie onderdelen en Azure Diagnostics 1.3 wijzigingen](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/).

## <a name="view-the-diagnostics-data"></a>De diagnostics-gegevens weergeven
Als u de diagnostische gegevens voor een cloudservice of virtuele machine hebt verzameld, kunt u het kunt weergeven.

### <a name="to-view-cloud-service-diagnostics-data"></a>Cloud service diagnostische gegevens weergeven
1. Uw cloudservice gebruikelijke manier te implementeren en vervolgens uit te voeren.
2. U kunt de diagnostics-gegevens weergeven in een rapport dat door Visual Studio gegenereerd of in de tabellen in uw opslagaccount. Naar de weergave de gegevens in een rapport openen Cloud Explorer of Server Explorer, open het snelmenu van het knooppunt voor de rol die u wilt gebruiken en selecteer vervolgens **weergave diagnostische gegevens**.
   
    ![De diagnostics-gegevens weergeven](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)
   
    Een rapport met de beschikbare gegevens wordt weergegeven.
   
    ![Microsoft Azure Diagnostics-rapport in Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)
   
    Als de meest recente gegevens niet wordt weergegeven, moet u wellicht wacht u totdat de overdracht is verstreken.
   
    Als u wilt de gegevens direct bijwerken, selecteert u de **vernieuwen** koppeling. Als u de gegevens automatisch bijgewerkt, selecteert u een interval in de **automatisch vernieuwen** keuzelijst met invoervak. De om foutgegevens te exporteren, selecteer de **exporteren naar CSV** om te maken van een bestand met door komma's gescheiden waarden die u in een Excel-werkblad openen kunt.
   
    Open in de Cloud Explorer of Server Explorer, het opslagaccount dat is gekoppeld aan de implementatie.
3. Open de diagnostics-tabellen in de tabel viewer en controleer de gegevens die u hebt verzameld. U kunt een blob-container openen voor IIS-logboeken en aangepaste logboeken. De volgende tabel bevat de tabellen of blob-containers die de gegevens voor de verschillende logboekbestanden bevatten. Naast de gegevens voor die logboekbestand items in de tabel bevatten **EventTickCount**, **DeploymentId**, **rol**, en **RoleInstance** , om te identificeren welke virtuele machine en de rol van de gegevens gegenereerd en wanneer. 
   
   | Diagnostische gegevens | Beschrijving | Locatie |
   | --- | --- | --- |
   | Toepassingslogboeken |Logboeken die uw code wordt gegenereerd door het aanroepen van methoden van de **System.Diagnostics.Trace** klasse. |WADLogsTable |
   | Gebeurtenislogboeken |Gegevens uit de Windows-gebeurtenislogboeken op de virtuele machines. Slaat Windows de informatie in deze logboeken, maar toepassingen en services ook gebruik van de logboeken fouten rapporteren of logboekgegevens. |WADWindowsEventLogsTable |
   | Prestatiemeteritems |U kunt gegevens verzamelen op elk prestatiemeteritem dat beschikbaar is op de virtuele machine. Het besturingssysteem biedt prestatiemeteritems, waaronder meer statistische gegevens, zoals geheugen en de processor tijd. |WADPerformanceCountersTable |
   | Logboeken van de infrastructuur |De logboeken die zijn gegenereerd uit de infrastructuur van diagnostische gegevens zelf. |WADDiagnosticInfrastructureLogsTable |
   | IIS-logboeken |De logboeken die record webaanvragen. Als uw cloudservice een aanzienlijke hoeveelheid verkeer opgehaald, zijn deze logboeken langdurige. Het is een goed idee om te verzamelen en opslaan van deze gegevens alleen wanneer dat nodig. |U vindt is mislukt-aanvraag wordt geregistreerd in de blob-container onder af-iis-failedreqlogs onder een pad op voor deze implementatie, rol en exemplaar. U vindt de volledige Logboeken onder af-iis-logboekbestanden. Vermeldingen voor elk bestand worden in de tabel WADDirectories gedaan. |
   | Crashdumps |Biedt binaire installatiekopieën van uw cloudservice-proces (meestal een werkrol). |af-crush-dumpbestanden voor blob-container |
   | Aangepaste logboekbestanden |Logboeken van de gegevens die u vooraf gedefinieerd. |U kunt in de code de locatie opgeven van aangepaste logboekbestanden in uw opslagaccount. U kunt bijvoorbeeld een aangepaste blob-container opgeven. |
4. Als u gegevens van een willekeurig type worden afgekapt, kunt u proberen verhogen van de buffer voor die gegevens type of verkort het interval tussen de overdracht van gegevens van de virtuele machine naar uw opslagaccount.
5. (Optioneel) Gegevens verwijderen uit het opslagaccount van tijd tot tijd om de algehele opslagkosten te verlagen.
6. Wanneer u een volledige implementatie, het bestand diagnostics.cscfg (.wadcfgx voor Azure SDK 2.5) wordt bijgewerkt in Azure en uw cloudservice neemt over wijzigingen in de configuratie van de diagnostische gegevens. Als u in plaats daarvan een bestaande implementatie bijwerkt, wordt het cscfg-bestand is niet bijgewerkt in Azure. U kunt nog steeds de diagnostische instellingen echter wijzigen door de stappen in de volgende sectie. Zie voor meer informatie over het uitvoeren van een volledige implementatie en het bijwerken van een bestaande implementatie [Publish Azure Application Wizard](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Virtuele machine diagnostische gegevens weergeven
1. Selecteer in het snelmenu voor de virtuele machine **weergave Diagnostics-gegevens**.
   
    ![Diagnostische gegevens weergeven in Azure een virtuele machine](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)
   
    De **Diagnostics samenvatting** dialoogvenster wordt weergegeven.
   
    ![Virtuele machine van Azure diagnostics samenvatting](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  
   
    Als de meest recente gegevens niet wordt weergegeven, moet u wellicht wacht u totdat de overdracht is verstreken.
   
    Als u wilt de gegevens direct bijwerken, selecteert u de **vernieuwen** koppeling. Als u de gegevens automatisch bijgewerkt, selecteert u een interval in de **automatisch vernieuwen** keuzelijst met invoervak. De om foutgegevens te exporteren, selecteer de **exporteren naar CSV** om te maken van een bestand met door komma's gescheiden waarden die u in een Excel-werkblad openen kunt.

## <a name="set-up-cloud-service-diagnostics-after-deployment"></a>Diagnostische gegevens cloud-service instellen na implementatie
Als u een probleem met een cloudservice die al wordt uitgevoerd onderzoeken, kunt u wenst te verzamelen van gegevens die u hebt opgegeven voordat u de rol oorspronkelijk geïmplementeerd. U kunt in dit geval starten dat gegevens worden verzameld door de instellingen in Server Explorer te wijzigen. U kunt instellen met diagnostische gegevens voor één exemplaar of voor alle exemplaren in een rol, afhankelijk van of u opent de **configuratie van diagnostische** in het dialoogvenster in het snelmenu voor het exemplaar of voor de rol. Als u het knooppunt rol configureert, worden eventuele wijzigingen die u aanbrengt gelden voor alle exemplaren. Als u het knooppunt exemplaar configureert, worden alle wijzigingen die u aanbrengt gelden alleen voor die instantie.

### <a name="to-set-up-diagnostics-for-a-running-cloud-service"></a>Voor het instellen van diagnostische gegevens voor een actieve cloudservice
1. Vouw in Server Explorer de **Cloudservices** knooppunt, en vouw vervolgens de lijst met knooppunten om de rol of -exemplaar (of beide) te vinden die u wilt onderzoeken.
   
    ![Diagnostische gegevens configureren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)
2. Selecteer in het snelmenu voor een exemplaar van knooppunt of rol knooppunt **Update diagnostische instellingen**, en selecteer vervolgens de diagnostische instellingen die u wilt verzamelen.
   
    Zie de sectie voor informatie over de configuratie-instellingen, **diagnostics gegevensbronnen instellen** in dit artikel. Zie de sectie voor informatie over het weergeven van de diagnostics-gegevens **de diagnostics-gegevens weergeven** in dit artikel.
   
    Als u gegevensverzameling in Server Explorer wijzigt, blijven de wijzigingen van kracht totdat u de cloudservice volledig opnieuw implementeren is. Als u de standaardwaarde gebruiken publicatie-instellingen, worden de wijzigingen niet overschreven. De standaardwaarde publicatie-instelling is bijwerken van de bestaande implementatie, in plaats van een volledig nieuwe installatie uitvoeren. Om ervoor te zorgen dat de instellingen tijdens de implementatie wissen, gaat u naar de **geavanceerde instellingen** tabblad in de wizard Publiceren en schakel vervolgens de **implementatie-update** selectievakje. Wanneer u opnieuw met dit selectievakje is uitgeschakeld implementeert, de instellingen terug die in het bestand .wadcfgx (of .wadcfg) zoals ingesteld door de **eigenschappen** editor voor de rol. Als u uw implementatie hebt bijgewerkt, houdt Azure de eerdere instellingen.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Problemen met Azure cloud service
Als u problemen met uw cloudserviceprojecten, ondervindt zoals een rol die opgehaald blijft steken in 'bezet' status herhaaldelijk wordt gerecycled of er een interne serverfout, zijn er hulpprogramma's en technieken waarmee u kunt opsporen en oplossen van het probleem. Zie voor specifieke voorbeelden van veelvoorkomende problemen en oplossingen en voor een overzicht van de concepten en hulpprogramma's waarmee u kunt opsporen en oplossen van deze fouten [PaaS Azure diagnostics-gegevens compute](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Vragen en antwoorden
**Wat de buffergrootte is en hoe lang deze moet?**

Op elk exemplaar van de virtuele machine beperken quota's hoeveel diagnostische gegevens kan worden opgeslagen op het lokale bestandssysteem. Bovendien het opgeven van een buffergrootte voor elk type diagnostics-gegevens die beschikbaar is. Deze buffergrootte fungeert als een quotum voor dit type gegevens. De algehele quota en de hoeveelheid geheugen die overblijft wilt vaststellen, Zie de onderkant van het dialoogvenster voor het gegevenstype van de diagnostische gegevens. Als u groter buffers of meerdere typen gegevens opgeeft, moet u de algehele quota benaderen. U kunt de algehele quota wijzigen door het wijzigen van het configuratiebestand diagnostics.wadcfg of .wadcfgx. De diagnostics-gegevens worden opgeslagen op hetzelfde bestandssysteem als de gegevens van uw toepassing. Als uw toepassing gebruikmaakt van een grote hoeveelheid schijfruimte, kunt u de algehele diagnostics quota mag niet verhogen.

**Wat is de periode voor de overdracht en hoe lang deze moet?**

De overdracht periode is de hoeveelheid tijd die is verstreken tussen gegevens worden vastgelegd. Na elke periode overdracht gegevens verplaatst van het lokale bestandssysteem op een virtuele machine aan tabellen in uw opslagaccount. Als de hoeveelheid gegevens die worden verzameld het quotum voor het einde van een overdracht-periode overschrijdt, wordt de oudere gegevens verwijderd. Als u weet verlies van gegevens omdat uw gegevens groter is dan de buffergrootte of de algehele quota, wilt u mogelijk Verklein de periode voor overdracht.

**Welke tijdzone zijn de tijdstempels in?**

Tijdstempels zijn in de lokale tijdzone van het datacenter dat als host fungeert voor uw cloudservice. Met de volgende drie keer stempel kolommen in de tabellen logboek worden gebruikt:

* **PreciseTimeStamp**: de ETW-tijdstempel van de gebeurtenis. Dat wil zeggen de tijd van de client wordt de gebeurtenis vastgelegd.
* **TIJDSTEMPEL**: de waarde voor **PreciseTimeStamp** afgeronde omlaag naar de grens van de frequentie uploaden. Als uw uploadfrequentie 5 minuten en de gebeurtenis tijd 00:17:12 is, is tijdstempel 00:15:00.
* **Tijdstempel**: het tijdstempel waarop de entiteit is gemaakt in de Azure-tabel.

**Hoe beheer ik kosten bij het verzamelen van diagnostische gegevens?**

De standaardinstellingen (**Meld niveau** ingesteld op **fout**, en **overdracht periode** ingesteld op **1 minuut**) zijn ontworpen om de kosten kunt minimaliseren. De compute-kosten vergroot wanneer u meer diagnostische gegevens verzamelen of als u de periode overdracht verkleint. Niet meer gegevens dan u nodig hebt en u niet vergeten om het verzamelen van gegevens uitschakelen wanneer u niet langer verzamelen. U kunt altijd het opnieuw inschakelen, zelfs tijdens runtime, zoals eerder in dit artikel wordt beschreven.

**Hoe ik Logboeken is mislukt-aanvragen verzamelen uit IIS?**

Standaard verzamelen niet IIS logboeken is mislukt-aanvragen. U kunt IIS instellen voor het verzamelen van Logboeken kan aanvragen door het bestand web.config voor de Webrol te bewerken.

**Ik ben traceringsinformatie niet ophalen uit RoleEntryPoint methoden zoals OnStart. Wat is er aan de hand?**

De methoden van **RoleEntryPoint** worden genoemd in de context van WAIISHost.exe niet in IIS. De configuratie-informatie in het bestand web.config die normaal gesproken schakelt tracering niet van toepassing. U lost dit probleem, een .config-bestand toevoegen aan uw webrolproject en noem het bestand overeenkomen met de uitvoer-assembly waarin de **RoleEntryPoint** code. De naam van het .config-bestand moet in het webrolproject standaard WAIISHost.exe.config. De volgende regels toevoegen aan dit bestand:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

In de **eigenschappen** Stel venster de **naar uitvoermap kopiëren** eigenschap **altijd kopiëren**.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over diagnostische gegevens van logboekregistratie in Azure, [diagnostische gegevens in Azure Cloud Services en virtuele machines inschakelen](cloud-services/cloud-services-dotnet-diagnostics.md) en [logboekregistratie van diagnostische gegevens van Web-Apps in Azure App Service](app-service/web-sites-enable-diagnostic-log.md).

