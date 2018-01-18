---
title: Azure AD Connect - AD FS-beheer en de aanpassing | Microsoft Docs
description: AD FS-beheer met Azure AD Connect en aanpassen van AD FS-aanmeldingspagina gebruikerservaring met Azure AD Connect en PowerShell.
keywords: AD FS, ADFS, AD FS-beheer zijn AAD Connect, Connect, aanmelden, AD FS aanpassing, trust, O365, Federatie, relying party herstellen
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: mtillman
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 49acea5c08a10ba3b60d0db5f05e30d573f5e507
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a>Beheren en aanpassen van Active Directory Federation Services met behulp van Azure AD Connect
Dit artikel wordt beschreven hoe u kunt beheren en aanpassen van Active Directory Federation Services (AD FS) met behulp van Azure Active Directory (Azure AD) verbinding maken. Dit omvat ook andere algemene AD FS-taken die u moet doen voor een volledige configuratie van een AD FS-farm.

| Onderwerp | Er wordt aangegeven |
|:--- |:--- |
| **Manage AD FS** | |
| [Herstellen van de vertrouwensrelatie](#repairthetrust) |Klik hier voor meer informatie over het herstellen van de federatieve vertrouwensrelatie met Office 365. |
| [Gefedereerd met Azure AD met behulp van alternatieve aanmeldings-ID](#alternateid) | Federatie met behulp van alternatieve aanmeldings-ID configureren  |
| [Een AD FS-server toevoegen](#addadfsserver) |Het uitbreiden van een AD FS-farm met een extra AD FS-server. |
| [Een AD FS Web Application Proxy-server toevoegen](#addwapserver) |Het uitbreiden van een AD FS-farm met een extra Webtoepassingsproxy (WAP)-server. |
| [Een federatieve domein toevoegen](#addfeddomain) |Het toevoegen van een federatieve domein. |
| [Het SSL-certificaat bijwerken](active-directory-aadconnectfed-ssl-update.md)| Het bijwerken van het SSL-certificaat voor AD FS-farm. |
| **Aanpassen van AD FS** | |
| [Een aangepast bedrijfslogo of afbeelding toevoegen](#customlogo) |Het aanpassen van een AD FS-aanmeldingspagina met een bedrijfslogo en afbeelding. |
| [Een beschrijving van de aanmeldingspagina toevoegen](#addsignindescription) |Het toevoegen van een beschrijving van de aanmeldingspagina. |
| [Wijzigen van de claimregels van AD FS](#modclaims) |Het wijzigen van de AD FS-claims voor verschillende scenario's met Federatie. |

## <a name="manage-ad-fs"></a>Beheren van AD FS
U kunt verschillende AD FS-gerelateerde taken in Azure AD Connect met minimale tussenkomst uitvoeren met behulp van de Azure AD Connect-wizard. Nadat u klaar bent met het Azure AD Connect installeert door de wizard uitvoert, kunt u de wizard opnieuw uitvoeren van extra taken uitvoeren.

## <a name="repairthetrust"></a>Herstellen van de vertrouwensrelatie 
U kunt Azure AD Connect gebruiken om te controleren van de huidige status van de AD FS en Azure AD vertrouwt en de benodigde stappen ondernemen om te herstellen van de vertrouwensrelatie. Volg deze stappen voor het herstellen van uw Azure AD en AD FS vertrouwen.

1. Selecteer **reparatie AAD en ADFS-vertrouwen** uit de lijst met aanvullende taken.
   ![Herstellen van AAD- en AD FS vertrouwen](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)

2. Op de **verbinding maken met Azure AD** pagina, Geef uw referenties voor de globale beheerder voor Azure AD en klikt u op **volgende**.
   ![Verbinding maken met Azure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)

3. Op de **RAS-referenties** pagina, voert u de referenties voor de domeinbeheerder.

   ![Referenties voor externe toegang](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    Nadat u op **volgende**, Azure AD Connect health certificaat controleert en ziet u eventuele problemen.

    ![Status van certificaten](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    De **klaar om te configureren** pagina bevat de lijst met acties die worden uitgevoerd om te herstellen van de vertrouwensrelatie.

    ![Klaar om te configureren](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. Klik op **installeren** te herstellen van de vertrouwensrelatie.

> [!NOTE]
> Azure AD Connect kan alleen herstellen of act op certificaten die zelf-ondertekend zijn. Azure AD Connect-certificaten van derden niet worden hersteld.

## <a name="alternateid"></a>Gefedereerd met Azure AD dat gebruikmaakt van AlternateID 
Het wordt aanbevolen dat de lokale gebruiker Principal Name(UPN) en de cloud principalnaam van gebruiker hetzelfde blijven. Als de UPN van de lokale gebruikmaakt van een niet-routeerbare domein (ex. Contoso.local) of kan niet worden gewijzigd vanwege lokale toepassingsafhankelijkheden, wordt aangeraden instellen van de alternatieve aanmeldings-ID. Alternatieve aanmeldings-ID kunt u een aanmeldingservaring configureren waarbij gebruikers zich kunnen aanmelden met een kenmerk dan de UPN, zoals e-mail. De keuze voor principalnaam van gebruiker in Azure AD Connect wordt standaard ingesteld op het kenmerk userPrincipalName in Active Directory. Als u een ander kenmerk voor de principalnaam van gebruiker kiezen en zijn Federatie met AD FS, vervolgens Azure AD Connect AD FS wilt configureren voor alternatieve aanmeldings-ID. Een voorbeeld van het kiezen van een ander kenmerk voor User Principal Name wordt hieronder weergegeven:

![Selectie van alternatieve ID-kenmerk](media/active-directory-aadconnect-federation-management/attributeselection.png)

Alternatieve aanmeldings-ID configureren voor AD FS bestaat uit twee belangrijke stappen:
1. **Configureer de juiste set met uitgifteregels claims**: claimregels voor de uitgifte van de Azure AD relying party trust voor het gebruik van het geselecteerde kenmerk UserPrincipalName als alternatieve-ID van de gebruiker worden gewijzigd.
2. **Schakel alternatieve aanmeldings-ID in de AD FS-configuratie**: de AD FS-configuratie is bijgewerkt, zodat AD FS kunt opzoeken van de gebruikers in de juiste forests met behulp van de alternatieve-ID. Deze configuratie wordt ondersteund voor AD FS in Windows Server 2012 R2 (met KB2919355) of hoger. Als de AD FS-servers 2012 R2 zijn, controleert de Azure AD Connect op de aanwezigheid van de vereiste KB. Als de KB niet wordt gedetecteerd, wordt een waarschuwing weergegeven nadat de configuratie is voltooid, zoals hieronder wordt weergegeven:

    ![Waarschuwing voor KB op 2012R2 ontbreekt](media/active-directory-aadconnect-federation-management/kbwarning.png)

    Installeer de vereiste op te lossen de configuratie in geval van een ontbrekende KB, [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) en herstel daarna de vertrouwensrelatie met behulp van [AAD herstellen en AD FS-vertrouwensrelatie](#repairthetrust).

> [!NOTE]
> Lees voor meer informatie over alternateID en stappen voor het handmatig configureren [Configuring Alternate Login ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)

## <a name="addadfsserver"></a>Een AD FS-server toevoegen 

> [!NOTE]
> Azure AD Connect vereist voor het toevoegen van een AD FS-server, het PFX-certificaat. Daarom kunt u deze bewerking uitvoeren als u de AD FS-farm met behulp van Azure AD Connect geconfigureerd.

1. Selecteer **een extra federatieserver implementeren**, en klik op **volgende**.

   ![Extra federatieserver](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. Op de **verbinding maken met Azure AD** pagina, Geef uw referenties hoofdbeheerder voor Azure AD en klikt u op **volgende**.

   ![Verbinding maken met Azure AD](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. Geef het domein beheerdersreferenties.

   ![Referenties voor de domeinbeheerder](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. Azure AD Connect wordt u gevraagd om het wachtwoord van het PFX-bestand dat u hebt opgegeven tijdens het configureren van uw nieuwe AD FS-farm met Azure AD Connect. Klik op **wachtwoord invoeren** het wachtwoord opgeven voor het PFX-bestand.

   ![Certificaatwachtwoord](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Geef een SSL-certificaat op](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. Op de **AD FS-Servers** pagina, voert u de servernaam of IP-adres moet worden toegevoegd aan de AD FS-farm.

   ![AD FS-servers](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. Klik op **volgende**, en Ga via de laatste **configureren** pagina. Nadat Azure AD Connect is voltooid op de servers toe te voegen aan de AD FS-farm, krijgt u de optie om te controleren of de connectiviteit.

   ![Klaar om te configureren](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![Installatie voltooid](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## <a name="addwapserver"></a>Een AD FS WAP-server toevoegen 

> [!NOTE]
> Als u wilt een WAP-server toevoegt, wordt in Azure AD Connect het PFX-certificaat vereist. Daarom kunt u deze bewerking alleen uitvoeren als u de AD FS-farm met behulp van Azure AD Connect geconfigureerd.

1. Selecteer **Web Application Proxy implementeren** uit de lijst met beschikbare taken.

   ![Webtoepassingsproxy implementeren](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. Geef de referenties van de globale beheerder van Azure.

   ![Verbinding maken met Azure AD](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. Op de **Geef SSL-certificaat** pagina, geeft u het wachtwoord voor het PFX-bestand dat u hebt opgegeven toen u de AD FS-farm met Azure AD Connect geconfigureerd.
   ![Certificaatwachtwoord](media/active-directory-aadconnect-federation-management/WapServer3.PNG)

    ![Geef een SSL-certificaat op](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. De server moet worden toegevoegd als een WAP-server toevoegen. Omdat de WAP-server kan niet worden toegevoegd aan het domein, vraagt de wizard om beheerdersreferenties op de server die wordt toegevoegd.

   ![Referenties van de beheerserver](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. Op de **vertrouwensrelatie proxyreferenties** beheerdersreferenties voor het configureren van de proxy vertrouwen en toegang tot de primaire server in de AD FS-farm.

   ![Vertrouwde proxyreferenties](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. Op de **klaar om te configureren** pagina de wizard geeft de lijst met acties die worden uitgevoerd.

   ![Klaar om te configureren](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. Klik op **installeren** om de configuratie te voltooien. Nadat de configuratie voltooid is, kunt u de wizard de optie om te controleren of de verbinding met de servers. Klik op **controleren** connectiviteit controleren.

   ![Installatie voltooid](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## <a name="addfeddomain"></a>Een federatieve domein toevoegen 

Het is gemakkelijk om toe te voegen van een domein dat gefedereerd met Azure AD met behulp van Azure AD Connect. Azure AD Connect wordt het domein voor Federatie en wijzigt de claimregels naar aanleiding van de verlener correct wanneer er meerdere domeinen die zijn gefedereerd met Azure AD.

1. Als u wilt een federatieve domein toevoegt, selecteert u de taak **toevoegen van een extra Azure AD-domein**.

   ![Extra Azure AD-domein](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. Geef de referenties van de hoofdbeheerder voor Azure AD op de volgende pagina van de wizard.

   ![Verbinding maken met Azure AD](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. Op de **RAS-referenties** pagina, beheerdersreferenties voor het domein opgeven.

   ![Referenties voor externe toegang](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. Op de volgende pagina biedt de wizard een lijst met Azure AD-domeinen die u kunt uw on-premises directory met federeren. Kies het domein in de lijst.

   ![Azure AD-domein](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    Nadat u ervoor het domein kiest, de wizard kunt u met de juiste informatie over verdere acties die de wizard wordt uitgevoerd en de impact van de configuratie. In sommige gevallen, als u een domein dat nog niet is geverifieerd in Azure AD, selecteert biedt de wizard u informatie over het controleren van het domein. Zie [uw aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory-domains-add-azure-portal.md) voor meer informatie.

5. Klik op **Volgende**. De **klaar om te configureren** pagina bevat de lijst met acties die door Azure AD Connect worden uitgevoerd. Klik op **installeren** om de configuratie te voltooien.

   ![Klaar om te configureren](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> Gebruikers van de toegevoegde federatieve domein moeten worden gesynchroniseerd voordat ze zich aanmelden bij Azure AD.

## <a name="ad-fs-customization"></a>AD FS-aanpassing
De volgende secties geven informatie over een aantal algemene taken die u wellicht moet worden uitgevoerd wanneer u uw AD FS-aanmeldingspagina aanpassen.

## <a name="customlogo"></a>Een aangepast bedrijfslogo of afbeelding toevoegen 
Wijzigen van het logo van het bedrijf dat wordt weergegeven op de **aanmelden** pagina, gebruikt u de volgende Windows PowerShell-cmdlet en syntaxis.

> [!NOTE]
> De aanbevolen dimensies voor het logo wordt 260 x 35 96 dpi-waarde met een bestandsgrootte van niet meer dan 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> De *TargetName* parameter is vereist. De naam van het standaardthema dat wordt geleverd met AD FS is standaard.

## <a name="addsignindescription"></a>Een beschrijving van de aanmeldingspagina toevoegen 
Toevoegen van een beschrijving van de aanmeldingspagina in de **aanmeldingspagina**, gebruik de volgende Windows PowerShell-cmdlet en syntaxis.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <a name="modclaims"></a>Wijzigen van de claimregels van AD FS 
AD FS biedt ondersteuning voor een uitgebreide claim taal die u kunt aangepaste claimregels maken. Zie voor meer informatie [de rol van de Claimregeltaal](https://technet.microsoft.com/library/dd807118.aspx).

De volgende secties wordt beschreven hoe u aangepaste regels voor sommige scenario's met betrekking tot Azure AD kan schrijven en AD FS-federatie.

### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Niet-wijzigbaar voorwaardelijke op een waarde in het kenmerk-ID
Azure AD Connect kunt u opgeven van een kenmerk moet worden gebruikt als een bronanker wanneer objecten worden gesynchroniseerd met Azure AD. Als de waarde in het aangepaste kenmerk niet leeg is, is het raadzaam een onveranderbare ID-claim uitgeven.

Bijvoorbeeld, kunt u selecteren **ms-ds-consistencyguid** als het kenmerk voor het bronanker en probleem **onveranderbare id genoemd** als **ms-ds-consistencyguid** in geval van het kenmerk heeft een waarde op basis van deze. Als er geen waarde op basis van het kenmerk is, geven **objectGuid** als de niet-wijzigbaar-ID. U kunt de reeks aangepaste claimregels zoals beschreven in de volgende sectie opstellen.

**Regel 1: Querykenmerken**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

In deze regel, waaruit u de waarden van gegevens ophaalt **ms-ds-consistencyguid** en **objectGuid** voor de gebruiker uit Active Directory. Wijzig de naam van de store naar de naam van een juiste store in uw AD FS-implementatie. Ook wijzigen, typt u de claims naar het juiste type voor uw federatieserver claims zoals is gedefinieerd voor **objectGuid** en **ms-ds-consistencyguid**.

Ook met behulp van **toevoegen** en niet **probleem**, u te voorkomen dat een uitgaande probleem voor de entiteit toe te voegen en de waarden als tussenliggende waarden kunt gebruiken. Geeft u de claim uit in een latere regel nadat u hebt vastgesteld welke waarde om te gebruiken als de niet-wijzigbaar-ID.

**Regel 2: Controleer of de ds-ms-consistencyguid voor de gebruiker bestaat**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Deze regel definieert een tijdelijke vlag aangeroepen **idflag** die is ingesteld op **useguid** als er geen **ms-ds-consistencyguid** ingevuld voor de gebruiker. De logica achter dit is het feit dat AD FS leeg claims niet toegestaan. Dus als u claims http://contoso.com/ws/2016/02/identity/claims/objectguid en http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid in regel 1 toevoegt, u uiteindelijk met eindigen een **msdsconsistencyguid** claim alleen als de waarde wordt voor de gebruiker gevuld. Als deze niet is ingevuld, wordt AD FS ziet dat het een lege waarde hebben en onmiddellijk verwijderd. Alle objecten hebben **objectGuid**, zodat deze claim wordt altijd er nadat regel 1 wordt uitgevoerd.

**Regel 3: Ms-ds-consistencyguid verlenen als niet-wijzigbaar ID indien aanwezig**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Dit is een impliciete **Exist** controleren. Als de waarde voor de claim bestaat, klikt u vervolgens uitgeeft die als de niet-wijzigbaar-ID. In het vorige voorbeeld wordt de **nameidentifier** claim. U hebt dit wijzigen in het juiste claimtype voor de ID van de niet-wijzigbaar in uw omgeving.

**Regel 4: ObjectGuid verlenen als niet-wijzigbaar-ID als ms-ds-consistencyGuid niet aanwezig is**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

In deze regel, bent u gewoon de tijdelijke vlag controleren **idflag**. U beslissen of u de claim uitgeven op basis van de waarde.

> [!NOTE]
> De volgorde van deze regels is belangrijk.

### <a name="sso-with-a-subdomain-upn"></a>Eenmalige aanmelding met een subdomein UPN
U kunt meer dan één domein dat gefedereerd met behulp van Azure AD Connect, zoals beschreven in toevoegen [toevoegen van een nieuwe federatieve domein](active-directory-aadconnect-federation-management.md#addfeddomain). U moet de UPN (User Principal Name) Gebruikersclaim wijzigen zodat de uitgevers-ID komt met het hoofddomein en niet het subdomein overeen, omdat het federatieve hoofddomein ook de onderliggende vallen.

De claimregel voor uitgevers-ID is standaard ingesteld als:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Standaard uitgever-ID claim](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

De standaardregel gewoon duurt het UPN-achtervoegsel en wordt gebruikt in de claim uitgever-ID. Bijvoorbeeld: John is een gebruiker in sub.contoso.com en contoso.com is gefedereerd met Azure AD. John voert john@sub.contoso.com als de gebruikersnaam tijdens het aanmelden bij Azure AD. De claimregel standaard uitgever-ID in AD FS afhandelt op de volgende manier:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Claim value:**  http://sub.contoso.com/adfs/services/trust/

Als u alleen het hoofddomein in de claimwaarde van de certificaatverlener wilt weergeven, wijzigen de claimregel zodat deze overeenkomt met het volgende:

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [opties aanmelden gebruiker](active-directory-aadconnect-user-signin.md).
