---
title: MFA software development kit voor aangepaste apps | Microsoft Docs
description: In dit artikel leest u hoe om te downloaden en de Azure MFA-SDK gebruiken voor verificatie in twee stappen voor uw aangepaste apps inschakelen.
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 1c152f67-be02-42a5-a0c7-246fb6b34377
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: joflore
ms.openlocfilehash: 7ae89241c67655fbcaa747c4cac224b898947f39
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Gebouw multi-factor Authentication in aangepaste Apps (SDK)

> [!IMPORTANT]
> De afschaffing van de Azure Multi-Factor Authentication Software Development Kit (SDK) is aangekondigd. Deze functie wordt niet langer ondersteund voor nieuwe klanten. Huidige klanten kunnen de SDK nog gebruiken tot en met 14 november 2018. Na deze datum kan de SDK niet meer worden aangeroepen. 

De Azure multi-factor Authentication Software Development Kit (SDK) kunt u de verificatie in twee stappen rechtstreeks in het aanmelden of transactie processen van toepassingen in uw Azure AD-tenant maken.

De multi-factor Authentication SDK is beschikbaar voor C#, Visual Basic (.NET), Java, Perl, PHP en Ruby. De SDK biedt een thin wrapper rond verificatie in twee stappen. Het bevat alles wat die u nodig hebt om uw code, inclusief opmerkingen broncodebestanden, bijvoorbeeld bestanden en een gedetailleerde Leesmij-bestand te schrijven. Elke SDK bevat ook een certificaat en de persoonlijke sleutel voor het versleutelen van transacties die uniek voor uw multi-factor Authentication-Provider zijn. Als u een provider hebt, kunt u de SDK in zoveel talen en indelingen downloaden als u nodig hebt.

De structuur van de API's in de multi-factor Authentication SDK is eenvoudig. Controleer één functie-aanroep om een API met de multi-factor-optie parameters (zoals de verificatie-modus) en de gebruikersgegevens (zoals het telefoonnummer of de pincode valideren). De API's vertalen de functieaanroep in web services aanvragen naar de cloud-gebaseerde Azure multi-factor Authentication-Service. Alle aanroepen moeten een verwijzing naar het persoonlijke certificaat dat is opgenomen in elke SDK bevatten.

Omdat de API's geen toegang tot gebruikers die zijn geregistreerd bij Azure Active Directory, moet u gebruikersgegevens in een bestand of de database opgeven. Ook bieden de API's geen informatie over de beheerfuncties van inschrijving of gebruiker, dus u moet deze processen in uw toepassing bouwen.

> [!IMPORTANT]
> Als u de SDK wilt downloaden, moet u een Azure Multi-Factor Authentication-provider maken, zelfs als u Azure MFA, AAD Premium of EMS-licenties hebt. Als u een Azure multi-factor Authentication-Provider voor dit doel maken en hebt al licenties, controleert u of voor het maken van de Provider met de **Per ingeschakelde gebruiker** model. Koppel de provider vervolgens aan de map die de licenties voor Azure MFA, Azure AD Premium of EMS bevat. Deze configuratie zorgt ervoor dat u wordt alleen gefactureerd als u meer unieke gebruikers met behulp van de SDK dan het aantal licenties dat u bezit hebt.


## <a name="download-the-sdk"></a>De SDK downloaden
De Azure multi-factor Authentication SDK downloaden vereist een [Azure multi-factor Authentication-Provider](multi-factor-authentication-get-started-auth-provider.md).  Hiervoor moet een volledige Azure-abonnement, zelfs als het eigendom van Azure MFA, Azure AD Premium of Enterprise Mobility Suite licenties zijn. De openbare methoden voor het downloaden van de SDK hebt gesteld, omdat de SDK is afgeschaft. Als u wilt downloaden van de SDK, moet u een ondersteuningsaanvraag openen met Microsoft. De SDK wordt geleverd alleen aan klanten die al van de SDK gebruikmaken. Nieuwe klanten worden niet vrijgegeven.

## <a name="whats-in-the-sdk"></a>Wat is er in de SDK
De SDK bevat de volgende items:

* **LEESMIJ**. Legt uit hoe u de multi-factor Authentication-API's gebruiken in een nieuwe of bestaande toepassing.
* **Bronbestanden** voor multi-factor Authentication
* **Clientcertificaat** die u gebruikt om te communiceren met de multi-factor Authentication-service
* **Persoonlijke sleutel** voor het certificaat
* **Aanroepen van resultaten.** Een lijst met oproepresultaatcodes. Om dit bestand openen, een toepassing met de tekst, zoals WordPad te gebruiken. Gebruik de oproepresultaatcodes om te testen en oplossen van de implementatie van multi-factor Authentication in uw toepassing. Ze zijn geen statuscodes voor verificatie.
* **Voorbeelden.** Voorbeeldcode voor een eenvoudige werkende implementatie van multi-factor Authentication.

> [!WARNING]
> Het clientcertificaat is een unieke persoonlijk certificaat die speciaal voor u is gegenereerd. Geen delen of dit bestand verloren. Het is uw sleutel voor de beveiliging van uw communicatie met de multi-factor Authentication-service.

## <a name="code-sample"></a>Codevoorbeeld
Dit voorbeeld ziet u hoe de API's gebruiken in de SDK van Azure multi-factor Authentication standaardmodus stem aanroep verificatie toevoegen aan uw toepassing. Standaardmodus is een telefoongesprek die de gebruiker reageert op door op de toets # te drukken.

Dit voorbeeld gebruikt de C# .NET 2.0 multi-factor Authentication SDK in een eenvoudige ASP.NET-toepassing met C#-serverzijde logica, maar het proces is vergelijkbaar in andere talen. Omdat de SDK bronbestanden, niet-uitvoerbare bestanden bevat, kunt u bouwen van de bestanden en ernaar te verwijzen of deze rechtstreeks in uw toepassing opnemen.

> [!NOTE]
> Bij het implementeren van multi-factor Authentication, de aanvullende methoden gebruiken (telefoongesprek of tekstbericht) als secundair of tertiair verificatie ter aanvulling van de primaire authenticatiemethode (gebruikersnaam en wachtwoord). Deze methoden zijn niet bedoeld als primaire verificatiemethoden.

### <a name="code-sample-overview"></a>Overzicht van de code-voorbeeld
Deze voorbeeldcode voor een eenvoudige demo webtoepassing maakt gebruik van een telefoongesprek met een antwoord van de hostsleutel # om te controleren of verificatie van de gebruiker. Deze factoren telefoongesprek wordt genoemd in de multi-factor Authentication standaardmodus.

De client-side '-code bevat geen een multi-Factor Authentication-specifieke elementen bevat. Omdat de aanvullende verificatiefactoren onafhankelijk van de primaire verificatie, kunt u ze kunt toevoegen zonder dat u de interface voor bestaande wijzigt. De API's in de multi-factor-SDK kunt u de gebruikerservaring aanpassen, maar moet u mogelijk niet op alle belang.

De code op de server wordt standaard verificatiemodus toegevoegd in stap 2. Er wordt een PfAuthParams-object gemaakt met de parameters die vereist voor de verificatie standaardmodus zijn: gebruikersnaam, getal, en de modus en het pad naar het clientcertificaat (CertFilePath), die vereist is in elke aanroep van de telefoon. Zie het voorbeeld van een bestand in de SDK voor een demonstratie van alle parameters in PfAuthParams.

De code wordt vervolgens het PfAuthParams-object doorgegeven aan de pf_authenticate()-functie. De geretourneerde waarde geeft aan het slagen of mislukken van de verificatie. De parameters, callStatus en Aanroepstatus, extra aanroep resultaat informatie bevatten. De oproepresultaatcodes worden beschreven in het bestand met resultaten aanroep in de SDK.

Deze minimale implementatie kan worden geschreven in een paar regels. In productiecode moet zou u bevatten echter meer geavanceerde foutafhandeling, extra databasecode en een betere gebruikerservaring.

### <a name="web-client-code"></a>Web-Client-Code
Hieronder vindt u een web-clientcode voor een demo-pagina.

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
Multi-factor Authentication is geconfigureerd en worden uitgevoerd in stap 2 in de volgende code van de serverzijde. Standaard-modus (MODE_STANDARD) is een telefoongesprek waarop de gebruiker reageert door # te drukken.

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
