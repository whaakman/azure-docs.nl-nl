---
title: Beperken van de client-IP's - Azure App Service | Microsoft Docs
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
ms.date: 07/30/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 805de614246028bc75268e83991fa7831b990325
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882324"
---
# <a name="azure-app-service-static-access-restrictions"></a>Statische toegangsbeperkingen voor Azure App Service #

Beperkingen voor Computertoegang kunnen u een prioriteit geordende lijst toestaan/weigeren van IP-adressen die zijn toegestaan voor toegang tot uw app definiëren. De acceptatielijst kan IPv4 en IPv6-adressen bevatten. Wanneer er een of meer vermeldingen, is er een impliciete weigeren alle die aan het einde van de lijst voorkomt.

De mogelijkheid toegangsbeperkingen werkt met alle App Service die wordt gehost werkbelastingen, waaronder; web-apps, API-apps, Linux-apps, Linux-container-apps en functies.

Wanneer een aanvraag wordt gedaan aan uw app, wordt het van IP-adres geëvalueerd op basis van de lijst met beperkingen voor Computertoegang. Als het adres is niet toegestaan voor toegang op basis van de regels in de lijst, de service reageert met een [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) statuscode.

De mogelijkheid toegangsbeperkingen is geïmplementeerd in de front-end-rollen van App Service, die zijn upstream van de werknemer hosts waarop uw code wordt uitgevoerd. Beperkingen voor Computertoegang is daarom nagenoeg netwerk ACL's.  

![toegangsstroom voor beperkingen](media/app-service-ip-restrictions/ip-restrictions-flow.png)

Gedurende een periode is de mogelijkheid beperkingen voor Computertoegang in de portal voor een laag bovenop de mogelijkheid ipSecurity in IIS. De huidige beperkingen voor Computertoegang capaciteit is anders. U kunt nog steeds ipSecurity configureren binnen de web.config van uw toepassing, maar de front-end op basis van toegangsbeperkingen regels worden toegepast voordat u IIS al het verkeer wordt bereikt.

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Toevoegen en bewerken van beperking van toegang door regels in de portal ##

Als u wilt een beperking toegangsregel toevoegen aan uw app, gebruikt u het menu te openen **netwerk**>**toegangsbeperkingen** en klikt u op **toegangsbeperkingen configureren**

![Netwerkopties van App Service](media/app-service-ip-restrictions/ip-restrictions.png)  

U kunt vanuit de gebruikersinterface van de beperkingen voor toegang tot de lijst met regels voor toegang-beperking gedefinieerd voor uw app bekijken.

![lijst met toegangsbeperkingen](media/app-service-ip-restrictions/ip-restrictions-browse.png)

Als uw regels zijn geconfigureerd zoals in deze afbeelding, uw app accepteert alleen verkeer van 131.107.159.0/24 en van elk IP-adres worden geweigerd.

U kunt klikken op **[+] toevoegen** om toe te voegen een nieuwe regel voor de beperking van toegang. Als u een regel toevoegt, wordt deze zijn onmiddellijk van kracht. Regels worden afgedwongen in volgorde van prioriteit begonnen met het laagste getal en neemt. Er is een impliciete weigeren die van kracht nadat u zelfs een enkele regel toegevoegd.

![een regel voor toegang tot beperkingen toevoegen](media/app-service-ip-restrictions/ip-restrictions-add.png)

Notatie van de IP-adres moet worden opgegeven in CIDR-notatie voor zowel IPv4 als IPv6-adressen. Als u wilt een exact adres opgeeft, kunt u er ongeveer als 1.2.3.4/32 waarin de eerste vier octetten vertegenwoordigen uw IP-adres en /32 is het masker. De IPv4-CIDR-notatie voor alle adressen is 0.0.0.0/0. Voor meer informatie over de CIDR-notatie, kunt u lezen [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  

U kunt klikken op een rij te bewerken van een bestaande regel voor de beperking van toegang. Bewerkingen zijn van kracht inclusief direct wijzigingen in volgorde van prioriteit.

![een toegangsregel beperking bewerken](media/app-service-ip-restrictions/ip-restrictions-edit.png)

Als u wilt een regel hebt verwijderd, klikt u op de **...**  op de regel en klik vervolgens op **verwijderen**.

![regel voor beperking van toegang verwijderen](media/app-service-ip-restrictions/ip-restrictions-delete.png)

U kunt ook toegang tot de implementatie in het volgende tabblad beperken. Elke regel aan toevoegen/bewerken/verwijderen, volgt u dezelfde stap als hierboven.

![lijst met toegangsbeperkingen](media/app-service-ip-restrictions/ip-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Programmatische manipulatie van beperking toegangsregels ##

Op dit moment is er geen CLI of PowerShell voor de nieuwe functie voor de toegangsbeperkingen, maar de waarden kunnen handmatig worden ingesteld met een PUT-bewerking op de app-configuratie in Resource Manager. U kunt bijvoorbeeld resources.azure.com gebruiken en bewerken van het blok ipSecurityRestrictions om toe te voegen van de vereiste JSON.

De locatie voor deze informatie in Resource Manager is:

management.azure.com/subscriptions/**subscription ID**/resourceGroups/**resource groups**/providers/Microsoft.Web/sites/**web app name**/config/web?api-version=2018-02-01

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
