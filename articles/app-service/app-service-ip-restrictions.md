---
title: Beperken van toegang tot - Azure App Service | Microsoft Docs
description: Beperkingen voor Computertoegang gebruiken met Azure App Service
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: d3c547fbc09aeb034df5b7ed579639e1ff4bc0b4
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705807"
---
# <a name="azure-app-service-access-restrictions"></a>Beperkingen voor Azure App Service-toegang #

Beperkingen voor Computertoegang kunnen u een prioriteit geordende toestaan/weigeren lijst waarmee de toegang tot het netwerk aan uw app te definiëren. De lijst kan bevatten, IP-adressen of subnetten van virtuele Azure-netwerk. Wanneer er een of meer vermeldingen zijn, is er vervolgens een impliciete 'Alles weigeren' die aan het einde van de lijst voorkomt.

De mogelijkheid toegangsbeperkingen werkt met alle App Service gehoste werk wordt geladen met inbegrip van; web-apps, API-apps, Linux-apps, Linux-container-apps en functies.

Wanneer een aanvraag wordt gedaan aan uw app, wordt het adres van de afzender geëvalueerd op basis van de regels van de IP-adres in de lijst met beperkingen voor toegang. Als het adres van de afzender in een subnet dat is geconfigureerd met service-eindpunten naar Microsoft.Web is, klikt u vervolgens het Bronsubnet vergeleken met de regels van het virtuele netwerk in de lijst met beperkingen voor toegang. Als het adres is niet toegestaan voor toegang op basis van de regels in de lijst, de service reageert met een [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) statuscode.

De mogelijkheid tot beperkingen is geïmplementeerd in de front-end-rollen van App Service, die zijn upstream van de werknemer hosts waarop uw code wordt uitgevoerd. Beperkingen voor Computertoegang is daarom nagenoeg netwerk ACL's.

De mogelijkheid toegang te beperken tot uw web-app uit een Azure Virtual Network (VNet) heet [service-eindpunten][serviceendpoints]. Service-eindpunten kunnen u tot een service met meerdere tenants in de geselecteerde subnetten beperkt. Dit moet worden ingeschakeld op zowel de zijde van de netwerken als de service die wordt ingeschakeld met. Het werkt niet als u wilt beperken het verkeer naar apps die worden gehost in een App Service Environment.  Als u zich in een App Service-omgeving, kunt u toegang tot uw app met IP-adresregels beheren.

![toegangsstroom voor beperkingen](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Toevoegen en bewerken van beperking van toegang door regels in de portal ##

Als u wilt een beperking toegangsregel toevoegen aan uw app, gebruikt u het menu te openen **netwerk**>**toegangsbeperkingen** en klikt u op **toegangsbeperkingen configureren**

![Netwerkopties van App Service](media/app-service-ip-restrictions/access-restrictions.png)  

U kunt vanuit de gebruikersinterface van de beperkingen voor toegang tot de lijst met regels voor toegang-beperking gedefinieerd voor uw app bekijken.

![lijst met toegangsbeperkingen](media/app-service-ip-restrictions/access-restrictions-browse.png)

Deze lijst wordt aangegeven dat alle van de huidige beperkingen die zich op uw app. Als u een VNet-beperking op uw app hebt, ziet u de tabel of service-eindpunten zijn ingeschakeld voor Microsoft.Web. Als er geen gedefinieerde beperkingen met betrekking tot uw app, wordt uw app vanaf elke locatie toegankelijk zijn.  

## <a name="adding-ip-address-rules"></a>IP-adresregels toe te voegen

U kunt klikken op **[+] toevoegen** om toe te voegen een nieuwe regel voor de beperking van toegang. Als u een regel toevoegt, wordt deze zijn onmiddellijk van kracht. Regels worden afgedwongen in volgorde van prioriteit begonnen met het laagste getal en neemt. Er is een impliciete weigeren die van kracht nadat u zelfs een enkele regel toegevoegd.

Wanneer u een regel maakt, moet u toestaan/weigeren en ook op het type regel selecteren. U zijn ook vereist voor de prioriteitswaarde en wat u bent beperkt de toegang tot.  U kunt desgewenst een naam en beschrijving toevoegen aan de regel.  

![een regel van de beperkingen van de IP-toegang toevoegen](media/app-service-ip-restrictions/access-restrictions-ip-add.png)

Om in te stellen van een IP-adres op basis van de regel, selecteer een type IPv4 of IPv6. Notatie van de IP-adres moet worden opgegeven in CIDR-notatie voor zowel IPv4 als IPv6-adressen. Als u wilt een exact adres opgeeft, kunt u er ongeveer als 1.2.3.4/32 waarin de eerste vier octetten vertegenwoordigen uw IP-adres en /32 is het masker. De IPv4-CIDR-notatie voor alle adressen is 0.0.0.0/0. Voor meer informatie over de CIDR-notatie, kunt u lezen [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). 

## <a name="service-endpoints"></a>Service-eindpunten

Service-eindpunten kunt u toegang tot geselecteerde Azure-netwerksubnetten te beperken. Om toegang te beperken voor een specifiek subnet, maakt u een regel voor het beperkingen met een type van het Virtueelnetwerk. U kunt het abonnement, VNet en subnet dat u wilt toestaan of weigeren van toegang met kiezen. Als service-eindpunten zijn nog niet ingeschakeld met Microsoft.Web voor het subnet dat u hebt geselecteerd, wordt deze automatisch ingeschakeld voor u, tenzij u het selectievakje gevraagd niet om dat te doen. De situatie waarin u wilt inschakelen op de app, maar niet het subnet is grotendeels met betrekking tot hebt u de machtigingen voor het inschakelen van service-eindpunten op het subnet of niet. Als u nodig hebt om op te halen van iemand anders om in te schakelen van service-eindpunten op het subnet, kunt u het selectievakje en uw App geconfigureerd voor service-eindpunten in afwachting van het later opnieuw wordt ingeschakeld op het subnet. 

![een VNet-regel voor toegang tot beperkingen toevoegen](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

Service-eindpunten kunnen niet worden gebruikt om toegang te beperken tot apps die worden uitgevoerd in een App Service Environment. Wanneer uw app zich in een App Service-omgeving, kunt u toegang tot uw app met IP-toegangsregels beheren. 

Service-eindpunten kunt u uw app met Application Gateways of andere apparaten WAF configureren. U kunt ook toepassingen met meerdere lagen met veilige back-ends. Lees voor meer informatie over enkele van de mogelijkheden [netwerkfuncties en App Service](networking-features.md).

## <a name="managing-access-restriction-rules"></a>Regels voor beperking van toegang beheren

U kunt klikken op een rij te bewerken van een bestaande regel voor de beperking van toegang. Bewerkingen zijn van kracht inclusief direct wijzigingen in volgorde van prioriteit.

![een toegangsregel beperking bewerken](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

Wanneer u een regel hebt bewerkt, kunt u het type tussen een regel voor IP-adres en een regel voor het Virtueelnetwerk niet wijzigen. 

![een toegangsregel beperking bewerken](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

Als u wilt een regel hebt verwijderd, klikt u op de **...**  op de regel en klik vervolgens op **verwijderen**.

![regel voor beperking van toegang verwijderen](media/app-service-ip-restrictions/access-restrictions-delete.png)

## <a name="blocking-a-single-ip-address"></a>Blokkering van één IP-adres ##

Bij het toevoegen van de eerste regel van de IP-beperking, de service wordt toegevoegd een expliciete **Alles weigeren** regel met een prioriteit van 2147483647. In de praktijk, de expliciete **Alles weigeren** regel worden de laatste regel uitgevoerd en blokkeert de toegang tot elk IP-adres dat niet expliciet is toegestaan met behulp van een **toestaan** regel.

Voor het scenario waar gebruikers wilt blokkeren expliciet een enkel IP-adres of IP-Adresblok, maar dat alles wat anders toegang, is het nodig zijn om toe te voegen een expliciete **Allow All** regel.

![blok één ip-adres](media/app-service-ip-restrictions/block-single-address.png)

## <a name="scm-site"></a>SCM-site 

Naast het beheren van toegang tot uw app, kunt u ook toegang tot de scm-site die worden gebruikt door uw app beperken. De scm-site is de web-eindpunt en ook de Kudu-console implementeren. U kunt afzonderlijk toegangsbeperkingen toewijst aan de scm-site uit de app of gebruik dezelfde set voor zowel de app en de scm-site. Wanneer u het selectievakje hebben de dezelfde beperkingen als uw app, wordt alles meetfout uit. Als u het selectievakje uitschakelt, worden de instellingen die u eerder had op de scm-site worden toegepast. 

![lijst met toegangsbeperkingen](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Programmatische manipulatie van beperking toegangsregels ##

Op dit moment is er geen CLI of PowerShell voor de nieuwe functie voor de toegangsbeperkingen, maar de waarden kunnen handmatig worden ingesteld met een [REST API van Azure](https://docs.microsoft.com/rest/api/azure/) PUT-bewerking op de app-configuratie in Resource Manager. U kunt bijvoorbeeld resources.azure.com gebruiken en bewerken van het blok ipSecurityRestrictions om toe te voegen van de vereiste JSON.

De locatie voor deze informatie in Resource Manager is:

management.azure.com/subscriptions/**subscription ID**/resourceGroups/**resource groups**/providers/Microsoft.Web/sites/**web app name**/config/web?api-version=2018-02-01

De JSON-syntaxis voor het vorige voorbeeld is:

    {
      "properties": {
        "ipSecurityRestrictions": [
          {
            "ipAddress": "122.133.144.0/24",
            "action": "Allow",
            "tag": "Default",
            "priority": 100,
            "name": "IP example rule"
          }
        ]
      }
    }

## <a name="function-app-ip-restrictions"></a>Functie-App-IP-beperkingen

IP-beperkingen zijn beschikbaar voor zowel functie-Apps met dezelfde functionaliteit als App Service-plannen. Inschakelen van IP-beperkingen, wordt de portal code-editor voor eventuele niet-toegestane IP-adressen uitgeschakeld.

[Hier voor meer informatie](../azure-functions/functions-networking-options.md#inbound-ip-restrictions)


<!--Links-->
[serviceendpoints]: https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview
