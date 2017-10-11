---
title: Schakel Microsoft Windows Hello voor bedrijven in uw organisatie | Microsoft Docs
description: Implementatie-instructies voor het Microsoft Passport inschakelen in uw organisatie.
services: active-directory
documentationcenter: 
keywords: Microsoft Passport configureren, Microsoft Windows Hello voor bedrijven-implementatie
author: MarkusVi
manager: femila
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: 58943e1e29755c983e55c675dd4fe7b75ac47b34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Schakel Microsoft Windows Hello voor bedrijven in uw organisatie
Na [domein Windows 10-apparaten verbinding te maken met Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), doet u het volgende om in te schakelen Microsoft Windows Hello voor bedrijven in uw organisatie:

1. System Center Configuration Manager implementeren  
2. Configureren van beleidsinstellingen
3. Het certificaatprofiel configureren  

## <a name="deploy-system-center-configuration-manager"></a>System Center Configuration Manager implementeren
Als u wilt implementeren op basis van Windows Hello voor bedrijven sleutels gebruikerscertificaten, moet u het volgende:

* **System Center Configuration Manager Current Branch** -u moet versie 1606 of hoger installeren. Zie voor meer informatie de [documentatie voor System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) en [System Center Configuration Manager-teamblog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
* **Openbare-sleutelinfrastructuur (PKI)** : als u wilt inschakelen Microsoft Windows Hello voor bedrijven met behulp van gebruikerscertificaten, u moet beschikken over een PKI. Als u niet hebt, of u niet wilt gebruiken voor gebruikerscertificaten, kunt u een nieuwe domeincontroller met Windows Server 2016 build 10551 (of hoger) geïnstalleerd kunt implementeren. Volg de stappen voor [een replica-domeincontroller installeren in een bestaand domein](https://technet.microsoft.com/library/jj574134.aspx) of [een nieuw Active Directory-forest installeren als u een nieuwe omgeving](https://technet.microsoft.com/library/jj574166). (De ISO's zijn beschikbaar voor downloaden op [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)

## <a name="configure-policy-settings"></a>Configureren van beleidsinstellingen
Voor het configureren van de Microsoft Windows Hello voor bedrijven-beleidsinstellingen, hebt u twee opties:

* Groepsbeleid in Active Directory 
* De System Center Configuration Manager 

Via Groepsbeleid in Active Directory is de aanbevolen methode voor het configureren van Microsoft Windows Hello voor bedrijven-instellingen voor beleid. 

Het gebruik van System Center Configuration Manager is de aanbevolen methode wanneer u ook gebruiken om certificaten te implementeren. Dit scenario:

* Zorgt voor compatibiliteit met de nieuwere implementatiescenario 's
* Aan de clientzijde Windows 10 versie 1607 of hoger vereist.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Configureer Microsoft Windows Hello voor bedrijven via Groepsbeleid in Active Directory
**Stappen**:

1. Open Serverbeheer en navigeer naar **extra** > **Group Policy Management**.
2. Ga naar het knooppunt dat overeenkomt met het domein waarin u wilt inschakelen met het Azure AD Join van Group Policy Management.
3. Met de rechtermuisknop op **Group Policy Objects**, en selecteer **nieuw**. Geef uw Group Policy Object een naam, bijvoorbeeld inschakelen Windows Hello voor bedrijven. Klik op **OK**.
4. Met de rechtermuisknop op uw nieuwe groepsbeleidsobject en selecteer vervolgens **bewerken**.
5. Navigeer naar **Computerconfiguratie** > **beleid** > **Beheersjablonen** > **Windows-onderdelen** > **Windows Hello voor bedrijven**.
6. Met de rechtermuisknop op **inschakelen Windows Hello voor bedrijven**, en selecteer vervolgens **bewerken**.
7. Selecteer de **ingeschakeld** keuzerondje en klik vervolgens op **toepassen**. Klik op **OK**.
8. U kunt nu het groepsbeleidsobject koppelen naar een locatie van uw keuze. Koppel het groepsbeleid aan het domein zodat dit beleid voor alle domein Windows 10-apparaten in uw organisatie. Bijvoorbeeld:
   * Een specifieke organisatie-eenheid (OE) in Active Directory waarop Windows 10-computers domein geplaatst worden
   * Een specifieke beveiligingsgroep met Windows 10 domeincomputers die automatisch wordt geregistreerd met Azure AD

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>Windows Hello voor bedrijven met System Center Configuration Manager configureren
**Stappen**:

1. Open de **System Center Configuration Manager**, en navigeer vervolgens naar **activa en naleving > instellingen voor naleving > toegang tot bedrijfsbronnen > Windows Hello voor bedrijven-profielen**.
   
    ![Windows Hello voor bedrijven configureren](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. Klik in de werkbalk bovenaan op **maken Windows Hello voor bedrijven-profiel**.
   
    ![Windows Hello voor bedrijven configureren](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. Op de **algemene** dialoogvenster de volgende stappen uitvoeren:
   
    ![Windows Hello voor bedrijven configureren](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    a. In de **naam** textbox, typ een naam voor uw profiel bijvoorbeeld **mijn WHfB profiel**.
   
    b. Klik op **Volgende**.
4. Op de **ondersteunde Platforms** dialoogvenster de platforms selecteren die worden ingericht met dit Windows Hello voor bedrijven-profiel en klik vervolgens op **volgende**.
   
    ![Windows Hello voor bedrijven configureren](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. Op de **instellingen** dialoogvenster de volgende stappen uitvoeren:
   
    ![Windows Hello voor bedrijven configureren](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    a. Als **configureren Windows Hello voor bedrijven**, selecteer **ingeschakeld**.
   
    b. Als **Trusted Platform Module (TPM) gebruiken**, selecteer **vereist**. 
   
    c. Als **verificatiemethode**, selecteer **op basis van certificaten**.
   
    d. Klik op **Volgende**.
6. Op de **samenvatting** dialoogvenster, klikt u op **volgende**.
7. Op de **voltooiing** dialoogvenster, klikt u op **sluiten**.
8. Klik in de werkbalk bovenaan op **implementeren**.
   
    ![Windows Hello voor bedrijven configureren](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-the-certificate-profile"></a>Het certificaatprofiel configureren
Als u verificatie op basis van certificaten voor lokale verificatie gebruikt, moet u configureren en implementeren van een certificaatprofiel. Deze taak moet u voor het instellen van een NDES-server en de siterol van het Certificaatregistratiepunt in System Center Configuration Manager. Zie voor meer informatie de [vereisten voor Certificaatprofielen in Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).

1. Open de **System Center Configuration Manager**, en navigeer vervolgens naar **activa en naleving > instellingen voor naleving > toegang tot bedrijfsbronnen > Certificaatprofielen**.
2. Selecteer een sjabloon met een smartcard aanmelden uitgebreide-sleutelgebruik (EKU).

Op de **SCEP-registratie** pagina van het certificaatprofiel moet u ervoor kiezen **Stalleren in Passport for Work, anders niet** als de **Key Storage Provider**.

## <a name="next-steps"></a>Volgende stappen
* [Windows 10 for the enterprise: Ways to use devices for work](active-directory-azureadjoin-windows10-devices-overview.md) (Windows 10 voor de onderneming: manieren om apparaten voor werk te gebruiken)
* [Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md) (Cloudmogelijkheden uitbreiden naar Windows 10-apparaten met behulp van Azure Active Directory Join)
* [Identiteiten zonder wachtwoorden via Microsoft Passport verifiëren](active-directory-azureadjoin-passport.md)
* [Learn about usage scenarios for Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md) (Gebruiksscenario’s voor Azure AD Join)
* [Connect domain-joined devices to Azure AD for Windows 10 experiences](active-directory-azureadjoin-devices-group-policy.md) (Apparaten die lid zijn van een domein verbinden met Azure AD voor Windows 10-ervaringen)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)

