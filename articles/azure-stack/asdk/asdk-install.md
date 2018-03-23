---
title: Installeer de Azure-Stack Development Kit (ASDK) | Microsoft Docs
description: Beschrijft het installeren van Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 7b8fe61731a9412c61152bc58e55deebb611d011
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="install-the-azure-stack-development-kit-asdk"></a>Installeer de Azure-Stack Development Kit (ASDK)
Na [voorbereiden van de hostcomputer ASDK](asdk-prepare-host.md), de ASDK kan worden geïmplementeerd in de installatiekopie van het CloudBuilder.vhdx met behulp van de volgende stappen in dit artikel.

## <a name="install-the-asdk"></a>De ASDK installeren
De stappen in dit artikel ziet u het implementeren van de ASDK met een grafische gebruikersinterface (GUI) dat is geleverd door downloaden en uitvoeren van de **asdk installer.ps1** PowerShell-script.

> [!NOTE]
> De gebruikersinterface van het installatieprogramma voor de Azure-Stack Development Kit is een open source-script op basis van WCF en PowerShell.


1. Nadat de computer is naar de installatiekopie van het CloudBuilder.vhdx opgestart, aanmelden met de administrator-referenties opgegeven wanneer u [development kit hostcomputer voorbereid](asdk-prepare-host.md) voor ASDK-installatie. Dit moet hetzelfde zijn als de development kit host lokale beheerdersreferenties.
2. Open een PowerShell-console met verhoogde bevoegdheid en voer de  **&lt;stationsletter > \AzureStack_Installer\asdk-installer.ps1** script (dit is mogelijk nu op een ander station dan C:\ in de afbeelding CloudBuilder.vhdx). Klik op **Install**.

    ![](media/asdk-install/1.PNG) 

3. In de id-Provider **Type** vervolgkeuzelijst, selecteer **Azure-Cloud** of **AD FS**. Onder **lokale Administrator-wachtwoord** typt u het lokale administrator-wachtwoord (die overeenkomen met de huidige geconfigureerde lokale administrator-wachtwoord) in de **wachtwoord** vak en klik vervolgens op  **Volgende**.
    - **Azure-Cloud**: Hiermee configureert u Azure Active Directory (Azure AD) als de id-provider. Om deze optie gebruikt, moet u een internetverbinding, de volledige naam van een Azure AD directory-tenant in de vorm van *domainname*. onmicrosoft.com of een Azure AD geverifieerd aangepaste domeinnaam en de referenties van de globale beheerder voor de opgegeven de map. 
    - **AD FS**: standaard stempel Active Directory wordt gebruikt als de id-provider. Het standaardaccount voor aanmelden is azurestackadmin@azurestack.local, en het wachtwoord te gebruiken die u hebt opgegeven als onderdeel van de installatie is.

    ![](media/asdk-install/2.PNG) 
    
    > [!NOTE]
    > Voor de beste resultaten, zelfs als u wilt gebruiken van een omgeving zonder verbinding Stack van Azure AD FS gebruikt als de id-provider wordt het aanbevolen voor het installeren van de ASDK tijdens verbinding met internet. De evaluatieversie van Windows Server 2016 opgenomen met de installatie van development kit kan dan worden geactiveerd tijdens de implementatie.
4. Selecteer een netwerkadapter wilt gebruiken voor de development kit en klik vervolgens op **volgende**.

    ![](media/asdk-install/3.PNG)

5. Selecteer DHCP of statische netwerkconfiguratie voor de BGPNAT01 virtuele machine.
    > [!TIP]
    > De VM BGPNAT01 is de edge-router die NAT- en VPN-mogelijkheden voor Azure-Stack biedt.

    - **DHCP** (standaard): de virtuele machine krijgt de IP-configuratie van de DHCP-server.
    - **Statische**: Gebruik deze optie alleen als DHCP niet een geldig IP-adres voor Azure-Stack toegang tot Internet toewijzen. **Een statisch IP-adres moet worden opgegeven met de lengte van het subnetmasker in CIDR-notatie (bijvoorbeeld 10.0.0.5/24)**.
    - Type in een geldige **server-IP-tijd** adres. Dit vereist veld stelt de time-server moet worden gebruikt door de development kit. Deze parameter moet worden opgegeven als een geldige tijd server IP-adres. Servernamen worden niet ondersteund.

      > [!TIP]
      > Ga voor een IP-adres naar [pool.ntp.org](http:\\pool.ntp.org) of time.windows.com pingen. 

    - **Eventueel**, instellen van de volgende waarden:
        - **VLAN-ID**: Hiermee stelt u de VLAN-ID. Gebruik deze optie alleen als de host en AzS BGPNAT01 VLAN-ID voor toegang tot het fysieke netwerk (en internet) moet configureren. 
        - **DNS-doorstuurserver**: een DNS-server is gemaakt als onderdeel van de Azure-Stack-implementatie. Geef uw bestaande infrastructuur DNS-server zodat computers binnen de oplossing voor het omzetten van namen buiten de stempel. De in het stempel DNS-server stuurt onbekende aanvragen voor naamomzetting naar deze server.

    ![](media/asdk-install/4.PNG)

6. Op de **network interface card eigenschappen controleren** pagina ziet u een voortgangsbalk weergegeven. Als verificatie voltooid is, klikt u op **volgende**.

    ![](media/asdk-install/5.PNG)

9. Op **samenvatting** pagina, klikt u op **implementeren** ASDK om installatie te starten op de hostcomputer van development kit.

    ![](media/asdk-install/6.PNG)

    > [!TIP]
    > U kunt hier ook de PowerShell-opdrachten voor installatie die wordt gebruikt voor het installeren van de development kit kopiëren. Dit is handig als u ooit moet [opnieuw implementeren van de ASDK op de host met behulp van PowerShell](asdk-deploy-powershell.md).

10. Als u een Azure AD-implementatie uitvoert, wordt u gevraagd om in te voeren van uw Azure AD-referenties voor het account van globale beheerder een paar minuten nadat setup is gestart.

    ![](media/asdk-install/7.PNG)

11. Het implementatieproces duurt een paar uur, gedurende welke de hostcomputer automatisch opnieuw eenmaal opgestart wordt. Als u de voortgang van de implementatie wilt, meld u als azurestack\AzureStackAdmin nadat de development kit host opnieuw is opgestart. Als de implementatie is geslaagd, de PowerShell-console wordt weergegeven: **voltooid: actie 'Implementatie'**. 
    > [!IMPORTANT]
    > Als u zich aanmeldt als lokale beheerder nadat de computer is toegevoegd aan het domein, kunt u de voortgang van de implementatie niet zien. Implementatie opnieuw uitgevoerd, in plaats daarvan Meld u aan als azurestack\AzureStackAdmin om te valideren dat deze wordt uitgevoerd niet.

    ![](media/asdk-install/8.PNG)

Gefeliciteerd, u hebt de ASDK geïnstalleerd.

Als de implementatie voor een bepaalde reden mislukt, kunt u [implementeren](asdk-redeploy.md) maakt of gebruik de volgende PowerShell-opdrachten, vanuit de dezelfde verhoogde PowerShell-venster, opnieuw opstarten van de implementatie van de laatste geslaagde stap:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

## <a name="next-steps"></a>Volgende stappen
[Post-implementatieconfiguratie](asdk-post-deploy.md)