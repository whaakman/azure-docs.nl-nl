---
title: Azure DNS-server registreert alias overzicht
description: Overzicht van ondersteuning voor aliasrecords in Microsoft Azure DNS.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 2/20/2019
ms.author: victorh
ms.openlocfilehash: 1f6d6b2ae5fd3a0c08d37b93c73656ac6bb71d67
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295637"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS-server registreert alias overzicht

Azure DNS-alias records zijn kwalificaties op een DNS-Recordset. Ze kunnen verwijzen naar andere Azure-resources binnen uw DNS-zone. U kunt bijvoorbeeld een alias-record-set die verwijst naar een openbaar IP-adres van Azure in plaats van een A-record maken. Uw alias-record set verwijst naar een Azure openbaar IP-adres dynamisch service-exemplaar. Als gevolg hiervan de alias-Recordset naadloos bijgewerkt tijdens de DNS-omzetting.

Een recordset alias wordt ondersteund voor de volgende recordtypen in Azure DNS-zone: 

- A 
- AAAA 
- CNAME 

> [!NOTE]
> Als u van plan bent een alias-record voor de recordtypen A of AAAA gebruiken om te verwijzen naar een [Azure Traffic Manager-profiel](../traffic-manager/quickstart-create-traffic-manager-profile.md) moet u ervoor zorgen dat de Traffic Manager-profiel slechts heeft [externe eindpunten](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints). U moet het IPv4- of IPv6-adres opgeven voor externe eindpunten in Traffic Manager. In het ideale geval gebruik statische IP-adressen.

## <a name="capabilities"></a>Functionaliteit

- **Wijs een openbaar IP-resource van een DNS-server A/AAAA-Recordset.** U kunt een A/AAAA-recordset maken en geef deze een alias recordset om te verwijzen naar een openbare IP-resource. De DNS-recordset wordt automatisch als het openbare IP-adres wordt gewijzigd of is verwijderd. Dangling DNS worden-records die naar onjuist IP-adressen verwijzen vermeden.

- **Verwijzen naar een Traffic Manager-profiel van een AAAA-DNS A/CNAME-Recordset.** U kunt een A/AAAA maken of CNAME-record ingesteld en aliasrecords gebruiken om te verwijzen naar een Traffic Manager-profiel. Dit is vooral nuttig wanneer u nodig hebt voor het routeren van verkeer in het toppunt van een zone als traditionele CNAME-records worden niet ondersteund voor het toppunt van een zone. Stel bijvoorbeeld dat uw Traffic Manager-profiel is myprofile.trafficmanager.net en uw zakelijke DNS-zone contoso.com. U kunt een alias recordset van type A/AAAA voor contoso.com (het toppunt van de zone) maken en verwijzen naar myprofile.trafficmanager.net.

- **Verwijzen naar een andere DNS-recordset binnen dezelfde regio.** Aliasrecords kunnen naar andere recordsets van hetzelfde type verwijzen. Een DNS CNAME-Recordset mag bijvoorbeeld een alias aan een andere CNAME-Recordset. Deze benadering is handig als u wilt dat aantal recordsets zijn aliassen en enkele niet-aliassen.

## <a name="scenarios"></a>Scenario's

Er zijn enkele algemene scenario's voor records van de Alias.

### <a name="prevent-dangling-dns-records"></a>Voorkomen dat dangling DNS-records

Een veelvoorkomend probleem met traditionele DNS-records is dangling records. Bijvoorbeeld, DNS-records die niet zijn bijgewerkt als gevolg van wijzigingen in IP-adressen. Dit probleem doet zich met name met A/AAAA of CNAME-recordtypen.

Met een traditionele DNS-zone-record, als de doel-IP-adres of de CNAME niet meer bestaat, moet de DNS-record die zijn gekoppeld aan dit handmatig worden bijgewerkt. In sommige organisaties heeft mogelijk een handmatige update niet is gebeurd in de tijd vanwege problemen met de procedure of als gevolg van de scheiding van functies en bijbehorende machtigingsniveaus. Een rol kan bijvoorbeeld de instantie voor het verwijderen van een CNAME- of IP-adres die deel uitmaakt van een toepassing hebben. Maar dit hoeft niet voldoende instantie van de DNS-record die naar deze doelen verwijst bij te werken. Een vertraging bij het bijwerken van de DNS-record kan mogelijk leiden tot een onderbreking van de gebruikers.

Aliasrecords voorkomen dangling verwijzingen door nauw koppeling van de levenscyclus van een DNS-record met een Azure-resource. Neem bijvoorbeeld een DNS-record die als een alias-record gekwalificeerd om te verwijzen naar een openbaar IP-adres of een Traffic Manager-profiel. Als de onderliggende resources zijn verwijderd, wordt de DNS-alias-record verwijderd op hetzelfde moment.

### <a name="update-dns-record-set-automatically-when-application-ip-addresses-change"></a>DNS-record-set automatisch bijgewerkt wanneer de toepassing IP-adressen wijzigen

In dit scenario is vergelijkbaar met het vorige voorbeeld. Misschien een toepassing wordt verplaatst, of de onderliggende virtuele machine opnieuw wordt opgestart. Een alias-record vervolgens automatisch bijgewerkt wanneer het IP-adres verandert voor de onderliggende openbare IP-adresresource. Hiermee voorkomt u mogelijke beveiligingsrisico's sturen de gebruikers naar een andere toepassing die het oude IP-adres is toegewezen.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Met load balancing hosting van toepassingen in de apex van de zone

Het DNS-protocol wordt voorkomen dat de toewijzing van CNAME-records in de apex van de zone. Als uw domein contoso.com, bijvoorbeeld u kunt de CNAME-records voor somelable.contoso.com; maken maar u kunt geen CNAME maken voor contoso.com zelf.
Deze beperking geeft een probleem voor toepassingseigenaren van een die beschikken over Netwerktaakverdeling toepassingen achter [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Omdat het maken van een CNAME-record met behulp van een Traffic Manager-profiel worden vereist, is het niet mogelijk om te verwijzen naar de Traffic Manager-profiel in het toppunt van de zone.

Dit probleem kan worden opgelost met aliasrecords. In tegenstelling tot de CNAME-records, aliasrecords kunnen worden gemaakt in de apex van de zone en toepassingseigenaren kunnen deze gebruiken om hun apexrecord zone verwijzen naar een Traffic Manager-profiel met externe eindpunten. Toepassingseigenaren van de kunnen verwijzen naar dezelfde Traffic Manager-profiel dat wordt gebruikt voor andere domeinen in hun DNS-zone.

Bijvoorbeeld: contoso.com en een www\.contoso.com kan verwijzen naar dezelfde Traffic Manager-profiel. Voor meer informatie over het gebruik van de aliasrecords met Azure Traffic Manager-profielen, Zie het gedeelte volgende stappen.

### <a name="point-zone-apex-to-azure-cdn-endpoints"></a>Wijs het toppunt van zone Azure CDN-eindpunten
Net als bij een Traffic Manager-profiel, kunt u ook aliasrecords gebruiken om te verwijzen de apex van de DNS-zone naar Azure CDN-eindpunten. Dit is handig als u statische website met Azure storage en Azure CDN te maken. U kunt vervolgens toegang tot de website zonder: toevoeging van 'www' aan de DNS-naam.

Bijvoorbeeld, als uw statische website met de naam www.contoso.com, uw gebruikers kunnen toegang tot uw site met behulp van contoso.com zonder de noodzaak www aan de DNS-naam toevoegen aan het begin.

Zoals eerder beschreven, wordt CNAME-records worden niet ondersteund in de apex van de zone. U kunt een CNAME-record dus niet gebruiken voor het contoso.com verwijzen naar uw CDN-eindpunt. In plaats daarvan kunt u een alias-record toppunt van de zone rechtstreeks verwijzen naar een CDN-eindpunt.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over de aliasrecords, raadpleegt u de volgende artikelen:

- [Zelfstudie: Een alias-record om te verwijzen naar een openbaar IP-adres van Azure configureren](tutorial-alias-pip.md)
- [Zelfstudie: Een alias-record ter ondersteuning van het toppunt van domeinnamen met Traffic Manager configureren](tutorial-alias-tm.md)
- [Veelgestelde vragen over DNS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
