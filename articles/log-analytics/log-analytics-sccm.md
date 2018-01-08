---
title: Maak verbinding met Configuration Manager met Log Analytics | Microsoft Docs
description: In dit artikel bevat de stappen voor het verbinden van Configuration Manager met logboekanalyse en analyseren van gegevens te starten.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: banders
ms.openlocfilehash: 7acf0cbd4f4cba885e6cc91dfe3cb68306a3649a
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Maak verbinding met Configuration Manager met Log Analytics
U kunt System Center Configuration Manager verbinden met logboekanalyse in OMS sync verzameling apparaatgegevens. Dit zijn de gegevens van uw Configuration Manager-hiërarchie beschikbaar in OMS.

## <a name="prerequisites"></a>Vereisten

Log Analytics biedt ondersteuning voor System Center Configuration Manager current branch versie 1606 of hoger.  

## <a name="configuration-overview"></a>Configuratie-overzicht
De volgende stappen geven een overzicht van het proces voor Configuration Manager verbinding met logboekanalyse.  

1. Configuration Manager registreert als een webtoepassing en/of Web-API-app in de Azure-portal en zorg ervoor dat u de client-ID en een geheime sleutel van de client van de registratie van Azure Active Directory. Zie [portal gebruik maken van Active Directory-toepassing en service-principal die toegang bronnen tot](../azure-resource-manager/resource-group-create-service-principal-portal.md) voor gedetailleerde informatie over het uitvoeren van deze stap.
2. In de Azure portal [Geef de Configuration Manager (de geregistreerde web-app) met machtigingen voor toegang tot OMS](#provide-configuration-manager-with-permissions-to-oms).
3. In Configuration Manager [toevoegen van een verbinding met de Wizard OMS-verbinding toevoegen](#add-an-oms-connection-to-configuration-manager).
4. In Configuration Manager [werk de verbindingseigenschappen](#update-oms-connection-properties) als de geheime sleutel met het wachtwoord of client ooit is verlopen of verloren gegaan is.
5. Met informatie uit de OMS-portal [downloaden en installeren van Microsoft Monitoring Agent](#download-and-install-the-agent) op de computer waarop de Configuration Manager service connection point sitesysteemrol. De agent verzendt gegevens van Configuration Manager met OMS.
6. In Log Analytics [verzamelingen importeren uit Configuration Manager](#import-collections) als computergroepen.
7. Weergeven in Log Analytics gegevens uit Configuration Manager als [computergroepen](log-analytics-computer-groups.md).

Meer informatie over het aansluiten van Configuration Manager met OMS op [synchroniseren van gegevens uit Configuration Manager met de Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Geef de Configuration Manager met OMS met machtigingen
De volgende procedure bevat de Azure-portal met machtigingen voor toegang tot OMS. U moet in het bijzonder verlenen de *rol van Inzender* voor gebruikers in de resourcegroep om de Azure-portal voor het verbinden van Configuration Manager met OMS.

> [!NOTE]
> U moet machtigingen opgeven in OMS voor Configuration Manager. Anders wordt een foutbericht weergegeven wanneer u de configuratiewizard in Configuration Manager gebruiken.
>
>

1. Open de [Azure-portal](https://portal.azure.com/) en klik op **Bladeren** > **logboekanalyse (OMS)** logboekanalyse (OMS) openen.  
2. Op **logboekanalyse (OMS)**, klikt u op **toevoegen** openen **OMS-werkruimte**.  
   ![OMS](./media/log-analytics-sccm/sccm-azure01.png)
3. Op **OMS-werkruimte**, geef de volgende informatie en klik vervolgens op **OK**.

   * **OMS-werkruimte**
   * **Abonnement**
   * **Resourcegroep**
   * **Locatie**
   * **Prijscategorie**  
     ![OMS](./media/log-analytics-sccm/sccm-azure02.png)  

     > [!NOTE]
     > Het bovenstaande voorbeeld maakt een nieuwe resourcegroep. De resourcegroep wordt alleen gebruikt om te voorzien van Configuration Manager worden machtigingen voor de OMS-werkruimte in dit voorbeeld.
     >
     >
4. Klik op **Bladeren** > **resourcegroepen** openen **resourcegroepen**.
5. In **resourcegroepen**, klikt u op de resourcegroep die u hierboven hebt gemaakt om te openen de &lt;Resourcegroepnaam&gt; instellingen.  
   ![groep broninstellingen](./media/log-analytics-sccm/sccm-azure03.png)
6. In &lt;Resourcegroepnaam&gt; instellingen, klikt u op toegangsbeheer (IAM) openen &lt;Resourcegroepnaam&gt; gebruikers.  
   ![resourcegroep gebruikers](./media/log-analytics-sccm/sccm-azure04.png)  
7. In &lt;Resourcegroepnaam&gt; , klikt u op **toevoegen** openen **toegang toevoegen**.
8. In **toegang toevoegen**, klikt u op **Selecteer een rol**, en selecteer vervolgens de **Inzender** rol.  
   ![Selecteer een rol](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klik op **gebruikers toevoegen**, selecteert u de Configuration Manager-gebruiker, klik op **Selecteer**, en klik vervolgens op **OK**.  
   ![gebruikers toevoegen](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Een OMS-verbinding toevoegen aan Configuration Manager
Als u wilt een OMS-verbinding toevoegen, uw Configuration Manager-omgeving moet hebben een [serviceaansluitpunt](https://technet.microsoft.com/library/mt627781.aspx) geconfigureerd voor de onlinemodus bevindt.

1. In de **beheer** werkruimte van Configuration Manager, selecteer **OMS Connector**. Hiermee opent u de **OMS verbinding Wizard toevoegen**. Selecteer **volgende**.
2. Op de **algemene** scherm, bevestig dat u de volgende acties hebt gedaan en dat u hebt de details voor elk item, en selecteer **volgende**.

   1. In de Azure portal hebt u Configuration Manager geregistreerd als een webtoepassing en/of Web-API-app en die u hebt de [client-ID van de registratie](../active-directory/active-directory-integrating-applications.md).
   2. U kunt een geheime sleutel van de app voor de geregistreerde app in Azure Active Directory hebt gemaakt in de Azure portal.  
   3. In de Azure portal kunt u de geregistreerde web-app hebt opgegeven met de machtiging voor toegang tot OMS.  
      ![Verbinding met OMS Wizard algemene pagina](./media/log-analytics-sccm/sccm-console-general01.png)
3. Op de **Azure Active Directory** scherm, instellingen voor de verbinding met OMS configureren door te geven uw **Tenant**, **Client-ID**, en **Client geheime sleutel** , selecteer daarna **volgende**.  
   ![Verbinding met OMS Wizard Azure Active Directory-pagina](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Als u bereikt de procedures geslaagd, klikt u vervolgens de informatie op de **OMS verbindingsconfiguratie** scherm automatisch wordt weergegeven op deze pagina. Informatie over de verbindingsinstellingen moet worden weergegeven voor uw **Azure-abonnement**, **Azure-resourcegroep**, en **Operations Management Suite-werkruimte**.  
   ![Verbinding met de pagina OMS Wizard OMS-verbinding](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. De wizard maakt verbinding met de OMS-service met behulp van de informatie die u hebt ingevoerd. Selecteer de apparaatverzamelingen die u wilt synchroniseren met OMS en klik vervolgens op **toevoegen**.  
   ![Selecteer verzamelingen](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Controleer de verbindingsinstellingen op de **samenvatting** scherm en selecteer vervolgens **volgende**. De **voortgang** scherm toont de verbindingsstatus en vervolgens moet **Complete**.

> [!NOTE]
> U moet OMS verbinden met de hoogste site in uw hiërarchie. Als u verbinding maken met OMS een zelfstandige primaire site en vervolgens een centrale beheersite toe te aan uw omgeving voegen, die u moet verwijderen en opnieuw maken van de OMS-verbinding in de nieuwe hiërarchie.
>
>

Nadat u hebt de Configuration Manager met OMS gekoppeld, kunt u toevoegen of verwijderen van verzamelingen en bekijk de eigenschappen van de OMS-verbinding.

## <a name="update-oms-connection-properties"></a>De eigenschappen van de OMS-verbinding bijwerken
Als een geheime sleutel met het wachtwoord of de client nooit verloopt of verloren, moet u de verbindingseigenschappen OMS handmatig bijwerken.

1. In Configuration Manager, gaat u naar **Cloudservices**, selecteer daarna **OMS-Connector** openen de **OMS verbindingseigenschappen** pagina.
2. Op deze pagina, klikt u op de **Azure Active Directory** tabblad om uw **Tenant**, **Client-ID**, **Client geheime sleutelverloop**. **Controleer of** uw **geheime sleutel van de Client** als deze is verlopen.

## <a name="download-and-install-the-agent"></a>Download en installeer de agent
1. In de OMS-portal [downloaden van het installatiebestand van de agent van OMS](log-analytics-windows-agent.md).
2. Gebruik een van de volgende methoden voor het installeren en configureren van de agent op de computer met de Configuration Manager service connection point sitesysteemrol:
   * [Installeer de agent via setup](log-analytics-windows-agent.md)
   * [Installeer de agent via de opdrachtregel](log-analytics-windows-agent.md)
   * [De agent installeert met DSC in Azure Automation](log-analytics-windows-agent.md)

## <a name="import-collections"></a>Verzamelingen importeren
Nadat u hebt een OMS-verbinding naar Configuration Manager toegevoegd en de agent hebt geïnstalleerd op de computer met de verbinding met de Configuration Manager sitesysteemrol, wordt de volgende stap verzamelingen importeren uit Configuration Manager in OMS als computergroepen.

Nadat invoer is ingeschakeld, wordt de lidmaatschapsgegevens van verzameling elke drie uur zodat de verzamelinglidmaatschappen huidige opgehaald. U kunt importeren op elk moment uitschakelen.

1. Klik in de OMS-portal op **instellingen**.
2. Klik op de **computergroepen** tabblad en klik vervolgens op de **SCCM** tabblad.
3. Selecteer **Import Configuration Manager-verzamelinglidmaatschappen** en klik vervolgens op **opslaan**.  
   ![Computergroepen - SCCM-tabblad](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Gegevens weergeven van Configuration Manager
Nadat u hebt een OMS-verbinding naar Configuration Manager toegevoegd en de agent op de computer met de Configuration Manager service connection point sitesysteemrol hebt geïnstalleerd, worden gegevens van de agent wordt verzonden met OMS. In de OMS, uw Configuration Manager-verzamelingen worden weergegeven als [computergroepen](log-analytics-computer-groups.md). Vindt u de groepen van de **Configuration Manager** pagina onder **computergroepen** in **instellingen**.

Nadat de verzamelingen zijn geïmporteerd, kunt u zien hoeveel computers met verzamelinglidmaatschappen zijn gedetecteerd. U ziet ook het aantal verzamelingen die zijn geïmporteerd.

![Computergroepen - SCCM-tabblad](./media/log-analytics-sccm/sccm-computer-groups02.png)

Als u op een, zoekopdracht wordt geopend ofwel alle van de geïmporteerde groepen of alle computers die deel uitmaken van elke groep. Met behulp van [logboek zoeken](log-analytics-log-searches.md), kunt u beginnen met gedetailleerde analyse van Configuration Manager-gegevens.

## <a name="next-steps"></a>Volgende stappen
* Gebruik [logboek zoeken](log-analytics-log-searches.md) om gedetailleerde informatie over uw Configuration Manager-gegevens weer te geven.
