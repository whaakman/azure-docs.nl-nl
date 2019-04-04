---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 11/25/2018
ms.author: crdun
ms.openlocfilehash: d71d52257b6e8cfa243207c9bfdb5c7de7d3dd37
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58890912"
---
1. Voeg het volgende toe in het projectbestand MainPage.xaml.cs **met behulp van** instructies:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Vervang de **AuthenticateAsync** methode met de volgende code:
   
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
   
            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;
   
            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }
   
            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;
   
                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;
   
                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: https://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Sign in with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider, "{url_scheme_of_your_app}");
   
                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now signed in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must sign in. Sign-In Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    In deze versie van **AuthenticateAsync**, de app probeert te gebruiken van referenties die zijn opgeslagen de **PasswordVault** voor toegang tot de service. Een gewone aanmelding wordt ook uitgevoerd wanneer er geen opgeslagen referenties.
   
   > [!NOTE]
   > Een token in cache is mogelijk verlopen en token verloopt na verificatie kan ook optreden wanneer de app gebruikt wordt. Zie voor informatie over het bepalen of een token is verlopen, [controleren voor verlopen verificatietokens](https://aka.ms/jww5vp). Zie voor een oplossing voor het afhandelen van autorisatiefouten met betrekking tot verlopen tokens, het bericht [Caching en verwerken van vervallen tokens in Azure Mobile Services SDK beheerd](https://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 
   > 
   > 
3. De app twee keer opnieuw opgestart.
   
    U ziet dat op het eerste lid is, aanmelden met de provider is weer vereist. Echter op de tweede keer opnieuw opstarten referenties in de cache worden gebruikt en de aanmelding wordt overgeslagen. 

