---
title: Maak verbinding met Configuration Manager met Log Analytics | Microsoft Docs
description: In dit artikel bevat de stappen voor het verbinden van Configuration Manager met logboekanalyse en analyseren van gegevens te starten.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: magoedte
ms.openlocfilehash: 5ff0687fe99f0853e29e5f0d814a8555c367027c
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Maak verbinding met Configuration Manager met Log Analytics
U kunt uw System Center Configuration Manager-omgeving met Azure Log Analytics sync verzameling apparaatgegevens verbinding en verwijzen naar deze verzamelingen in Log Analytics en Azure Automation.  

## <a name="prerequisites"></a>Vereisten

Log Analytics biedt ondersteuning voor System Center Configuration Manager current branch versie 1606 of hoger.  

## <a name="configuration-overview"></a>Configuratie-overzicht
De volgende stappen geven een overzicht van de stappen voor het configureren van Configuration Manager-integratie met logboekanalyse.  

1. Configuration Manager registreert als een webtoepassing en/of Web-API-app in de Azure-portal en zorg ervoor dat u de client-ID en een geheime sleutel van de client van de registratie van Azure Active Directory. Zie [portal gebruik maken van Active Directory-toepassing en service-principal die toegang bronnen tot](../azure-resource-manager/resource-group-create-service-principal-portal.md) voor gedetailleerde informatie over het uitvoeren van deze stap.
2. In de Azure portal [verlenen van Configuration Manager (de geregistreerde web-app) met machtigingen voor toegang tot logboekanalyse](#grant-configuration-manager-with-permissions-to-log-analytics).
3. In Configuration Manager [toevoegen van een verbinding met de Wizard OMS-verbinding toevoegen](#add-an-oms-connection-to-configuration-manager).
4. In Configuration Manager [werk de verbindingseigenschappen](#update-oms-connection-properties) als de geheime sleutel met het wachtwoord of client ooit is verlopen of verloren gegaan is.
5. [Download en installeer de Microsoft Monitoring Agent](#download-and-install-the-agent) op de computer waarop de Configuration Manager service connection point sitesysteemrol. De agent verzendt de Configuration Manager-gegevens naar de werkruimte voor logboekanalyse.
6. In Log Analytics [verzamelingen importeren uit Configuration Manager](#import-collections) als computergroepen.
7. Weergeven in Log Analytics gegevens uit Configuration Manager als [computergroepen](log-analytics-computer-groups.md).

Meer informatie over het aansluiten van Configuration Manager met OMS op [synchroniseren van gegevens uit Configuration Manager met de Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="grant-configuration-manager-with-permissions-to-log-analytics"></a>Verleen Configuration Manager met machtigingen voor logboekanalyse
In de volgende procedure verleent u aan de *Inzender* rol in de werkruimte voor logboekanalyse voor het AD-toepassing en service-principal die u eerder hebt gemaakt voor Configuration Manager.  Als u nog geen een werkruimte, Zie [een werkruimte maken in Azure-logboekanalyse](log-analytics-quick-create-workspace.md) voordat u doorgaat.  Hierdoor kan de Configuration Manager om te verifiëren en verbinding maken met uw werkruimte voor logboekanalyse.  

> [!NOTE]
> U moet machtigingen opgeven in Log Analytics voor Configuration Manager. Anders wordt een foutbericht weergegeven wanneer u de configuratiewizard in Configuration Manager gebruiken.
>

1. Klik in de Azure-portal op **alle services** gevonden in de linkerbovenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Selecteer in de lijst met Log Analytics-werkruimten, de werkruimte te wijzigen.
3. Selecteer in het linkerdeelvenster **Access Control (IAM)**.
4. Klik op de pagina toegangsbeheer **toevoegen** en de **machtigingen toevoegen** deelvenster wordt weergegeven.
5. In de **machtigingen toevoegen** deelvenster onder de **rol** vervolgkeuzelijst, selecteer de **Inzender** rol.  
6. Onder de **toewijzen van toegang tot** vervolgkeuzelijst, selecteer de Configuration Manager-toepassing eerder hebt gemaakt in AD in en klik vervolgens op **OK**.  

## <a name="download-and-install-the-agent"></a>Download en installeer de agent
Lees het artikel [verbinding maken met Windows-computers naar de Log Analytics-service in Azure](log-analytics-agent-windows.md) om te begrijpen van de methoden die beschikbaar zijn voor het installeren van Microsoft Monitoring Agent op de computer die als host fungeert voor de Configuration Manager-service verbinding sitesysteemrol.  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Een OMS-verbinding toevoegen aan Configuration Manager
Als u wilt een OMS-verbinding toevoegen, uw Configuration Manager-omgeving moet hebben een [serviceaansluitpunt](https://technet.microsoft.com/library/mt627781.aspx) geconfigureerd voor de onlinemodus bevindt.

1. In de **beheer** werkruimte van Configuration Manager, selecteer **OMS Connector**. Hiermee opent u de **OMS verbinding Wizard toevoegen**. Selecteer **Volgende**.
2. Op de **algemene** scherm, bevestig dat u de volgende acties hebt gedaan en dat u hebt de details voor elk item, en selecteer **volgende**.

   1. In de Azure portal hebt u Configuration Manager geregistreerd als een webtoepassing en/of Web-API-app en die u hebt de [client-ID van de registratie](../active-directory/active-directory-integrating-applications.md).
   2. U kunt een geheime sleutel van de app voor de geregistreerde app in Azure Active Directory hebt gemaakt in de Azure portal.  
   3. In de Azure portal kunt u de geregistreerde web-app hebt opgegeven met de machtiging voor toegang tot OMS.  
      ![Verbinding met OMS Wizard algemene pagina](./media/log-analytics-sccm/sccm-console-general01.png)
3. Op de **Azure Active Directory** scherm, het configureren van instellingen voor de verbinding met logboekanalyse doordat uw **Tenant**, **Client-ID**, en **Client De geheime sleutel**, selecteer daarna **volgende**.  
   ![Verbinding met OMS Wizard Azure Active Directory-pagina](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Als u bereikt de procedures geslaagd, klikt u vervolgens de informatie op de **OMS verbindingsconfiguratie** scherm automatisch wordt weergegeven op deze pagina. Informatie over de verbindingsinstellingen moet worden weergegeven voor uw **Azure-abonnement**, **Azure-resourcegroep**, en **Operations Management Suite-werkruimte**.  
   ![Verbinding met de pagina OMS Wizard OMS-verbinding](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. De wizard maakt verbinding met de Log Analytics-service met behulp van de informatie die u hebt ingevoerd. Selecteer de apparaatverzamelingen die u wilt synchroniseren met de service en klik vervolgens op **toevoegen**.  
   ![Selecteer verzamelingen](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Controleer de verbindingsinstellingen op de **samenvatting** scherm en selecteer vervolgens **volgende**. De **voortgang** scherm toont de verbindingsstatus en vervolgens moet **Complete**.

> [!NOTE]
> U moet de bovenste site verbinding te maken in uw hiërarchie met logboekanalyse. Als u een zelfstandige primaire site te met Log Analytics verbinden en vervolgens een centrale beheersite toe te aan uw omgeving voegen, die u moet verwijderen en opnieuw maken van de verbinding in de nieuwe hiërarchie.
>
>

Nadat u de Configuration Manager aan Log Analytics hebt gekoppeld, kunt u toevoegen of verwijderen van verzamelingen en de eigenschappen van de verbinding weergeven.

## <a name="update-log-analytics-connection-properties"></a>Verbindingseigenschappen voor logboekanalyse bijwerken
Als een geheime sleutel met het wachtwoord of de client nooit verloopt of verloren, moet u de verbindingseigenschappen van logboekanalyse handmatig bijwerken.

1. In Configuration Manager, gaat u naar **Cloudservices**, selecteer daarna **OMS-Connector** openen de **OMS verbindingseigenschappen** pagina.
2. Op deze pagina, klikt u op de **Azure Active Directory** tabblad om uw **Tenant**, **Client-ID**, **Client geheime sleutelverloop**. **Controleer of** uw **geheime sleutel van de Client** als deze is verlopen.

## <a name="import-collections"></a>Verzamelingen importeren
Nadat u hebt een OMS-verbinding naar Configuration Manager toegevoegd en de agent hebt geïnstalleerd op de computer met de verbinding met de Configuration Manager sitesysteemrol, wordt de volgende stap verzamelingen importeren uit Configuration Manager in logboekanalyse als computergroepen.

Nadat u de eerste configuratie voor het importeren van apparaatverzamelingen uit uw hiërarchie hebt voltooid, wordt de lidmaatschapsgegevens van verzameling elke drie uur om het lidmaatschap van de huidige opgehaald. U kunt dit op elk moment uitschakelen.

1. Klik in de Azure-portal op **alle services** gevonden in de linkerbovenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. Selecteer de werkruimte Configuration Manager is geregistreerd bij in uw lijst met Log Analytics-werkruimten.  
3. Selecteer **Geavanceerde instellingen**.<br><br> ![Geavanceerde instellingen van Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecteer **computergroepen** en selecteer vervolgens **SCCM**.  
5. Selecteer **Import Configuration Manager-verzamelinglidmaatschappen** en klik vervolgens op **opslaan**.  
   ![Computergroepen - SCCM-tabblad](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Gegevens weergeven van Configuration Manager
Nadat u hebt een OMS-verbinding naar Configuration Manager toegevoegd en de agent op de computer met de Configuration Manager service connection point sitesysteemrol hebt geïnstalleerd, worden gegevens van de agent wordt verzonden met logboekanalyse. In Log Analytics uw Configuration Manager-verzamelingen worden weergegeven als [computergroepen](log-analytics-computer-groups.md). Vindt u de groepen van de **Configuration Manager** pagina onder **Settings\Computer groepen**.

Nadat de verzamelingen zijn geïmporteerd, kunt u zien hoeveel computers met verzamelinglidmaatschappen zijn gedetecteerd. U ziet ook het aantal verzamelingen die zijn geïmporteerd.

![Computergroepen - SCCM-tabblad](./media/log-analytics-sccm/sccm-computer-groups02.png)

Als u op een, zoekopdracht wordt geopend ofwel alle van de geïmporteerde groepen of alle computers die deel uitmaken van elke groep. Met behulp van [logboek zoeken](log-analytics-log-searches.md), kunt u beginnen met gedetailleerde analyse van Configuration Manager-gegevens.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [logboek zoeken](log-analytics-log-searches.md) om gedetailleerde informatie over uw Configuration Manager-gegevens weer te geven.
