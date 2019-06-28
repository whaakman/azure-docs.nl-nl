---
title: Over het beheren van apparaten met behulp van Azure portal | Microsoft Docs
description: Leer hoe u Azure portal gebruiken om apparaten te beheren.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4a0037d46db67460d507c6e92ab550f7d9c2fbe
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341417"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Apparaat-id's met behulp van de Azure-portal beheren

Met Apparaatbeheer identiteit in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen van apparaten die voldoen aan uw normen voor beveiliging en naleving.

Dit artikel:

- Wordt ervan uitgegaan dat u bekend met bent de [identiteit Apparaatbeheer in Azure Active Directory-inleiding](overview.md)
- Biedt u informatie over het beheren van uw apparaat-id's met behulp van de Azure AD-portal

## <a name="manage-device-identities"></a>Apparaatidentiteiten beheren

De Azure AD-portal biedt u een centrale locatie voor het beheren van uw apparaat-id's. U toegang hebt tot deze locatie met behulp van een [directe koppeling](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices) of door de volgende handmatige stappen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) als administrator.
2. Klik op de navigatiebalk links **Active Directory**.

   ![Apparaatinstellingen configureren](./media/device-management-azure-portal/01.png)

3. In de **beheren** sectie, klikt u op **apparaten**.

   ![Apparaatinstellingen configureren](./media/device-management-azure-portal/74.png)

De **apparaten** pagina kunt u naar:

- De apparaatinstellingen configureren
- Apparaten zoeken
- Apparaat-id-beheertaken uitvoeren
- Apparaat-gerelateerde auditlogboeken bekijken  
  
## <a name="configure-device-settings"></a>Apparaatinstellingen configureren

Voor het beheren van uw apparaat-id's met behulp van de Azure AD-portal, de apparaten moeten zijn [geregistreerd, of deel uitmaken](overview.md) naar Azure AD. Als beheerder, kunt u het proces van het registreren en het toevoegen van apparaten door het configureren van de instellingen voor apparaten aanpassen.

![Apparaatinstellingen configureren](./media/device-management-azure-portal/22.png)

De instellingenpagina voor het apparaat kunt u configureren:

![Een Intune-apparaat beheren](./media/device-management-azure-portal/21.png)

- **Gebruikers kunnen apparaten koppelen aan Azure AD** -deze instelling kunt u de gebruikers die hun apparaten kunnen registreren selecteren [in Azure AD join apparaten](overview.md#azure-ad-joined-devices). De standaardwaarde is **alle**.

> [!NOTE]
> **Gebruikers kunnen apparaten koppelen aan Azure AD** instelling is alleen van toepassing op Azure AD join op Windows 10.

- **Extra lokale beheerders op Azure AD gekoppelde apparaten** -kunt u de gebruikers die lokale beheerdersrechten op een apparaat worden verleend. Gebruikers die zijn toegevoegd, worden toegevoegd aan de *Apparaatbeheerders* rol in Azure AD. Globale beheerders in Azure AD en eigenaren van de apparaten standaard lokale beheerdersrechten worden verleend. Deze optie is een premium-editie-mogelijkheid beschikbaar via producten, zoals Azure AD Premium of Enterprise Mobility Suite (EMS).
- **Gebruikers kunnen hun apparaten registreren bij Azure AD** -u moet deze instelling in om Windows 10 pc, iOS, Android en macOs-apparaten moet configureren [geregistreerd](overview.md#azure-ad-registered-devices) met Azure AD. Als u selecteert **geen**, apparaten, zijn niet toegestaan om u te registreren bij Azure AD. Registratie bij Microsoft Intune of Mobile Device Management (MDM) voor Office 365 is registratie vereist. Als u een van deze services hebt geconfigureerd **alle** is geselecteerd en **is** niet beschikbaar. Als u kunt deze instelling uit Intune hebt ingeschakeld, en vervolgens de opties hier grijs weergegeven.
- **Multi-factor Authentication worden toegevoegd aan apparaten vereist** -u kunt kiezen of gebruikers zijn vereist voor een tweede verificatiefactor naar [join](overview.md#azure-ad-joined-devices) hun apparaat naar Azure AD. De standaardwaarde is **Nee**. Het is raadzaam om meervoudige verificatie vereisen bij het registreren van een apparaat. Voordat u multi-factor authentication voor deze service inschakelt, moet u ervoor zorgen dat de Azure multi-factor Authentication is geconfigureerd voor de gebruikers die hun apparaten registreren. Zie voor meer informatie het artikel [implementatie van Azure multi-factor Authentication](../authentication/howto-mfa-getstarted.md). 

> [!NOTE]
> **Multi-factor Authentication worden toegevoegd aan apparaten vereist** instelling geldt niet voor hybride Azure AD gekoppelde apparaten.

- **Maximum aantal apparaten** -deze instelling kunt u het maximum aantal apparaten dat een gebruiker in Azure AD hebben mag selecteren. Als een gebruiker dit quotum bereikt, dat niet meer mogelijk extra apparaten worden toegevoegd totdat een of meer van de bestaande apparaten worden verwijderd. Het quotum van het apparaat wordt geteld voor alle apparaten die zijn toegevoegd aan Azure AD of Azure AD geregistreerd vandaag nog. De standaardwaarde is **20**.

> [!NOTE]
> **Maximum aantal apparaten** instelling geldt niet voor hybride Azure AD gekoppelde apparaten.

- **Gebruikers kunnen instellingen en app-gegevens synchroniseren via apparaten** -deze instelling is standaard ingesteld op **NONE**. Specifieke gebruikers of groepen of alle selecteren, kunt van de gebruiker instellingen en app-gegevens kunnen synchroniseren via hun Windows 10-apparaten. Meer informatie over de werking van synchronisatie in Windows 10.
Deze optie is een premium-capaciteit beschikbaar via producten, zoals Azure AD Premium of Enterprise Mobility Suite (EMS).

## <a name="locate-devices"></a>Apparaten zoeken

U hebt twee opties geregistreerd en die is toegevoegd aan apparaten vinden:

- **Alle apparaten** in de **beheren** sectie van de **apparaten** pagina  

   ![Alle apparaten](./media/device-management-azure-portal/41.png)

- **Apparaten** in de **beheren** gedeelte van een **gebruiker** pagina

   ![Alle apparaten](./media/device-management-azure-portal/43.png)

Met beide opties, krijgt u een weergave die:

- Hiermee kunt u zoeken naar apparaten met behulp van de weergavenaam als filter.
- Biedt u gedetailleerd overzicht van geregistreerde en verbonden apparaten
- Hiermee kunt u algemene beheertaken voor apparaat uitvoeren

![Alle apparaten](./media/device-management-azure-portal/51.png)

Voor sommige iOS-apparaten kunnen de namen van apostroffen met verschillende tekens die lijkt op apostroffen gebruiken. Dus zoekt naar dergelijke apparaten een lastige is-als u niet ziet zoekresultaten correct ervoor te zorgen dat de zoektekenreeks overeenkomende aanhalingsteken bevat.

## <a name="device-identity-management-tasks"></a>Beheertaken voor apparaat-id

Als een globale beheerder of cloud-apparaatbeheerder, kunt u de geregistreerde of gekoppelde apparaten beheren. Er kunnen beheerders van de Intune-Service:

- Update-apparaten - voorbeelden zijn dagelijkse bewerkingen zoals het inschakelen/uitschakelen van apparaten
- Verwijderen van apparaten: wanneer een apparaat buiten gebruik is gesteld en moet worden verwijderd in Azure AD

In deze sectie vindt u informatie over algemene beheertaken voor apparaat-id.

### <a name="manage-an-intune-device"></a>Een Intune-apparaat beheren

Als u een Intune-beheerder bent, kunt u apparaten die zijn gemarkeerd als beheren **Microsoft Intune**.

![Een Intune-apparaat beheren](./media/device-management-azure-portal/31.png)

### <a name="enable--disable-an-azure-ad-device"></a>Inschakelen / uitschakelen van een Azure AD-apparaat

Als u wilt inschakelen / uitschakelen van een apparaat, hebt u twee opties:

- Het menu taken ('...') op de **alle apparaten** pagina

   ![Een Intune-apparaat beheren](./media/device-management-azure-portal/71.png)

- De werkbalk op de **apparaten** pagina

   ![Een Intune-apparaat beheren](./media/device-management-azure-portal/32.png)

**Opmerking:**

- U moet een globale beheerder of apparaatbeheerder van het in Azure AD inschakelen / uitschakelen van een apparaat in de cloud. 
- Als u een apparaat uitschakelt, wordt voorkomen dat een apparaat is verificatie met Azure AD, waardoor het apparaat toegang krijgen tot uw Azure AD-resources die zijn beveiligd door apparaat CA of met behulp van de referenties van uw WH4B.

### <a name="delete-an-azure-ad-device"></a>Een Azure AD-apparaat verwijderen

Als u wilt een apparaat verwijdert, hebt u twee opties:

- Het menu taken ('...') op de **alle apparaten** pagina

   ![Een Intune-apparaat beheren](./media/device-management-azure-portal/72.png)

- De werkbalk op de **apparaten** pagina

   ![Een apparaat verwijderen](./media/device-management-azure-portal/34.png)

**Opmerking:**

- U moet een globale beheerder of Intune-beheerder in Azure AD te verwijderen van een apparaat.
- Als u een apparaat verwijdert:
   - Hiermee voorkomt dat een apparaat toegang tot uw Azure AD-resources.
   - Hiermee verwijdert u alle details die zijn gekoppeld aan het apparaat, bijvoorbeeld, BitLocker-sleutels voor Windows-apparaten.  
   - Hiermee geeft u een niet-herstelbare activiteit en wordt niet aanbevolen, tenzij deze vereist is.

Als een apparaat wordt beheerd door een andere instantie voor beheer van (bijvoorbeeld Microsoft Intune), ervoor zorgen dat het apparaat is gewist / buiten gebruik gesteld voordat het apparaat wordt verwijderd in Azure AD.

### <a name="view-or-copy-device-id"></a>Weergeven of kopiëren van apparaat-ID

U kunt een apparaat-ID gebruiken om te controleren of de details van de apparaat-ID op het apparaat of met behulp van PowerShell tijdens het oplossen van problemen. Voor toegang tot de optie kopiëren, klikt u op het apparaat.

![Een apparaat-ID weergeven](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>Weergeven of kopiëren van BitLocker-sleutels

U kunt bekijken en kopiëren van de BitLocker-sleutels om te helpen gebruikers om te herstellen van het versleutelde station. Deze sleutels zijn alleen beschikbaar voor Windows-apparaten die zijn versleuteld en hebben hun sleutels opgeslagen in Azure AD. Bij het openen van details van het apparaat, kunt u deze sleutels kopiëren.

![BitLocker-sleutels weergeven](./media/device-management-azure-portal/36.png)

Als u wilt weergeven of kopieer de BitLocker-sleutels, moet u de eigenaar van het apparaat of een gebruiker die heeft ten minste één van de volgende rollen toegewezen:

- Globale beheerder
- Helpdesk-beheerder
- Beveiligingsbeheerder
- Beveiligingslezer
- Intune-servicebeheerder

> [!NOTE]
> Hybrid Azure AD gekoppelde Windows 10-apparaten beschikt niet over een eigenaar. Dus als u naar een apparaat door de eigenaar zoekt en deze niet hebt gevonden, zoeken op het apparaat-ID.

## <a name="audit-logs"></a>Auditlogboeken

Apparaat-activiteiten zijn beschikbaar via de activiteitenlogboeken. Deze logboeken bevatten de activiteiten die zijn geactiveerd door de device registratieservice en door gebruikers:

- Maken van het apparaat en toe te voegen eigenaars / gebruikers op het apparaat
- Wijzigingen in de instellingen voor apparaten
- Apparaatbewerkingen, zoals het verwijderen of bijwerken van een apparaat

Uw beginpunt voor de controlegegevens is **auditlogboeken** in de **activiteit** sectie van de **apparaten** pagina.

![Auditlogboeken](./media/device-management-azure-portal/61.png)

Een controlelogboek heeft een standaardlijstweergave die het volgende laat zien:

- De datum en tijd van de gebeurtenis
- De doelen
- De initiator / actor (wie) van een activiteit
- De activiteit (wat)

![Auditlogboeken](./media/device-management-azure-portal/63.png)

U kunt de lijstweergave aanpassen door te klikken op **Kolommen** op de werkbalk.

![Auditlogboeken](./media/device-management-azure-portal/64.png)

Als u de gerapporteerde gegevens wilt beperken tot een niveau dat geschikt is voor u, kunt u de controlegegevens filteren met de volgende velden:

- Categorie
- Resourcetype van activiteit
- Activiteit
- Datumbereik
- Doel
- Gestart door (Actor)

Naast de filters, kunt u zoeken naar specifieke vermeldingen.

![Auditlogboeken](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Volgende stappen

[Verouderde apparaten beheren in Azure AD](manage-stale-devices.md)
