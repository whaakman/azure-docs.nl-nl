---
title: De hybride Azure AD join van uw apparaten beheren | Microsoft Docs
description: Meer informatie over het beheren van de hybride Azure AD join van uw apparaten in Azure Active Directory.
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
ms.openlocfilehash: a2ae1d3f4166bfaa035902aaa5dc101636a98646
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54117534"
---
# <a name="control-the-hybrid-azure-ad-join-of-your-devices"></a>De hybride Azure AD-deelname van uw apparaten beheren

Lid worden van hybride Azure Active Directory (Azure AD) is een proces naar uw on-premises domein apparaten automatisch wordt geregistreerd bij Azure AD. Er zijn gevallen waarin u niet wilt dat al uw apparaten automatisch worden geregistreerd. Dit geldt, bijvoorbeeld tijdens de initiële implementatie om te controleren of alles werkt zoals verwacht.

In dit artikel bevat richtlijnen voor hoe u hybride Azure AD join van uw apparaten kunt beheren. 


## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent:

-  [Inleiding tot apparaatbeheer in Azure Active Directory](../device-management-introduction.md)
 
-  [Uw hybride Azure Active Directory join-implementatie plannen](hybrid-azuread-join-plan.md)

-  [Configure hybride Azure Active Directory join voor beheerde domeinen](hybrid-azuread-join-managed-domains.md) of [configureren hybride Azure Active Directory join voor federatieve domeinen](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Huidige Windows-apparaten beheren

Voor apparaten met het besturingssysteem voor Windows-bureaublad, de ondersteunde versie is de Windows 10 Jubileumupdate (versie 1607) of hoger. Als een best practice, een upgrade uitvoeren naar de nieuwste versie van Windows 10.

Alle Windows huidige apparaten automatisch wordt geregistreerd bij Azure AD op het apparaat start of de gebruiker zich aanmelden. U kunt dit gedrag beheren met behulp van een groepsbeleidsobject (GPO) of de System Center Configuration Manager.

Voor het beheren van huidige Windows-apparaten, moet u naar: 


1.  **Op alle apparaten**: Uitschakelen van automatische device Registration service.
2.  **Aan geselecteerde apparaten**: Automatische apparaatregistratie inschakelen.

Nadat u hebt gecontroleerd dat alles werkt zoals verwacht, u kunt automatische apparaatregistratie voor alle apparaten opnieuw inschakelen.



### <a name="group-policy-object"></a>Group Policy Object 

U kunt het gedrag van de registratie van apparaten van uw apparaten beheren met de volgende GPO implementeren: **Aan domein gekoppelde computers registreren als apparaten**.

Om in te stellen het groepsbeleidsobject:

1.  Open **Serverbeheer**, en ga vervolgens naar **extra** > **Group Policy Management**.

2.  Ga naar het knooppunt van het domein dat overeenkomt met het domein waar u de automatische registratie in-of uitschakelen.

3.  Met de rechtermuisknop op **Group Policy Objects**, en selecteer vervolgens **nieuw**.

4.  Voer een naam (bijvoorbeeld **Hybrid Azure AD join**) voor de Group Policy Object. 

5.  Selecteer **OK**.

6.  Met de rechtermuisknop op uw nieuwe groepsbeleidsobject en selecteer vervolgens **bewerken**.

7.  Ga naar **Computerconfiguratie** > **beleid** > **Beheersjablonen** > **Windows Onderdelen** > **Device Registration service**. 

8.  Met de rechtermuisknop op **Domeincomputers registreren als apparaten**, en selecteer vervolgens **bewerken**.

    > [!NOTE] 
    > Deze sjabloon Groepsbeleid is gewijzigd van eerdere versies van de console Groepsbeleidsbeheer. Als u een eerdere versie van de-console gebruikt, gaat u naar **Computerconfiguratie** > **beleid** > **Beheersjablonen**  >  **Windows-onderdelen** > **Workplace Join** > **automatisch workplace join-clientcomputers**. 

9.  Selecteer een van de volgende instellingen en selecteer vervolgens **toepassen**:

    - **Uitgeschakelde**: Om te voorkomen dat automatische apparaatregistratie.
    - **Ingeschakeld**: Automatische apparaatregistratie inschakelen.

10. Selecteer **OK**.

U moet het groepsbeleidsobject koppelen aan een andere locatie van uw keuze. Bijvoorbeeld, u dit beleid voor alle domein huidige apparaten instellen in uw organisatie, het groepsbeleidsobject koppelen aan het domein. Als u wilt een gecontroleerde implementatie doen, moet u dit beleid instellen op domein Windows huidige apparaten die deel uitmaken van een organisatie-eenheid of een beveiligingsgroep.

### <a name="configuration-manager-controlled-deployment"></a>Implementatie van Configuration Manager beheerd 

U kunt het gedrag van de registratie van apparaten van uw huidige apparaten beheren door het configureren van de volgende clientinstelling: **Nieuwe Windows 10 domein apparaten automatisch wordt geregistreerd bij Azure Active Directory**.

De client-instelling configureren:

1.  Open **Configuration Manager**, en ga vervolgens naar **Cloudservices**.

2.  Onder **apparaatinstellingen**, selecteer een van de volgende instellingen voor **nieuwe Windows 10 domein apparaten automatisch wordt geregistreerd bij Azure Active Directory**:

    - **Geen**: Om te voorkomen dat automatische apparaatregistratie.
    - **Ja**: Automatische apparaatregistratie inschakelen.


3.  Selecteer **OK**.
    

U moet deze clientinstelling instelt op een locatie van uw keuze te koppelen. Bijvoorbeeld, voor het configureren van deze client-instelling voor alle huidige Windows-apparaten in uw organisatie, koppelt u de clientinstelling voor het domein. Als u wilt een gecontroleerde implementatie doen, kunt u de clientinstelling voor Windows domein apparaten die deel uitmaken van een organisatie-eenheid of een beveiligingsgroep configureren.

> [!Important]
> Hoewel de configuratie van de voorgaande zorgt dat bestaande domein Windows 10-apparaten, mogelijk nog steeds probeert uit te voeren van de hybride Azure AD join vanwege de mogelijke vertraging in de toepassing van Groepsbeleid apparaten die nieuw lid van het domein worden zijn of Configuration Manager-instellingen op de apparaten. 
>
> Om dit te voorkomen, wordt u aangeraden dat u een nieuwe Sysprep-afbeelding maakt (als voorbeeld voor een inrichtingsmethode gebruikt). Het maken van een apparaat dat nooit eerder hebt toegevoegd aan hybrid Azure AD join en heeft al het Groepsbeleid instellen is of Configuration Manager client-instelling wordt toegepast. U moet de nieuwe installatiekopie ook gebruiken voor het inrichten van nieuwe computers die lid worden van domein van uw organisatie. 

## <a name="control-windows-down-level-devices"></a>Downlevel Windows-apparaten beheren

Voor het registreren van Windows downlevel-apparaten die u wilt downloaden en installeren van de Windows Installer-pakket (.msi) van Download Center op de [Microsoft Workplace Join voor Windows 10-computers](https://www.microsoft.com/download/details.aspx?id=53554) pagina.

U kunt het pakket implementeren met behulp van een software-distributiesysteem zoals [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Het pakket biedt ondersteuning voor de standaard installatie op de achtergrond-opties met de stille parameter. De huidige vertakking van Configuration Manager biedt voordelen ten opzichte van eerdere versies, zoals de mogelijkheid voor het bijhouden van voltooide registraties.

Het installatieprogramma maakt een geplande taak op het systeem dat wordt uitgevoerd in de context van de gebruiker. De taak wordt geactiveerd wanneer de gebruiker zich aanmeldt bij Windows. De taak op de achtergrond lid wordt van het apparaat met Azure AD met de referenties van de gebruiker na verificatie met Azure AD.

Voor het beheren van de device Registration service, moet u het Windows Installer-pakket implementeren alleen op een geselecteerde groep Windows downlevel-apparaten. Als u hebt gecontroleerd dat alles werkt zoals verwacht, u kunt het pakket op alle downlevel-apparaten worden uitgerold.


## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot apparaatbeheer in Azure Active Directory](../device-management-introduction.md)



