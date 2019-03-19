---
title: Installeer de Azure Stack Development Kit (ASDK) | Microsoft Docs
description: Beschrijft het installeren van de Azure Stack Development Kit (ASDK).
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
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: 509dafb02d456e3f612e1de8a5bc1cf4b010a05a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58113853"
---
# <a name="install-the-azure-stack-development-kit-asdk"></a>De Azure Stack Development Kit (ASDK) installeren
Na [voorbereiden van de hostcomputer ASDK](asdk-prepare-host.md), de ASDK kan worden geïmplementeerd in de installatiekopie van het CloudBuilder.vhdx met behulp van de volgende stappen in dit artikel.

## <a name="install-the-asdk"></a>De ASDK installeren
De stappen in dit artikel ziet u over het implementeren van de ASDK met behulp van een grafische gebruikersinterface (GUI) dat is opgegeven door te downloaden en uitvoeren van de **asdk installer.ps1** PowerShell-script.

> [!NOTE]
> De gebruikersinterface van het installatieprogramma voor de Azure Stack Development Kit is een open-source-script op basis van WCF en PowerShell.


1. Nadat de computer is naar de installatiekopie van het CloudBuilder.vhdx opgestart, log in met behulp van de referenties van de opgegeven wanneer u [voorbereid de hostcomputer development kit](asdk-prepare-host.md) voor ASDK installatie. Dit moet hetzelfde zijn als de development kit host lokale administrator-referenties.
2. Open een PowerShell-console met verhoogde bevoegdheid en voer de  **&lt;stationsletter > \AzureStack_Installer\asdk-installer.ps1** PowerShell-script. Houd er rekening mee dat het script nu op een ander station dan C:\ in de afbeelding CloudBuilder.vhdx kan zijn. Klik op **Install**.

    ![](media/asdk-install/1.PNG) 

3. In de id-Provider **Type** vervolgkeuzelijst, selecteer **Azure China-Cloud**, **Azure US Government-Cloud**, **AD FS**, of **Azure-Cloud**. Onder **lokale Administrator-wachtwoord** typt u het lokale administrator-wachtwoord (dit moet overeenkomen met de huidige geconfigureerde lokale administrator-wachtwoord) in de **wachtwoord** vak en klik vervolgens op  **Volgende**.

    ![](media/asdk-install/2.PNG) 
  
   Als u ervoor een Azure-abonnement-id-provider kiest, moet u een internetverbinding, de volledige naam van een Azure AD directory-tenant in de vorm van *domainname*. onmicrosoft.com of een Azure AD gecontroleerd aangepast domein naam en een globale Administrator-referenties voor de opgegeven map.<br><br>Na de implementatie is Azure Active Directory-globale beheerder toestemming niet vereist. Bepaalde bewerkingen is echter mogelijk de referentie van de globale beheerder. Bijvoorbeeld, een resource provider installatieprogramma script of een nieuwe functie die een machtiging worden verleend. U kunt tijdelijk opnieuw installeren van de account hoofdbeheerder machtigingen of gebruik een afzonderlijke globale beheerder-account dat is eigenaar van de *providerabonnement standaard*.<br><br>Als u AD FS als de id-provider, wordt de standaard stempel directory-service gebruikt. Het standaardaccount voor het aanmelden is azurestackadmin@azurestack.local, en het wachtwoord te gebruiken is die u als onderdeel van de installatie is opgegeven.

   > [!NOTE]
   > Voor optimale resultaten, zelfs als u wilt gebruiken van een niet-verbonden Azure Stack-omgeving met behulp van AD FS als id-provider, is het raadzaam om te installeren de ASDK terwijl u verbinding hebt met internet. Op die manier kunnen kan de evaluatieversie van Windows Server 2016 opgenomen in de development kit-installatie worden geactiveerd tijdens de implementatie.

4. Selecteer een netwerkadapter wilt gebruiken voor de development kit en klik vervolgens op **volgende**.

    ![](media/asdk-install/3.PNG)

5. Op de **netwerkconfiguratie** pagina, Geef een geldige **IP-adres van tijd** adres. Dit vereiste veld Hiermee stelt u de time-server moet worden gebruikt door de development kit. Deze parameter moet worden opgegeven als het IP-adres van een geldige tijd-server. De namen worden niet ondersteund.

      > [!TIP]
      > Als u een tijdserver IP-adres zoekt, gaat u naar [ntppool.org](https://www.ntppool.org/) of time.windows.com pingen. 

    **U kunt desgewenst**, kunt u bieden een **DNS-doorstuurserver** IP-adres. Een DNS-server gemaakt als onderdeel van de Azure Stack-implementatie. Geef uw bestaande infrastructuur DNS-server zodat computers in de oplossing voor het omzetten van namen buiten de stempel. De DNS-server in het stempel onbekende naam resolutie stuurt aanvragen door naar deze server.

    ![](media/asdk-install/4.PNG)

6. Op de **network interface card eigenschappen verifiëren** pagina, ziet u een voortgangsbalk weergegeven. Als verificatie voltooid is, klikt u op **volgende**.

    ![](media/asdk-install/5.PNG)

7. Op **samenvatting** pagina, klikt u op **implementeren** ASDK om installatie te starten op de hostcomputer development kit.

    ![](media/asdk-install/6.PNG)

    > [!TIP]
    > U kunt hier ook de PowerShell-opdrachten voor instellingen die worden gebruikt voor het installeren van de development kit kopiëren. Dit is handig als u ooit moet [opnieuw implementeren van de ASDK op de host met behulp van PowerShell](asdk-deploy-powershell.md).

8. Als u een Azure AD-implementatie uitvoert, wordt u gevraagd om in te voeren van de accountreferenties van uw Azure AD-hoofdbeheerder een paar minuten nadat setup is gestart.

9. Het implementatieproces duurt een paar uur, gedurende welke de hostcomputer automatisch opnieuw eenmaal opgestart wordt. Als u controleren van de voortgang van de implementatie wilt, meld u als azurestack\AzureStackAdmin nadat de development kit host opnieuw is opgestart. Als de implementatie is geslaagd, wordt de PowerShell-console wordt weergegeven: **VOLTOOID: Actie 'Implementatie'**. 
    > [!IMPORTANT]
    > Als u zich aanmeldt als een lokale beheerder nadat de computer is toegevoegd aan het domein azurestack, kunt u de voortgang van de implementatie niet weergegeven. Implementatie opnieuw uitvoeren, meld u in plaats daarvan als azurestack\AzureStackAdmin aan om te valideren dat deze wordt uitgevoerd niet.

    ![](media/asdk-install/7.PNG)

Gefeliciteerd, u hebt de ASDK is geïnstalleerd.

Als de implementatie voor een of andere reden mislukt, kunt u [opnieuw implementeren](asdk-redeploy.md) maakt of gebruik de volgende PowerShell-opdrachten, vanuit de dezelfde verhoogde PowerShell-venster, opnieuw opstarten van de implementatie van de laatste geslaagde stap:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

## <a name="next-steps"></a>Volgende stappen
[Post-implementatieconfiguratie](asdk-post-deploy.md)
