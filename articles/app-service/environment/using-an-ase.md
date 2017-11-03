---
title: Gebruik van een Azure App Service-omgeving
description: Het maken, publiceren en schalen van apps in een Azure App Service-omgeving
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 64746f7b1a09e35b35e794f5a11d69bef39a03a0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-an-app-service-environment"></a>Gebruik van een App Service-omgeving #

## <a name="overview"></a>Overzicht ##

Azure App Service-omgeving is een implementatie van Azure App Service in een subnet in virtuele Azure-netwerk van een klant. Deze bestaat uit:

- **FrontPage-ends**: de front-ends zijn waar HTTP/HTTPS wordt beëindigd in een App Service-omgeving (as-omgeving).
- **Werknemers**: de werknemers zijn de resources die uw apps te hosten.
- **Database**: de database bevat informatie die de omgeving bepaalt.
- **Opslag**: de opslag wordt gebruikt voor het hosten van de klant gepubliceerde apps.

> [!NOTE]
> Er zijn twee versies van App Service-omgeving: ASEv1 en ASEv2. ASEv1, moet u de resources beheren voordat u ze kunt gebruiken. Zie voor informatie over het configureren en beheren van ASEv1, [configureren van een App Service-omgeving v1][ConfigureASEv1]. De rest van dit artikel is gericht op ASEv2.
>
>

U kunt een as-omgeving (ASEv1 en ASEv2) implementeren met een externe of interne VIP voor toegang tot Apps. De implementatie met een externe VIP wordt gewoonlijk aangeduid met een externe as-omgeving. De interne versie heet de as ILB-omgeving omdat deze gebruikmaakt van een interne load balancer (ILB). Zie voor meer informatie over de as ILB-omgeving, [maken en gebruiken een ILB-as-omgeving][MakeILBASE].

## <a name="create-a-web-app-in-an-ase"></a>Een web-app maken in een as-omgeving ##

Voor het maken van een web-app in een as-omgeving, gebruiken hetzelfde proces als wanneer u normaal gesproken maakt, maar met een paar kleine verschillen. Wanneer u een nieuwe App Service-abonnement maken:

- In plaats van een geografische locatie in waarop uw app implementeren, u een as-omgeving als uw locatie kiest te kiezen.
- Alle App Service-abonnementen gemaakt in een as-omgeving moeten zich in een geïsoleerd prijscategorie.

Als u een as-omgeving hebt, kunt u een door de instructies in [maken van een App Service-omgeving][MakeExternalASE].

Een web-app maken in een as-omgeving:

1. Selecteer **nieuwe** > **Web en mobiel** > **Web-App**.

2. Voer een naam voor de web-app. Als u al een App Service-abonnement in een as-omgeving hebt geselecteerd, geeft de domeinnaam voor de app de domeinnaam van de as-omgeving.

    ![Web-app-naam selecteren][1]

3. Selecteer een abonnement.

4. Geef een naam voor een nieuwe resourcegroep of selecteer **gebruik bestaande** en selecteer een optie uit de vervolgkeuzelijst.

5. Selecteer een bestaand App Service-abonnement in uw as-omgeving of een nieuwe maken met de volgende stappen:

    a. Selecteer **maken van nieuwe**.

    b. Voer de naam van uw App Service-plan.

    c. Selecteer uw as-omgeving in de **locatie** vervolgkeuzelijst.

    d. Selecteer een **geïsoleerd** prijscategorie. Selecteer **Selecteer**.

    e. Selecteer **OK**.
    
    ![Geïsoleerde Prijscategorieën][2]

6. Selecteer **Maken**.

## <a name="how-scale-works"></a>Hoe werkt schalen ##

Elke App Service-app wordt uitgevoerd in een App Service-plan. Het model van de container is omgevingen Houd App Service-abonnementen en App Service-abonnementen Houd apps. Wanneer u een app schalen, kunt u de App Service-abonnement schalen en schalen dus alle apps in hetzelfde abonnement.

In ASEv2, wanneer u een App Service-abonnement schaal wordt de benodigde infrastructuur automatisch toegevoegd. Er is een vertraging te schaalbewerkingen terwijl de infrastructuur wordt toegevoegd. In ASEv1, kan de benodigde infrastructuur moet worden toegevoegd voordat u kunt maken of uitbreiden van uw App Service-abonnement. 

In de App Service multitenant is schalen meestal onmiddellijke omdat een groep die direct beschikbaar is voor de ondersteuning. Er is geen dergelijke buffer in een as-omgeving, en bronnen worden toegewezen na nodig.

In een as-omgeving, kunt u maximaal 100 exemplaren schalen. De instanties van 100 kunnen alles in één enkele App Service-abonnement of verdeeld over meerdere App Service-abonnementen.

## <a name="ip-addresses"></a>IP-adressen ##

App Service biedt de mogelijkheid om toe te wijzen een toegewijde IP-adres aan een app. Deze mogelijkheid is beschikbaar nadat u een SSL op IP-basis geconfigureerd zoals beschreven in [een bestaande aangepaste SSL-certificaat binden aan Azure-web-apps][ConfigureSSL]. In een as-omgeving is er echter een opmerkelijke uitzondering. U kunt extra IP-adressen moet worden gebruikt voor een IP-gebaseerde SSL in een ILB-as-omgeving niet toevoegen.

In ASEv1 moet u de IP-adressen toewijzen als resources voordat u ze kunt gebruiken. In ASEv2 gebruikt u deze van uw app net als in de multitenant-App Service. Er is altijd één spare adres in ASEv2 maximaal 30 IP-adressen. Telkens wanneer is u een gebruikt, een andere toegevoegd, zodat een adres altijd direct beschikbaar voor gebruik is. Een moment vertraging vereist is voor het toewijzen van een ander IP-adres, waardoor het toevoegen van IP-adressen snel achter elkaar.

## <a name="front-end-scaling"></a>Front-schaling ##

In ASEv2, wanneer u uw App Service-abonnementen worden uitgebreid worden werknemers automatisch toegevoegd om ze te ondersteunen. Elke as-omgeving met twee front-ends gemaakt. Bovendien eindigt de voorgrond automatisch scale-out met een snelheid van één front-end voor elke 15 exemplaren in uw App Service-abonnementen. Bijvoorbeeld, als u 15 exemplaren hebt, hebt u drie front-ends. Als u op 30 exemplaren schalen, hebt u vier FrontPage-ends, enzovoort.

Dit aantal front-ends moet hetgeen ruim voldoende is voor de meeste scenario's. Echter, kunt u uitschalen sneller. U kunt de verhouding tussen zo laag één front end voor elke vijf exemplaren wijzigen. Er is een kosten voor het wijzigen van de verhouding tussen het. Zie voor meer informatie [prijzen voor Azure App Service][Pricing].

Front-resources zijn het HTTP/HTTPS-eindpunt voor de as-omgeving. Geheugengebruik per front-end is met de front-standaardconfiguratie consistent ongeveer 60 procent. Klant-werkbelastingen uitvoeren niet op een front-end. De belangrijkste factor voor een front-end met betrekking tot het schalen is de CPU, is vooral als gevolg van HTTPS-verkeer.

## <a name="app-access"></a>App-toegang ##

In een externe as-omgeving verschilt het domein dat wordt gebruikt wanneer u apps maken die van de multitenant-App Service. Het bevat de naam van de as-omgeving. Zie voor meer informatie over het maken van een externe as-omgeving [maken van een App Service-omgeving][MakeExternalASE]. De naam van het domein in een externe as-omgeving ziet eruit als *.&lt; asename&gt;. p.azurewebsites.net*. Bijvoorbeeld, als de naam van uw as-omgeving _externe-as-omgeving_ en hosten van een app aangeroepen _contoso_ in dat as-omgeving, u dit op de volgende URL's bereikt:

- Contoso.external ase.p.azurewebsites.net
- Contoso.SCM.external ase.p.azurewebsites.net

De URL contoso.scm.external-ase.p.azurewebsites.net wordt gebruikt voor toegang tot de Kudu-console of voor het publiceren van uw app met behulp van web implementeren. Zie voor informatie over de Kudu-console, [Kudu-console voor Azure App Service][Kudu]. De Kudu-console biedt u een webgebruikersinterface voor foutopsporing, het uploaden van bestanden, bestanden en nog veel meer bewerken.

In een ILB as-omgeving bepaalt u het domein tijdens de implementatie. Zie voor meer informatie over het maken van een as-omgeving voor de ILB [maken en gebruiken een ILB-as-omgeving][MakeILBASE]. Als u de domeinnaam opgeven _ilb ase.info_, gebruiken de apps in die as-omgeving dat domein tijdens het maken van de app. Voor de app met de naam _contoso_, wordt de URL's zijn:

- Contoso.ILB ase.info
- Contoso.SCM.ILB ase.info

## <a name="publishing"></a>Publiceren ##

Net als bij de multitenant-App Service in een as-omgeving kunt u publiceren met:

- Web-implementatie.
- FTP.
- Continue integratie.
- Slepen en neerzetten in de Kudu-console.
- Een IDE, zoals Visual Studio, Eclipse of IntelliJ IDEA.

Met een externe as-omgeving werken op dezelfde manier deze alle publicatieopties. Zie voor meer informatie [-implementatie in Azure App Service][AppDeploy]. 

Het belangrijkste verschil met het publiceren is ten opzichte van een as ILB-omgeving. De publicerende eindpunten zijn met een ILB as-omgeving, alle beschikbare alleen via de ILB. De ILB is op een privé IP-adres in het subnet van de as-omgeving in het virtuele netwerk. Als u geen toegang tot de ILB netwerk hebt, kunt u alle apps op die as-omgeving niet publiceren. Zoals vermeld in [maken en gebruiken een ILB-as-omgeving][MakeILBASE], moet u DNS configureren voor de apps in het systeem. Dat betekent onder meer het SCM-eindpunt. Als ze zijn niet correct gedefinieerd, kunt u niet publiceren. Er moet ook uw IDE netwerktoegang hebben tot de ILB om te kunnen publiceren naar deze.

Internetgebaseerde CI systemen, zoals GitHub en Visual Studio Team Services, werkt niet met een ILB-as-omgeving omdat het publishing eindpunt niet Internet toegankelijk is. In plaats daarvan moet u een CI-besturingssysteem dat gebruikmaakt van een pull-model, zoals Dropbox gebruiken.

Het domein dat de as ILB-omgeving is gemaakt met de publicatie eindpunten voor apps in een ILB-as-omgeving gebruiken Kunt u deze bekijken in het profiel voor het publiceren van de app en de portalblade van de app (in **overzicht** > **Essentials** en ook in **eigenschappen**). 

## <a name="pricing"></a>Prijzen ##

De prijzen SKU aangeroepen **geïsoleerd** is gemaakt voor gebruik met ASEv2. Alle App Service-abonnementen die worden gehost in ASEv2 zijn in de SKU-prijzen geïsoleerd. Geïsoleerde App Service plan tarieven kunnen verschillen per regio. 

Naast de prijs voor uw App Service-abonnementen is er een vast tarief voor de as-omgeving zelf. De snelheid platte verandert niet met de grootte van uw as-omgeving en betaalt voor de infrastructuur as-omgeving op een standaard frequentie van 1 aanvullende schalen front-voor elke 15 exemplaren van de App Service-plan.  

Als de standaardfrequentie van de schaal van 1-front-end voor elke 15 exemplaren van de App Service-plan niet snel genoeg is, kunt u de verhouding op welke front-ends worden toegevoegd of de grootte van de front-ends aanpassen.  Wanneer u de verhouding of grootte aanpast, betaalt u voor de front-kernen die niet standaard zou worden toegevoegd.  

Bijvoorbeeld, als u de schaal verhouding tussen 10 aanpast, is een front-end toegevoegd voor elke 10 exemplaren in uw App Service-abonnementen. De platte vergoeding geldt voor een snelheid van de schaal van één front-end voor elke 15 exemplaren. Met een ratio van de schaal van 10 betaalt u een vast bedrag voor het derde front-end die toegevoegd voor de 10 exemplaren van de App Service-plan. U hoeft niet te betalen wanneer u 15 exemplaren bereikt omdat deze automatisch is toegevoegd.

Als u de grootte van de front-ends 2 kernen aangepast, maar niet van de verhouding tussen wijzigen vervolgens betaalt u voor de extra kernen.  Een as-omgeving wordt gemaakt met 2 front-ends, dus ook onder de automatische schaling drempelwaarde die u voor 2 extra kernen betalen zou als u de grootte van 2 core front-ends verhoogd.

Zie voor meer informatie [prijzen voor Azure App Service][Pricing].

## <a name="delete-an-ase"></a>Verwijderen van een as-omgeving ##

Verwijderen van een as-omgeving: 

1. Gebruik **verwijderen** boven aan de **App Service-omgeving** blade. 

2. Voer de naam van uw as-omgeving om te bevestigen dat u wilt verwijderen. Wanneer u een as-omgeving verwijdert, verwijdert u alle van de inhoud van deze ook. 

    ![Verwijderen van een as-omgeving][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../app-service-deploy-local-git.md
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
