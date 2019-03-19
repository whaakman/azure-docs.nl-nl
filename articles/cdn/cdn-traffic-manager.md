---
title: Instellen van failover tussen meerdere Azure CDN-eindpunten met Azure Traffic Manager | Microsoft Docs
description: Meer informatie over het instellen van Azure Traffic Manager met Azure CDN-eindpunten.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: magattus
ms.custom: ''
ms.openlocfilehash: afadef8b29927f909af5be1e1204180724258b74
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58167062"
---
# <a name="set-up-failover-across-multiple-azure-cdn-endpoints-with-azure-traffic-manager"></a>Instellen van failover tussen meerdere Azure CDN-eindpunten met Azure Traffic Manager

Wanneer u Azure Content Delivery Network (CDN) configureert, kunt u de optimale provider en de prijscategorie selecteren voor uw behoeften. Azure CDN, maakt met de wereldwijd gedistribueerde infrastructuur standaard lokale en geografische redundantie en globale taakverdeling om beschikbaarheid van de service en prestaties te verbeteren. Als een locatie niet beschikbaar is voor het leveren van inhoud, aanvragen worden automatisch doorgestuurd naar een andere locatie en de optimale pop-server (op basis van factoren zoals de locatie en de server aanvraagbelasting) wordt gebruikt om elke clientaanvraag. 
 
Als u meerdere CDN-profielen hebt, kunt u de beschikbaarheid en prestaties met Azure Traffic Manager verder verbeteren. U kunt Azure Traffic Manager met Azure CDN gebruiken om taken te verdelen tussen meerdere CDN-eindpunten voor failover, geo-load balancing en andere scenario's. In een typische failover-scenario, worden eerst alle aanvragen van clients omgeleid naar het primaire CDN-profiel. Als het profiel niet beschikbaar is, worden aanvragen vervolgens doorgegeven aan het secundaire CDN-profiel totdat uw primaire CDN-profiel weer online is. Met behulp van Azure Traffic Manager op deze manier zorgt ervoor dat uw web-App is altijd beschikbaar. 

In dit artikel bevat richtlijnen en een voorbeeld van het instellen van failover met **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profielen.

## <a name="set-up-azure-cdn"></a>Instellen van Azure CDN 
Maak twee of meer Azure CDN-profielen en -eindpunten met andere providers.

1. Maken een **Azure CDN Standard van Verizon** en **Azure CDN Standard van Akamai** profiel met de volgende stappen in [maken van een nieuw CDN-profiel](cdn-create-new-endpoint.md#create-a-new-cdn-profile).
 
   ![CDN meerdere profielen](./media/cdn-traffic-manager/cdn-multiple-profiles.png)

2. In elk van de nieuwe profielen, maakt u ten minste één eindpunt met de volgende stappen in [maken van een nieuw CDN-eindpunt](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint).

## <a name="set-up-azure-traffic-manager"></a>Stel Azure Traffic Manager
Een Azure Traffic Manager-profiel maken en instellen van taakverdeling over uw CDN-eindpunten. 

1. Een Azure Traffic Manager-profiel maken met de volgende stappen in [een Traffic Manager-profiel maken](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile). 

    Voor **routeringsmethode**, selecteer **prioriteit**.

2. Toevoegen van uw CDN-eindpunten in uw Traffic Manager-profiel met de volgende stappen in [toevoegen Traffic Manager-eindpunten](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-create-profile#add-traffic-manager-endpoints)

    Voor **Type**, selecteer **externe eindpunten**. Voor **prioriteit**, voer een getal.

    Maak bijvoorbeeld *cdndemo101akamai.azureedge.net* met een prioriteit van *1* en *cdndemo101verizon.azureedge.net* met een prioriteit van *2*.

   ![CDN-eindpunten voor traffic manager](./media/cdn-traffic-manager/cdn-traffic-manager-endpoints.png)


## <a name="set-up-custom-domain-on-azure-cdn-and-azure-traffic-manager"></a>Aangepast domein in Azure CDN en Azure Traffic Manager instellen
Na het instellen van uw CDN- en Traffic Manager-profielen, volg deze stappen voor het toevoegen van DNS-toewijzing en het registreren van aangepast domein aan het CDN-eindpunten. Voor dit voorbeeld wordt de aangepaste domeinnaam is *cdndemo101.dustydogpetcare.online*.

1. Ga naar de website voor de domeinprovider van uw aangepaste domein, zoals GoDaddy, en twee DNS CNAME-vermeldingen te maken. 

    a. Voor de eerste CNAME-vermelding, uw aangepaste domein, met het subdomein cdnverify, worden toegewezen aan uw CDN-eindpunt. Dit item is een vereiste stap voor het registreren van het aangepaste domein naar het CDN-eindpunt dat u hebt toegevoegd aan Traffic Manager in stap 2.

      Bijvoorbeeld: 

      `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101akamai.azureedge.net`  

    b. Voor de tweede CNAME-vermelding, uw aangepaste domein, zonder dat het subdomein cdnverify, worden toegewezen aan uw CDN-eindpunt. Dit item wordt het aangepaste domein toegewezen aan Traffic Manager. 

      Bijvoorbeeld: 
      
      `cdndemo101.dustydogpetcare.online  CNAME  cdndemo101.trafficmanager.net`   

    > [!NOTE]
    > Als uw domein momenteel live is en kan niet worden onderbroken, voert u deze stap laatste. Controleer of het CDN-eindpunten en traffic manager domeinen live zijn voordat u uw aangepaste domein DNS op Traffic Manager bijwerken.
    >


2.  Selecteer het eerste CDN-eindpunt (Akamai) in uw Azure CDN-profiel. Selecteer **aangepast domein toevoegen** en invoer *cdndemo101.dustydogpetcare.online*. Controleer of het vinkje om te valideren van het aangepaste domein is groen. 

    Maakt gebruik van Azure CDN de *cdnverify* subdomein voor het valideren van de DNS-toewijzing voor het voltooien van het registratieproces. Zie voor meer informatie, [maakt u een CNAME-DNS-record](cdn-map-content-to-custom-domain.md#create-a-cname-dns-record). In deze stap kunt Azure CDN voor het herkennen van het aangepaste domein, zodat deze op de aanvragen reageren kan.

3.  Terug naar de website voor de domeinprovider van uw aangepaste domein en bijwerken van de eerste DNS-toewijzing die u hebt gemaakt, zodat het aangepaste domein is toegewezen aan uw tweede CDN-eindpunt.
                             
    Bijvoorbeeld: 

    `cdnverify.cdndemo101.dustydogpetcare.online  CNAME  cdnverify.cdndemo101verizon.azureedge.net`  

4. Van uw Azure CDN-profiel, selecteer het tweede CDN-eindpunt (Verizon) en Herhaal stap 2. Selecteer **aangepast domein toevoegen**, en invoer *cdndemo101.dustydogpetcare.online*.
 
Nadat u deze stappen hebt voltooid, is uw multi-CDN-service met failover-functionaliteit ingesteld met Azure Traffic Manager. U moet toegang hebben tot de test worden URL's van uw aangepaste domein. De functionaliteit voor het testen, het primaire CDN-eindpunt uitschakelen en controleer of dat de aanvraag juist is overgezet naar het secundaire CDN-eindpunt. 

## <a name="next-steps"></a>Volgende stappen
U kunt ook instellen andere methoden voor het doorsturen, zoals geografische om taken te verdelen tussen verschillende CDN-eindpunten. Zie voor meer informatie, [configureren van de geografische verkeersrouteringsmethode met Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).



