---
title: Overzicht van de FQDN-naam tags voor de Firewall van Azure
description: Meer informatie over de FQDN-codes in de Firewall van Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 4/23/2019
ms.author: victorh
ms.openlocfilehash: 7a412589f4e86f2a49d07f2d01ca34bf30fd528b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64721177"
---
# <a name="fqdn-tags-overview"></a>Overzicht van de FQDN-naam-tags

Een FQDN-tag vertegenwoordigt een groep van de volledig gekwalificeerde domeinnamen (FQDN's) die zijn gekoppeld aan bekende services van Microsoft. U kunt een FQDN-tag in regels voor application gebruiken om toe te staan de vereiste uitgaande netwerkverkeer via de firewall.

Als u wilt toestaan handmatig Windows Update-netwerkverkeer via de firewall, moet u bijvoorbeeld meerdere regels per de Microsoft-documentatie voor application maken. Gebruik van tags van de FQDN-naam, kunt u een toepassing-regel maken, zijn de **Windows Updates** markeren en nu netwerkverkeer naar eindpunten door uw firewall kunnen stromen van Microsoft Windows Update.

U kunt uw eigen labels FQDN-naam niet maken of kunt u opgeven welk FQDN's zijn opgenomen in een tag. Microsoft beheert de FQDN-namen die zijn opgenomen door de FQDN-tag en updates van het label als FQDN's wijzigen. 

<!--- screenshot of application rule with a FQDN tag.-->

De volgende tabel ziet u de huidige FQDN-tags die u kunt gebruiken. Microsoft houdt deze tags en u kunt verwachten dat andere labels worden regelmatig toegevoegd.

## <a name="current-fqdn-tags"></a>Huidige FQDN-tags

|Label van de FQDN-naam  |Description  |
|---------|---------|
|Windows Update     |Toestaan van uitgaande toegang tot Microsoft Update, zoals beschreven in [over het configureren van een Firewall voor Software-Updates](https://technet.microsoft.com/library/bb693717.aspx).|
|Windows Diagnostische gegevens|Uitgaande toegang toestaat aan alle [Windows diagnostische gegevens over eindpunten](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints).|
|MAPS (Microsoft Active Protection Service)|Uitgaande toegang toestaat aan [MAPS](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/).|
|App Service-omgeving (ASE)|Biedt uitgaande toegang tot verkeer met ASE-platform. Deze tag dekt niet klantspecifieke Storage en SQL eindpunten die zijn gemaakt door as-omgeving. Deze moeten worden ingeschakeld [Service-eindpunten](../virtual-network/tutorial-restrict-network-access-to-resources.md) of handmatig worden toegevoegd.<br><br>Zie voor meer informatie over de integratie van Azure-Firewall met ASE [vergrendelen van een App Service Environment](../app-service/environment/firewall-integration.md#configuring-azure-firewall-with-your-ase).|
|Azure Backup|Biedt uitgaande toegang tot de back-up van Azure-services.|
|Azure HDInsight<br>(Preview)|Uitgaande toegang voor HDInsight-platform verkeer toestaat. Deze tag dekt niet klantspecifieke Storage of SQL-verkeer van HDInsight. Schakel deze met [Service-eindpunten](../virtual-network/tutorial-restrict-network-access-to-resources.md) of deze handmatig toevoegen.|

> [!NOTE]
> Bij het selecteren van FQDN code in een toepassing-regel, het veld protocol: poort moet worden ingesteld op **https**.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het implementeren van een Azure-Firewall, [zelfstudie: Implementeren en configureren van de Firewall van Azure met behulp van de Azure-portal](tutorial-firewall-deploy-portal.md).