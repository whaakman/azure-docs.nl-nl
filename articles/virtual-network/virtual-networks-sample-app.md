---
title: Azure-voorbeeldtoepassing voor gebruik met DMZ's | Microsoft Docs
description: Deze eenvoudige webtoepassing na het maken van een DMZ voor het testen van verkeer stroom scenario's implementeren
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 60340ab7-b82b-40e0-bd87-83e41fe4519c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 8506238e41c5d9dac8d76d729d4919b30a0528b9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sample-application-for-use-with-dmzs"></a>Voorbeeld van een toepassing voor gebruik met DMZ 's
[Ga terug naar de grens van Best Practices beveiligingspagina][HOME]

Deze PowerShell-scripts kunnen lokaal worden uitgevoerd op de servers IIS01 en AppVM01 installeren en een eenvoudige webtoepassing die wordt weergegeven een HTML-pagina van de front-IIS01-server met inhoud van de back-endserver van AppVM01 instellen.

Deze toepassing biedt een eenvoudige testomgeving voor veel van de voorbeelden DMZ en hoe wijzigingen op de eindpunten, nsg's, UDR en Firewall regels verkeersstromen kunnen beïnvloeden.

## <a name="firewall-rule-to-allow-icmp"></a>Firewallregel voor het toestaan van ICMP
Deze eenvoudige PowerShell-instructie kan worden uitgevoerd op een Windows-VM naar ICMP (Ping)-verkeer. Deze update firewall kunt u gemakkelijker testen en het oplossen van problemen doordat de ping-protocol door te geven via de windows firewall (voor de meeste Linux-distributies die ICMP standaard ingeschakeld is).

```PowerShell
# Turn On ICMPv4
New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
    -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
```

Als u de volgende scripts gebruikt, is deze toevoeging firewall-regel voor de eerste instructie.

## <a name="iis01---web-application-installation-script"></a>IIS01 - script voor installatie van Web application
Dit script wordt:

1. Open IMCPv4 (Ping) op de lokale server windows firewall voor het testen van eenvoudiger
2. Installatie van IIS en de .net Framework v4.5
3. Een ASP.NET-webpagina en een Web.config-bestand maken
4. Wijzig de standaardgroep van toepassingen om toegang tot het bestand te vereenvoudigen
5. De anonieme gebruiker ingesteld op uw beheerdersaccount en het wachtwoord

Deze PowerShell-script moet lokaal worden uitgevoerd terwijl er RDP in IIS01 was.

```PowerShell
# IIS Server Post Build Config Script
# Get Admin Account and Password
    Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
    $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
    $thePassword = Read-Host -Prompt "The Admin Password"

# Turn On ICMPv4
    Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Install IIS
    Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
    add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console

# Create Web App Pages
    Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
    $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
    <%@ Import Namespace="System.IO" %>
    <script language="vb" runat="server">
        Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
            Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
            Dim objStreamReader As StreamReader
            objStreamReader = File.OpenText(FILENAME)
            Dim contents As String = objStreamReader.ReadToEnd()
            lblOutput.Text = contents
            objStreamReader.Close()
            lblTime.Text = Now()
        End Sub
    </script>

    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
        <title>DMZ Example App</title>
    </head>
    <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
      <form id="frmMain" runat="server">
        <div>
          <h1>Looks like you made it!</h1>
          This is a page from the inside (a web server on a private network),<br />
          and it is making its way to the outside! (If you are viewing this from the internet)<br />
          <br />
          The following sections show:
          <ul style="margin-top: 0px;">
            <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
            <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
            <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
          </ul>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Image File Linked from the Internet</b>:<br />
            <br />
            <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
        </div>
      </form>
    </body>
    </html>'

    $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.web>
        <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
        <httpRuntime targetFramework="4.5" />
        <identity impersonate="true" />
        <customErrors mode="Off"/>
      </system.web>
      <system.webServer>
        <defaultDocument>
          <files>
            <add value="Home.aspx" />
          </files>
        </defaultDocument>
      </system.webServer>
    </configuration>'

    $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
    $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii

# Set App Pool to Clasic Pipeline to remote file access will work easier
    Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
    c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
    c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost

# Make sure the IIS settings take
    Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
    Restart-Service -Name W3SVC

    Write-Host
    Write-Host "Web App Creation Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="appvm01---file-server-installation-script"></a>AppVM01 - script voor installatie van bestand server
Dit script stelt u de back-end voor deze eenvoudige toepassing. Dit script wordt:

1. Open IMCPv4 (Ping) op de firewall voor het testen van eenvoudiger
2. Maak een map voor de website
3. Maak een tekstbestand worden op afstand toegang op basis van de webpagina
4. Machtigingen instellen voor de mappen en bestanden voor anonieme toegang toestaan
5. Verbeterde beveiliging van Internet Explorer om toe te staan eenvoudiger kunt bladeren vanaf deze server uitschakelen 

> [!IMPORTANT]
> **Kunt u beter**: nooit verbeterde beveiliging van Internet Explorer uitschakelen op een productieserver, plus het is doorgaans een goed idee surfen op Internet vanaf een productieserver. Openen van bestandsshares voor anonieme toegang is ook een goed idee, maar gereed hier voor eenvoud.
> 
> 

Deze PowerShell-script moet lokaal worden uitgevoerd terwijl er RDP in AppVM01 was. PowerShell is vereist om te worden uitgevoerd als beheerder om te controleren of de uitvoering te doen slagen.

```PowerShell
# AppVM01 Server Post Build Config Script
# PowerShell must be run as Administrator for Net Share commands to work

# Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Create Directory
    New-Item "C:\WebShare" -ItemType Directory

# Write out Rand.txt
    $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
    $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii

# Set Permissions on share
    $Acl = Get-Acl "C:\WebShare"
    $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
    $Acl.SetAccessRule($AccessRule)
    Set-Acl "C:\WebShare" $Acl

# Create network share
    Net Share WebShare=C:\WebShare "/grant:Everyone,READ"

# Turn Off IE Enhanced Security Configuration for Admins
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0

    Write-Host
    Write-Host "File Server Set up Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="dns01---dns-server-installation-script"></a>DNS01 - script voor installatie van DNS-server
Er is geen script dat is opgenomen in deze voorbeeldtoepassing voor het instellen van de DNS-server. Als het testen van de firewall-regels, NSG of UDR moet bevatten van DNS-verkeer, wordt de server DNS01 moet handmatig worden ingesteld. De configuratie van de xml-bestand en Resource Manager-sjabloon voor beide voorbeelden bevat DNS01 als de primaire DNS-server en de openbare DNS-server die door niveau 3 als de back-DNS-server worden gehost. De niveau 3-DNS-server zou worden de DNS-server gebruikt voor niet-lokale verkeer en met DNS01 niet ingesteld, geen lokale netwerk DNS zou optreden.

## <a name="next-steps"></a>Volgende stappen
* Het script IIS01 uitvoeren op een IIS-server
* File Server-script uitvoeren op AppVM01
* Blader naar het openbare IP-adres op IIS01 build valideren

<!--Link References-->
[HOME]: ../best-practices-network-security.md
