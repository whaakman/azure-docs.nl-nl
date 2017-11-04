---
title: De Azure-Stack Development Kit implementeren | Microsoft Docs
description: Informatie over het voorbereiden van de Azure-Stack Development Kit en voer de PowerShell-script voor het implementeren van deze.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/17/2017
ms.author: erikje
ms.openlocfilehash: b67cabf0ecdb48f137bfcfbce95eee568a1c298d
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>De Azure-Stack Development Kit implementeren

*Van toepassing op: Azure stapelen Development Kit*

Voor het implementeren van de [Azure Stack Development Kit](azure-stack-poc.md), moet u de volgende stappen:

1. [Het implementatiepakket downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try) de Cloudbuilder.vhdx ophalen.
2. [Voorbereiden van de cloudbuilder.vhdx](#prepare-the-development-kit-host) door het uitvoeren van het script asdk installer.ps1 voor het configureren van de computer (de host development kit) waarop u wilt installeren development kit. Na deze stap wordt de development kit host naar de Cloudbuilder.vhdx opgestart.
3. [Implementeer de development kit](#deploy-the-development-kit) op de host van development kit.

> [!NOTE]
> Voor de beste resultaten, zelfs als u wilt gebruiken van een omgeving zonder verbinding Azure Stack verdient implementeren tijdens verbinding met internet. De evaluatieversie van Windows Server 2016 kan dan worden geactiveerd tijdens de implementatie.
> 
> 

## <a name="download-and-extract-the-development-kit"></a>Downloaden en uitpakken van de development kit
1. Voordat u het downloaden, zorg dat de computer voldoet aan de volgende vereisten:

   * De computer moet ten minste 60 GB aan vrije schijfruimte hebben.
   * [.NET framework 4.6 (of een latere versie)](https://aka.ms/r6mkiy) moet worden geïnstalleerd.

2. [Ga naar de pagina aan de slag](https://azure.microsoft.com/overview/azure-stack/try/?v=try), geef de details op en klikt u op **indienen**.
3. Onder **Download de software**, klikt u op **Azure Stack Development Kit**.
4. Voer het gedownloade AzureStackDownloader.exe-bestand.
5. In de **Azure Stack Development Kit Downloader** venster, volg de stappen 1 t/m 5.
6. Nadat het downloaden is voltooid, klikt u op **uitvoeren** de MicrosoftAzureStackPOC.exe starten.
7. Lees de gebruiksrechtovereenkomst scherm en informatie van de Wizard zelfstandig uitpakken en klik op **volgende**.
8. Raadpleeg de privacyverklaring scherm en informatie van de Wizard zelfstandig uitpakken en klik vervolgens op **volgende**.
9. Selecteer de bestemming voor de bestanden worden uitgepakt, klikt u op **volgende**.
   * De standaardwaarde is: <drive letter>:\<huidige map > \Microsoft Azure Stack
10. Raadpleeg de bestemming locatie scherm en informatie van de Wizard zelfstandig uitpakken en klik vervolgens op **extraheren** de CloudBuilder.vhdx (~ 25 GB) en ThirdPartyLicenses.rtf bestanden uitpakken. Dit kan enige tijd duren.

> [!NOTE]
> Nadat u de bestanden hebt uitgepakt, kunt u de exe- en bin bestanden voor de ruimte op de computer verwijderen. Of u deze bestanden naar een andere locatie zo dat als u wilt implementeren u hoeft niet opnieuw te downloaden de bestanden kunt verplaatsen.
> 
> 

## <a name="prepare-the-development-kit-host"></a>De development kit host voorbereiden
1. Zorg ervoor dat u kunt fysiek verbinding met de development kit host maken of fysieke console-toegang (zoals KVM) hebben. Nadat u de host development kit in stap 13 hieronder opnieuw opstarten, moet u deze toegang hebben.
2. Controleer of de development kit host voldoet aan de [minimumvereisten](azure-stack-deploy.md). U kunt de [implementatie Checker voor Azure-Stack](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) om te bevestigen dat uw vereisten.
3. Meld u aan als lokale beheerder op de host van uw development kit.
4. Kopieer of verplaats het bestand CloudBuilder.vhdx naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx).
5. Voer het volgende script voor het downloaden van het installatiebestand van development kit (asdk installer.ps1) naar de map c:\AzureStack_Installer op uw development kit host.
    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
    $LocalPath = 'c:\AzureStack_Installer'

    # Create folder
    New-Item $LocalPath -Type directory

    # Download file
    Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
    ```
6. Open een PowerShell-console met verhoogde bevoegdheid > Voer het script C:\AzureStack_Installer\asdk-installer.ps1 > klikt u op **omgeving voorbereiden**.
7. Op de **Selecteer Cloudbuilder vhdx** pagina van het installatieprogramma, blader naar en selecteer het cloudbuilder.vhdx-bestand dat u in de vorige stappen hebt gedownload.
8. Optioneel: Controleer de **toevoegen van stuurprogramma's** op te geven van een map met de extra stuurprogramma's die u wilt dat op de host.
9. Op de **optionele instellingen** pagina, geeft u het lokale administrator-account voor de host van development kit. Als u deze referenties niet opgeeft, moet u KVM-toegang tot de host tijdens het installatieproces hieronder.
10. Op de **optionele instellingen** pagina, hebt u de optie voor het volgende instellen:
    - **Computernaam**: deze optie stelt u de naam van de host van development kit. De naam moet voldoen aan de vereisten van de FQDN-naam en moet zijn dan 15 tekens of minder. De standaardwaarde is de naam van een willekeurige computer door Windows wordt gegenereerd.
    - **Tijdzone**: Hiermee stelt u de tijdzone voor de host van development kit. De standaardwaarde is (UTC-8:00) Pacific Time (VS en Canada).
    - **Statische IP-configuratie**: Hiermee stelt u uw implementatie een statische IP-adres gebruiken. Wanneer het installatieprogramma opnieuw wordt opgestart in de cloudbuilder.vhx, worden de netwerkinterfaces anders geconfigureerd met DHCP.
11. Klik op **Volgende**.
12. Als u een statisch IP-configuratie in de vorige stap hebt gekozen, moet u nu het volgende doen:
    - Selecteer een netwerkadapter. Zorg ervoor dat u kunt verbinding maken met de netwerkadapter voordat u op **volgende**.
    - Zorg ervoor dat de **IP-adres**, **Gateway**, en **DNS** waarden juist zijn en klik vervolgens op **volgende**.
13. Klik op **volgende** starten van het voorbereidingsproces.
14. Wanneer de voorbereiding blijkt **voltooid**, klikt u op **volgende**.
15. Klik op **nu opnieuw opstarten** opnieuw opgestart in de cloudbuilder.vhdx en doorgaan met het implementatieproces.

## <a name="deploy-the-development-kit"></a>De development kit implementeren
1. Meld u aan als lokale beheerder op de host van development kit. Gebruik de referenties die zijn opgegeven in de vorige stappen.

    > [!IMPORTANT]
    > Voor implementaties van Azure Active Directory vereist Azure Stack toegang tot Internet, rechtstreeks of via een transparentproxy. De implementatie ondersteunt slechts één NIC voor netwerken. Als er meerdere NIC's, zorg ervoor dat slechts één is ingeschakeld (en alle andere protocollen zijn uitgeschakeld) voordat het script voor implementatie in de volgende sectie wordt uitgevoerd.
    
2. Open een PowerShell-console met verhoogde bevoegdheid > Voer het script \AzureStack_Installer\asdk-installer.ps1 (dit is mogelijk op een ander station in de Cloudbuilder.vhdx) > klikt u op **installeren**.
3. In de **Type** de optie **Azure-Cloud** of **ADFS**.
    - **Azure-Cloud**: Azure Active Directory is de id-provider. Gebruik deze parameter om op te geven van een specifieke map waar de AAD-account globale beheerdersmachtigingen heeft. Volledige naam van een AAD-Directory-tenant. Bijvoorbeeld,. onmicrosoft.com. 
    - **ADFS**: de standaardstempel Directory Service is de id-provider en is het standaardaccount voor aanmelden azurestackadmin@azurestack.local, en het wachtwoord te gebruiken die u hebt opgegeven als onderdeel van de installatie is.
4. Onder **lokale administrator-wachtwoord**, in de **wachtwoord** vak, typt u het lokale administrator-wachtwoord (die overeenkomen met de huidige geconfigureerde lokale administrator-wachtwoord) en klik vervolgens op **Volgende**.
5. Selecteer een netwerkadapter wilt gebruiken voor de development kit en klik vervolgens op **volgende**.
6. Selecteer DHCP of statische netwerkconfiguratie voor de BGPNAT01 virtuele machine.
    - **DHCP** (standaard): de virtuele machine krijgt de IP-configuratie van de DHCP-server.
    - **Statische**: Gebruik deze optie alleen als DHCP niet een geldig IP-adres voor Azure-Stack toegang tot Internet toewijzen. Een statisch IP-adres moet worden opgegeven met de lengte van het subnetmasker (bijvoorbeeld 10.0.0.5/24).
7. Stel desgewenst de volgende waarden:
    - **VLAN-ID**: Hiermee stelt u de VLAN-ID. Gebruik deze optie alleen als de host en AzS BGPNAT01 VLAN-ID voor toegang tot het fysieke netwerk (en Internet) moet configureren. 
    - **DNS-doorstuurserver**: een DNS-server is gemaakt als onderdeel van de Azure-Stack-implementatie. Geef uw bestaande infrastructuur DNS-server zodat computers binnen de oplossing voor het omzetten van namen buiten de stempel. De in het stempel DNS-server stuurt onbekende aanvragen voor naamomzetting naar deze server.
    - **Tijdserver**: dit veld wordt de tijdserver ingesteld en moet een IP-adres vereist. Ga voor een IP-adres naar [pool.ntp.org](http:\\pool.ntp.org) of time.windows.com pingen. 
8. Klik op **Volgende**. 
9. Op de **network interface card eigenschappen controleren** pagina ziet u een voortgangsbalk weergegeven. 
    - Als er op het **een update kan niet worden gedownload**, volg de instructies op de pagina.
    - Wanneer deze zegt **voltooid**, klikt u op **volgende**.
10. Op **samenvatting** pagina, klikt u op **implementeren**.
11. Als u de implementatie van een Azure Active Directory, wordt u gevraagd de referenties van uw Azure Active Directory-hoofdbeheerder-account invoeren.
12. De implementatie kan enkele uren, waarin het systeem automatisch opnieuw eenmaal opgestart duren.
   
   > [!IMPORTANT]
   > Als u de voortgang van de implementatie wilt, moet u zich aanmelden als azurestack\AzureStackAdmin. Als u zich aanmeldt als lokale beheerder nadat de computer is toegevoegd aan het domein, kunt u de voortgang van de implementatie niet zien. Implementatie opnieuw uitgevoerd, in plaats daarvan Meld u aan als azurestack\AzureStackAdmin om te valideren dat deze wordt uitgevoerd niet.
   > 
   > 
   
    Als de implementatie is geslaagd, de PowerShell-console wordt weergegeven: **voltooid: actie 'Implementatie'**.
   
Als de implementatie mislukt, kunt u het volgende opnieuw PowerShell-script van de dezelfde PowerShell-venster met verhoogde bevoegdheid:

```powershell
cd c:\CloudDeployment\Setup
.\InstallAzureStackPOC.ps1 -Rerun
```

Dit script wordt opnieuw opgestart voor de implementatie van de laatste stap die zijn voltooid.

U kunt [implementeren](azure-stack-redeploy.md) vanaf het begin.


## <a name="reset-the-password-expiration-to-180-days"></a>Opnieuw instellen van het wachtwoord verloopt 180 dagen

Volgende stappen om ervoor te zorgen dat het wachtwoord voor de development kit host te snel niet verloopt, nadat u hebt geïmplementeerd:

Het vervalbeleid voor wachtwoord wijzigen vanuit Powershell:
1. Voer de opdracht; uit het Powershell-venster Set-ADDefaultDomainPasswordPolicy - MaxPasswordAge 180.00:00:00-identiteit azurestack.local

Het vervalbeleid voor wachtwoorden handmatig wijzigen:
1. Open op de host van de kit ontwikkeling **Group Policy Management** en navigeer naar **Group Policy Management** – **Forest: azurestack.local** – **domeinen** – **azurestack.local**.
2. Klik met de rechtermuisknop **standaarddomeinbeleid** en klik op **bewerken**.
3. In de Editor voor Groepsbeleidsbeheer, gaat u naar **Computerconfiguratie** – **beleid** – **Windows-instellingen** – **beveiligingsinstellingen**– **Beleidsregels van account** – **wachtwoordbeleid**.
4. Dubbelklik in het rechterdeelvenster op **Maximum wachtwoord leeftijd**.
5. In de **Maximum wachtwoord leeftijd eigenschappen** wijziging van het dialoogvenster de **wachtwoord verloopt over** van waarde naar 180 en klik vervolgens op **OK**.


## <a name="next-steps"></a>Volgende stappen

[PowerShell installeren](azure-stack-powershell-configure-quickstart.md)

[Azure-Stack registreren bij uw Azure-abonnement](azure-stack-register.md)

[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)

