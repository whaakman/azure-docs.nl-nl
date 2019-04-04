---
title: Advanced Threat Protection voor Azure Storage
description: Azure Storage Advanced Threat Protection voor de detectie van afwijkingen in Accountactiviteit en een melding van mogelijk schadelijke pogingen om toegang te krijgen tot uw account configureren.
services: storage
author: rmatchoro
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: monhaber
ms.manager: shaik
ms.openlocfilehash: 78338ece1bc70d8410bd71183a34aaf1a52f2d1b
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904114"
---
# <a name="advanced-threat-protection-for-azure-storage"></a>Advanced Threat Protection voor Azure Storage

Advanced Threat Protection voor Azure Storage biedt een extra laag met beveiligingsinformatie die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van opslagaccounts gedetecteerd. Deze extra beveiliging kunt u om bedreigingen zonder de noodzaak om te worden van een beveiligingsexpert of beheren van controle beveiligingssystemen. 

Beveiligingswaarschuwingen worden geactiveerd bij afwijkingen in de activiteit is uitgevoerd.  Deze waarschuwingen zijn geïntegreerd met [Azure Security Center](https://azure.microsoft.com/services/security-center/), en ook via e-mail worden verzonden naar alle abonnementsbeheerders met details van verdachte activiteiten en aanbevelingen voor het onderzoeken en bedreigingen verhelpen.

> [!NOTE]
> * Advanced Threat Protection voor Azure Storage is momenteel alleen beschikbaar voor de Blob-opslag.
> * Zie voor meer informatie, inclusief een gratis 30-daagse proefversie aan, prijzen de [pagina met prijzen van Azure Security Center]( https://azure.microsoft.com/en-us/pricing/details/security-center/).
> * ATP voor Azure storage-functie is momenteel niet beschikbaar in Azure government en onafhankelijke cloud-regio's.

Advanced Threat Protection voor Azure Storage neemt diagnostische logboeken van lees-, schrijf- en delete-aanvragen naar Blob-opslag voor detectie van bedreigingen. U kunt gerelateerde opslagactiviteit in Storage Analytics logboekregistratie weergeven voor het onderzoeken van de waarschuwingen van Advanced Threat Protection. Zie voor meer informatie over het [Opslaganalyse, logboekregistratie configureren](storage-monitor-storage-account.md#configure-logging).

## <a name="set-up-advanced-threat-protection"></a>Advanced Threat Protection instellen 

### <a name="using-the-portal"></a>De portal gebruiken

1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com/).

2. Navigeer naar de configuratiepagina van de Azure Storage-account dat u wilt beveiligen. In de **instellingen** weergeeft, schakelt **Advanced Threat Protection**.

3. In de **Advanced Threat Protection** configuratieblade
    * Schakel **ON** geavanceerde *Threat Protection*
    * Klik op **opslaan** de nieuwe of bijgewerkte Advanced Threat Protection-beleid op te slaan. (De prijzen in de afbeelding zijn bijvoorbeeld uitsluitend.)

![Azure Storage advanced threat protection inschakelen](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="using-azure-security-center"></a>Met behulp van Azure Security Center
Wanneer u zich abonneert op de Standard-laag in Azure Security Center Advanced Threat Protection is ingesteld in uw storage-accounts. Zie voor meer informatie [een upgrade uitvoert naar Standard van Security Center-prijscategorie voor verbeterde beveiliging](https://docs.microsoft.com/azure/security-center/security-center-pricing). (De prijzen in de afbeelding zijn bijvoorbeeld uitsluitend.)

![Standard-laag in ASC](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing.png)

### <a name="using-azure-resource-manager-templates"></a>Met behulp van Azure Resource Manager-sjablonen

Gebruik een Azure Resource Manager-sjabloon voor het implementeren van een Azure Storage-account met Advanced Threat Protection ingeschakeld.
Zie voor meer informatie, [Storage-account met Advanced Threat Protection](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/).

### <a name="using-azure-policy"></a>Met behulp van Azure Policy

Azure-beleid gebruiken om in te schakelen van Advanced Threat Protection voor opslagaccounts in een bepaald abonnement of resourcegroep bevinden.

1. De Azure lunch **beleid - definities** pagina.

1. Zoek de **implementeren Advanced Threat Protection op Storage-Accounts** beleid.

     ![Beleid voor zoeken](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)
  
1. Selecteer een Azure-abonnement of resourcegroep.

    ![Abonnement of groep selecteren](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. Het beleid toewijst.

    ![Pagina met beleid-definities](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="using-rest-api"></a>REST API gebruiken
Rest-API-opdrachten gebruiken voor het maken, bijwerken of de instelling van Advanced Threat Protection voor een specifiek opslagaccount ophalen.

* [Advanced Threat Protection - maken](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Advanced Threat Protection - ophalen](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="using-azure-powershell"></a>Azure PowerShell gebruiken

Gebruik de volgende PowerShell-cmdlets:

  * [Advanced Threat Protection inschakelen](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
  * [Ophalen van Advanced Threat Protection](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
  * [Advanced Threat Protection uitschakelen](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

## <a name="explore-security-anomalies"></a>Verken onregelmatigheden voor beveiliging

Wanneer storage afwijkingen in de activiteit is uitgevoerd, ontvangt u een e-mailmelding met informatie over de verdachte beveiligingsgebeurtenis. Details van de gebeurtenis zijn onder andere:

* De aard van de anomaliedetectie
* Naam van het opslagaccount
* De tijd van de gebeurtenis
* Het opslagtype
* De mogelijke oorzaken 
* Welke onderzoeksstappen
* De stappen voor herstel


Het e-mailbericht ook vindt u informatie over mogelijke oorzaken en aanbevolen acties om te onderzoeken en oplossen van de mogelijke bedreiging.

![Azure Storage advanced threat protection e-mailwaarschuwing](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

U kunt bekijken en beheren van uw huidige beveiligingswaarschuwingen in Azure Security Center [tegel beveiligingswaarschuwingen](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts). Te klikken op een specifieke waarschuwing bevat details en acties voor de huidige bedreiging te onderzoeken en toekomstige bedreigingen-adressering.

![Azure Storage advanced threat protection e-mailwaarschuwing](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Protection-waarschuwingen

Waarschuwingen worden gegenereerd door ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van storage-accounts. Deze gebeurtenissen kunnen de volgende waarschuwing is geactiveerd:

### <a name="anomalous-access-pattern-alerts"></a>Waarschuwingen voor afwijkende toegang-patroon

* **Toegang vanaf ongebruikelijke locatie**: Deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot een opslagaccount. Bijvoorbeeld wanneer iemand toegang heeft tot een opslagaccount vanuit een ongebruikelijke geografische locatie.
Mogelijke oorzaken:
   * Een aanvaller toegang heeft gehad tot uw storage-account
   * Een bevoegde gebruiker toegang heeft gehad tot uw storage-account van een nieuwe locatie
 
* **Toepassing Anomaliedetectie**: Deze waarschuwing geeft aan dat een ongebruikelijke toepassing toegang gehad dit opslagaccount wordt gebruikt tot heeft. Mogelijke oorzaken:
   * Een aanvaller toegang heeft gehad tot uw storage-account met behulp van een nieuwe toepassing.
   * Een bevoegde gebruiker heeft een nieuwe toepassing/browser gebruikt voor toegang tot uw storage-account.

* **Anonieme toegang**: Deze waarschuwing geeft aan dat er een wijziging in het toegangspatroon tot een opslagaccount. Bijvoorbeeld: dit account is anoniem gebruikt (dat wil zeggen zonder verificatie), die is onverwacht vergeleken met de recente toegangspatroon voor dit account.
Mogelijke oorzaken:
   * Een aanvaller misbruik heeft gemaakt van openbare leestoegang voor een container.
   * Openbare leestoegang voor een container heeft gebruikgemaakt van een geldige gebruiker of toepassing.

### <a name="anomalous-extractupload-alerts"></a>Waarschuwingen voor afwijkende extract/uploaden

* **Gegevensexfiltratie**: Deze waarschuwing geeft aan dat een ongebruikelijk grote hoeveelheden gegevens is uitgepakt in vergelijking met recente activiteit op dit storage-container. Mogelijke oorzaken:
   * Een aanvaller heeft een grote hoeveelheid gegevens opgehaald uit een container. (Bijvoorbeeld: exfiltratie/inbreuk op gegevens, niet-geautoriseerde overdracht van gegevens)
   * Een geldige gebruiker of toepassing heeft een ongebruikelijke hoeveelheid gegevens opgehaald uit een container. (Bijvoorbeeld: onderhoudsactiviteiten)

* **Onverwachte verwijderen**: Deze waarschuwing geeft aan dat een of meer onverwachte delete-bewerkingen in een storage-account, in vergelijking met recente activiteiten voor dit account heeft plaatsgevonden. Mogelijke oorzaken:
   * Een aanvaller heeft gegevens verwijderd van uw opslagaccount.
   * Een bevoegde gebruiker heeft een ongebruikelijke verwijdering uitgevoerd.

* **Azure Cloud Service-pakket uploaden**: Deze waarschuwing geeft aan dat een Azure Cloud Service-pakket (.cspkg-bestand) is geüpload naar een opslagaccount in een ongebruikelijke manier, in vergelijking met recente activiteiten voor dit account. Mogelijke oorzaken: 
   * Een aanvaller heeft schadelijke code implementeren vanuit uw storage-account aan een Azure-cloud-service is voorbereid.
   * Een bevoegde gebruiker is een betrouwbare service-implementatie is voorbereid.

### <a name="suspicious-storage-activities-alerts"></a>Waarschuwingen voor opslag van verdachte activiteiten

* **Toegang tot de machtiging wijzigen**: Deze waarschuwing geeft aan dat de machtigingen van de opslagcontainer op ongewone wijze zijn gewijzigd. Mogelijke oorzaken: 
   * Een aanvaller kan containermachtigingen om te verzwakken van de beveiliging is gewijzigd.
   * Een bevoegde gebruiker heeft containermachtigingen voor de gewijzigd.

* **Toegang tot inspectie**: Deze waarschuwing geeft aan dat de toegangsrechten van een storage-account zijn gecontroleerd op een ongebruikelijke manier, in vergelijking met recente activiteiten voor dit account. Mogelijke oorzaken: 
   * Een aanvaller heeft reconnaissance uitgevoerd voor een toekomstige aanvallen.
   * Een bevoegde gebruiker is onderhoud uitgevoerd op het storage-account.

* **Data Exploration**: Deze waarschuwing geeft aan dat blobs of containers in een storage-account is geïnventariseerd op ongewone wijze, vergeleken met recente activiteiten voor dit account. Mogelijke oorzaken: 
   * Een aanvaller heeft reconnaissance uitgevoerd voor een toekomstige aanvallen.
   * Een geldige gebruiker of de toepassingslogica is verkend gegevens binnen het opslagaccount.






## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [registreert in Azure Storage-accounts](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Meer informatie over [Azure Security Center](../../security-center/security-center-intro.md)
