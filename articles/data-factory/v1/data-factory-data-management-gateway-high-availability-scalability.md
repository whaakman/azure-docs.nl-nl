---
title: Hoge beschikbaarheid met data management gateway in Azure Data Factory | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe u uit een data management gateway kunt schalen door het toevoegen van meer knooppunten en schaal omhoog door het aantal gelijktijdige taken die kunnen worden uitgevoerd op een knooppunt te verhogen.
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 195a1a4810de478b77538716fa8d1362428864d8
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="data-management-gateway---high-availability-and-scalability-preview"></a>Data Management Gateway - hoge beschikbaarheid en schaalbaarheid (Preview)
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [zelf gehost integratie runtime in versie 2](../create-self-hosted-integration-runtime.md). 


Dit artikel helpt u bij het configureren van oplossing voor hoge beschikbaarheid en schaalbaarheid met Data Management Gateway / integratie.    

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u al bekend met de basisprincipes van integratie Runtime (eerdere Data Management Gateway bent). Als u niet het geval is, Zie [Data Management Gateway](data-factory-data-management-gateway.md).

>**Deze preview-functie wordt officieel ondersteund op Data Management Gateway versie 2.12.xxxx.x en hoger**. Zorg ervoor dat u versie 2.12.xxxx.x of hoger. Download de nieuwste versie van Data Management Gateway [hier](https://www.microsoft.com/download/details.aspx?id=39717).

## <a name="overview"></a>Overzicht
U kunt gegevens management gateways die zijn geïnstalleerd op meerdere lokale computers met een enkele logische gateway vanuit de portal kunt koppelen. Deze machines heten **knooppunten**. U kunt maximaal hebben **vier knooppunten** die zijn gekoppeld aan een logische gateway. De voordelen van meerdere knooppunten (op lokale computers met de gateway is geïnstalleerd) voor een logische gateway zijn:  

- Prestaties van de verplaatsing van gegevens tussen on-premises en cloud gegevensarchieven.  
- Als een van de knooppunten uitgeschakeld voor een bepaalde reden wordt, nog andere knooppunten steeds beschikbaar voor het verplaatsen van de gegevens. 
- Als een van de knooppunten offline worden gehaald voor onderhoud moet, nog andere knooppunten steeds beschikbaar voor het verplaatsen van de gegevens.

U kunt ook configureren met het aantal **gelijktijdige data movement taken** die kunnen worden uitgevoerd op een knooppunt als de mogelijkheid om gegevens te verplaatsen tussen on-premises en cloud opschalen gegevensarchieven. 

De Azure-portal kunt u de status van deze knooppunten, waardoor u beslissen of u wilt toevoegen of verwijderen van een knooppunt van de logische gateway bewaken. 

## <a name="architecture"></a>Architectuur 
Het volgende diagram biedt een architectuuroverzicht van de van schaalbaarheid en beschikbaarheid van onderdeel van Data Management Gateway: 

![Data Management Gateway - hoge beschikbaarheid en schaalbaarheid](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-high-availability-and-scalability.png)

Een **logische gateway** is van de gateway die u aan een gegevensfactory in de Azure portal toevoegt. Eerder, kan u slechts één lokale Windows-machine met Data Management Gateway is geïnstalleerd met een logische gateway koppelen. Dit lokale gateway-apparaat wordt een knooppunt genoemd. Nu u maximaal kunt koppelen **vier fysieke knooppunten** met een logische gateway. Een logische gateway met meerdere knooppunten heet een **met meerdere knooppunten gateway**.  

Alle deze knooppunten zijn **active**. Alle data movement taken om gegevens te verplaatsen tussen on-premises en cloud kunnen verwerken gegevensarchieven. Een van de knooppunten fungeren als de dispatcher- en werkrollen. Andere knooppunten in de groepen zijn worker-knooppunten. Een **dispatcher** knooppunt data movement taken/taken van de cloudservice ophaalt en verzendt ze naar worker-knooppunten (inclusief zelf). Een **worker** knooppunt wordt uitgevoerd gegevens gegevensverplaatsing taken om gegevens te verplaatsen tussen on-premises en cloud gegevensarchieven. Alle knooppunten zijn werknemers. Slechts één knooppunt kan worden dispatch- en werkrollen.    

Kan u doorgaans begint met één knooppunt en **uitschalen** meer knooppunten, zoals de bestaande knooppunten worden overbelast met het laden van de verplaatsing van gegevens toevoegen. U kunt ook **opschalen** de mogelijkheid van de verplaatsing van gegevens van een gateway-knooppunt door het aantal gelijktijdige taken die mogen worden uitgevoerd op het knooppunt te verhogen. Deze mogelijkheid is ook beschikbaar met één knooppunt gateway (zelfs wanneer de functie voor schaalbaarheid en beschikbaarheid is niet ingeschakeld). 

Een gateway met meerdere knooppunten houdt u de referenties voor gegevensopslag synchroon op alle knooppunten. Als er een knooppunt naar verbindingsprobleem is, kunnen de referenties zijn niet gesynchroniseerd. Als u referenties voor een lokale gegevensarchief die gebruikmaakt van een gateway instelt, worden referenties opgeslagen op het knooppunt dispatcher/worker. De dispatcher-knooppunt wordt gesynchroniseerd met andere worker-knooppunten. Dit proces wordt ook wel **referenties sync**. Het communicatiekanaal tussen knooppunten kan worden **versleutelde** door een openbare SSL/TLS-certificaat. 

## <a name="set-up-a-multi-node-gateway"></a>Instellen van een gateway met meerdere knooppunten
Deze sectie wordt ervan uitgegaan dat u hebt doorlopen de volgende twee artikelen of bekend bent met concepten in deze artikelen: 

- [Data Management Gateway](data-factory-data-management-gateway.md) -bevat een gedetailleerd overzicht van de gateway.
- [Gegevens verplaatsen tussen on-premises en cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md) -bevat een overzicht met stapsgewijze instructies voor het gebruik van een gateway met één knooppunt.  

> [!NOTE]
> Raadpleeg voordat u een data management gateway op een lokale Windows-machine installeert, vereisten die worden vermeld [het belangrijkste artikel](data-factory-data-management-gateway.md#prerequisites).

1. In de [scenario](data-factory-move-data-between-onprem-and-cloud.md#create-gateway), tijdens het maken van een logische gateway inschakelen de **hoge beschikbaarheid en schaalbaarheid** functie. 

    ![Data Management Gateway - enable hoge beschikbaarheid en schaalbaarheid](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-enable-high-availability-scalability.png)
2. In de **configureren** pagina, gebruikt u een **snelle installatie** of **handmatige installatie** koppelen aan een gateway installeren op het eerste knooppunt (een lokale Windows-machine).

    ![Data Management Gateway - expliciete of handmatige installatie](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-express-manual-setup.png)

    > [!NOTE]
    > Als u de snelle installatie-optie gebruikt, wordt de communicatie knooppunt naar uitgevoerd zonder versleuteling. Naam van het knooppunt is hetzelfde als de naam van de machine. Handmatige installatie gebruiken als de communicatie van de knooppunten moet worden versleuteld of u wilt dat de knooppuntnaam van een van uw keuze op te geven. Knooppuntnamen kunnen niet later worden bewerkt.
3. Als u ervoor kiest **snelle installatie**
    1. U ziet het volgende bericht nadat de gateway is geïnstalleerd:

        ![Data Management Gateway - snelle installatie geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/express-setup-success.png)
    2. Data Management Configuration Manager voor de gateway starten door [deze instructies](data-factory-data-management-gateway.md#configuration-manager). Ziet u de gatewaynaam, knooppuntnaam, status, enzovoort.

        ![Data Management Gateway - installatie is geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)
4. Als u ervoor kiest **handmatige installatie**:
    1. Download het installatiepakket van het Microsoft Download Center, deze gateway installeren op uw computer uitvoeren.
    2. Gebruik de **verificatiesleutel** van de **configureren** pagina om de gateway te registreren.
    
        ![Data Management Gateway - installatie is geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-authentication-key.png)
    3. In de **nieuwe gateway-knooppunt** pagina kunt u een aangepaste bieden **naam** naar het gateway-knooppunt. Naam van het knooppunt is standaard hetzelfde als de naam van de machine.    

        ![Data Management Gateway - naam opgeven](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-name.png)
    4. In de volgende pagina kunt u kiezen of u wilt **Schakel versleuteling voor communicatie van knooppunt naar**. Klik op **overslaan** uitschakelen codering (standaard).

        ![Data Management Gateway - codering inschakelen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-node-encryption.png)  
    
        > [!NOTE]
        > Wijzigen van de versleuteling wordt alleen ondersteund als er een enkele gateway-knooppunt in de logische gateway. Te wilt wijzigen dat de modus versleuteling wanneer een gateway meerdere knooppunten heeft in de volgende stappen: verwijderen van alle knooppunten behalve één knooppunt, wijzig de modus voor versleuteling en voegt u de knooppunten opnieuw toe.
        > 
        > Zie [vereisten voor TLS/SSL-certificaten](#tlsssl-certificate-requirements) sectie voor een lijst met vereisten voor het gebruik van een TLS/SSL-certificaat. 
    5. Nadat de gateway is geïnstalleerd, klikt u op Start Configuration Manager:
    
        ![Handmatige installatie - launch configuration manager](media/data-factory-data-management-gateway-high-availability-scalability/manual-setup-launch-configuration-manager.png)   
    6. u ziet Data Management Gateway Configuration Manager op het knooppunt (lokale Windows-computer) waarin verbindingsstatus **gatewaynaam**, en **knooppuntnaam**.  

        ![Data Management Gateway - installatie is geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)

        > [!NOTE]
        > Als u de gateway op een Azure VM inricht, kunt u [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-mutiple-vms-with-data-management-gateway). Dit script maakt een logische gateway, stelt u de virtuele machines met Data Management Gateway-software is geïnstalleerd en registreert ze met de logische gateway. 
6. Start in de Azure-portal, de **Gateway** pagina: 
    1. Klik op de data factory startpagina in de portal op **gekoppelde Services**.
    
        ![Startpagina van de gegevensfactory](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-home-page.png)
    2. Selecteer de **gateway** om te zien de **Gateway** pagina:
    
        ![Startpagina van de gegevensfactory](media/data-factory-data-management-gateway-high-availability-scalability/linked-services-gateway.png)
    4. U ziet de **Gateway** pagina:   

        ![Gateway met één knooppunt weergeven](media/data-factory-data-management-gateway-high-availability-scalability/gateway-first-node-portal-view.png) 
7. Klik op **knooppunt toevoegen** op de werkbalk om een knooppunt toevoegen aan de logische gateway. Als u van plan bent de snelle installatie gebruiken, moet u deze stap doen vanuit de on-premises machine die worden toegevoegd als een knooppunt met de gateway. 

    ![Data Management Gateway - knooppunt menu toevoegen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)
8. Stappen zijn vergelijkbaar met het instellen van het eerste knooppunt. De Configuration Manager-UI kunt u de knooppuntnaam niet instellen als u de optie voor handmatige installatie: 

    ![Configuration Manager - tweede gateway installeren](media/data-factory-data-management-gateway-high-availability-scalability/install-second-gateway.png)
9. Nadat de gateway is geïnstalleerd op het knooppunt, wordt het volgende scherm weergegeven in het hulpprogramma Configuration Manager:  

    ![Configuration Manager - installatie tweede gateway geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/second-gateway-installation-successful.png)
10. Als u opent de **Gateway** pagina in de portal ziet u twee knooppunten van de gateway nu: 

    ![Gateway met twee knooppunten in de portal](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)
11. Als u wilt verwijderen een gateway-knooppunt, klikt u op **knooppunt verwijderen** op de werkbalk, selecteert u het knooppunt dat u wilt verwijderen en klik vervolgens op **verwijderen** van de werkbalk. Deze actie wordt het geselecteerde knooppunt verwijderd uit de groep. Houd er rekening mee dat deze actie niet de data management gateway-software uit het knooppunt (lokale Windows-machine verwijderen). Gebruik **software** in het Configuratiescherm op de on-premises verwijderen van de gateway. Wanneer u gateway vanuit het knooppunt verwijdert, wordt deze automatisch verwijderd in de portal.   

## <a name="upgrade-an-existing-gateway"></a>Upgrade van een bestaande gateway
U kunt een bestaande gateway voor het gebruik van de functie voor hoge beschikbaarheid en schaalbaarheid upgraden. Deze functie werkt alleen met knooppunten met de data management gateway met versie > = 2.12.xxxx. U kunt zien dat de versie van data management gateway is geïnstalleerd op een computer in de **Help** tabblad van de Data Management Gateway Configuration Manager. 

1. Bijwerken van de gateway op de on-premises machine naar de nieuwste versie door de volgende door te downloaden en uitvoeren van een MSI setup-pakket van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Zie [installatie](data-factory-data-management-gateway.md#installation) sectie voor meer informatie.  
2. Navigeer naar de Azure-portal. Start de **Data Factory pagina** van uw gegevensfactory. Klik op de tegel van de gekoppelde services starten de **gekoppelde servicepagina**. Selecteer de gateway starten de **pagina gateway**. Klik op en schakel **Preview-functie** zoals weergegeven in de volgende afbeelding: 

    ![Data Management Gateway - preview-functie inschakelen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-existing-gateway-enable-high-availability.png)   
2. Zodra de preview-functie is ingeschakeld in de portal, sluit u alle pagina's. Open de **pagina gateway** om te zien van de nieuwe preview-gebruikersinterface (UI).
 
    ![Data Management Gateway - enable preview-functie geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview-success.png)

    ![Data Management Gateway - preview-gebruikersinterface](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview.png)

    > [!NOTE]
    > Naam van het eerste knooppunt is tijdens de upgrade is de naam van de machine. 
3. Voeg nu een knooppunt. In de **Gateway** pagina, klikt u op **knooppunt toevoegen**.  

    ![Data Management Gateway - knooppunt menu toevoegen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)

    Volg de instructies uit de vorige sectie voor het instellen van het knooppunt. 

### <a name="installation-best-practices"></a>Best practices voor installatie

- Energiebeheerschema op de hostcomputer voor de gateway zodanig configureren dat de machine sluimerstand niet. Als de hostmachine in de slaapstand, reageert de gateway niet op gegevensaanvragen.
- Back-up van het certificaat dat hoort bij de gateway.
- Zorg ervoor dat alle knooppunten van vergelijkbare configuratie (aanbevolen) voor ideaal prestaties zijn. 
- Voeg ten minste twee knooppunten voor hoge beschikbaarheid garanderen.  

### <a name="tlsssl-certificate-requirements"></a>Vereisten voor TLS/SSL-certificaten
Hier volgen de vereisten voor het TLS/SSL-certificaat dat wordt gebruikt voor het beveiligen van communicatie tussen integratie runtime knooppunten:

- Het certificaat moet een openbaar vertrouwde X509 v3-certificaat. Het is raadzaam om gebruik te maken van certificaten die zijn uitgegeven door een openbare (derde) certificeringsinstantie (CA).
- Elk knooppunt van de runtime integratie moet vertrouwen voor dit certificaat, evenals de client-computer waarop de toepassing van de referentie-manager wordt uitgevoerd. 
> [!NOTE]
> Referentie manager-toepassing wordt gebruikt tijdens het instellen van referenties veilig van de Wizard kopiëren / Azure-Portal. En dit kan zijn gestarte vanaf een willekeurige computer binnen hetzelfde netwerk bevindt als de on-premises / persoonlijke gegevensopslag.
- Met wild card certificaten worden ondersteund. Als uw FQDN-naam **node1.domain.contoso.com**, kunt u ***. domain.contoso.com** als de onderwerpnaam van het certificaat.
- SAN-certificaten worden niet aanbevolen omdat alleen het laatste item in de alternatieve onderwerpnamen wordt gebruikt en alle andere worden genegeerd vanwege een beperking van de huidige. Bijvoorbeeld u hebt een SAN-certificaat waarvan SAN zijn **node1.domain.contoso.com** en **node2.domain.contoso.com**, kunt u dit certificaat alleen gebruiken op de machine waarvan FQDN **node2.domain.contoso.com**.
- Ondersteunt sleutelgrootte voor SSL-certificaten wordt ondersteund door Windows Server 2012 R2.
- Certificaten met CNG sleutels worden niet ondersteund. Doesrted DoesDoes ondersteuning geen voor certificaten met CNG-sleutels.

#### <a name="faq-when-would-i-not-enable-this-encryption"></a>Veelgestelde vragen over: Wanneer ik niet mogelijk deze versleuteling?
Codering inschakelen kunt toevoegen bepaalde kosten om uw infrastructuur (eigenaar openbaar certificaat) daarom kunt u overslaan-versleuteling inschakelen in de onderstaande gevallen:
- Wanneer de runtime integratie wordt uitgevoerd op een vertrouwd netwerk of een netwerk met transparante versleuteling zoals IP/SEC. Omdat deze communicatie tussen alleen beperkt in uw vertrouwde netwerk, moet u mogelijk geen extra codering.
- Wanneer de runtime integratie wordt niet uitgevoerd in een productieomgeving. Zo kunt u TLS/SSL-certificaat kosten verminderen.


## <a name="monitor-a-multi-node-gateway"></a>Monitor voor een gateway met meerdere knooppunten
### <a name="multi-node-gateway-monitoring"></a>Bewaking van de gateway met meerdere knooppunten
U kunt vrijwel in realtime momentopname van Resourcegebruik (CPU, geheugen, network(in/out), enz.) op elk knooppunt samen met de status van gateway-knooppunten weergeven in de Azure portal. 

![Data Management Gateway - bewaking van meerdere knooppunten](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)

U kunt inschakelen **geavanceerde instellingen** in de **Gateway** pagina voor geavanceerde metrische gegevens zoals **netwerk**(in/uit), **rol & Status van de referentie**, die u kan helpen bij foutopsporing gateway-problemen en **gelijktijdige taken** (actief / beperken) die kan worden gewijzigd / gewijzigde dienovereenkomstig tijdens prestaties afstemmen. De volgende tabel bevat beschrijvingen van kolommen in de **Gateway-knooppunten** lijst:  

Bewaking van eigenschap | Beschrijving
:------------------ | :---------- 
Naam | Naam van de logische gateway en de knooppunten die zijn gekoppeld aan de gateway.  
Status | Status van de logische gateway en de gateway-knooppunten. Voorbeeld: Online/Offline/Limited/enz. Zie voor meer informatie over deze statussen [gatewaystatus](#gateway-status) sectie. 
Versie | Toont de versie van de logische gateway en elk gateway-knooppunt. De versie van de logische gateway wordt bepaald op basis van de versie van het merendeel van de knooppunten in de groep. Als er knooppunten met verschillende versies in de logische gateway setup alleen de knooppunten met hetzelfde versienummer als de functie logische gateway correct zijn. Anderen in de beperkte modus zijn en moeten handmatig worden bijgewerkt (alleen als automatische updates is mislukt). 
Beschikbaar geheugen | Beschikbaar geheugen op een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime. 
CPU-gebruik | CPU-gebruik van een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime. 
Networking (In/Out) | Netwerkgebruik van een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime. 
Gelijktijdige taken (actief / beperken) | Het aantal taken of taken die op elk knooppunt worden uitgevoerd. Deze waarde is een momentopname van een bijna realtime. Limiet geeft aan dat het maximum aantal gelijktijdige taken voor elk knooppunt. Deze waarde is gedefinieerd op basis van de grootte van de machine. U kunt de limiet voor gelijktijdige taakuitvoering in geavanceerde scenario's, opschalen verhogen waar CPU / geheugen / netwerk is onder gebruikt, maar de activiteiten worden een time-out opgetreden. Deze mogelijkheid is ook beschikbaar met één knooppunt gateway (zelfs wanneer de functie voor schaalbaarheid en beschikbaarheid is niet ingeschakeld). Zie voor meer informatie [schalen overwegingen](#scale-considerations) sectie. 
Rol | Er zijn twee typen rollen – Dispatcher- en werkrollen. Alle knooppunten zijn werknemers, wat betekent dat ze kunnen alle worden gebruikt om uit te voeren taken. Er is slechts één dispatcher knooppunt, die wordt gebruikt voor het pull-taken/taken van cloudservices en ze verzenden naar andere worker-knooppunten (inclusief zelf). 

![Data Management Gateway - geavanceerde bewaking van meerdere knooppunten](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-advanced.png)

### <a name="gateway-status"></a>Status van gateway

De volgende tabel bevat de mogelijke statussen van een **gateway-knooppunt**: 

Status  | Opmerkingen/scenario 's
:------- | :------------------
Online | Knooppunt verbonden met Data Factory-service.
Offline | Knooppunt is offline.
Upgraden | Het knooppunt wordt automatisch bijgewerkt.
Beperkt | Vanwege een verbindingsprobleem. Kan worden veroorzaakt door HTTP-poort 8050 probleem, service bus-verbindingsprobleem of synchronisatieprobleem in de referentie. 
Inactief | Er is een knooppunt in een configuratie van de configuratie van andere knooppunten van de meeste andere.<br/><br/> Een knooppunt mag inactief zijn als er geen verbinding met andere knooppunten maken. 


De volgende tabel bevat de mogelijke statussen van een **logische gateway**. De status van de gateway is afhankelijk van de status van de gateway-knooppunten. 

Status | Opmerkingen
:----- | :-------
Moet worden geregistreerd | Er is geen knooppunt is nog aan deze logische gateway geregistreerd
Online | Gateway-knooppunten zijn online.
Offline | Er is geen knooppunt online status.
Beperkt | Niet alle knooppunten in deze gateway zijn in orde. Deze status is een waarschuwing dat een bepaald knooppunt niet beschikbaar. <br/><br/>Kan worden veroorzaakt door referentie synchronisatieprobleem op dispatcher/werkrolknooppunt. 

### <a name="pipeline-activities-monitoring"></a>Pijplijn / activiteiten bewaken
De Azure portal biedt een pijplijn implementatiebewakingservaring met gedetailleerde knooppunt mate van details. Bijvoorbeeld: er wordt weergegeven welke activiteiten uitgevoerd op welk knooppunt. Deze informatie kan nuttig zijn bij het begrijpen van prestatieproblemen op een bepaald knooppunt, spreken vanwege netwerkbeperking. 

![Data Management Gateway - bewaking voor pijplijnen van meerdere knooppunten](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-pipelines.png)

![Data Management Gateway - pipeline-details](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-pipeline-details.png)

## <a name="scale-considerations"></a>Overwegingen met betrekking tot schaal

### <a name="scale-out"></a>Uitschalen
Wanneer de **beschikbaar geheugen laag is** en de **CPU-gebruik is hoog**, een nieuw knooppunt toevoegen helpt u scale-out van de belasting op computers. Als activiteiten vanwege time-out- of gateway-knooppunt offline mislukken, is het handig als u een knooppunt aan de gateway toevoegt.
 
### <a name="scale-up"></a>Omhoog schalen
Wanneer het beschikbare geheugen en de CPU niet goed worden benut, maar de niet-actieve capaciteit 0 is, moet u opschalen door het aantal gelijktijdige taken die kunnen worden uitgevoerd op een knooppunt te verhogen. U kunt ook opschalen activiteiten zijn een time-out opgetreden omdat de gateway is overbelast. Zoals u in de volgende afbeelding, verhoogt u de maximale capaciteit van een knooppunt. Het is raadzaam om eerst verdubbelen.  

![Data Management Gateway - scale-overwegingen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-scale-considerations.png)


## <a name="known-issuesbreaking-changes"></a>Bekende problemen/recente wijzigingen

- U kunt op dit moment kunnen maximaal vier fysieke gateway knooppunten voor één logische gateway hebben. Als u meer dan vier knooppunten nodig voor betere prestaties, een e-mail sturen naar [ DMGHelp@microsoft.com ](mailto:DMGHelp@microsoft.com).
- Een gateway-knooppunt kan niet opnieuw registreren met de verificatiesleutel van een andere logische gateway overschakelen van de huidige logische gateway. Verwijder de gateway van het knooppunt, installeer de gateway opnieuw als opnieuw wilt registreren, en bij de verificatiesleutel voor de logische gateway registreren. 
- Als HTTP-proxy vereist voor alle gateway-knooppunten is, instellen van de proxy in diahost.exe.config en diawp.exe.config en Serverbeheer gebruiken om ervoor te zorgen dat alle knooppunten dezelfde diahost.exe.config en diawip.exe.config hebben. Zie [proxy-instellingen configureren](data-factory-data-management-gateway.md#configure-proxy-server-settings) sectie voor meer informatie. 
- Als u wilt wijzigen versleuteling modus voor knooppunt naar communicatie in Gateway Configuration Manager, alle knooppunten in de portal, behalve een te verwijderen. Vervolgens voegt u knooppunten terug na het wijzigen van de modus voor versleuteling.
- Gebruik van een officiële SSL-certificaat als u kiest voor het versleutelen van het knooppunt naar communicatiekanaal. Zelf-ondertekend certificaat mogelijk problemen met de netwerkverbinding zoals hetzelfde certificaat kan niet worden vertrouwd in certificeren lijst met op andere computers. 
- U kunt een gateway-knooppunt naar een logische gateway niet registreren wanneer de knooppunt-versie lager dan de versie van de logische gateway is. Alle knooppunten van de logische gateway verwijderen uit portal, zodat u kunt een lagere versie node(downgrade) registreren deze. Als u alle knooppunten van een logische gateway verwijdert, handmatig installeren en registreren van nieuwe knooppunten op dat logische gateway. Snelle installatie wordt in dit geval niet ondersteund.
- U niet de snelle installatie gebruiken voor het installeren van knooppunten aan een bestaande logische gateway cloud referenties nog wordt gebruikt. U kunt controleren waar de referenties van de Gateway Configuration Manager op het tabblad instellingen worden opgeslagen.
- U niet de snelle installatie gebruiken voor het installeren van de knooppunten naar een bestaande logische gateway met knooppunt naar versleuteling ingeschakeld. Als de instelling van de modus voor versleuteling omvat het handmatig toevoegen van certificaten, is snelle installatie optie niet meer een. 
- Voor het kopiëren van een bestand vanaf on-premises omgeving, moet niet u \\localhost of C:\files meer sinds localhost of een lokaal station mogelijk niet toegankelijk is via alle knooppunten. Gebruik in plaats daarvan \\ServerName\files bestanden locatie op te geven.


## <a name="rolling-back-from-the-preview"></a>Terugdraaien van de Preview-versie 
Als u wilt terugdraaien vanuit de evaluatieversie van alle knooppunten maar één knooppunt te verwijderen. Het maakt niet uit welke knooppunten verwijderen, maar zorg ervoor dat er ten minste één knooppunt in de logische gateway. U kunt een knooppunt verwijderen door de gateway op de computer verwijderen of door met de Azure portal. In de Azure-portal in de **Data Factory** gekoppelde services te starten klikt u op de **gekoppelde services** pagina. Selecteer de gateway starten de **Gateway** pagina. Op de pagina Gateway ziet u de knooppunten die zijn gekoppeld aan de gateway. De pagina kunt u een knooppunt verwijderen van de gateway.
 
Na het verwijderen, klikt u op **preview-functies** in de dezelfde Azure portal-pagina, en schakel de preview-functie. U hebt uw gateway uit naar één knooppunt GA (algemene beschikbaarheid) gateway opnieuw instellen.


## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen:
- [Data Management Gateway](data-factory-data-management-gateway.md) -bevat een gedetailleerd overzicht van de gateway.
- [Gegevens verplaatsen tussen on-premises en cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md) -bevat een overzicht met stapsgewijze instructies voor het gebruik van een gateway met één knooppunt. 
