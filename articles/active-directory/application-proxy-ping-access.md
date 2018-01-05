---
title: Verificatie op basis van een header met PingAccess voor Azure AD-toepassingsproxy | Microsoft Docs
description: Toepassingen publiceren met PingAccess en App-Proxy naar header gebaseerde verificatie ondersteunt.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: bfff8ebff87b6c3c501202e95c463a0f4e235ffc
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Verificatie op basis van een koptekst voor eenmalige aanmelding met toepassingsproxy en PingAccess

Azure Active Directory-toepassingsproxy en PingAccess hebben hun samen Azure Active Directory om klanten te bieden met toegang tot zelfs meer toepassingen. PingAccess breidt de [bestaande toepassingsproxy aanbiedingen](active-directory-application-proxy-get-started.md) om op te nemen van één aanmelding toegang tot toepassingen die gebruikmaken van headers voor verificatie.

## <a name="what-is-pingaccess-for-azure-ad"></a>Wat is PingAccess voor Azure AD?

PingAccess voor Azure Active Directory is een aanbieding van PingAccess waarmee u gebruikerstoegang en eenmalige aanmelding geven tot toepassingen die gebruikmaken van headers voor verificatie. Een toepassing Proxy beschouwt deze apps zoals elke andere, met behulp van Azure AD verifiëren van toegang en vervolgens verkeer via de connector-service wordt doorgegeven. PingAccess bevindt zich voor de apps en zet het toegangstoken van Azure AD in een koptekst, zodat de toepassing de verificatie in de indeling die kan gelezen ontvangt.

Uw gebruikers won't iets anders zien wanneer ze zich aanmelden voor gebruik van uw zakelijke apps. Ze kunnen nog steeds overal werken uit op elk apparaat. 

Aangezien de toepassingsproxy-connectors externe verkeer naar alle apps, ongeacht hun verificatietype omleiden, moeten ze blijven automatisch, maar ook voor taakverdeling.

## <a name="how-do-i-get-access"></a>Hoe krijg ik toegang?

Aangezien dit scenario wordt aangeboden via een samenwerking tussen Azure Active Directory en PingAccess, moet u licenties voor beide services. Azure Active Directory Premium-abonnementen bevatten echter een PingAccess standaardlicentie die betrekking heeft op maximaal 20 toepassingen. Als u moet u meer dan 20 headers gebaseerde toepassingen publiceert, kunt u een extra licentie aanschaffen bij PingAccess. 

Zie [Azure Active Directory-edities](active-directory-editions.md) voor meer informatie.

## <a name="publish-your-application-in-azure"></a>Uw toepassing publiceren in Azure

In dit artikel is bedoeld voor personen die een app met dit scenario voor het eerst wilt publiceren. Dit helpt bij het aan de slag met de toepassings- en PingAccess, naast de publishing stappen. Als u beide services al hebt geconfigureerd, maar een refresher op de publicatie stappen wilt, kunt u de installatie van de connector overslaan en verplaatsen naar [uw app toevoegen aan Azure AD met toepassingsproxy](#add-your-app-to-Azure-AD-with-Application-Proxy).

>[!NOTE]
>Omdat dit scenario een verbinding tussen Azure AD is en PingAccess, enkele van de instructies aanwezig zijn op de identiteit van de Ping-site.

### <a name="install-an-application-proxy-connector"></a>Een Application Proxy connector installeren

Als u al Application Proxy ingeschakeld hebt en hebt een connector is geïnstalleerd, kunt u deze sectie overslaan en verplaatst op naar [uw app toevoegen aan Azure AD met toepassingsproxy](#add-your-app-to-azure-ad-with-application-proxy).

De connector voor toepassingsproxy is een Windows Server-service die het verkeer van uw werknemers extern naar uw gepubliceerde apps wordt verwezen. Zie voor meer installatie-instructies, [toepassingsproxy inschakelen in de Azure portal](active-directory-application-proxy-enable.md).

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als globale beheerder.
2. Selecteer **Azure Active Directory** > **toepassingsproxy**.
3. Selecteer **Connector downloaden** om de Application Proxy connector downloaden te starten. Volg de installatie-instructies.

   ![Toepassingsproxy inschakelen en de connector downloaden](./media/application-proxy-ping-access/install-connector.png)

4. Downloaden van de connector moet automatisch toepassingsproxy inschakelen voor uw directory, maar als u dit niet kunt selecteren **Enable Application Proxy**.


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a>Uw app toevoegen aan Azure AD met toepassingsproxy

Er zijn twee stappen die u moet nemen in de Azure portal. Eerst moet u uw toepassing met toepassingsproxy publiceren. Vervolgens moet u voor het verzamelen van informatie over de app die u kunt tijdens de stappen PingAccess gebruiken.

Volg deze stappen voor het publiceren van uw app. Voor een meer overzicht van de stappen 1-8, Zie gedetailleerde [toepassingen publiceren met Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md).

1. Als dat niet in de laatste sectie aanmelden bij de [Azure-portal](https://portal.azure.com) als globale beheerder.
2. Selecteer **Azure Active Directory** > **bedrijfstoepassingen**.
3. Selecteer **toevoegen** boven aan de blade.
4. Selecteer **On-premises toepassing**.
5. Vul de vereiste velden met informatie over uw nieuwe app. Gebruik de volgende richtlijnen voor de instellingen:
   - **Interne URL**: normaal bieden u de URL die u u naar de aanmeldingspagina van de app gaat als u op het bedrijfsnetwerk bevinden. Voor dit scenario moet de connector de proxy PingAccess behandelen als de voorpagina van de app. Gebruik de volgende notatie: `https://<host name of your PA server>:<port>`. De poort is 3000 standaard, maar u kunt deze configureren in PingAccess.

    > [!WARNING]
    > Voor dit type eenmalige aanmelding, worden de interne URL moet gebruikmaken van https en http niet gebruiken.

   - **Methode voor verificatie vooraf**: Azure Active Directory
   - **URL in de Headers vertalen**: Nee

   >[!NOTE]
   >Als dit uw eerste toepassing, moet u poort 3000 gebruiken om te starten en keert u terug naar het bijwerken van deze instelling als u de configuratie van uw PingAccess wijzigt. Als dit uw app tweede of hoger, moet dit overeen met de Listener die u hebt geconfigureerd in PingAccess. Meer informatie over [luisteraars in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).

6. Selecteer **toevoegen** onderaan de blade. Uw toepassing wordt toegevoegd en het menu Snel starten wordt geopend.
7. Selecteer in het menu Snel starten **een gebruiker toewijzen voor het testen van**, en ten minste één gebruiker toevoegen aan de toepassing. Zorg ervoor dat deze testaccount toegang heeft tot de on-premises toepassing.
8. Selecteer **toewijzen** om op te slaan van de toewijzing van de gebruiker test.
9. Selecteer op de blade app management **eenmalige aanmelding**.
10. Kies **headers gebaseerde aanmelding** uit de vervolgkeuzelijst. Selecteer **Opslaan**.

   >[!TIP]
   >Als dit de eerste keer is op basis van een koptekst eenmalige aanmelding, moet u PingAccess installeren. Gebruik de koppeling om te zorgen dat uw Azure-abonnement is automatisch gekoppeld aan uw installatie PingAccess, in op deze pagina voor eenmalige aanmelding PingAccess downloaden. U kunt nu de downloadsite openen of naar deze pagina later terugkomen. 

   ![Selecteer op basis van een koptekst eenmalige aanmelding](./media/application-proxy-ping-access/sso-header.PNG)

11. Sluit de blade van Enterprise-toepassingen of schuif helemaal naar links om te keren naar het menu van Azure Active Directory.
12. Selecteer **App registraties**.

   ![Selecteer de App-registraties](./media/application-proxy-ping-access/app-registrations.png)

13. Selecteer de app die u zojuist hebt toegevoegd, klikt u vervolgens **antwoord-URL's**.

   ![Selecteer de antwoord-URL 's](./media/application-proxy-ping-access/reply-urls.png)

14. Controleer de externe URL die u hebt toegewezen aan uw app in stap 5 is als in de lijst antwoord-URL's. Als dit niet het geval is, moet u deze nu toevoegen.
15. Selecteer op het tabblad app-instellingen **vereist machtigingen**.

  ![Selecteer de vereiste machtigingen](./media/application-proxy-ping-access/required-permissions.png)

16. Selecteer **Toevoegen**. Kies voor de API **Windows Azure Active Directory**, klikt u vervolgens **Selecteer**. Voor de machtigingen kiest **lezen en schrijven van alle toepassingen** en **aanmelden en gebruikersprofiel lezen**, vervolgens **Selecteer** en **gedaan**.  

  ![Machtigingen selecteren](./media/application-proxy-ping-access/select-permissions.png)

17. Machtigingen verlenen voordat u het scherm machtigingen sluit. 
![Machtigingen toekennen](media/application-proxy-ping-access/grantperms.png)

### <a name="collect-information-for-the-pingaccess-steps"></a>Informatie voor de stappen PingAccess verzamelen

1. Selecteer op de blade van uw app-instellingen **eigenschappen**. 

  ![Eigenschappen selecteren](./media/application-proxy-ping-access/properties.png)

2. Sla de **toepassings-Id** waarde. Dit wordt gebruikt voor de client-ID wanneer u PingAccess configureert.
3. Selecteer op het tabblad app-instellingen **sleutels**.

  ![Selecteer sleutels](./media/application-proxy-ping-access/Keys.png)

4. Een sleutel maken door te voeren van een sleutel beschrijving en een vervaldatum kiezen uit de vervolgkeuzelijst.
5. Selecteer **Opslaan**. Een GUID die wordt weergegeven in de **waarde** veld.

  Deze waarde nu niet opslaan omdat het niet mogelijk om het te bekijken opnieuw nadat u dit venster sluiten.

  ![Een nieuwe sleutel maken](./media/application-proxy-ping-access/create-keys.png)

6. Sluit de blade van App-registraties of schuif helemaal naar links om te keren naar het menu van Azure Active Directory.
7. Selecteer **eigenschappen**.
8. Sla de **map-ID** GUID.

### <a name="optional---update-graphapi-to-send-custom-fields"></a>Optioneel - Update GraphAPI voor het verzenden van aangepaste velden

Zie voor een lijst van beveiligingstokens die door Azure AD voor verificatie verzonden, [Azure AD-tokenverwijzing](./develop/active-directory-token-and-claims.md). Als u een aangepaste claim die door andere tokens verzonden, met grafiek Verkenner of het manifest voor de toepassing in de Azure-Portal te stelt u het veld app *acceptMappedClaims* naar **True**.    

In dit voorbeeld wordt een grafiek Explorer:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```
In dit voorbeeld wordt de [Azure-portal](https://portal.azure.com) naar udpate de *acceptedMappedClaims* veld:
1. Meld u aan bij de [Azure Portal](https://portal.azure.com) als globale beheerder.
2. Selecteer **Azure Active Directory** > **App registraties**.
3. Selecteer uw toepassing > **Manifest**.
4. Selecteer **bewerken**, zoekt de *acceptedMappedClaims* veld en wijzig de waarde in **true**.
![App-manifest](media/application-proxy-ping-access/application-proxy-ping-access-manifest.PNG)
1. Selecteer **Opslaan**.

>[!NOTE]
>Voor het gebruik van een aangepaste claim, hebt u ook een aangepast beleid gedefinieerd en toegewezen aan de toepassing.  Dit beleid moet alle vereiste aangepaste kenmerken bevatten.
>
>Beleidsdefinitie en toewijzing kunnen worden gedaan via PowerShell, Azure AD Graph Explorer of MS Graph.  Als u dit in PowerShell doen, moet u mogelijk eerst gebruiken `New-AzureADPolicy `en deze vervolgens toewijzen aan de toepassing met `Set-AzureADServicePrincipalPolicy`.  Zie voor meer informatie de [documentatie bij Azure Active Directory-beleid](active-directory-claims-mapping.md#claims-mapping-policy-assignment).

### <a name="optional---use-a-custom-claim"></a>Optioneel - gebruik een aangepaste claim
Als u uw toepassing gebruikt u een aangepaste claim en aanvullende velden opnemen, zorg ervoor dat u ook hebt [een aangepaste claims toewijzing van beleid gemaakt en toegewezen aan de toepassing](active-directory-claims-mapping.md#claims-mapping-policy-assignment).

## <a name="download-pingaccess-and-configure-your-app"></a>PingAccess downloaden en configureren van uw app

Nu dat u de stappen voor de installatie Azure Active Directory hebt voltooid, kunt u met het configureren van PingAccess op verplaatsen. 

De gedetailleerde stappen voor het PingAccess deel uitmaken van dit scenario blijven in de documentatie van de identiteit van de Ping [PingAccess configureren voor Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).

Deze stappen maakt u het proces van het ophalen van een account PingAccess als u er nog geen hebt, de PingAccess-Server installeren en een Azure AD OIDC providerverbinding maken met de ID van de Directory die u hebt gekopieerd uit de Azure-portal. Vervolgens gebruikt u de waarden toepassings-ID en sleutel voor het maken van een websessie op PingAccess. U kunt daarna Identiteitstoewijzing instellen en maken van een virtuele host, de site en de toepassing.

### <a name="test-your-app"></a>Uw app testen

Als u al deze stappen hebt voltooid, moet uw app actief en werkend. Als u wilt testen, open een browser en Ga naar de externe URL die u hebt gemaakt toen u de app in Azure hebt gepubliceerd. Aanmelden met de testaccount dat u hebt toegewezen aan de app.

## <a name="next-steps"></a>Volgende stappen

- [PingAccess configureren voor Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Hoe biedt Azure AD-toepassingsproxy eenmalige aanmelding](application-proxy-sso-overview.md)
- [Toepassingsproxy oplossen](active-directory-application-proxy-troubleshoot.md)
