---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/09/2018
ms.author: cephalin
ms.openlocfilehash: 2e6697b07f305a78ef4cc3f462c76804160a9d12
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572062"
---
Nadat de records voor uw domeinnaam zijn doorgegeven, zou het mogelijk voor gebruik met uw browser om te controleren dat de naam van uw aangepaste domein kan worden gebruikt voor toegang tot uw web-app in Azure App Service.

> [!NOTE]
> Het kan even duren voordat uw CNAME worden doorgegeven via de DNS-systeem. U kunt een service zoals <a href="http://www.digwebinterface.com/"> http://www.digwebinterface.com/ </a> om te controleren of de CNAME beschikbaar is.
> 
> 

Als u uw web-app hebben niet als een Traffic Manager-eindpunt hebt toegevoegd, moet u dit doen voordat de naamomzetting werkt als het aangepaste domein naam routes naar Traffic Manager. Traffic Manager stuurt vervolgens naar uw web-app. Gebruik de informatie in [toevoegen of verwijderen-eindpunten](../articles/traffic-manager/traffic-manager-endpoints.md) om toe te voegen van uw web-app als een eindpunt in uw Traffic Manager-profiel.

> [!NOTE]
> Als uw web-app niet wordt vermeld bij het toevoegen van een eindpunt, moet u controleren of deze is geconfigureerd voor **Standard** modus voor App Service-plan. U moet gebruiken **Standard** modus voor uw web-app om te kunnen werken met Traffic Manager.
> 
> 

1. Open in uw browser de [Azure Portal](https://portal.azure.com).
2. In de **Web-Apps** en klik op de naam van uw web-app, selecteer **instellingen**, en selecteer vervolgens **aangepaste domeinen**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. In de **aangepaste domeinen** blade, klikt u op **hostnaam toevoegen**.
4. Gebruik de **hostnaam** tekstvakken in te voeren van de naam van het Traffic Manager-domein te koppelen aan deze web-app.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Klik op **valideren** om op te slaan van de configuratie van de naam.
6. Wanneer u op klikt **valideren** Azure domeinverificatie werkstroom wordt gestart. Hiermee wordt gecontroleerd voor domeineigendom, evenals de beschikbaarheid en het rapport succes hostnaam of gedetailleerde fout met prescriptieve richtlijnen voor het oplossen van de fout.    
7. Na een succesvolle validatie **hostnaam toevoegen** knop activeren en kunt u zich op de hostnaam toewijzen. Nu gaat u naar de naam van uw aangepaste domein in een browser. U ziet nu uw app wordt uitgevoerd met behulp van uw aangepaste domeinnaam. 
   
   Zodra de configuratie is voltooid, de naam van het aangepaste domein wordt weergegeven in de **domeinnamen** sectie van uw web-app.

Op dit moment zou het mogelijk in te voeren van de naam van het Traffic Manager-domein in uw browser en Zie dat het met succes u naar uw web-app gaat.

