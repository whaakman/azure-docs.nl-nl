---
title: Azure AD Connect - AD FS-beheer en aanpassingen | Microsoft Docs
description: AD FS-beheer met Azure AD Connect en aanpassingen van AD FS-aanmelden gebruikerservaring met Azure AD Connect en PowerShell.
keywords: AD FS, ADFS, AD FS-beheer, AAD Connect, verbinding maken, zich aanmelden, AD FS aanpassing, trust, O365, Federatie, relying party herstellen
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: a9a7848069300d5f52d16585a55313643e02bc72
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244454"
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a>Beheren en aanpassen van Active Directory Federation Services met behulp van Azure AD Connect
Dit artikel wordt beschreven hoe u kunt beheren en aanpassen van Active Directory Federation Services (AD FS) met behulp van Azure Active Directory (Azure AD) Connect. Dit omvat ook andere algemene AD FS-taken die u moet uitvoeren voor een volledige configuratie van AD FS-farm.

| Onderwerp | Er wordt aangegeven |
|:--- |:--- |
| **AD FS beheren** | |
| [Herstel de vertrouwensrelatie](#repairthetrust) |Klik hier voor meer informatie over het herstellen van de federatieve vertrouwensrelatie met Office 365. |
| [Federeren met Azure AD met behulp van alternatieve aanmeldings-ID ](#alternateid) | Federatie met behulp van alternatieve aanmeldings-ID configureren  |
| [Een AD FS-server toevoegen](#addadfsserver) |Over het uitbreiden van een AD FS-farm met een aanvullende AD FS-server. |
| [Een AD FS Web Application Proxy-server toevoegen](#addwapserver) |Over het uitbreiden van een AD FS-farm met een extra Webtoepassingsproxy (WAP)-server. |
| [Een federatief domein toevoegen](#addfeddomain) |Het toevoegen van een federatief domein. |
| [Het SSL-certificaat bijwerken](how-to-connect-fed-ssl-update.md)| Het bijwerken van het SSL-certificaat voor AD FS-farm. |
| **Aanpassen van AD FS** | |
| [Een aangepast bedrijfslogo of een afbeelding toevoegen](#customlogo) |Over het aanpassen van een AD FS-aanmeldingspagina met een bedrijfslogo en afbeelding. |
| [Een beschrijving van de aanmelding toevoegen](#addsignindescription) |Hoe u een beschrijving van de aanmeldingspagina toevoegen. |
| [AD FS claimregels wijzigen](#modclaims) |Klik hier voor meer informatie over het wijzigen van de AD FS-claims voor verschillende scenario's voor Federatie. |

## <a name="manage-ad-fs"></a>AD FS beheren
U kunt verschillende AD FS-gerelateerde taken uitvoeren in Azure AD Connect met minimale tussenkomst met behulp van de Azure AD Connect-wizard. Wanneer u klaar bent met het Azure AD Connect installeert met behulp van de wizard, kunt u de wizard opnieuw uitvoeren van extra taken kunt uitvoeren.

## <a name="repairthetrust"></a>Herstel de vertrouwensrelatie 
U kunt Azure AD Connect gebruiken om te controleren of de huidige status van de AD FS en Azure AD vertrouwen en gepaste actie ondernemen om te herstellen van de vertrouwensrelatie. Volg deze stappen voor het herstellen van uw Azure AD en AD FS-vertrouwensrelatie.

1. Selecteer **herstellen AAD en AD FS vertrouwen** uit de lijst met aanvullende taken.
   ![Herstellen van AAD en AD FS vertrouwen](./media/how-to-connect-fed-management/RepairADTrust1.PNG)

2. Op de **verbinding maken met Azure AD** pagina, Geef uw referenties van de globale beheerder voor Azure AD en klikt u op **volgende**.
   ![Verbinding maken met Azure AD](./media/how-to-connect-fed-management/RepairADTrust2.PNG)

3. Op de **referenties voor externe toegang** pagina, typ de referenties voor de domeinbeheerder.

   ![Referenties voor externe toegang](./media/how-to-connect-fed-management/RepairADTrust3.PNG)

    Nadat u op **volgende**, Azure AD Connect controleert de certificaatstatus van het en laat zien of er problemen zijn.

    ![Status van certificaten](./media/how-to-connect-fed-management/RepairADTrust4.PNG)

    De **klaar om te configureren** pagina bevat een lijst van acties die zullen worden uitgevoerd om te herstellen van de vertrouwensrelatie.

    ![Klaar om te configureren](./media/how-to-connect-fed-management/RepairADTrust5.PNG)

4. Klik op **installeren** te herstellen van de vertrouwensrelatie.

> [!NOTE]
> Azure AD Connect kan alleen herstellen of act op certificaten die zelf-ondertekend zijn. Azure AD Connect kan certificaten van derden niet herstellen.

## <a name="alternateid"></a>Federeren met Azure AD met behulp van AlternateID 
Het wordt aanbevolen dat de on-premises gebruiker-Principal Name(UPN) en de User Principal Name van de cloud hetzelfde blijven. Als de lokale UPN gebruikmaakt van een niet-routeerbare domein (ex.) Contoso.local) of kan niet worden gewijzigd vanwege lokale toepassingsafhankelijkheden, is het raadzaam instellen van alternatieve aanmeldings-ID. Alternatieve aanmeldings-ID kunt u een aanmelding configureren waar gebruikers zich kunnen aanmelden met een kenmerk dan hun UPN, zoals e-mail. De keuze voor de User Principal Name in de standaardwaarden van de Azure AD Connect op het kenmerk userPrincipalName in Active Directory. Als u een ander kenmerk voor de User Principal Name kiezen en zijn federeren met AD FS, klikt u vervolgens Azure AD Connect AD FS configureert voor alternatieve aanmeldings-ID. Hieronder ziet u een voorbeeld van het kiezen van een ander kenmerk voor de User Principal Name:

![Alternatieve id-kenmerk selecteren](./media/how-to-connect-fed-management/attributeselection.png)

Configureren van alternatieve aanmeldings-ID voor AD FS bestaat uit twee belangrijke stappen:
1. **Configureren van de juiste set uitgifte claims**: de claimregels voor uitgifte in de Azure AD relying party vertrouwt voor het gebruik van het geselecteerde kenmerk UserPrincipalName als de alternatieve ID van de gebruiker worden gewijzigd.
2. **Alternatieve aanmeldings-ID in de AD FS-configuratie inschakelen**: de AD FS-configuratie is bijgewerkt, zodat de AD FS kunt opzoeken van de gebruikers in de juiste forests met behulp van de alternatieve-ID. Deze configuratie wordt ondersteund voor AD FS in Windows Server 2012 R2 (met KB2919355) of hoger. Als de AD FS-servers 2012 R2 zijn, wordt Azure AD Connect controleert op de aanwezigheid van de vereiste KB. Als de KB niet wordt gedetecteerd, wordt een waarschuwing weergegeven nadat de configuratie is voltooid, zoals hieronder wordt weergegeven:

    ![Waarschuwing voor ontbrekende KB op 2012R2](./media/how-to-connect-fed-management/kbwarning.png)

    Als u wilt herstellen van de configuratie in het geval van ontbrekende KB, installeert de vereiste [KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590) en herstel daarna de vertrouwensrelatie met behulp van [herstellen AAD en AD FS-vertrouwensrelatie](#repairthetrust).

> [!NOTE]
> Lees voor meer informatie over alternateID en stappen voor het handmatig configureren [Configuring Alternate Login ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)

## <a name="addadfsserver"></a>Een AD FS-server toevoegen 

> [!NOTE]
> Als u wilt toevoegen van een AD FS-server, vereist Azure AD Connect het PFX-certificaat. Daarom kunt u deze bewerking uitvoeren alleen als u de AD FS-farm met behulp van Azure AD Connect hebt geconfigureerd.

1. Selecteer **een extra Federation-Server implementeren**, en klikt u op **volgende**.

   ![Aanvullende federation-server](./media/how-to-connect-fed-management/AddNewADFSServer1.PNG)

2. Op de **verbinding maken met Azure AD** pagina, Voer uw globale beheerdersreferenties voor Azure AD en klikt u op **volgende**.

   ![Verbinding maken met Azure AD](./media/how-to-connect-fed-management/AddNewADFSServer2.PNG)

3. Het domein administrator-referenties opgeven.

   ![Referenties voor de domeinbeheerder](./media/how-to-connect-fed-management/AddNewADFSServer3.PNG)

4. Azure AD Connect wordt gevraagd om het wachtwoord van het PFX-bestand dat u hebt opgegeven tijdens het configureren van uw nieuwe AD FS-farm met Azure AD Connect. Klik op **wachtwoord invoeren** voor het wachtwoord voor het PFX-bestand.

   ![Certificaatwachtwoord](./media/how-to-connect-fed-management/AddNewADFSServer4.PNG)

    ![Geef een SSL-certificaat op](./media/how-to-connect-fed-management/AddNewADFSServer5.PNG)

5. Op de **AD FS-Servers** pagina, typ de servernaam of IP-adres moet worden toegevoegd aan de AD FS-farm.

   ![AD FS-servers](./media/how-to-connect-fed-management/AddNewADFSServer6.PNG)

6. Klik op **volgende**, en Ga door middel van de laatste **configureren** pagina. Nadat Azure AD Connect is voltooid op de servers toe te voegen aan de AD FS-farm, krijgt u de optie om te controleren of de verbinding.

   ![Klaar om te configureren](./media/how-to-connect-fed-management/AddNewADFSServer7.PNG)

    ![De installatie is voltooid](./media/how-to-connect-fed-management/AddNewADFSServer8.PNG)

## <a name="addwapserver"></a>Een AD FS WAP-server toevoegen 

> [!NOTE]
> Als u wilt een WAP-server toevoegt, wordt in Azure AD Connect het PFX-certificaat vereist. Daarom kunt u deze bewerking alleen uitvoeren als u de AD FS-farm met behulp van Azure AD Connect hebt geconfigureerd.

1. Selecteer **implementeren Web Application Proxy** uit de lijst met beschikbare taken.

   ![Web Application Proxy implementeren](./media/how-to-connect-fed-management/WapServer1.PNG)

2. Geef de referenties van de globale beheerder van Azure.

   ![Verbinding maken met Azure AD](./media/how-to-connect-fed-management/wapserver2.PNG)

3. Op de **opgeven SSL-certificaat** pagina, geeft u het wachtwoord voor het PFX-bestand dat u hebt opgegeven tijdens het configureren van de AD FS-farm met Azure AD Connect.
   ![Certificaatwachtwoord](./media/how-to-connect-fed-management/WapServer3.PNG)

    ![Geef een SSL-certificaat op](./media/how-to-connect-fed-management/WapServer4.PNG)

4. De server moet worden toegevoegd als een WAP-server toevoegen. Omdat de WAP-server kan niet worden toegevoegd aan het domein, wordt de wizard gevraagd om administratieve referenties voor de server die wordt toegevoegd.

   ![Referenties van de beheerserver](./media/how-to-connect-fed-management/WapServer5.PNG)

5. Op de **vertrouwde proxyreferenties** pagina, geeft u beheerdersreferenties voor het configureren van de proxy vertrouwen en toegang tot de primaire server in de AD FS-farm.

   ![Vertrouwde proxyreferenties](./media/how-to-connect-fed-management/WapServer6.PNG)

6. Op de **klaar om te configureren** pagina, de wizard bevat de lijst met acties die worden uitgevoerd.

   ![Klaar om te configureren](./media/how-to-connect-fed-management/WapServer7.PNG)

7. Klik op **installeren** om de configuratie te voltooien. Nadat de configuratie voltooid is, geeft u de wizard de optie om te controleren of de verbinding met de servers. Klik op **controleren** om te controleren op connectiviteit.

   ![De installatie is voltooid](./media/how-to-connect-fed-management/WapServer8.PNG)

## <a name="addfeddomain"></a>Een federatief domein toevoegen 

Het is gemakkelijk om toe te voegen van een domein dat gefedereerd met Azure AD met behulp van Azure AD Connect. Azure AD Connect wordt het domein voor Federatie toegevoegd en wijzigt de claimregels overeenkomen met de uitgever als er meerdere domeinen gefedereerd met Azure AD.

1. Als u wilt een federatief domein toevoegen, selecteert u de taak **toevoegen van een extra Azure AD-domein**.

   ![Extra Azure AD-domein](./media/how-to-connect-fed-management/AdditionalDomain1.PNG)

2. Op de volgende pagina van de wizard referenties van de globale beheerder voor Azure AD.

   ![Verbinding maken met Azure AD](./media/how-to-connect-fed-management/AdditionalDomain2.PNG)

3. Op de **referenties voor externe toegang** pagina, het domein administrator-referenties opgeven.

   ![Referenties voor externe toegang](./media/how-to-connect-fed-management/additionaldomain3.PNG)

4. Op de volgende pagina bevat de wizard een lijst van Azure AD-domeinen die u kunt uw on-premises directory met federeren. Kies het domein in de lijst.

   ![Azure AD-domein](./media/how-to-connect-fed-management/AdditionalDomain4.PNG)

    Nadat u het domein hebt gekozen, de wizard kunt u met de juiste informatie over verdere acties die de wizard gaat ondernemen en het effect van de configuratie. In sommige gevallen, als u een domein dat nog niet is geverifieerd in Azure AD, selecteert biedt de wizard u informatie om te controleren of het domein. Zie [uw aangepaste domeinnaam toevoegen aan Azure Active Directory](../active-directory-domains-add-azure-portal.md) voor meer informatie.

5. Klik op **Volgende**. De **klaar om te configureren** pagina bevat een lijst van acties die door Azure AD Connect wordt uitgevoerd. Klik op **installeren** om de configuratie te voltooien.

   ![Klaar om te configureren](./media/how-to-connect-fed-management/AdditionalDomain5.PNG)

> [!NOTE]
> Gebruikers van de toegevoegde federatief domein moeten worden gesynchroniseerd voordat ze zich aanmelden bij Azure AD.

## <a name="ad-fs-customization"></a>AD FS-aanpassing
De volgende secties vindt u informatie over een aantal van de algemene taken die u uitvoeren moet mogelijk wanneer u uw AD FS-aanmeldingspagina aanpassen.

## <a name="customlogo"></a>Een aangepast bedrijfslogo of een afbeelding toevoegen 
Wijzigen van het logo van het bedrijf dat wordt weergegeven op de **aanmelden** pagina, gebruikt u de volgende Windows PowerShell-cmdlet en syntaxis.

> [!NOTE]
> De aanbevolen afmetingen van het logo wordt 260 x 35 \@ 96 dpi-waarde met een grootte niet langer zijn dan 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> De *TargetName* parameter is vereist. De naam van het standaardthema dat wordt geleverd met AD FS is standaard.

## <a name="addsignindescription"></a>Een beschrijving van de aanmelding toevoegen 
Om toe te voegen, een beschrijving van de aanmeldingspagina in de **aanmeldingspagina**, gebruik de volgende Windows PowerShell-cmdlet en syntaxis.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <a name="modclaims"></a>AD FS claimregels wijzigen 
AD FS biedt ondersteuning voor een uitgebreide claim taal die u gebruiken kunt om aangepaste claimregels te maken. Zie voor meer informatie, [de rol van de Claim Regeltaal](https://technet.microsoft.com/library/dd807118.aspx).

De volgende secties wordt beschreven hoe u aangepaste regels voor enkele scenario's die gerelateerd aan Azure AD zijn kunt schrijven en AD FS-federatie.

### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Onveranderbare ID voorwaardelijke op een waarde aanwezig is in het kenmerk
Azure AD Connect kunt u opgeven van een kenmerk moet worden gebruikt als een bronanker wanneer objecten worden gesynchroniseerd met Azure AD. Als de waarde in het aangepaste kenmerk niet leeg zijn is, is het raadzaam een onveranderbare ID-claim uitgeven.

Bijvoorbeeld, kunt u **ms-ds-consistencyguid** als het kenmerk voor het bronanker en probleem **ImmutableID** als **ms-ds-consistencyguid** in geval van het kenmerk heeft een waarde op basis van deze. Als er geen waarde op basis van het kenmerk is, geven **objectGuid** als de onveranderbare ID. U kunt de set met aangepaste claimregels zoals beschreven in de volgende sectie maken.

**Regel 1: Querykenmerken**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

In deze regel u query's de waarden van **ms-ds-consistencyguid** en **objectGuid** voor de gebruiker uit Active Directory. Wijzig de naam van de store naar de naam van een juiste store in uw AD FS-implementatie. Ook wijzigen, typt u de claims naar het juiste type voor uw federatieserver claims zoals gedefinieerd voor **objectGuid** en **ms-ds-consistencyguid**.

Ook met behulp van **toevoegen** en niet **probleem**, u voorkomen dat een uitgaande probleem voor de entiteit toe te voegen en de waarden kunt gebruiken als tussenliggende waarden. U de claim in een latere regel uitgeeft nadat u instellen welke waarde om te gebruiken als de onveranderbare ID.

**Regel 2: Controleer of ms-ds-consistencyguid voor de gebruiker bestaat**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Deze regel wordt gedefinieerd een tijdelijke vlag met de naam **idflag** die is ingesteld op **useguid** als er geen **ms-ds-consistencyguid** ingevuld voor de gebruiker. De logica achter dit is het feit dat AD FS leeg claims niet toegestaan. Dit het geval is bij het toevoegen van claims http://contoso.com/ws/2016/02/identity/claims/objectguid en http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid in regel 1, krijgt u uiteindelijk met een **msdsconsistencyguid** claim alleen als de waarde voor de gebruiker is ingevuld. Als dit niet is ingevuld, wordt AD FS ziet dat het een lege waarde heeft en onmiddellijk zakt. Alle objecten hebben **objectGuid**, zodat deze claim wordt altijd er nadat de regel 1 wordt uitgevoerd.

**Regel 3: Ms-ds-consistencyguid uitgeven als onveranderbare ID indien aanwezig**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Dit is een impliciete **Exist** controleren. Als de waarde voor de claim bestaat, klikt u vervolgens probleem dat als de onveranderbare ID. Het vorige voorbeeld wordt de **nameidentifier** claim. U hebt dit aan de juiste claimtype wijzigen voor de onveranderbare ID in uw omgeving.

**Regel 4: ObjectGuid uitgeven als onveranderbare ID als ms-ds-consistencyGuid niet aanwezig is**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

In deze regel, bent u gewoon de tijdelijke vlag controleren **idflag**. U beslissen of geeft u de claim op basis van de waarde ervan.

> [!NOTE]
> De volgorde van deze regels is belangrijk.

### <a name="sso-with-a-subdomain-upn"></a>Eenmalige aanmelding met een subdomein UPN

U kunt meer dan één domein dat gefedereerd met behulp van Azure AD Connect, zoals beschreven in toevoegen [toevoegen van een nieuwe federatief domein](how-to-connect-fed-management.md#addfeddomain). Azure AD Connect versie 1.1.553.0 en de meest recente wordt u automatisch de juiste claimregel voor issuerID gemaakt. Als u Azure AD Connect versie 1.1.553.0 niet gebruiken of als laatste, wordt u aangeraden [Claimregels voor Azure AD Rapportkoptekst](https://aka.ms/aadrptclaimrules) hulpprogramma wordt gebruikt voor het genereren en de juiste claimregels voor de Azure AD-vertrouwensrelatie instellen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [aanmeldingsopties voor gebruiker](plan-connect-user-signin.md).
