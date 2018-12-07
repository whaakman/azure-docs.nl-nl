---
title: Azure Key Vault-beperkingsrichtlijnen
description: Key Vault wordt beperkt het aantal gelijktijdige aanroepen om te voorkomen dat bronnen overmatig worden gebruikt.
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: ''
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: bryanla
ms.openlocfilehash: eba4499a71efc84a142e8839861e33c7d7db5461
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53011889"
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Key Vault-beperkingsrichtlijnen

Beperking is een proces dat u starten die het aantal gelijktijdige oproepen aan de Azure-service beperkt om te voorkomen dat bronnen overmatig worden gebruikt. Azure Key Vault (AKV) is ontworpen voor het verwerken van een groot aantal aanvragen. Als er een groot aantal aanvragen optreedt, zorgt van de client aanvragen beperking onderhouden van optimale prestaties en betrouwbaarheid van de Azure Sleutelkluis-service.

Limieten voor bandbreedteregeling hangen af van het scenario. Bijvoorbeeld, als u een groot aantal schrijfbewerkingen uitvoert, is de mogelijkheid voor de beperking hoger dan als u alleen leesbewerkingen uitvoert.

## <a name="how-does-key-vault-handle-its-limits"></a>Hoe wordt de grenzen in Key Vault verwerkt?

Er zijn er Servicelimieten in Key Vault te voorkomen dat misbruik van resources en ervoor zorgen dat quality of service voor alle clients van de Sleutelkluis. Wanneer een service-drempel wordt overschreden, beperkt Key Vault nieuwe aanvragen van die client gedurende een bepaalde periode. Als dit gebeurt, Key Vault retourneert HTTP-statuscode 429 (te veel aanvragen), en de aanvragen mislukken. Ook mislukte aanvragen die 429 geteld naar de beperkingslimieten bijgehouden door Key Vault. 

Als u een geldige zakelijke-aanvraag voor hogere beperkingslimieten hebt, neem dan contact met ons.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Hoe u uw app in reactie op Servicelimieten beperken

Hieronder vindt u **aanbevolen procedures** voor de beperking van uw app:
- Verminder het aantal bewerkingen per aanvraag.
- Verminder de frequentie van aanvragen.
- Vermijd direct nieuwe pogingen. 
    - Alle aanvragen gebracht op basis van de gebruikslimieten.

Wanneer u foutafhandeling van uw app kunt implementeren, gebruikt u de HTTP-foutcode 429 voor het detecteren van client-side '-beperking. Als de aanvraag is mislukt opnieuw met een foutcode HTTP 429, ondervindt u nog steeds een limiet voor de Azure-service. Echter ook doorgaan met de aanbevolen client-side methode beperken, de aanvraag opnieuw proberen totdat het is gelukt.

Code implementeren die exponentieel uitstel. In dit voorbeeld [zelfstudie](tutorial-net-create-vault-azure-web-app.md) laten we zien hoe u exponentieel uitstel doet
```
     public async Task OnGetAsync()
     {
         Message = "Your application description page.";
         int retries = 0;
         bool retry = false;
         try
         {
             /* The below 4 lines of code shows you how to use AppAuthentication library to fetch secrets from your Key Vault*/
             AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
             KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
             var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                     .ConfigureAwait(false);
             Message = secret.Value;

             /* The below do while logic is to handle throttling errors thrown by Azure Key Vault. It shows how to do exponential backoff which is the recommended client side throttling*/
             do
             {
                 long waitTime = Math.Min(getWaitTime(retries), 2000000);
                 secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                     .ConfigureAwait(false);
                 retry = false;
             } 
             while(retry && (retries++ < 10));
         }
         /// <exception cref="KeyVaultErrorException">
         /// Thrown when the operation returned an invalid status code
         /// </exception>
         catch (KeyVaultErrorException keyVaultException)
         {
             Message = keyVaultException.Message;
             if((int)keyVaultException.Response.StatusCode == 429)
                 retry = true;
         }
     }

     // This method implements exponential backoff incase of 429 errors from Azure Key Vault
     private static long getWaitTime(int retryCount)
     {
         long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
         return waitTime;
     }
```

### <a name="recommended-client-side-throttling-method"></a>Aanbevolen methode voor client-side '-beperking

Beginnen met beperking van de client met behulp van een benadering exponentieel uitstel op HTTP-foutcode 429:

1. Wacht 1 seconde, aanvraag voor opnieuw proberen
2. Als u nog steeds beperkt 2 seconden wachten, opnieuw proberen de aanvraag
3. Als u nog steeds beperkt 4 seconden wachten, opnieuw proberen de aanvraag
4. Als u nog steeds beperkt 8 seconden wachten, opnieuw proberen de aanvraag
5. Als u nog steeds beperkt 16 seconden wachten, opnieuw proberen de aanvraag

Op dit moment moet niet worden aan te bieden u HTTP 429-responscodes.

## <a name="see-also"></a>Zie ook

Zie voor een uitgebreidere richting van de beperking op de Microsoft Cloud, [patroon beperking](https://docs.microsoft.com/azure/architecture/patterns/throttling).

