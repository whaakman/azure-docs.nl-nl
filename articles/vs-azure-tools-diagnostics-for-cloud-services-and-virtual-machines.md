---
title: Instellen van diagnostische gegevens voor Azure Cloud Services en virtual machines | Microsoft Docs
description: Meer informatie over het instellen van diagnostische gegevens voor het opsporen van fouten in Azure cloude services en virtual machines (VM's) in Visual Studio.
services: visual-studio-online
documentationcenter: na
author: mikejo
manager: douge
editor: ''
ms.assetid: e70cd7b4-6298-43aa-adea-6fd618414c26
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/28/2018
ms.author: mikejo
ms.openlocfilehash: 8f32165a7a54ead06d57a3d8d1b4282498dca635
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969613"
---
# <a name="set-up-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Instellen van diagnostische gegevens voor Azure Cloud Services en virtual machines
Wanneer u nodig hebt om op te lossen van een Azure-cloudservice of virtuele machine, gebruikt u Visual Studio eenvoudig instellen van Azure Diagnostics. Diagnostische gegevens worden vastgelegd systeemgegevens en logboekregistratie op de virtuele machines en instanties van virtuele machines waarop uw cloudservice wordt uitgevoerd. Diagnostische gegevens worden overgedragen naar een opslagaccount die u kiest. Zie voor meer informatie over diagnostische gegevens van logboekregistratie in Azure, [Diagnostische logboekregistratie inschakelen voor Web-Apps in Azure App Service](app-service/web-sites-enable-diagnostic-log.md).

In dit artikel laten we zien u hoe u Visual Studio gebruiken om te schakelen en het instellen van Azure Diagnostics, zowel vóór als na de implementatie. Meer informatie over het instellen van diagnostische gegevens op Azure virtual machines, het selecteren van de typen diagnostische gegevens te verzamelen en weergeven van de gegevens nadat deze verzameld.

U kunt een van de volgende opties gebruiken voor het instellen van Azure Diagnostics:

* Wijzigen van instellingen voor diagnostische gegevens in de **configuratie van diagnostische** in het dialoogvenster in Visual Studio. De instellingen worden opgeslagen in een bestand met de naam diagnostics.wadcfgx (in de Azure SDK 2.4 en eerdere versies, het bestand is met de naam diagnostics.wadcfg). U ook u kunt het configuratiebestand rechtstreeks wijzigen. Als u het bestand handmatig bijwerken, wordt de configuratie van wijzigingen van kracht de volgende keer dat u de cloud implementeren naar Azure-service of de service wordt uitgevoerd in de emulator.
* Cloud Explorer of Server Explorer in Visual Studio gebruiken om te wijzigen van de diagnostische instellingen voor een cloudservice of virtuele machine die wordt uitgevoerd.

## <a name="azure-sdk-26-diagnostics-changes"></a>Azure SDK 2.6 diagnostische gegevens van wijzigingen
De volgende wijzigingen toepassen op de Azure SDK 2.6 en hoger projecten in Visual Studio:

* De lokale emulator biedt nu ondersteuning voor diagnostische gegevens. Dit betekent dat u kunt verzamelen van diagnostische gegevens en ervoor te zorgen dat de juiste traceringen in uw toepassing wordt gemaakt tijdens het ontwikkelen en testen in Visual Studio. De verbindingsreeks `UseDevelopmentStorage=true` Hiermee schakelt u het verzamelen van diagnostische gegevens tijdens het uitvoeren van uw cloud service-project in Visual Studio met behulp van de Azure-opslagemulator. Alle diagnostische gegevens worden verzameld in de opslag-storage-account.
* De diagnostische gegevens van storage account connection string `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` wordt opgeslagen in het serviceconfiguratiebestand (.cscfg). In de Azure SDK 2.5, is het opslagaccount voor diagnostische gegevens in het bestand diagnostics.wadcfgx opgegeven.

De verbindingsreeks werkt anders op een aantal belangrijke punten in de Azure SDK 2.6 en hoger ten opzichte van de Azure SDK 2.4 en eerder:

* In de Azure SDK 2.4 en eerdere versies, wordt de verbindingsreeks gebruikt als een runtime door de invoegtoepassing diagnostische gegevens om op te halen van de gegevens over het opslagaccount voor het overbrengen van logboeken met diagnostische gegevens.
* In de Azure SDK 2.6 en hoger, Visual Studio maakt gebruik van de verbindingsreeks van diagnostische gegevens voor het instellen van de Azure Diagnostics-extensie met de gegevens van de juiste storage-account tijdens het publiceren. U kunt de verbindingsreeks gebruiken voor het definiëren van verschillende opslagaccounts voor verschillende configuraties die Visual Studio tijdens het publiceren gebruikt. Echter, omdat de invoegtoepassing diagnostische gegevens niet beschikbaar is nadat de Azure SDK 2.5, het cscfg-bestand zelf niet instellen de extensie voor diagnostische gegevens. U moet de extensie apart instellen met behulp van hulpprogramma's zoals Visual Studio of PowerShell.
* Ter vereenvoudiging van het proces van het instellen van de extensie voor diagnostische gegevens met behulp van PowerShell, bevat de uitvoer van het pakket van Visual Studio de openbare configuratie-XML voor de extensie voor diagnostische gegevens voor elke rol. Visual Studio gebruikt de verbindingsreeks van diagnostische gegevens voor het vullen van de gegevens over het opslagaccount in de openbare configuratie. De openbare configuratiebestanden worden gemaakt in de map-extensies. De openbare configuratiebestanden gebruiken het naamgevingspatroon PaaSDiagnostics. &lt;rolnaam\>. PubConfig.xml. Een PowerShell-implementaties kunnen dit patroon gebruiken om elke configuratie worden toegewezen aan een rol.
* De [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) maakt gebruik van de verbindingsreeks in het .cscfg-bestand voor toegang tot de gegevens van de diagnostische gegevens. De gegevens worden weergegeven op de **bewaking** tabblad. De verbindingsreeks is vereist voor het instellen van de service om weer te geven van uitgebreide bewakingsgegevens in de portal.

## <a name="migrate-projects-to-azure-sdk-26-and-later"></a>Migreren van projecten op Azure SDK 2.6 en hoger
Wanneer u van Azure SDK 2.5 naar Azure SDK 2.6 of later, migreert als u een opslagaccount voor diagnostische gegevens opgegeven in het bestand .wadcfgx had, blijft het storage-account in dat bestand. Als u wilt profiteren van de flexibiliteit van het gebruik van verschillende opslagaccounts voor verschillende opslagconfiguraties, moet u handmatig de verbindingsreeks toevoegen aan uw project. Als u een project waarnaar u migreert van Azure SDK 2.4 of eerder naar de Azure SDK 2.6, blijven de verbindingsreeksen voor diagnostische gegevens behouden. Houd echter rekening met de wijzigingen in hoe verbindingsreeksen worden behandeld in de Azure SDK 2.6, beschreven in de vorige sectie.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hoe Visual Studio Hiermee bepaalt u het opslagaccount voor diagnostische gegevens
* Als een verbindingsreeks diagnostische gegevens in het .cscfg-bestand is opgegeven, wordt het in Visual Studio gebruikt om in te stellen de extensie voor diagnostische gegevens tijdens het publiceren en bij het genereren van de openbare configuratie-XML-bestanden tijdens het verpakken.
* Als een verbindingsreeks diagnostics niet in het cscfg-bestand opgegeven is, terugvalt Visual Studio op het gebruik van het opslagaccount dat opgegeven in het bestand .wadcfgx voor het instellen van de extensie voor diagnostische gegevens voor het publiceren en voor het genereren van de openbare configuratie-XML bestanden tijdens het verpakken.
* De verbindingsreeks voor de diagnostische gegevens in het .cscfg-bestand heeft voorrang op de storage-account in het bestand .wadcfgx. Als een verbindingsreeks diagnostics is opgegeven in het .cscfg-bestand, Visual Studio maakt gebruik van deze verbindingsreeks en de storage-account in .wadcfgx worden genegeerd.

### <a name="what-does-the-update-development-storage-connection-strings-check-box-do"></a>Wat is het selectievakje 'Bijwerken ontwikkeling storage-verbindingsreeksen...'?
De **ontwikkeling storage-verbindingsreeksen bijwerken voor diagnostische gegevens en Caching met Microsoft Azure storage-accountreferenties, bij het publiceren naar Microsoft Azure** selectievakje is een handige manier om bij te werken van alle opslag verbinding met het account tekenreeksen met de Azure storage-account dat u tijdens het publiceren opgeeft.

Bijvoorbeeld, als u dit selectievakje inschakelt en de verbindingsreeks van diagnostische gegevens Hiermee geeft u `UseDevelopmentStorage=true`, wanneer u het project naar Azure publiceren, Visual Studio automatisch de verbindingsreeks van diagnostische gegevens met de storage-account dat u hebt opgegeven in de wizard publiceren. Echter, als een echte storage-account is opgegeven als de verbindingstekenreeks diagnostische gegevens, dat account wordt gebruikt in plaats daarvan.

## <a name="diagnostics-functionality-differences-in-azure-sdk-24-and-earlier-vs-azure-sdk-25-and-later"></a>Diagnostische gegevens over functionaliteit verschillen in de Azure SDK 2.4 en eerdere vs. Azure SDK 2.5 of hoger
Als u de upgrade van uw project vanaf de Azure SDK 2.4 en eerder naar de Azure SDK 2.5 of hoger, houd rekening met de volgende functionaliteit verschillen voor diagnostische gegevens:

* **Configuratie-API's zijn afgeschaft**. Programmatische configuratie van diagnostische gegevens is beschikbaar in de Azure SDK 2.4 en eerdere versies, maar is afgeschaft in Azure SDK 2.5 of hoger. Als de configuratie van de diagnostische gegevens op dit moment is gedefinieerd in de code, moet u deze instellingen opnieuw in de gemigreerde project voor diagnostische gegevens blijven werken opnieuw configureren. Het configuratiebestand van de diagnostische gegevens voor Azure SDK 2.4 is diagnostics.wadcfg. Het configuratiebestand voor diagnostische gegevens voor de Azure SDK 2.5 of hoger is diagnostics.wadcfgx.
* **Diagnostische gegevens voor cloud service-toepassingen kunnen worden geconfigureerd op het rolniveau van de**. In de Azure SDK 2.5 of hoger, instellen niet u diagnostische gegevens voor cloud service-toepassingen op het exemplaarniveau van het.
* **Telkens wanneer u uw app implementeert, de configuratie van de diagnostische gegevens wordt bijgewerkt**. Hierdoor kunnen problemen met de functiepariteit als u de configuratie van uw diagnostische gegevens vanuit Visual Studio Server Explorer wijzigen en implementeert u uw app opnieuw.
* **In de Azure SDK 2.5 of hoger, crashdumps zijn geconfigureerd in het configuratiebestand van de diagnostische gegevens en niet in code**. Als u crashdumps die zijn geconfigureerd in de code hebt, moet u handmatig de configuratie van code overbrengen naar het configuratiebestand. Crashdumps worden niet overgedragen tijdens de migratie naar Azure SDK 2.6.

## <a name="turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them"></a>Diagnostische gegevens in cloudserviceprojecten inschakelen voordat u ze implementeert
U kunt diagnostische gegevens voor rollen die worden uitgevoerd in Azure bij het uitvoeren van de service in de emulator vóór de implementatie verzamelen in Visual Studio. Alle wijzigingen in de instellingen voor diagnostische gegevens in Visual Studio zijn opgeslagen in het configuratiebestand diagnostics.wadcfgx. Deze instellingen bepalen het opslagaccount waar de diagnostische gegevens wordt opgeslagen als u uw cloudservice implementeert.

[!INCLUDE [cloud-services-wad-warning](../includes/cloud-services-wad-warning.md)]

### <a name="to-turn-on-diagnostics-in-visual-studio-before-deployment"></a>Diagnostische gegevens in Visual Studio vóór de implementatie inschakelen

1. Selecteer in het snelmenu voor de rol, **eigenschappen**. In van de rol **eigenschappen** in het dialoogvenster, selecteer de **configuratie** tabblad.
2. In de **Diagnostics** sectie, zorg ervoor dat de **diagnostische gegevens inschakelen** selectievakje is ingeschakeld.
   
    ![Toegang tot de optie diagnostische gegevens inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)
3. Als u het opslagaccount voor diagnostische gegevens, schakelt u de knop met het weglatingsteken (...).
   
    ![Geef de storage-account te gebruiken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)
4. In de **Create Storage Connection String** dialoogvenster vak, opgeven of u verbinding maken wilt met behulp van de Azure-opslagemulator, een Azure-abonnement of referenties handmatig worden ingevoerd.
   
    ![In het dialoogvenster van Storage-account](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)
   
   * Als u selecteert **Microsoft Azure-opslagemulator**, de verbindingsreeks is ingesteld op `UseDevelopmentStorage=true`.
   * Als u selecteert **uw abonnement**, selecteert u de Azure-abonnement dat u wilt gebruiken en voer de accountnaam van een. Selecteer voor het beheren van uw Azure-abonnementen, **Accounts beheren**.
   * Als u selecteert **referenties handmatig worden ingevoerd**, voer de naam en sleutel van het Azure-account dat u wilt gebruiken.
5. Om weer te geven de **configuratie van diagnostische** in het dialoogvenster, selecteer **configureren**. Met uitzondering van **algemene** en **logboekmappen**, elk tabblad vertegenwoordigt een gegevensbron voor diagnostische gegevens die u kunt verzamelen. De standaardwaarde **algemene** tabblad biedt diagnostische gegevens over de volgende opties voor het verzamelen van gegevens: **alleen fouten**, **alle informatie**, en **aangepast plan**. De standaardwaarde **alleen fouten** optie maakt gebruik van de minimale hoeveelheid opslag, omdat deze geen waarschuwingen of berichten voor tracering van de gegevensoverdracht. De **alle informatie** optie de meeste gegevens worden overgebracht, maakt gebruik van de meeste opslag en daarom is de duurste optie.

   > [!NOTE]
   > Minimaal ondersteunde grootte voor de "Schijf quotum in MB" is 4GB. Echter, als u geheugendumps verzamelen, verhoogt dit op een hogere waarde, zoals 10GB.
   >
  
    ![Azure diagnostics- en configuratie inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
6. Selecteer voor dit voorbeeld wordt de **aangepast plan** optie, zodat u de verzamelde gegevens kunt aanpassen.
7. In de **schijfquotum in MB** vak, kunt u instellen hoeveel ruimte toe te wijzen in uw opslagaccount voor diagnostische gegevens. U kunt wijzigen of accepteer de standaardwaarde.
8. Op elk tabblad van diagnostische gegevens die u wenst te verzamelen, selecteer de **inschakelen overdragen van \<Logboektype\>**  selectievakje. Bijvoorbeeld, als u wenst te verzamelen van toepassingslogboeken, op de **toepassingslogboeken** tabblad de **inschakelen van de overdracht van toepassingslogboeken** selectievakje. Geef ook andere informatie die vereist voor elk gegevenstype diagnostische gegevens. Zie de sectie voor configuratie-informatie voor elk tabblad, **instellen van diagnostische gegevens-gegevensbronnen** verderop in dit artikel. 
9. Nadat u de verzameling van alle diagnostische gegevens die u hebt ingeschakeld, selecteert u **OK**.
10. Zoals gewoonlijk uitvoeren van uw Azure-cloud service-project in Visual Studio. Als u uw toepassing gebruikt, wordt informatie dat u hebt ingeschakeld in het foutenlogboek wordt opgeslagen in de Azure storage-account dat u hebt opgegeven.

## <a name="turn-on-diagnostics-on-azure-virtual-machines"></a>Diagnostische gegevens op Azure virtual machines inschakelen
U kunt diagnostische gegevens voor Azure virtual machines verzamelen in Visual Studio.

### <a name="to-turn-on-diagnostics-on-azure-virtual-machines"></a>Diagnostische gegevens op Azure virtual machines inschakelen

1. In Server Explorer, selecteert u de Azure-knooppunt en klikt u vervolgens verbinding maken met uw Azure-abonnement, als u nog niet bent verbonden.
2. Vouw de **virtuele Machines** knooppunt. U kunt maken van een nieuwe virtuele machine of Selecteer een bestaand knooppunt.
3. Selecteer in het snelmenu voor de virtuele machine die u wilt, **configureren**. In het dialoogvenster voor virtuele machine-configuratie wordt weergegeven.
   
    ![Een virtuele machine van Azure configureren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)
4. Als dit nog niet is geïnstalleerd, voegt u de Microsoft Monitoring Agent Diagnostics-extensie toe. Met deze extensie kunt u diagnostische gegevens voor de virtuele machine van Azure te verzamelen. Onder **extensies geïnstalleerd**, in de **selecteert u een extensie beschikbaar** vervolgkeuzelijst keuzelijst, selecteer **Microsoft Monitoring Agent Diagnostics**.
   
    ![Een virtuele machine van Azure-extensie installeren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)
   
    > [!NOTE]
   > Andere extensies diagnostische gegevens zijn beschikbaar voor uw virtuele machines. Zie voor meer informatie, [extensies voor virtuele machines en functies voor Windows](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).
   > 
   > 
5. Om toe te voegen van de extensie en bekijk de **configuratie van diagnostische** in het dialoogvenster, selecteer **toevoegen**.
6. Als u een opslagaccount, selecteer **configureren**, en selecteer vervolgens **OK**.
   
    Elk tabblad (met uitzondering van **algemene** en **logboekmappen**) staat voor een gegevensbron voor diagnostische gegevens die u kunt verzamelen.
   
    ![Azure diagnostics- en configuratie inschakelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)
   
    Het standaardtabblad **algemene**, biedt u de volgende opties voor het diagnostische gegevens verzamelen: **alleen fouten**, **alle informatie**, en **aangepast plan**. De standaardoptie **alleen fouten**, wordt de minimale hoeveelheid opslag omdat deze geen waarschuwingen of berichten voor tracering van de gegevensoverdracht. De **alle informatie** optie de meeste gegevens worden overgebracht en is daarom de duurste optie in termen van opslag.
7. Selecteer voor dit voorbeeld wordt de **aangepast plan** optie zodat u de gegevens kunt aanpassen die worden verzameld.
8. De **schijfquotum in MB** vak geeft aan hoeveel ruimte u in uw opslagaccount voor diagnostische gegevens wilt toewijzen. Als u wilt, kunt u de standaardwaarde wijzigen.
9. Op elk tabblad van diagnostische gegevens die u wenst te verzamelen, selecteer de **inschakelen overdragen van \<Logboektype\>**  selectievakje.
   
    Bijvoorbeeld, als u wenst te verzamelen van toepassingslogboeken, selecteert u de **inschakelen van de overdracht van toepassingslogboeken** selectievakje op de **toepassingslogboeken** tabblad. Geef ook andere informatie die zijn voor elk gegevenstype diagnostische gegevens vereist. Zie de sectie voor configuratie-informatie voor elk tabblad, **instellen van diagnostische gegevens-gegevensbronnen** verderop in dit artikel.
10. Nadat u de verzameling van alle gegevens van diagnostische gegevens die u hebt ingeschakeld, selecteert u **OK**.
11. Sla het bijgewerkte project.
    
    Een bericht in de **Microsoft Azure-activiteitenlogboek** venster geeft aan dat de virtuele machine is bijgewerkt.

## <a name="set-up-diagnostics-data-sources"></a>Instellen van diagnostische gegevens-gegevensbronnen
Nadat u het verzamelen van diagnostische gegevens inschakelt, kunt u precies welke gegevensbronnen die u wenst te verzamelen en welke gegevens worden verzameld. De volgende secties beschrijven de tabbladen in de **configuratie van diagnostische** in het dialoogvenster en welke optie voor elke configuratie betekent.

### <a name="application-logs"></a>Toepassingslogboeken
Toepassingslogboeken zijn diagnostische gegevens die wordt geproduceerd door een webtoepassing. Als u vastleggen toepassingslogboeken wilt, selecteert u de **inschakelen van de overdracht van toepassingslogboeken** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van toepassingslogboeken naar uw opslagaccount, wijzigt de **overdracht periode (min)** waarde. Ook kunt u de hoeveelheid gegevens die zijn vastgelegd in het logboek door in te stellen de **melden niveau** waarde. Selecteer bijvoorbeeld **uitgebreid** meer informatie, of selecteer **kritieke** om vast te leggen alleen kritieke fouten. Als u een toepassingsspecifieke diagnosefuncties-provider die toepassingslogboeken verzendt hebt, kunt u de logboeken vastleggen door toe te voegen van de provider-GUID in de **Provider GUID** vak.

  ![Toepassingslogboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

Zie voor meer informatie over toepassingslogboeken [Diagnostische logboekregistratie inschakelen voor Web-Apps in Azure App Service](app-service/web-sites-enable-diagnostic-log.md).

### <a name="windows-event-logs"></a>Windows-gebeurtenislogboeken
Voor het vastleggen van Windows-gebeurtenislogboeken, selecteer de **overdracht van het Windows-gebeurtenislogboeken inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van logboeken naar uw opslagaccount, wijzigt de **overdracht periode (min)** waarde. Schakel de selectievakjes voor de typen gebeurtenissen die u wilt bijhouden.

![Gebeurtenislogboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Als u Azure SDK 2.6 of hoger gebruikt en u wilt een aangepaste gegevensbron opgeven, voert u deze in de **\<gegevensbronnaam\>** tekstvak in en selecteer vervolgens **toevoegen**. De gegevensbron wordt toegevoegd aan het bestand diagnostics.cfcfg.

Als u van Azure SDK 2.5 gebruikmaakt en om op te geven van een aangepaste gegevensbron, kunt u het toevoegen aan de `WindowsEventLog` sectie van de diagnostics.wadcfgx bestand, zoals in het volgende voorbeeld:

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Prestatiemeteritems
Gegevens van prestatiemeteritems kunt u vinden knelpunten van het systeem en de systeem- en toepassingsprestaties verder kunt afstemmen. Zie voor meer informatie, [prestatiemeteritems maken en gebruiken in een Azure-toepassing](https://msdn.microsoft.com/library/azure/hh411542.aspx). Voor het vastleggen van prestatiemeteritems, selecteer de **overdracht van prestatiemeteritems inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van logboeken naar uw opslagaccount, wijzigt de **overdracht periode (min)** waarde. Schakel de selectievakjes voor de prestatiemeteritems die u wilt bijhouden.

![Prestatiemeteritems](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Voer het prestatiemeteritem met behulp van de syntaxis van de voorgestelde voor het volgen van een prestatiemeteritem die niet wordt vermeld. en selecteer vervolgens **toevoegen**. Het besturingssysteem op de virtuele machine bepaalt welke prestatiemeteritems die u kunt volgen. Zie voor meer informatie over de syntaxis van [Geef een pad voor prestatiemeteritems](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx).

### <a name="infrastructure-logs"></a>Protokoly infrastruktury
Protokoly infrastruktury hebben informatie over de diagnostische Azure-infrastructuur, de RemoteAccess-module en de RemoteForwarder-module. Voor het verzamelen van informatie over de infrastructuur-Logboeken, selecteer de **overdracht van infrastructuur Logboeken inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van infrastructuur logboeken naar uw opslagaccount, wijzigt de **overdracht periode (min)** waarde.

![Logboeken met diagnostische gegevens over infrastructuur](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

Zie voor meer informatie, [gegevens voor logboekregistratie verzamelen met behulp van Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="log-directories"></a>Logboekmappen
Logboekmappen die zijn gegevens die worden verzameld van logboekmappen die voor Internet Information Services (IIS)-aanvragen, mislukte aanvragen of mappen die u kiest. Om vast te leggen logboekmappen, selecteer de **inschakelen van de overdracht van logboekmappen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van logboeken naar uw opslagaccount, wijzigt de **overdracht periode (min)** waarde.

Schakel de selectievakjes van de logboeken die u wenst te verzamelen, zoals **IIS-logboeken** en **mislukte verzoeken** Logboeken. Standaard opslag containernamen zijn opgegeven, maar u kunt de namen wijzigen.

U kunt Logboeken vanuit een willekeurige map vastleggen. Geef het pad op in de **logboek van Absolute Directory** uit en selecteer vervolgens **map toevoegen**. De logboeken worden vastgelegd in de opgegeven containers.

![Logboekmappen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>ETW-Logboeken
Als u [Event Tracing voor Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803\(v=vs.85\).aspx) (ETW) en u wilt vastleggen van ETW-Logboeken, selecteert u de **overdracht van ETW-Logboeken inschakelen** selectievakje. Als u wilt vergroten of verkleinen van het interval tussen de overdracht van logboeken naar uw opslagaccount, wijzigt de **overdracht periode (min)** waarde.

De gebeurtenissen worden vastgelegd van bronnen van gebeurtenissen en gebeurtenismanifesten die u opgeeft. Een gebeurtenisbron opgeven in de **gebeurtenisbronnen** sectie, voer een naam in en selecteer vervolgens **gebeurtenisbron toevoegen**. Op deze manier kunt u een gebeurtenismanifest in de **Gebeurtenismanifesten** uit en selecteer vervolgens **toevoegen gebeurtenis Manifest**.

![ETW-Logboeken](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

De ETW-framework in ASP.NET wordt ondersteund door middel van klassen in de [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) naamruimte. De naamruimte Microsoft.WindowsAzure.Diagnostics waarvoor overneemt en breidt de standaard [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) klassen, maakt het gebruik van [System.Diagnostics.aspx](https://msdn.microsoft.com/library/system.diagnostics(v=vs.110)) als een logboekregistratie Framework in de Azure-omgeving. Zie voor meer informatie, [overnemen van logboekregistratie en tracering in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) en [diagnostische gegevens in Azure Cloud Services en virtual machines inschakelen](cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Crashdumps
Voor het vastleggen van informatie over wanneer een rolinstantie vastloopt, selecteer de **inschakelen van de overdracht van Crash geheugendumps** selectievakje. (Omdat ASP.NET de meeste uitzonderingen verwerkt, dit is meestal alleen nuttig voor werkrollen.) Als u wilt vergroten of verkleinen van de hoeveelheid opslagruimte die aan de crashdumps besteed, wijzigt de **Directory quotum (%)** waarde. U kunt de opslagcontainer waarin de crashdumps worden opgeslagen en selecteert u of u wilt vastleggen wijzigen een **volledige** of **Mini** dump.

De processen die momenteel worden bijgehouden, worden weergegeven in de volgende schermafbeelding. Schakel de selectievakjes voor de processen die u wilt vastleggen. Voer de procesnaam om toe te voegen een ander proces aan de lijst, en selecteer vervolgens **proces toevoegen**.

![Crashdumps](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

Zie voor meer informatie, [overnemen van logboekregistratie en tracering in Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) en [Microsoft Azure Diagnostics deel 4: aangepaste logboekregistratie onderdelen en Azure Diagnostics 1.3 wijzigingen](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/).

## <a name="view-the-diagnostics-data"></a>De diagnostische gegevens weergeven
Nadat u de diagnostische gegevens voor een cloudservice of virtuele machine hebt verzameld, kunt u deze kunt bekijken.

### <a name="to-view-cloud-service-diagnostics-data"></a>Cloud service diagnostics-gegevens weergeven
1. Uw cloudservice als Normaal implementeren en vervolgens uit te voeren.
2. U kunt de diagnostische gegevens weergeven in een rapport dat door Visual Studio gegenereerd of in de tabellen in uw storage-account. Naar de weergave de gegevens in een rapport, open Cloud Explorer of Server Explorer, open het snelmenu van het knooppunt voor de rol die u wilt, en selecteer vervolgens **diagnostische gegevens weergeven**.
   
    ![Gegevens van de diagnostische gegevens weergeven](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)
   
    Een rapport met de beschikbare gegevens wordt weergegeven.
   
    ![Microsoft Azure Diagnostics-rapport in Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)
   
    Als de meest recente gegevens niet wordt weergegeven, moet u mogelijk wachten gedurende de overdracht moet zijn verstreken.
   
    Als u wilt de gegevens direct bijwerken, selecteert u de **vernieuwen** koppeling. Als u wilt de gegevens automatisch bijgewerkt, selecteert u een interval in de **automatisch vernieuwen** vervolgkeuzelijst met box. De om foutgegevens te exporteren, selecteert u de **exporteren naar CSV** om te maken van een bestand met door komma's gescheiden waarden die u in een Excel-werkblad openen kunt.
   
    Open het storage-account dat is gekoppeld aan de implementatie in Cloud Explorer of in Server Explorer.
3. Open de diagnostische gegevens van tabellen in de viewer voor tabel en controleer de gegevens die u hebt verzameld. Voor IIS-logboeken en aangepaste logboeken, kunt u een blob-container openen. De volgende tabel bevat de tabellen of de blob-containers die de gegevens voor de verschillende logboekbestanden bevatten. Naast de gegevens voor dat logboekbestand items in de tabel bevatten **EventTickCount**, **DeploymentId**, **rol**, en **RoleInstance** , om te identificeren welke virtuele machine en de rol van de gegevens gegenereerd en wanneer. 
   
   | Diagnostische gegevens | Beschrijving | Locatie |
   | --- | --- | --- |
   | Toepassingslogboeken |Logboeken die uw code wordt gegenereerd door het aanroepen van methoden van de **System.Diagnostics.Trace** klasse. |WADLogsTable |
   | Gebeurtenislogboeken |Gegevens uit de Windows-gebeurtenislogboeken op de virtuele machines. Windows slaat informatie op in deze logboeken, maar toepassingen en services ook gebruik van de logboeken fouten rapporteren of logboekgegevens. |WADWindowsEventLogsTable |
   | Prestatiemeteritems |U kunt gegevens verzamelen van een prestatiemeteritem dat beschikbaar is op de virtuele machine. Het besturingssysteem biedt prestatiemeters, die veel, zoals de tijd voor het gebruik en de processor van geheugen statistieken. |WADPerformanceCountersTable |
   | Protokoly infrastruktury |De logboeken die zijn gegenereerd op basis van de diagnostische gegevens over infrastructuur zelf. |WADDiagnosticInfrastructureLogsTable |
   | IIS-logboeken |De logboeken die record webaanvragen. Als uw cloudservice een aanzienlijke hoeveelheid verkeer wordt, kunnen deze logboeken lang zijn. Er is een goed idee om te verzamelen en opslaan van deze gegevens alleen wanneer u ze nodig hebt. |Kan de niet-request-Logboeken in de blob-container onder wad-iis-failedreqlogs onder een pad voor die implementatie, de rol en het exemplaar, kunt u vinden. U vindt de volledige Logboeken onder wad-iis-logboekbestanden. Vermeldingen voor elk bestand worden in de tabel WADDirectories gemaakt. |
   | Crashdumps |Biedt binaire kopieën van uw cloud Services-proces (meestal een werkrol). |wad-crush-dumpbestanden voor foutopsporing blob-container |
   | Aangepaste logboekbestanden |Logboeken van de gegevens die u vooraf gedefinieerd. |U kunt opgeven in de code de locatie van het aangepaste logboekbestanden in uw opslagaccount. U kunt bijvoorbeeld een aangepaste blob-container opgeven. |
4. Als u van elk type gegevens is afgekapt, kunt u proberen verhogen van de buffer voor die gegevens type of verkort het interval tussen de overdracht van gegevens van de virtuele machine naar uw opslagaccount.
5. (Optioneel) Gegevens verwijderen uit het opslagaccount van tijd tot tijd te verminderen van kosten voor opslag.
6. Wanneer u een volledige implementatie, het bestand diagnostics.cscfg (.wadcfgx voor Azure SDK 2.5) wordt bijgewerkt in Azure en uw cloudservice neemt het eventuele wijzigingen aan de configuratie van diagnostische gegevens. Als u in plaats daarvan een bestaande implementatie bijwerkt, wordt het cscfg-bestand wordt niet bijgewerkt in Azure. U kunt de diagnostische instellingen, echter nog steeds wijzigen met de volgende stappen in de volgende sectie. Zie voor meer informatie over het uitvoeren van een volledige implementatie en het bijwerken van een bestaande implementatie [Wizard voor Azure-toepassing publiceren](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Diagnostische gegevens van virtuele machines weergeven
1. Selecteer in het snelmenu voor de virtuele machine, **weergave diagnostische gegevens**.
   
    ![Diagnostische gegevens weergeven in een virtuele machine van Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)
   
    De **souhrn Diagnostiky** in het dialoogvenster wordt weergegeven.
   
    ![Souhrn diagnostiky op de virtuele machine van Azure](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  
   
    Als de meest recente gegevens niet wordt weergegeven, moet u mogelijk wachten gedurende de overdracht moet zijn verstreken.
   
    Als u wilt de gegevens direct bijwerken, selecteert u de **vernieuwen** koppeling. Als u wilt de gegevens automatisch bijgewerkt, selecteert u een interval in de **automatisch vernieuwen** vervolgkeuzelijst met box. De om foutgegevens te exporteren, selecteert u de **exporteren naar CSV** om te maken van een bestand met door komma's gescheiden waarden die u in een Excel-werkblad openen kunt.

## <a name="set-up-cloud-service-diagnostics-after-deployment"></a>Cloud service diagnostics instellen na de implementatie
Als u een probleem met een cloudservice die al wordt uitgevoerd onderzoekt, kunt u het verzamelen van gegevens die u hebt opgegeven voordat u de rol oorspronkelijk geïmplementeerd. In dit geval kun u die gegevens verzamelen door de instellingen in Server Explorer te wijzigen. U kunt instellen met diagnostische gegevens voor één exemplaar of voor alle exemplaren in een rol, afhankelijk van of u opent de **configuratie van diagnostische** in het dialoogvenster in het snelmenu voor het exemplaar of voor de rol. Als u het knooppunt rol configureert, zijn alle wijzigingen die u van toepassing op alle exemplaren. Als u het knooppunt exemplaar configureert, worden alle wijzigingen die u aanbrengt gelden alleen voor dat exemplaar.

### <a name="to-set-up-diagnostics-for-a-running-cloud-service"></a>Voor het instellen van diagnostische gegevens voor een actieve cloudservice
1. Vouw in Server Explorer, de **Cloudservices** knooppunt, en vouw vervolgens de lijst met knooppunten om de rol of exemplaar (of beide) te vinden die u wilt onderzoeken.
   
    ![Diagnostische gegevens configureren](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)
2. Selecteer in het snelmenu voor een exemplaar van knooppunt of de rol knooppunt **Update diagnostische instellingen**, en selecteer vervolgens de diagnostische instellingen die u wenst te verzamelen.
   
    Voor informatie over de configuratie-instellingen, Zie de sectie **instellen van diagnostische gegevens-gegevensbronnen** in dit artikel. Zie de sectie voor informatie over het weergeven van de diagnostische gegevens, **de diagnostische gegevens inzien** in dit artikel.
   
    Als u gegevensverzameling in Server Explorer wijzigt, blijft de wijzigingen van kracht totdat u uw cloudservice volledig opnieuw implementeren is. Als u de standaard publicatie-instellingen, worden de wijzigingen niet overschreven. De standaardwaarde publicatie-instelling is om bij te werken van de bestaande implementatie, in plaats van een volledige herimplementatie doen. Om ervoor te zorgen dat de instellingen tijdens de implementatie wissen, gaat u naar de **geavanceerde instellingen** tabblad in de wizard Publiceren en schakel vervolgens de **implementatie update** selectievakje. Wanneer u met dit selectievakje is uitgeschakeld implementeren, de instellingen op de accounts in het bestand .wadcfgx (of .wadcfg) herstellen zoals ingesteld door de **eigenschappen** editor voor de rol. Als u uw implementatie hebt bijgewerkt, houdt Azure de eerdere instellingen.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Problemen met Azure cloud service
Als u problemen met uw cloud service-projecten, ondervindt zoals een rol die in een status 'bezet' vastloopt herhaaldelijk wordt gerecycled of een interne serverfout optreedt, en zijn er hulpmiddelen en technieken waarmee u kunt vaststellen en oplossen van het probleem. Zie voor specifieke voorbeelden van veelvoorkomende problemen en oplossingen, en voor een overzicht van de concepten en hulpprogramma's waarmee u kunt vaststellen en oplossen van deze fouten, [Azure PaaS compute diagnostics data](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

## <a name="q--a"></a>Vragen en antwoorden
**Wat de buffergrootte is en hoe groot moet het zijn?**

Op elke virtuele machine-instantie beperken quota's hoeveel diagnostische gegevens kan worden opgeslagen op het lokale bestandssysteem. Daarbij geeft u op een buffergrootte voor elk type diagnostische gegevens die beschikbaar is. Deze buffergrootte fungeert als een quotum van dat type gegevens. Om te bepalen het algehele quotum en de hoeveelheid geheugen die blijft, Zie onderaan in het dialoogvenster voor het gegevenstype van diagnostische gegevens. Als u groter buffers of meer typen gegevens opgeeft, moet u het algehele quotum benaderen. U kunt het algehele quotum wijzigen door het wijzigen van het configuratiebestand diagnostics.wadcfg of .wadcfgx. De diagnostische gegevens worden opgeslagen op hetzelfde bestandssysteem als de gegevens van uw toepassing. Als uw toepassing gebruikmaakt van een grote hoeveelheid schijfruimte, kunt u het algehele quotum van diagnostische gegevens mag niet verhogen.

**Wat is de periode voor de overdracht en hoe lang deze moet?**

De overdracht-periode is de hoeveelheid tijd die is verstreken tussen gegevens worden vastgelegd. Na elke periode van de overdracht van worden gegevens verplaatst van het lokale bestandssysteem op een virtuele machine aan tabellen in uw storage-account. Als de hoeveelheid gegevens die worden verzameld het quotum voor het einde van de periode van een overdracht overschrijdt, wordt oudere gegevens verwijderd. Als u gegevens verliest omdat uw gegevens groter is dan de buffergrootte van de of het algehele quotum, wilt u mogelijk Verklein de periode voor de overdracht.

**Welke tijdzone zijn de tijdstempels in?**

Tijdstempels zijn in de lokale tijdzone van het datacenter dat als host fungeert voor uw cloudservice. Met de volgende drie tijdstempel kolommen in de log-tabellen worden gebruikt:

* **PreciseTimeStamp**: de ETW-tijdstempel van de gebeurtenis. Dat wil zeggen, de tijd die de gebeurtenis is vastgelegd van de client.
* **TIJDSTEMPEL**: de waarde voor **PreciseTimeStamp** afgerond naar de grens van de frequentie uploaden. Bijvoorbeeld, als uw uploadfrequentie 5 minuten en de gebeurtenis tijd 00:17:12 is, is TIMESTAMP 00:15:00 uur.
* **Tijdstempel**: het tijdstempel waarop de entiteit is gemaakt in de Azure-tabel.

**Hoe beheer ik kosten bij het verzamelen van diagnostische gegevens?**

De standaardinstellingen (**melden niveau** ingesteld op **fout**, en **overdracht periode** ingesteld op **1 minuut**) zijn ontworpen om kosten te minimaliseren. Uw kosten voor rekenuren verhogen als u meer diagnostische gegevens verzamelen of als u Verklein de periode van de overdracht. Niet meer gegevens dan u nodig hebt en u niet vergeten om het verzamelen van gegevens uitschakelen wanneer u ze niet meer nodig hebt niet verzamelen. U kunt altijd het opnieuw inschakelen, zelfs tijdens runtime, zoals eerder in dit artikel beschreven.

**Hoe ik kan geen aanvraag logboeken verzamelen van IIS?**

IIS niet standaard is mislukt-request-logboeken te verzamelen. U kunt IIS zodanig instellen dat is mislukt-aanvraag om Logboeken te verzamelen door het bewerken van het bestand web.config voor de Webrol.

**Ik ben trace-informatie niet ophalen uit RoleEntryPoint methoden, zoals OnStart. Wat is er aan de hand?**

De methoden van **RoleEntryPoint** worden genoemd in de context van WAIISHost.exe, niet in IIS. De configuratiegegevens in web.config die normaal gesproken schakelt tracering niet van toepassing. U lost dit probleem, een .config-bestand toevoegen aan uw webproject rol en noem het bestand overeenkomt met de uitvoer-assembly met de **RoleEntryPoint** code. De naam van het .config-bestand moet in het webrolproject standaard WAIISHost.exe.config. De volgende regels toevoegen aan dit bestand:

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

In de **eigenschappen** venster de **naar uitvoermap kopiëren** eigenschap **altijd kopiëren**.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over diagnostische gegevens van logboekregistratie in Azure, [diagnostische gegevens in Azure Cloud Services en virtual machines inschakelen](cloud-services/cloud-services-dotnet-diagnostics.md) en [Diagnostische logboekregistratie inschakelen voor Web-Apps in Azure App Service](app-service/web-sites-enable-diagnostic-log.md).

