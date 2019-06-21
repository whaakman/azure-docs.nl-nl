---
title: Lijst van veilige contactpersonen de Azure-portal URL's | Microsoft Docs
description: Deze URL's toevoegen aan proxyserverbypass om te communiceren met de Azure-portal en de bijbehorende services
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 06/13/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 87ec600a2f6c4a560ec7cbb064b561fa76e2b615
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305067"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Lijst van veilige contactpersonen de Azure-portal URL's op uw firewall of proxy-server

Configureren voor goede prestaties en connectiviteit tussen uw lokale - of WAN-netwerk en de Azure-cloud, on-premises beveiligingsapparaten als u wilt omzeilen beveiligingsbeperkingen voor de Azure-portal URL's. Netwerkbeheerders implementeren vaak proxyservers, firewalls of andere apparaten om te helpen beveiligen en krijgt u controle over hoe gebruikers toegang het internet tot. Regels die zijn ontworpen om gebruikers te beschermen kunnen echter soms blokkeren of vertragen legitieme bedrijfsgerelateerde internetverkeer, met inbegrip van communicatie tussen u en Azure. Voor het optimaliseren van verbinding tussen uw netwerk en de Azure-portal en de services, raden wij u aan Azure-portal URL's voor uw lijst van veilige contactpersonen.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Azure-portal-URL's voor proxy overslaan

De volgende lijst met URL's toevoegen aan uw proxyserver of firewall waarmee netwerkverkeer naar deze eindpunten te omzeilen: beperkingen:

* *.aadcdn.microsoftonline-p.com
* *.aimon.applicationinsights.io
* *.azure.com
* *.azuredatalakestore.net
* *.azureedge.net
* *.exp.azure.com
* *.ext.azure.com
* *.gfx.ms
* *.account.microsoft.com
* *.hosting.portal.azure.net
* *.marketplaceapi.microsoft.com
* *.microsoftonline.com
* *.msauth.net
* *.msftauth.net
* *.portal.azure.com
* *.portalext.visualstudio.com
* *.sts.microsoft.com
* *.vortex.data.microsoft.com
* *.vscommerce.visualstudio.com
* *.vssps.visualstudio.com
* *.wpc.azureedge.net

> [!NOTE]
> Verkeer naar deze eindpunten gebruikt standaard TCP-poorten voor HTTP (80)- en HTTPS (443).
>
>
## <a name="next-steps"></a>Volgende stappen

* Lijst van veilige contactpersonen IP-adressen wilt? Downloaden van de lijst met [Microsoft Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).
* Andere Microsoft-services gebruik extra URL's en IP-adressen voor de connectiviteit. Zie voor het optimaliseren van verbinding met het netwerk voor Microsoft 365-services, [instellen van uw netwerk voor Office 365](/office365/enterprise/set-up-network-for-office-365).
