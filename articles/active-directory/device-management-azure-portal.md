---
title: Het beheren van apparaten met behulp van de Azure portal - preview | Microsoft Docs
description: Informatie over het gebruik van de Azure-portal om apparaten te beheren.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 4b46e1627a229b0649d9ccd2550cd28fda9849f8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="managing-devices-using-the-azure-portal---preview"></a>Het beheren van apparaten met behulp van de Azure portal - preview

>[!NOTE]
>Deze mogelijkheid is momenteel in de openbare preview. Wees voorbereid om te herstellen of verwijderen van eventuele wijzigingen. De functie is beschikbaar in een abonnement voor Azure Active Directory (Azure AD) tijdens de openbare preview. Wanneer de functie algemeen beschikbaar wordt, kunnen sommige aspecten van de functie echter een Azure Active Directory premium-abonnement nodig.


Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen vanaf apparaten die voldoen aan uw standaarden voor beveiliging en naleving. 

Dit onderwerp:

- Wordt ervan uitgegaan dat u bekend met bent de [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md)

- Biedt u informatie over het beheren van uw apparaten met de Azure portal


Om apparaten te beheren in de Azure portal, moet u op **apparaten** in de **beheren** sectie van de de **Azure Active Directory** blade.

![Een apparaat met Intune beheren](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a>Apparaatinstellingen configureren

Voor het beheren van uw apparaten met de Azure portal, moeten deze worden geregistreerd of toegevoegd aan Azure AD. Als beheerder, kunt u het proces van registreren en door apparaten te koppelen door het configureren van de instellingen aanpassen.

![Een apparaat met Intune beheren](./media/device-management-azure-portal/22.png)


De apparaat-instellingenblade kunt u configureren:

- **Gebruikers kunnen apparaten te koppelen aan Azure AD** - deze instellingen kunt u selecteren welke gebruikers apparaten met Azure AD verbinden kunnen. De standaardwaarde is **alle**.

- **Aanvullende lokale beheerders op Azure AD die lid zijn van de apparaten** -kunt u de gebruikers die lokale beheerdersrechten op een apparaat worden verleend. Gebruikers die zijn toegevoegd, worden toegevoegd aan de *Apparaatbeheerders* rol in Azure AD. Globale beheerders in Azure AD en eigenaren van de apparaten zijn lokale beheerdersrechten standaard toegekend. Deze optie is een premium edition-functie beschikbaar via producten zoals Azure AD Premium of Enterprise Mobility Suite (EMS). 

- **Gebruikers kunnen hun apparaten registreren met Azure AD** -moet u deze instelling om apparaten worden geregistreerd bij Azure AD te laten configureren. Als u selecteert **geen**, apparaten zijn niet toegestaan om te registreren wanneer ze niet Azure AD zijn toegevoegd of hybride die lid zijn van Azure AD. Registratie bij Microsoft Intune of Mobile Device Management (MDM) voor Office 365 moet worden geregistreerd. Als u een van deze services hebt geconfigureerd **alle** is geselecteerd en **** is niet beschikbaar...

- **Multi-factor Authentication om toe te voegen apparaten vereisen** -kunt u kiezen of gebruikers zijn vereist voor een tweede verificatiefactor om hun apparaat toevoegen aan Azure AD. De standaardwaarde is **Nee**. Het is raadzaam om meervoudige verificatie vereisen wanneer een apparaat wordt geregistreerd. Voordat u multi-factor authentication voor deze service inschakelt, moet u ervoor zorgen dat multi-factor authentication-server is geconfigureerd voor de gebruikers die hun apparaten registreren. Zie voor meer informatie over andere Azure multi-factor authentication-services [aan de slag met Azure multi-factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **Maximum aantal apparaten** -deze instelling schakelt u het maximum aantal apparaten dat een gebruiker in Azure AD hebben kan selecteren. Als een gebruiker dit quotum bereikt, dat niet meer mogelijk extra apparaten toevoegen totdat een of meer van de bestaande apparaten zijn verwijderd. De aanhalingstekens van het apparaat wordt beschouwd voor alle apparaten die Azure AD zijn toegevoegd of Azure AD vandaag geregistreerd. De standaardwaarde is **20**.

- **Gebruikers kunnen instellingen en app-gegevens synchroniseren via apparaten** -deze instelling is standaard ingesteld op ****. Specifieke gebruikers of groepen of alle selecteren die u kunt de gebruiker instellingen en app-gegevens kunnen synchroniseren via hun Windows 10-apparaten. Meer informatie over de werking van synchronisatie in Windows 10.
Deze optie is een premium-functie beschikbaar via producten zoals Azure AD Premium of Enterprise Mobility Suite (EMS).
 
    ![Een apparaat met Intune beheren](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a>Apparaten zoeken

Als een beheerder hebt in de Azure portal u twee opties geregistreerd en gekoppelde apparaten vinden:

- **Alle apparaten** in de **beheren** sectie van de **apparaten** blade  

    ![Alle apparaten](./media/device-management-azure-portal/41.png)


- **Apparaten** in de **beheren** gedeelte van een **gebruiker** blade
 
    ![Alle apparaten](./media/device-management-azure-portal/43.png)



Met beide opties, kun je op een weergave die:


- Hiermee kunt u zoeken naar apparaten met behulp van de weergavenaam op die als filter.

- Biedt u gedetailleerd overzicht van geregistreerde en gekoppelde apparaten

- Hiermee kunt u algemene taken uitvoeren
   

![Alle apparaten](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Taken voor het beheer van apparaten

Als beheerder, kunt u de geregistreerde of gekoppelde apparaten beheren. Deze sectie vindt u informatie over algemene taken.


**Een apparaat met Intune beheren** -als u een Intune-beheerder bent, kunt u beheren met apparaten die zijn gemarkeerd als **Microsoft Intune**. Een beheerder kan aanvullende apparaat zien 

![Een apparaat met Intune beheren](./media/device-management-azure-portal/31.png)


**Inschakelen / uitschakelen van een Azure AD-apparaat**

Als u wilt in- of uitschakelen van een apparaat, moet u een globale beheerder in Azure AD. Als u een apparaat wordt voorkomen dat een apparaat toegang tot uw Azure AD-resources.  U kunt u het apparaat uitschakelen door op *...* Klik op het apparaat voor meer informatie.

 
![Een apparaat met Intune beheren](./media/device-management-azure-portal/33.png)

Als u een apparaat wijzigt de status in de **ingeschakeld** kolom **Nee**.

![Een apparaat uitschakelen](./media/device-management-azure-portal/32.png)


**Een Azure AD-apparaat verwijderen** : verwijderen van een apparaat, moet u een globale beheerder in Azure AD.  
Een apparaat verwijderen:
 
- Voorkomen dat een apparaat toegang tot uw Azure AD-resources 

- Hiermee verwijdert u alle gegevens die zijn gekoppeld aan het apparaat, bijvoorbeeld, BitLocker-sleutels voor Windows-apparaten  

- Hiermee geeft u een niet-herstelbare activiteit en wordt niet aanbevolen, tenzij deze vereist is

Als een apparaat wordt beheerd door een andere instantie voor beheer van (bijvoorbeeld Microsoft Intune), Controleer of het apparaat gewist / buiten gebruik gesteld voordat het apparaat wordt verwijderd in Azure AD.

U kunt ook selecteren '...' het apparaat verwijderen of klik op het apparaat voor meer informatie
 
![Een apparaat verwijderen](./media/device-management-azure-portal/34.png)


**Weergeven of kopiëren van apparaat-ID** -kunt u een apparaat-ID om te controleren of de details van de apparaat-ID op het apparaat of met behulp van PowerShell tijdens het oplossen van problemen. Voor toegang tot de optie kopiëren, klikt u op het apparaat.

![Een apparaat-ID weergeven](./media/device-management-azure-portal/35.png)
  

**Weergeven of kopiëren van BitLocker-sleutels** -als u een beheerder bent, kunt u bekijken en kopiëren van de BitLocker-sleutels, zodat gebruikers kunnen hun versleutelde station te herstellen. Deze sleutels zijn alleen beschikbaar voor Windows-apparaten die zijn versleuteld en hun sleutels hebt opgeslagen in Azure AD. Bij het openen van de details van het apparaat, kunt u deze sleutels kopiëren.
 
![BitLocker-sleutels weergeven](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Controlelogboeken


De apparaat-activiteiten zijn beschikbaar via de activiteitenlogboeken van de. Dit omvat activiteiten die zijn geactiveerd door de device registratieservice of door de gebruiker:

- Maken van het apparaat en het toevoegen van eigenaars/gebruikers op het apparaat

- Apparaatinstellingen

- Apparaatbewerkingen zoals het verwijderen of bijwerken van een apparaat
 
Uw toegangspunt voor de controle gegevens is **controlelogboeken** in de **activiteit** sectie van de **apparaten* blade.

![Controlelogboeken](./media/device-management-azure-portal/61.png)


Een controlelogboek heeft een standaardlijstweergave die het volgende laat zien:

- de datum en tijd van de gebeurtenis

- de doelen

- de initiator / actor (die) van een activiteit

- de activiteit (wat)

![Controlelogboeken](./media/device-management-azure-portal/63.png)

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.
 
![Controlelogboeken](./media/device-management-azure-portal/64.png)


Als u de gerapporteerde gegevens wilt beperken tot een niveau dat geschikt is voor u, kunt u de controlegegevens filteren met de volgende velden:

- Catergory
- Resourcetype van activiteit
- Activiteit
- Datumbereik
- doel
- Gestart door (Actor)

Naast de filters, kunt u zoeken naar specifieke vermeldingen.

![Controlelogboeken](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Volgende stappen

* [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md)



