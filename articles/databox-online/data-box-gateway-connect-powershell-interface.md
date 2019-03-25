---
title: Verbinding maken met en beheren van Microsoft Azure Data Box-Gateway-apparaat via de Windows PowerShell-interface | Microsoft Docs
description: Beschrijft hoe u verbinding maken met en gegevens in het Gateway vervolgens beheren via de Windows PowerShell-interface.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/21/2019
ms.author: alkohli
ms.openlocfilehash: 67caaa2c6c9bd615d0b88bdd5de3442b46aa32cb
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403490"
---
# <a name="manage-an-azure-data-box-gateway-device-via-windows-powershell"></a>Een Azure Data Box-Gateway-apparaat via Windows PowerShell beheren

Azure Data Box-Gateway-oplossing kunt u gegevens naar Azure verzenden via het netwerk. Dit artikel worden enkele van de configuratie en beheer taken voor uw gegevens in het Gateway-apparaat. U kunt de Azure portal, de lokale web-UI of de Windows PowerShell-interface gebruiken om uw apparaat te beheren.

In dit artikel richt zich op de taken die u met behulp van de PowerShell-interface.

In dit artikel bevat de volgende procedures:

- Verbinding maken met de PowerShell-interface
- Een ondersteuningssessie starten
- Een ondersteuningspakket maken
- Certificaat uploaden
- Opstarten in niet-DHCP-omgeving
- Gegevens van een apparaat weergeven

## <a name="connect-to-the-powershell-interface"></a>Verbinding maken met de PowerShell-interface

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Een ondersteuningspakket maken

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Certificaat uploaden

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="boot-up-in-non-dhcp-environment"></a>Opstarten in niet-DHCP-omgeving

[!INCLUDE [Boot up in non-DHCP environment](../../includes/data-box-edge-gateway-boot-non-dhcp.md)]

## <a name="view-device-information"></a>Gegevens van een apparaat weergeven

[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="next-steps"></a>Volgende stappen

- Implementeer [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) in de Azure Portal.
