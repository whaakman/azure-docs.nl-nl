---
title: 'Maken en installeren van de configuratiebestanden voor P2S-VPN-clients voor verificatie van Azure: PowerShell: Azure | Microsoft Docs'
description: Maken en installeren van Windows en Mac OS X-VPN-client configuratiebestanden voor verificatie via certificaat P2S.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: b18e01a780f6371ecae3298a6f3f41949090b9e8
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-point-to-site-configurations"></a>Maken en installeer VPN client configuratiebestanden voor systeemeigen Azure authentication punt-naar-Site-configuraties van het certificaat

Configuratiebestanden voor VPN-clients bevinden zich in een zip-bestand. Configuratiebestanden bevatten de instellingen die vereist zijn voor een native client voor Windows of Mac IKEv2 VPN-verbinding maken met een VNet via punt-naar-Site-verbindingen die gebruikmaken van systeemeigen Azure certificaatverificatie.

### <a name="workflow"></a>P2S-werkstroom

  1. De Azure VPN-gateway voor een P2S-verbinding instellen.
  2. Basiscertificaat en clientcertificaten te genereren. De openbare sleutel van het root-certificaat uploaden naar Azure en de clientcertificaten installeren op de clientapparaten. De certificaten worden gebruikt voor het verifiëren van gebruikers verbinding probeert te maken.
  3. De configuratie van de VPN-client voor de verificatieoptie van uw keuze verkrijgen en gebruiken voor het instellen van de VPN-client op uw Windows- en Mac-apparaten. Hiermee kunt u verbinding maken met Azure VNets via een punt-naar-Site-verbinding.

>[!NOTE]
>Configuratiebestanden voor clients zijn specifiek voor de VPN-configuratie voor het VNet. Als er wijzigingen in de punt-naar-Site VPN-configuratie na het genereren van de VPN-client configuratiebestanden, zoals het VPN-protocoltype of verificatie, moet u voor het genereren van nieuwe VPN-client-configuratiebestanden voor de apparaten van gebruikers.
>
>

## <a name="generate"></a>Configuratiebestanden voor VPN-clients genereren

Voordat u begint, zorg ervoor dat alle verbindende gebruikers een geldig certificaat geïnstalleerd op het apparaat van de gebruiker hebben. Zie voor meer informatie over het installeren van een clientcertificaat [een clientcertificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md).

U kunt de configuratiebestanden voor clients met behulp van PowerShell, genereren of met behulp van de Azure-portal. De methode retourneert het dezelfde zip-bestand. Pak het bestand om weer te geven van de volgende mappen:

  * **WindowsAmd64** en **WindowsX86**, die het installatieprogramma voor Windows 32-bits en 64-bits pakketten, respectievelijk bevatten. De **WindowsAmd64** installer-pakket is voor alle 64-bits Windows-clients niet alleen AMD-processors ondersteunde.
  * **Algemene**, bevat algemene informatie gebruikt om de configuratie van uw eigen VPN-client te maken. Deze map negeren. De algemene map is opgegeven als IKEv2 of SSTP + IKEv2 op de gateway is geconfigureerd. Als er slechts SSTP is geconfigureerd, klikt u vervolgens is de algemene map niet aanwezig.

### <a name="zipportal"></a>Bestanden met de Azure portal genereren

1. Ga in de Azure-portal naar de virtuele netwerkgateway voor het virtuele netwerk dat u verbinding wilt maken.
2. Klik op de pagina van de gateway virtuele netwerk **punt-naar-site-configuratie**.
3. Aan de bovenkant van de configuratiepagina voor punt-naar-site, klikt u op **downloaden VPN-client**. Het duurt enkele minuten voor de client configuratiepakket voor het genereren van.
4. Uw browser geeft aan dat het zip-bestand van een client-configuratie beschikbaar is. Dit is dezelfde naam als uw gateway. Pak het bestand om de mappen weer te geven.

### <a name="zipps"></a>Bestanden met PowerShell genereren

1. Bij genereren van het VPN-clientconfiguratie van bestanden, de waarde voor '-AuthenticationMethod' is 'EapTls'. Genereren van de configuratiebestanden van de VPN-client met de volgende opdracht:

  ```powershell
  $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

  $profile.VPNProfileSASUrl
  ```
2. Kopieer de URL naar uw browser om te downloaden van het zip-bestand en pak het bestand om de mappen weer te geven.

## <a name="installwin"></a>Een configuratiepakket van de Windows VPN-client installeren

Als de versie overeenkomt met de architectuur voor de client, kunt u de dezelfde configuratiepakket voor VPN-client op elke Windows-clientcomputer. Voor de lijst met client-besturingssystemen die worden ondersteund, Zie de sectie punt-naar-Site van de [Veelgestelde vragen over het VPN-Gateway](vpn-gateway-vpn-faq.md#P2S).

Gebruik de volgende stappen uit om de systeemeigen Windows VPN-client voor verificatie via certificaat te configureren:

1. Selecteer de configuratiebestanden voor VPN-client die met de architectuur van de Windows-computer overeenkomen. Kies het 'VpnClientSetupAmd64' installer-pakket voor een 64-bits processorarchitectuur. Kies het 'VpnClientSetupX86' installer-pakket voor een 32-bits processorarchitectuur. 
2. Dubbelklik op het pakket om het te installeren. Als u een SmartScreen-melding ziet, klikt u op **Meer info** en vervolgens op **Toch uitvoeren**.
3. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. 
4. Voordat u probeert om verbinding te controleren of kunt u een clientcertificaat hebt geïnstalleerd op de clientcomputer. Een clientcertificaat is vereist voor verificatie wanneer u het verificatietype systeemeigen Azure certificaat. Zie voor meer informatie over het genereren van certificaten [certificaten genereren](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Zie voor meer informatie over het installeren van een clientcertificaat [een clientcertificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="installmac"></a>Configuratie van VPN-client op Macs (OS X)

Azure biedt geen mobileconfig-bestand voor systeemeigen Azure certificaatverificatie. U moet de systeemeigen IKEv2 VPN-client handmatig te configureren op elke Mac die met Azure verbinden zullen. De **algemene** map bevat alle informatie die u moet deze configureren. Als u de algemene map in uw download niet ziet, is het waarschijnlijk dat IKEv2 niet is geselecteerd als een tunneltype. Nadat IKEv2 is geselecteerd, genereert het zip-bestand om opnieuw op te halen van de algemene map. De algemene map bevat de volgende bestanden:

* **VpnSettings.xml**, die belangrijke instellingen zoals het adres en de tunnel servertype bevat. 
* **VpnServerRoot.cer**, die het basiscertificaat dat is vereist voor de validatie van de Azure VPN-Gateway tijdens de installatie voor P2S-verbinding bevat.

Gebruik de volgende stappen voor het configureren van de systeemeigen VPN-client op Mac voor verificatie via certificaat. U hebben moet de volgende stappen uit op elke Mac die met Azure verbinden zullen:

1. Importeer de **VpnServerRoot** basiscertificaat voor de Mac. Dit kan worden gedaan door het bestand kopiëren via uw Mac en erop te dubbelklikken.  
Klik op **toevoegen** om te importeren.

  ![certificaat toevoegen](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Te dubbelklikken op het certificaat mogelijk niet weergegeven de **toevoegen** dialoogvenster, maar het certificaat is geïnstalleerd in de juiste opslag. U kunt controleren voor het certificaat in de sleutelketen aanmelding onder de categorie van certificaten.
  
2. Controleer of u een clientcertificaat dat is uitgegeven door het basiscertificaat dat u hebt geüpload naar Azure wanneer u u P2S-instellingen geconfigureerd hebt geïnstalleerd. Dit wijkt af van de VPNServerRoot die u in de vorige stap hebt geïnstalleerd. Het clientcertificaat wordt gebruikt voor verificatie en is vereist. Zie voor meer informatie over het genereren van certificaten [certificaten genereren](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Zie voor meer informatie over het installeren van een clientcertificaat [een clientcertificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Open de **netwerk** dialoogvenster onder **Netwerkvoorkeuren** en klik op **'+'** voor het maken van een nieuwe VPN-client verbindingsprofiel voor P2S verbinding met het Azure VNet.

  De **Interface** waarde is 'VPN' en **VPN-Type** waarde is 'IKEv2'. Geef een naam voor het profiel in de **servicenaam** veld en klik vervolgens op **maken** het profiel voor VPN-client verbinding maken.

  ![Netwerk](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
4. In de **algemene** map van de **VpnSettings.xml** bestand, kopieert u de **VpnServer** tagwaarde. Plak deze waarde in de **serveradres** en **externe ID** velden van het profiel.

  ![Server-info](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
5. Klik op **verificatie-instellingen** en selecteer **certificaat**. 

  ![verificatie-instellingen](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
6. Klik op **selecteren...** om te kiezen in het clientcertificaat dat u wilt gebruiken voor verificatie. Dit is het certificaat dat u in stap 2 hebt geïnstalleerd.

  ![certificaat](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
7. **Kies een identiteit** geeft een lijst met certificaten die u kunt kiezen uit. Het juiste certificaat selecteren en klik vervolgens op **doorgaan**.

  ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
8. In de **lokale ID** veld, geef de naam van het certificaat (uit stap 6). In dit voorbeeld is het 'ikev2Client.com'. Klik vervolgens op **toepassen** knop de wijzigingen wilt opslaan.

  ![toepassen](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
9. Op de **netwerk** dialoogvenster, klikt u op **toepassen** alle wijzigingen op te slaan. Klik vervolgens op **Connect** starten van de P2S-verbinding met het Azure VNet.

## <a name="next-steps"></a>Volgende stappen

Ga terug naar het artikel [uw P2S-configuratie voltooien](vpn-gateway-howto-point-to-site-rm-ps.md).

Voor P2S informatie over probleemoplossing, [punt-naar-site-verbindingen voor probleemoplossing voor Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).