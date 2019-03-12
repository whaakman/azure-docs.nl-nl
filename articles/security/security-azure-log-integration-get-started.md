---
title: Aan de slag met Azure-Logboekintegratie | Microsoft Docs
description: Informatie over het installeren van de service Azure-Logboekintegratie en logboeken van Azure Storage, Azure-auditlogboeken en Azure Security Center-waarschuwingen integreren.
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 01/14/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 7e43af7d749719c2f69df9b53766c5452931884b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57542910"
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure-Logboekintegratie met Azure Diagnostics-logboeken en Windows event forwarding


>[!IMPORTANT]
> De functie van de integratie met Azure Log worden afgeschaft door 06/01/2019. AzLog downloads zijn uitgeschakeld op 27 juni 2018. Voor informatie over wat te doen verplaatsen forward revisie van het bericht [gebruikt Azure monitor om te integreren met SIEM-hulpprogramma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

U moet Azure-Logboekintegratie alleen gebruiken als een [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) connector niet beschikbaar is via de leverancier van uw Security Incident and Event Management (SIEM).

Azure-Logboekintegratie maakt Logboeken in Azure beschikbaar voor uw SIEM zodat u een uniforme security-dashboard voor al uw activa maken kunt.
Neem contact op met de leverancier van uw SIEM voor meer informatie over de status van een Azure Monitor-connector.

> [!IMPORTANT]
> Als uw primaire interesse met het verzamelen van Logboeken van de virtuele machine, bevatten de meeste SIEM-leveranciers met deze optie in de oplossing. Met behulp van de SIEM is-connector van de leverancier altijd de voorkeur alternatief.

Dit artikel helpt u aan de slag met Azure-Logboekintegratie. Dit artikel gaat over het installeren van de Azure-Logboekintegratie-service en de service te integreren met Azure Diagnostics. De Azure-Logboekintegratie-service verzamelt vervolgens informatie over Windows-gebeurtenislogboeken van het kanaal Windows-beveiligingslogboek van virtuele machines die zijn geïmplementeerd in een Azure-infrastructuur als een service. Dit is vergelijkbaar met *-gebeurtenis doorsturen* die u mogelijk in een on-premises systeem.

> [!NOTE]
> De uitvoer van Azure-Logboekintegratie integreren met een SIEM wordt uitgevoerd door de SIEM zelf. Zie voor meer informatie, [Azure-Logboekintegratie integreren met uw on-premises SIEM](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/).

De Azure-Logboekintegratie-service wordt uitgevoerd op een fysieke of een virtuele computer met Windows Server 2008 R2 of hoger (Windows Server 2016 of Windows Server 2012 R2 heeft de voorkeur).

Een fysieke computer kunt on-premises uitvoert of op een host-site. Als u kiest voor het uitvoeren van de Azure-Logboekintegratie-service op een virtuele machine, de virtuele machine mag zich op locatie of in een openbare cloud, zoals in Microsoft Azure.

De fysieke of virtuele machine met de service Azure-Logboekintegratie vereist dat de netwerkverbinding met de openbare cloud van Azure. In dit artikel bevat informatie over de vereiste configuratie.

## <a name="prerequisites"></a>Vereisten

Ten minste, met de installatie van Azure-Logboekintegratie moeten de volgende items:

* Een **Azure-abonnement**. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een **opslagaccount** die kunnen worden gebruikt voor Windows Azure Diagnostics (WAD) registreren. U kunt een vooraf geconfigureerde opslagaccount gebruiken of een nieuw opslagaccount maken. Verderop in dit artikel wordt beschreven hoe u de storage-account te configureren.

  > [!NOTE]
  > Afhankelijk van uw scenario, een storage-account mogelijk niet vereist. Een storage-account is vereist voor de Azure Diagnostics-scenario in dit artikel besproken.

* **Twee systemen**: 
  * Een computer waarop de Azure-Logboekintegratie-service wordt uitgevoerd. Deze machine verzamelt alle logboekgegevens die later in uw SIEM is geïmporteerd. Dit systeem:
    * Kunnen zich on-premises of gehost in Microsoft Azure.  
    * Moet worden uitgevoerd als een x64 versie van Windows Server 2008 R2 SP1 of hoger, en Microsoft .NET Framework 4.5.1 is geïnstalleerd. Om te bepalen welke .NET versie is geïnstalleerd, Zie [te bepalen welke versies van .NET Framework zijn geïnstalleerd](https://msdn.microsoft.com/library/hh925568).  
    * Moet zijn verbonden met het Azure Storage-account dat is gebruikt voor logboekregistratie van Azure Diagnostics. Verderop in dit artikel wordt beschreven hoe u verbinding bevestigen.
  * Een machine die u wilt bewaken. Dit is een virtuele machine die wordt uitgevoerd als een [virtuele Azure-machine](../virtual-machines/virtual-machines-windows-overview.md). De logboekgegevens van deze machine is verzonden naar de Azure-Logboekintegratie service-machine.

Kijk eens in de volgende video voor een snelle demonstratie van hoe u een virtuele machine maken met behulp van de Azure-portal:<br /><br />


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Tijdens het testen, kunt u elk systeem dat voldoet aan de minimale besturingssysteemvereisten. Voor een productie-omgeving, de belasting moet u mogelijk om te plannen voor omhoog of uitschalen.

U kunt meerdere exemplaren van de service Azure-Logboekintegratie uitvoeren. U kunt echter slechts één exemplaar van de service per fysieke of virtuele machine uitvoeren. Bovendien kunt u taakverdeling diagnostische gegevens van Azure storage-accounts voor WAD. Het aantal abonnementen voor voor de exemplaren is gebaseerd op uw capaciteit.

> [!NOTE]
> Op dit moment zijn er geen specifieke aanbevelingen over wanneer u de instanties van Azure-Logboekintegratie machines (dat wil zeggen, machines waarop de Azure-Logboekintegratie-service wordt uitgevoerd) of uitschalen voor storage-accounts of -abonnementen. Vergroten/verkleinen beslissingen op basis van uw metingen prestaties in elk van deze gebieden.

Om prestaties te verbeteren, hebt u ook de mogelijkheid om te schalen van de Azure-Logboekintegratie-service. Aan de hand van de volgende metrische gegevens voor prestaties kunt u het formaat van de machines die u kiest voor het uitvoeren van de Azure-Logboekintegratie-service:

* Op de computer van een 8-processor (core), kan één exemplaar van Azure-Logboekintegratie ongeveer 24 uur per dag miljoen gebeurtenissen per dag (ongeveer 1 miljoen gebeurtenissen per uur) verwerken.
* Op een computer 4-processor (core), kan één exemplaar van Azure-Logboekintegratie ongeveer 1,5 miljoen gebeurtenissen per dag (ongeveer 62,500 gebeurtenissen per uur) verwerken.

## <a name="install-azure-log-integration"></a>Installeren van Azure-Logboekintegratie

Uitvoeren via de instellen routine. Kies of u telemetrie om informatie te verstrekken aan Microsoft.

De service Azure-Logboekintegratie verzamelt telemetriegegevens van de machine waarop deze geïnstalleerd.  

Telemetrische gegevens die worden verzameld omvat het volgende:

* De uitzonderingen die tijdens het uitvoeren van Azure-Logboekintegratie optreden.
* Metrische gegevens over het aantal query's en gebeurtenissen verwerkt.
* Statistieken over welke Azlog.exe opdrachtregelopties worden gebruikt. 

> [!NOTE]
> Het is raadzaam dat u toestaan dat Microsoft verzamelt telemetriegegevens. U kunt het verzamelen van telemetriegegevens uitschakelen door uit te schakelen de **Microsoft toestemming geven voor het verzamelen van telemetriegegevens** selectievakje.
>

![Schermafbeelding van het deelvenster installatie met het selectievakje telemetrie is ingeschakeld](./media/security-azure-log-integration-get-started/telemetry.png)


Het installatieproces wordt beschreven in de volgende video:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]

## <a name="post-installation-and-validation-steps"></a>Stappen na de installatie en validatie

Nadat u de basisconfiguratie hebt voltooid, u kunt na de installatie en validatie aan de stappen uitvoeren:

1. Open PowerShell als beheerder. Ga vervolgens naar C:\Program Files\Microsoft Azure-Logboekintegratie.
2. Importeer de Azure-Logboekintegratie-cmdlets. Voer het script voor het importeren van de cmdlets `LoadAzlogModule.ps1`. ENTER `.\LoadAzlogModule.ps1`, en druk op Enter (Let op het gebruik van **.\\**  in deze opdracht). U ziet er ongeveer als wat wordt weergegeven in de volgende afbeelding:

  ![Schermafbeelding van de uitvoer van de opdracht LoadAzlogModule.ps1](./media/security-azure-log-integration-get-started/loaded-modules.png)
3. Configureer vervolgens de Azure-Logboekintegratie voor het gebruik van een specifieke Azure-omgeving. Een *Azure-omgeving* is het type Azure-clouddatacenter die u wilt werken. Hoewel er verschillende Azure-omgevingen, die momenteel zijn, zijn de desbetreffende opties **AzureCloud** of **AzureUSGovernment**. Uitvoeren van PowerShell als beheerder, zorg ervoor dat u zich in C:\Program Files\Microsoft Azure Log Integration\. Voer deze opdracht uit:

  `Set-AzlogAzureEnvironment -Name AzureCloud` (voor **AzureCloud**)
  
  Als u gebruiken de US Government Azure-cloud wilt, gebruikt u **AzureUSGovernment** voor de **-naam** variabele. Op dit moment worden niet andere Azure-clouds ondersteund.  

  > [!NOTE]
  > U kunt feedback niet ontvangen wanneer de opdracht is geslaagd. 

4. Voordat u een systeem bewaken kunt, moet u de naam van het opslagaccount dat wordt gebruikt voor Azure Diagnostics. In de Azure-portal, gaat u naar **virtuele machines**. Zoek naar een Windows virtuele machine die u wilt bewaken. In de **eigenschappen** sectie, selecteer **diagnostische instellingen**.  Selecteer **Agent**. Noteer de naam van opslagaccount dat opgegeven. U moet deze accountnaam voor een latere stap.

  ![Schermafbeelding van het deelvenster diagnostische instellingen van Azure](./media/security-azure-log-integration-get-started/storage-account-large.png) 

  ![Schermafbeelding van inschakelen bewaking op gastniveau-knop](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)

  > [!NOTE]
  > Als controle is niet ingeschakeld wanneer de virtuele machine is gemaakt, kunt u dit inschakelen zoals wordt weergegeven in de voorgaande afbeelding.

5. Ga nu terug naar de Azure-Logboekintegratie-machine. Controleer of dat u verbinding hebt met de storage-account uit het systeem waarop u Azure-Logboekintegratie hebt geïnstalleerd. De computer waarop de service Azure-Logboekintegratie moet toegang hebben tot het opslagaccount om informatie die wordt vastgelegd door Azure Diagnostics op elk van de bewaakte systemen te halen. Connectiviteit controleren: 
  1. [Download Azure Storage Explorer](http://storageexplorer.com/).
  2. Voer setup uit.
  3. Wanneer de installatie is voltooid, selecteert u **volgende**. Laat de **Start Microsoft Azure Storage Explorer** selectievakje is ingeschakeld.  
  4. Meld u aan bij Azure.
  5. Controleer of u het opslagaccount dat u hebt geconfigureerd voor Azure Diagnostics kunt zien: 

   ![Schermopname van het storage-accounts in Storage Explorer](./media/security-azure-log-integration-get-started/storage-explorer.png)

  6. Een paar opties worden weergegeven onder de storage-accounts. Onder **tabellen**, ziet u een tabel met de naam **WADWindowsEventLogsTable**.

  Als controle is niet ingeschakeld wanneer de virtuele machine is gemaakt, kunt u, zoals eerder beschreven.


## <a name="integrate-windows-vm-logs"></a>Windows VM-logboeken integreren

In deze stap configureert u de computer waarop de service Azure-Logboekintegratie verbinding maken met de storage-account met de logboekbestanden.

Als u wilt deze stap hebt voltooid, moet u een aantal dingen:  
* **FriendlyNameForSource**: Een beschrijvende naam die u op het opslagaccount toepassen kunt dat u hebt geconfigureerd voor de virtuele machine voor het opslaan van gegevens uit Azure Diagnostics.
* **StorageAccountName**: De naam van het opslagaccount dat u hebt opgegeven bij het configureren van Azure Diagnostics.  
* **StorageKey**: De opslagsleutel voor het opslagaccount waarin de Azure Diagnostics-gegevens zijn opgeslagen voor deze virtuele machine.  

Als u wilt de opslagsleutel, voert u de volgende stappen uit:
1. Ga naar de [Azure Portal](http://portal.azure.com).
2. Selecteer in het navigatiedeelvenster **alle services**.
3. In de **Filter** Voer **opslag**. Selecteer **opslagaccounts**.

  ![Schermafbeelding van storage-accounts in alle services](./media/security-azure-log-integration-get-started/filter.png)

4. Er wordt een lijst weergegeven van de storage-accounts. Dubbelklik op het account dat u aan te melden opslag toegewezen.

  ![Schermafbeelding van een lijst met storage-accounts](./media/security-azure-log-integration-get-started/storage-accounts.png)

5. Selecteer **Toegangssleutels** bij **Instellingen**.

  ![Schermafbeelding van de optie toegang sleutels in het menu](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)

6. Kopie **key1**, en sla deze op een veilige locatie die toegankelijk is voor de volgende stap.
7. Open een opdrachtpromptvenster als beheerder op de server waarop u Azure-Logboekintegratie hebt geïnstalleerd. (Zorg ervoor dat open een opdrachtpromptvenster als beheerder, en niet PowerShell).
8. Ga naar C:\Program Files\Microsoft Azure-Logboekintegratie.
9. Voer deze opdracht uit: `Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>`.
 
  Voorbeeld:
  
  `Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

  Als u de abonnements-ID wilt worden weergegeven in de gebeurtenis XML, moet u de abonnements-ID toevoegen aan de beschrijvende naam:

  `Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`
  
  Voorbeeld:
  
  `Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

> [!NOTE]
> Wacht tot 60 minuten en bekijk vervolgens de gebeurtenissen die worden opgehaald uit het opslagaccount. Als u wilt de gebeurtenissen in Azure-Logboekintegratie weergeven, selecteert u **logboeken** > **Windows logboeken** > **doorgestuurde gebeurtenissen**.

De volgende video bevat informatie over de voorgaande stappen:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="if-data-isnt-showing-up-in-the-forwarded-events-folder"></a>Als gegevens niet wordt weergegeven in de map doorgestuurde gebeurtenissen
Als gegevens niet wordt weergegeven in de map doorgestuurde gebeurtenissen na een uur, voert u deze stappen uit:

1. Controleer de machine waarop de Azure-Logboekintegratie-service wordt uitgevoerd. Controleer of deze toegang heeft tot Azure. Connectiviteit wilt testen, in een browser, probeert u naar de [Azure-portal](https://portal.azure.com).
2. Zorg ervoor dat het gebruikersaccount Azlog schrijfmachtiging voor de map users\Azlog heeft.
  1. Open Verkenner.
  2. Ga naar C:\users.
  3. Met de rechtermuisknop op C:\users\Azlog.
  4. Selecteer **Security**.
  5. Selecteer **NT Service\Azlog**. Controleer de machtigingen voor het account. Als het account ontbreekt op dit tabblad, of als de juiste machtigingen niet worden weergegeven, kunt u de machtigingen van de account op dit tabblad verlenen.
3. Wanneer u de opdracht uitvoert `Azlog source list`, zorg ervoor dat het opslagaccount dat is toegevoegd in de opdracht `Azlog source add` wordt weergegeven in de uitvoer.
4. Als u wilt zien als er fouten worden gerapporteerd door de service Azure-Logboekintegratie, gaat u naar **logboeken** > **Windows logboeken** > **toepassing**.

Als u problemen ondervindt tijdens de installatie en configuratie, kunt u een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer voor de service, **Logboekintegratie**.

Een andere ondersteuningsoptie is de [Log integratie van MSDN-forum Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). In de MSDN-forum bieden de community ondersteuning door het beantwoorden van vragen en tips en trucs over hoe u optimaal gebruikmaken van Azure-Logboekintegratie delen. Het team van Azure-Logboekintegratie controleert ook dit forum. Ze helpen wanneer ze kunnen.

## <a name="integrate-azure-activity-logs"></a>Azure-activiteitenlogboeken integreren

De Azure-activiteitenlogboek is een abonnementlogboek biedt inzicht in gebeurtenissen op abonnementsniveau die hebben plaatsgevonden in Azure. Dit omvat een scala aan gegevens van operationele gegevens van de Azure Resource Manager-updates op Service Health-gebeurtenissen. De Azure Security Center-waarschuwingen zijn ook opgenomen in dit logboek.
> [!NOTE]
> Voordat u de stappen in dit artikel, moet u controleren de [aan de slag](security-azure-log-integration-get-started.md) en voltooi de stappen bevat.

### <a name="steps-to-integrate-azure-activity-logs"></a>Stappen voor het Azure-activiteitenlogboeken integreren

1. Open de opdrachtprompt en voer deze opdracht uit:  ```cd c:\Program Files\Microsoft Azure Log Integration```
2. Voer deze opdracht uit:  ```azlog createazureid```

    Met deze opdracht vraagt u om uw Azure-aanmelding. De opdracht maakt u een Azure Active Directory service-principal in de Azure AD-tenants die als host fungeren de waarin zich de aangemelde gebruiker een beheerder, CO-beheerder of eigenaar van een Azure-abonnementen. De opdracht mislukt als de gebruiker aangemeld alleen een gastgebruiker in de Azure AD-tenant is. Azure-verificatie wordt gedaan via Azure AD. Een service-principal voor Azure-Logboekintegratie maakt, wordt de Azure AD-identiteit die toegang krijgt tot het lezen van de Azure-abonnementen.
3.  Voer de volgende opdracht om de Azure-Logboekintegratie service-principal gemaakt in de vorige stap toegang tot het lezen van het activiteitenlogboek voor het abonnement. U moet een eigenaar van het abonnement van de opdracht uit te voeren.

    ```Azlog.exe authorize subscriptionId``` Voorbeeld:

   ```AZLOG.exe authorize ba2c2367-d24b-4a32-17b5-4443234859```

4.  Controleer de volgende mappen om te bevestigen dat de Azure Active Directory audit log JSON-bestanden worden gemaakt in deze:
    - C:\Users\azlog\AzureResourceManagerJson
    - C:\Users\azlog\AzureResourceManagerJsonLD

> [!NOTE]
> Neem contact op met de leverancier van uw SIEM voor specifieke instructies over hoe u de informatie in de JSON-bestanden in uw beveiligingsgegevens en event management (SIEM) system brengt.

Hulp is beschikbaar via de [integratie van MSDN-Forum van Azure Log](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Dit forum kunt mensen in de Azure-Logboekintegratie community ter ondersteuning van elkaar met vragen, antwoorden, tips en trucs. Bovendien het team van Azure-Logboekintegratie dit forum wordt bewaakt en helpt wanneer dat mogelijk is.

U kunt ook openen een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md). Selecteer Logboekintegratie als de service waarvoor u ondersteuning aanvraagt.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-Logboekintegratie, de volgende artikelen: Voordat u de stappen in dit artikel, moet u lees dan het artikel van Get-gestarte en voltooi de stappen bevat.

* [Inleiding tot Azure-Logboekintegratie](security-azure-log-integration-overview.md). In dit artikel vindt u Azure-Logboekintegratie, de belangrijkste mogelijkheden en hoe het werkt.
* [Stappen voor het configureren van partners](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/). Dit blogbericht wordt beschreven hoe u Azure-Logboekintegratie om te werken met oplossingen van partners Splunk, HP ArcSight en IBM QRadar configureren. Hierin wordt beschreven onze huidige richtlijnen over het configureren van de SIEM-onderdelen. Neem contact op met de leverancier van uw SIEM voor meer informatie.
* [Veelgestelde vragen (FAQ) Azure-Logboekintegratie](security-azure-log-integration-faq.md). Deze Veelgestelde vragen vindt u antwoorden op veelgestelde vragen over Azure-Logboekintegratie.
* [Azure Security Center-waarschuwingen integreren met Azure-Logboekintegratie](../security-center/security-center-integrating-alerts-with-log-integration.md). Dit artikel leest u hoe u wilt synchroniseren van waarschuwingen van Beveiligingscentrum en VM-beveiligingsgebeurtenissen die worden verzameld door Azure Diagnostics en Azure-activiteit Logboeken. U kunt de logboeken synchroniseren met behulp van de logboeken van Azure Monitor of de SIEM-oplossing.
* [Nieuwe functies voor Azure Diagnostics en Azure-auditlogboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/). Dit blogbericht bevat een inleiding tot Azure-auditlogboeken en andere functies die u kunnen helpen meer inzicht krijgen in de bewerkingen van uw Azure-resources.
