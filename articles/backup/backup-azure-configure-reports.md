---
title: Rapporten voor Azure Backup configureren
description: Power BI-rapporten configureren voor Azure Backup met behulp van de Recovery Services-kluis.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 07/26/2018
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c59653bf3709f7798fd92a44fa420b99f2cbc6b6
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733552"
---
# <a name="configure-azure-backup-reports"></a>Azure Backup-rapporten configureren
In dit artikel wordt stap voor het configureren van rapporten voor Azure Backup met behulp van de Recovery Services-kluis en voor toegang tot deze rapporten met Power BI besproken. Nadat u deze stappen uitvoert, kunt u rechtstreeks naar Power BI om alle rapporten weer te geven, aanpassen en maken van rapporten gaan. 

## <a name="supported-scenarios"></a>Ondersteunde scenario's
1. Azure Backup-rapporten worden ondersteund voor back-up van virtuele Azure-machine en back-up van bestanden/mappen naar de cloud met Azure Recovery Services-Agent.
2. Rapporten voor Azure SQL, DPM en Azure Backup Server worden niet ondersteund op dit moment.
3. U kunt rapporten weergeven tussen kluizen en -abonnementen, als hetzelfde opslagaccount is geconfigureerd voor elk van de kluizen. Opslagaccount geselecteerd moet zich in dezelfde regio als de recovery services-kluis.
4. De frequentie van geplande vernieuwing voor de rapporten is 24 uur in Power BI. U kunt ook een ad-hoc-vernieuwing van de rapporten in Power BI, waarin de aanvraag van de meest recente gegevens in klant storage-account wordt gebruikt voor het weergeven van rapporten uitvoeren. 

## <a name="prerequisites"></a>Vereisten
1. Maak een [Azure storage-account](../storage/common/storage-quickstart-create-account.md) te configureren voor rapporten. Dit opslagaccount wordt gebruikt voor het opslaan van rapporten gerelateerde gegevens.
2. [Een Power BI-account maken](https://powerbi.microsoft.com/landing/signin/) als u wilt weergeven, aanpassen en uw eigen rapporten maken met Power BI-portal.
3. Registreer de resourceprovider **Microsoft.insights** als niet al is geregistreerd, aan het abonnement van storage-account en aan het abonnement van de Recovery Services-kluis om in te schakelen waarvoor gegevens zijn gerapporteerd aan de opslag -account. Als u wilt doen, gaat u naar Azure portal > abonnementen > resourceproviders en -controle voor deze provider te registreren. 

## <a name="configure-storage-account-for-reports"></a>Opslagaccount voor rapporten configureren
Gebruik de volgende stappen uit om te configureren van het opslagaccount voor de recovery services-kluis met behulp van Azure portal. Dit is een eenmalige configuratie en nadat storage-account is geconfigureerd, gaat u naar Power BI rechtstreeks naar het inhoudspakket weergeven en gebruiken van rapporten.
1. Als u al een Recovery Services-kluis openen, gaat u verder met de volgende stap. Als u nog geen een Recovery Services-kluis is geopend, maar in Azure portal worden, klikt u op **alle services**.

   * Typ in de lijst met resources **Recovery Services**.
   * Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer **Recovery Services-kluizen** wordt weergegeven, klikt u erop.

      ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     De lijst met Recovery Services-kluizen wordt weergegeven. Selecteer in de lijst met Recovery Services-kluizen een kluis.

     Het geselecteerde kluisdashboard wordt geopend.
2. Uit de lijst met items die onder de kluis wordt weergegeven, klikt u op **Backup-rapporten** onder de sectie bewaking en rapporten te configureren van het opslagaccount voor rapporten.

      ![Selecteer Backup-rapporten menu-item stap 2](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. Klik op de blade back-up rapporten **diagnostische instellingen** koppeling. Hiermee opent u de diagnostische instellingen gebruikersinterface die wordt gebruikt voor het pushen van gegevens naar de klant storage-account.

      ![Schakel diagnostische gegevens stap 3](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. Klik op de koppeling **diagnostische gegevens inschakelen**. Hiermee opent u de gebruikersinterface voor het configureren van storage-account. 

      ![Schakel diagnostische gegevens van stap 4](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. Voer de naam van instelling in het veld **naam** en selecteer **archiveren naar een Opslagaccount** selectievakje zodat reporting gegevens kunt beginnen met stromen met de storage-account.

      ![Schakel diagnostische gegevens stap 5](./media/backup-azure-configure-reports/select-setting-name.png)
6. Klik op Storage-Account kiezen en selecteer de relevante abonnement en storage-account in de lijst voor het opslaan van rapporten gegevens en klik op **OK**.

      ![Selecteer de storage-account-stap 6](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. Selecteer **AzureBackupReport** selectievakje inschakelen in de sectie en verplaats de schuifregelaar naar Selecteer bewaarperiode voor deze waarvoor gegevens zijn gerapporteerd. Rapportgegevens in het opslagaccount dat wordt opgeslagen voor de periode die is geselecteerd met behulp van deze schuifregelaar.

      ![Storage-account-stap 7 opslaan](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. Controleer alle de wijzigingen en klikt u op **opslaan** knop bovenaan, zoals wordt weergegeven in de bovenstaande afbeelding. Deze actie zorgt ervoor dat alle wijzigingen worden opgeslagen en storage-account is nu geconfigureerd voor het opslaan van de rapportagegegevens.

9. De tabel met diagnostische instellingen worden nu weergegeven voor de nieuwe instelling is ingeschakeld voor de kluis. Als deze niet wordt weergegeven, vernieuwt u de tabel om te zien van de instelling van de bijgewerkte.

      ![Diagnostische instelling stap 9 weergeven](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> Wanneer u rapporten door op te slaan van de storage-account configureert, moet u **24 uur wachten** voor initiële gegevens-push te voltooien. Alleen na die tijd moet u Azure Backup-inhoudspakket in Power BI importeren. Raadpleeg [gedeelte met veelgestelde vragen](#frequently-asked-questions) voor meer informatie. 
>
>

## <a name="view-reports-in-power-bi"></a>Rapporten weergeven in Power BI 
Nadat storage-account configureren met behulp van de recovery services-kluis-rapporten, duurt het ongeveer 24 uur voor het melden van gegevens te starten van stromen in. Gebruik de volgende stappen voor het weergeven van rapporten in Power BI na 24 uur van het instellen van de storage-account:
1. [Meld u aan](https://powerbi.microsoft.com/landing/signin/) naar Power BI.
2. Klik op **gegevens ophalen** en klikt u op **ophalen** onder **Services** in Content Pack-bibliotheek. Gebruik de stappen in [Power BI-documentatie voor toegang tot inhoudspakket](https://powerbi.microsoft.com/documentation/powerbi-content-packs-services/).

     ![Import-inhoudspakket](./media/backup-azure-configure-reports/content-pack-import.png)
3. Type **Azure Backup** in de zoekbalk typt en op **nu downloaden**.

      ![Inhoudspakket ophalen](./media/backup-azure-configure-reports/content-pack-get.png)
4. Voer de naam van het opslagaccount in stap 5 hierboven hebt geconfigureerd en klik op **volgende** knop.

    ![De opslagaccountnaam invoeren](./media/backup-azure-configure-reports/content-pack-storage-account-name.png)    
5. Voer de opslagaccountsleutel voor dit opslagaccount. U kunt [bekijken en kopiëren van de toegangssleutels voor opslag](../storage/common/storage-account-manage.md#access-keys) door te navigeren naar uw opslagaccount in Azure portal. 

     ![Storage-account invoeren](./media/backup-azure-configure-reports/content-pack-storage-account-key.png) <br/>
     
6. Klik op **aanmelden** knop. Nadat de aanmelding is gelukt, krijgt u **importeren van gegevens** melding.

    ![Inhoudspakket importeren](./media/backup-azure-configure-reports/content-pack-importing-data.png) <br/>
    
    Na enige tijd, krijgt u **succes** melding nadat het importeren voltooid is. Het duurt iets langer voor het importeren van het inhoudspakket, als er een grote hoeveelheid gegevens in de storage-account.
    
    ![Importeren geslaagd-inhoudspakket](./media/backup-azure-configure-reports/content-pack-import-success.png) <br/>
    
7. Zodra de gegevens is geïmporteerd, **Azure Backup** inhoudspakket is zichtbaar in **Apps** in het navigatiedeelvenster. De lijst bevat nu back-up van Azure-dashboard, rapporten en gegevensset met een gele ster zojuist geïmporteerde rapporten waarmee wordt aangegeven. 

     ![Azure Backup-inhoudspakket](./media/backup-azure-configure-reports/content-pack-azure-backup.png) <br/>
     
8. Klik op **Azure Backup** onder Dashboards, waarin een set met vastgemaakte belangrijke rapporten.

      ![Azure Backup-dashboard](./media/backup-azure-configure-reports/azure-backup-dashboard.png) <br/>
9. Als u wilt de volledige set met rapporten weergeven, klikt u op een rapport in het dashboard.

      ![Status van Azure back-up-taak](./media/backup-azure-configure-reports/azure-backup-job-health.png) <br/>
10. Klik op elk tabblad in de rapporten om rapporten in het desbetreffende gebied weer te geven.

      ![Tabbladen met Azure Backup-rapporten](./media/backup-azure-configure-reports/reports-tab-view.png)


## <a name="frequently-asked-questions"></a>Veelgestelde vragen
1. **Hoe kan ik controleren als rapportagegegevens is gestart stromen naar storage-account?**
    
    U kunt naar het opslagaccount dat is geconfigureerd en containers selecteren. Als de container een vermelding voor insights-logs-azurebackupreport heeft, betekent dit dat rapportagegegevens is gestart stromen.

2. **Wat is de frequentie van de gegevens-push naar storage-account en Azure Backup-inhoudspakket in Power BI?**

   Voor gebruikers van de dag 0, zou het ongeveer 24 uur duren om gegevens te pushen naar storage-account. Zodra deze initiële push voltooid is, worden gegevens worden vernieuwd met de volgende frequentie wordt weergegeven in de onderstaande afbeelding. 
      * Gegevens met betrekking tot **taken, waarschuwingen, back-Upitems, kluizen, beveiligde Servers en beleidsregels** wordt doorgestuurd voor opslagaccount als de klant en wanneer ze wordt vastgelegd.
      * Gegevens met betrekking tot **opslag** wordt gepusht naar klant storage-account om de 24 uur.
   
    ![Azure Backup-rapporten gegevens pushen frequentie](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

  Power BI beschikt over een [geplande vernieuwing eenmaal per dag](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). U kunt handmatig vernieuwen van de gegevens in Power BI uitvoeren voor het inhoudspakket.

3. **Hoe lang kan ik de rapporten behouden?** 

   Tijdens het configureren van storage-account, kunt u de bewaarperiode van rapportgegevens selecteren in de storage-account (met stap 6 van de storage-account configureren voor rapporten hierboven). Behalve dat u kunt [rapporten analyseren in excel](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) en deze wilt opslaan voor een langere bewaartermijn, aan de hand van uw behoeften. 

4. **Zie ik mijn gegevens in rapporten na het configureren van het storage-account?**

   Alle gegevens die zijn gegenereerd na **"configuratie van storage-account"** gepusht naar het storage-account en is beschikbaar in rapporten. Echter, **In voortgang van taken worden niet gepusht** voor rapportage. Nadat de taak is voltooid of mislukt, wordt deze verzonden naar rapporten.

5. **Als ik de storage-account om rapporten weer te geven al hebt geconfigureerd, kan ik de configuratie voor het gebruik van een ander opslagaccount wijzigen?** 

   Ja, kunt u de configuratie te verwijzen naar een ander opslagaccount. U moet het meest recent geconfigureerde opslagaccount gebruiken bij het verbinden met Azure Backup-inhoudspakket. Nadat een ander opslagaccount is geconfigureerd, zou ook nieuwe gegevens in dit opslagaccount stromen. Maar oudere gegevens (vóór het wijzigen van de configuratie) nog steeds in de oudere storage-account.

6. **Kan ik rapporten bekijken tussen kluizen en -abonnementen?** 

   Ja, kunt u hetzelfde opslagaccount in verschillende kluizen om cross-kluis rapporten weer te geven. U kunt ook hetzelfde opslagaccount voor kluizen configureren voor abonnementen. U kunt vervolgens dit opslagaccount wordt gebruikt bij het verbinden met Azure Backup-inhoudspakket in Power BI gebruiken om de rapporten weer te geven. Het geselecteerde opslagaccount moet echter wel in dezelfde regio als de recovery services-kluis.
   
## <a name="troubleshooting-errors"></a>Fouten oplossen
| Foutdetails | Oplossing |
| --- | --- |
| Na het instellen van het opslagaccount voor Backup-rapporten, **Opslagaccount** toont nog steeds **niet geconfigureerd**. | Als u de storage-account is geconfigureerd, wordt de rapportagegegevens flow ondanks dit probleem. U lost dit probleem, gaat u naar Azure portal > alle services > diagnostische instellingen > RS kluis > Instellingen bewerken. De eerder geconfigureerde instelling verwijderen en een nieuwe instelling van de dezelfde blade maken. Deze keer stelt u het veld **naam** naar **service**. Dit moet het geconfigureerde opslagaccount worden weergegeven. |
|Na het importeren van Azure Backup-inhoudspakket in Power BI, de fout **404-container is niet gevonden** wordt weergegeven. | Zoals aangegeven in dit document, moet u wachten gedurende 24 uur na het configureren van rapporten in Recovery Services-kluis correct weergegeven in Power BI. Als u probeert te krijgen tot de rapporten voor 24 uur, krijgt u deze fout omdat de volledige gegevens is nog niet aanwezig zijn om geldige rapporten weer te geven. |

## <a name="next-steps"></a>Volgende stappen
Nu dat u het opslagaccount en geïmporteerd Azure Backup-inhoudspakket hebt geconfigureerd, wordt de volgende stap is het pas deze rapporten en rapportage gegevensmodel gebruiken om rapporten te maken. Zie de volgende artikelen voor meer informatie.

* [Met behulp van Azure Backup reporting gegevensmodel](backup-azure-reports-data-model.md)
* [Filters gebruiken om rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [Het maken van rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)

