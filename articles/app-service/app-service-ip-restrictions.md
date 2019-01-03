---
title: Beperken van de client-IP's - Azure App Service | Microsoft Docs
description: IP-beperkingen gebruiken met Azure App Service
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
ms.date: 07/30/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 337d71c84ace7f44c2668cf2344d9083c4a85bee
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53651059"
---
# <a name="azure-app-service-static-ip-restrictions"></a>Statische IP-beperkingen voor Azure App Service #

IP-beperkingen kunt u een prioriteit geordende lijst toestaan/weigeren van IP-adressen die zijn toegestaan voor toegang tot uw app te definiëren. De acceptatielijst kan IPv4 en IPv6-adressen bevatten. Wanneer er een of meer vermeldingen, is er een impliciete weigeren alle die aan het einde van de lijst voorkomt. 

De mogelijkheid IP-beperkingen werkt met alle App Service die wordt gehost werkbelastingen, waaronder; web-apps, API-apps, Linux-apps, Linux-container-apps en functies. 

Wanneer een aanvraag wordt gedaan aan uw app, wordt het van IP-adres geëvalueerd op basis van de lijst met IP-beperkingen. Als het adres is niet toegestaan voor toegang op basis van de regels in de lijst, de service reageert met een [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) statuscode.

De mogelijkheid IP-beperkingen is geïmplementeerd in de front-end-rollen van App Service, die zijn upstream van de werknemer hosts waarop uw code wordt uitgevoerd. IP-beperkingen zijn daarom effectief netwerk ACL's.  

![IP-beperkingen voor stroom](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Gedurende een periode is de mogelijkheid IP-beperkingen in de portal voor een laag bovenop de mogelijkheid ipSecurity in IIS. De huidige beperkingen voor IP-capaciteit is anders. U kunt nog steeds ipSecurity configureren binnen de web.config van uw toepassing, maar de front-end op basis van IP-beperkingen regels worden toegepast voordat IIS al het verkeer wordt bereikt.

## <a name="adding-and-editing-ip-restriction-rules-in-the-portal"></a>Toevoegen en bewerken van IP-beperking regels in de portal ##

Een regel voor het IP-beperkingen toevoegen aan uw app, gebruikt u het menu te openen **netwerk**>**IP-beperkingen** en klikt u op **IP-beperkingen configureren**

![Netwerkopties van App Service](media/app-service-ip-restrictions/ip-restrictions.png)  

Vanaf de gebruikersinterface van de IP-beperkingen, kunt u de lijst met IP-beperking regels gedefinieerd voor uw app bekijken.

![lijst met IP-beperkingen](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Als uw regels zijn geconfigureerd zoals in deze afbeelding, uw app accepteert alleen verkeer van 131.107.159.0/24 en van elk IP-adres worden geweigerd.

U kunt klikken op **[+] toevoegen** om toe te voegen een nieuwe regel voor het IP-beperkingen. Als u een regel toevoegt, wordt deze zijn onmiddellijk van kracht. Regels worden afgedwongen in volgorde van prioriteit begonnen met het laagste getal en neemt. Er is een impliciete weigeren die van kracht nadat u zelfs een enkele regel toegevoegd. 

![een regel voor het IP-beperking toevoegen](media/app-service-ip-restrictions/ip-restrictions-add.png)

Notatie van de IP-adres moet worden opgegeven in CIDR-notatie voor zowel IPv4 als IPv6-adressen. Als u wilt een exact adres opgeeft, kunt u er ongeveer als 1.2.3.4/32 waarin de eerste vier octetten vertegenwoordigen uw IP-adres en /32 is het masker. De IPv4-CIDR-notatie voor alle adressen is 0.0.0.0/0. Voor meer informatie over de CIDR-notatie, kunt u lezen [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  

U kunt klikken op een rij aan een bestaande regel voor het IP-beperkingen bewerken. Bewerkingen zijn van kracht inclusief direct wijzigingen in volgorde van prioriteit.

![een regel voor het IP-beperkingen bewerken](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Als u wilt een regel hebt verwijderd, klikt u op de **...**  op de regel en klik vervolgens op **verwijderen**. 

![regel voor het IP-beperkingen verwijderen](media/app-service-ip-restrictions/ip-restrictions-delete.png)

## <a name="programmatic-manipulation-of-ip-restriction-rules"></a>Programmatische manipulatie van IP-beperking regels ##

Er is momenteel geen CLI of PowerShell voor de nieuwe functie van de IP-beperkingen, maar de waarden kunnen handmatig worden ingesteld met een PUT-bewerking op de app-configuratie in Resource Manager. U kunt bijvoorbeeld resources.azure.com gebruiken en bewerken van het blok ipSecurityRestrictions om toe te voegen van de vereiste JSON. 

De locatie voor deze informatie in Resource Manager is:

Management.Azure.com/subscriptions/**abonnements-ID**/resourceGroups/**resourcegroepen**/providers/Microsoft.Web/sites/**web-appnaam**  /config/website? API-version = 2018-02-01

De JSON-syntaxis voor het vorige voorbeeld is:

    "ipSecurityRestrictions": [
      {
        "ipAddress": "131.107.159.0/24",
        "action": "Allow",
        "tag": "Default",
        "priority": 100,
        "name": "allowed access"
      }
    ],
