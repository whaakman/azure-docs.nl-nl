---
title: Een Azure App Service-omgeving gebruiken
description: Over het maken, publiceren en apps in een Azure App Service environment schalen
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
ms.openlocfilehash: b3550c771b4c2916987c66f318010e5bb246fa39
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446847"
---
# <a name="use-an-app-service-environment"></a>Een App Service-omgeving gebruiken #

## <a name="overview"></a>Overzicht ##

Azure App Service Environment is een implementatie van Azure App Service in een subnet in de Azure-netwerk van een klant. Deze bestaat uit:

- **Front-ends**: de front-ends zijn waar de HTTP/HTTPS in een App Service environment (ASE) wordt beëindigd.
- **Werknemers**: de werknemers zijn de resources die uw apps hosten.
- **Database**: de database bevat informatie die u de omgeving definieert.
- **Opslag**: de opslag wordt gebruikt voor het hosten van de klant gepubliceerde apps.

> [!NOTE]
> Er zijn twee versies van App Service-omgeving: ASEv1 en ASEv2. In ASEv1 moet u de resources beheren, voordat u ze kunt gebruiken. Zie voor meer informatie over het configureren en beheren van ASEv1, [configureren van een App Service environment v1][ConfigureASEv1]. De rest van dit artikel is gericht op ASEv2.
>
>

U kunt een as-omgeving (ASEv1 en ASEv2) implementeren met een extern of intern VIP-adres voor toegang tot Apps. De implementatie met een extern VIP-adres wordt vaak een externe as-omgeving genoemd. De interne versie heet de ILB as-omgeving omdat het gebruikmaakt van een interne load balancer (ILB). Zie voor meer informatie over de ILB as-omgeving, [maken en gebruiken een ILB as-omgeving][MakeILBASE].

## <a name="create-a-web-app-in-an-ase"></a>Een web-app maken in een as-omgeving ##

Voor het maken van een web-app in een as-omgeving, gebruikt u dezelfde procedure als wanneer u deze normaal gesproken maakt, maar met enkele kleine verschillen. Wanneer u een nieuwe App Service-plan maken:

- In plaats van het kiezen van een geografische locatie op waar om uw app te implementeren, kunt u een as-omgeving als locatie kiezen.
- Alle App Service-plannen hebt gemaakt in een as-omgeving moeten zich in een geïsoleerd prijscategorie.

Als u een as-omgeving hebt, kunt u maken volgt u de instructies in [maken van een App Service environment][MakeExternalASE].

Een web-app maken in een as-omgeving:

1. Selecteer **een resource maken** > **Web en mobiel** > **Web-App**.

1. Voer een naam voor de web-app. Als u al een App Service-plan in een as-omgeving hebt geselecteerd, geeft de domeinnaam op voor de app de domeinnaam van de as-omgeving.

    ![Web-app-naam selecteren][1]

1. Selecteer een abonnement.

1. Voer een naam in voor een nieuwe resourcegroep of selecteer **gebruik bestaande** en selecteer in de vervolgkeuzelijst.

1. Selecteer uw besturingssysteem. 

    * Die als host fungeert voor een Linux-app in een as-omgeving is een nieuwe preview-functie, het is raadzaam dat u geen Linux-apps toevoegen in een as-omgeving die productieworkloads momenteel wordt uitgevoerd. 
    * Toevoegen van een Linux-app in een as-omgeving, betekent dat de ASE ook opgenomen in de preview-modus worden. 

1. Selecteer een bestaand App Service-plan in de as-omgeving of maak een nieuwe door de volgende stappen:

    a. Selecteer **Maak een nieuwe**.

    b. Voer de naam van uw App Service-plan.

    c. Selecteer de as-omgeving in de **locatie** vervolgkeuzelijst. Die als host fungeert voor een Linux-app in een as-omgeving is alleen ingeschakeld in 6 regio's, op dit moment: **VS-West, VS-Oost, West-Europa, Noord-Europa, Australië-Oost, Zuidoost-Azië.** 

    d. Selecteer een **geïsoleerd** prijscategorie. Selecteer **Selecteer**.

    e. Selecteer **OK**.
    
    ![Geïsoleerde categorieën][2]

    > [!NOTE]
    > Linux-web-apps en web-apps voor Windows kunnen niet in de dezelfde App Service-Plan, maar kunnen zich in hetzelfde App Service Environment. 
    >

1. Selecteer **Maken**.

## <a name="how-scale-works"></a>Hoe werkt schalen ##

Elke App Service-app wordt uitgevoerd in een App Service-plan. Het model van de container is omgevingen Houd App Service-plannen en App Service-plannen Houd apps. Wanneer u een app, kunt u schalen van het App Service-plan en schaal dus alle apps in hetzelfde abonnement.

In ASEv2, wanneer u de schaal van een App Service-plan, wordt de benodigde infrastructuur automatisch toegevoegd. Er is een vertraging om bewerkingen te schalen, terwijl de infrastructuur wordt toegevoegd. In ASEv1, kan de benodigde infrastructuur moet worden toegevoegd voordat u kunt maken of uitbreiden van uw App Service-plan. 

In de multitenant-App Service is schalen meestal direct omdat een groep resources gemakkelijk beschikbaar zijn voor de ondersteuning. Er is geen dergelijke buffer in een as-omgeving en bronnen bij moet worden toegewezen.

U kunt maximaal 100 exemplaren schalen in een as-omgeving. De 100 instanties kunnen allemaal op één enkele App Service-plan of verdeeld over meerdere App Service-plannen.

## <a name="ip-addresses"></a>IP-adressen ##

App Service biedt de mogelijkheid om toe te wijzen een toegewezen IP-adres aan een app. Deze mogelijkheid is beschikbaar nadat u een IP-gebaseerd SSL configureren zoals beschreven in [een bestaand aangepast SSL-certificaat binden aan Azure WebApps][ConfigureSSL]. In een as-omgeving is er echter een uitzondering die aandacht vereisen. U kunt extra IP-adressen moet worden gebruikt voor een op IP gebaseerde SSL in een ILB as-omgeving niet toevoegen.

In ASEv1 moet u de IP-adressen als resources toewijzen voordat u ze kunt gebruiken. In ASEv2 gebruikt u deze vanuit uw app net als in de multitenant-App Service. Er is altijd één spare-adres in ASEv2 maximaal 30 IP-adressen. Telkens wanneer is u een, andere toegevoegd, zodat een adres altijd direct beschikbaar voor gebruik is. Een tijd vertraging is vereist voor het toewijzen van een ander IP-adres, waardoor het toevoegen van IP-adressen snel achter elkaar.

## <a name="front-end-scaling"></a>Front-endschaling ##

In ASEv2, wanneer u uw App Service-plannen uitschalen worden werknemers automatisch toegevoegd om ze te ondersteunen. Elke as-omgeving is gemaakt met twee front-ends. Bovendien de-front-ends automatisch scale-out tegen een tarief van één front-end voor elke 15-exemplaren in uw App Service-plannen. Bijvoorbeeld, als u 15 exemplaren hebt, hebt u drie front-ends. Als u de schaal aan 30 exemplaren, hebt u vier front-ends, enzovoort.

Dit aantal front-ends moet ruim voldoende is voor de meeste scenario's. U kunt echter opwaarts schalen met een hogere snelheid. U kunt de verhouding tussen het zo laag één front end voor elke vijf exemplaren wijzigen. Er is een kosten in rekening gebracht voor het wijzigen van de verhouding. Zie voor meer informatie, [prijzen voor Azure App Service][Pricing].

Front-endresources zijn de HTTP/HTTPS-eindpunt voor de as-omgeving. Geheugengebruik per front-end is met de front-end-configuratie van de standaard consistent ongeveer 60 procent. Klantwerkbelastingen uitvoeren niet op een front-end. De belangrijkste factor voor een front-end met betrekking tot het schalen is de CPU, die wordt aangedreven voornamelijk door HTTPS-verkeer.

## <a name="app-access"></a>App-toegang ##

In een externe as-omgeving, moet aan het domein dat wordt gebruikt bij het maken van apps wijkt af van de multitenant-App Service. Het bevat de naam van de as-omgeving. Zie voor meer informatie over het maken van een externe as-omgeving [maken van een App Service environment][MakeExternalASE]. De naam van het domein in een externe as-omgeving ziet eruit als *.&lt; asename&gt;. p.azurewebsites.net*. Bijvoorbeeld, als de naam van de as-omgeving _external ase_ en hosten van een app met de naam _contoso_ in deze as-omgeving, u deze met de volgende URL's opent:

- contoso.external-ase.p.azurewebsites.net
- contoso.scm.external-ase.p.azurewebsites.net

De URL contoso.scm.external-ase.p.azurewebsites.net wordt gebruikt voor toegang tot de Kudu-console of voor het publiceren van uw app met behulp van web implementeren. Zie voor informatie over de Kudu-console, [Kudu-console voor Azure App Service][Kudu]. De Kudu-console biedt u een web-UI voor foutopsporing, uploaden van bestanden, bestanden en nog veel meer bewerken.

In een ILB as-omgeving bepaalt u het domein tijdens de implementatie. Zie voor meer informatie over het maken van een ILB as-omgeving [maken en gebruiken een ILB as-omgeving][MakeILBASE]. Als u de domeinnaam opgeven _ilb-ase.info_, de apps in deze as-omgeving dat domein gebruiken tijdens het maken van apps. Voor de app met de naam _contoso_, wordt de URL's zijn:

- Contoso.ILB ase.info
- Contoso.SCM.ILB ase.info

## <a name="publishing"></a>Publiceren ##

Net als bij de multitenant-App Service in een as-omgeving kunt u publiceren met:

- Webimplementatie.
- FTP.
- Continue integratie.
- Slepen en neerzetten in de Kudu-console.
- Een IDE, zoals Visual Studio, Eclipse en IntelliJ IDEE.

Met een externe as-omgeving werken deze alle publicatieopties hetzelfde. Zie voor meer informatie, [implementatie in Azure App Service][AppDeploy]. 

Het belangrijkste verschil met het publiceren is met betrekking tot een ILB as-omgeving. De publicatie-eindpunten zijn met een ILB as-omgeving, alle alleen beschikbaar via de ILB. De ILB is op een privé IP-adres in de ASE-subnet in het virtuele netwerk. Als u geen toegang tot het netwerk naar de ILB, kunt u alle apps in deze as-omgeving niet publiceren. Zoals vermeld in [maken en gebruiken een ILB as-omgeving][MakeILBASE], moet u DNS configureren voor de apps in het systeem. Dit is inclusief de SCM-eindpunt. Als ze zijn niet correct gedefinieerd, kunt u niet publiceren. Er moet ook uw IDE's netwerktoegang hebben tot de ILB om te kunnen publiceren naar deze.

Op internet gebaseerde CI-systemen, zoals GitHub en Visual Studio Team Services, werken niet met een ILB as-omgeving omdat het publicatie-eindpunt niet toegankelijk is via Internet is. In plaats hiervan moet u een CI-systeem gebruiken dat gebruikmaakt van een pull-model, bijvoorbeeld Dropbox.

De publicatie-eindpunten voor apps in een ILB AS-omgeving maken gebruik van het domein waarmee de ILB AS-omgeving is gemaakt. U kunt dit zien in het publicatieprofiel van de app en in de portalblade van de app (in **overzicht** > **Essentials** en ook in **eigenschappen**). 

## <a name="pricing"></a>Prijzen ##

De prijzen SKU met de naam **geïsoleerd** is gemaakt voor gebruik met ASEv2. Alle App Service-abonnementen die worden gehost in ASEv2 zijn in de geïsoleerde prijs-SKU. Geïsoleerde App Service-plan tarieven kunnen afwijken per regio. 

Naast de prijs voor uw App Service-abonnementen is er een vast tarief voor as-omgeving zelf. Vast bedrag verandert niet met de grootte van de as-omgeving en betaalt voor de infrastructuur as-omgeving op een standaard-tarief voor 1 extra schalen front-end voor elke 15 App Service-plan-exemplaren.  

Als het standaardtarief voor de schaal van 1-front-end voor elke 15 exemplaren van de App Service-plan niet snel genoeg is, kunt u de verhouding bij welke front-ends worden toegevoegd of de grootte van de front-ends aanpassen.  Als u de verhouding tussen of de grootte aanpassen, betaalt u voor de front-end-kernen dat kunnen niet standaard worden toegevoegd.  

Bijvoorbeeld, als u de schaal verhouding tussen het 10 aanpast, wordt een front-end toegevoegd voor elke 10 exemplaren in uw App Service-plannen. De vast tarief omvat een tarief van de schaal van één front-end voor elke 15-exemplaren. Met een ratio van de schaal van 10, kunt u een bedrag betaalt voor de derde front-end die wordt toegevoegd voor de 10 exemplaren van de App Service-plan. U hoeft te betalen voor wanneer u 15 exemplaren bereikt omdat deze automatisch is toegevoegd.

Als u de grootte van de front-ends op 2 kernen aangepast, maar niet van de verhouding tussen wijzigen vervolgens betaalt u voor de extra cores.  Een as-omgeving wordt gemaakt met 2 front-ends, dus ook onder de automatisch vergroten/verkleinen drempelwaarde u voor 2 extra kernen betaalt als u de grootte tot 2 core front-ends verhoogd.

Zie voor meer informatie, [prijzen voor Azure App Service][Pricing].

## <a name="delete-an-ase"></a>Een as-omgeving verwijderen ##

Een as-omgeving verwijderen: 

1. Gebruik **verwijderen** aan de bovenkant van de **App Service-omgeving** blade. 

1. Voer de naam van de as-omgeving om te bevestigen dat u wilt verwijderen. Als u een as-omgeving verwijdert, verwijdert u alle ook inhoud binnen het. 

    ![As-omgeving verwijderen][3]

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
[NSGs]: ../../virtual-network/security-overview.md
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
