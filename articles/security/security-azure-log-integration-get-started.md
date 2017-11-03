---
title: Aan de slag met Azure-logboekanalyse-integratie | Microsoft Docs
description: Informatie over het installeren van de Azure-logboekanalyse integration-service en het integreren van Logboeken van Azure storage, Azure controlelogboeken en Azure Security Center-waarschuwingen.
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
ms.date: 07/26/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: 9d39ecd513386b75b4b640721f80991caaf9ade8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure-logboekanalyse-integratie met de logboekregistratie van Azure Diagnostics- en Windows Event Forwarding
Integratie van Azure Log (AzLog) kunt u het integreren van onbewerkte logboeken van uw Azure-resources in uw on-premises Security Information en Event Management SIEM ()-systemen. Deze integratie maakt het mogelijk om een dashboard met geïntegreerde beveiliging voor alle activa, lokaal of in de cloud, zodat u kunt samenvoegen, correleren, analyseren en waarschuwing voor beveiligingsgebeurtenissen die zijn gekoppeld aan uw toepassingen.
>[!NOTE]
Raadpleeg voor meer informatie over de integratie van Azure-logboek van de [overzicht van de integratie van Azure Log](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview).

In dit artikel helpt u aan de slag met Azure Log integratie door te focussen op de installatie van de service Azlog en de service integreren met Azure Diagnostics. De integratie van Azure Log-service vervolgens kan worden Windows-gebeurtenislogboek om informatie te verzamelen van het Windows-gebeurtenis beveiligingskanaal van virtuele machines in Azure IaaS geïmplementeerd. Dit is vergelijkbaar met 'Event Forwarding' dat u on-premises mogelijk gebruikt.

>[!NOTE]
>De mogelijkheid om de uitvoer van de Azure-logboekanalyse-integratie in de SIEM wordt verstrekt door de SIEM zelf. Zie het artikel [integratie van Azure Log integratie met uw On-premises SIEM](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) voor meer informatie.

Moet duidelijk - de integratie van Azure Log-service wordt uitgevoerd op een fysieke of virtuele computer die van Windows Server 2008 R2 gebruikmaakt of hoger besturingssysteem (Windows Server 2012 R2 of Windows Server 2016 zijn voorkeur).

De fysieke computer kunt on-premises uitgevoerd (of op een site hoster). Als u kiest voor het uitvoeren van de integratie van Azure Log-service op een virtuele machine, waarop de virtuele machine mag zich lokaal of in een openbare cloud, zoals Microsoft Azure.

De fysieke of virtuele machine met de Azure Log Integration-service vereist netwerkverbinding met de openbare Azure-cloud. Stappen in dit artikel vindt u informatie van de configuratie.

## <a name="prerequisites"></a>Vereisten
De installatie van AzLog vereist ten minste de volgende items:
* Een **Azure-abonnement**. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een **opslagaccount** die kunnen worden gebruikt voor diagnostische logboekregistratie voor Windows Azure (gebruik een vooraf geconfigureerde opslagaccount of maak een nieuwe – wordt ziet u hoe het configureren van het opslagaccount verderop in dit artikel)
  >[!NOTE]
  Afhankelijk van uw scenario kan een opslagaccount niet worden vereist. Voor de Azure diagnostics is die in dit artikel in een scenario nodig.
* **Twee systemen**: een computer die de integratie van Azure Log-service wordt uitgevoerd en een machine die wordt bewaakt en de logboekregistratie informatie verzonden naar de service Azlog machine hebben.
   * Een machine die u bewaken wilt: dit is een virtuele machine wordt uitgevoerd als een [Azure virtuele Machine](../virtual-machines/virtual-machines-windows-overview.md)
   * Een machine met de Azure-logboekanalyse integration-service; Deze computer worden verzameld van de logboekgegevens die later worden geïmporteerd in uw SIEM.
    * Dit systeem kan nu on-premises of in Microsoft Azure.  
    * Het moet worden uitgevoerd een x64 versie van WindowsServer 2008 R2 SP1 of hoger en .NET Framework 4.5.1 is geïnstalleerd. U kunt bepalen de .NET-versie die is geïnstalleerd door de volgende het artikel [hoe: bepalen dat .NET Framework-versies zijn geïnstalleerd](https://msdn.microsoft.com/library/hh925568)  
    Het moet zijn verbonden met de Azure storage-account voor Azure Diagnostische logboekregistratie gebruikt. We geven instructies verderop in dit artikel over hoe u deze connectiviteit kunt bevestigen

Voor een snelle demonstratie van het proces van een maken van een virtuele machine via de Azure-portal Bekijk de video hieronder.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]



## <a name="deployment-considerations"></a>Overwegingen bij de implementatie
Als u Azure Log integratie test, kunt u een systeem dat voldoet aan de minimale besturingssysteemvereisten. Voor een productieomgeving kan de belasting vereisen echter u van plan bent voor het schalen van omhoog of uit.

Als gebeurtenis volume hoog is, kunt u meerdere exemplaren van de integratie van Azure Log-service (één exemplaar per fysieke of virtuele machine) uitvoeren. U kunt bovendien saldo opslagaccounts Azure Diagnostics voor Windows (af) en het aantal abonnementen bieden aan de exemplaren moeten worden gebaseerd op de capaciteit van de laden.
>[!NOTE]
Op dit moment er geen specifieke aanbevelingen voor het uitbreiden van exemplaren van Azure-logboekanalyse integratie machines (dat wil zeggen, machines met de Azure-logboekanalyse integration-service) of voor storage-accounts of abonnementen. Schalen van de beslissingen die moet worden gebaseerd op uw prestaties metingen in elk van deze gebieden.

U hebt ook de optie voor het schalen van de integratie van Azure Log-service om prestaties te verbeteren. De volgende maatstaven voor prestaties kan u helpen bij het formaat van de machines die u wilt uitvoeren van de Azure-logboekanalyse integration-service:
* Op een machine met 8-processor (core), kan één exemplaar van Azlog Integrator ongeveer 24 miljoen gebeurtenissen per dag (~1M/hour) verwerken.

* Op een machine 4-processor (core), kan één exemplaar van Azlog Integrator ongeveer 1,5 miljoen gebeurtenissen per dag (~62.5K/hour) verwerken.

## <a name="install-azure-log-integration"></a>Azure-logboekanalyse-integratie installeren
Om Azure Log Integration installeert, moet u downloaden de [integratie van Azure-logboekanalyse](https://www.microsoft.com/download/details.aspx?id=53324) -bestand voor installatie. De setup-routine doorlopen en bepalen of u wilt bieden telemetrie-informatie naar Microsoft.  

![Installatiescherm met telemetrie selectievakje is ingeschakeld](./media/security-azure-log-integration-get-started/telemetry.png)

*
> [!NOTE]
> Het is raadzaam dat u Microsoft voor het verzamelen van telemetriegegevens toestaan. U kunt verzamelen van telemetriegegevens uitschakelen door deze optie uitschakelt.
>


De integratie van Azure Log-service verzamelt telemetriegegevens van de computer waarop deze is geïnstalleerd.  

Verzamelde telemetriegegevens is:

* Uitzonderingen die tijdens het uitvoeren van de integratie van Azure-logboekanalyse optreden
* Metrische gegevens over het aantal query's en gebeurtenissen die zijn verwerkt
* Statistieken over welke Azlog.exe opdrachtregelopties worden gebruikt

Het installatieproces wordt beschreven in de onderstaande video.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]



## <a name="post-installation-and-validation-steps"></a>Na de installatie-en validatie
Nadat de routine basisinstellingen is voltooid, bent u gereed stap uit te voeren na de installatie en Validatiestappen:
1. Open een PowerShell-venster met verhoogde bevoegdheid en navigeer naar **c:\Program Files\Microsoft Azure Log-integratie**
2. De eerste stap die u moet nemen is om op te halen van de Cmdlets AzLog is geïmporteerd. U kunt dit doen door het uitvoeren van het script **LoadAzlogModule.ps1** (Let op de '. \ ' in de volgende opdracht). Type **.\LoadAzlogModule.ps1** en druk op **ENTER**.  
U ziet er ongeveer zo wat wordt weergegeven in de afbeelding hieronder. </br></br>
![Installatiescherm met telemetrie selectievakje is ingeschakeld](./media/security-azure-log-integration-get-started/loaded-modules.png) </br></br>
3. Nu moet u AzLog voor het gebruik van een specifieke Azure-omgeving te configureren. Een 'Azure-omgeving' is het 'type' van de Azure-cloud datacenter die u wilt werken. Hoewel er verschillende Azure-omgevingen op dit moment, zijn de opties op dit moment relevante **AzureCloud** of **AzureUSGovernment**.   Zorg ervoor dat u in de verhoogde PowerShell-omgeving **c:\program files\Microsoft Azure Log-integratie\** </br></br>
    Zodra er, voer de opdracht: </br>
    ``Set-AzlogAzureEnvironment -Name AzureCloud``(voor Azure commercieel)

      >[!NOTE]
      Als de opdracht is geslaagd, ontvangt u geen opmerkingen of vragen hebt.  Als u de US Government Azure-cloud gebruiken wilt, gebruikt u **AzureUSGovernment** (voor de variabele van-naam) voor de cloud van de overheid van de Verenigde Staten. Andere Azure-clouds worden niet ondersteund op dit moment.  
4. Voordat u een systeem kunt bewaken moet u de naam van het storage-account gebruikt voor Azure Diagnostics.  Navigeer in de Azure-portal naar **virtuele machines** en zoekt u de virtuele machine die u wilt bewaken. In de **eigenschappen** sectie **diagnostische instellingen**.  Klik op **Agent** en noteer de opslagaccountnaam die is opgegeven. U moet deze accountnaam voor een latere stap.
![Diagnostische instellingen voor Azure](./media/security-azure-log-integration-get-started/storage-account-large.png) </br></br>

      ![Diagnostische instellingen voor Azure](./media/security-azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)
      >[!NOTE]
      Als controle is niet ingeschakeld tijdens het maken van de virtuele machine krijgt u de optie in te schakelen zoals hierboven.
5. Nu schakelt we onze aandacht terug naar de Azure-logboekanalyse integratie machine. We moeten controleren of u verbinding hebt met het Opslagaccount van het systeem waar u de integratie van Azure Log geïnstalleerd. De fysieke computer of virtuele machine met de Azure Log Integration-service moet toegang tot het opslagaccount voor het ophalen van informatie die wordt geregistreerd door Azure Diagnostics zoals geconfigureerd in elk van de bewaakte systemen.  
  1. U kunt Azure Storage Explorer downloaden [hier](http://storageexplorer.com/).
  2. De setup-routine doorlopen
  3. Zodra de installatie is voltooid Klik **volgende** en laat het selectievakje **Start Microsoft Azure Storage Explorer** gecontroleerd.  
  4. Aanmelden bij Azure.
  5. Controleren of u het opslagaccount dat u hebt geconfigureerd voor Azure Diagnostics kan zien.  
![Opslagaccounts](./media/security-azure-log-integration-get-started/storage-account.jpg) </br></br>
   6. U ziet dat er een aantal opties onder storage-accounts. Een van beide is **tabellen**. Onder **tabellen** ziet u een met de naam **WADWindowsEventLogsTable**. </br></br>
   ![Storage-accounts](./media/security-azure-log-integration-get-started/storage-explorer.png) </br>

## <a name="integrate-azure-diagnostic-logging"></a>Integratie van Azure Diagnostische logboekregistratie
In deze stap configureert u de computer waarop de integratie van Azure Log-service verbinding maken met het opslagaccount waarin de logboekbestanden.
Voltooi deze stap moeten we een aantal dingen vooraf.  
* **FriendlyNameForSource:** dit is een beschrijvende naam die u kunt toepassen op het opslagaccount dat u de virtuele machine voor het opslaan van gegevens van Azure Diagnostics hebt geconfigureerd
* **StorageAccountName:** dit is de naam van het opslagaccount dat u hebt opgegeven bij het instellen van Azure diagnotics.  
* **StorageKey:** dit is de opslagsleutel voor het opslagaccount waarin de Azure Diagnostics-gegevens zijn opgeslagen voor deze virtuele machine.  

Voer de volgende stappen voor het verkrijgen van de opslagsleutel:
 1. Blader naar de [Azure-portal](http://portal.azure.com).
 2. In het navigatievenster van de Azure-console, schuif naar beneden en klik op **meer services.**

 ![Meer Services](./media/security-azure-log-integration-get-started/more-services.png)
 3. Voer **opslag** in de **Filter** in het tekstvak. Klik op **opslagaccounts** (deze wordt weergegeven nadat u hebt ingevoerd **opslag**)

   ![filtervak](./media/security-azure-log-integration-get-started/filter.png)
 4. Een lijst met opslagaccounts wordt weergegeven, dubbelklikt u op het account dat u hebt toegewezen aan opslag voor toepassingslogboeken.

   ![lijst met storage-accounts](./media/security-azure-log-integration-get-started/storage-accounts.png)
 5. Klik op **toegangssleutels** in de **instellingen** sectie.

  ![Toegangstoetsen](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 6. Kopiëren **key1** en plaatsen op een veilige locatie die toegankelijk is voor de volgende stap.

   ![twee toegangssleutels](./media/security-azure-log-integration-get-started/storage-account-access-keys.png)
 7. Open een opdrachtprompt met verhoogde bevoegdheid (Opmerking dat we maken gebruik van een verhoogd opdrachtpromptvenster hier, niet een verhoogde PowerShell-console) op de server waarop u Azure Log integratie hebt geïnstalleerd.
 8. Navigeer naar **c:\Program Files\Microsoft Azure-logboekanalyse-integratie**
 9. Voer ``Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey> `` uit. </br> Bijvoorbeeld ``Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`` als u de abonnements-ID dat wilt worden weergegeven in de gebeurtenisstroom XML, de abonnements-ID toe aan de beschrijvende naam toegevoegd: ``Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`` of bijvoorbeeld``Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==``

>[!NOTE]  
60 minuten wachten en vervolgens de gebeurtenissen die worden opgehaald uit de storage-account weergeven. Als u wilt weergeven, openen **Logboeken > Windows-Logboeken > doorgestuurde gebeurtenissen** op de Integrator Azlog.

Hier ziet u een video gaat over de stappen hierboven.
> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Wat gebeurt er als gegevens, wordt niet weergegeven in de map doorgestuurde gebeurtenissen?
Als u na een uur gegevens, wordt niet weergegeven de **doorgestuurde gebeurtenissen** map, klikt u vervolgens:

1. De computer waarop de integratie van Azure Log-service wordt uitgevoerd en controleer of deze toegang Azure tot. Connectiviteit testen probeert te openen de [Azure-portal](http://portal.azure.com) vanuit de browser.
2. Zorg ervoor dat het gebruikersaccount **Azlog** schrijfrechten heeft op de map **users\Azlog**.
  <ol type="a">
   <li>Open **Windows Verkenner** </li>
  <li> Navigeer naar **c:\users** </li>
  <li> Klik met de rechtermuisknop op **c:\users\Azlog** </li>
  <li> Klik op **beveiliging**  </li>
  <li> Klik op **NT Service\Azlog** en controleer de machtigingen voor het account. U kunt als het account op dit tabblad ontbreekt of als de juiste machtigingen momenteel niet zijn de accountrechten op dit tabblad verlenen.</li>
  </ol>
3.Zorg ervoor dat het storage-account toegevoegd in de opdracht **Azlog bron toevoegen** wordt weergegeven wanneer u de opdracht uitvoert **Azlog bronlijst**.
4. Ga naar **Logboeken > Windows-Logboeken > toepassing** om te zien of er fouten zijn gerapporteerd door de integratie van Azure-logboekanalyse.


Als u problemen ondervindt wordt uitgevoerd tijdens de installatie en configuratie, opent u een [ondersteuningsaanvraag](../azure-supportability/how-to-create-azure-support-request.md), selecteer **logboek integratie** als de service waarvoor u ondersteuning aanvraagt.

Een andere ondersteuningsoptie is de [Azure Log integratie MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). Hier kunt de community ondersteuning voor elkaar met vragen, antwoorden, tips en trucs over het ophalen van de meest buiten Azure Log-integratie. Bovendien de integratie van Azure Log-team dit forum wordt bewaakt en wanneer we kunt.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende documenten voor meer informatie over de integratie van Azure-logboek:

* [Microsoft Azure Log-integratie voor Azure logboeken](https://www.microsoft.com/download/details.aspx?id=53324) : Download Center voor meer informatie over de systeemvereisten en instructies op Azure-logboekanalyse-integratie installeren.
* [Inleiding tot Azure-logboekanalyse integratie](security-azure-log-integration-overview.md) : dit document bevat een inleiding tot Azure-logboekanalyse integratie, de belangrijkste mogelijkheden en hoe het werkt.
* [Partner configuratiestappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dit blogbericht ziet u de integratie van Azure-logboekanalyse werken met partneroplossingen Splunk, HP ArcSight en IBM QRadar configureren. Dit is de huidige richtlijnen over het configureren van de SIEM-onderdelen. Neem contact op met de leverancier van uw SIEM eerst voor meer informatie.
* [Azure-logboekanalyse integratie Veelgestelde vragen (FAQ)](security-azure-log-integration-faq.md) -deze Veelgestelde vragen over de antwoorden op vragen over Azure-logboekanalyse-integratie.
* [Waarschuwingen met Azure Security Center integreren Meld integratie](../security-center/security-center-integrating-alerts-with-log-integration.md) : dit document ziet u hoe Security Center-waarschuwingen, samen met de virtuele machine beveiligingsgebeurtenissen die door Azure Diagnostics- en Azure activiteitenlogboeken, zijn verzameld met uw logboekanalyse synchroniseren of SIEM-oplossing.
* [Nieuwe functies voor Azure diagnoses en Azure controlelogboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) : dit blogbericht maakt u kennis met Azure controlelogboeken en andere functies waarmee u inzicht in de bewerkingen van uw Azure-resources.
