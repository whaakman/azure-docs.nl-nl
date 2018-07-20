---
title: Azure MFA software development kit voor aangepaste apps
description: In dit artikel wordt beschreven hoe u kunt downloaden en de Azure MFA-SDK gebruiken om in te schakelen van verificatie in twee stappen voor uw aangepaste apps.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 6b82ba53e7a469b01d77865831c2f5fb37f71044
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160838"
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Building multi-factor Authentication in aangepaste Apps (SDK)

> [!IMPORTANT]
> De afschaffing van de Azure Multi-Factor Authentication Software Development Kit (SDK) is aangekondigd. Deze functie wordt niet meer worden ondersteund voor nieuwe klanten. Huidige klanten kunnen de SDK nog gebruiken tot en met 14 november 2018. Na deze datum kan de SDK niet meer worden aangeroepen. 

De Azure multi-factor Authentication Software Development Kit (SDK) kunt u verificatie in twee rechtstreeks in de aanmelding of transactie processen van toepassingen in uw Azure AD-tenant maken.

De SDK voor multi-factor Authentication is beschikbaar voor C#, Visual Basic (.NET), Java, Perl, PHP en Ruby. De SDK biedt een thin wrapper rond verificatie in twee stappen. Dit omvat alles wat die u nodig hebt om uw code, inclusief opmerkingen broncodebestanden, van de voorbeeldbestanden en een gedetailleerde Leesmij-bestand te schrijven. Elke SDK bevat ook een certificaat en de persoonlijke sleutel voor het versleutelen van transacties die uniek voor uw multi-factor Authentication-Provider zijn. Als u een provider hebt, kunt u de SDK in zo veel talen en -indelingen kunt downloaden als u nodig hebt.

De structuur van de API's in de SDK voor multi-factor Authentication is eenvoudig. Controleer één functie aanroepen van een API met de optie multi-factor-parameters (zoals controle-modus) en de gebruikersgegevens (zoals het telefoonnummer of de pincode valideren). De API's vertalen de functieaanroep naar web services-aanvragen naar de cloud gebaseerde Azure multi-factor Authentication-Service. Alle aanroepen, moeten een verwijzing naar het persoonlijke certificaat dat is opgenomen in elke SDK bevatten.

Omdat de API's geen toegang tot gebruikers die zijn geregistreerd bij Azure Active Directory hebt, kunt u gegevens van de gebruiker in een bestand of de database moet opgeven. Ook bieden de API's geen functies voor inschrijving of gebruiker, dus moet u deze processen inbouwen in uw toepassing.

> [!IMPORTANT]
> Als u de SDK wilt downloaden, moet u een Azure Multi-Factor Authentication-provider maken, zelfs als u Azure MFA, AAD Premium of EMS-licenties hebt. Als u Azure multi-factor Authentication-Provider voor dit doel maken en al licenties hebt, zorg ervoor dat u de Provider maakt met de **Per ingeschakelde gebruiker** model. Koppel de provider vervolgens aan de map die de licenties voor Azure MFA, Azure AD Premium of EMS bevat. Deze configuratie zorgt ervoor dat u alleen een rekening ontvangt als u meer unieke gebruikers met de SDK gebruiken dan het aantal licenties dat u bezit hebt.


## <a name="download-the-sdk"></a>De SDK downloaden
Downloaden van de SDK van Azure multi-factor Authentication vereist een [Azure multi-factor Authentication-Provider](concept-mfa-authprovider.md).  Hiervoor moet een volledige Azure-abonnement, zelfs als het eigendom van Azure MFA, Azure AD Premium of Enterprise Mobility Suite-licenties zijn. De openbare methoden van het downloaden van de SDK hebt genomen, omdat de SDK is afgeschaft. Als u nodig hebt voor het downloaden van de SDK, moet u een ondersteuningsaanvraag openen met Microsoft. De SDK is geleverd alleen aan klanten die al van de SDK gebruikmaken. Nieuwe klanten, niet worden toegevoegd.

## <a name="whats-in-the-sdk"></a>Wat is er in de SDK
De SDK bevat de volgende items:

* **LEESMIJ-BESTAND**. Wordt uitgelegd hoe u de multi-factor Authentication-API's in een nieuwe of bestaande toepassing.
* **De bronbestanden** voor meervoudige verificatie
* **Clientcertificaat** die u gebruikt om te communiceren met de multi-factor Authentication-service
* **Persoonlijke sleutel** voor het certificaat
* **Resultaten aanroepen.** Een lijst met oproepresultaatcodes. Als u wilt dit bestand opent, een toepassing met tekst, zoals WordPad te gebruiken. Gebruik de oproepresultaatcodes voor testen en probleemoplossing van de implementatie van multi-factor Authentication in uw toepassing. Ze zijn niet statuscodes voor verificatie.
* **Voorbeelden.** Voorbeeldcode voor een eenvoudige werkende implementatie van multi-factor Authentication.

> [!WARNING]
> Het clientcertificaat is een unieke persoonlijke certificaat dat is gegenereerd met name voor u. Geen delen of dit bestand verloren gaan. Het is uw sleutel voor de beveiliging van uw communicatie met de multi-factor Authentication-service.

## <a name="code-sample"></a>Codevoorbeeld
Dit codevoorbeeld ziet u hoe u de API's in de Azure multi-factor Authentication SDK standaardmodus aanroep stemverificatie toevoegen aan uw toepassing. Standaard-modus is een telefoongesprek die de gebruiker door de toets # te drukken op reageert.

In dit voorbeeld maakt gebruik van de C# .NET 2.0 SDK multi-factor Authentication in een eenvoudige ASP.NET-toepassing met C#-serverzijde logica, maar het proces is vergelijkbaar in andere talen. Omdat de SDK bronbestanden, niet-uitvoerbare bestanden bevat, kunt u bouwen van de bestanden en ernaar te verwijzen of deze rechtstreeks in uw toepassing opnemen.

> [!NOTE]
> Bij het implementeren van multi-factor Authentication, gebruikt u de aanvullende methoden (telefonische oproep of SMS-bericht) als secundair of tertiair verificatie om te vormen een aanvulling op de primaire verificatiemethode (gebruikersnaam en wachtwoord). Deze methoden zijn niet bedoeld als primaire verificatiemethoden.

### <a name="code-sample-overview"></a>Overzicht van de voorbeeld-code
Deze voorbeeldcode voor een eenvoudige web-demo-toepassing maakt gebruik van een telefoongesprek met een # antwoord om te controleren of verificatie van de gebruiker. Deze factoren telefoongesprek wordt genoemd in multi-factor Authentication standaardmodus.

De client-side-code bevat geen elke multi-factor Authentication-specifieke elementen bevat. Omdat de aanvullende verificatiefactoren onafhankelijk van de primaire verificatie zijn, kunt u ze kunt toevoegen zonder te hoeven wijzigen van de interface voor bestaande. De API's in de multi-factor Authentication-SDK kunt u de gebruikerservaring aanpassen, maar mogelijk moet u niet alles helemaal te wijzigen.

De code op de server wordt verificatie in de standard-modus toegevoegd in stap 2. Een PfAuthParams-object wordt gemaakt met de parameters die vereist voor verificatie van de standard-modus zijn: gebruikersnaam, getal, en -modus en het pad naar het clientcertificaat (CertFilePath), die vereist is in elke aanroep van de telefoon. Zie het voorbeeld van een bestand in de SDK voor een demonstratie van alle parameters in PfAuthParams.

De code wordt vervolgens het PfAuthParams-object doorgegeven aan de functie pf_authenticate(). De geretourneerde waarde geeft aan dat het slagen of mislukken van de verificatie. De parameters, callStatus en Aanroepstatus, extra aanroep resultaat informatie bevatten. De oproepresultaatcodes worden beschreven in het bestand met resultaten aanroep in de SDK.

Deze minimale implementatie kan worden geschreven in een paar regels. Bij de productiecode zou u bevatten echter meer geavanceerde foutafhandeling, extra databasecode en een verbeterde gebruikerservaring.

### <a name="web-client-code"></a>Web-clientcode
Hier volgt een web-clientcode voor een demo-pagina.

    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="\_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Code op de server
Multi-factor Authentication is geconfigureerd en uitvoeren in stap 2 in de volgende code van de serverzijde. Standaard-modus (MODE_STANDARD) is een telefoongesprek waarop de gebruiker reageert door op de toets # te drukken.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class \_Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "5555555555";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = "Multi-Factor Authentication failed.";
                }
            }

        }
    }
