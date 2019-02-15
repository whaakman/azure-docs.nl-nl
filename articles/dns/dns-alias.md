---
title: Azure DNS-server registreert alias overzicht
description: Overzicht van ondersteuning voor aliasrecords in Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 52653252df3efd3e12fa974ed82cd2557eee93d0
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301244"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS-server registreert alias overzicht

Azure DNS-alias records zijn kwalificaties op een DNS-Recordset. Ze kunnen verwijzen naar andere Azure-resources binnen uw DNS-zone. U kunt bijvoorbeeld een alias-record-set die verwijst naar een openbaar IP-adres van Azure in plaats van een A-record maken. Uw alias-record set verwijst naar een Azure openbaar IP-adres dynamisch service-exemplaar. Als gevolg hiervan de alias-Recordset naadloos bijgewerkt tijdens de DNS-omzetting.

Een recordset alias wordt ondersteund voor de volgende recordtypen in Azure DNS-zone: 

- A 
- AAAA 
- CNAME 

> [!NOTE]
> Aliasrecords voor de A of AAAA recordtypen voor Azure Traffic Manager worden alleen ondersteund voor extern eindpunttypen. U moet het IPv4- of IPv6-adres, waar nodig, voor externe eindpunten in Traffic Manager opgeven. In het ideale geval gebruik van statische IP-adressen voor het adres.

## <a name="capabilities"></a>Functionaliteit

- **Wijs een openbaar IP-resource van een DNS-server A/AAAA-Recordset.** U kunt een A/AAAA-recordset maken en geef deze een alias recordset om te verwijzen naar een openbare IP-resource.

- **Verwijzen naar een Traffic Manager-profiel van een AAAA-DNS A/CNAME-Recordset.** U kunt verwijzen naar de CNAME van een Traffic Manager-profiel van een DNS CNAME-Recordset. Een voorbeeld is contoso.trafficmanager.net. Nu kunt u ook verwijzen naar een Traffic Manager-profiel met externe eindpunten van een A of AAAA-recordset in de DNS-zone.

   > [!NOTE]
   > Aliasrecords voor de A of AAAA recordtypen voor Traffic Manager worden alleen ondersteund voor extern eindpunttypen. U moet het IPv4- of IPv6-adres, waar nodig, voor externe eindpunten in Traffic Manager opgeven. In het ideale geval gebruik van statische IP-adressen voor het adres.
   
- **Verwijzen naar een andere DNS-recordset binnen dezelfde regio.** Aliasrecords kunnen naar andere recordsets van hetzelfde type verwijzen. Een DNS CNAME-recordset kan bijvoorbeeld een alias aan een andere CNAME-recordset van hetzelfde type zijn. Deze benadering is handig als u wilt dat aantal recordsets zijn aliassen en enkele niet-aliassen.

## <a name="scenarios"></a>Scenario's
Er zijn enkele algemene scenario's voor records van de Alias.

### <a name="prevent-dangling-dns-records"></a>Voorkomen dat dangling DNS-records
 In Azure DNS-zones, kunnen de aliasrecords nauw samen om bij te houden de levenscyclus van Azure-resources worden gebruikt. Resources omvatten een openbaar IP-adres of een Traffic Manager-profiel. Een veelvoorkomend probleem met traditionele DNS-records is dangling records. Dit probleem doet zich met name met A/AAAA of CNAME-recordtypen. 

Met een traditionele DNS-zone-record, als de doel-IP-adres of de CNAME niet meer bestaat, weet de record van DNS-zone niet. Als gevolg hiervan moet de record handmatig worden bijgewerkt. In sommige organisaties, kan dit handmatig bijwerken niet is gebeurd in-time. Deze kan ook problematisch zijn vanwege de scheiding van functies en bijbehorende machtigingsniveaus.

Een rol kan bijvoorbeeld de instantie voor het verwijderen van een CNAME- of IP-adres die deel uitmaakt van een toepassing hebben. Maar dit hoeft niet voldoende instantie van de DNS-record die naar deze doelen verwijst bij te werken. Een vertraging optreedt tussen wanneer het IP-adres of de CNAME wordt verwijderd en de DNS-record die naar het verwijst wordt verwijderd. Deze vertraging veroorzaken een storing voor gebruikers.

Aliasrecords verwijdert de complexiteit die zijn gekoppeld aan dit scenario. Ze helpen om te voorkomen dat dangling verwijzingen. Neem bijvoorbeeld een DNS-record die als een alias-record gekwalificeerd om te verwijzen naar een openbaar IP-adres of een Traffic Manager-profiel. Als de onderliggende resources zijn verwijderd, wordt de DNS-alias-record verwijderd op hetzelfde moment. Dit proces zorgt ervoor dat gebruikers nooit een storing afnemen.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>DNS-zones automatisch bijgewerkt wanneer de toepassing IP-adressen wijzigen

In dit scenario is vergelijkbaar met het vorige voorbeeld. Misschien een toepassing wordt verplaatst, of de onderliggende virtuele machine opnieuw wordt opgestart. Een alias-record vervolgens automatisch bijgewerkt wanneer het IP-adres verandert voor de onderliggende openbare IP-adresresource. Hiermee voorkomt u mogelijke beveiligingsrisico's van het doorsturen van gebruikers naar een andere toepassing die het oude IP-adres heeft.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Met load balancing hosting van toepassingen in de apex van de zone

Het DNS-protocol wordt voorkomen dat de toewijzing van iets anders dan een A of AAAA-record in de apex van de zone. Bijvoorbeeld: contoso.com. Deze beperking geeft een probleem voor toepassingseigenaren van een die beschikken over Netwerktaakverdeling toepassingen achter Traffic Manager. Het is niet mogelijk om te verwijzen naar de Traffic Manager-profiel van de zone apexrecord. Als gevolg hiervan moeten toepassingseigenaren een tijdelijke oplossing gebruiken. Een omleiding op het niveau van de toepassing moet van het toppunt van de zone omleiden naar een ander domein. Een voorbeeld is een omleiding van contoso.com naar www.contoso.com. Deze overeenkomst geeft een single point of failure voor de omleidingsfunctie.

Dit probleem is met aliasrecords, niet meer bestaat. Toepassingseigenaren van kunnen nu hun apexrecord zone verwijzen naar een Traffic Manager-profiel met externe eindpunten. Toepassingseigenaren van de kunnen verwijzen naar dezelfde Traffic Manager-profiel dat wordt gebruikt voor andere domeinen in hun DNS-zone. Bijvoorbeeld: contoso.com en www.contoso.com kunnen verwijzen naar dezelfde Traffic Manager-profiel. Dit geldt zolang het Traffic Manager-profiel alleen externe eindpunten zijn geconfigureerd heeft.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de aliasrecords, raadpleegt u de volgende artikelen:

- [Zelfstudie: Een alias-record om te verwijzen naar een openbaar IP-adres van Azure configureren](tutorial-alias-pip.md)
- [Zelfstudie: Een alias-record ter ondersteuning van het toppunt van domeinnamen met Traffic Manager configureren](tutorial-alias-tm.md)
- [Veelgestelde vragen over DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
