---
title: Aan de slag met Azure Log integratie | Microsoft Docs
description: Informatie over het installeren van de integratie van Azure Log-service en het integreren van Logboeken van Azure Storage, Azure controlelogboeken en Azure Security Center-waarschuwingen.
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 06/07/2018
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 5aab890340fcdd87e1b3788d8bcca903c43da1da
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235743"
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure Log-integratie met Azure Diagnostics logboekregistratie en Windows event forwarding


>[!IMPORTANT]
> De functie van de integratie met Azure-logboekanalyse door 01-06/2019 afgeschaft. Door 27 Jun 2018 AzLog downloads uitgeschakeld. Voor hulp bij het wat te doen zwevend forward revisie van het bericht [gebruik Azure-monitor voor de integratie met SIEM's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

U moet Azure-logboekanalyse integratie alleen gebruiken als een [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) connector is niet beschikbaar vanuit de leverancier van uw Security Incident en Event Management (SIEM).

Integratie van Azure Log beschikbaar Azure logboeken naar uw SIEM zodat u een geïntegreerde beveiliging dashboard voor alle activa maken kunt.
Neem contact op met de leverancier van uw SIEM voor meer informatie over de status van een Azure-Monitor-connector.

> [!IMPORTANT]
> Als uw primaire interesse met het verzamelen van Logboeken van de virtuele machine, omvatten de meeste leveranciers van SIEM deze optie in de oplossing. Met behulp van de SIEM is-connector van de leverancier altijd het aanbevolen alternatief is.

In dit artikel helpt u aan de slag met Azure Log-integratie. Dit artikel gaat over het installeren van de integratie van Azure Log-service en de service integreren met Azure Diagnostics. De integratie van Azure Log-service verzamelt vervolgens Windows-gebeurtenislogboek-informatie van het kanaal Windows-gebeurtenis voor beveiliging van virtuele machines die zijn geïmplementeerd in een Azure-infrastructuur als een service. Dit is vergelijkbaar met *-gebeurtenis doorsturen* die u mogelijk in een on-premises systeem.

> [!NOTE]
> De uitvoer van de integratie van Azure Log integreren met een SIEM wordt uitgevoerd door de SIEM zelf. Zie voor meer informatie [Azure Log-integratie integreren met uw SIEM lokale](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/).

De integratie van Azure Log-service wordt uitgevoerd op een fysieke of een virtuele computer met Windows Server 2008 R2 of hoger (Windows Server 2016 of Windows Server 2012 R2 heeft de voorkeur).

Een fysieke computer kunt on-premises uitgevoerd of op een site host. Als u kiest voor het uitvoeren van de integratie van Azure Log-service op een virtuele machine, de virtuele machine mag zich lokaal of in een openbare cloud, zoals in Microsoft Azure.

De fysieke of virtuele machine met de Azure Log Integration-service vereist netwerkverbinding met de openbare Azure-cloud. In dit artikel bevat informatie over de vereiste configuratie.

## <a name="prerequisites"></a>Vereisten

Ten minste wilt installeren Azure Log integratie, moet de volgende items:

* Een **Azure-abonnement**. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een **opslagaccount** die kunnen worden gebruikt voor Windows Azure Diagnostics (af) logboekregistratie. U kunt een vooraf geconfigureerde opslagaccount gebruiken of een nieuw opslagaccount maken. Verderop in dit artikel worden beschreven het configureren van het opslagaccount.

  > [!NOTE]
  > Afhankelijk van uw scenario, een opslagaccount mogelijk niet meer vereist. Een opslagaccount is vereist voor het scenario met een Azure Diagnostics die in dit artikel.

* **Twee systemen**: 
  * Een computer die de integratie van Azure Log-service wordt uitgevoerd. Deze computer worden verzameld van de logboekgegevens die later in uw SIEM is geïmporteerd. Dit systeem:
    * Kan lokaal of gehost in Microsoft Azure.  
    * Moet worden uitgevoerd als een x64 versie van Windows Server 2008 R2 SP1 of hoger, en Microsoft .NET Framework 4.5.1 is geïnstalleerd. Om te bepalen van de .NET-versie die is geïnstalleerd, Zie [bepalen welke versies van .NET Framework zijn geïnstalleerd](https://msdn.microsoft.com/library/hh925568).  
    * Moet zijn verbonden met het Azure Storage-account dat is gebruikt voor logboekregistratie van Azure Diagnostics. Verderop in dit artikel wordt beschreven hoe connectiviteit bevestigen.
  * Een machine die u wilt bewaken. Dit is een virtuele machine wordt uitgevoerd als een [virtuele machine van Azure](../virtual-machines/virtual-machines-windows-overview.md). De logboekgegevens van deze computer is verzonden naar de integratie van Azure Log-service-machine.

Voor een snelle demonstratie van het maken van een virtuele machine met behulp van de Azure-portal te kijken in de volgende video:<br /><br />


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Tijdens het testen, kunt u een systeem dat voldoet aan de minimale besturingssysteemvereisten. Voor een productieomgeving, de belasting moet u mogelijk om te plannen voor omhoog schalen of uitbreiden.

U kunt meerdere exemplaren van de integratie van Azure Log-service uitvoeren. U kunt echter slechts één exemplaar van de service per fysieke of virtuele machine uitvoeren. Bovendien kunt u taakverdeling diagnostische gegevens van Azure storage-accounts voor af. Het aantal abonnementen bieden aan de exemplaren is gebaseerd op de capaciteit van de.

> [!NOTE]
> Op dit moment zijn er specifieke aanbevelingen over het uitbreiden van exemplaren van logboek-integratie van Azure-machines (dat wil zeggen, machines waarop de integratie van Azure Log-service wordt uitgevoerd) of voor storage-accounts of abonnementen. Beslissingen nemen vergroten/verkleinen op basis van uw metingen prestaties in elk van deze gebieden.

Om prestaties te verbeteren, hebt u ook de optie voor het schalen van de integratie van Azure Log-service. Aan de hand van de volgende maatstaven voor prestaties kunt u het formaat van de machines die u wilt uitvoeren van de integratie van Azure Log-service:

* Op een machine met 8-processor (core), kan één exemplaar van Azure Log integratie verwerken ongeveer 24 miljoen gebeurtenissen per dag (ongeveer 1 miljoen gebeurtenissen per uur).
* Op een machine 4-processor (core) kan één exemplaar van Azure Log integratie verwerken ongeveer 1,5 miljoen gebeurtenissen per dag (ongeveer 62,500 gebeurtenissen per uur).

## <a name="install-azure-log-integration"></a>Azure-logboekanalyse-integratie installeren

Voor het installeren van de integratie van Azure Log downloaden de [Azure Log integratie](https://www.microsoft.com/download/details.aspx?id=53324) -bestand voor installatie. De installatie te voltooien. Kies of u informatie te verstrekken telemetrie naar Microsoft.

De integratie van Azure Log-service verzamelt telemetriegegevens van de computer waarop deze geïnstalleerd.  

Telemetriegegevens die worden verzameld, omvat het volgende:

* De uitzonderingen die tijdens het uitvoeren van de integratie van Azure-logboek optreden.
* Metrische gegevens over het aantal query's en gebeurtenissen verwerkt.
* Statistieken over welke Azlog.exe opdrachtregelopties worden gebruikt. 

> [!NOTE]
> Het is raadzaam dat u Microsoft voor het verzamelen van telemetriegegevens toestaan. U kunt de verzameling van telemetriegegevens uitschakelen door het selectievakje de **Microsoft toestemming geven voor het verzamelen van telemetriegegevens** selectievakje.
>

![Schermopname van het deelvenster installatie met het selectievakje telemetrie ingeschakeld](./media/security-azure-log-integration-get-started/telemetry.png)


Het installatieproces wordt beschreven in de volgende video:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]

## <a name="post-installation-and-validation-steps"></a>Stappen na de installatie en validatie

Nadat u basisinstellingen hebt voltooid, bent u klaar stappen na de installatie en validatie uit te voeren:

1. Open PowerShell als beheerder. Ga vervolgens naar C:\Program Files\Microsoft Azure Log-integratie.
2. Importeer de integratie van Azure Log-cmdlets. Voer het script voor het importeren van de cmdlets `LoadAzlogModule.ps1`. Voer `.\LoadAzlogModule.ps1`, en druk op Enter (Let op het gebruik van **.\\**  in deze opdracht). U ziet er ongeveer zo wat wordt weergegeven in de volgende afbeelding:

  ![Schermafbeelding van de uitvoer van de opdracht LoadAzlogModule.ps1](./media/security-azure-log-integration-get-started/loaded-modules.png)
3. Configureer vervolgens de integratie van Azure-logboek voor het gebruik van een specifieke Azure-omgeving. Een *Azure-omgeving* is het type van de Azure-cloud-datacenter die u wilt werken. Hoewel er momenteel verschillende Azure-omgevingen zijn, zijn de desbetreffende opties **AzureCloud** of **AzureUSGovernment**. Uitvoeren van PowerShell als beheerder, zorg ervoor dat u in C:\Program Files\Microsoft Azure Log Integration\. Voer deze opdracht:

  `Set-AzlogAzureEnvironment -Name AzureCloud` (voor **AzureCloud**)
  
  Als u de US Government Azure-cloud gebruiken wilt, gebruikt u **AzureUSGovernment** voor de **-naam** variabele. Momenteel worden niet andere Azure-clouds ondersteund.  

  > [!NOTE]
  > U kunt feedback niet ontvangen als de opdracht is geslaagd. 

4. Voordat u een systeem bewaken kunt, moet u de naam van het opslagaccount dat wordt gebruikt voor Azure Diagnostics. In de Azure portal, gaat u naar **virtuele machines**. Zoek naar een virtuele Windows-computer die u wilt bewaken. In de **eigenschappen** sectie **diagnostische instellingen**.  Selecteer **Agent**. Noteer de opslagaccountnaam die opgegeven. U moet deze accountnaam voor een latere stap.

  ![Schermopname van het deelvenster Azure Diagnostics-instellingen](./media/security-azure-log-integration-get-started/storage-account-large.png) 

  ![Bewaking knop gastniveau schermopname van inschakelen](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)

  > [!NOTE]
  > Als controle is niet ingeschakeld wanneer de virtuele machine is gemaakt, kunt u deze inschakelen zoals weergegeven in de vorige afbeelding.

5. Ga nu terug naar de integratie van Azure Log-machine. Controleer of u verbinding hebt met het opslagaccount van het systeem waar u de integratie van Azure Log geïnstalleerd. De computer waarop de integratie van Azure Log-service wordt uitgevoerd moet toegang tot het opslagaccount voor het ophalen van informatie die wordt vastgelegd door Azure Diagnostics op elk van de bewaakte systemen. Om te controleren of de verbinding: 
  1. [Azure Storage Explorer downloaden](http://storageexplorer.com/).
  2. Setup is voltooid.
  3. Wanneer de installatie is voltooid, selecteert u **volgende**. Laat de **Start Microsoft Azure Storage Explorer** selectievakje is ingeschakeld.  
  4. Meld u aan bij Azure.
  5. Controleren of u het opslagaccount dat u hebt geconfigureerd voor Azure Diagnostics kan zien: 

   ![Schermafbeelding van de storage-accounts in Opslagverkenner](./media/security-azure-log-integration-get-started/storage-explorer.png)

  6. Enkele opties worden weergegeven onder de storage-accounts. Onder **tabellen**, ziet u een tabel met de naam **WADWindowsEventLogsTable**.

  Als controle is niet ingeschakeld wanneer de virtuele machine is gemaakt, kunt u, zoals eerder beschreven.


## <a name="integrate-windows-vm-logs"></a>Integratie van Windows-Logboeken voor VM

In deze stap configureert u de computer waarop de integratie van Azure Log-service verbinding maken met het opslagaccount waarin de logboekbestanden.

Deze stap voltooien, moet u een aantal dingen:  
* **FriendlyNameForSource**: een beschrijvende naam die u op het opslagaccount toepassen kunt dat u hebt geconfigureerd voor de virtuele machine voor het opslaan van gegevens van Azure Diagnostics.
* **StorageAccountName**: de naam van het opslagaccount dat u hebt opgegeven bij het instellen van Azure Diagnostics.  
* **StorageKey**: de opslagsleutel voor het opslagaccount waarin de Azure Diagnostics-gegevens zijn opgeslagen voor deze virtuele machine.  

Als u de opslagsleutel, moet u de volgende stappen uitvoeren:
1. Ga naar de [Azure Portal](http://portal.azure.com).
2. Selecteer in het navigatiedeelvenster **alle services**.
3. In de **Filter** Voer **opslag**. Selecteer **opslagaccounts**.

  ![Schermafbeelding van opslagaccounts in alle services](./media/security-azure-log-integration-get-started/filter.png)

4. Een lijst met opslagaccounts weergegeven. Dubbelklik op het account dat u aan te melden opslag toegewezen.

  ![Schermafbeelding van een lijst met storage-accounts](./media/security-azure-log-integration-get-started/storage-accounts.png)

5. Selecteer **Toegangssleutels** bij **Instellingen**.

  ![Schermafbeelding van de optie toegang sleutels in het menu](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)

6. Kopiëren **key1**, en sla deze op een veilige locatie die toegankelijk is voor de volgende stap.
7. Open op de server waarop u Azure Log integratie hebt geïnstalleerd, een opdrachtpromptvenster als administrator. (Zorg ervoor dat een opdrachtpromptvenster open als beheerder, en niet PowerShell).
8. Ga naar C:\Program Files\Microsoft Azure-logboekanalyse-integratie.
9. Deze opdracht uitvoeren: `Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>`.
 
  Voorbeeld:
  
  `Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

  Als u de abonnements-ID wilt worden weergegeven in de gebeurtenisstroom XML, moet u de abonnements-ID toevoegen aan de beschrijvende naam:

  `Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`
  
  Voorbeeld:
  
  `Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

> [!NOTE]
> Wacht tot 60 minuten en bekijk vervolgens de gebeurtenissen die worden opgehaald uit de storage-account. Selecteer om te geven de gebeurtenissen in logboek integratie van Azure, **logboeken** > **Windows-logboeken** > **doorgestuurde gebeurtenissen**.

De volgende video wordt ingegaan op de voorgaande stappen:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="if-data-isnt-showing-up-in-the-forwarded-events-folder"></a>Als de gegevens wordt niet weergegeven in de map doorgestuurde gebeurtenissen
Als de gegevens wordt niet weergegeven in de map doorgestuurde gebeurtenissen na een uur, moet u deze stappen voltooien:

1. Controleer de computer die de integratie van Azure Log-service wordt uitgevoerd. Controleer of deze toegang Azure tot. Probeer connectiviteit, testen in een browser naar de [Azure-portal](http://portal.azure.com).
2. Zorg ervoor dat het gebruikersaccount Azlog schrijfmachtiging voor de map users\Azlog heeft.
  1. Open Verkenner.
  2. Ga naar C:\users.
  3. Met de rechtermuisknop op C:\users\Azlog.
  4. Selecteer **beveiliging**.
  5. Selecteer **NT Service\Azlog**. Controleer de machtigingen voor het account. Als het account op dit tabblad ontbreekt of als de juiste machtigingen niet worden weergegeven, kunt u de machtigingen van de account op dit tabblad verlenen.
3. Wanneer u de opdracht uitvoert `Azlog source list`, zorg ervoor dat de opslag account dat is toegevoegd in de opdracht `Azlog source add` wordt vermeld in de uitvoer.
4. Als er fouten zijn gemeld van de integratie van Azure Log-service, Ga naar **logboeken** > **Windows-logboeken** > **toepassing**.

Als u problemen ondervindt tijdens de installatie en configuratie, kunt u een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer voor de service **logboek integratie**.

Een andere ondersteuningsoptie is de [Azure Log integratie MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). De community kan ondersteuning bieden door het beantwoorden van vragen en te delen tips en trucs over het ophalen van de meest buiten Azure Log-integratie in de MSDN-forum. Het team van Azure Log integratie bewaakt ook dit forum. Ze helpen wanneer ze kunnen.

## <a name="integrate-azure-activity-logs"></a>Azure activiteitenlogboeken integreren

De Azure Activity Log is een abonnementlogboek biedt inzicht in het abonnement op gebeurtenissen die hebben plaatsgevonden in Azure. Dit omvat een bereik van gegevens van operationele gegevens van de Azure Resource Manager-updates op Service Health-gebeurtenissen. Azure Security Center Alerts zijn ook opgenomen in dit logboek.
> [!NOTE]
> Voordat u de stappen in dit artikel, moet u controleren de [aan de slag](security-azure-log-integration-get-started.md) en voltooi de stappen bevat.

### <a name="steps-to-integrate-azure-activity-logs"></a>Stappen voor het integreren van Logboeken van de Azure-activiteit

1. Open de opdrachtprompt en voer deze opdracht uit:  ```cd c:\Program Files\Microsoft Azure Log Integration```
2. Deze opdracht uitvoeren:  ```azlog createazureid```

    Met deze opdracht wordt u gevraagd om uw Azure-aanmelding. De opdracht maakt u een Azure Active Directory service-principal in de Azure AD-tenants die als host fungeren van de Azure-abonnementen waarin de aangemelde gebruiker een beheerder, CO-beheerder of een eigenaar is. De opdracht mislukt als de aangemelde gebruiker een gastgebruiker in de Azure AD-tenant. Verificatie in Azure wordt gedaan door Azure AD. Maken van een service-principal voor Azure Log integratie maakt de identiteit van de Azure AD die toegang krijgt tot het lezen van de Azure-abonnementen.
3.  Voer de volgende opdracht voor het autoriseren van de integratie van Azure Log-service-principal gemaakt in de vorige stap toegang tot het lezen van het activiteitenlogboek voor het abonnement. U moet een eigenaar voor het abonnement op de opdracht uit te voeren.

    ```Azlog.exe authorize subscriptionId``` Voorbeeld:

```AZLOG.exe authorize ba2c2367-d24b-4a32-17b5-4443234859```
4.  Controleer de volgende mappen om te bevestigen dat de Azure Active Directory audit log JSON-bestanden in deze zijn gemaakt:
    - C:\Users\azlog\AzureResourceManagerJson
    - C:\Users\azlog\AzureResourceManagerJsonLD

> [!NOTE]
> Neem contact op met de leverancier van uw SIEM voor specifieke instructies op de informatie in de JSON-bestanden te brengen in uw security information en event management (SIEM)-systeem.

Hulp is beschikbaar via de [Azure Log integratie MSDN-Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Dit forum kunt mensen in de Azure Log integratie community ter ondersteuning van elkaar met vragen, antwoorden, tips en slagen. Bovendien het team van Azure Log integratie dit forum wordt bewaakt en helpt wanneer dat mogelijk is.

U kunt ook openen een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer logboek integratie als de service waarvoor u ondersteuning aanvraagt.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over de integratie van Azure Log: voordat u de stappen in dit artikel, moet u lees het artikel van Get-gestart en er de stappen uitvoeren.

* [Integratie van Azure Log voor Azure logboeken](https://www.microsoft.com/download/details.aspx?id=53324). Het Download Center bevat details, systeemvereisten en installatie-instructies voor de integratie van Azure-logboek.
* [Inleiding tot Azure-logboekanalyse integratie](security-azure-log-integration-overview.md). In dit artikel vindt u Azure Log integratie, de belangrijkste mogelijkheden en hoe het werkt.
* [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/). Dit blogbericht ziet u hoe Azure Log integratie werken met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren. Beschrijft onze richtlijnen voor het huidige over het configureren van de SIEM-onderdelen. Neem contact op met de leverancier van uw SIEM voor meer informatie.
* [Integratie van Azure Log Veelgestelde vragen (FAQ)](security-azure-log-integration-faq.md). Deze Veelgestelde vragen over de antwoorden op veelgestelde vragen over Azure Log-integratie.
* [Waarschuwingen van Azure Security Center integreren met Azure Log integratie](../security-center/security-center-integrating-alerts-with-log-integration.md). Dit artikel laat zien hoe u wilt synchroniseren Security Center-waarschuwingen en beveiligingsgebeurtenissen voor virtuele machine die door Azure Diagnostics- en Azure activiteit worden verzameld Logboeken. U kunt de logboeken synchroniseren met behulp van uw Azure-logboekanalyse of SIEM-oplossing.
* [Nieuwe functies voor Azure Diagnostics- en Azure controlelogboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/). Dit blogbericht maakt u kennis met Azure controlelogboeken en andere functies waarmee u kunnen meer inzicht krijgen in de bewerkingen van uw Azure-resources.
