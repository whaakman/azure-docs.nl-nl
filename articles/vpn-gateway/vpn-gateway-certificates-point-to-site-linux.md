---
title: 'Certificaten genereren en exporteren voor punt-naar-site: Linux: CLI: Azure | Microsoft Docs'
description: Maak een zelfondertekend basis certificaat, Exporteer de open bare sleutel en Genereer client certificaten met behulp van de Linux-CLI (strongSwan).
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/31/2019
ms.author: alzam
ms.openlocfilehash: 79840010561746f2885909123012ffb4efbea5f6
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990317"
---
# <a name="generate-and-export-certificates"></a>Certificaten genereren en exporteren

Punt-naar-site-verbindingen gebruiken certificaten voor verificatie. In dit artikel wordt beschreven hoe u een zelfondertekend basis certificaat maakt en client certificaten genereert met behulp van de Linux CLI en strongSwan. Als u andere certificaat instructies zoekt, raadpleegt u de [Power shell](vpn-gateway-certificates-point-to-site.md) -of [makecert](vpn-gateway-certificates-point-to-site-makecert.md) -artikelen. Voor informatie over het installeren van strongSwan met behulp van de gebruikers interface in plaats van CLI, zie de stappen in het artikel [client configuratie](point-to-site-vpn-client-configuration-azure-cert.md#install) .

## <a name="generate-and-export"></a>Genereren en exporteren
[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Volgende stappen

Ga door met uw punt-naar-site-configuratie om [configuratie bestanden voor VPN-clients te maken en te installeren](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).
