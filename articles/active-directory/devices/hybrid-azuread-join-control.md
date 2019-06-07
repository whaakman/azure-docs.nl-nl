---
title: Gecontroleerde validatie van hybride Azure AD join - Azure AD
description: Meer informatie over het uitvoeren van een gecontroleerde validatie van hybride Azure AD join voordat u dit inschakelt in de hele organisatie in één keer
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: article
ms.date: 05/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd5b388f92a875fb2635037a6eae3ff3b6a95793
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66513291"
---
# <a name="controlled-validation-of-hybrid-azure-ad-join"></a>Gecontroleerde validatie van hybride Azure AD-deelname

Wanneer alle van de vereisten gemaakt zijn, wordt automatisch Windows-apparaten registreren als apparaten in uw Azure AD-tenant. De status van deze apparaat-id's in Azure AD wordt verwezen als hybride Azure AD join. Meer informatie over de concepten die in dit artikel vindt u in de artikelen [Inleiding tot Apparaatbeheer in Azure Active Directory](overview.md) en [uw hybride Azure Active Directory join-implementatie plannen ](hybrid-azuread-join-plan.md).

Organisaties kunnen wilt een gecontroleerde validatie van hybride Azure AD join voordat u dit inschakelt in de hele organisatie in één keer te doen. In dit artikel wordt uitgelegd hoe u kunt een gecontroleerde validatie van hybride Azure AD join uitvoeren.

## <a name="controlled-validation-of-hybrid-azure-ad-join-on-windows-current-devices"></a>Gecontroleerde validatie van hybride Azure AD join op huidige Windows-apparaten

Voor apparaten met het besturingssysteem voor Windows-bureaublad, de ondersteunde versie is de Windows 10 Jubileumupdate (versie 1607) of hoger. Als een best practice, een upgrade uitvoeren naar de nieuwste versie van Windows 10.

Als u wilt een gecontroleerde validatie van hybride Azure AD join op huidige Windows-apparaten doen, moet u naar:

1. Schakel de Service Connection Point (SCP) vermelding in Active Directory (AD) indien aanwezig
1. Client-side-registerinstelling voor SCP op uw domein computers met behulp van een groepsbeleidsobject (GPO) configureren
1. Als u AD FS gebruikt, moet u ook de client-side-registerinstelling voor SCP configureren op de AD FS-server met behulp van een groepsbeleidsobject  



### <a name="clear-the-scp-from-ad"></a>Schakel het SCP uit Active Directory

Gebruik de Active Directory Services Interfaces Editor (ADSI bewerken) om de SCP-objecten in AD.

1. Start de **ADSI Edit** bureaubladtoepassing uit en werkstation voor beheer of een domeincontroller als een Enterprise-beheerder.
1. Verbinding maken met de **naamgevingscontext configuratie** van uw domein.
1. Blader naar **CN = configuratie, DC = contoso, DC = com** > **CN = Services** > **CN = registratie apparaatconfiguratie**
1. Klik met de rechtermuisknop op de leaf-object onder **CN = registratie apparaatconfiguratie** en selecteer **eigenschappen**
   1. Selecteer **trefwoorden** uit de **Kenmerkeditor** venster en klikt u op **bewerken**
   1. Selecteer de waarden van **azureADId** en **azureADName** (één op een tijdstip) en klikt u op **verwijderen**
1. Sluiten **ADSI bewerken**


### <a name="configure-client-side-registry-setting-for-scp"></a>Client-side-registerinstelling voor SCP configureren

Gebruik het volgende voorbeeld om te maken van een groepsbeleidsobject (GPO) voor het implementeren van een instelling in het register configureren van een SCP-vermelding in het register van uw apparaten.

1. Open een Group Policy Management console en maak een nieuwe Group Policy Object in uw domein.
   1. Geef uw nieuwe groepsbeleidsobject een naam (bijvoorbeeld ClientSideSCP).
1. Bewerk het GPO en zoekt u het volgende pad: **Computerconfiguratie** > **voorkeuren** > **Windows-instellingen** > **register**
1. Met de rechtermuisknop op het register en selecteer **nieuw** > **registeritem**
   1. Op de **algemene** tabblad, configureer de volgende
      1. Actie: **Update**
      1. Component: **HKEY_LOCAL_MACHINE**
      1. Sleutelpad: **SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ\AAD**
      1. Waardenaam: **TenantId**
      1. Waardetype: **REG_SZ**
      1. Waardegegevens: De GUID of **map-ID** van uw Azure AD-exemplaar (deze waarde kan worden gevonden de **Azure-portal** > **Azure Active Directory**  >   **Eigenschappen** > **map-ID**)
   1. Klik op **OK**
1. Met de rechtermuisknop op het register en selecteer **nieuw** > **registeritem**
   1. Op de **algemene** tabblad, configureer de volgende
      1. Actie: **Update**
      1. Component: **HKEY_LOCAL_MACHINE**
      1. Sleutelpad: **SOFTWARE\Microsoft\Windows\CurrentVersion\CDJ\AAD**
      1. Waardenaam: **TenantName**
      1. Waardetype: **REG_SZ**
      1. Waardegegevens: Uw geverifieerde **domeinnaam** in Azure AD (bijvoorbeeld `contoso.onmicrosoft.com` of andere geverifieerde domeinnaam op in uw directory)
   1. Klik op **OK**
1. Sluit de editor voor het nieuwe groepsbeleidsobject
1. Het nieuwe groepsbeleidsobject koppelen aan de gewenste organisatie-eenheid met domein computers die deel uitmaken van uw beheerde implementatie populatie

### <a name="configure-ad-fs-settings"></a>AD FS-instellingen configureren

Als u AD FS gebruikt, moet u eerst aan de clientzijde SCP met behulp van de instructies die hierboven worden vermeld, maar het groepsbeleidsobject koppelen aan uw AD FS-servers configureren. Deze configuratie is vereist voor AD FS tot stand brengen van de bron voor apparaat-id's als Azure AD.

## <a name="controlled-validation-of-hybrid-azure-ad-join-on-windows-down-level-devices"></a>Gecontroleerde validatie van hybride Azure AD join op Windows downlevel-apparaten

Voor het registreren van Windows downlevel-apparaten, organisaties moeten installeren [Microsoft Workplace Join voor Windows 10-computers](https://www.microsoft.com/download/details.aspx?id=53554) beschikbaar is op het Microsoft Download Center.

U kunt het pakket implementeren met behulp van een software-distributiesysteem zoals [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager). Het pakket biedt ondersteuning voor de standaard installatie op de achtergrond-opties met de stille parameter. De huidige vertakking van Configuration Manager biedt voordelen ten opzichte van eerdere versies, zoals de mogelijkheid voor het bijhouden van voltooide registraties.

Het installatieprogramma maakt een geplande taak op het systeem die wordt uitgevoerd in de context van de gebruiker. De taak wordt geactiveerd wanneer de gebruiker zich aanmeldt bij Windows. De taak op de achtergrond lid wordt van het apparaat met Azure AD met de referenties van de gebruiker na verificatie met Azure AD.

Voor het beheren van de device Registration service, moet u het Windows Installer-pakket implementeren op de geselecteerde groep Windows downlevel-apparaten.

> [!NOTE]
> Als een SCP niet is geconfigureerd in AD, dan dezelfde benadering volgt zoals beschreven op [configureren van client-side-registerinstelling voor SCP](#configure-client-side-registry-setting-for-scp)) op uw domein computers met behulp van een groepsbeleidsobject (GPO).


Nadat u hebt gecontroleerd dat alles werkt zoals verwacht, kunt u de rest van uw Windows-apparaten voor huidige en eerdere automatisch registreren met Azure AD door [SCP met Azure AD Connect configureren](hybrid-azuread-join-managed-domains.md#configure-hybrid-azure-ad-join).

## <a name="next-steps"></a>Volgende stappen

[De implementatie van uw hybride Azure Active Directory-deelname plannen](hybrid-azuread-join-plan.md)
