---
title: Overzicht van de FQDN-naam tags voor de Firewall van Azure
description: Meer informatie over de FQDN-codes in de Firewall van Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 6dc7d20d31d9399355b2b3de90ea90f2f3e07af5
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224644"
---
# <a name="fqdn-tags-overview"></a>Overzicht van de FQDN-naam-tags

Een FQDN-tag vertegenwoordigt een groep van de volledig gekwalificeerde domeinnamen (FQDN's) die zijn gekoppeld aan bekende services van Microsoft. U kunt een FQDN-tag in regels voor application waarmee het vereiste uitgaande netwerkverkeer via de firewall.

Als u wilt toestaan handmatig Windows Update-netwerkverkeer via de firewall, moet u bijvoorbeeld meerdere regels per de Microsoft-documentatie voor application maken. Gebruik van tags van de FQDN-naam, kunt u een toepassing-regel maken, zijn de **Windows Updates** markeren en nu netwerkverkeer naar eindpunten door uw firewall kunnen stromen van Microsoft Windows Update.

U kunt uw eigen labels FQDN-naam niet maken of kunt u opgeven welk FQDN's zijn opgenomen in een tag. Microsoft beheert de FQDN-namen die zijn opgenomen door de FQDN-tag en updates van het label als FQDN's wijzigen. 

<!--- screenshot of application rule with a FQDN tag.-->

De volgende tabel ziet u de huidige FQDN-tags die u kunt gebruiken. Microsoft houdt deze tags en u kunt verwachten dat andere labels worden regelmatig toegevoegd.

|Label van de FQDN-naam  |Beschrijving  |
|---------|---------|
|Windows Update     |Toestaan van uitgaande toegang tot Microsoft Update, zoals beschreven in [over het configureren van een Firewall voor Software-Updates](https://technet.microsoft.com/library/bb693717.aspx).|
|Windows Diagnostische gegevens|Uitgaande toegang toestaat aan alle [Windows diagnostische gegevens over eindpunten](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints).|
|MAPS (Microsoft Active Protection Service)|Uitgaande toegang toestaat aan [MAPS](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/).|
|App Service-omgeving (ASE)|Biedt uitgaande toegang tot verkeer met ASE-platform. Deze tag dekt niet klantspecifieke Storage en SQL eindpunten die zijn gemaakt door as-omgeving. Deze moeten worden ingeschakeld [Service-eindpunten](../virtual-network/tutorial-restrict-network-access-to-resources.md) of handmatig worden toegevoegd.|
|Azure Backup|Biedt uitgaande toegang tot de back-up van Azure-services.

> [!NOTE]
> Bij het selecteren van FQDN code in een toepassing-regel, het veld protocol: poort moet worden ingesteld op **https**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het implementeren van een Azure-Firewall, [zelfstudie: implementeer en configureer de Firewall van Azure met behulp van de Azure-portal](tutorial-firewall-deploy-portal.md).