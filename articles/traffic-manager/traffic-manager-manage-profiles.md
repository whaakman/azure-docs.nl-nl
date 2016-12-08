---
title: Azure Traffic Manager-profielen beheren | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u de geschiedenis van een Azure Traffic Manager-profiel maakt, uitschakelt, inschakelt, verwijdert en weergeeft.
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 7e7de7dc1eca6903403afef03fdd6afb98ff16c9

---

# <a name="manage-an-azure-traffic-manager-profile"></a>Een Azure Traffic Manager-profiel beheren

Traffic Manager-profielen gebruiken verkeersrouteringsmethoden voor het beheren van de distributie van verkeer naar uw cloudservices of de website-eindpunten. In dit artikel wordt uitgelegd hoe u deze profielen kunt maken en beheren.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Een Traffic Manager-profiel maken met Snelle invoer

U kunt in de klassieke Azure-portal met Snelle invoer snel een Traffic Manager-profiel maken. Met Snelle invoer kunt u profielen met standaardconfiguratie-instellingen maken. U kunt Snelle invoer echter niet gebruiken voor instellingen als de set eindpunten (cloudservices en websites), de failovervolgorde, de routeringsmethode voor het failoververkeer en bewakingsinstellingen. Zodra u het profiel hebt gemaakt, kunt u deze instellingen configureren in de klassieke Azure-portal. Traffic Manager biedt ondersteuning voor maximaal 200 eindpunten per profiel. Voor de meeste gebruiksscenario's zijn echter slechts enkele eindpunten vereist.

### <a name="to-create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

1. **Implementeer uw cloudservices en websites naar uw productieomgeving.** Zie [Cloudservices](http://go.microsoft.com/fwlink/p/?LinkId=314074) voor meer informatie over cloudservices. Zie [Websites](http://go.microsoft.com/fwlink/p/?LinkId=393327) voor meer informatie over websites.
2. **Meld u aan bij de klassieke Azure Portal.** Klik linksonder in de portal op **Nieuw**. Klik vervolgens op **Network Services > Traffic Manager** en dan op **Snelle invoer** om uw profiel te configureren.
3. **Configureer het DNS-voorvoegsel.** Geef voor het DNS-voorvoegsel van uw Traffic Manager-profiel een unieke naam op. U kunt alleen het voorvoegsel voor een Traffic Manager-domeinnaam opgeven.
4. **Selecteer het abonnement.** Selecteer het juiste Azure-abonnement. Elk profiel is gekoppeld aan één abonnement. Als u slechts één abonnement hebt, wordt deze optie niet weergegeven.
5. **Selecteer de verkeersrouteringsmethode.** Selecteer bij **Traffic Routing Policy** (Beleid voor verkeersroutering) de verkeersrouteringsmethode. Zie [Over de verkeersrouteringsmethoden voor Traffic Manager](traffic-manager-routing-methods.md) voor meer informatie over verkeersrouteringsmethoden.
6. **Klik op Maken om het profiel te maken**. Zodra het profiel is geconfigureerd, kunt u in het deelvenster Traffic Manager van de klassieke Azure-portal zoeken uw profiel zoeken.
7. **Configureer eindpunten, de bewaking en andere instellingen in de klassieke Azure Portal.** Met Snelle invoer worden alleen basisinstellingen geconfigureerd. Het is noodzakelijk aanvullende instellingen te configureren, zoals de lijst van eindpunten en de volgorde van de eindpunt-failover.

## <a name="disable-enable-or-delete-a-profile"></a>Een profiel uitschakelen, inschakelen of verwijderen

U kunt een bestaand profiel uitschakelen zodat Traffic Manager geen gebruikersaanvragen naar de geconfigureerde eindpunten verwijst. Wanneer u een Traffic Manager-profiel uitschakelt, blijven het profiel en de informatie in het profiel intact en kunt u het profiel en de inhoud bewerken in de Traffic Manager-interface.  Verwijzingen worden hervat wanneer u het profiel opnieuw inschakelt. Wanneer u in de klassieke Azure-portal een Traffic Manager-profiel maakt, wordt het profiel automatisch ingeschakeld. Als u een profiel niet meer nodig hebt, kunt u het verwijderen.

### <a name="to-disable-a-profile"></a>Een profiel uitschakelen

1. Als u een aangepaste domeinnaam gebruikt, wijzigt u de CNAME-record op de Internet-DNS-server zodanig dat deze niet langer naar uw Traffic Manager-profiel wijst.
2. Het verkeer wordt niet meer via de Traffic Manager-profielinstellingen omgeleid naar de eindpunten.
3. Selecteer het profiel dat u wilt uitschakelen. Markeer het profiel op de pagina Traffic Manager door op de kolom naast de profielnaam te klikken. Denk eraan dat, als u op de naam van het profiel of de pijl naast de naam klikt, de instellingenpagina voor het profiel wordt geopend.
4. Nadat u het profiel hebt geselecteerd, klikt u onder aan de pagina op **Uitschakelen**.

### <a name="to-enable-a-profile"></a>Een profiel inschakelen

1. Selecteer het profiel dat u wilt uitschakelen. Markeer het profiel op de pagina Traffic Manager door op de kolom naast de profielnaam te klikken. Denk eraan dat, als u op de naam van het profiel of de pijl naast de naam klikt, de instellingenpagina voor het profiel wordt geopend.
2. Nadat u het profiel hebt geselecteerd, klikt u onder aan de pagina op **Inschakelen**.
3. Als u een aangepaste domeinnaam gebruikt, maakt u een CNAME-resourcerecord op de Internet-DNS-server zodat deze naar de domeinnaam van uw Traffic Manager-profiel wijst.
4. Het verkeer wordt weer omgeleid naar de eindpunten.

### <a name="to-delete-a-profile"></a>Een profiel verwijderen

1. Zorg ervoor dat de DNS-resourcerecord op uw Internet DNS-server geen CNAME-resourcerecord meer gebruikt die naar de domeinnaam van uw Traffic Manager-profiel wijst.
2. Selecteer het profiel dat u wilt uitschakelen. Markeer het profiel op de pagina Traffic Manager door op de kolom naast de profielnaam te klikken. Denk eraan dat, als u op de naam van het profiel of de pijl naast de naam klikt, de instellingenpagina voor het profiel wordt geopend.
3. Nadat u het profiel hebt geselecteerd, klikt u onder aan de pagina op **Verwijderen**.

## <a name="view-traffic-manager-profile-change-history"></a>De wijzigingsgeschiedenis van uw Traffic Manager-profiel weergeven

U kunt de wijzigingsgeschiedenis voor uw Traffic Manager-profiel in de klassieke Azure-portal bekijken in Management Services.

### <a name="to-view-your-traffic-manager-change-history"></a>De wijzigingsgeschiedenis van uw Traffic Manager weergeven

1. Klik in het linkerdeelvenster van de klassieke Azure-portal op **Beheerservices**.
2. Klik op de pagina Beheerservices op **Bewerkingslogboeken**.
3. U kunt op de pagina Bewerkingslogboeken een filter toepassen om de wijzigingsgeschiedenis voor uw Traffic Manager-profiel weer te geven. Nadat u de filteropties hebt geselecteerd, klikt u op het vinkje om de resultaten weer te geven.

   * Als u de wijzigingen voor al uw profielen wilt weergeven, selecteert u uw abonnement en de gewenste periode. Vervolgens selecteert u in het snelmenu **Type** de optie **Traffic Manager**.
   * Als u wilt filteren op de profielnaam, typt u de naam van het profiel in het veld **Servicenaam** of selecteert u de naam in het snelmenu.
   * Als u de details voor elke afzonderlijke wijziging wilt weergeven, selecteert u de rij met de wijziging die u wilt weergeven en klikt u onder aan de pagina op **Details**. In het venster **Bewerkingsdetails** kunt u de XML-weergave van het API-object bekijken dat is gemaakt of bijgewerkt als onderdeel van de bewerking.

## <a name="next-steps"></a>Volgende stappen

* [Een eindpunt toevoegen](traffic-manager-endpoints.md)
* [Methode voor failover-routering configureren](traffic-manager-configure-failover-routing-method.md)
* [Routeringsmethode voor round robin configureren](traffic-manager-configure-round-robin-routing-method.md)
* [Routeringsmethode voor prestaties configureren](traffic-manager-configure-performance-routing-method.md)
* [Het internetdomein van een bedrijf naar een Traffic Manager-domeinnaam laten wijzen](traffic-manager-point-internet-domain.md)
* [Problemen met Traffic Manager in gedegradeerde status oplossen](traffic-manager-troubleshooting-degraded.md)



<!--HONumber=Nov16_HO5-->


