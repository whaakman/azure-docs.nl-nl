<properties
    pageTitle="De Azure Stack POC implementeren | Microsoft Azure"
    description="Informatie over het voorbereiden van de Azure Stack POC en het uitvoeren van het PowerShell-script voor de implementatie van de Azure Stack POC."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/29/2016"
    ms.author="erikje"/>


# De Azure Stack POC implementeren
Als u de Azure Stack POC wilt implementeren, moet u eerst [de implementatiecomputer voorbereiden](#prepare-the-deployment-machine) en vervolgens [het PowerShell-script voor implementatie uitvoeren](#run-the-powershell-deployment-script).

## De implementatiecomputer voorbereiden

1. Zorg ervoor dat u fysiek verbinding kunt maken met de implementatiecomputer of fysieke toegang hebt tot de console (zoals KVM). U hebt deze toegang nodig nadat u de implementatiecomputer opnieuw hebt opgestart in stap 9.
 
2. Zorg ervoor dat de implementatiecomputer voldoet aan de [minimumvereisten](azure-stack-deploy.md). U kunt het [Implementatiechecker voor Azure Stack Technical Preview 1](https://gallery.technet.microsoft.com/Deployment-Checker-for-76d824e1) gebruiken om te bevestigen dat u voldoet aan de vereisten.

3.  [Download](http://aka.ms/ReqOSforAzureStack) en installeer Windows Server 2016 Datacenter Edition Technical Preview 4 EN-US (volledige editie).

4.  [Download](https://azure.microsoft.com/overview/azure-stack/try/?v=try) het implementatiepakket Azure Stack POC naar een map op de C-schijf (bijvoorbeeld c:\\AzureStack).

5.  Voer het bestand **Microsoft Azure Stack POC.exe** uit.

    Hiermee maakt u de map \\Microsoft Azure Stack POC\\ met de volgende items:

    -   DeployAzureStack.ps1: PowerShell-script voor de installatie van de Azure Stack POC

    -   MicrosoftAzureStackPOC.vhdx: Azure Stack-gegevenspakket

    -   SQLServer2014.vhdx: SQL Server-VHD

    -   WindowsServer2012R2DatacenterEval.vhd

    -   WindowsServer2016Datacenter.vhdx: Windows Server 2016 Datacenter-VHD (inclusief KB 3124262)

    > [AZURE.IMPORTANT] U moet ten minste 128 GB vrije schijfruimte op het fysieke opstartvolume hebben.

6. Kopieer WindowsServer2016Datacenter.vhdxnaar de C:\-schijf en geef het bestand de naam MicrosoftAzureStackPOCBoot.vhdx.

7. Klik in de Verkenner met de rechtermuisknop op MicrosoftAzureStackPOCBoot.vhdx en klik op **Koppelen**.

8. Open een opdrachtpromptvenster als beheerder en voer de onderstaande bcdboot-opdracht uit. Met deze opdracht maakt u een dual-bootomgeving. Vanaf nu moet u de bovenste opstartoptie gebruiken om op te starten.

        bcdboot <mounted drive letter>:\windows

9. Start de computer opnieuw op. Windows Setup wordt automatisch uitgevoerd terwijl het VHD-systeem wordt voorbereid. Hierna moet u fysiek verbinding maken met de implementatiecomputer of fysieke toegang hebben tot de console om de volgende stappen uit te kunnen voeren.

10. Als uw BIOS deze optie bevat, moet u deze configureren zodat de lokale tijd wordt gebruikt in plaats van UTC-tijd.

11. Geef uw land, taal, toetsenbord en andere voorkeuren op wanneer u hierom wordt gevraagd. Als u wordt gevraagd om de productcode, kunt u deze vinden in [Systeemvereisten en installatie](https://technet.microsoft.com/library/mt126134.aspx).

12. Stel het wachtwoord voor het beheerdersaccount in en meld u aan met de accountnaam en het wachtwoord van dit account als hierom wordt gevraagd.

13. Als u geen DHCP hebt, stelt u de statische configuratie in op de NIC nadat u opnieuw hebt opgestart en zich hebt aangemeld.

14. Schakel externe bureaubladverbindingen in door naar de **Systeemeigenschappen** te gaan en de optie **Externe verbindingen met deze computer toestaan** te selecteren.

15. Meld u aan met een lokaal account met beheerdersmachtigingen.

16. Controleer of er **precies** vier schijven voor Azure Stack POC-gegevens:
  - Zichtbaar zijn in Schijfbeheer
  - Niet worden gebruikt
  - Als Online, niet-toegewezen worden weergeven

17. Controleer of de host niet is gekoppeld aan een domein.

18. Gebruik Internet Explorer en controleer de netwerkverbinding met Azure.com.

> [AZURE.IMPORTANT] De implementatie TP1 POC ondersteunt precies vier schijven voor de functies voor opslag en slechts één NIC voor netwerken.
>
> - Gebruik **voor opslag** Apparaatbeheer of WMI om alle andere stations uit te schakelen (waarbij het niet genoeg is om de schijven offline te brengen via Schijfbeheer).
>
> - Zorg er **voor netwerkmogelijkheden** voor dat slechts één NIC is ingeschakeld, als u er meerdere hebt (en dat alle andere zijn uitgeschakeld), voordat u het onderstaande implementatiescript uitvoert.
>
> Als u de bovenstaande stappen gebruikt voor het opstarten van de VHD, moet u deze updates toepassen na het opstarten naar de VHD en voordat u het implementatiescript uitvoert.

## Voer het PowerShell-script voor implementatie uit

1. Verplaats de volgende implementatiebestanden D:\Microsoft Azure Stack POC\ naar C:\Microsoft Azure Stack POC\.
    - DeployAzureStack.ps1
    - MicrosoftAzureStackPOC.vhdx
    - SQLServer14.vhdx
    - WindowsServer2012R2DatacenterEval.vhd
    - WindowsServer2016Datacenter.vhdx

2.  Open PowerShell als beheerder.

3.  Ga in PowerShell naar de locatie van Azure Stack (\\Microsoft Azure Stack POC\\ als u de standaardlocatie gebruikt).

4.  Voer de implementatieopdracht uit:

        .\DeployAzureStack.ps1 –Verbose

    In China gebruikt u de volgende opdracht:

        .\DeployAzureStack.ps1 –Verbose -UseAADChina $true

    De implementatie wordt gestart en de domeinnaam Azure Stack POC wordt vastgelegd als azurestack.local.

5.  Voer in de prompt **Voer het wachtwoord voor de ingebouwde beheerder in** een wachtwoord in en bevestig het. Dit is het wachtwoord voor alle virtuele machines. Noteer dit servicebeheerderswachtwoord.

6.  Druk bij **Meld u aan bij uw Azure-account op de pop-uppagina voor verificatie in Azure** op een willekeurige toets om het dialoogvenster Aanmelden bij Microsoft Azure te openen.

7.  Voer de aanmeldingsgegevens voor uw Azure Active Directory-account in. Deze gebruiker moet de globale beheerder in de directory-tenant zijn

8.  Voer *y* in bij de bevestigingsprompt voor accountselectie in PowerShell. Hiermee maakt u twee gebruikers en drie toepassingen voor Azure Stack in deze directory-tenant: een beheerder voor Azure-Stack, een tenant-gebruiker voor de TiP-testen en één aanvraag per provider voor de resourceproviders Portal, API en Bewaking. Daarnaast voegt het installatieprogramma toestemmingen voor Azure PowerShell, XPlat CLI en Visual Studio toe aan die directory-tenant.

9.  Voer bij **Microsoft Azure Stack POC is gereed om te implementeren. Doorgaan?** *y* in.

10.  Het implementatieproces duurt een paar uur, waarin het systeem verschillende keren automatisch opnieuw wordt opgestart. Als u zich aanmeldt tijdens de implementatie, wordt automatisch een PowerShell-venster geopend waarin de voortgang van de implementatie wordt weergegeven. Het PowerShell-venster wordt gesloten nadat de implementatie is voltooid.

11. Meld u op de computer met de Azure Stack POC aan als een AzureStack\beheerder, open **Serverbeheer** en schakel **Verbeterde beveiliging van Internet Explorer** uit voor beheerders en gebruikers.

Als de implementatie is mislukt en er een datum- of tijdfout wordt weergegeven, configureert u de BIOS zodat de lokale tijd wordt gebruikt in plaats van UTC. Voer de implementatie vervolgens opnieuw uit.

Start het script opnieuw als het mislukt. Als de pogingen blijven mislukken, wist u het script en begint u opnieuw.

U vindt de scriptlogboeken op de POC-host `C:\ProgramData\microsoft\azurestack`.

### Optionele parameters voor DeployAzureStack.ps1

**AADCredential** (PSCredential) - stelt de gebruikersnaam en het wachtwoord voor Azure Active Directory in. Als deze parameter niet is opgegeven, vraagt het script om de gebruikersnaam en het wachtwoord.

**AADTenant** (string) - stelt de tenant-directory in. Gebruik deze parameter om een specifieke map op te geven wanneer het AAD-account gemachtigd is om meerdere mappen te beheren. Als deze parameter niet is opgegeven, vraagt het script om de map.

**AdminPassword** (SecureString) - stelt het standaard beheerderswachtwoord in. Als deze parameter niet is opgegeven, vraagt het script om het wachtwoord.

**Force** (Switch) - stelt de cmdlet zo in dat deze wordt uitgevoerd zonder bevestigingen.

**NATVMStaticGateway** (String) - stelt de standaardgateway in die wordt gebruikt in het statische IP-adres voor de NATVM. Gebruik deze parameter alleen als de DHCP geen geldig IP-adres voor toegang tot het internet kan toewijzen. Als deze parameter wordt gebruikt, moet u ook de parameter NATVMStaticIP gebruiken.
Bijvoorbeeld: `.\DeployAzureStack.ps1 –Verbose -NATVMStaticIP 10.10.10.10/24 – NATVMStaticGateway 10.10.10.1`

**NATVMStaticIP** (string) - stelt een extra statisch IP-adres voor de NATVM in. Gebruik deze parameter alleen als de DHCP geen geldig IP-adres voor toegang tot het internet kan toewijzen.
Bijvoorbeeld: `.\DeployAzureStack.ps1 –Verbose -NATVMStaticIP 10.10.10.10/24`

**NoAutoReboot** (Switch) - stelt het script zo in dat het wordt uitgevoerd zonder dat automatisch opnieuw wordt opgestart.

**ProxyServer** (String) - stelt de proxygegevens in. Gebruik deze parameter alleen als uw omgeving een proxy moet gebruiken voor toegang tot het internet. Proxyservers die aanmeldingsgegevens gebruiken, worden niet ondersteund.
Bijvoorbeeld: `.\DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080`

**PublicVLan** (String) - stelt de VLAN-ID in. Gebruik deze parameter alleen als de host en NATVM de VLAN-ID moeten configureren voor toegang tot het fysieke netwerk (en internet).
Bijvoorbeeld: `.\DeployAzureStack.ps1 –Verbose –PublicVLan 305`

**TIPServiceAdminCredential** (PSCredential) - stelt de aanmeldingsgegevens in van een bestaand Azure Active Directory-account van een servicebeheerder. Dit account wordt gebruikt door TiP (Test in Production). Als deze parameter niet is opgegeven, wordt er automatisch een account gemaakt.

**TIPTenantAdminCredential** (PSCredential) - stelt de aanmeldingsgegevens in van een bestaand Azure Active Directory-account van een tenantbeheerder. Dit account wordt gebruikt door TiP (Test in Production). Als deze parameter niet is opgegeven, wordt er automatisch een account gemaakt.

**UseAADChina**(Boolean) - stel deze Booleaanse parameter in op $true als u de Microsoft Azure Stack POC met Azure China (Mooncake) wilt implementeren.

## Automatische TiP-tests uitschakelen (optioneel)

Microsoft Azure Stack Technical Preview 1 bevat een set validatietesten die kunnen worden gebruikt tijdens het implementatieproces en volgens een terugkerend dagelijks schema. Deze testen simuleren acties die worden uitgevoerd door een Azure Stack-tenant. Er worden gebruikersaccounts voor Test-in-POC (TiP) gemaakt in uw Azure Active Directory om de testen uit te voeren. U kunt deze TiP-testen uitschakelen na een geslaagde implementatie. 

**Automatische TiP-testen uitschakelen**

  - Voer de volgende cmdlet uit op de ClientVM:

  `Disable-ScheduledTask -TaskName AzureStackSystemvalidationTask`

**De resultaten weergeven**

  - Voer de volgende cmdlet uit op de ClientVM:

  `Get-AzureStackTiPTestsResult`



## Telemetrie voor Microsoft Azure Stack POC uitschakelen (optioneel)


Voordat u de Microsoft Azure Stack POC implementeert, kunt u telemetrie uitschakelen voor Microsoft Azure Stack op de computer waarmee de implementatie wordt uitgevoerd. Als u deze functie wilt uitschakelen op één computer, raadpleegt u: [http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq](http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq) en stelt u de instelling **Diagnostische gegevens en gebruiksgegevens** in op **Basic**.



U kunt na de implementatie van de Microsoft Azure Stack POC telemetrie uitschakelen voor alle virtuele machines die zijn toegevoegd aan het Azure Stack-domein. Als u een groepsbeleid wilt maken en uw telemetrie-instellingen op deze virtuele machines wilt beheren, raadpleegt u: [https://technet.microsoft.com/library/mt577208(v=vs.85).aspx\#BKMK\_UTC](https://technet.microsoft.com/library/mt577208%28v=vs.85%29.aspx#BKMK_UTC) en selecteert u **0** of **1** voor het groepsbeleid **Telemetrie toestaan**. Er zijn twee virtuele machines (bgpvm en natvm) die niet worden toegevoegd aan het Azure Stack-domein. Als u de instellingen van Diagnostische gegevens en gebruiksgegevens afzonderlijk wilt wijzigen op deze virtuele machines, raadpleegt u:  [http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq](http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq).

## Volgende stappen

[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)



<!--HONumber=Sep16_HO3-->


