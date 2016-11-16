---
title: Eindpunten beheren in Azure Traffic Manager | Microsoft Docs
description: Dit artikel helpt u bij het toevoegen, verwijderen, inschakelen en uitschakelen van eindpunten vanuit Azure Traffic Manager.
services: traffic-manager
documentationcenter: 
author: sdwheeler
manager: carmonm
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e6af6652d39fad5812c15e19fa29c757595a78b6

---

# <a name="add-disable-enable-or-delete-endpoints"></a>Eindpunten toevoegen, uitschakelen, inschakelen of verwijderen

De functie Web Apps in Azure App Service biedt al failover- en round-robinverkeersroutering voor websites in een datacenter, ongeacht de websitemodus. Met Azure Traffic Manager kunt u failover- en round-robinverkeersroutering opgeven voor websites en cloudservices in verschillende datacenters. Het eerste dat u moet doen om deze functionaliteit te gebruiken, is het cloudservice- of website-eindpunt toevoegen aan Traffic Manager.

> [!NOTE]
> In dit artikel wordt het gebruik van de klassieke portal uitgelegd. De klassieke Azure Portal ondersteunt alleen het maken en de toewijzing van cloudservices en Web-apps als eindpunten. De nieuwe [Azure Portal](https://portal.azure.com) is de voorkeursinterface.

U kunt ook afzonderlijke eindpunten uitschakelen die deel uitmaken van een Traffic Manager-profiel. Als u een eindpunt uitschakelt, blijft het onderdeel van het profiel, maar het profiel gedraagt zich alsof het eindpunt er niet in is opgenomen. Deze actie is handig als u een eindpunt dat in de onderhoudsmodus staat of opnieuw wordt geïmplementeerd, tijdelijk wilt verwijderen. Zodra het eindpunt opnieuw actief is, kan het worden ingeschakeld.

> [!NOTE]
> Het uitschakelen van een eindpunt heeft geen invloed op de implementatiestatus in Azure. Een eindpunt met een goede status blijft actief en in staat om verkeer te verwerken, zelfs als het in Traffic Manager is uitgeschakeld. Bovendien heeft het uitschakelen van een eindpunt in een bepaald profiel geen invloed op de status ervan in een ander profiel.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Een cloudservice- of website-eindpunt toevoegen

1. Zoek in het deelvenster Traffic Manager in de klassieke Azure Portal het Traffic Manager-profiel met de eindpuntinstellingen die u wilt wijzigen. Klik op de pijl rechts van de profielnaam om de instellingenpagina te openen.
2. Klik boven aan de pagina op **Eindpunten** om de eindpunten weer te geven die al deel uitmaken van uw configuratie.
3. Klik onder aan de pagina op **Toevoegen** om naar de pagina **Service-eindpunten toevoegen** te gaan. De cloudservices worden op deze pagina standaard onder **Service-eindpunten** weergegeven.
4. Als het cloudservices betreft, selecteert u de cloudservices in de lijst om ze toe te voegen als eindpunten voor dit profiel. Als u de naam van de cloudservice wist, wordt deze verwijderd uit de lijst met eindpunten.
5. Als het websites betreft, klikt u op de vervolgkeuzelijst **Servicetype** en selecteert u vervolgens **Web-app**.
6. Selecteer de websites in de lijst om ze toe te voegen als eindpunten voor dit profiel. Als u de naam van de website wist, wordt deze verwijderd uit de lijst met eindpunten. U kunt slechts één website per Azure-datacenter (ook wel bekend als een regio) selecteren. Wanneer u de eerste website selecteert, kunnen de andere websites in hetzelfde datacenter niet meer worden geselecteerd. Houd er ook rekening mee dat alleen standaardwebsites worden weergegeven.
7. Nadat u de eindpunten voor dit profiel hebt geselecteerd, klikt u op het vinkje in de rechterbenedenhoek om uw wijzigingen op te slaan.

> [!NOTE]
> Nadat een eindpunt hebt toegevoegd aan of verwijderd uit een profiel met de verkeersrouteringsmethode *Failover*, wordt de failover-prioriteitenlijst mogelijk niet geordend zoals u dat wilt. U kunt de volgorde van de Failover-prioriteitenlijst aanpassen op de pagina Configuratie. Zie voor meer informatie [Configure Failover traffic routing](traffic-manager-configure-failover-routing-method.md) (Failover-verkeersroutering configureren).

## <a name="to-disable-an-endpoint"></a>Een eindpunt uitschakelen

1. Zoek in het deelvenster Traffic Manager in de klassieke Azure Portal het Traffic Manager-profiel met de eindpuntinstellingen die u wilt wijzigen. Klik op de pijl rechts van de profielnaam om de instellingenpagina te openen.
2. Klik boven aan de pagina op **Eindpunten** om de eindpunten weer te geven die deel uitmaken van uw configuratie.
3. Klik op het eindpunt dat u wilt uitschakelen en klik vervolgens onder aan de pagina op **Uitschakelen**.
4. Clients blijven verkeer verzenden naar het eindpunt voor de duur van de Time-to-Live (TTL). U kunt de TTL wijzigen op de pagina Configuratie van het Traffic Manager-profiel.

## <a name="to-enable-an-endpoint"></a>Een eindpunt inschakelen

1. Zoek in het deelvenster Traffic Manager in de klassieke Azure Portal het Traffic Manager-profiel met de eindpuntinstellingen die u wilt wijzigen. Klik op de pijl rechts van de profielnaam om de instellingenpagina te openen.
2. Klik boven aan de pagina op **Eindpunten** om de eindpunten weer te geven die deel uitmaken van uw configuratie.
3. Klik op het eindpunt dat u wilt inschakelen en klik vervolgens onder aan de pagina op **Inschakelen**.
4. Clients worden omgeleid naar het ingeschakelde eindpunt op basis van de instellingen van het profiel.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Een cloudservice- of website-eindpunt verwijderen

1. Zoek in het deelvenster Traffic Manager in de klassieke Azure Portal het Traffic Manager-profiel met de eindpuntinstellingen die u wilt wijzigen. Klik op de pijl rechts van de profielnaam om de instellingenpagina te openen.
2. Klik boven aan de pagina op **Eindpunten** om de eindpunten weer te geven die al deel uitmaken van uw configuratie.
3. Klik op de pagina Eindpunten op de naam van het eindpunt dat u uit het profiel wilt verwijderen.
4. Klik onder aan de pagina op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* [Traffic Manager-profielen beheren](traffic-manager-manage-profiles.md)
* [Routeringsmethoden configureren](traffic-manager-configure-routing-method.md)
* [Problemen met Traffic Manager in gedegradeerde status oplossen](traffic-manager-troubleshooting-degraded.md)
* [Prestatieoverwegingen voor Traffic Manager](traffic-manager-performance-considerations.md)
* [Bewerkingen op Traffic Manager (REST API-referentiemateriaal)](http://go.microsoft.com/fwlink/p/?LinkID=313584)




<!--HONumber=Nov16_HO2-->


