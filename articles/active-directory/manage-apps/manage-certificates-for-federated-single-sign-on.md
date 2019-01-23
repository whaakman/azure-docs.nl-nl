---
title: Federatiecertificaten beheren in Azure AD | Microsoft Docs
description: Informatie over het aanpassen van de vervaldatum voor de federatiecertificaten en het vernieuwen van certificaten die binnenkort vervallen.
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: barbkess
ms.reviewer: jeedes
ms.openlocfilehash: 48f001ab0634451b1e339b346e03f5d15cdbf9c6
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54463844"
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Certificaten voor federatieve eenmalige aanmelding in Azure Active Directory beheren
In dit artikel bevat informatie over veelgestelde vragen en informatie met betrekking tot de certificaten die Azure Active Directory (Azure AD) maakt voor het maken van federatieve eenmalige aanmelding (SSO) met uw SaaS-toepassingen. Toepassingen toevoegen vanuit de galerie met Azure AD-app of met behulp van de sjabloon van een toepassing buiten de galerie. De toepassing configureren met behulp van de optie voor federatieve eenmalige aanmelding.

In dit artikel is alleen relevant voor apps die zijn geconfigureerd voor het gebruik van Azure AD-eenmalige aanmelding via de SAML-federation, zoals wordt weergegeven in het volgende voorbeeld:

![Azure AD voor eenmalige aanmelding](./media/manage-certificates-for-federated-single-sign-on/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automatisch gegenereerde certificaat voor de galerie en niet toepassingen
Wanneer u een nieuwe toepassing uit de galerie toevoegen en configureren van een aanmelding op basis van SAML, genereert Azure AD een certificaat voor de toepassing die is geldig gedurende drie jaar. U kunt dit certificaat uit downloaden de **SAML-handtekeningcertificaat** sectie. In deze sectie mogelijk voor galerietoepassingen, een optie voor het downloaden van het certificaat of de metagegevens, afhankelijk van de vereiste van de toepassing worden weergegeven.

![Azure AD eenmalige aanmelding](./media/manage-certificates-for-federated-single-sign-on/saml_certificate_download.png)

## <a name="customize-the-expiration-date-for-your-federation-certificate-and-roll-it-over-to-a-new-certificate"></a>De vervaldatum voor het certificaat van uw federatieve aanpassen en meenemen naar een nieuw certificaat
Certificaten zijn standaard ingesteld op verlopen na drie jaar. U kunt een andere vervaldatum voor uw certificaat door de volgende stappen te voltooien.
De schermafbeeldingen Salesforce om het voorbeeld te gebruiken, maar deze stappen kunnen toepassen op alle federatieve SaaS-Apps.

1. In de [Azure-portal](https://aad.portal.azure.com), klikt u op **bedrijfstoepassing** in het linkerdeelvenster en klik vervolgens op **nieuwe toepassing** op de **overzicht** pagina:

   ![Open de wizard Eenmalige aanmelding configureren](./media/manage-certificates-for-federated-single-sign-on/enterprise_application_new_application.png)

2. Zoeken naar de toepassing in de galerie en selecteer vervolgens de toepassing die u wilt toevoegen. Als u de vereiste toepassing niet vinden, de toepassing toevoegen met behulp van de **niet in de galerij toepassing** optie. Deze functie is alleen beschikbaar in de Azure AD Premium (P1 en P2)-SKU.

    ![Azure AD eenmalige aanmelding](./media/manage-certificates-for-federated-single-sign-on/add_gallery_application.png)

3. Klik op de **eenmalige aanmelding** koppeling in het linkerdeelvenster en wijzigt **modus voor één aanmelding** naar **SAML gebaseerde aanmelding**. Deze stap genereert een driejarig-certificaat voor uw toepassing.

4. Als u wilt een nieuw certificaat maken, klikt u op de **nieuw certificaat maken** herstelkoppeling in de **SAML-handtekeningcertificaat** sectie.

    ![Een nieuw certificaat genereren](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

5. De **Maak een nieuw certificaat** koppeling opent u het besturingselement van de agenda. U kunt een datum en tijd van maximaal drie jaar na de huidige datum. De geselecteerde datum en tijd is de nieuwe datum en tijd van het nieuwe certificaat. Klik op **Opslaan**.

    ![Download en upload het certificaat](./media/manage-certificates-for-federated-single-sign-on/certifcate_date_selection.PNG)

6. Het nieuwe certificaat is nu beschikbaar voor downloaden. Klik op de **certificaat** koppeling om deze te downloaden. Uw certificaat is op dit moment niet actief. Wanneer u meenemen naar dit certificaat wilt, selecteert u de **nieuw certificaat activeren** selectievakje in en klikt u op **opslaan**. Vanaf dat moment begint Azure AD met behulp van het nieuwe certificaat voor ondertekening van het antwoord.

7.  Voor meer informatie over het uploaden van het certificaat naar uw bepaalde SaaS-toepassing, klikt u op de **zelfstudie over configuratie weergave** koppeling.

## <a name="certificate-expiration-notification-email"></a>E-mail met melding voor certificaat verlopen

Azure AD stuurt een e-mailbericht melding 60, 30 en 7 dagen voordat het SAML-certificaat is verlopen. Om op te geven het e-mailadres voor de locatie waar u de melding wordt verzonden:

- Op de Azure Active Directory-toepassing eenmalige aanmelding pagina, gaat u naar het veld e-mailmelding.
- Voer het e-mailadres dat de e-mailmelding verlopen certificaat moet ontvangen. Dit veld gebruikt standaard de e-mailadres van de beheerder die de toepassing toegevoegd.

U ontvangt de e-mailmelding van aadnotification@microsoft.com. Zorg dat u dit e-mailbericht aan uw contactpersonen toevoegen om te voorkomen dat het e-mailbericht naar uw locatie spam. 

## <a name="renew-a-certificate-that-will-soon-expire"></a>Een certificaat verlengen dat binnenkort vervallen
De volgende stappen uit voor vernieuwen moeten leiden tot aanzienlijke downtime voor uw gebruikers. De schermafbeeldingen in deze sectie functie Salesforce als een voorbeeld, maar deze stappen kunt toepassen op alle federatieve SaaS-Apps.

1. Op de **Azure Active Directory** toepassing **eenmalige aanmelding** pagina, het genereren van het nieuwe certificaat voor uw toepassing. U kunt dit doen door te klikken op de **nieuw certificaat maken** herstelkoppeling in de **SAML-handtekeningcertificaat** sectie.

    ![Een nieuw certificaat genereren](./media/manage-certificates-for-federated-single-sign-on/create_new_certficate.png)

2. Selecteer de gewenste vervaldatum en -tijd voor het nieuwe certificaat en klik op **opslaan**. Een datum die met het bestaande certificaat overlapt te selecteren, zorgt u ervoor dat eventuele uitvaltijd vanwege het verlopen certificaat beperkt is. 

3. Als de app kan automatisch rollover van een certificaat, stelt u het nieuwe certificaat te activeren.  Meld u aan de app om te controleren of deze werkt.

4. Als de app niet automatisch het nieuwe certificaat ophalen, maar kunt verwerken van meer dan één certificaat ondertekenen, voordat de oude versie is verlopen, upload het nieuwe knooppunt naar de app, en vervolgens gaat u terug naar de portal en het actieve certificaat. 

5. Als de app slechts één certificaat tegelijk verwerken kan, kies een venster downtime, download het nieuwe certificaat te uploaden naar de toepassing, keert u terug naar de Azure-Portal en het nieuwe certificaat instellen als actief. 
   
6. Activeer het nieuwe certificaat in Azure AD, selecteer de **nieuw certificaat activeren** selectievakje in en klik op de **opslaan** knop aan de bovenkant van de pagina. Dit wordt getotaliseerd via het nieuwe certificaat aan de Azure AD. De status van het certificaat wordt gewijzigd van **nieuw** naar **Active**. Vanaf dat moment begint Azure AD met behulp van het nieuwe certificaat voor ondertekening van het antwoord. 
   
    ![Een nieuw certificaat genereren](./media/manage-certificates-for-federated-single-sign-on/new_certificate_download.png)

## <a name="related-articles"></a>Verwante artikelen:
* [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](../saas-apps/tutorial-list.md)
* [Application Management in Azure Active Directory](what-is-application-management.md) (Toepassingsbeheer in Azure Active Directory)
* [Toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](what-is-single-sign-on.md)
* [Oplossen van problemen met SAML gebaseerde eenmalige aanmelding](../develop/howto-v1-debug-saml-sso-issues.md)
