---
title: Rapporten voor Azure Backup configureren
description: Power BI-rapporten voor Azure Backup configureren met behulp van een Recovery Services-kluis.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: adigan
ms.openlocfilehash: 945d8690ccb61916455ba4137f59c67c9a3d9f76
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237685"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup-rapporten configureren
In dit artikel ziet u stappen te volgen voor het configureren van rapporten voor Azure Backup met behulp van een Recovery Services-kluis. U ziet ook hoe u toegang tot rapporten met behulp van Power BI. Nadat u deze stappen hebt voltooid, gaat u rechtstreeks naar Power BI om te bekijken, aanpassen en rapporten maken.

> [!IMPORTANT]
> vanaf 1 november 2018 ervaren sommige klanten mogelijk problemen bij het laden van gegevens in de Azure Backup-app in Power BI. In de foutmelding staat 'Er zijn extra tekens aangetroffen aan het eind van de JSON-invoer. De uitzondering is opgetreden door de IDataReader-interface.'
Dit probleem treedt op vanwege een wijziging in de indeling waarin gegevens in de opslagaccount worden geladen.
Download de meest recente App (versie 1.8) om dit probleem te voorkomen.
>
>

## <a name="supported-scenarios"></a>Ondersteunde scenario's
- Azure Backup-rapporten worden ondersteund voor virtuele machine van Azure back-up en bestands- en back-up naar de cloud met behulp van de Azure Recovery Services-Agent.
- Rapporten voor Azure SQL Database, Azure-bestandsshares, Data Protection Manager en Azure Backup server worden niet ondersteund op dit moment.
- U kunt rapporten weergeven in kluizen en abonnementen, als het opslagaccount is geconfigureerd voor elk van de kluizen. Het geselecteerde opslagaccount moet zich in dezelfde regio als de Recovery Services-kluis.
- De frequentie van geplande vernieuwing voor de rapporten is 24 uur in Power BI. U kunt ook een ad-hoc-vernieuwing van de rapporten in Power BI uitvoeren. In dit geval wordt de meest recente gegevens in het opslagaccount van de klant gebruikt om rapporten weer te geven.

## <a name="prerequisites"></a>Vereisten
- Maak een [Azure storage-account](../storage/common/storage-quickstart-create-account.md) te configureren voor rapporten. Dit opslagaccount wordt gebruikt voor het opslaan van gegevens met betrekking tot de rapporten.
- [Een Power BI-account maken](https://powerbi.microsoft.com/landing/signin/) als u wilt weergeven, aanpassen en uw eigen rapporten maken met behulp van de Power BI-portal.
- Registreer de resourceprovider **Microsoft.insights**, als deze niet al geregistreerd. Gebruik de abonnementen voor het opslagaccount en de Recovery Services-kluis zodat rapportagegegevens met de storage-account kan stromen. Als u wilt deze stap uitvoert, gaat u naar de Azure portal, selecteer **abonnement** > **resourceproviders**, en controleer of deze provider te registreren.

## <a name="configure-storage-account-for-reports"></a>Opslagaccount voor rapporten configureren
Volg deze stappen voor het opslagaccount voor een Recovery Services-kluis configureren met behulp van de Azure-portal. Dit is een eenmalige configuratie. Nadat het opslagaccount dat is geconfigureerd, gaat u rechtstreeks naar Power BI het inhoudspakket weergeven en gebruiken van rapporten.

1. Als u al een Recovery Services-kluis openen, gaat u naar de volgende stap. Als u geen Recovery Services-kluis openen, klikt u in Azure portal, selecteer **alle services**.

   * Voer in de lijst met resources **herstelservices**.
   * Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer de melding **Recovery Services-kluizen**, selecteert u deze.

      ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

   * De lijst met Recovery Services-kluizen wordt weergegeven. Selecteer in de lijst met Recovery Services-kluizen een kluis.

     Het geselecteerde kluisdashboard wordt geopend.
2. In de lijst met items dat wordt weergegeven op de kluis onder de **bewaking en rapporten** sectie, selecteer **Backup-rapporten** het configureren van het opslagaccount voor rapporten.

      ![Selecteer Backup-rapporten menu-item stap 2](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. Op de **Backup-rapporten** blade, selecteer de **diagnostische instellingen** koppeling. Deze koppeling wordt geopend de **diagnostische instellingen** -gebruikersinterface die wordt gebruikt om gegevens te pushen naar een opslagaccount van de klant.

      ![Schakel diagnostische gegevens stap 3](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. Selecteer **diagnostische gegevens inschakelen** openen van een gebruikersinterface te gebruiken voor het configureren van een storage-account.

      ![Schakel diagnostische gegevens van stap 4](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. In de **naam** voert u de Instellingsnaam van een. Selecteer de **archiveren naar een opslagaccount** selectievakje zodat reporting gegevens kunt beginnen met het doorgestuurd naar het opslagaccount.

      ![Schakel diagnostische gegevens stap 5](./media/backup-azure-configure-reports/select-setting-name.png)
6. Selecteer **opslagaccount**, selecteer het gewenste abonnement en het opslagaccount in de lijst voor het opslaan van de rapportagegegevens en selecteer **OK**.

      ![Selecteer de storage-account-stap 6](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. Onder de **Log** sectie, selecteer de **AzureBackupReport** selectievakje. Verplaats de schuifregelaar om te selecteren voor deze reporting gegevens een retentieperiode. Rapportgegevens in het opslagaccount dat wordt opgeslagen voor de periode die zijn geselecteerd met behulp van deze schuifregelaar.

      ![Storage-account-stap 7 opslaan](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. Bekijk alle wijzigingen en selecteer **opslaan**. Deze actie zorgt ervoor dat alle wijzigingen worden opgeslagen en het storage-account is nu geconfigureerd om op te slaan van rapportagegegevens.

9. De **diagnostische instellingen** tabel ziet u nu de nieuwe instelling is ingeschakeld voor de kluis. Als deze niet wordt weergegeven, vernieuwt u de tabel om te zien van de instelling van de bijgewerkte.

      ![Diagnostische instelling stap 9 weergeven](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> Na het configureren van rapporten door op te slaan van het opslagaccount *24 uur wachten* voor de initiële gegevens-push te voltooien. Importeer de Azure-back-up in Power BI-App alleen na deze periode. Zie voor meer informatie de [gedeelte met veelgestelde vragen](#frequently-asked-questions).
>
>

## <a name="view-reports-in-power-bi"></a>Rapporten weergeven in Power BI
Nadat u een opslagaccount voor rapporten met behulp van een Recovery Services-kluis hebt geconfigureerd, duurt het ongeveer 24 uur voor rapportagegegevens te starten stromen. Volg deze stappen voor het weergeven van rapporten in Power BI na 24 uur van het instellen van een storage-account.
Als u wilt aanpassen en delen van het rapport, een werkruimte maken en voer de volgende stappen uit

1. [Meld u aan](https://powerbi.microsoft.com/landing/signin/) naar Power BI.
2. Selecteer **Gegevens ophalen**. In de **meer manieren om te maken van uw eigen inhoud**, selecteer **Service inhoudspakketten**. Volg de stappen in de [Power BI-documentatie verbinding maken met een service](https://powerbi.microsoft.com/documentation/powerbi-content-packs-services/).

3. In de **zoeken** balk, voer **Azure Backup** en selecteer **nu downloaden**.

      ![Inhoudspakket ophalen](./media/backup-azure-configure-reports/content-pack-get.png)
4. Voer de naam van het opslagaccount dat is geconfigureerd in de vorige stap 5 en selecteer **volgende**.

    ![De opslagaccountnaam invoeren](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Met behulp van verificatiemethode 'Key', voer de opslagaccountsleutel voor dit opslagaccount. Naar [bekijken en kopiëren van de toegangssleutels voor opslag](../storage/common/storage-account-manage.md#access-keys), gaat u naar uw opslagaccount in Azure portal.

     ![Storage-account invoeren](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>

6. Selecteer **aanmelden**. Nadat de aanmelding is geslaagd, ziet u een melding met importeren gegevens.

    ![Inhoudspakket importeren](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>

    Nadat het importeren is voltooid, ziet u een **succes** melding. Als de hoeveelheid gegevens in het opslagaccount dat groot is, is het duurt iets langer voor het importeren van het inhoudspakket.

    ![Importeren geslaagd-inhoudspakket](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>

7. Nadat gegevens zijn geïmporteerd, de **Azure Backup** inhoudspakket is zichtbaar in **Apps** in het navigatiedeelvenster. Onder **Dashboards**, **rapporten**, en **gegevenssets**, in de lijst staan nu back-up van Azure.

8. Onder **Dashboards**, selecteer **Azure Backup**, waarin een aantal belangrijke rapporten vastgemaakt.

      ![Azure Backup-dashboard](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. Als u de volledige set met rapporten, selecteert u een rapport in het dashboard.

      ![Status van Azure back-up-taak](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Elk tabblad in de rapporten om rapporten weer in het desbetreffende gebied te selecteren.

      ![Azure Backup-rapporten tabbladen](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-storage-account"></a>Hoe kan ik als rapportagegegevens is gestart doorgestuurd naar een opslagaccount controleren?
Ga naar het opslagaccount dat u hebt geconfigureerd, en selecteer containers. Als de container een vermelding voor insights-logs-azurebackupreport heeft, betekent dit dat rapportagegegevens is gestart stromen.

### <a name="what-is-the-frequency-of-data-push-to-a-storage-account-and-the-azure-backup-content-pack-in-power-bi"></a>Wat is de frequentie van de gegevens-push naar een opslagaccount en de Azure Backup-inhoudspakket in Power BI?
  Het duurt ongeveer 24 uur om gegevens te pushen naar een opslagaccount voor dag 0-gebruikers. Nadat deze initiële push is voltooid, worden gegevens worden vernieuwd met de frequentie wordt weergegeven in de volgende afbeelding.

  * Gegevens met betrekking tot **taken**, **waarschuwingen**, **back-Upitems**, **kluizen**, **beschermde Servers**, en  **Beleid** naar een opslagaccount van de klant wordt gepusht, en wanneer ze wordt vastgelegd.

  * Gegevens met betrekking tot **opslag** wordt gepusht naar een opslagaccount van de klant om de 24 uur.

       ![Azure Backup-rapporten gegevens pushen frequentie](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  * Power BI beschikt over een [geplande vernieuwing eenmaal per dag](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). U kunt handmatig vernieuwen van de gegevens in Power BI uitvoeren voor het inhoudspakket.

### <a name="how-long-can-i-retain-reports"></a>Hoe lang kan ik rapporten behouden?
Wanneer u een storage-account configureert, kunt u een bewaarperiode voor gegevens in de storage-account. Voer stap 6 in de [Opslagaccount voor rapporten](backup-azure-configure-reports.md#configure-storage-account-for-reports) sectie. U kunt ook [rapporten in Excel analyseren](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) en deze wilt opslaan voor een langere bewaarperiode, op basis van uw behoeften.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-storage-account"></a>Zie ik mijn gegevens in rapporten nadat ik de storage-account configureren?
 Alle gegevens die zijn gegenereerd na het configureren van een storage-account wordt doorgestuurd naar het opslagaccount en is beschikbaar in rapporten. Taken in uitvoering worden niet gepusht voor rapportage. Nadat de taak is voltooid of mislukt, wordt deze verzonden naar rapporten.

### <a name="if-i-already-configured-the-storage-account-to-view-reports-can-i-change-the-configuration-to-use-another-storage-account"></a>Als ik de storage-account om rapporten weer te geven al hebt geconfigureerd, kan ik de configuratie voor het gebruik van een ander opslagaccount wijzigen?
Ja, kunt u de configuratie te verwijzen naar een ander opslagaccount. Gebruik de meest recent geconfigureerde storage-account terwijl u verbinding met de Azure Backup-inhoudspakket maken. Nadat een ander opslagaccount is geconfigureerd, loopt nieuwe gegevens ook in dit opslagaccount. Oudere gegevens (voordat u de configuratie wijzigt) wordt nog steeds in de oudere storage-account.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Kan ik rapporten bekijken in kluizen en abonnementen?
Ja, kunt u hetzelfde opslagaccount in verschillende kluizen om cross-kluis rapporten weer te geven. U kunt ook hetzelfde opslagaccount voor kluizen configureren voor abonnementen. Vervolgens kunt u dit storage-account terwijl u verbinding maken met de Azure Backup-inhoudspakket in Power BI om de rapporten weer te geven. Het geselecteerde opslagaccount moet zich in dezelfde regio als de Recovery Services-kluis.

## <a name="troubleshooting-errors"></a>Fouten oplossen
| Foutdetails | Oplossing |
| --- | --- |
| Na het instellen van het opslagaccount voor Backup-rapporten, **opslagaccount** toont nog steeds **niet geconfigureerd**. | Als u een storage-account is geconfigureerd, loopt uw rapportgegevens ondanks dit probleem. U lost dit probleem, gaat u naar de Azure portal en selecteer **alle services** > **diagnostische instellingen** > **Recovery Services-kluis**  >  **Instelling bewerken**. De eerder geconfigureerde instelling verwijderen en een nieuwe instelling op de dezelfde blade maken. Deze keer in de **naam** Schakel **service**. Het opslagaccount wordt nu weergegeven. |
|Na het importeren van de Azure Backup-inhoudspakket in Power BI is een fout '404-container is niet gevonden' bericht wordt weergegeven. | Zoals eerder vermeld, moet u wachten 24 uur na het configureren van rapporten in de Recovery Services-kluis correct weergegeven in Power BI. Als u probeert te krijgen tot de rapporten voor 24 uur, wordt dit foutbericht wordt weergegeven, omdat de volledige gegevens is nog niet aanwezig zijn om geldige rapporten weer te geven. |

## <a name="next-steps"></a>Volgende stappen
Nadat u de storage-account configureert en importeren van de Azure Backup-inhoudspakket, wordt de volgende stappen zijn rapporten aanpassen en een gegevensmodel reporting gebruiken om rapporten te maken. Zie de volgende artikelen voor meer informatie.

* [Een Azure-Backup reporting gegevensmodel gebruiken](backup-azure-reports-data-model.md)
* [Filterrapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Rapporten maken in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
