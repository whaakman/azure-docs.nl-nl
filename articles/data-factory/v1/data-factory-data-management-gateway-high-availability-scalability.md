---
title: Hoge beschikbaarheid met data management gateway in Azure Data Factory | Microsoft Docs
description: Dit artikel wordt uitgelegd hoe kunt u een data management gateway uitschalen door toevoeging van meer knooppunten en schaal omhoog door het aantal gelijktijdige taken die kunnen worden uitgevoerd op een knooppunt.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: bc8cacd6d52de0367a0ea14748e548b9d32f47ef
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016764"
---
# <a name="data-management-gateway---high-availability-and-scalability-preview"></a>Data Management Gateway - hoge beschikbaarheid en schaalbaarheid (Preview)
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [zelf-hostende integratieruntime in](../create-self-hosted-integration-runtime.md). 


Dit artikel helpt u bij het configureren van oplossing met hoge beschikbaarheid en schaalbaarheid met Data Management Gateway / integratie.    

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u al bekend met de basisprincipes van Integration Runtime (eerdere Data Management Gateway bent). Als u niet het geval is, Zie [Data Management Gateway](data-factory-data-management-gateway.md).

>**Deze preview-functie is officieel ondersteund op Data Management Gateway versie 2.12.xxxx.x en hoger**. Zorg ervoor dat u versie 2.12.xxxx.x of hoger. Download de nieuwste versie van Data Management Gateway [hier](https://www.microsoft.com/download/details.aspx?id=39717).

## <a name="overview"></a>Overzicht
U kunt data management gateways die zijn geïnstalleerd op meerdere on-premises machines met een enkele logische gateway vanuit de portal koppelen. Deze machines heten **knooppunten**. U kunt maximaal hebben **vier knooppunten** die zijn gekoppeld aan een logische gateway. De voordelen van meerdere knooppunten (on-premises machines met de gateway is geïnstalleerd) voor een logische gateway zijn:  

- Prestaties van de verplaatsing van gegevens tussen on-premises en cloud gegevensarchieven.  
- Als een van de knooppunten uitgeschakeld voor een of andere reden wordt, zijn andere knooppunten nog steeds beschikbaar voor het verplaatsen van de gegevens. 
- Als een van de knooppunten offline worden gehaald voor onderhoud moet, zijn andere knooppunten nog steeds beschikbaar voor het verplaatsen van de gegevens.

U kunt ook het aantal **gelijktijdige verkeer taken** die kunnen worden uitgevoerd op een knooppunt de mogelijkheid om gegevens te verplaatsen tussen on-premises en cloud kan worden uitgebreid gegevensarchieven. 

Met behulp van de Azure-portal, kunt u de status van deze knooppunten die u u bij het beslissen of u helpt wilt toevoegen of verwijderen van een knooppunt uit het logische gateway bewaken. 

## <a name="architecture"></a>Architectuur 
Het volgende diagram biedt een overzicht van de architectuur van schaalbaarheid en beschikbaarheid-functie van de Data Management Gateway: 

![Data Management Gateway - hoge beschikbaarheid en schaalbaarheid](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-high-availability-and-scalability.png)

Een **logische gateway** is de gateway die u aan een data factory in Azure portal toevoegt. Eerder kon u slechts één on-premises Windows-computer koppelen met Data Management Gateway is geïnstalleerd met een logische gateway. Deze on-premises machine van de gateway heet een knooppunt. Nu u maximaal kunt koppelen **vier fysieke knooppunten** met een logische gateway. Een logische gateway met meerdere knooppunten heet een **meerdere knooppunten gateway**.  

Alle deze knooppunten zijn **active**. Ze allemaal kunnen worden verwerkt gegevens verkeer taken voor het verplaatsen van gegevens tussen on-premises en cloud gegevensarchieven. Een van de knooppunten fungeren als de functie voor berichtverzending- en werkrollen. Andere knooppunten in de groepen zijn worker-knooppunten. Een **dispatcher** knooppunt haalt gegevens verkeer taken/taken van de cloudservice en verzendt ze naar de worker-knooppunten (inclusief zelf). Een **worker** knooppunt wordt uitgevoerd gegevens verkeer taken voor het verplaatsen van gegevens tussen on-premises en cloud gegevensarchieven. Alle knooppunten zijn werknemers. Slechts één knooppunt kan worden verzending- en werkrollen.    

U kan doorgaans begint met één knooppunt en **uitschalen** meer knooppunten als de bestaande knooppunten worden overspoeld met het laden van de verplaatsing van gegevens toevoegen. U kunt ook **omhoog schalen** de mogelijkheid voor het verplaatsen van gegevens van een gateway-knooppunt door het aantal gelijktijdige taken die mogen worden uitgevoerd op het knooppunt. Deze mogelijkheid is ook beschikbaar met één knooppunt gateway (zelfs als de functie voor schaalbaarheid en beschikbaarheid is niet ingeschakeld). 

Een gateway met meerdere knooppunten houdt u de referenties van de gegevensopslag gesynchroniseerd voor alle knooppunten. Als er een probleem met de netwerkverbinding van knooppunt-naar-knooppunt, kunnen de referenties zijn niet gesynchroniseerd. Wanneer u referenties voor een on-premises-gegevensarchief dat gebruikmaakt van een gateway hebt ingesteld, worden er opgeslagen referenties op het knooppunt verzender/werknemer. De functie voor berichtverzending knooppunt wordt gesynchroniseerd met andere worker-knooppunten. Dit proces staat bekend als **referenties synchronisatie**. Het communicatiekanaal tussen knooppunten kan worden **versleutelde** door een openbare SSL/TLS-certificaat. 

## <a name="set-up-a-multi-node-gateway"></a>Instellen van een gateway met meerdere knooppunten
In deze sectie wordt ervan uitgegaan dat u hebt doorlopen in de volgende twee artikelen of bekend bent met concepten in deze artikelen: 

- [Data Management Gateway](data-factory-data-management-gateway.md) -biedt een gedetailleerd overzicht van de gateway.
- [Gegevens verplaatsen tussen on-premises en cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md) -bevat een overzicht met stapsgewijze instructies voor het gebruik van een gateway met een enkel knooppunt.  

> [!NOTE]
> Voordat u een data management gateway op een on-premises Windows-computer installeert, Zie vereisten die worden vermeld [het belangrijkste artikel](data-factory-data-management-gateway.md#prerequisites).

1. In de [scenario](data-factory-move-data-between-onprem-and-cloud.md#create-gateway), inschakelen tijdens het maken van een logische gateway, de **hoge beschikbaarheid en schaalbaarheid** functie. 

    ![Data Management Gateway - inschakelen hoge beschikbaarheid en schaalbaarheid](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-enable-high-availability-scalability.png)
2. In de **configureren** pagina, gebruikt u een **snelle installatie van** of **handmatige installatie** koppelen aan een gateway installeren op het eerste knooppunt (een on-premises Windows-computer).

    ![Data Management Gateway - uitdrukkelijk of handmatige installatie](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-express-manual-setup.png)

    > [!NOTE]
    > Als u de snelle installatie-optie gebruikt, wordt de communicatie knooppunt-naar-knooppunt uitgevoerd zonder versleuteling. Naam van het knooppunt is hetzelfde als de naam van de machine. Gebruik de handmatige installatie als de communicatie tussen knooppunten moet worden versleuteld of u wilt de knooppuntnaam van een van uw keuze op te geven. Knooppuntnamen kunnen niet later worden bewerkt.
3. Als u ervoor kiest **snelle installatie**
    1. U ziet het volgende bericht nadat de gateway is geïnstalleerd:

        ![Data Management Gateway - snelle installatie geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/express-setup-success.png)
    2. Data Management Configuration Manager starten voor de gateway door [deze instructies](data-factory-data-management-gateway.md#configuration-manager). U ziet de gatewaynaam van de, naam van het knooppunt, status, enzovoort.

        ![Data Management Gateway - installatie is geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)
4. Als u ervoor kiest **handmatige installatie**:
    1. Download het installatiepakket van het Microsoft Download Center, worden gateway installeren op uw computer worden uitgevoerd.
    2. Gebruik de **verificatiesleutel** uit de **configureren** pagina om de gateway te registreren.
    
        ![Data Management Gateway - installatie is geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-authentication-key.png)
    3. In de **nieuwe gateway-knooppunt** pagina, kunt u een aangepaste bieden **naam** naar de gateway-knooppunt. Naam van het knooppunt is standaard hetzelfde als de naam van de machine.    

        ![Data Management Gateway - naam opgeven](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-name.png)
    4. In de volgende pagina kunt u kiezen of u wilt **versleuteling inschakelen voor communicatie van knooppunt-naar-knooppunt**. Klik op **overslaan** om uit te schakelen codering (standaard).

        ![Data Management Gateway - versleuteling inschakelen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-node-encryption.png)  
    
        > [!NOTE]
        > Wijzigen van de versleutelingsmodus wordt alleen ondersteund wanneer u een knooppunt één gateway in de logische gateway. Als u wilt de versleutelingsmodus wijzigen wanneer een gateway meerdere knooppunten heeft, de volgende stappen uitvoeren: het verwijderen van alle knooppunten, met uitzondering van één knooppunt, de versleutelingsmodus wijzigen en vervolgens de knooppunten opnieuw toevoegen.
        > 
        > Zie [TLS/SSL-certificaatvereisten](#tlsssl-certificate-requirements) sectie voor een lijst van de vereisten voor het gebruik van een TLS/SSL-certificaat. 
    5. Nadat de gateway is geïnstalleerd, klikt u op Configuration Manager starten:
    
        ![Handmatige installatie - launch configuration manager](media/data-factory-data-management-gateway-high-availability-scalability/manual-setup-launch-configuration-manager.png)   
    6. ziet u de Data Management Gateway Configuration Manager op het knooppunt (on-premises Windows computer), waarin connectiviteitsstatus **gatewaynaam**, en **knooppuntnaam**.  

        ![Data Management Gateway - installatie is geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)

        > [!NOTE]
        > Als u de gateway op een Azure VM inricht, kunt u [deze Azure Resource Manager-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-mutiple-vms-with-data-management-gateway). Met dit script maakt een logische gateway, stelt u de virtuele machines met Data Management Gateway-software is geïnstalleerd en registreert deze met de logische gateway. 
6. Start in Azure portal, de **Gateway** pagina: 
    1. Klik op de pagina data factory start in de portal, **gekoppelde Services**.
    
        ![Startpagina van de gegevensfactory](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-home-page.png)
    2. Selecteer de **gateway** om te zien de **Gateway** pagina:
    
        ![Startpagina van de gegevensfactory](media/data-factory-data-management-gateway-high-availability-scalability/linked-services-gateway.png)
    4. U ziet de **Gateway** pagina:   

        ![Gateway met één knooppunt weergeven](media/data-factory-data-management-gateway-high-availability-scalability/gateway-first-node-portal-view.png) 
7. Klik op **knooppunt toevoegen** op de werkbalk om een knooppunt toevoegen aan de logische gateway. Als u van plan bent te gebruiken van snelle installatie, voert u deze stap van de on-premises computer die als een knooppunt wordt toegevoegd aan de gateway. 

    ![Data Management Gateway - knooppuntmenu toevoegen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)
8. Stappen zijn vergelijkbaar met het instellen van het eerste knooppunt. De Configuration Manager-UI kunt u de naam van het knooppunt wordt ingesteld als u de handmatige installatie-optie kiezen: 

    ![Configuration Manager - tweede gateway installeren](media/data-factory-data-management-gateway-high-availability-scalability/install-second-gateway.png)
9. Nadat de gateway is geïnstalleerd op het knooppunt, wordt het volgende scherm weergegeven in het hulpprogramma Configuration Manager:  

    ![Configuration Manager - installatie tweede gateway geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/second-gateway-installation-successful.png)
10. Als u opent de **Gateway** pagina in de portal ziet u twee gatewayknooppunten nu: 

    ![Gateway met twee knooppunten in de portal](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)
11. Als u wilt verwijderen van een gateway-knooppunt, klikt u op **knooppunt verwijderen** op de werkbalk en selecteer het knooppunt dat u wilt verwijderen, en klik vervolgens op **verwijderen** via de werkbalk. Deze actie wordt het geselecteerde knooppunt van de groep verwijderd. Houd er rekening mee dat deze actie niet de data management gateway-software wordt verwijderd uit het knooppunt (on-premises Windows-computer). Gebruik **programma's toevoegen of verwijderen** in het Configuratiescherm op de on-premises naar de gateway verwijderen. Wanneer u de gateway van het knooppunt verwijdert, wordt deze automatisch verwijderd in de portal.   

## <a name="upgrade-an-existing-gateway"></a>Upgrade van een bestaande gateway
U kunt een upgrade uitvoeren van een bestaande gateway voor het gebruik van de functie voor hoge beschikbaarheid en schaalbaarheid. Deze functie werkt alleen met knooppunten met de data management gateway van versie > = 2.12.xxxx. U kunt zien dat de versie van data management gateway is geïnstalleerd op een computer in de **Help** tabblad van Data Management Gateway Configuratiebeheer. 

1. De gateway op de on-premises machine naar de nieuwste versie bijwerken door de volgende door te downloaden en uitvoeren van een MSI-bestand setup-pakket uit de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). Zie [installatie](data-factory-data-management-gateway.md#installation) sectie voor meer informatie.  
2. Navigeer naar de Azure-portal. Start de **Data Factory-pagina** van uw gegevensfactory. Klik op de tegel van de gekoppelde services om te starten de **gekoppelde servicepagina**. Selecteer de gateway om te starten de **pagina gateway**. Klik op en schakel **Preview-functie** zoals wordt weergegeven in de volgende afbeelding: 

    ![Data Management Gateway - preview-functie inschakelen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-existing-gateway-enable-high-availability.png)   
2. Zodra de preview-functie is ingeschakeld in de portal, sluit u alle pagina's. Open de **pagina gateway** om te zien van de nieuwe preview-gebruikersinterface (UI).
 
    ![Data Management Gateway - inschakelen preview-functie geslaagd](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview-success.png)

    ![Data Management Gateway - voorbeeld-UI](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview.png)

    > [!NOTE]
    > Naam van het eerste knooppunt is tijdens de upgrade is de naam van de machine. 
3. Voeg nu een knooppunt toe. In de **Gateway** pagina, klikt u op **knooppunt toevoegen**.  

    ![Data Management Gateway - knooppuntmenu toevoegen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)

    Volg de instructies uit de vorige sectie voor het instellen van het knooppunt. 

### <a name="installation-best-practices"></a>Best practices voor installatie

- Energiebeheerschema op de hostcomputer voor de gateway zodanig configureren dat de machine sluimerstand niet. Als de hostmachine in de slaapstand, wordt de gateway niet reageert op gegevensaanvragen.
- Back-up van het certificaat dat is gekoppeld aan de gateway.
- Zorg ervoor dat alle knooppunten zijn vergelijkbare configuratie voor de ideale prestaties (aanbevolen). 
- Voeg ten minste twee knooppunten voor hoge beschikbaarheid.  

### <a name="tlsssl-certificate-requirements"></a>Vereisten voor TLS/SSL-certificaat
Hier volgen de vereisten voor het TLS/SSL-certificaat dat wordt gebruikt voor het beveiligen van communicatie tussen de integration runtime-knooppunten:

- Het certificaat moet een openbaar vertrouwde X509 v3-certificaat. U wordt aangeraden dat u gebruik van certificaten die zijn uitgegeven door een openbare (derde) certificeringsinstantie (CA).
- Elk knooppunt voor integration runtime moet dit certificaat, evenals de client-computer waarop de toepassing Referentiebeheer vertrouwen. 
> [!NOTE]
> Toepassing Referentiebeheer wordt gebruikt bij het instellen van beveiligde referentie van de Wizard kopiëren / Azure Portal. En dit kan zijn van een computer in hetzelfde netwerk bevinden als de on-premises geactiveerde / persoonlijke gegevensopslag.
- Jokertekens certificaten worden ondersteund. Als uw FQDN-naam is **node1.domain.contoso.com**, kunt u ***. domain.contoso.com** als onderwerpnaam van het certificaat.
- SAN-certificaten worden niet aanbevolen, omdat alleen het laatste item van de alternatieve namen voor onderwerpen worden gebruikt en alle andere worden genegeerd vanwege een beperking. Bijvoorbeeld u hebt een SAN-certificaat waarvan SAN zijn **node1.domain.contoso.com** en **node2.domain.contoso.com**, kunt u dit certificaat alleen gebruiken op de machine waarvan FQDN **node2.domain.contoso.com**.
- Ondersteunt sleutelgrootte ondersteund door Windows Server 2012 R2 voor SSL-certificaten.
- Het certificaat met CNG sleutels worden niet ondersteund.

#### <a name="faq-when-would-i-not-enable-this-encryption"></a>FAQ: Wanneer kan ik deze versleuteling niet inschakelen?
Versleuteling is ingeschakeld kunt toevoegen bepaalde daarom kosten verbonden aan de infrastructuur (die eigenaar is openbaar certificaat) kunt u overslaan inschakelen van versleuteling in de volgende gevallen:
- Als de integratieruntime wordt uitgevoerd in een vertrouwd netwerk of een netwerk met transparante versleuteling, zoals IP per seconde Omdat deze communicatie kanaal alleen is beperkt in uw vertrouwde netwerk, moet u mogelijk aanvullende versleuteling.
- Wanneer de integratieruntime wordt niet uitgevoerd in een productieomgeving. Zo kunt u TLS/SSL-certificaat kosten verminderen.


## <a name="monitor-a-multi-node-gateway"></a>Een gateway met meerdere knooppunten controleren
### <a name="multi-node-gateway-monitoring"></a>Bewaking van meerdere knooppunten gateway
U kunt de near-real-time-momentopname van Resourcegebruik (CPU, geheugen, network(in/out), enzovoort) op elk knooppunt, samen met de status van de gateway-knooppunten weergeven in de Azure-portal. 

![Data Management Gateway - bewaking van meerdere knooppunten](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)

U kunt inschakelen **geavanceerde instellingen** in de **Gateway** pagina voor geavanceerde metrische gegevens, zoals **netwerk**(in/uit), **rollen en referentiestatus**, dit is handig bij het opsporen van gatewayproblemen, en **gelijktijdige taken** (actief / beperken) die kan worden gewijzigd / gewijzigde dienovereenkomstig tijdens het afstemmen van prestaties. De volgende tabel bevat beschrijvingen van de kolommen in de **Gatewayknooppunten** lijst:  

Bewaking van eigenschap | Description
:------------------ | :---------- 
Name | De naam van de logische-gateway en de knooppunten die zijn gekoppeld aan de gateway.  
Status | De status van de logische-gateway en de gateway-knooppunten. Voorbeeld: Online/Offline/Limited/enz. Zie voor meer informatie over deze statussen [gatewaystatus](#gateway-status) sectie. 
Versie | Toont de versie van de logische gateway en de gateway-knooppunt. De versie van de logische gateway wordt bepaald op basis van de versie van het merendeel van de knooppunten in de groep. Als er knooppunten met verschillende versies in de logische gateway-instellingen, alleen de knooppunten met het versienummer hetzelfde als de functie logische gateway correct zijn. Anderen in de beperkte modus zijn en moeten handmatig worden bijgewerkt (alleen als automatisch bijwerken is mislukt). 
Beschikbaar geheugen | Beschikbaar geheugen op een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime. 
CPU-gebruik | CPU-gebruik van een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime. 
Netwerken (In/uit) | Het netwerkgebruik van een gateway-knooppunt. Deze waarde is een momentopname van een bijna realtime. 
Gelijktijdige taken (actief / beperken) | Het aantal taken of taken die worden uitgevoerd op elk knooppunt. Deze waarde is een momentopname van een bijna realtime. Limiet geeft aan dat het maximum aantal gelijktijdige taken voor elk knooppunt. Deze waarde is gedefinieerd op basis van de machinegrootte. U kunt de limiet voor gelijktijdige taakuitvoering in geavanceerde scenario's kan worden uitgebreid verhogen waar CPU / geheugen / netwerk wordt benut, maar de activiteiten zijn een time-out opgetreden. Deze mogelijkheid is ook beschikbaar met één knooppunt gateway (zelfs als de functie voor schaalbaarheid en beschikbaarheid is niet ingeschakeld). Zie voor meer informatie, [overwegingen met betrekking tot schalen](#scale-considerations) sectie. 
Rol | Er zijn twee soorten rollen – Dispatcher- en werkrollen. Alle knooppunten zijn werknemers, wat betekent dat ze kunnen allemaal worden gebruikt voor het uitvoeren van taken. Er is slechts één functie voor berichtverzending knooppunt, die wordt gebruikt voor het ophalen van taken/taken uit cloudservices en ze verzenden naar verschillende worker-knooppunten (inclusief zelf). 

![Data Management Gateway - geavanceerde controle van meerdere knooppunten](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-advanced.png)

### <a name="gateway-status"></a>Gatewaystatus

De volgende tabel bevat de mogelijke statussen van een **gateway-knooppunt**: 

Status  | Opmerkingen /-scenario 's
:------- | :------------------
Online | Knooppunt is verbonden met Data Factory-service.
Offline | Knooppunt is offline.
Upgraden | Het knooppunt wordt automatisch bijgewerkt.
Beperkt | Vanwege een probleem met de netwerkverbinding. Kan worden veroorzaakt door HTTP-poort 8050 probleem, service bus-connectiviteitsprobleem of synchronisatieprobleem in de referentie. 
Inactief | Er is een knooppunt in een van de configuratie van andere knooppunten van de meeste andere configuratie.<br/><br/> Een knooppunt mag inactief zijn als er geen verbinding met andere knooppunten. 


De volgende tabel bevat de mogelijke statussen van een **logische gateway**. Status van de gateway, is afhankelijk van de status van de gateway-knooppunten. 

Status | Opmerkingen
:----- | :-------
Moet worden geregistreerd | Kan geen knooppunt is nog geregistreerd bij deze logische gateway
Online | Gateway-knooppunten zijn online
Offline | Kan geen knooppunt in de online status.
Beperkt | Niet alle knooppunten in deze gateway zijn in orde. Deze status is een waarschuwing dat een bepaald knooppunt is mogelijk niet beschikbaar. <br/><br/>Kan worden veroorzaakt door synchronisatieprobleem in de referentie op knooppunt verzender/werknemer. 

### <a name="pipeline-activities-monitoring"></a>Pijplijn / activiteiten controleren
De Azure portal biedt een pijplijn bewaken met gedetailleerde knooppunt gedetailleerde gegevens. Deze geeft bijvoorbeeld welke activiteiten die worden uitgevoerd op welk knooppunt. Deze informatie kan nuttig zijn bij het begrijpen van prestatieproblemen met op een bepaald knooppunt, bijvoorbeeld vanwege een netwerkbeperking van het. 

![Data Management Gateway - bewaking voor pijplijnen van meerdere knooppunten](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-pipelines.png)

![Data Management Gateway - details van de pijplijn](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-pipeline-details.png)

## <a name="scale-considerations"></a>Overwegingen voor schalen

### <a name="scale-out"></a>Uitschalen
Wanneer de **beschikbare geheugen laag is** en de **CPU-gebruik is hoog**, een nieuw knooppunt toevoegen helpt u scale-out van de belasting tussen computers. Als de activiteiten zijn mislukken vanwege een time-out of gateway-knooppunt offline, is het handig als u een knooppunt aan de gateway toevoegt.
 
### <a name="scale-up"></a>Omhoog schalen
Als het beschikbare geheugen en CPU niet goed worden gebruikt, maar de inactieve capaciteit 0 is, moet u omhoog schalen door het aantal gelijktijdige taken die kunnen worden uitgevoerd op een knooppunt. U kunt ook omhoog schalen wanneer activiteiten zijn time-outs opgetreden omdat de gateway overbelast is. Zoals weergegeven in de volgende afbeelding, kunt u de maximale capaciteit voor een knooppunt verhogen. Het is raadzaam om te beginnen met verdubbelen.  

![Data Management Gateway - overwegingen voor schalen](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-scale-considerations.png)


## <a name="known-issuesbreaking-changes"></a>Bekende problemen/belangrijke wijzigingen

- U kunt op dit moment maximaal vier fysieke gateway-knooppunten voor één logische gateway hebben. Als u meer dan vier knooppunten voor betere prestaties, stuur een e-mail naar [ DMGHelp@microsoft.com ](mailto:DMGHelp@microsoft.com).
- U registreren een gateway-knooppunt opnieuw met de verificatiesleutel uit een andere logische gateway overschakelen van de huidige logische gateway niet. Om te opnieuw te registreren, de gateway van het knooppunt verwijderen en opnieuw installeren van de gateway registreren bij de verificatiesleutel voor de logische gateway. 
- Als HTTP-proxy vereist voor alle gatewayknooppunten van uw is, de proxy in diahost.exe.config en diawp.exe.config instellen en de server manager gebruiken om te controleren of dat alle knooppunten dezelfde diahost.exe.config en diawip.exe.config hebben. Zie [proxy-instellingen configureren](data-factory-data-management-gateway.md#configure-proxy-server-settings) sectie voor meer informatie. 
- Als u wilt wijzigen versleutelingsmodus voor de communicatie van knooppunt-naar-knooppunt in Gateway Configuration Manager, verwijdert u alle knooppunten in de portal, met uitzondering van een. Vervolgens voegt u knooppunten opnieuw na het wijzigen van de versleutelingsmodus.
- Gebruik een officiële SSL-certificaat als u kiest voor het versleutelen van het communicatiekanaal van knooppunt-naar-knooppunt. Zelf-ondertekend certificaat mogelijk problemen met de netwerkverbinding zoals hetzelfde certificaat kan niet worden vertrouwd in certificeren lijst met op andere computers. 
- U kunt een gateway-knooppunt aan een logische gateway niet registreren bij versie van het knooppunt lager dan de versie van de logische gateway is. Alle knooppunten van de logische gateway verwijderen uit de portal zodat u kunt een lagere versie node(downgrade) registreren deze. Als u alle knooppunten van een logische gateway verwijdert, handmatig installeren en registreren van nieuwe knooppunten met die logische gateway. Snelle installatie wordt in dit geval niet ondersteund.
- U kunt snelle installatie niet gebruiken met de installatie van de knooppunten met een bestaande logische gateway, namelijk is nog steeds gebruik van cloudreferenties. U kunt controleren waarbij de referenties van de Gateway Configuration Manager op het tabblad instellingen worden opgeslagen.
- U kunt snelle installatie niet gebruiken met de installatie van de knooppunten met een bestaande logische gateway, die knooppunt-naar-knooppunt versleuteling ingeschakeld is. Als de versleutelingsmodus instellen, moet certificaten handmatig toe te voegen, is snelle installatie niet meer een optie. 
- Een bestandskopie van on-premises omgeving, moet u niet gebruiken \\localhost of C:\files meer sinds localhost of een lokaal station mogelijk niet meer toegankelijk via alle knooppunten. In plaats daarvan gebruik \\ServerName\files bestanden locatie op te geven.


## <a name="rolling-back-from-the-preview"></a>Het terugdraaien van een van de Preview-versie 
Om terug te draaien van de Preview-versie, verwijdert u alle knooppunten maar één knooppunt. Het maakt niet uit welke knooppunten verwijderd, maar zorg ervoor dat u ten minste één knooppunt hebt in de logische gateway. U kunt een knooppunt verwijderen door het verwijderen van de gateway op de computer of met behulp van de Azure-portal. In de Azure-portal in de **Data Factory** pagina, klikt u gekoppelde services om te starten op de **gekoppelde services** pagina. Selecteer de gateway om te starten de **Gateway** pagina. In de Gateway-pagina ziet u de knooppunten die zijn gekoppeld aan de gateway. De pagina kunt u een knooppunt verwijderen uit de gateway.
 
Nadat u hebt verwijderd, klikt u op **preview-functies** in de dezelfde Azure portal-pagina, en schakel de preview-functie. U hebt uw gateway naar één knooppunt algemene beschikbaarheid (algemene beschikbaarheid)-gateway opnieuw instellen.


## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen:
- [Data Management Gateway](data-factory-data-management-gateway.md) -biedt een gedetailleerd overzicht van de gateway.
- [Gegevens verplaatsen tussen on-premises en cloud gegevensarchieven](data-factory-move-data-between-onprem-and-cloud.md) -bevat een overzicht met stapsgewijze instructies voor het gebruik van een gateway met een enkel knooppunt. 
