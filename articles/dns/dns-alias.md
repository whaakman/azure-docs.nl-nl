---
title: Azure DNS-server registreert alias overzicht
description: Overzicht van ondersteuning voor aliasrecords in Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 05a6a1700de2b8b334b40d9efd9b8d79e9e7241f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957556"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS-server registreert alias overzicht

Azure DNS-aliasrecords zijn kwalificatie op een DNS-record-set waarmee u om te verwijzen naar andere Azure-resources binnen uw DNS-zone. Bijvoorbeeld, kunt u een alias recordset maken die verwijst naar een Azure openbare IP-adres in plaats daarvan een A-record. Omdat de alias-recordset naar een service-exemplaar van Azure openbaar IP-adres dynamisch verwijst, de alias-Recordset zelf naadloos bijgewerkt tijdens de DNS-omzetting.

Een recordset alias wordt ondersteund voor de volgende recordtypen in Azure DNS-zone: A, AAAA en CNAME. 

> [!NOTE]
> Aliasrecords voor de A of AAAA recordtypen voor Traffic Manager worden alleen ondersteund voor Extern eindpunt typen. U moet het IPv4- of IPv6-adres (in het ideale geval statische IP-adressen) als geschikt is voor externe eindpunten in Traffic Manager opgeven.

## <a name="capabilities"></a>Functionaliteit

- **Wijs een openbaar IP-resource van een DNS-server A/AAAA-Recordset**. U kunt een A/AAAA-recordset maken en kunt u een recordset van de alias om te verwijzen naar een openbare IP-resource.
- **Verwijzen naar een Traffic Manager-profiel van een AAAA-DNS A/CNAME-Recordset**. Naast de mogelijkheid om te verwijzen naar de CNAME van een Traffic Manager-profiel (bijvoorbeeld: contoso.trafficmanager.net) van een DNS CNAME-recordset, u kunt nu ook verwijzen naar een Traffic Manager-profiel met externe eindpunten, van een A of AAAA-recordset in uw DNS zone.
   > [!NOTE]
   > Aliasrecords voor de A of AAAA recordtypen voor Traffic Manager worden alleen ondersteund voor Extern eindpunt typen. U moet het IPv4- of IPv6-adres (in het ideale geval statische IP-adressen) als geschikt is voor externe eindpunten in Traffic Manager opgeven.
- **Verwijzen naar een andere DNS-recordset binnen dezelfde zone**. Aliasrecords kunnen verwijzen naar aan de andere recordsets van hetzelfde type. U kunt bijvoorbeeld een DNS CNAME-recordset worden van een alias voor een andere CNAME-recordset van hetzelfde type hebben. Dit is handig als u wilt dat bepaalde recordsets aliassen worden en sommige als niet-aliassen in termen van gedrag.

## <a name="scenarios"></a>Scenario's
Er zijn enkele algemene scenario's voor aliasrecords:

### <a name="prevent-dangling-dns-records"></a>Voorkomen dat dangling DNS-records
Uit in Azure DNS-zones, aliasrecords kunnen worden gebruikt voor het nauwkeurig bijhouden van de levenscyclus van Azure-resources, zoals een openbaar IP-adres en Traffic Manager-profiel. Een van de veelvoorkomende problemen met traditionele DNS-records is 'dangling records', met name met A/AAAA of CNAME-recordtypen. 

Met een traditionele DNS-zone-record, als de doel-IP-adres of de CNAME niet meer bestaat, de record van DNS-zone heeft geen kennis mee en moet handmatig worden bijgewerkt. In sommige organisaties deze handmatige update niet kan gebeuren in de tijd of kan problematisch zijn vanwege de scheiding van functies en bijbehorende machtigingsniveaus.

De rol die gemachtigd is om te verwijderen van een CNAME- of IP-adres die behoren tot een toepassing kan bijvoorbeeld geen voldoende instantie van de DNS-record die naar deze doelen verwijst bij te werken. Als gevolg hiervan kunnen er een vertraging tussen wanneer het IP-adres of de CNAME wordt verwijderd en de DNS-record die verwijst naar het wordt verwijderd, kan dat een storing veroorzaken voor eindgebruikers.

Aliasrecords verwijderen van de complexiteit die zijn gekoppeld aan dit scenario en te voorkomen dat dergelijke dangling verwijzingen. Wanneer een DNS-record als een alias-record is gekwalificeerd om te verwijzen naar een openbaar IP-adres of een Traffic Manager-profiel, en als de onderliggende resources zijn verwijderd, wordt ook de DNS-alias-record verwijderd op hetzelfde moment. Dit zorgt ervoor dat de eindgebruikers lijden nooit een storing.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>DNS-zones automatisch bijgewerkt wanneer de toepassing IP-adressen wijzigen

Net als bij het voorgaande scenario, als een toepassing wordt verplaatst of als de onderliggende virtuele machine opnieuw wordt opgestart, een alias-record wordt automatisch bijgewerkt wanneer het IP-adres voor de onderliggende resources voor openbaar IP-adres wordt gewijzigd. Als eindgebruikers worden doorgestuurd naar een andere toepassing die het oude IP-adres heeft, kunt u potentiële beveiligingsrisico's voorkomen.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Toepassingen in de apex van de zone met gelijke taakverdeling host

Het DNS-protocol wordt voorkomen dat de toewijzing van iets anders dan een A of AAAA-record in de apex van de zone (bijvoorbeeld: contoso.com). Dit geeft een probleem voor toepassingseigenaren van een die beschikken over load balance toepassingen achter een Traffic Manager, omdat deze niet mogelijk om te verwijzen naar de Traffic Manager-profiel van de zone apexrecord. Als gevolg hiervan toepassingseigenaren moest een tijdelijke oplossing gebruiken. Bijvoorbeeld, een omleiding op het niveau van de toepassing zodat in het toppunt van de zone worden omgeleid naar een ander domein (van contoso.com naar www.contoso.com). Dit biedt een single point of failure in termen van de omleidings-functionaliteit.

Dit probleem is met aliasrecords, niet meer bestaat. Toepassingseigenaren van kunnen nu hun apexrecord zone verwijzen naar een Traffic Manager-profiel met externe eindpunten. Met deze functie kunnen toepassingseigenaren verwijzen naar dezelfde Traffic Manager-profiel dat wordt gebruikt voor andere domeinen in hun DNS-zone. Bijvoorbeeld: contoso.com en www.contoso.com kunnen verwijzen beide naar dezelfde Traffic Manager-profiel, zolang de Traffic Manager-profiel alleen externe eindpunten zijn geconfigureerd heeft.

## <a name="next-steps"></a>Volgende stappen

Als u wilt meer lean over aliasrecords, raadpleegt u de follwing-artikelen:

- [Zelfstudie: Een alias-record om te verwijzen naar een openbare Azure-IP-adres configureren](tutorial-alias-pip.md)
- [Zelfstudie: Een alias-record ter ondersteuning van het toppunt van domeinnamen met Traffic Manager configureren](tutorial-alias-tm.md)
- [Veelgestelde vragen over DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
