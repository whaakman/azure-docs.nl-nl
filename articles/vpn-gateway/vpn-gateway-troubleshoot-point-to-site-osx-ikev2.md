---
title: Problemen met Azure punt-naar-Site VPN-verbindingen van clients voor Mac OS X | Microsoft Docs
description: Procedures voor het oplossen van P2S Mac OS X-VPN-client-verbindingen
services: vpn-gateway
documentationcenter: na
author: anzaman
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: alzam
ms.openlocfilehash: 1cf8195cbf65f27c71a4db18c0c61c8a25673acd
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
ms.locfileid: "30248161"
---
# <a name="troubleshoot-point-to-site-vpn-connections-from-mac-os-x-vpn-clients"></a>Problemen met punt-naar-Site VPN-verbindingen van Mac OS X-VPN-clients

In dit artikel helpt bij het oplossen van problemen met de netwerkverbinding van Mac OS X met de systeemeigen VPN-client en IKEv2 punt-naar-Site. De VPN-client in Mac voor IKEv2 is een zeer eenvoudige en staat niet toe dat veel aanpassingen. Er zijn slechts vier instellingen die moeten worden gecontroleerd:

* Serveradres
* Externe-ID
* Lokale ID
* Verificatie-instellingen
* De versie van het besturingssysteem (10.11 of hoger)


## <a name="VPNClient"></a> Problemen met certificaten gebaseerde verificatie
1. Controleer de instellingen van de VPN-client. Ga naar de **netwerkinstelling** door te drukken opdracht + Shift en typ 'VPN' om te controleren van de instellingen van de VPN-client. In de lijst, klikt u op de VPN-vermelding die moet worden onderzocht.

  ![IKEv2-certificaat gebaseerde verificatie](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2cert1.jpg)
2. Controleer de **serveradres** is de volledige FQDN en de cloudapp.net bevat.
3. De **externe ID** moet hetzelfde zijn als het serveradres (Gateway FQDN).
4. De **lokale ID** moet hetzelfde zijn als de **onderwerp** van het clientcertificaat.
5. Klik op **verificatie-instellingen** om de pagina verificatie-instellingen te openen.

  ![Verificatie-instellingen](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth2.jpg)
6. Controleer **certificaat** is geselecteerd in de vervolgkeuzelijst.
7. Klik op de **Selecteer** knop en controleer of het juiste certificaat is geselecteerd. Klik op **OK** wijzigingen wilt opslaan.

## <a name="ikev2"></a>Problemen met de gebruikersnaam en wachtwoord voor verificatie

1. Controleer de instellingen van de VPN-client. Ga naar de **netwerkinstelling** door te drukken opdracht + Shift en typ 'VPN' om te controleren van de instellingen van de VPN-client. In de lijst, klikt u op de VPN-vermelding die moet worden onderzocht.

  ![IKEv2 gebruikersnaamwachtwoord](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2user3.jpg)
2. Controleer de **serveradres** is de volledige FQDN en de cloudapp.net bevat.
3. De **externe ID** moet hetzelfde zijn als het serveradres (Gateway FQDN).
4. De **lokale ID** kan leeg zijn.
5. Klik op de **verificatie-instelling** knop en controleer of 'Gebruikersnaam' is geselecteerd in de vervolgkeuzelijst.

  ![Verificatie-instellingen](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/ikev2auth4.jpg)
6. Controleer of de juiste referenties worden opgegeven.

## <a name="additional"></a>Extra stappen

Als u de vorige stappen en alles juist is geconfigureerd, downloadt u [Wireshark](https://www.wireshark.org/#download) en een pakketopname uit te voeren.

1. Filteren op *iskmp* en bekijk de **IKE_SA** pakketten. U moet kunnen om te kijken naar de details van SA voorstel onder de **nettolading: beveiligingskoppeling**. 
2. Controleer of de client en de server een gemeenschappelijke set hebben.

  ![Pakket](./media/vpn-gateway-troubleshoot-point-to-site-osx-ikev2/packet5.jpg)

## <a name="next-steps"></a>Volgende stappen
Zie voor aanvullende hulp [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
