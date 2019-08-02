---
title: Aan de slag met Azure Log Integration | Microsoft Docs
description: Meer informatie over het installeren van de Azure Log Integration-service en het integreren van logboeken van Azure Storage, Azure audit logs en Azure Security Center-waarschuwingen.
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: 53f67a7c-7e17-4c19-ac5c-a43fabff70e1
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 05/28/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 861a72b6abdc3d06cc4737630c92d34eee2e8545
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727674"
---
# <a name="azure-log-integration-with-azure-diagnostics-logging-and-windows-event-forwarding"></a>Azure Log Integration met Azure Diagnostics logboek registratie en Windows Event forwarding


>[!IMPORTANT]
> De Azure-functie voor logboek integratie wordt afgeschaft door 06/15/2019. AzLog-down loads zijn op 27 juni 2018 uitgeschakeld. Raadpleeg voor meer informatie over wat u verder kunt doen met [Azure monitor om te integreren met Siem-hulpprogram ma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Gebruik Azure-logboek integratie alleen als er geen [Azure monitor](/azure/monitoring-and-diagnostics/monitoring-get-started) -connector beschikbaar is van uw Siem-leverancier (Security Incident and Event Management).

Azure Log Integration maakt Azure-logboeken beschikbaar voor uw SIEM zodat u een geïntegreerd beveiligings dashboard kunt maken voor al uw activa.
Neem contact op met uw SIEM-leverancier voor meer informatie over de status van een Azure Monitor-connector.

> [!IMPORTANT]
> Als de logboeken van de virtuele machine worden verzameld, bevatten de meeste SIEM-leveranciers deze optie in hun oplossing. Het gebruik van de SIEM-connector van de leverancier is altijd het voor keur alternatief.

Dit artikel helpt u aan de slag te gaan met Azure Log Integration. Het is gericht op het installeren van de Azure Log Integration-service en het integreren van de service met Azure Diagnostics. De Azure Log Integration-service verzamelt vervolgens informatie over het Windows-gebeurtenis logboek van het Windows-beveiligings gebeurtenis kanaal van virtuele machines die zijn geïmplementeerd in een Azure-infra structuur als een service. Dit is vergelijkbaar met het *door sturen van gebeurtenissen* die u kunt gebruiken in een on-premises systeem.

> [!NOTE]
> Het integreren van de uitvoer van Azure Log Integration met een SIEM wordt uitgevoerd door de SIEM zelf. Zie [Azure log Integration integreren met uw on-premises Siem](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/)voor meer informatie.

De Azure Log Integration-service wordt uitgevoerd op een fysieke of virtuele computer met Windows Server 2008 R2 of hoger (de voor keur is Windows Server 2016 of Windows Server 2012 R2).

Een fysieke computer kan on-premises of op een hosting site worden uitgevoerd. Als u ervoor kiest om de Azure Log Integration-service op een virtuele machine uit te voeren, kan de virtuele machine on-premises of in een open bare Cloud worden geplaatst, zoals in Microsoft Azure.

De fysieke of virtuele machine waarop de Azure Log Integration-service wordt uitgevoerd, vereist een netwerk verbinding met de open bare Azure-Cloud. In dit artikel vindt u informatie over de vereiste configuratie.

## <a name="prerequisites"></a>Vereisten

Voor het installeren van Azure Log Integration hebt u Mini maal de volgende items nodig:

* Een **Azure-abonnement**. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een **opslag account** dat kan worden gebruikt voor de logboek registratie van Windows Azure Diagnostics (WAD). U kunt een vooraf geconfigureerd opslag account gebruiken of een nieuw opslag account maken. Verderop in dit artikel wordt beschreven hoe u het opslag account configureert.

  > [!NOTE]
  > Afhankelijk van uw scenario is er mogelijk geen opslag account vereist. Voor het Azure Diagnostics scenario dat in dit artikel wordt behandeld, is een opslag account vereist.

* **Twee systemen**: 
  * Een computer waarop de Azure Log Integration-service wordt uitgevoerd. Deze computer verzamelt alle logboek gegevens die later worden geïmporteerd in uw SIEM. Dit systeem:
    * Kan on-premises zijn of worden gehost in Microsoft Azure.  
    * Er moet een x64-versie van Windows Server 2008 R2 SP1 of hoger worden uitgevoerd en Microsoft .NET 4.5.1 is geïnstalleerd. Zie [bepalen welke .NET Framework versies zijn geïnstalleerd](https://msdn.microsoft.com/library/hh925568)om te bepalen welke .net-versie er is geïnstalleerd.  
    * Moet verbinding hebben met het Azure Storage-account dat wordt gebruikt voor Azure Diagnostics logboek registratie. Verderop in dit artikel wordt beschreven hoe de verbinding moet worden bevestigd.
  * Een computer die u wilt bewaken. Dit is een VM die wordt uitgevoerd als een [virtuele machine van Azure](/azure/virtual-machines/virtual-machines-windows-overview). De logboek gegevens van deze computer worden naar de Azure Log Integration-service computer verzonden.

Bekijk de volgende video voor een snelle demonstratie van het maken van een virtuele machine met behulp van de Azure Portal:<br /><br />


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Create-a-Virtual-Machine/player]

## <a name="deployment-considerations"></a>Overwegingen bij de implementatie

Tijdens het testen kunt u elk systeem gebruiken dat voldoet aan de minimum vereisten voor het besturings systeem. Voor een productie omgeving moet u er mogelijk voor zorgen dat u moet plannen voor het schalen van omhoog of schalen.

U kunt meerdere exemplaren van de Azure Log Integration-service uitvoeren. U kunt echter slechts één exemplaar van de service per fysieke of virtuele machine uitvoeren. Daarnaast kunt u taak verdeling Azure Diagnostics opslag accounts voor WAD. Het aantal abonnementen dat moet worden verstrekt aan de instanties is gebaseerd op uw capaciteit.

> [!NOTE]
> Momenteel hebben we geen specifieke aanbevelingen over wanneer u exemplaren van Azure Log Integration machines uitbreidt (dat wil zeggen, machines met de Azure Log Integration-service) of voor opslag accounts of-abonnementen. Maak beslissingen op basis van uw prestatie waarnemingen in elk van deze gebieden.

U kunt de prestaties verbeteren door de Azure Log Integration-service ook omhoog te schalen. Met de volgende metrische gegevens over prestaties kunt u de grootte van de computers die u kiest om de Azure Log Integration-service uit te voeren:

* Op een computer met 8 processors (kern geheugens) kan één exemplaar van Azure Log Integration ongeveer 24.000.000 gebeurtenissen per dag verwerken (ongeveer 1.000.000 gebeurtenissen per uur).
* Op een computer met 4 processors (kern geheugens) kan één exemplaar van Azure Log Integration ongeveer 1.500.000 gebeurtenissen per dag verwerken (ongeveer 62.500 gebeurtenissen per uur).

## <a name="install-azure-log-integration"></a>Azure Log Integration installeren

Voer uit met de instellings routine. Kies of u telemetrie-informatie aan micro soft wilt geven.

De Azure Log Integration-service verzamelt telemetriegegevens van de computer waarop deze is geïnstalleerd.  

De verzamelde telemetriegegevens omvatten het volgende:

* Uitzonde ringen die optreden tijdens het uitvoeren van Azure Log Integration.
* Metrische gegevens over het aantal verwerkte query's en gebeurtenissen.
* Statistieken over welke Azlog. exe-opdracht regel opties worden gebruikt. 

> [!NOTE]
> U kunt micro soft het beste telemetrie-gegevens te laten verzamelen. U kunt het verzamelen van telemetriegegevens uitschakelen door het selectie vakje **micro soft toestaan om telemetriegegevens te verzamelen** in te scha kelen.
>

![Scherm afbeelding van het deel venster installatie, met het selectie vakje telemetrie geselecteerd](./media/azure-log-integration-get-started/telemetry.png)


Het installatie proces wordt behandeld in de volgende video:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Install-Azure-Log-Integration/player]

## <a name="post-installation-and-validation-steps"></a>Stappen na de installatie en validatie

Nadat u de basis installatie hebt voltooid, kunt u de stappen na de installatie en validatie uitvoeren:

1. Open PowerShell als beheerder. Ga vervolgens naar C:\Program Files\Microsoft Azure Log Integration.
2. Importeer de Azure Log Integration-cmdlets. Voer het script `LoadAzlogModule.ps1`uit om de cmdlets te importeren. Voer `.\LoadAzlogModule.ps1`in en druk vervolgens op ENTER (Let op het gebruik van **.\\**  in deze opdracht). Er wordt een fout weer gegeven zoals in de volgende afbeelding:

   ![Scherm afbeelding van de uitvoer van de opdracht LoadAzlogModule. ps1](./media/azure-log-integration-get-started/loaded-modules.png)
3. Configureer vervolgens Azure Log Integration voor het gebruik van een specifieke Azure-omgeving. Een *Azure-omgeving* is het type Azure-Cloud centrum waarmee u wilt werken. Hoewel er meerdere Azure-omgevingen zijn, zijn er momenteel de relevante opties **Cloud** of **AzureUSGovernment**. Power shell uitvoeren als beheerder, zorg ervoor dat u zich in C:\Program Files\Microsoft Azure log Integration\. bevindt Voer vervolgens de volgende opdracht uit:

   `Set-AzlogAzureEnvironment -Name AzureCloud`(voor **Cloud**)
  
   Als u de Azure-Cloud voor de Amerikaanse overheid wilt gebruiken, gebruikt u **AzureUSGovernment** voor de variabele **-name** . Momenteel worden andere Azure-Clouds niet ondersteund.  

   > [!NOTE]
   > U ontvangt geen feedback wanneer de opdracht is geslaagd. 

4. Voordat u een systeem kunt bewaken, hebt u de naam nodig van het opslag account dat wordt gebruikt voor Azure Diagnostics. Ga in het Azure Portal naar **virtuele machines**. Zoek naar een virtuele Windows-machine die u wilt bewaken. Selecteer **Diagnostische instellingen**in de sectie **Eigenschappen** .  Selecteer vervolgens **agent**. Noteer de naam van het opslag account dat is opgegeven. U hebt deze account naam nodig voor een latere stap.

   ![Scherm opname van het deel venster met Azure Diagnostics instellingen](./media/azure-log-integration-get-started/storage-account-large.png) 

   ![Scherm opname van de knop bewaking op gast niveau inschakelen](./media/azure-log-integration-get-started/azure-monitoring-not-enabled-large.png)

   > [!NOTE]
   > Als controle niet is ingeschakeld toen de virtuele machine werd gemaakt, kunt u deze inschakelen zoals wordt weer gegeven in de vorige afbeelding.

5. Ga nu terug naar de Azure Log Integration machine. Controleer of u verbinding hebt met het opslag account van het systeem waarop u Azure Log Integration hebt geïnstalleerd. De computer waarop de Azure Log Integration-service wordt uitgevoerd, moet toegang hebben tot het opslag account om gegevens op te halen die door Azure Diagnostics op elk van de bewaakte systemen worden vastgelegd. De connectiviteit controleren: 
   1. [Down load Azure Storage Explorer](https://storageexplorer.com/).
   2. Voltooi de installatie.
   3. Wanneer de installatie is voltooid, selecteert u **volgende**. Zorg ervoor dat het selectie vakje **start Microsoft Azure Storage Explorer** ingeschakeld.  
   4. Meld u aan bij Azure.
   5. Controleer of u het opslag account kunt zien dat u hebt geconfigureerd voor Azure Diagnostics: 

   ![Scherm opname van opslag accounts in Storage Explorer](./media/azure-log-integration-get-started/storage-explorer.png)

   1. Er worden enkele opties weer gegeven onder opslag accounts. Onder **tabellen**ziet u een tabel met de naam **WADWindowsEventLogsTable**.

   Als controle niet is ingeschakeld toen de virtuele machine werd gemaakt, kunt u deze inschakelen, zoals eerder is beschreven.


## <a name="integrate-windows-vm-logs"></a>Windows VM-logboeken integreren

In deze stap configureert u de computer waarop de Azure Log Integration-service wordt uitgevoerd, om verbinding te maken met het opslag account dat de logboek bestanden bevat.

Voor het uitvoeren van deze stap hebt u een aantal dingen nodig:  
* **FriendlyNameForSource**: Een beschrijvende naam die u kunt Toep assen op het opslag account dat u hebt geconfigureerd voor de virtuele machine om gegevens op te slaan van Azure Diagnostics.
* **StorageAccountName**: De naam van het opslag account dat u hebt opgegeven bij het configureren van Azure Diagnostics.  
* **StorageKey**: De opslag sleutel voor het opslag account waarin de Azure Diagnostics informatie voor deze virtuele machine wordt opgeslagen.  

Voer de volgende stappen uit om de opslag sleutel te verkrijgen:
1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Selecteer in het navigatie venster **alle services**.
3. Voer in het vak **filter** de **opslag ruimte**in. Selecteer vervolgens **opslag accounts**.

   ![Scherm opname van opslag accounts in alle services](./media/azure-log-integration-get-started/filter.png)

4. Er wordt een lijst met opslag accounts weer gegeven. Dubbel klik op het account dat u hebt toegewezen aan logboek opslag.

   ![Scherm opname van een lijst met opslag accounts](./media/azure-log-integration-get-started/storage-accounts.png)

5. Selecteer **Toegangssleutels** bij **Instellingen**.

   ![Scherm afbeelding van de optie toegangs sleutels in het menu](./media/azure-log-integration-get-started/storage-account-access-keys.png)

6. Kopieer **key1**en sla deze op een veilige locatie op die u voor de volgende stap kunt gebruiken.
7. Open een opdracht prompt venster als beheerder op de server waarop u Azure Log Integration hebt geïnstalleerd. (Open een opdracht prompt venster als beheerder en niet Power shell).
8. Ga naar C:\Program Files\Microsoft Azure Log Integration.
9. Voer deze opdracht uit `Azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>`:.
 
   Voorbeeld:
  
   `Azlog source add Azlogtest WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

   Als u wilt dat de abonnements-ID wordt weer gegeven in de gebeurtenis-XML, voegt u de abonnements-ID toe aan de beschrijvende naam:

   `Azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>`
  
   Voorbeeld:
  
   `Azlog source add Azlogtest.YourSubscriptionID WAD Azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==`

> [!NOTE]
> Wacht tot 60 minuten en bekijk vervolgens de gebeurtenissen die worden opgehaald uit het opslag account. Als u de gebeurtenissen wilt weer geven, selecteert u in azure log Integration **Logboeken** > doorgestuurde**gebeurtenissen**van**Windows** > -Logboeken.

In de volgende video worden de voor gaande stappen behandeld:<br /><br />

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Enable-Diagnostics-and-Storage/player]


## <a name="if-data-isnt-showing-up-in-the-forwarded-events-folder"></a>Als er geen gegevens worden weer gegeven in de map doorgestuurde gebeurtenissen
Als de gegevens na een uur niet worden weer gegeven in de map doorgestuurde gebeurtenissen, voert u de volgende stappen uit:

1. Controleer de computer waarop de Azure Log Integration-service wordt uitgevoerd. Bevestig dat het toegang tot Azure heeft. Als u de verbinding wilt testen, probeert u in een browser naar de [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat het gebruikers account Azlog schrijf machtiging heeft voor de map users\Azlog.
   1. Open Verkenner.
   2. Ga naar C:\Users.
   3. Klik met de rechter muisknop op C:\users\Azlog.
   4. Selecteer **beveiliging**.
   5. Selecteer **NT Service\Azlog**. Controleer de machtigingen voor het account. Als het account ontbreekt op dit tabblad of als de juiste machtigingen niet worden weer gegeven, kunt u de account machtigingen op dit tabblad verlenen.
3. Wanneer u de opdracht `Azlog source list`uitvoert, moet u ervoor zorgen dat het opslag account dat is toegevoegd aan de opdracht `Azlog source add` wordt weer gegeven in de uitvoer.
4. Als u wilt zien of er fouten worden gerapporteerd van de Azure log Integration-service, gaat u naar **Logboeken** > **Windows** > -logboeken-**toepassing**.

Als u problemen ondervindt tijdens de installatie en configuratie, kunt u een [ondersteunings aanvraag](../../azure-supportability/how-to-create-azure-support-request.md)maken. Selecteer voor de service **logboek integratie**.

Een andere ondersteunings optie is het [Azure log Integration MSDN-forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureLogIntegration). In het MSDN-forum kan de Community ondersteuning bieden door vragen te beantwoorden en tips en trucs te delen over hoe u optimaal gebruik kunt maken van Azure Log Integration. Het Azure Log Integration-team bewaakt ook dit forum. Ze helpen elke keer wanneer dat mogelijk is.

## <a name="integrate-azure-activity-logs"></a>Azure-activiteiten logboeken integreren

De Azure-activiteitenlogboek is een abonnementlogboek biedt inzicht in gebeurtenissen op abonnementsniveau die hebben plaatsgevonden in Azure. Dit omvat een reeks gegevens, van Azure Resource Manager operationele gegevens naar updates op Service Health-gebeurtenissen. De Azure Security Center waarschuwingen zijn ook opgenomen in dit logboek.
> [!NOTE]
> Voordat u de stappen in dit artikel probeert uit te voeren, moet u het artikel [aan de slag](azure-log-integration-get-started.md) bekijken en de stappen daar volt ooien.

### <a name="steps-to-integrate-azure-activity-logs"></a>Stappen voor het integreren van Azure-activiteiten logboeken

1. Open de opdracht prompt en voer deze opdracht uit:```cd c:\Program Files\Microsoft Azure Log Integration```
2. Voer deze opdracht uit:```azlog createazureid```

    Met deze opdracht wordt u gevraagd om uw Azure-aanmelding. De opdracht maakt vervolgens een Azure Active Directory Service-Principal in de Azure AD-tenants die als host fungeren voor de Azure-abonnementen waarin de aangemelde gebruiker een beheerder, een mede beheerder of een eigenaar is. Als de aangemelde gebruiker alleen een gast gebruiker in de Azure AD-Tenant is, mislukt de opdracht. Verificatie voor Azure wordt uitgevoerd via Azure AD. Het maken van een service-principal voor Azure Log Integration maakt de Azure AD-identiteit die toegang krijgt voor het lezen van Azure-abonnementen.
3. Voer de volgende opdracht uit om de Azure Log Integration service-principal die u in de vorige stap hebt gemaakt, te machtigen voor het lezen van het activiteiten logboek voor het abonnement. U moet een eigenaar van het abonnement zijn om de opdracht uit te voeren.

   ```Azlog.exe authorize subscriptionId```Hierbij

   ```AZLOG.exe authorize ba2c2367-d24b-4a32-17b5-4443234859```

4. Controleer de volgende mappen om te controleren of de Azure Active Directory de JSON-bestanden van het audit logboek in deze zijn gemaakt:
   - C:\Users\azlog\AzureResourceManagerJson
   - C:\Users\azlog\AzureResourceManagerJsonLD

> [!NOTE]
> Neem contact op met de leverancier van uw SIEM voor specifieke instructies over het inbrengen van de informatie in de JSON-bestanden in uw SIEM-systeem (Security Information and Event Management).

Community-ondersteuning is beschikbaar via het [MSDN-forum van Azure log Integration](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Met dit forum kunnen mensen in de Azure Log Integration Community elkaar ondersteunen met behulp van vragen, antwoorden, tips en trucs. Daarnaast controleert het Azure Log Integration-team dit forum en kunt u altijd op elk gewenst moment.

U kunt ook een ondersteunings [aanvraag](../../azure-supportability/how-to-create-azure-support-request.md)openen. Selecteer integratie van Logboeken als de service waarvoor u ondersteuning aanvraagt.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over Azure Log Integration: Voordat u de stappen in dit artikel probeert uit te voeren, moet u het artikel aan de slag bekijken en de stappen daar volt ooien.

* [Inleiding tot Azure log Integration](azure-log-integration-overview.md). In dit artikel wordt uitgelegd hoe u Azure Log Integration, de belangrijkste mogelijkheden en hoe het werkt.
* [Stappen](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/)voor de configuratie van de partner. In dit blog bericht ziet u hoe u Azure Log Integration kunt configureren voor gebruik met partner oplossingen Splunk, HP ArcSight en IBM QRadar. Onze huidige richt lijnen voor het configureren van de SIEM-onderdelen worden beschreven. Neem contact op met de leverancier van uw SIEM voor meer informatie.
* [Azure log Integration Veelgestelde vragen (FAQ)](azure-log-integration-faq.md). In deze veelgestelde vragen vindt u antwoorden op veelgestelde vragen over Azure Log Integration.
* [Azure Security Center-waarschuwingen integreren met Azure log Integration](/azure/security-center/security-center-integrating-alerts-with-log-integration). In dit artikel wordt beschreven hoe u Security Center waarschuwingen en beveiligings gebeurtenissen voor virtuele machines die worden verzameld door Azure Diagnostics en activiteiten logboeken van Azure kunt synchroniseren. U synchroniseert de logboeken met behulp van uw Azure Monitor-Logboeken of SIEM-oplossing.
* [Nieuwe functies voor Azure Diagnostics en Azure-controle logboeken](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/). In dit blog bericht wordt uitgelegd hoe u Azure audit logs en andere functies kunt gebruiken om inzicht te krijgen in de bewerkingen van uw Azure-resources.
