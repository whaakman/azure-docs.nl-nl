---
title: Overgang van zelf-ondertekend naar openbare CA-certificaten voor P2S-gateways | Azure VPN-Gateway | Microsoft Docs
description: Dit artikel helpt u met succes worden overgezet naar de nieuwe openbare CA-certificaten voor P2S-gateways.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: cherylmc
ms.openlocfilehash: a5634054746d7e60aff154165f651f7c815c39a3
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100819"
---
# <a name="transition-from-self-signed-to-public-ca-certificates-for-p2s-gateways"></a>Overgang van zelf-ondertekend naar openbare CA-certificaten voor P2S-gateways

Azure VPN-Gateway wordt niet meer zelfondertekende certificaten verstrekt aan gateways voor P2S-verbindingen. Uitgegeven certificaten zijn nu ondertekend door een openbare certificeringsinstantie (CA). Echter, oude gateways mogelijk nog steeds gebruik van zelfondertekende certificaten. Deze zelfondertekende certificaten zijn in de buurt van de vervaldatum en moeten worden overgezet naar een openbare CA-certificaten.

Eerder, zelfondertekend certificaat voor de gateway nodig om 18 maanden worden bijgewerkt. VPN-clientconfiguratiebestanden moest vervolgens worden gegenereerd en wordt opnieuw geïmplementeerd op alle P2S-clients. Verplaatsen naar een openbare CA-certificaten wordt voorkomen dat deze beperking. Naast de overgang voor certificaten biedt deze wijziging ook platform verbeteringen, betere metrische gegevens en de stabiliteit is verbeterd.

Alleen oude gateways worden beïnvloed door deze wijziging. Als uw gatewaycertificaat worden overgezet moet, ontvangt u communicatie of pop-up in Azure portal. U kunt controleren om te zien als uw gateway wordt beïnvloed door met de stappen in dit artikel.

>[!IMPORTANT]
>De overgang is gepland voor maart 12,2019 starten om 18:00 UTC. Als u liever een andere tijdvenster, kunt u een ondersteuningsaanvraag maken. U kunt een van de volgende vragen:
>
>* 06:00 UTC op 25 februari
>* 18:00 UTC op 25 februari
>* 06:00 UTC op 1 maart
>* 18:00 UTC op 1 maart
>
>**Alle resterende gateways veranderen maart 12 2019 vanaf 18:00 UTC**.
>

## <a name="1-verify-your-certificate"></a>1. Controleer of het certificaat

1. Controleer als u worden beïnvloed door deze update. Downloaden van uw huidige VPN-clientconfiguratie met behulp van de stappen in [in dit artikel](point-to-site-vpn-client-configuration-azure-cert.md).

2. Open of pak het zipbestand en blader naar de map "Generic". In de algemene map, ziet u twee bestanden, van de een *VPNSettings.xml*.
3. Open *VPNSettings.xml* in een xml-viewer/editor. In het xml-bestand, zoek naar de volgende velden:

  * `<ServerCertRootCn>DigiCert Global Root CA</ServerCertRootCn>`
  * `<ServerCertIssuerCn>DigiCert Global Root CA</ServerCertIssuerCn>`
4. Als *ServerCertRotCn* en *ServerCertIssuerCn* 'DigiCert globale basis-CA' zijn, u niet worden beïnvloed door deze update en u niet wilt doorgaan met de stappen in dit artikel. Als ze iets anders worden weergegeven, het gatewaycertificaat maakt deel uit van de update en overgezet.

## <a name="2-check-certificate-transition-schedule"></a>2. Certificaat overgang planning controleren

Als uw certificaat onderdeel van de update is, wordt het gatewaycertificaat worden overgezet. Raadpleeg de **belangrijk** Opmerking voor overgangstijden. Nadat de update, P2S-clients niet mogelijk om verbinding met hun oude profiel te maken. U moet nieuwe profielen voor VPN-client genereren en te installeren op clients.

## <a name="3-generate-vpn-client-configuration-profile"></a>3. VPN-client-configuratieprofiel genereren

Zodra het certificaat is overgegaan, download u het nieuwe VPN-profiel (VPN-clientconfiguratiebestanden) vanuit de Azure-portal. Zie voor stappen [Create and install VPN-clientconfiguratiebestanden](point-to-site-vpn-client-configuration-azure-cert.md). U hoeft niet om nieuwe clientcertificaten te genereren.

## <a name="4-deploy-vpn-client-profile"></a>4. VPN-client-profiel implementeren

Implementeer het nieuwe profiel op alle clients voor punt-naar-site-VPN. VPN-clients geen verbinding maken totdat de nieuwe VPN-profiel voor punt-naar-site-verbindingen is gedownload en geïmplementeerd voor clientapparaten. De clientcertificaten die al zijn geïnstalleerd op de VPN-clientcomputers hoeft geen te worden uitgegeven.

## <a name="5-connect-the-vpn-client"></a>5. Verbinding maken met de VPN-client

Verbinding met Azure via de VPN-client zoals u gewend bent.
