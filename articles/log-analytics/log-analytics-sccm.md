---
title: Configuration Manager koppelen aan Log Analytics | Microsoft Docs
description: In dit artikel bevat de stappen voor Configuration Manager koppelen aan Log Analytics en analyseren van gegevens.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 611f3e70425cd6c80f8a976606dc6cd592571c6e
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503465"
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Configuration Manager koppelen aan Log Analytics
U kunt uw System Center Configuration Manager-omgeving koppelen aan Azure Log Analytics apparaat verzameling om gegevens te synchroniseren en verwijzen naar deze verzamelingen in Log Analytics en Azure Automation.  

## <a name="prerequisites"></a>Vereiste onderdelen

Log Analytics biedt ondersteuning voor System Center Configuration Manager current branch versie 1606 en hoger.  

## <a name="configuration-overview"></a>Configuratie-overzicht
De volgende stappen geven een overzicht van de stappen voor het configureren van Configuration Manager-integratie met Log Analytics.  

1. Configuration Manager registreert als een webtoepassing en/of Web-API-app in Azure portal, en zorg ervoor dat u de client-ID en de geheime sleutel van de client van de registratie van Azure Active Directory hebt. Zie [portal gebruiken voor het maken van Active Directory toepassing en service-principal die toegang hebben tot resources](../azure-resource-manager/resource-group-create-service-principal-portal.md) voor gedetailleerde informatie over het uitvoeren van deze stap.
2. In de Azure-portal [verlenen van Configuration Manager (de geregistreerde web-app) met een machtiging voor toegang tot Log Analytics](#grant-configuration-manager-with-permissions-to-log-analytics).
3. In Configuration Manager [toevoegen van een verbinding met de Wizard OMS-verbinding toevoegen](#add-an-oms-connection-to-configuration-manager).
4. In Configuration Manager [bijwerken van de verbindingseigenschappen](#update-oms-connection-properties) als de geheime sleutel van het wachtwoord of de client ooit is verlopen of verloren gegaan is.
5. [Download en installeer de Microsoft Monitoring Agent](#download-and-install-the-agent) op de computer waarop de Configuration Manager service connection point sitesysteemrol. De agent verzendt de Configuration Manager-gegevens naar de Log Analytics-werkruimte.
6. In Log Analytics, [verzamelingen importeren uit Configuration Manager](#import-collections) als computergroepen.
7. Weergeven in Log Analytics, gegevens uit Configuration Manager als [computergroepen](log-analytics-computer-groups.md).

U kunt meer lezen over het verbinden van Configuration Manager met OMS op [synchroniseren van gegevens uit Configuration Manager met de Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="grant-configuration-manager-with-permissions-to-log-analytics"></a>Verleen Configuration Manager met machtigingen voor Log Analytics
In de volgende procedure, verleent u de *Inzender* rol in uw Log Analytics-werkruimte aan de Active Directory-toepassing en service-principal die u eerder hebt gemaakt voor Configuration Manager.  Als u een werkruimte niet al hebt, raadpleegt u [een werkruimte maken in Azure Log Analytics](log-analytics-quick-create-workspace.md) voordat u doorgaat.  Hiermee kunt Configuration Manager om te verifiëren en verbinding maken met uw Log Analytics-werkruimte.  

> [!NOTE]
> U moet machtigingen opgeven in Log Analytics voor Configuration Manager. Anders ontvangt u een foutbericht weergegeven wanneer u de configuratiewizard in Configuration Manager gebruiken.
>

1. Klik in Azure Portal in de linkerbovenhoek op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. In de lijst met Log Analytics-werkruimten, selecteer de werkruimte te wijzigen.
3. Selecteer in het linkerdeelvenster **Access Control (IAM)**.
4. In de Access Control-pagina, klikt u op **toevoegen** en de **machtigingen toevoegen** deelvenster wordt weergegeven.
5. In de **machtigingen toevoegen** deelvenster onder de **rol** vervolgkeuzelijst, selecteer de **Inzender** rol.  
6. Onder de **toegang toewijzen aan** vervolgkeuzelijst, selecteer de Configuration Manager-toepassing eerder hebt gemaakt in AD, en klik vervolgens op **OK**.  

## <a name="download-and-install-the-agent"></a>De agent downloaden en installeren
Lees het artikel [verbinding maken met Windows-computers naar de Log Analytics-service in Azure](log-analytics-agent-windows.md) om te begrijpen van de methoden die beschikbaar zijn voor het installeren van de Microsoft Monitoring Agent op de computer die als host fungeert de service van Configuration Manager verbinding sitesysteemrol.  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Een OMS-verbinding toevoegen aan Configuration Manager
Als u wilt toevoegen een OMS-verbinding, uw Configuration Manager-omgeving moet hebben een [serviceaansluitpunt](https://technet.microsoft.com/library/mt627781.aspx) geconfigureerd voor de onlinemodus bevindt.

1. In de **beheer** werkruimte van Configuration Manager, selecteer **OMS-Connector**. Hiermee opent u de **Wizard voor OMS-verbinding toevoegen**. Selecteer **Volgende**.
2. Op de **algemene** scherm, bevestigt u dat u de volgende acties hebt uitgevoerd en dat u details voor elk item hebben, en selecteer vervolgens **volgende**.

   1. In Azure portal kunt u Configuration Manager als een webtoepassing en/of Web-API-app hebt geregistreerd, en dat u hebt de [client-ID van de registratie](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md).
   2. U kunt een geheime sleutel van de app voor de geregistreerde app in Azure Active Directory hebt gemaakt in de Azure-portal.  
   3. In de Azure-portal, kunt u de geregistreerde web-app hebt opgegeven met de machtiging voor toegang tot OMS.  
      ![Verbinding met OMS Wizard algemene pagina](./media/log-analytics-sccm/sccm-console-general01.png)
3. Op de **Azure Active Directory** scherm, instellingen voor de verbinding met Log Analytics configureren door te geven uw **Tenant**, **Client-ID**, en **Client Geheime sleutel**en selecteer vervolgens **volgende**.  
   ![Verbinding met OMS Wizard Azure Active Directory-pagina](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Als u bereikt de procedures is, klikt u vervolgens de informatie op de **OMS verbindingsconfiguratie** scherm wordt automatisch weergegeven op deze pagina. Informatie over de instellingen van de verbinding moet worden weergegeven voor uw **Azure-abonnement**, **Azure-resourcegroep**, en **Operations Management Suite-werkruimte**.  
   ![Verbinding met de pagina met OMS Wizard OMS te verbinden](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. De wizard maakt verbinding met de Log Analytics-service met behulp van de gegevens die u hebt ingevoerd. Selecteer de apparaatverzamelingen die u wilt synchroniseren met de service en klik vervolgens op **toevoegen**.  
   ![Verzamelingen selecteren](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Controleer de verbindingsinstellingen op de **samenvatting** scherm en selecteer vervolgens **volgende**. De **voortgang** scherm ziet u de status van de verbinding en vervolgens moet **voltooid**.

> [!NOTE]
> U moet verbinding maken met de site op hoogste niveau in uw hiërarchie naar Log Analytics. Als u een zelfstandige primaire site verbinden met Log Analytics en vervolgens een centrale beheersite aan uw omgeving toevoegen, die u moet verwijderen en opnieuw maken van de verbinding in de nieuwe hiërarchie.
>
>

Nadat u de Configuration Manager aan Log Analytics hebt gekoppeld, kunt u toevoegen of verwijderen van verzamelingen en bekijk de eigenschappen van de verbinding.

## <a name="update-log-analytics-connection-properties"></a>Log Analytics-verbindingseigenschappen bijwerken
Als een wachtwoord of de client geheime sleutel ooit is verlopen of verloren gaat, moet u handmatig bijwerken van de eigenschappen van de Log Analytics.

1. In Configuration Manager, gaat u naar **Cloudservices**en selecteer vervolgens **OMS-Connector** openen de **OMS verbindingseigenschappen** pagina.
2. Op deze pagina, klikt u op de **Azure Active Directory** tabblad om uw **Tenant**, **Client-ID**, **Client geheime sleutel verloopt**. **Controleer of** uw **geheime sleutel van de Client** als deze is verlopen.

## <a name="import-collections"></a>Verzamelingen importeren
Nadat u hebt toegevoegd een OMS-verbinding naar Configuration Manager en de agent hebt geïnstalleerd op de computer met de verbinding met de Configuration Manager punt sitesysteemrol, de volgende stap is het verzamelingen importeren uit Configuration Manager in Log Analytics als computergroepen.

Nadat u de eerste configuratie voor het apparaatverzamelingen importeren uit uw hiërarchie hebt voltooid, wordt het lidmaatschap Verzamelingsgegevens elke drie uur om het lidmaatschap van de huidige opgehaald. U kunt dit op elk moment uitschakelen.

1. Klik in Azure Portal in de linkerbovenhoek op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. Selecteer in de lijst met Log Analytics-werkruimten, de Configuration Manager is geregistreerd bij de werkruimte.  
3. Selecteer **Geavanceerde instellingen**.<br><br> ![Geavanceerde instellingen van Log Analytics](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecteer **computergroepen** en selecteer vervolgens **SCCM**.  
5. Selecteer **Import Configuration Manager-verzamelingslidmaatschappen** en klik vervolgens op **opslaan**.  
   ![Computergroepen - SCCM-tabblad](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Gegevens weergeven uit Configuration Manager
Nadat u hebt toegevoegd een OMS-verbinding naar Configuration Manager en de agent op de computer met de Configuration Manager service connection point sitesysteemrol hebt geïnstalleerd, worden gegevens van de agent wordt verzonden naar Log Analytics. In Log Analytics, uw Configuration Manager-verzamelingen worden weergegeven als [computergroepen](log-analytics-computer-groups.md). U vindt de groepen van de **Configuration Manager** pagina onder **Settings\Computer groepen**.

Nadat de verzamelingen die zijn geïmporteerd, kunt u zien hoeveel computers met verzamelingslidmaatschappen gedetecteerd. U ziet ook het aantal verzamelingen die zijn geïmporteerd.

![Computergroepen - SCCM-tabblad](./media/log-analytics-sccm/sccm-computer-groups02.png)

Wanneer u op een klikt, zoeken wordt geopend, ofwel alle van de geïmporteerde groepen of alle computers die deel uitmaken van elke groep. Met behulp van [zoeken in logboeken](log-analytics-log-searches.md), kun u gedetailleerde analyse van Configuration Manager-gegevens.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoeken in logboeken](log-analytics-log-searches.md) om gedetailleerde informatie over uw Configuration Manager-gegevens weer te geven.
