---
title: Advanced Threat Protection voor Azure Storage
description: Configureer Azure Storage Advanced Threat Protection om afwijkingen in de account activiteit te detecteren en u te informeren over mogelijk schadelijke pogingen om toegang te krijgen tot uw account.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: 5d0b41344f4a77f3205799be7c30526e2a150523
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479929"
---
# <a name="advanced-threat-protection-for-azure-storage"></a>Advanced Threat Protection voor Azure Storage

Advanced Threat Protection voor Azure Storage biedt een extra laag met beveiligingsinformatie voor de detectie van ongebruikelijke en mogelijk schadelijke pogingen om toegang te verkrijgen tot of aanvallen op opslagaccounts. Deze beveiligingslaag biedt u de mogelijkheid bedreigingen te verhelpen zonder dat u een beveiligings expert of beveiligings bewakings systemen hoeft te beheren. 

Beveiligings waarschuwingen worden geactiveerd wanneer afwijkingen in de activiteit optreden.  Deze beveiligings waarschuwingen zijn geïntegreerd met [Azure Security Center](https://azure.microsoft.com/services/security-center/)en worden ook via e-mail verzonden naar abonnements beheerders, met details over verdachte activiteiten en aanbevelingen voor het onderzoeken en oplossen van bedreigingen.

> [!NOTE]
> * Advanced Threat Protection voor Azure Storage is momenteel alleen beschikbaar voor de Blob-opslag.
> * Voor prijs informatie, inclusief een gratis proef versie van 30 dagen, raadpleegt u de [pagina met Azure Security Center prijzen]( https://azure.microsoft.com/pricing/details/security-center/).
> * ATP voor Azure Storage-functie is momenteel niet beschikbaar in azure Government en soevereine Cloud regio's.

Geavanceerde bedreigings beveiliging voor Azure Storage opname van Diagnostische logboeken met lees-, schrijf-en verwijder aanvragen naar Blob-opslag voor detectie van bedreigingen. Als u de waarschuwingen van Advanced Threat Protection wilt onderzoeken, kunt u gerelateerde opslag activiteiten bekijken met Opslaganalyse logboek registratie. Zie [Opslaganalyse logboek registratie configureren](storage-monitor-storage-account.md#configure-logging)voor meer informatie.

## <a name="set-up-advanced-threat-protection"></a>Geavanceerde bedreigings beveiliging instellen 

### <a name="using-the-portal"></a>De portal gebruiken

1. Start de Azure Portal op [https://portal.azure.com](https://portal.azure.com/).

2. Ga naar de configuratie pagina van het Azure Storage account dat u wilt beveiligen. Selecteer op de pagina **instellingen** de optie **geavanceerde beveiliging tegen bedreigingen**.

3. In de Blade **geavanceerde beveiliging tegen bedreigingen** configureren
    * Advanced  *Threat Protection* inschakelen
    * Klik op **Opslaan** om het nieuwe of bijgewerkte geavanceerde beveiligings beleid voor bedreigingen op te slaan. (Prijzen in de afbeelding zijn alleen bedoeld als voor beeld.)

![Azure Storage Advanced Threat Protection inschakelen](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

### <a name="using-azure-security-center"></a>Met behulp van Azure Security Center

Wanneer u zich abonneert op de Standard-laag in Azure Security Center, wordt geavanceerde beveiliging tegen bedreigingen automatisch ingesteld op al uw opslag accounts. U kunt de geavanceerde beveiliging tegen bedreigingen voor uw opslag accounts onder een specifiek abonnement als volgt in-of uitschakelen:

1. Start **Azure Security Center** in het [Azure Portal]([https://portal.azure.com).
1. Klik in het hoofd menu op **prijzen & instellingen**.
1. Klik op het abonnement dat u wilt in-of uitschakelen bedreigings beveiliging voor de bijbehorende opslag accounts.

    ![Abonnement selecteren](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-subscription.png)

1. Klik op **prijs categorie**.
1. Klik in de sectie **prijs categorie op resource type selecteren** in de rij **opslag accounts** op **ingeschakeld** of **uitgeschakeld**.

    ![ATP in Security Center inschakelen](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-pricing2.png)
1. Klik op **Opslaan**.

### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager sjablonen gebruiken

Gebruik een Azure Resource Manager sjabloon om een Azure Storage account te implementeren waarop Advanced Threat Protection is ingeschakeld. Zie [opslag account met geavanceerde beveiliging tegen bedreigingen](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/)voor meer informatie.

### <a name="using-azure-policy"></a>Azure Policy gebruiken

Gebruik een Azure Policy om geavanceerde bedreigings beveiliging in te scha kelen voor opslag accounts onder een specifiek abonnement of resource groep.

1. Start de pagina Azure **Policy-Definitions** .

1. Zoek naar het beleid **geavanceerde beveiliging tegen bedreigingen implementeren op opslag accounts** .

     ![Zoek beleid](./media/storage-advanced-threat-protection/storage-atp-policy-definitions.png)
  
1. Selecteer een Azure-abonnement of resource groep.

    ![Abonnement of groep selecteren](./media/storage-advanced-threat-protection/storage-atp-policy2.png)

1. Wijs het beleid toe.

    ![Pagina beleids definities](./media/storage-advanced-threat-protection/storage-atp-policy1.png)

### <a name="using-rest-api"></a>REST API gebruiken
Gebruik rest API-opdrachten om de geavanceerde instelling voor beveiliging tegen bedreigingen te maken, bij te werken of op te halen voor een specifiek opslag account.

* [Advanced Threat Protection-maken](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
* [Advanced Threat Protection-ophalen](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="using-azure-powershell"></a>Azure PowerShell gebruiken

Gebruik de volgende Power shell-cmdlets:

  * [Advanced Threat Protection inschakelen](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
  * [Geavanceerde beveiliging tegen bedreigingen verkrijgen](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
  * [Geavanceerde bedreigings beveiliging uitschakelen](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

## <a name="explore-security-anomalies"></a>Beveiligings afwijkingen verkennen

Wanneer er afwijkingen optreden bij de opslag activiteit, ontvangt u een e-mail melding met informatie over de verdachte beveiligings gebeurtenis. Details van de gebeurtenis zijn onder andere:

* De aard van de anomalie
* De naam van het opslag account
* De tijd van de gebeurtenis
* Het opslag type
* De mogelijke oorzaken 
* De onderzoek stappen
* De herstels tappen


Het e-mail bericht bevat ook informatie over mogelijke oorzaken en aanbevolen acties voor het onderzoeken en oplossen van mogelijke bedreigingen.

![E-mail bericht over de geavanceerde beveiliging tegen bedreigingen Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

U kunt uw huidige beveiligings waarschuwingen controleren en beheren via de [tegel beveiligings waarschuwingen](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts)van Azure Security Center. Als u op een specifieke waarschuwing klikt, vindt u details en acties voor het onderzoeken van de huidige dreiging en het adresseren van toekomstige bedreigingen.

![E-mail bericht over de geavanceerde beveiliging tegen bedreigingen Azure Storage](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Beveiligings waarschuwingen

Waarschuwingen worden gegenereerd door ongebruikelijke en mogelijk schadelijke pogingen om opslag accounts te openen of misbruik te maken. Deze gebeurtenissen kunnen de volgende waarschuwingen activeren:

### <a name="anomalous-access-pattern-alerts"></a>Waarschuwingen voor afwijkende toegangs patronen

* **Toegang vanaf ongebruikelijke locatie**: Deze waarschuwing wordt geactiveerd wanneer iemand toegang heeft gekregen tot een opslag account vanuit een ongebruikelijke geografische locatie.
Mogelijke oorzaken:
   * Een aanvaller heeft toegang tot uw opslag account
   * Een rechtmatige gebruiker heeft toegang verkregen tot uw opslag account vanaf een nieuwe locatie
 
* **Toepassings afwijkingen**: Deze waarschuwing geeft aan dat een ongewone toepassing toegang heeft gekregen tot dit opslag account. Mogelijke oorzaken:
   * Een aanvaller heeft toegang tot uw opslag account via een nieuwe toepassing.
   * Een rechtmatige gebruiker heeft een nieuwe toepassing/browser gebruikt om toegang te krijgen tot uw opslag account.

* **Anonieme toegang**: Deze waarschuwing geeft aan dat dit account anoniem is geopend (d.w.z. zonder enige verificatie), wat niet wordt verwacht, vergeleken met het recente toegangs patroon voor dit account.
Mogelijke oorzaken:
   * Een aanvaller heeft open bare Lees toegang tot een container.
   * Een rechtmatige gebruiker of toepassing heeft open bare Lees toegang voor een container gebruikt.

* **Tor-afwijkingen**: Deze waarschuwing geeft aan dat de toegang tot dit account is verkregen vanaf een IP-adres dat een actief eind knooppunt van Tor (een anoniem-proxy) wordt genoemd. Bij de ernst van deze waarschuwing wordt rekening gehouden met het gebruikte verificatie type (indien van toepassing) en of dit het eerste geval van dergelijke toegang is.
Mogelijke oorzaken:
   * Een aanvaller heeft toegang tot uw opslag account via Tor.
   * Een rechtmatige gebruiker heeft toegang verkregen tot uw opslag account via Tor.


### <a name="anomalous-extractupload-alerts"></a>Afwijkende uitpak-en upload waarschuwingen

* **Gegevens exfiltration**: Deze waarschuwing geeft aan dat een ongebruikelijk grote hoeveelheid gegevens is geëxtraheerd vergeleken met de recente activiteit op deze opslag container. Mogelijke oorzaken:
   * Een aanvaller heeft een grote hoeveelheid gegevens uit een container geëxtraheerd. (Bijvoorbeeld: gegevens exfiltration/inbreuk, niet-geautoriseerde overdracht van gegevens)
   * Een rechtmatige gebruiker of toepassing heeft een ongebruikelijke hoeveelheid gegevens uit een container geëxtraheerd. (Bijvoorbeeld: onderhouds activiteit)

* **Onverwachte verwijdering**: Deze waarschuwing geeft aan dat er een of meer onverwachte verwijderings bewerkingen zijn uitgevoerd in een opslag account, vergeleken met de recente activiteit voor dit account. Mogelijke oorzaken:
   * Een aanvaller heeft gegevens uit uw opslag account verwijderd.
   * Een rechtmatige gebruiker heeft een ongebruikelijke verwijdering uitgevoerd.

* **Azure-Cloud service pakket uploaden**: Deze waarschuwing geeft aan dat een Azure-Cloud service pakket (. cspkg-bestand) op een ongebruikelijke manier is geüpload naar een opslag account, vergeleken met de recente activiteit voor dit account. Mogelijke oorzaken: 
   * Een aanvaller is voor bereid op het implementeren van schadelijke code vanuit uw opslag account naar een Azure-Cloud service.
   * Een rechtmatige gebruiker is voor bereid voor een legitieme service-implementatie.

### <a name="suspicious-storage-activities-alerts"></a>Waarschuwingen voor verdachte opslag activiteiten

* **Wijziging van toegangs machtigingen**: Deze waarschuwing geeft aan dat de toegangs machtigingen van deze opslag container op een ongebruikelijke manier zijn gewijzigd. Mogelijke oorzaken: 
   * Een aanvaller heeft container machtigingen gewijzigd om de beveiliging te verzwakken.
   * Een rechtmatige gebruiker heeft container machtigingen gewijzigd.

* **Toegangs controle**: Deze waarschuwing geeft aan dat de toegangs machtigingen van een opslag account op een ongebruikelijke manier zijn geïnspecteerd, vergeleken met de recente activiteit van dit account. Mogelijke oorzaken: 
   * Een aanvaller heeft Reconnaissance uitgevoerd voor een toekomstige aanval.
   * Een rechtmatige gebruiker heeft onderhoud uitgevoerd op het opslag account.

* **Gegevens verkennen**: Deze waarschuwing geeft aan dat blobs of containers in een opslag account op een ongebruikelijke manier zijn geïnventariseerd, vergeleken met de recente activiteit van dit account. Mogelijke oorzaken: 
   * Een aanvaller heeft Reconnaissance uitgevoerd voor een toekomstige aanval.
   * Een rechtmatige gebruiker-of toepassings logica heeft verkennen van gegevens binnen het opslag account.






## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Logboeken in azure Storage-accounts](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Meer informatie over [Azure Security Center](../../security-center/security-center-intro.md)
