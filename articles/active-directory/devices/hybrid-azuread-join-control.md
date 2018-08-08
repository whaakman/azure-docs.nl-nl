---
title: Hoe het configureren van hybride Azure Active Directory apparaten gekoppelde | Microsoft Docs
description: Informatie over het configureren van hybride Azure Active Directory verbonden apparaten.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 9ffc84009adfca60e9ae6b188b65b15e874e7d9c
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622167"
---
# <a name="how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>De hybride Azure AD join van uw apparaten beheren

Hybride Azure AD join is een proces naar uw on-premises domein apparaten automatisch wordt geregistreerd bij Azure AD. Er zijn gevallen waarin u niet wilt dat al uw apparaten automatisch te registreren. Dit is voor voorbeeld true, wordt tijdens de initiële implementatie om te controleren of alles werkt zoals verwacht.

In dit artikel biedt richtlijnen voor hoe u hybride Azure AD join van uw apparaten kunt beheren. 


## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent:

-  [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md)
 
-  [De implementatie van uw hybride Azure Active Directory-deelname plannen](hybrid-azuread-join-plan.md)

-  Configureren van hybride Azure Active Directory join voor [beheerde domeinen](hybrid-azuread-join-managed-domains.md) of [federatieve domeinen](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Huidige Windows-apparaten beheren

Voor apparaten met het besturingssysteem voor Windows-bureaublad, de ondersteunde versie is de Windows 10 Jubileumupdate (versie 1607) of hoger. Als een best practice, een upgrade uitvoeren naar de nieuwste versie van Windows 10.

Alle windows huidige apparaten automatisch wordt geregistreerd bij Azure AD op het apparaat start of de gebruiker zich aanmelden. U kunt dit gedrag met een groepsbeleidsobject (GPO) of System Center Configuration Manager beheren.

Voor het beheren van huidige Windows-apparaten, moet u naar: 

1.  **Op alle apparaten**: uitschakelen van automatische device Registration service.
2.  **Aan geselecteerde apparaten**: automatische apparaatregistratie inschakelen.

Als u hebt gecontroleerd dat alles werkt zoals verwacht, bent u klaar om in te schakelen automatische apparaatregistratie voor alle apparaten opnieuw uit.



## <a name="group-policy-object"></a>Group Policy Object 

U kunt het gedrag van de registratie van apparaten van uw apparaten beheren met het implementeren van de volgende GPO: **Domeincomputers als apparaten registreren**

**Het instellen van het groepsbeleidsobject**:

1.  Open **Serverbeheer**, en ga vervolgens naar **extra \> Group Policy Management**.

2.  Ga naar het knooppunt van het domein dat overeenkomt met het domein waar u de automatische registratie inschakelen/uitschakelen.

3.  Met de rechtermuisknop op **Group Policy Objects**, en selecteer vervolgens **nieuw**.

4.  Typ een naam (bijvoorbeeld *Hybrid Azure AD join*) voor de Groepsbeleid-object. 

5.  Klik op **OK**.

6.  Met de rechtermuisknop op uw nieuwe groepsbeleidsobject en selecteer vervolgens **bewerken**.

7.  Ga naar **Computerconfiguratie \> beleid \> Beheersjablonen \> Windows-onderdelen \> Device Registration service**. 

8.  Met de rechtermuisknop op **Domeincomputers registreren als apparaten**, en selecteer vervolgens **bewerken**.

    > [!NOTE] 
    > Deze groepsbeleidssjabloon is gewijzigd van eerdere versies van de console Groepsbeleidsbeheer. Als u een eerdere versie van de-console gebruikt, gaat u naar **Computerconfiguratie \> beleid \> Beheersjablonen \> Windows-onderdelen \> Workplace Join \> Automatisch workplace join-clientcomputers**. 

9.  Selecteer een van de volgende instellingen en klik vervolgens op **toepassen**:

    - **Uitgeschakelde** : als u wilt voorkomen dat de automatische apparaatregistratie
    - **Ingeschakeld** : de automatische apparaatregistratie inschakelen

10. Klik op **OK**.

U moet het groepsbeleidsobject koppelen aan een andere locatie van uw keuze. Bijvoorbeeld, u dit beleid voor alle domein huidige apparaten instellen in uw organisatie, het groepsbeleidsobject koppelen aan het domein. Als u wilt een gecontroleerde implementatie doen, moet u dit beleid instellen op domein Windows huidige apparaten die deel uitmaken van een organisatie-eenheid of een beveiligingsgroep.

### <a name="configuration-manager-controlled-deployment"></a>Implementatie van Configuration Manager beheerd 

U kunt het gedrag van de registratie van apparaten van uw huidige apparaten beheren door het configureren van de volgende clientinstelling: ** nieuwe Windows 10 domein apparaten automatisch wordt geregistreerd bij azure Active Directory **


**Om in te stellen van de client-instelling**:

1.  Open **Configuration Manager**, en ga vervolgens naar **Cloudservices**.

2.  Onder **apparaatinstellingen**, selecteer een van de volgende instellingen voor **nieuwe Windows 10 domein apparaten automatisch wordt geregistreerd bij azure Active Directory**:

    - **Geen** : als u wilt voorkomen dat de automatische apparaatregistratie
    - **Ja** : de automatische apparaatregistratie inschakelen


3.  Klik op **OK**.
    

U moet deze clientinstelling instelt op een locatie van uw keuze te koppelen. Bijvoorbeeld, voor het configureren van deze client-instelling voor alle huidige Windows-apparaten in uw organisatie, koppelt u de clientinstelling voor het domein. Als u wilt een gecontroleerde implementatie doen, kunt u de clientinstelling voor Windows domein apparaten die deel uitmaken van een organisatie-eenheid of een beveiligingsgroep configureren.

> [!Important]
> Terwijl de bovenstaande configuratie van bestaande Windows 10-apparaten van een domein, is het mogelijk voor nieuw domein door apparaten om nog steeds te koppelen aan Azure AD complete hybride vanwege de mogelijke vertraging in de toepassing van Groepsbeleid of Configuration Manager-instellingen op het nieuwe domein Windows 10-apparaat. Om dit te voorkomen, is het aanbevolen dat u een nieuwe sysprep-afbeelding (gebruikt als voorbeeld voor een inrichtingsmethode) maken van een apparaat dat is nooit eerder hebt toegevoegd aan hybrid Azure AD en dat al het bovenstaande instelling voor Groepsbeleid toegepast of Configuration Manager-client instelling die is toegepast. U moet de nieuwe installatiekopie ook gebruiken voor het inrichten van nieuwe computers die lid worden van domein van uw organisatie. 

## <a name="control-windows-down-level-devices"></a>Windows downlevel-apparaten beheren

Voor het registreren van Windows downlevel-apparaten die u wilt downloaden en installeren van Windows Installer-pakket (.msi) van Download Center op de [Microsoft Workplace Join voor Windows 10-computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) pagina.

U kunt het pakket implementeren met behulp van het systeem in een software-distributie, zoals System Center Configuration Manager. Het pakket biedt ondersteuning voor de opties standaard op de achtergrond installeren met de stille parameter. [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager) Current Branch biedt extra voordelen van eerdere versies, zoals de mogelijkheid voor het bijhouden van voltooide registraties.

Het installatieprogramma maakt een geplande taak op het systeem dat wordt uitgevoerd in de context van de gebruiker. De taak wordt geactiveerd wanneer de gebruiker zich aanmeldt bij Windows. De taak op de achtergrond lid wordt van het apparaat met Azure AD met de referenties van de gebruiker na verificatie met Azure AD.


Voor het beheren van de device Registration service, moet u het Windows Installer-pakket implementeren alleen op een geselecteerde groep Windows downlevel-apparaten. Als u hebt gecontroleerd dat alles werkt zoals verwacht, bent u gereed voor implementatie van het pakket naar alle downlevel-apparaten.


## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md)



