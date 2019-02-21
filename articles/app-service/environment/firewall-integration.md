---
title: 'App Service-omgeving uitgaand verkeer: Azure vergrendelen'
description: Hierin wordt beschreven hoe om te integreren met Azure-Firewall voor het beveiligen van uitgaand verkeer
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: af6a32d7e32f23561b207c729402eaea7925f520
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56453848"
---
# <a name="locking-down-an-app-service-environment"></a>Vergrendelen van een App Service Environment

De App Service Environment (ASE) heeft een aantal externe afhankelijkheden die hiervoor toegang tot alleen juist werken. De as-omgeving is aanwezig in de klant Azure Virtual Network (VNet). Klanten moeten het ASE-afhankelijkheid verkeer dat is een probleem voor klanten die u wilt vergrendelen alle uitgaande verkeer van het VNet toestaan.

Er zijn een aantal binnenkomende afhankelijkheden met een as-omgeving. Het inkomend managementverkeer kan niet worden verzonden via een firewall-apparaat. De bronadressen voor dit verkeer bekend zijn en worden gepubliceerd in de [beheeradressen van App Service-omgeving](https://docs.microsoft.com/azure/app-service/environment/management-addresses) document. U kunt Network Security Group regels maken met deze informatie om binnenkomend verkeer te beveiligen.

De uitgaande ASE-afhankelijkheden zijn bijna volledig gedefinieerd met FQDN's, die geen statische adressen achter ze hebben. Het ontbreken van statische adressen betekent dat Netwerkbeveiligingsgroepen (nsg's) kan niet worden gebruikt voor het vergrendelen van het uitgaande verkeer van een as-omgeving. De adressen wijzigen vaak genoeg dat kan niet een regels op basis van de huidige oplossing instellen en maakt u nsg's. 

De oplossing voor beveiliging van de uitgaande adressen ligt in het gebruik van een firewall-apparaat dat uitgaand verkeer op basis van domeinnamen kunt beheren. Firewall van Azure kunt beperken uitgaande HTTP en HTTPS-verkeer op basis van de FQDN-naam van de bestemming.  

## <a name="configuring-azure-firewall-with-your-ase"></a>Firewall voor Azure configureren met de as-omgeving 

De stappen voor het uitgaande verkeer van uw bestaande as-omgeving met Azure-Firewall vergrendelen zijn:

1. Service-eindpunten op SQL-, opslag- en Event Hub op uw ASE-subnet inschakelen. Om dit te doen, gaat u naar de portal voor netwerken > subnetten en selecteer Microsoft.EventHub, Microsoft.SQL en Microsoft.Storage in de vervolgkeuzelijst van Service-eindpunten. Wanneer u ingeschakeld voor Azure SQL-service-eindpunten hebt, moeten een Azure SQL-afhankelijkheden die uw apps hebben met service-eindpunten worden geconfigureerd. 

   ![Selecteer de service-eindpunten][2]
  
1. Maak een subnet met de naam AzureFirewallSubnet in de VNet waar de as-omgeving bestaat. Volg de aanwijzingen in de [documentatie bij Azure Firewall](https://docs.microsoft.com/azure/firewall/) te maken van uw Azure-Firewall.
1. Vanuit de gebruikersinterface van de Firewall Azure > regels > toepassing regelverzameling, selecteer toevoegen-toepassing-regelverzameling. Geef een naam op, prioriteit, en stel toestaan. Geef een naam op in de sectie van de labels FQDN-naam, stelt u de bron-adressen op * en selecteer de App Service-omgeving FQDN-Tag en de Windows Update. 
   
   ![Toepassing-regel toevoegen][1]
   
1. Vanuit de gebruikersinterface van de Firewall Azure > regels > netwerk regelverzameling, selecteert u de netwerk-regelverzameling toevoegen. Geef een naam op, prioriteit en stel toestaan. Geef een naam in de sectie regels, selecteer **elke**, stel * naar de bron- en -adressen, en stelt u de poorten op 123. Deze regel kan het systeem om uit te voeren van de klok synchroniseren met het NTP. Maak een andere regel dezelfde manier om te sorteren op poort 12000 eventuele problemen met het systeem.

   ![NTP netwerk regel toevoegen][3]

1. Een routetabel maken met de beheeradressen van [beheeradressen van App Service-omgeving]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) met de volgende hop van Internet. De vermeldingen in de routetabel zijn vereist om te voorkomen dat problemen met asymmetrische routering. Routes voor de IP-adresafhankelijkheden, zoals hieronder wordt vermeld in de afhankelijkheden van de IP-adres met de volgende hop Internet toevoegen. Een virtueel apparaat route toevoegen aan de routetabel voor 0.0.0.0/0 met de volgende hop wordt de Firewall van Azure privé IP-adres. 

   ![Een routetabel maken][4]
   
1. De routetabel die u hebt gemaakt met uw ASE-subnet toewijzen.

#### <a name="deploying-your-ase-behind-a-firewall"></a>Implementatie van de as-omgeving achter een firewall

De stappen voor het implementeren van de as-omgeving achter een firewall zijn hetzelfde als de bestaande as-omgeving configureren met een Azure-Firewall, behalve dat u moet uw ASE-subnet maken en de vorige stappen. Voor het maken van de as-omgeving in een bestaand subnet, moet u een Resource Manager-sjabloon gebruiken, zoals beschreven in het document op [uw ASE maken met Resource Manager-sjabloon](https://docs.microsoft.com/azure/app-service/environment/create-from-template).

## <a name="application-traffic"></a>Toepassingsverkeer 

De bovenstaande stappen kunt uw as-omgeving om te werken zonder problemen. U moet nog steeds dingen om aan de toepassingsbehoeften van uw te configureren. Er zijn twee problemen voor toepassingen in een as-omgeving die is geconfigureerd met Azure-Firewall.  

- Afhankelijkheden van toepassing moeten worden toegevoegd aan de Azure-Firewall of de routetabel. 
- Routes moeten worden gemaakt voor de toepassingsverkeer om te voorkomen dat problemen met asymmetrische routering

Als uw toepassingen afhankelijkheden hebt, moeten ze worden toegevoegd aan uw Azure-Firewall. Sta HTTP/HTTPS-verkeer en het netwerk van regels voor alle andere regels toepassing maken. 

Als u het adresbereik dat uw toepassing-verkeer voor paginaweergaven, zijn afkomstig van weet, kunt u toevoegen dat aan de routetabel die is toegewezen aan uw ASE-subnet. Als het adresbereik grote of niet opgegeven is, kunt u een netwerkapparaat, zoals de Application Gateway gebruiken om u te bieden één adres toe te voegen aan uw routetabel. Lees voor meer informatie over het configureren van een toepassingsgateway met uw ILB as-omgeving [uw ILD ASE integreren met een Application Gateway](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway)

![As-omgeving met Azure-Firewall-verbindingsprocedure][5]

Deze gebruik van de toepassingsgateway is slechts één voorbeeld van het configureren van uw systeem. Als u dit pad volgt, moet u zou een route toevoegen aan de routetabel van de ASE-subnet, dus het op antwoordverkeer dat wordt verzonden naar de toepassingsgateway zou er rechtstreeks. 

## <a name="logging"></a>Logboekregistratie 

Firewall van Azure kunt Logboeken verzenden naar Azure Storage, Event Hub of Azure Monitor-Logboeken. Als u wilt uw app integreren met een ondersteunde bestemming, gaat u naar de Firewall van Azure portal > diagnostische logboeken en de logboeken voor de gewenste bestemming in te schakelen. Als u met Azure Monitor-logboeken integreert, ziet u logboekregistratie voor verkeer dat wordt verzonden naar de Firewall van Azure. Als u wilt zien van het verkeer dat wordt geweigerd, opent u de portal van uw Log Analytics-werkruimte > Logboeken en voert u een query zoals 

    AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
 
Uw Azure-Firewall integreren met Azure Monitor-Logboeken is zeer nuttig bij het eerst ophalen van een toepassing werkt wanneer u zich niet bewust bent van alle van de afhankelijkheden van toepassing. U kunt meer informatie over Azure Monitor-logboeken van [analyseren logboekgegevens in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)
 
## <a name="dependencies"></a>Afhankelijkheden

De volgende informatie is alleen vereist als u wilt een firewallapparaat dan Azure-Firewall configureren. 

- Service-eindpunt kan services moeten worden geconfigureerd met service-eindpunten.
- IP-adres afhankelijkheden zijn voor niet-HTTP/S-verkeer (zowel TCP en UDP-verkeer)
- FQDN-naam HTTP/HTTPS-eindpunten kunnen worden geplaatst in uw firewall-apparaat.
- Jokertekens HTTP/HTTPS-eindpunten zijn afhankelijkheden die met de as-omgeving op basis van een aantal kwalificaties kunnen verschillen. 
- Linux-afhankelijkheden zijn alleen een probleem als u Linux-apps in de as-omgeving implementeert. Als u geen Linux-apps in uw ASE implementeert, klikt u vervolgens hoeft deze adressen niet te worden toegevoegd aan de firewall. 


#### <a name="service-endpoint-capable-dependencies"></a>Service-eindpunt kan afhankelijkheden 

| Eindpunt |
|----------|
| Azure SQL |
| Azure Storage |
| Azure Event Hub |

#### <a name="ip-address-dependencies"></a>Afhankelijkheden van de IP-adres

| Eindpunt | Details |
|----------| ----- |
| \*:123 | Controle van de klok NTP. Verkeer wordt gecontroleerd op meerdere eindpunten op poort 123 |
| \*:12000 | Deze poort wordt gebruikt voor het bewaken van een systeem. Als de geblokkeerd en enkele problemen moeilijker te sorteren worden, maar de as-omgeving blijft functioneren |

Met een Azure-Firewall krijgt u automatisch alle onderstaande geconfigureerd met de FQDN-codes. 

#### <a name="fqdn-httphttps-dependencies"></a>FQDN-naam HTTP/HTTPS-afhankelijkheden 

| Eindpunt |
|----------|
|graph.windows.net:443 |
|login.live.com:443 |
|login.windows.com:443 |
|login.windows.net:443 |
|login.microsoftonline.com:443 |
|client.wns.windows.com:443 |
|definitionupdates.microsoft.com:443 |
|go.microsoft.com:80 |
|go.microsoft.com:443 |
|www.microsoft.com:80 |
|www.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|ocsp.msocsp.com:443 |
|mscrl.microsoft.com:443 |
|mscrl.microsoft.com:80 |
|crl.microsoft.com:443 |
|crl.microsoft.com:80 |
|www.thawte.com:443 |
|crl3.digicert.com:80 |
|ocsp.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|crl.verisign.com:80 |
|ocsp.verisign.com:80 |
|cacerts.digicert.com:80 |
|azperfcounters1.blob.core.windows.net:443 |
|azurewatsonanalysis-prod.core.windows.net:443 |
|global.metrics.nsatc.net:80   |
|az-prod.metrics.nsatc.net:443 |
|antares.metrics.nsatc.net:443 |
|azglobal-black.azglobal.metrics.nsatc.net:443 |
|azglobal-red.azglobal.metrics.nsatc.net:443 |
|antares-black.antares.metrics.nsatc.net:443 |
|antares-red.antares.metrics.nsatc.net:443 |
|maupdateaccount.blob.core.windows.net:443 |
|clientconfig.passport.net:443 |
|packages.microsoft.com:443 |
|schemas.microsoft.com:80 |
|schemas.microsoft.com:443 |
|management.core.windows.net:443 |
|management.core.windows.net:80 |
|management.azure.com:443 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge.net:443 |
|validation-v2.sls.microsoft.com:443 |
|flighting.cp.wd.microsoft.com:443 |
|dmd.metaservices.microsoft.com:80 |
|admin.core.windows.net:443 |
|azureprofileruploads.blob.core.windows.net:443 |
|azureprofileruploads2.blob.core.windows.net:443 |
|azureprofileruploads3.blob.core.windows.net:443 |
|azureprofileruploads4.blob.core.windows.net:443 |
|azureprofileruploads5.blob.core.windows.net:443 |

#### <a name="wildcard-httphttps-dependencies"></a>Jokertekens HTTP/HTTPS-afhankelijkheden 

| Eindpunt |
|----------|
|gr-Prod-\*.cloudapp.net:443 |
| \*.management.azure.com:443 |
| \*.update.microsoft.com:443 |
| \*.windowsupdate.microsoft.com:443 |
|grmdsprod\*mini\*.servicebus.windows.net:443 |
|grmdsprod\*lini\*.servicebus.windows.net:443 |
|grsecprod\*mini\*.servicebus.windows.net:443 |
|grsecprod\*lini\*.servicebus.windows.net:443 |
|graudprod\*mini\*.servicebus.windows.net:443 |
|graudprod\*lini\*.servicebus.windows.net:443 |

#### <a name="linux-dependencies"></a>Linux-afhankelijkheden 

| Eindpunt |
|----------|
|wawsinfraprodbay063.blob.core.windows.net:443 |
|registry-1.docker.io:443 |
|auth.docker.IO:443 |
|production.cloudflare.docker.com:443 |
|download.docker.com:443 |
|us.archive.ubuntu.com:80 |
|download.mono-project.com:80 |
|packages.treasuredata.com:80|
|security.ubuntu.com:80 |

<!--Image references-->
[1]: ./media/firewall-integration/firewall-apprule.png
[2]: ./media/firewall-integration/firewall-serviceendpoints.png
[3]: ./media/firewall-integration/firewall-ntprule.png
[4]: ./media/firewall-integration/firewall-routetable.png
[5]: ./media/firewall-integration/firewall-topology.png
