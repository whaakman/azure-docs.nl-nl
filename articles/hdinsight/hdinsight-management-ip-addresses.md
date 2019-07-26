---
title: IP-adressen van Azure HDInsight-beheer
description: Meer informatie over welke IP-adressen u binnenkomend verkeer moet toestaan, om netwerk beveiligings groepen en door de gebruiker gedefinieerde routes correct te configureren voor virtuele netwerken met Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/19/2019
ms.openlocfilehash: 0a41d1d57257db8f88481766e65eb8ee7569da87
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479191"
---
# <a name="hdinsight-management-ip-addresses"></a>IP-adressen van HDInsight-beheer

Als u netwerk beveiligings groepen (Nsg's) of door de gebruiker gedefinieerde routes (Udr's) gebruikt om inkomend verkeer naar uw HDInsight-cluster te beheren, moet u ervoor zorgen dat uw cluster kan communiceren met de essentiële Azure-status-en beheer Services.  Sommige IP-adressen voor deze services zijn regio specifiek, en sommige van deze zijn van toepassing op alle Azure-regio's. Mogelijk moet u ook verkeer van de Azure DNS-service toestaan als u geen aangepaste DNS gebruikt.

In de volgende secties worden de specifieke IP-adressen besproken die moeten worden toegestaan.

## <a name="azure-dns-service"></a>Azure DNS-service

Als u de door Azure verschafte DNS-service gebruikt, moet u toegang toestaan vanuit __168.63.129.16__ op poort 53. Zie voor meer informatie de [naam omzetting voor vm's en rollen instanties](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document. Als u aangepaste DNS gebruikt, slaat u deze stap over.

## <a name="health-and-management-services-all-regions"></a>Status-en beheer Services: Alle regio's

Verkeer toestaan van de volgende IP-adressen voor Azure HDInsight-status-en beheer services die van toepassing zijn op alle Azure-regio's:

| IP-adres van bron | Bestemming  | Direction |
| ---- | ----- | ----- |
| 168.61.49.99 | \*:443 | Binnenkomend |
| 23.99.5.239 | \*:443 | Binnenkomend |
| 168.61.48.131 | \*:443 | Binnenkomend |
| 138.91.141.162 | \*:443 | Binnenkomend |

## <a name="health-and-management-services-specific-regions"></a>Status-en beheer Services: Specifieke regio's

Verkeer toestaan van de IP-adressen die worden vermeld voor de Azure HDInsight-status-en beheer Services in de specifieke Azure-regio waar uw resources zich bevinden:

> [!IMPORTANT]  
> Als de Azure-regio die u gebruikt niet wordt vermeld, gebruikt u alleen de vier IP-adressen uit de vorige sectie.

| Country | Regio | Toegestane IP-adressen van bron | Toegestaan doel | Direction |
| ---- | ---- | ---- | ---- | ----- |
| Azië | Azië - oost | 23.102.235.122</br>52.175.38.134 | \*:443 | Binnenkomend |
| &nbsp; | Azië - zuidoost | 13.76.245.160</br>13.76.136.249 | \*:443 | Binnenkomend |
| Australië | Australië - oost | 104.210.84.115</br>13.75.152.195 | \*:443 | Binnenkomend |
| &nbsp; | Australië - zuidoost | 13.77.2.56</br>13.77.2.94 | \*:443 | Binnenkomend |
| Brazilië | Brazilië - zuid | 191.235.84.104</br>191.235.87.113 | \*:443 | Binnenkomend |
| Canada | Canada - oost | 52.229.127.96</br>52.229.123.172 | \*:443 | Binnenkomend |
| &nbsp; | Canada - midden | 52.228.37.66</br>52.228.45.222 |\*: 443 | Binnenkomend |
| China | China - noord | 42.159.96.170</br>139.217.2.219</br></br>42.159.198.178</br>42.159.234.157 | \*:443 | Binnenkomend |
| &nbsp; | China - oost | 42.159.198.178</br>42.159.234.157</br></br>42.159.96.170</br>139.217.2.219 | \*:443 | Binnenkomend |
| &nbsp; | China - noord 2 | 40.73.37.141</br>40.73.38.172 | \*:443 | Binnenkomend |
| &nbsp; | China - oost 2 | 139.217.227.106</br>139.217.228.187 | \*:443 | Binnenkomend |
| Europa | Europa - noord | 52.164.210.96</br>13.74.153.132 | \*:443 | Binnenkomend |
| &nbsp; | Europa -west| 52.166.243.90</br>52.174.36.244 | \*:443 | Binnenkomend |
| Frankrijk | Frankrijk - centraal| 20.188.39.64</br>40.89.157.135 | \*:443 | Binnenkomend |
| Duitsland | Duitsland - centraal | 51.4.146.68</br>51.4.146.80 | \*:443 | Binnenkomend |
| &nbsp; | Duitsland - noordoost | 51.5.150.132</br>51.5.144.101 | \*:443 | Binnenkomend |
| India | India - centraal | 52.172.153.209</br>52.172.152.49 | \*:443 | Binnenkomend |
| &nbsp; | India Zuid | 104.211.223.67<br/>104.211.216.210 | \*:443 | Binnenkomend |
| Japan | Japan - oost | 13.78.125.90</br>13.78.89.60 | \*:443 | Binnenkomend |
| &nbsp; | Japan - west | 40.74.125.69</br>138.91.29.150 | \*:443 | Binnenkomend |
| Zuid-Korea | Korea - centraal | 52.231.39.142</br>52.231.36.209 | \*:443 | Binnenkomend |
| &nbsp; | Korea - zuid | 52.231.203.16</br>52.231.205.214 | \*:443 | Binnenkomend
| Verenigd Koninkrijk | Verenigd Koninkrijk West | 51.141.13.110</br>51.141.7.20 | \*:443 | Binnenkomend |
| &nbsp; | Verenigd Koninkrijk Zuid | 51.140.47.39</br>51.140.52.16 | \*:443 | Binnenkomend |
| Verenigde Staten | US - centraal | 13.89.171.122</br>13.89.171.124 | \*:443 | Binnenkomend |
| &nbsp; | East US | 13.82.225.233</br>40.71.175.99 | \*:443 | Binnenkomend |
| &nbsp; | US - noord-centraal | 157.56.8.38</br>157.55.213.99 | \*:443 | Binnenkomend |
| &nbsp; | US - west-centraal | 52.161.23.15</br>52.161.10.167 | \*:443 | Binnenkomend |
| &nbsp; | US - west | 13.64.254.98</br>23.101.196.19 | \*:443 | Binnenkomend |
| &nbsp; | US - west 2 | 52.175.211.210</br>52.175.222.222 | \*:443 | Binnenkomend |

Voor informatie over de IP-adressen die voor Azure Government moeten worden gebruikt, raadpleegt u het document [Azure Government Intelligence en Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) .

Zie de sectie [netwerk verkeer beheren](hdinsight-plan-virtual-network-deployment.md#networktraffic) voor meer informatie.

Als u door de gebruiker gedefinieerde routes (Udr's) gebruikt, moet u een route opgeven en uitgaand verkeer van het VNET naar de bovenstaande IP-adressen toestaan met de volgende hop ingesteld op ' Internet '.

## <a name="next-steps"></a>Volgende stappen

* [Virtuele netwerken voor Azure HDInsight-clusters maken](hdinsight-create-virtual-network.md)
