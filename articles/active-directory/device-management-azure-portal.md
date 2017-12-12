---
title: Het beheren van apparaten met de Azure portal | Microsoft Docs
description: Informatie over het gebruik van de Azure-portal om apparaten te beheren.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1e0d40b996e181a606d16d26633f890b9169ecbb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="managing-devices-using-the-azure-portal"></a>Het beheer van apparaten met de Azure portal


Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen vanaf apparaten die voldoen aan uw standaarden voor beveiliging en naleving. 

Dit onderwerp:

- Wordt ervan uitgegaan dat u bekend met bent de [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md)

- Biedt u informatie over het beheren van uw apparaten met de Azure portal

## <a name="manage-devices"></a>Apparaten beheren 

De Azure-portal biedt een centrale locatie voor het beheren van uw apparaten. U toegang krijgen tot deze locatie met behulp van een [directe koppeling](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) of met de volgende handmatige stappen:

1. Kana bij de [Azure-portal](https://portal.azure.com) als administrator.

2. Klik op de navigatiebalk links, **Active Directory**.

    ![Apparaatinstellingen configureren](./media/device-management-azure-portal/01.png)

3. In de **beheren** sectie, klikt u op **apparaten**.

    ![Apparaatinstellingen configureren](./media/device-management-azure-portal/11.png)
 
De **apparaten** pagina kunt u:

- Uw instellingen voor het beheer van apparaten configureren

- Apparaten zoeken

- Apparaat-beheertaken uitvoeren

- Bekijk de gerelateerde Apparaatbeheer controlelogboeken  
  

## <a name="configure-device-settings"></a>Apparaatinstellingen configureren

Voor het beheren van uw apparaten met de Azure portal uw apparaten, moeten deze [ingeschreven of die lid zijn van](device-management-introduction.md#getting-devices-under-the-control-of-azure-ad) naar Azure AD. Als beheerder, kunt u het proces van registreren en door apparaten te koppelen door het configureren van de instellingen aanpassen. 

![Apparaatinstellingen configureren](./media/device-management-azure-portal/22.png)

De instellingenpagina voor het apparaat kunt u configureren:

![Een apparaat met Intune beheren](./media/device-management-azure-portal/21.png)


- **Gebruikers kunnen apparaten te koppelen aan Azure AD** -deze instelling schakelt u de gebruikers die u kunnen selecteren [apparaten te koppelen aan](device-management-introduction.md#azure-ad-joined-devices) naar Azure AD. De standaardwaarde is **alle**.

- **Aanvullende lokale beheerders op Azure AD die lid zijn van de apparaten** -kunt u de gebruikers die lokale beheerdersrechten op een apparaat worden verleend. Gebruikers die zijn toegevoegd, worden toegevoegd aan de *Apparaatbeheerders* rol in Azure AD. Globale beheerders in Azure AD en eigenaren van de apparaten zijn lokale beheerdersrechten standaard toegekend. Deze optie is een premium edition-functie beschikbaar via producten zoals Azure AD Premium of Enterprise Mobility Suite (EMS). 

- **Gebruikers kunnen hun apparaten registreren met Azure AD** -u moet deze instelling en ervoor zorgen dat apparaten worden [geregistreerd](device-management-introduction.md#azure-ad-registered-devices) met Azure AD. Als u selecteert **geen**, apparaten zijn niet toegestaan om te registreren wanneer ze niet Azure AD zijn toegevoegd of hybride die lid zijn van Azure AD. Registratie bij Microsoft Intune of Mobile Device Management (MDM) voor Office 365 moet worden geregistreerd. Als u een van deze services hebt geconfigureerd **alle** is geselecteerd en is **NONE** niet beschikbaar...

- **Multi-factor Authentication om toe te voegen apparaten vereisen** -kunt u kiezen of gebruikers zijn vereist voor een tweede verificatiefactor naar [join](device-management-introduction.md#azure-ad-joined-devices) hun apparaat bij Azure AD. De standaardwaarde is **Nee**. Het is raadzaam om meervoudige verificatie vereisen wanneer een apparaat wordt geregistreerd. Voordat u multi-factor authentication voor deze service inschakelt, moet u ervoor zorgen dat multi-factor authentication-server is geconfigureerd voor de gebruikers die hun apparaten registreren. Zie voor meer informatie over andere Azure multi-factor authentication-services [aan de slag met Azure multi-factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **Maximum aantal apparaten** -deze instelling schakelt u het maximum aantal apparaten dat een gebruiker in Azure AD hebben kan selecteren. Als een gebruiker dit quotum bereikt, dat niet meer mogelijk extra apparaten toevoegen totdat een of meer van de bestaande apparaten zijn verwijderd. De aanhalingstekens van het apparaat wordt beschouwd voor alle apparaten die Azure AD zijn toegevoegd of Azure AD vandaag geregistreerd. De standaardwaarde is **20**.

- **Gebruikers kunnen instellingen en app-gegevens synchroniseren via apparaten** -deze instelling is standaard ingesteld op **NONE**. Specifieke gebruikers of groepen of alle selecteren die u kunt de gebruiker instellingen en app-gegevens kunnen synchroniseren via hun Windows 10-apparaten. Meer informatie over de werking van synchronisatie in Windows 10.
Deze optie is een premium-functie beschikbaar via producten zoals Azure AD Premium of Enterprise Mobility Suite (EMS).
 




## <a name="locate-devices"></a>Apparaten zoeken

U hebt twee opties geregistreerd en gekoppelde apparaten vinden:

- **Alle apparaten** in de **beheren** sectie van de **apparaten** pagina  

    ![Alle apparaten](./media/device-management-azure-portal/41.png)


- **Apparaten** in de **beheren** gedeelte van een **gebruiker** pagina
 
    ![Alle apparaten](./media/device-management-azure-portal/43.png)



Met beide opties, kun je op een weergave die:


- Hiermee kunt u zoeken naar apparaten met behulp van de weergavenaam op die als filter.

- Biedt u gedetailleerd overzicht van geregistreerde en gekoppelde apparaten

- Hiermee kunt u algemene taken uitvoeren
   

![Alle apparaten](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Taken voor het beheer van apparaten

Als beheerder, kunt u de geregistreerde of gekoppelde apparaten beheren. Deze sectie vindt u informatie over algemene taken.


### <a name="manage-an-intune-device"></a>Een apparaat met Intune beheren

Als u een Intune-beheerder bent, kunt u beheren met apparaten die zijn gemarkeerd als **Microsoft Intune**. Een beheerder kan aanvullende apparaat zien 

![Een apparaat met Intune beheren](./media/device-management-azure-portal/31.png)


### <a name="enable--disable-an-azure-ad-device"></a>Inschakelen / uitschakelen van een Azure AD-apparaat

Als u wilt inschakelen / uitschakelen van een apparaat, hebt u twee opties:

- Het menu taken ('...') op de **alle apparaten** pagina

    ![Een apparaat met Intune beheren](./media/device-management-azure-portal/71.png)

- De werkbalk op de **apparaten** pagina

    ![Een apparaat met Intune beheren](./media/device-management-azure-portal/32.png)


**Opmerkingen:**

- U moet een globale beheerder in Azure AD inschakelen / uitschakelen van een apparaat. 
- Als u een apparaat wordt voorkomen dat een apparaat toegang tot uw Azure AD-resources. 



### <a name="delete-an-azure-ad-device"></a>Een Azure AD-apparaat verwijderen

Als u wilt verwijderen van een apparaat, hebt u twee opties:

- Het menu taken ('...') op de **alle apparaten** pagina

    ![Een apparaat met Intune beheren](./media/device-management-azure-portal/72.png)

- De werkbalk op de **apparaten** pagina

    ![Een apparaat verwijderen](./media/device-management-azure-portal/34.png)


**Opmerkingen:**

- U moet een globale beheerder in Azure AD te verwijderen van een apparaat.  

- Een apparaat verwijderen:
 
    - Hiermee voorkomt dat een apparaat toegang tot uw Azure AD-resources. 

    - Hiermee verwijdert u alle gegevens die zijn gekoppeld aan het apparaat, bijvoorbeeld, BitLocker-sleutels voor Windows-apparaten.  

    - Hiermee geeft u een niet-herstelbare activiteit en wordt niet aanbevolen, tenzij deze vereist is.

Als een apparaat wordt beheerd door een andere instantie voor beheer van (bijvoorbeeld Microsoft Intune), Controleer of het apparaat gewist / buiten gebruik gesteld voordat het apparaat wordt verwijderd in Azure AD.

 


### <a name="view-or-copy-device-id"></a>Weergeven of kopiëren van apparaat-ID

U kunt een apparaat-ID gebruiken om te controleren of de details van de apparaat-ID op het apparaat of met behulp van PowerShell tijdens het oplossen van problemen. Voor toegang tot de optie kopiëren, klikt u op het apparaat.

![Een apparaat-ID weergeven](./media/device-management-azure-portal/35.png)
  

### <a name="view-or-copy-bitlocker-keys"></a>Weergeven of kopiëren van BitLocker-sleutels

Als u een beheerder bent, kunt u deze kunt bekijken en kopiëren van de BitLocker-sleutels, zodat gebruikers kunnen hun versleutelde station te herstellen. Deze sleutels zijn alleen beschikbaar voor Windows-apparaten die zijn versleuteld en hun sleutels hebt opgeslagen in Azure AD. Bij het openen van de details van het apparaat, kunt u deze sleutels kopiëren.
 
![BitLocker-sleutels weergeven](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Controlelogboeken


Apparaat-activiteiten zijn beschikbaar via de activiteitenlogboeken van de. Dit omvat activiteiten die zijn geactiveerd door de device registratieservice en door gebruikers:

- Maken van het apparaat en toe te voegen eigenaars / gebruikers op het apparaat

- Apparaatinstellingen

- Apparaatbewerkingen zoals het verwijderen of bijwerken van een apparaat
 
Uw toegangspunt voor de controle gegevens is **controlelogboeken** in de **activiteit** sectie van de **apparaten** pagina.

![Controlelogboeken](./media/device-management-azure-portal/61.png)


Een controlelogboek heeft een standaardlijstweergave die het volgende laat zien:

- de datum en tijd van de instantie

- de doelen

- de initiator / actor (die) van een activiteit

- de activiteit (wat)

![Controlelogboeken](./media/device-management-azure-portal/63.png)

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.
 
![Controlelogboeken](./media/device-management-azure-portal/64.png)


Als u de gerapporteerde gegevens wilt beperken tot een niveau dat geschikt is voor u, kunt u de controlegegevens filteren met de volgende velden:

- Category
- Resourcetype van activiteit
- Activiteit
- Datumbereik
- Doel
- Gestart door (Actor)

Naast de filters, kunt u zoeken naar specifieke vermeldingen.

![Controlelogboeken](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md)



