---
title: Vergrendelen van uitgaand verkeer van Azure App Service Environment
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
ms.date: 09/24/2018
ms.author: ccompy
ms.openlocfilehash: 5f2dd31488ae61bec061a81986a208bd328bf39b
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49093615"
---
# <a name="locking-down-an-app-service-environment"></a>Vergrendelen van een App Service Environment

De App Service Environment (ASE) heeft een aantal externe afhankelijkheden die hiervoor toegang tot alleen juist werken. De as-omgeving is aanwezig in de klant Azure Virtual Network (VNet). Klanten moeten het ASE-afhankelijkheid verkeer dat is een probleem voor klanten die u wilt vergrendelen alle uitgaande verkeer van het VNet toestaan.

Er zijn een aantal binnenkomende afhankelijkheden met een as-omgeving. Het inkomend managementverkeer kan niet worden verzonden via een firewall-apparaat. De bronadressen voor dit verkeer bekend zijn en worden gepubliceerd in de [beheeradressen van App Service-omgeving](https://docs.microsoft.com/azure/app-service/environment/management-addresses) document. U kunt Network Security Group regels maken met deze informatie om binnenkomend verkeer te beveiligen.

De uitgaande ASE-afhankelijkheden zijn bijna volledig gedefinieerd met FQDN's, die geen statische adressen achter ze hebben. Het ontbreken van statische adressen betekent dat Netwerkbeveiligingsgroepen (nsg's) kan niet worden gebruikt voor het vergrendelen van het uitgaande verkeer van een as-omgeving. De adressen wijzigen vaak genoeg dat kan niet een regels op basis van de huidige oplossing instellen en maakt u nsg's. 

De oplossing voor beveiliging van de uitgaande adressen ligt in het gebruik van een firewall-apparaat dat uitgaand verkeer op basis van domeinnamen kunt beheren. Het Azure Networking-team heeft een nieuw netwerkapparaat in Preview-versie met de naam Azure Firewall plaatsen. De Firewall van Azure is geschikt beperken uitgaande HTTP en HTTPS-verkeer op basis van de DNS-naam van het doel is.  

## <a name="configuring-azure-firewall-with-your-ase"></a>Firewall voor Azure configureren met de as-omgeving 

De stappen voor het vergrendelen van uitgaand verkeer vanuit uw ASE met Azure-Firewall zijn:

1. Een Azure-Firewall in het VNet waarin de as-omgeving is, of maken. [Azure Firewall documentatie bij](https://docs.microsoft.com/azure/firewall/)
2. Selecteer de App-Tag voor Fully Qualified Domain Name Service-omgeving vanuit de gebruikersinterface van de Firewall voor Azure
3. Een routetabel maken met de beheeradressen van [beheeradressen van App Service-omgeving]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) met de volgende hop van Internet. De vermeldingen in de routetabel zijn vereist om te voorkomen dat problemen met asymmetrische routering. 
4. Routes voor de IP-adresafhankelijkheden, zoals hieronder wordt vermeld in de afhankelijkheden van de IP-adres met de volgende hop Internet toevoegen. 
5. Een route toevoegen aan de routetabel voor 0.0.0.0/0 met de volgende hop wordt uw netwerkapparaat met Firewall van Azure
6. Service-eindpunten maken voor uw ASE-subnet naar Azure SQL en Azure Storage
7. De routetabel die u hebt gemaakt met uw ASE-subnet toewijzen  

## <a name="application-traffic"></a>Toepassingsverkeer 

De bovenstaande stappen kunt uw as-omgeving om te werken zonder problemen. U moet nog steeds dingen om aan de toepassingsbehoeften van uw te configureren. Er zijn twee problemen voor toepassingen in een as-omgeving die is geconfigureerd met Azure-Firewall.  

- Afhankelijkheid FQDN's van de toepassing moet worden toegevoegd aan de Azure-Firewall of de routetabel
- Routes moeten worden gemaakt voor de adressen die verkeer afkomstig zijn uit om te voorkomen dat problemen met asymmetrische routering

Als uw toepassingen afhankelijkheden hebt, moeten ze worden toegevoegd aan uw Azure-Firewall. Sta HTTP/HTTPS-verkeer en het netwerk van regels voor alle andere regels toepassing maken. 

Als u het adresbereik dat uw toepassing-verkeer voor paginaweergaven, zijn afkomstig van weet, kunt u toevoegen dat aan de routetabel die is toegewezen aan uw ASE-subnet. Als het adresbereik grote of niet opgegeven is, kunt u een netwerkapparaat, zoals de Application Gateway gebruiken om u te bieden één adres toe te voegen aan uw routetabel. Lees voor meer informatie over het configureren van een toepassingsgateway met uw ILB as-omgeving [uw ILD ASE integreren met een Application Gateway](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway)


## <a name="dependencies"></a>Afhankelijkheden

De Azure App Service heeft een aantal externe afhankelijkheden. Ze kunnen worden claimregelsettype opgesplitst in verschillende gebieden:

- Service-eindpunt kan services van Service-eindpunten met moeten worden ingesteld als u wilt vergrendelen uitgaand netwerkverkeer.
- IP-adreseindpunten niet worden behandeld met een domeinnaam. Dit is een probleem voor firewallapparaten die u verwacht dat alle HTTPS-verkeer domeinnamen gebruiken. De IP-adreseindpunten moeten worden toegevoegd aan de routetabel waarin die is ingesteld op het ASE-subnet.
- FQDN-naam HTTP/HTTPS-eindpunten kunnen worden geplaatst in uw firewall-apparaat.
- Jokertekens HTTP/HTTPS-eindpunten zijn afhankelijkheden die met de as-omgeving op basis van een aantal kwalificaties kunnen verschillen. 
- Linux-afhankelijkheden zijn alleen een probleem als u Linux-apps in de as-omgeving implementeert. Als u geen Linux-apps in uw ASE implementeert, klikt u vervolgens hoeft deze adressen niet te worden toegevoegd aan de firewall. 


#### <a name="service-endpoint-capable-dependencies"></a>Service-eindpunt kan afhankelijkheden 

| Eindpunt |
|----------|
| Azure SQL |
| Azure Storage |
| Met Azure Key Vault |


#### <a name="ip-address-dependencies"></a>IP-adres-afhankelijkheden 

| Eindpunt |
|----------|
| 40.77.24.27:443 |
| 13.82.184.151:443 |
| 13.68.109.212:443 |
| 13.90.249.229:443 |
| 13.91.102.27:443 |
| 104.45.230.69:443 |
| 168.62.226.198:12000 |


#### <a name="fqdn-httphttps-dependencies"></a>FQDN-naam HTTP/HTTPS-afhankelijkheden 

| Eindpunt |
|----------|
|graph.windows.net:443 |
|login.live.com:443 |
|Login.Windows.com:443 |
|Login.Windows.NET:443 |
|login.microsoftonline.com:443 |
|client.WNS.Windows.com:443 |
|definitionupdates.Microsoft.com:443 |
|go.Microsoft.com:80 |
|go.Microsoft.com:443 |
|www.microsoft.com:80 |
|www.Microsoft.com:443 |
|wdcpalt.Microsoft.com:443 |
|wdcp.Microsoft.com:443 |
|OCSP.msocsp.com:443 |
|mscrl.Microsoft.com:443 |
|mscrl.microsoft.com:80 |
|CRL.Microsoft.com:443 |
|crl.microsoft.com:80 |
|www.Thawte.com:443 |
|crl3.digicert.com:80 |
|OCSP.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|CRL.VeriSign.com:80 |
|OCSP.VeriSign.com:80 |
|azperfcounters1.BLOB.Core.Windows .net: 443 |
|azurewatsonanalysis prod.core.windows.net:443 |
|Global.metrics.nsatc.NET:80   |
|AZ-prod.metrics.nsatc.net:443 |
|antares.metrics.nsatc.NET:443 |
|azglobal black.azglobal.metrics.nsatc.net:443 |
|azglobal red.azglobal.metrics.nsatc.net:443 |
|voorziening black.antares.metrics.nsatc.net:443 |
|voorziening red.antares.metrics.nsatc.net:443 |
|maupdateaccount.BLOB.Core.Windows.NET:443 |
|clientconfig.Passport.NET:443 |
|Packages.Microsoft.com:443 |
|schemas.Microsoft.com:80 |
|schemas.Microsoft.com:443 |
|Management.Core.Windows.NET:443 |
|Management.Core.Windows.NET:80 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge .net: 443 |
|validatie-v2.sls.microsoft.com:443 |
|flighting.CP.WD.Microsoft.com:443 |
|DMD.metaservices.Microsoft.com:80 |
|Admin.Core.Windows.NET:443 |
|azureprofileruploads.BLOB.Core.Windows.NET:443 |
|azureprofileruploads2.BLOB.Core.Windows .net: 443 |
|azureprofileruploads3.BLOB.Core.Windows .net: 443 |
|azureprofileruploads4.BLOB.Core.Windows .net: 443 |
|azureprofileruploads5.BLOB.Core.Windows .net: 443 |

#### <a name="wildcard-httphttps-dependencies"></a>Jokertekens HTTP/HTTPS-afhankelijkheden 

| Eindpunt |
|----------|
|GR-Prod -\*. cloudapp.net:443 |
| \*. management.azure.com:443 |
| \*. update.microsoft.com:443 |
| \*. windowsupdate.microsoft.com:443 |
|grmdsprod\*mini\*. servicebus.windows.net:443 |
|grmdsprod\*lini\*. servicebus.windows.net:443 |
|grsecprod\*mini\*. servicebus.windows.net:443 |
|grsecprod\*lini\*. servicebus.windows.net:443 |
|graudprod\*mini\*. servicebus.windows.net:443 |
|graudprod\*lini\*. servicebus.windows.net:443 |

#### <a name="linux-dependencies"></a>Linux-afhankelijkheden 

| Eindpunt |
|----------|
|wawsinfraprodbay063.BLOB.Core.Windows .net: 443 |
|register-1.docker.io:443 |
|auth.docker.IO:443 |
|Production.cloudflare.docker.com:443 |
|Download.docker.com:443 |
|US.Archive.Ubuntu.com:80 |
|Download.mono project.com:80 |
|Packages.treasuredata.com:80|
|Security.Ubuntu.com:80 |

