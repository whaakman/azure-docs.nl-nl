---
title: Rapporten voor Azure Backup configureren
description: In dit artikel wordt gesproken over het configureren van Power BI-rapporten voor Azure Backup aan de hand van de Recovery Services-kluis.
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 86e465f1-8996-4a40-b582-ccf75c58ab87
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/10/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40433df5ebe90aec3a9294f2c5a6083c4567b161
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="configure-azure-backup-reports"></a>Azure Backup-rapporten configureren
In dit artikel wordt gesproken over de stappen voor het configureren van rapporten voor Azure Backup aan de hand van de Recovery Services-kluis en toegang tot deze rapporten via Power BI. Na deze stappen uitvoert, kunt u direct naar Power BI om alle rapporten weer te geven, aanpassen en rapporten maken gaan. 

## <a name="supported-scenarios"></a>Ondersteunde scenario's
1. Azure Backup-rapporten worden ondersteund voor virtuele machine van Azure back-up- en back-up van bestanden en mappen naar de cloud met behulp van de Azure Recovery Services Agent.
2. Rapporten voor Azure SQL, DPM en Azure Backup-Server worden niet ondersteund op dit moment.
3. U kunt rapporten weergeven tussen kluizen en abonnementen, als hetzelfde opslagaccount is geconfigureerd voor elk van de kluizen. Geselecteerde opslagaccount moet in dezelfde regio bevinden als de recovery services-kluis.
4. De frequentie van geplande vernieuwing voor de rapporten is 24 uur in Power BI. U kunt ook een ad-hoc vernieuwing van de rapporten in Power BI, waarin de meest recente gegevens van de case in klant storage-account wordt gebruikt voor het weergeven van rapporten uitvoeren. 
5. Azure Backup-rapporten worden momenteel niet ondersteund in nationale clouds.

## <a name="prerequisites"></a>Vereisten
1. Maak een [Azure storage-account](../storage/common/storage-create-storage-account.md#create-a-storage-account) deze wilt configureren voor rapporten. Dit opslagaccount wordt gebruikt voor het opslaan van de bijbehorende gegevens van rapporten.
2. [Een Power BI-account maken](https://powerbi.microsoft.com/landing/signin/) wilt weergeven, aanpassen en uw eigen rapporten maken met Power BI-portal.
3. De registerbronprovider is **Microsoft.insights** als niet al is geregistreerd, met het abonnement van de storage-account en met het abonnement van de Recovery Services-kluis om in te schakelen reporting gegevens moeten worden overgebracht naar de opslag account. Hiertoe dezelfde, gaat u naar de Azure portal > abonnement > resourceproviders en controleer of deze provider te registreren. 

## <a name="configure-storage-account-for-reports"></a>Opslagaccount voor rapporten configureren
Gebruik de volgende stappen voor het configureren van het opslagaccount voor de recovery services-kluis met Azure portal. Dit is een eenmalige configuratie en zodra storage-account is geconfigureerd, gaat u naar Power BI rechtstreeks naar het inhoudspakket weergeven en rapporten gebruikmaken.
1. Als u al een Recovery Services-kluis opent, kunt u doorgaan naar volgende stap. Als er nog geen Recovery Services-kluis is geopend, maar Azure Portal wordt weergeven, klikt u in het menu Hub op **Bladeren**.

   * Typ in de lijst met resources **Recovery Services**.
   * Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer **Recovery Services-kluizen** wordt weergegeven, klikt u erop.

      ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     De lijst met Recovery Services-kluizen wordt weergegeven. Selecteer in de lijst met Recovery Services-kluizen een kluis.

     Het geselecteerde kluisdashboard wordt geopend.
2. In de lijst met items die onder de kluis wordt weergegeven, klikt u op **back-up rapporten** onder de sectie bewaking en rapporten voor het configureren van het opslagaccount voor rapporten.

      ![Selecteer back-up rapporten menu-item stap 2](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. Klik op de blade back-up rapporten **diagnostische instellingen** koppeling. Hiermee opent u de diagnostische instellingen gebruikersinterface die wordt gebruikt voor het opslagaccount van de klant-gegevens worden gepusht.

      ![Stap 3 van diagnostische gegevens inschakelen](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. Klik op de koppeling **diagnostische gegevens inschakelen**. Hiermee opent u de gebruikersinterface voor het configureren van de storage-account. 

      ![Diagnostische gegevens stap 4 inschakelen](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. Geef de naam van de instelling in het veld **naam** en selecteer **archiveren naar een Opslagaccount** inschakelen, zodat reporting gegevens kunt beginnen met het stromende naar het opslagaccount.

      ![Schakel diagnostische gegevens stap 5](./media/backup-azure-configure-reports/select-setting-name.png)
6. Objectkiezer Storage-Account op en selecteer de relevante abonnement en storage-account in de lijst voor het opslaan van gegevens en klik op **OK**.

      ![Selecteer de storage-account-stap 6](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. Selecteer **AzureBackupReport** selectievakje onder de sectie logboek en verplaats de schuifregelaar naar select bewaarperiode voor dit rapportgegevens. Rapportgegevens in het opslagaccount wordt bewaard gedurende de periode die met deze schuifregelaar geselecteerd.

      ![Storage-account stap 7 opslaan](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. Controleer alle wijzigingen en klik op **opslaan** knop bovenaan, zoals wordt weergegeven in de bovenstaande afbeelding. Deze actie zorgt ervoor dat al uw wijzigingen zijn opgeslagen en storage-account is nu geconfigureerd voor het opslaan van rapportgegevens.

9. De tabel diagnostische instellingen worden nu weergegeven voor de nieuwe instelling is ingeschakeld voor de kluis. Als het niet wordt weergegeven, vernieuwt u de tabel om te zien van de bijgewerkte instelling.

      ![Diagnostische instelling stap 9 weergeven](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> Wanneer u rapporten door op te slaan storage-account configureert, moet u **24 uur wachten** voor initiële gegevens-push te voltooien. Azure Backup in Power BI-inhoudspakket importeert u alleen na dit tijdstip. Raadpleeg [sectie Veelgestelde vragen over](#frequently-asked-questions) voor meer informatie. 
>
>

## <a name="view-reports-in-power-bi"></a>Rapporten weergeven in Power BI 
Nadat het opslagaccount voor het configureren van rapporten met behulp van de recovery services-kluis, duurt het ongeveer 24 uur voor het melden van gegevens stromende om in te starten. Gebruik de volgende stappen om rapporten te bekijken in Power BI na 24 uur voor het instellen van de storage-account:
1. [Meld u aan](https://powerbi.microsoft.com/landing/signin/) naar Power BI.
2. Klik op **gegevens ophalen** en klik op Get onder **Services** in de Inhoudsbibliotheek Pack. Gebruik de stappen die worden vermeld in [Power BI-documentatie voor toegang tot inhoudspakket](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-packs-services/).

     ![Het inhoudspakket importeren](./media/backup-azure-configure-reports/content-pack-import.png)
3. Type **Azure Backup** in de zoekbalk en op **nu downloaden**.

      ![Inhoudspakket ophalen](./media/backup-azure-configure-reports/content-pack-get.png)
4. Voer de naam van de opslag in stap 5 hierboven hebt geconfigureerd en klik op **volgende** knop.

    ![Voer opslagaccountnaam in](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Voer de opslagaccountsleutel voor dit opslagaccount. U kunt [bekijken en kopiëren van de toegangssleutels voor opslag](../storage/common/storage-create-storage-account.md#manage-your-storage-account) door te navigeren naar uw storage-account in Azure-portal. 

     ![Voer het opslagaccount](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Klik op **aanmelden** knop. Nadat de aanmelding is geslaagd, u krijgt **importeren van gegevens** melding.

    ![Inhoudspakket importeren](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    Na enige tijd krijgt u **geslaagd** melding nadat het importeren voltooid is. Het kan importeren van het inhoudspakket iets langer duren als er een grote hoeveelheid gegevens in het opslagaccount.
    
    ![Geslaagd-inhoudspakket importeren](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. Wanneer u gegevens importeert **Azure Backup** inhoudspakket is zichtbaar in **Apps** in het navigatiedeelvenster. De lijst bevat nu Azure Backup-dashboard en rapporten, gegevensset met een gele ster zojuist geïmporteerde rapporten die aangeeft. 

     ![Azure Backup-inhoudspakket](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Klik op **Azure Backup** onder Dashboards, waarmee een reeks vastgezette belangrijkste rapporten wordt weergegeven.

      ![Azure Backup-dashboard](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. De volledige set van rapporten, klikt u op elk rapport in het dashboard.

      ![Status van de taak Azure back-up](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Klik op elk tabblad in de rapporten rapporten weer te geven in het desbetreffende gebied.

      ![Tabbladen met Azure Backup-rapporten](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Veelgestelde vragen
1. **Hoe kan ik controleren als rapportagegegevens is gestart, waarbij de storage-account?**
    
    U kunt gaat u naar het opslagaccount dat is geconfigureerd en containers selecteren. Als de container een vermelding voor insights-logboeken-azurebackupreport heeft, betekent dit dat rapportagegegevens is gestart stromende.

2. **Wat is de frequentie van de gegevens-push naar opslagaccount en de Azure Backup-inhoudspakket in Power BI?**

   Voor gebruikers Day 0, zou het ongeveer 24 uur duren om gegevens te pushen naar storage-account. Zodra deze initiële push compelete is, kunnen gegevens worden vernieuwd met de volgende frequentie weergegeven in de onderstaande afbeelding. 
      * Gegevens hebben betrekking op **taken, waarschuwingen, back-Upitems, kluizen, beveiligde Servers en -beleid** wordt doorgegeven voor de klant storage-account als en wanneer deze fout wordt vastgelegd.
      * Gegevens hebben betrekking op **opslag** geduwd naar klant storage-account om de 24 uur.
   
    ![Azure Backup-rapporten gegevens push frequentie](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  Power BI heeft een [geplande vernieuwing eenmaal per dag](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). U kunt handmatig vernieuwen van de gegevens in Power BI uitvoeren voor het inhoudspakket.

3. **Hoe lang kan ik de rapporten behouden?** 

   Tijdens het configureren van de storage-account kunt u de bewaarperiode van rapportagegegevens selecteren in de storage-account (met stap 6 in de storage-account configureren voor rapporten sectie hierboven). Behalve dat u kunt [rapporten analyseren in excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) en deze wilt opslaan voor een langere bewaartermijn volgens uw behoeften. 

4. **Ziet ik mijn gegevens in rapporten na het configureren van het storage-account?**

   Alle gegevens die zijn gegenereerd na **'storage-account configureren'** wordt gepusht naar het opslagaccount en in rapporten beschikbaar zijn. Echter, **In voortgang taken niet worden gepusht** voor rapportage. Zodra de taak is voltooid of mislukt, wordt het verzonden tot rapporten.

5. **Als ik de storage-account voor de weergave rapporten al hebt geconfigureerd, kan ik de configuratie voor het gebruik van een ander opslagaccount wijzigen?** 

   Ja, kunt u de configuratie om te verwijzen naar een ander opslagaccount. U moet het meest recent geconfigureerde storage-account gebruiken bij het verbinden met Azure Backup-inhoudspakket. Zodra een ander opslagaccount is geconfigureerd, zou ook nieuwe gegevens in dit opslagaccount stromen. Maar nog steeds in het oudere opslagaccount met oudere gegevens (vóór het wijzigen van de configuratie).

6. **Kan ik rapporten bekijken tussen kluizen en abonnementen?** 

   Ja, kunt u hetzelfde opslagaccount configureren op verschillende kluizen cross-kluis rapporten weer te geven. U kunt ook hetzelfde opslagaccount voor kluizen configureren voor abonnementen. U kunt dit opslagaccount vervolgens bij het verbinden met Azure Backup in Power BI-inhoudspakket gebruiken om de rapporten weer te geven. Het geselecteerde opslagaccount moet echter in dezelfde regio bevinden als de recovery services-kluis.
   
## <a name="troubleshooting-errors"></a>Het oplossen van problemen
| Details van fouten | Oplossing |
| --- | --- |
| Na het instellen van het opslagaccount voor back-up-rapporten **Opslagaccount** nog steeds **niet geconfigureerd**. | Als u de storage-account is geconfigureerd, worden uw rapportagegegevens overgebracht ondanks dit probleem. U lost dit probleem, gaat u naar Azure portal > meer Services > diagnostische instellingen > RS kluis > instelling bewerken. De eerder geconfigureerde instelling verwijderen en een nieuwe instelling maken op basis van de dezelfde blade. Deze tijd stelt u het veld **naam** naar **service**. Dit moet het geconfigureerde storage-account worden weergegeven. |
|Inhoud pack in Power BI, de fout na het importeren van Azure Backup **404-container is niet gevonden** wordt weergegeven. | Zoals voorgesteld in dit document, moet u 24 uur na het configureren van rapporten in de Recovery Services-kluis voor een goed overzicht in Power BI wachten. Als u probeert te krijgen tot de rapporten voordat 24 uur, krijgt u deze fout omdat voltooid is nog niet aanwezig is om geldige rapporten weer te geven. |

## <a name="next-steps"></a>Volgende stappen
Nu u de storage-account en de geïmporteerde inhoudspakket van Azure Backup hebt geconfigureerd, de volgende stap is reporting gegevensmodel gebruiken om rapporten te maken en aanpassen van deze rapporten. Raadpleeg de volgende artikelen voor meer informatie.

* [Met behulp van Azure Backup-gegevensmodel rapportage](backup-azure-reports-data-model.md)
* [Filteren van rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Maken van rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

