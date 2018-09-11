---
title: 'Genereren en exporteren van certificaten voor punt-naar-Site: Linux: CLI: Azure | Microsoft Docs'
description: Een zelfondertekend basiscertificaat maken, de openbare sleutel exporteren en clientcertificaten genereren via de Linux (strongSwan) CLI.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/06/2018
ms.author: cherylmc
ms.openlocfilehash: 8647b3b3eda980dbd5d5ec368b6b4b13949ecaf1
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44305740"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-linux-strongswan-cli"></a>Genereren en exporteren van certificaten voor punt-naar-Site met behulp van Linux strongSwan CLI

Punt-naar-Site-verbindingen gebruiken certificaten om te verifiëren. Dit artikel ziet u hoe u een zelfondertekend basiscertificaat maken en clientcertificaten genereren via de Linux-CLI en strongSwan. Als u naar ander certificaat-instructies zoekt, Zie de [Powershell](vpn-gateway-certificates-point-to-site.md) of [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) artikelen.

> [!NOTE]
> De stappen in dit artikel is strongSwan vereist.
>

De configuratie van de computer die wordt gebruikt voor de stappen in dit artikel is het volgende:

| | |
|---|---|
|**Computer**| Ubuntu Server 16.04<br>ID_LIKE = debian<br>PRETTY_NAME = "16.04.4 Ubuntu LTS"<br>VERSION_ID = "16.04" |
|**Afhankelijkheden**| apt-get install strongswan ikev2 strongswan-invoegtoepassing-eap-tls<br>apt-get install libstrongswan-standaard-invoegtoepassingen |

## <a name="install-strongswan"></a>StrongSwan installeren

1. `apt-get install strongswan-ikev2 strongswan-plugin-eap-tls`
2. `apt-get install libstrongswan-standard-plugins`

Zie voor meer informatie over het installeren van strongSwan met behulp van de grafische interface van de stappen in de [clientconfiguratie](point-to-site-vpn-client-configuration-azure-cert.md#install) artikel.

## <a name="generate-keys-and-certificate"></a>Genereren van sleutels en certificaten

1. Het CA-certificaat genereren.

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```
2. Afdrukken op de CA-certificaat in Base 64-indeling. Dit is de indeling die wordt ondersteund door Azure. U zult dit later uploaden naar Azure als onderdeel van uw P2S-configuratie.

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```
3. Het gebruikerscertificaat genereren.

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```
4. Genereer een p12-bundel met het gebruikerscertificaat. Deze bundel wordt gebruikt in de volgende stappen bij het werken met de [clientconfiguratiebestanden](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```

## <a name="next-steps"></a>Volgende stappen

Ga door met de punt-naar-Site-configuratie met [Create and install VPN-clientconfiguratiebestanden](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).