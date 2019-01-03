---
title: Wederzijdse TLS-verificatie - Azure App Service configureren
description: Informatie over het configureren van uw app voor het gebruik van verificatie van clientcertificaten op TLS.
services: app-service
documentationcenter: ''
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.custom: seodec18
ms.openlocfilehash: d441329bc3f279e95b2ee302db53d78f786c3470
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53650394"
---
# <a name="how-to-configure-tls-mutual-authentication-for-azure-app-service"></a>Wederzijdse TLS-verificatie voor Azure App Service configureren
## <a name="overview"></a>Overzicht
U kunt toegang tot uw Azure App Service-app beperken door in te schakelen van verschillende soorten verificatie voor deze. Een manier om dit te doen is om te verifiëren met behulp van een clientcertificaat als de aanvraag via TLS/SSL is. Dit mechanisme wordt TLS wederzijdse verificatie of verificatie en in dit artikel wordt beschreven hoe u uw app ingesteld voor het gebruik van verificatie van clientcertificaten clientcertificaat genoemd.

> **Opmerking:** Als u toegang uw site via HTTP en niet HTTPS tot, ontvangt u geen een clientcertificaat. Dus als uw toepassing clientcertificaten vereist moet u niet toestaan aanvragen voor uw toepassing via HTTP.
> 
> 

## <a name="configure-app-service-for-client-certificate-authentication"></a>App Service configureren voor verificatie van clientcertificaten
Als u uw app instelt om clientcertificaten te vereisen, moet u de instelling van de site clientCertEnabled voor uw app toevoegen en stel deze in op true. Deze instelling kan ook worden geconfigureerd in Azure portal onder de blade van de SSL-certificaten.

U kunt de [ARMClient hulpprogramma](https://github.com/projectkudu/ARMClient) om eenvoudig de REST-API-aanroep ze moesten maken. Nadat u zich met het hulpprogramma aanmeldt, moet u de volgende opdracht:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

vervangen van alles in {} met informatie voor uw app en het maken van een bestand met de naam enableclientcert.json met de volgende JSON-inhoud:

    {
        "location": "My App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Zorg ervoor dat u de waarde van "locatie" wijzigen waar uw app bevindt zich bijvoorbeeld Noord-centraal VS of Westelijke VS enzovoort.

U kunt ook https://resources.azure.com aan of de `clientCertEnabled` eigenschap `true`.

> **Opmerking:** Als u ARMClient vanuit Powershell uitvoert, moet u als escape voor de \@ symbool voor het JSON-bestand met een vorige tick '.
> 
> 

## <a name="accessing-the-client-certificate-from-app-service"></a>Toegang tot het certificaat vanuit App Service
Als u van ASP.NET gebruikmaakt en uw app configureren voor het gebruik van verificatie van clientcertificaten, het certificaat zijn beschikbaar via de **HttpRequest.ClientCertificate** eigenschap. Voor andere toepassingsstacks is het clientcertificaat beschikbaar in uw app met een met base64 gecodeerde waarde in de aanvraagheader "X-ARR-ClientCert". Uw toepassing kunt maken van een certificaat van deze waarde en vervolgens worden gebruikt voor verificatie en autorisatie in uw toepassing.

## <a name="special-considerations-for-certificate-validation"></a>Speciale overwegingen voor validatie van het servercertificaat
Het clientcertificaat dat wordt verzonden naar de toepassing gaat niet via een validatie met het Azure App Service-platform. Dit certificaat wordt gevalideerd, is de verantwoordelijkheid van de app. Hier volgt een ASP.NET-voorbeeldcode die eigenschappen voor certificaat voor verificatiedoeleinden wordt gebruikt valideert.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
