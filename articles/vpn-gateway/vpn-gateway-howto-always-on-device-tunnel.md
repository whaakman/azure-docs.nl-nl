---
title: Een altijd op de VPN-tunnel voor VPN-Gateway configureren
description: Stappen voor het configureren van altijd op de VPN-tunnel voor VPN-Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 07/09/2019
ms.author: cherylmc
ms.openlocfilehash: 81822297dcf9370fc8ce7f7ce0285689c31606ce
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67695817"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Een AlwaysOn-VPN-apparaattunnel configureren

Een van de nieuwe functies van de client voor Windows 10 virtuele particuliere netwerk (VPN) is de mogelijkheid om te onderhouden van een VPN-verbinding. Is altijd op een Windows 10-functie waarmee het actieve VPN-profiel voor automatisch verbinding maken en die verbonden blijven op basis van triggers, namelijk aanmelden van gebruikers, wijziging van de netwerkstatus of actieve apparaatscherm.

Virtuele netwerkgateways van Azure kunnen worden gebruikt met Windows 10 Always On tot stand brengen van permanente gebruiker tunnels, evenals de apparaat-tunnels naar Azure. In dit artikel helpt u bij het configureren van een apparaat altijd op VPN-tunnel.

Altijd op de VPN-verbindingen zijn twee soorten tunnels:

* **Apparaat-tunnel** maakt verbinding met de opgegeven VPN-servers voordat gebruikers zich in het apparaat. Voorafgaand aan de aanmelding connectiviteitsscenario's en beheerdoeleinden apparaat gebruik van apparaat-tunnel.

* **Gebruiker-tunnel** verbinding maakt alleen na een aanmeldingsnaam gebruiker het apparaat. Gebruiker tunnel kunnen gebruikers toegang krijgen tot bedrijfsresources via VPN-servers.

Zowel de apparaat-tunnel als de gebruiker tunnel werken onafhankelijk van elkaar met de VPN-profielen. Ze kunnen worden aangesloten op hetzelfde moment en verschillende verificatiemethoden en andere VPN-configuratie-instellingen zo nodig kunnen gebruiken.

## <a name="1-configure-the-gateway"></a>1. De gateway configureren

Configureer de VPN-gateway voor het gebruik van IKEv2 en verificatie op basis van een certificaat met behulp van dit [punt-naar-site-artikel](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

## <a name="2-configure-the-user-tunnel"></a>2. De gebruiker-tunnel configureren

1. Clientcertificaten installeren op de Windows 10-client, zoals wordt weergegeven in deze [punt-naar-site VPN-client artikel](point-to-site-how-to-vpn-client-install-azure-cert.md). Het certificaat moet zich in de huidige gebruiker Store
2. Configureren van de client altijd op VPN via met behulp van PowerShell, SCCM of Intune [deze instructies](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections).

## <a name="3-configure-the-device-tunnel"></a>3. De apparaat-tunnel configureren

De volgende vereisten moeten worden voldaan om een apparaat-tunnel is stand te brengen:

* Het apparaat moet een domein gekoppelde computer met Windows 10 Enterprise- of Education-versie 1709 of hoger.
* De tunnel kan alleen worden geconfigureerd voor de ingebouwde VPN-oplossing voor Windows en tot stand is gebracht IKEv2 met verificatie met computercertificaten gebruiken. 
* Slechts één apparaat tunnel kan worden geconfigureerd per apparaat.

1. Clientcertificaten installeren op de Windows 10-client, zoals wordt weergegeven in deze [punt-naar-site VPN-client artikel](point-to-site-how-to-vpn-client-install-azure-cert.md). Het certificaat moet zich in het archief van de lokale computer.
1. Gebruik [deze instructies](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config#vpn-device-tunnel-configuration) een VPN-profiel maken en configureren van apparaat-tunnel in de context van het lokale SYSTEEMACCOUNT gebruikt.

### <a name="configuration-example-for-device-tunnel"></a>Configuratievoorbeeld van de voor apparaat-tunnel

Nadat u de virtuele netwerkgateway geconfigureerd en het clientcertificaat in het archief van de lokale computer op de Windows 10-client geïnstalleerd, moet u de volgende voorbeelden gebruiken om te configureren wanneer u een tunnel client-apparaat.

1. Kopieer de volgende tekst en sla het bestand als ***devicecert.ps1***.

   ```
   Param(
   [string]$xmlFilePath,
   [string]$ProfileName
   )

   $a = Test-Path $xmlFilePath
   echo $a

   $ProfileXML = Get-Content $xmlFilePath

   echo $XML

   $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

   $Version = 201606090004

   $ProfileXML = $ProfileXML -replace '<', '&lt;'
   $ProfileXML = $ProfileXML -replace '>', '&gt;'
   $ProfileXML = $ProfileXML -replace '"', '&quot;'

   $nodeCSPURI = './Vendor/MSFT/VPNv2'
   $namespaceName = "root\cimv2\mdm\dmmap"
   $className = "MDM_VPNv2_01"

   $session = New-CimSession

   try
   {
   $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
   $newInstance.CimInstanceProperties.Add($property)

   $session.CreateInstance($namespaceName, $newInstance)
   $Message = "Created $ProfileName profile."
   Write-Host "$Message"
   }
   catch [Exception]
   {
   $Message = "Unable to create $ProfileName profile: $_"
   Write-Host "$Message"
   exit
   }
   $Message = "Complete."
   Write-Host "$Message"
   ```
1. Kopieer de volgende tekst en sla het bestand als ***VPNProfile.xml*** in dezelfde map als **devicecert.ps1**. Bewerk de volgende tekst zodat deze overeenkomt met uw omgeving.

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>`
   * `<Address>192.168.3.5</Address>`
   * `<Address>192.168.3.4</Address>`

   ```
   <VPNProfile>  
     <NativeProfile>  
   <Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>  
   <NativeProtocolType>IKEv2</NativeProtocolType>  
   <Authentication>  
     <MachineMethod>Certificate</MachineMethod>  
   </Authentication>  
   <RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
    <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
   <DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
     </NativeProfile> 
     <!-- use host routes(/32) to prevent routing conflicts -->  
     <Route>  
   <Address>192.168.3.5</Address>  
   <PrefixSize>32</PrefixSize>  
     </Route>  
     <Route>  
   <Address>192.168.3.4</Address>  
   <PrefixSize>32</PrefixSize>  
     </Route>  
   <!-- need to specify always on = true --> 
     <AlwaysOn>true</AlwaysOn> 
   <!-- new node to specify that this is a device tunnel -->  
    <DeviceTunnel>true</DeviceTunnel>
   <!--new node to register client IP address in DNS to enable manage out -->
   <RegisterDNS>true</RegisterDNS>
   </VPNProfile>
   ```
1. Download **PsExec** van [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) en pak de bestanden naar **C:\PSTools**.
1. Start PowerShell door uit te voeren vanaf een Admin CMD-prompt:

   ```
   C:\PsTools\PsExec.exe Powershell for 32-bit Windows
   C:\PsTools\PsExec64.exe Powershell for 64-bit Windows
   ```

   ![PowerShell](./media/vpn-gateway-howto-always-on-device-tunnel/powershell.png)
1. Ga in PowerShell naar de map waar **devicecert.ps1** en **VPNProfile.xml** zich bevinden en voer de volgende opdracht uit:

   ```powershell
   C:\> .\devicecert.ps1 .\VPNProfile.xml MachineCertTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-howto-always-on-device-tunnel/machinecerttest.png)
1. Voer **rasphone**.

   ![Rasphone](./media/vpn-gateway-howto-always-on-device-tunnel/rasphone.png)
1. Zoek de **MachineCertTest** vermelding en klikt u op **Connect**.

   ![Verbinding maken](./media/vpn-gateway-howto-always-on-device-tunnel/connect.png)
1. Als de verbinding is geslaagd, moet u de computer opnieuw opstarten. De tunnel wordt automatisch verbinding maken.

## <a name="cleanup"></a>Opschonen

Als u wilt verwijderen van het profiel, moet u de volgende opdracht uitvoeren:

![Opschonen](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor het oplossen van problemen [problemen met Azure point-to-site-verbinding](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
