---
title: Aan de slag met Log Analytics | Microsoft Docs
description: U kunt binnen enkele minuten aan de slag met Log Analytics.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/02/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: f3be7887a33578da1b2df1f5aa02e09c50f22390
ms.openlocfilehash: 5aa5e5006bf1d0d13f96494cc13df2c970f0d765


---
Opmerking: als u Microsoft Azure Government Cloud hebt geopend, gaat u in plaats hiervan naar https://review.docs.microsoft.com/en-us/azure/azure-government/documentation-government-services-monitoringandmanagement#log-analytics.

# <a name="get-started-with-log-analytics"></a>Aan de slag met Log Analytics
Binnen enkele minuten kunt u aan de slag met Log Analytics in de Microsoft Operations Management Suite (OMS). U beschikt over twee opties voor het maken van een OMS-werkruimte (vergelijkbaar met een account):

* Microsoft Operations Management Suite-website
* Microsoft Azure-abonnement

U kunt een gratis OMS-werkruimte maken met behulp van de OMS-website. U kunt ook een Microsoft Azure-abonnement gebruiken om een gratis Log Analytics-werkruimte te maken. In beide gevallen zijn de werkruimten even functioneel. Met gratis werkruimten kunt u slechts 500 MB aan gegevens per dag naar de OMS-service verzenden. Voor alle Azure-werkruimten is een Azure-abonnement vereist. U kunt dat abonnement ook gebruiken voor toegang tot andere Azure-services. Ongeacht de methode die u gebruikt om de werkruimte te maken, maakt u de werkruimte met een Microsoft-account of een organisatieaccount.

Hier volgt een overzicht van het proces:

![diagram van de voorbereidingen](./media/log-analytics-get-started/oms-onboard-diagram.png)

## <a name="log-analytics-prerequisites-and-deployment-considerations"></a>Vereisten en overwegingen voor implementatie van Log Analytics
* U hebt een betaald abonnement op Microsoft Azure nodig om volledig gebruik te kunnen maken van Log Analytics. Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) waarmee u gedurende 30 dagen toegang hebt tot alle Azure-services. U kunt ook een gratis OMS-account maken via de website van [Operations Management Suite](http://microsoft.com/oms).
* U moet een werkruimte maken
* Op elke Windows-computer waarop u gegevens wilt verzamelen, moet Windows Server 2008 SP1 of hoger worden uitgevoerd
* [Firewall](log-analytics-proxy-firewall.md)toegang tot de adressen van de OMS-webservice
* Stel vast of uw computers directe toegang tot internet hebben. Als dit niet het geval is, hebben ze een gatewayserver nodig voor toegang tot de sites van de OMS-webservice. Alle toegang gaat via HTTPS. U kunt een [OMS-gatewayserver](log-analytics-oms-gateway.md) instellen om verkeer van servers door te sturen naar OMS als er op de computers geen toegang tot internet beschikbaar is.
* Als u Operations Manager gebruikt, worden Operations Manager 2012 SP1 UR6 en hoger en Operations Manager 2012 R2 UR2 en hoger door Log Analytics ondersteund. Proxyondersteuning is toegevoegd aan Operations Manager 2012 SP1 UR7 en Operations Manager 2012 R2 UR3. Stel vast hoe dit moet worden geïntegreerd met OMS.
* Bepaal welke technologieën en servers gegevens verzenden naar OMS. Bijvoorbeeld: domeincontrollers, SQL Server, enzovoort.
* Verleen machtigingen aan gebruikers in OMS en Azure.
* Als u zich zorgen maakt over het gegevensgebruik, kunt u elke oplossing afzonderlijk implementeren en de invloed op de prestaties testen voordat u extra oplossingen toevoegt.
* Controleer het gegevensgebruik en de prestaties terwijl u oplossingen en functies aan Log Analytics toevoegt. Dit omvat de gebeurtenisverzameling, logboekverzameling, verzameling van prestatiegegevens, enzovoort. U kunt beter beginnen met minimale verzameling totdat het gegevensgebruik of de invloed op de prestaties zijn geïdentificeerd.
* Controleer of de Windows-agents niet ook worden beheerd met behulp van Operations Manager, want dit leidt tot dubbele gegevens. Dit geldt ook voor op Azure gebaseerde agents waarvoor Azure Diagnostics is ingeschakeld.
* Controleer nadat u agents hebt geïnstalleerd, of de agent goed werkt. Als dit niet het geval is, controleert u of Cryptography API: Next Generation (CNG) Key Isolation niet is uitgeschakeld via Groepsbeleid.
* Voor een aantal Log Analytics-oplossingen gelden aanvullende vereisten

## <a name="sign-up-in-3-steps-using-oms"></a>In drie stappen registreren met OMS
1. Ga naar de [Microsoft Operations Management Suite](http://microsoft.com/oms)-website. Meld u aan met uw Microsoft-account, zoals Outlook.com, of met een organisatie-account dat door uw bedrijf of onderwijsinstelling wordt aangeboden voor gebruik met Office 365 of andere Microsoft-services.
2. Geef een unieke werkruimtenaam op. Een werkruimte is een logische container waarin uw beheergegevens worden opgeslagen. Werkruimtes bieden u een manier om gegevens te partitioneren tussen verschillende teams in uw organisatie, aangezien de gegevens uitsluitend van toepassing zijn voor de relevante werkruimte. Geef een e-mailadres op, evenals de regio waarin u uw gegevens hebt opgeslagen.  
    ![werkruimte maken en abonnement koppelen](./media/log-analytics-get-started/oms-onboard-create-workspace-link01.png)
3. Maak vervolgens een nieuw gratis Azure-abonnement of geef een koppeling op naar een bestaand Azure-abonnement.  
   ![werkruimte maken en abonnement koppelen](./media/log-analytics-get-started/oms-onboard-create-workspace-link02.png)

U kunt nu aan de slag met de Operations Management Suite-portal.

In [Werkruimten beheren](log-analytics-manage-access.md) vindt u meer informatie over het instellen van uw werkruimte en het koppelen van bestaande Azure-gebruikersaccounts aan werkruimten die zijn gemaakt met de Operations Management Suite.

## <a name="sign-up-quickly-using-microsoft-azure"></a>Snel aanmelden met Microsoft Azure
1. Ga naar [Azure Portal](https://portal.azure.com) en meld u aan. Blader door de lijst met services en selecteer **Log Analytics**.  
    ![Azure Portal](./media/log-analytics-get-started/oms-onboard-azure-portal.png)
2. Klik op **Toevoegen** en selecteer opties voor de volgende items:
   * Naam voor de **OMS-werkruimte**
   * **Abonnement**: als u meerdere abonnementen hebt, kiest u het abonnement dat u aan de nieuwe werkruimte wilt koppelen.
   * **Resourcegroep**
   * **Locatie**
   * **Prijscategorie**  
       ![snel maken](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klik op **OK**. U krijgt nu een lijst te zien met uw werkruimten.
4. Selecteer een werkruimte om de details ervan in Azure Portal te bekijken.       
    ![details van de werkruimte](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         
5. Klik op de koppeling van de **OMS Portal** om de website van Operations Management-Suite te openen met uw nieuwe werkruimte.

U kunt nu aan de slag met de Operations Management Suite-portal.

In [Toegang tot Log Analytics beheren](log-analytics-manage-access.md) kunt u meer informatie vinden over het instellen van uw werkruimte en het koppelen van bestaande werkruimten die u hebt gemaakt met de Operations Management Suite, aan Azure-abonnementen.

## <a name="get-started-with-the-operations-management-suite-portal"></a>Aan de slag met de Operations Management Suite-portal
Als u oplossingen wilt kiezen en verbinding wilt maken met de servers die u wilt beheren, klikt u op de tegel **Instellingen** en volgt u de stappen in deze sectie.  

![aan de slag](./media/log-analytics-get-started/oms-onboard-get-started.png)  

1. **Oplossingen toevoegen**: de geïnstalleerde oplossingen weergeven.  
    ![oplossingen](./media/log-analytics-get-started/oms-onboard-solutions.png)  
    Klik op **Ga naar de galerie** om meer oplossingen toe te voegen.  
    ![oplossingen](./media/log-analytics-get-started/oms-onboard-solutions02.png)  
    Selecteer een oplossing en klik vervolgens op **Toevoegen**.
2. **Verbinding maken met een bron**: kies hoe u verbinding wilt maken met uw serveromgeving om gegevens te verzamelen:

   * Direct verbinding maken met een willekeurige Windows-server of -client door een agent te installeren.
   * Linux-servers verbinden met de OMS-agent voor Linux.
   * Een Azure Storage-account gebruiken dat is geconfigureerd met de diagnostische virtuele Azure-machine-extensie van Windows of Linux.
   * System Center Operations Manager gebruiken om uw beheergroepen of de gehele Operations Manager-implementatie te koppelen.
   * Schakel Windows-telemetrie in om Upgradeanalyse te gebruiken.
       ![verbonden bronnen](./media/log-analytics-get-started/oms-onboard-data-sources.png)    
3. **Gegevens verzamelen**Configureer ten minste één gegevensbron om uw werkruimte met gegevens te vullen. Klik op **Opslaan** als u klaar bent.    

    ![gegevens verzamelen](./media/log-analytics-get-started/oms-onboard-logs.png)    

## <a name="optionally-connect-windows-computers-by-installing-an-agent"></a>U kunt optioneel Windows-computers verbinden door een agent te installeren
Het volgende voorbeeld laat zien hoe een Windows-agent installeert.

1. Klik op de tegel **Instellingen**, klik op het tabblad **Verbonden bronnen**, klik op een tabblad voor het type bron dat u wilt toevoegen en download een agent of lees meer informatie over het inschakelen van een agent. Klik bijvoorbeeld op **Windows-agent downloaden (64-bits)**. Voor Windows-agents kunt u de agent alleen installeren op Windows Server 2008 SP 1 of hoger of op Windows 7 SP1 of hoger.
2. Installeer de agent op een of meer servers. U kunt agents één voor één installeren of een meer geautomatiseerde methode met een [aangepast script](log-analytics-windows-agents.md) gebruiken. U kunt ook een bestaande distributie-oplossing voor software gebruiken die u mogelijk hebt.
3. Nadat u akkoord bent gegaan met de gebruiksrechtovereenkomst en u de installatiemap hebt gekozen, selecteert u de optie voor het **koppelen van de agent aan Azure Log Analytics (OMS)**.   
    ![agent instellen](./media/log-analytics-get-started/oms-onboard-agent.png)
4. Op de volgende pagina wordt u gevraagd om uw werkruimte-ID en werkruimtesleutel. Uw werkruimte-ID en -sleutel worden weergegeven op het scherm waarop u het agentbestand hebt gedownload.  
    ![agentsleutels](./media/log-analytics-get-started/oms-onboard-mma-keys.png)  

    ![servers toevoegen](./media/log-analytics-get-started/oms-onboard-key.png)
5. Tijdens de installatie klikt u op **Geavanceerd** om desgewenst uw proxyserver in te stellen en verificatiegegevens op te geven. Klik op de knop **Volgende** om terug te keren naar het scherm met de werkruimtegegevens.
6. Klik op **Volgende** om uw werkruimte-ID en -sleutel te valideren. Als er fouten worden gevonden, kunt u op **Terug** klikken om correcties aan te brengen. Wanneer uw werkruimte-ID en -sleutel zijn gevalideerd, klikt u op **Installeren** om de agentinstallatie te voltooien.
7. Klik in het Configuratiescherm op Microsoft Monitoring Agent > tabblad Azure Log Analytics (OMS). Er wordt een groen vinkje weergegeven wanneer de agents met de Operations Management Suite-service communiceren. Dit duurt in eerste instantie 5-10 minuten.

> [!NOTE]
> De oplossingen voor capaciteitsbeheer en configuratiebeoordeling worden momenteel niet ondersteund door servers die rechtstreeks zijn verbonden met de Operations Management Suite.


U kunt de agent ook verbinden met System Center Operations Manager 2012 SP1 en hoger. Als u dit wilt doen, selecteert u de optie voor het **verbinden van de agent met System Center Operations Manager**. Als u die optie kiest, kunt u gegevens verzenden naar de service zonder extra hardware of belasting van uw beheergroepen.

Meer informatie over het verbinden van agents met de Operations Management Suite vindt u in [Windows-computers verbinden met Log Analytics](log-analytics-windows-agents.md).

## <a name="optionally-connect-servers-using-system-center-operations-manager"></a>Servers verbinden met System Center Operations Manager (optioneel)
1. Selecteer **Beheer** in de Operations Manager-console.
2. Vouw het knooppunt **Operational Insights** uit en selecteer **Verbinding met Operational Insights**.

   > [!NOTE]
   > Afhankelijk van het updatepakket van SCOM dat u gebruikt, wordt er een knooppunt voor *System Center Advisor*, *Operational Insights* of *Operations Management Suite* weergegeven.
   >
   >
3. Klik rechts bovenaan op de koppeling **Registreren bij Operational Insights** en volg de instructies.
4. Nadat de registratiewizard is voltooid, klikt u op de koppeling **Een computer/groep toevoegen**.
5. In het dialoogvenster **Computer zoeken** kunt u zoeken naar computers of groepen die worden bewaakt door Operations Manager. Selecteer computers of groepen om deze toe te voegen aan Log Analytics, klik op **Toevoegen** en klik vervolgens op **OK**. U kunt controleren of de OMS-service gegevens ontvangt door naar de tegel **Gebruik** in de Operations Management Suite-portal te gaan. Gegevens moeten na ongeveer 5 tot 10 minuten worden weergegeven.

Meer informatie over het verbinden van Operations Manager met de Operations Management Suite vindt u in [Operations Manager verbinden met Log Analytics](log-analytics-om-agents.md).

## <a name="optionally-analyze-data-from-cloud-services-in-microsoft-azure"></a>Gegevens uit cloudservices analyseren in Microsoft Azure (optioneel)
Met de Operations Management-Suite kunt u snel zoeken naar gebeurtenis- en IIS-logboeken voor cloudservices en virtuele machines door diagnostische gegevens in te schakelen voor Azure Cloud Services. U kunt ook extra inzichten ontvangen voor uw virtuele Azure-machines door de Microsoft Monitoring Agent te installeren. Meer informatie over het configureren van uw Azure-omgeving voor het gebruik van de Operations Management Suite vindt u in [Azure Storage verbinden met Log Analytics](log-analytics-azure-storage.md).

## <a name="next-steps"></a>Volgende stappen
* [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](log-analytics-add-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.
* Vertrouwd raken met [zoekopdrachten naar logboeken](log-analytics-log-searches.md) om gedetailleerde informatie te bekijken die is verzameld door oplossingen.
* [Dashboards](log-analytics-dashboards.md) gebruiken voor het opslaan en weergeven van uw eigen aangepaste zoekopdrachten.



<!--HONumber=Feb17_HO1-->


