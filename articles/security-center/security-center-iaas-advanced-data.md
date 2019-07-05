---
title: Geavanceerde beveiliging van gegevens voor IaaS in Azure Security Center | Microsoft Docs
description: " Informatie over het inschakelen van geavanceerde beveiliging voor IaaS in Azure Security Center. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: monhaber
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: monhaber
ms.openlocfilehash: ed94b92a34e2989c9f2226c344ac4d34a279eeac
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551850"
---
# <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-public-preview"></a>Geavanceerde beveiliging voor SQL-servers op Azure Virtual Machines (openbare Preview)
Geavanceerde beveiliging voor SQL-Servers op Azure Virtual Machines is een geïntegreerde-pakket voor geavanceerde mogelijkheden voor de beveiliging van SQL. Momenteel (in openbare preview-versie) bevat functionaliteit voor zichtbaar te maken en potentiële databaseproblemen beperkende en opsporen van afwijkende activiteiten die op een bedreiging voor uw database wijzen kunnen. 

Deze beveiliging bieden voor Azure VM's SQL-servers is gebaseerd op de fundamentele technologie die wordt gebruikt de [geavanceerde gegevensbeveiliging van Azure SQL Database-pakket](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).


## <a name="overview"></a>Overzicht

Geavanceerde gegevensbeveiliging biedt een geavanceerde SQL-beveiligingsmogelijkheden, die bestaat uit een evaluatie van beveiligingsproblemen en Advanced Threat Protection.

* [Evaluatie van beveiligingsproblemen](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) is een eenvoudig te configureren van de service die u kunt detecteren, volgen en u helpen potentiële databaseproblemen oplossen. Het biedt meer inzicht in uw beveiligingsstatus en bevat de stappen voor het oplossen van beveiligingsproblemen met zich mee en uw database fortifications te verbeteren.
* [Advanced Threat Protection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van uw SQL-server aangeeft. Continu bewaakt uw database voor verdachte activiteiten en vindt u op actie gerichte beveiligingswaarschuwingen over afwijkende patronen voor databasetoegang. Deze waarschuwingen bieden de details van verdachte activiteiten en aanbevolen acties om te onderzoeken en tegenhouden.

## <a name="get-started-with-advanced-data-security-for-sql-on-azure-vms"></a>Aan de slag met geavanceerde beveiliging van gegevens voor SQL op Azure Virtual machines

De volgende stappen uit om u aan de slag met geavanceerde beveiliging van gegevens voor SQL in openbare Preview van Azure VM's.

### <a name="set-up-advanced-data-security-for-sql-on-azure-vms"></a>Instellen van geavanceerde gegevensbeveiliging voor SQL op Azure Virtual machines

**Voordat u begint met**: U moet een werkruimte voor logboekanalyse voor het opslaan van de beveiligingslogboeken die wordt geanalyseerd. Als u geen hebt, wordt u een eenvoudig maken kunt, zoals uitgelegd in [een Log Analytics-werkruimte maken in Azure portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

1. Verbinding maken met de virtuele machine die als host fungeert voor de SQL-server naar de Log Analytics-werkruimte. Zie voor instructies [verbinding maken met Windows-computers naar Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).

1. Van Azure Marketplace, gaat u naar de [oplossing voor gegevensbeveiliging voor SQL-geavanceerde](https://ms.portal.azure.com/#create/Microsoft.SQLAdvancedDataSecurity).
(U kunt zoeken met behulp van de zoekfunctie van marketplace vinden in de volgende afbeelding.) De **SQL Advanced gegevensbeveiliging** pagina wordt geopend.

    ![Geavanceerde beveiliging voor IaaS](./media/security-center-advanced-iaas-data/sql-advanced-data-security.png)

1. Klik op **Create**. De werkruimten worden weergegeven.

    ![Beveiliging van geavanceerde gegevens maken](./media/security-center-advanced-iaas-data/sql-advanced-data-create.png)

1. Selecteer de werkruimte wilt gebruiken en op **maken**.

   ![Werkruimte selecteren](./media/security-center-advanced-iaas-data/sql-workspace.png)

1. Start opnieuw op de [virtuele machine van SQL server](https://docs.microsoft.com/sql/database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services?view=sql-server-2017).


## <a name="explore-and-investigate-security-alerts"></a>Verken en beveiligingswaarschuwingen onderzoeken

U kunt weergeven en beheren van uw huidige beveiligingswaarschuwingen.

1. Klik op **Security Center** > **beveiligingswaarschuwingen**, en klik op een waarschuwing.

    ![Waarschuwing zoeken](./media/security-center-advanced-iaas-data/find-alert.png)

1. Uit de **aangevallen Resource** kolom, klik op een resource die is aangevallen.

1. Als u wilt weergeven Waarschuwingsdetails en acties voor de huidige bedreiging te onderzoeken en toekomstige bedreigingen-adressering, schuif naar beneden de **algemene informatie** pagina en klik in de **herstelstappen** sectie, klikt u op de  **ONDERZOEKSVOLGORDE** koppeling.

    ![Herstelstappen](./media/security-center-advanced-iaas-data/remediation-steps.png)

1. Als u de logboeken die gekoppeld zijn aan het activeren van de waarschuwing, gaat u naar **Log analytics-werkruimten** en voer de volgende stappen uit:

     > [!NOTE]
     > Als **Log analytics-werkruimten** niet wordt weergegeven op het menu links, klikt u op **alle services**, en zoek naar de **Log analytics-werkruimten**.

    1. Zorg ervoor dat de kolommen worden weergegeven de **prijscategorie** en **WorkspaceID** kolommen. (**Log analytics-werkruimten** > **kolommen bewerken**, toevoegen **prijscategorie** en **WorkspaceID**.)

     ![Kolommen bewerken](./media/security-center-advanced-iaas-data/edit-columns.png)

    1. Klik op de werkruimte die de logboeken van de waarschuwing heeft.

    1. Onder de **algemene** menu, klikt u op **Logboeken**

    1. Klik op het oogpictogram naast **SQLAdvancedThreatProtection** tabel. De logboeken worden weergegeven.

     ![Logboeken weergeven](./media/security-center-advanced-iaas-data/view-logs.png)

## <a name="set-up-email-notification-for-atp-alerts"></a>Instellen van e-mailmeldingen voor waarschuwingen voor ATP 

U kunt instellen dat een lijst met ontvangers een e-mailmelding ontvangen wanneer ASC waarschuwingen worden gegenereerd. Het e-mailbericht bevat een directe koppeling naar de waarschuwing in Azure Security Center met alle relevante informatie. 

1. Ga naar **Security Center** > **prijzen & stellingen** en klik op het betreffende abonnement

    ![Instellingen voor abonnement](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. Uit de **instellingen** menu klikt u op **e-mailmeldingen**. 
1. In de **e-mailadres** tekst voert u de e-mailadressen voor het ontvangen van meldingen. U kunt meer dan één e-mailadres invoeren door de e-mailadressen scheiden met een komma (,).  Bijvoorbeeld admin1@mycompany.com,admin2@mycompany.com,admin3@mycompany.com

      ![E-mailinstellingen](./media/security-center-advanced-iaas-data/email-settings.png)

1. In de **e-mailmelding** instellingen, stelt u de volgende opties:
  
    * **Verzenden van e-mailmeldingen voor waarschuwingen met hoge urgentie**: Verzenden in plaats van het verzenden van e-mailberichten voor alle waarschuwingen, alleen voor waarschuwingen met hoge urgentie.
    * **Ook e-mailmeldingen te verzenden naar abonnementseigenaren**:  Meldingen naar de eigenaren van abonnementen te verzenden.

1. In de rechterbovenhoek van de **e-mailmeldingen** scherm, klikt u op **opslaan**.

  > [!NOTE]
  > Klik op **opslaan** voordat u sluit het venster, of de nieuwe **e-mailmelding** instellingen worden niet opgeslagen.

## <a name="explore-vulnerability-assessment-reports"></a>Beveiligingslek met betrekking tot Assessment Reports verkennen

Het dashboard van de evaluatie van beveiligingsproblemen biedt een overzicht van de resultaten van de beoordeling voor alle databases. U kunt de distributie van databases op basis van SQL Server-versie, samen met een samenvatting van mislukte versus het doorgeven van databases en een algemeen overzicht van de mislukte controles op basis van risico's distributie weergeven.

U kunt uw resultaten van de beoordeling beveiligingsproblemen en -rapporten rechtstreeks vanuit de Log Analytics weergeven.

1. Navigeer naar uw Log Analytics-werkruimte met de oplossing voor geavanceerde beveiliging van gegevens.
1. Navigeer naar **oplossingen** en selecteer de **evaluatie van beveiligingsproblemen SQL** oplossing.
1. In de **samenvatting** deelvenster, klikt u op **samenvatting weergeven** en selecteer uw **evaluatierapport SQL-beveiligingsproblemen**.

    ![SQL-evaluatierapport](./media/security-center-advanced-iaas-data/ads-sql-server-1.png)

    Het rapportagedashboard wordt geladen. Zorg ervoor dat het tijdvenster is ingesteld op ten minste de **afgelopen 7 dagen** omdat beveiligingslek evaluatie scans worden uitgevoerd op uw databases op een vast schema van één keer per 7 dagen.

    ![Afgelopen 7 dagen instellen](./media/security-center-advanced-iaas-data/ads-sql-server-2.png)

1. Als u wilt inzoomen voor meer informatie, klikt u op een van de dashboard-elementen. Bijvoorbeeld:

   1. Klikt u op een door een beveiligingslek in de **mislukt controleert samenvatting** sectie om een Log Analytics-tabel met de resultaten voor deze controle voor alle databases weer te geven. Resultaten hebben worden eerst weergegeven.

   1. Klik vervolgens op via details van elk beveiligingsprobleem, met inbegrip van de beschrijving van de beveiligingsproblemen en impact, status, bijbehorende risico's en de werkelijke resultaten voor deze database wilt weergeven. U ziet ook de werkelijke Query die is uitgevoerd voor het uitvoeren van deze controle, en herstelinformatie voor het oplossen van dit beveiligingsprobleem.

    ![Werkruimte selecteren](./media/security-center-advanced-iaas-data/ads-sql-server-3.png)

    ![Werkruimte selecteren](./media/security-center-advanced-iaas-data/ads-sql-server-4.png)

1. U kunt geen Log Analytics-query's uitvoeren op uw gegevens van de evaluatie van beveiligingsproblemen resultaten om de gegevens volgens uw behoeften.

## <a name="advanced-threat-protection-for-sql-servers-on-azure-vms-alerts"></a>Advanced Threat Protection voor SQL-Servers op virtuele machines in Azure-waarschuwingen
Waarschuwingen worden gegenereerd door ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van SQL-Servers. Deze gebeurtenissen kunnen de volgende waarschuwing is geactiveerd:

### <a name="anomalous-access-pattern-alerts-supported-in-public-preview"></a>Afwijkende toegang patroon waarschuwingen (ondersteund in openbare preview-versie)

* **Toegang vanaf ongebruikelijke locatie:** Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de SQL server, waarbij iemand heeft aangemeld bij de SQL server vanuit een ongebruikelijke geografische locatie. Mogelijke oorzaken:
     * Een aanvaller of voormalige schadelijke werknemers toegang heeft gehad tot uw SQL-Server.
     * Een bevoegde gebruiker heeft toegang tot uw SQL-Server vanaf een nieuwe locatie.
* **Toegang vanaf een mogelijk schadelijke toepassing**: deze waarschuwing wordt geactiveerd wanneer een mogelijk schadelijke toepassing wordt gebruikt voor toegang tot de database. Mogelijke oorzaken:
     * Een aanvaller die probeert tot een inbreuk op uw SQL met behulp van gebruikelijk aanvalsprogramma's.
     * Een legitieme indringingstests in actie.
* **Toegang vanaf ongebruikelijke**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot de SQL server, waarbij iemand zich heeft aangemeld met de SQL-server met behulp van een ongebruikelijke klant (SQL-gebruiker). Mogelijke oorzaken:
     * Een aanvaller of voormalige schadelijke werknemers toegang heeft gehad tot uw SQL-Server. 
     * Een bevoegde gebruiker toegang heeft gekregen voor uw SQL-Server uit met een nieuwe-principal.
* **Brute force SQL-referenties**: Deze waarschuwing wordt geactiveerd wanneer er een abnormaal groot aantal mislukte aanmeldingen met andere referenties. Mogelijke oorzaken:
     * Een aanvaller die probeert tot een inbreuk op uw SQL met brute kracht.
     * Een legitieme indringingstests in actie.

### <a name="potential-sql-injection-attacks-coming"></a>Mogelijke SQL-injectieaanvallen (afkomstig)

* **Kwetsbaarheid voor SQL-injectie**: Deze waarschuwing wordt geactiveerd wanneer een toepassing een foutieve SQL-instructie in de database genereert. Deze waarschuwing kan duiden op een mogelijk beveiligingsprobleem met SQL-injectieaanvallen. Mogelijke oorzaken:
     * Een fout in de toepassingscode die de foutieve SQL-instructie maakt
     * Toepassingscode of opgeslagen procedures schonen gebruikersinvoer niet op tijdens het construeren van de foutieve SQL-instructie, dit kan worden misbruikt voor SQL-injectie
* **Mogelijke SQL-injectie**: Deze waarschuwing wordt geactiveerd wanneer een actieve aanval wordt uitgevoerd tegen een geïdentificeerde toepassing beveiligingslek voor SQL-injectie. Dit betekent dat de aanvaller schadelijke SQL-instructies probeert te injecteren met de kwetsbare toepassingscode of opgeslagen procedures.
