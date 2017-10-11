---
title: Azure Mobile Engagement - back-end-integratie
description: Verbinding maken met Azure Mobile Engagement met een SharePoint-back-end campagnes maken van SharePoint
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 06297b43-579f-46e6-8a58-961a68f9aa09
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d49f1094f4c3f170f3618f3e19e42266f9ae8858
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement---api-integration"></a>Azure Mobile Engagement - API-integratie
In een geautomatiseerde marketing-systeem, maken en activeren van de marketingcampagnes ook automatisch uitgevoerd. Voor dit doel - Azure Mobile Engagement kunt maken van dergelijke geautomatiseerde marketingcampagnes ook met behulp van API's. 

Klanten gebruiken doorgaans de Mobile Engagement-front-end interface aankondigingen/polls enzovoort als onderdeel van hun marketingcampagnes maken. Maar zodra de marketingcampagnes volwassen, hoeft een gebruikmaken van de gegevens in de back endsystemen (zoals een CRM-systeem of CMS-systeem zoals SharePoint) moet worden vergrendeld zodat een volledig geautomatiseerde pijplijn kan worden gemaakt die campagnes maakt in Mobile Engagement dynamisch op basis van de gegevens die van de back endsystemen. 

![][5]

Deze zelfstudie doorloopt van een scenario waarbij een SharePoint-bedrijven gebruiker vult een SharePoint-lijst met marketing gegevens en een geautomatiseerd proces items uit de lijst wordt opgehaald en verbinding maakt met de Mobile Engagement-systeem met de beschikbare REST-API's voor het maken van een campagne van de SharePoint-gegevens. 

> [!IMPORTANT]
> In het algemeen kunt u dit voorbeeld als uitgangspunt voor informatie over het aanroepen van Mobile Engagement REST API, zoals het details van de twee belangrijke aspecten van het aanroepen van de API - verificatie en doorgegeven parameters. 
> 
> 

## <a name="sharepoint-integration"></a>Integratie met SharePoint
1. Hier ziet u hoe de SharePoint-voorbeeldlijst eruit ziet. **Titel**, **categorie**, **NotificationTitle**, **bericht** en **URL** worden gebruikt voor het maken van de aankondiging. Er is een kolom met de naam **IsProcessed** die wordt gebruikt door het proces van het automation voorbeeld in de vorm van een consoleprogramma. U kunt deze console programma als een webtaak Azure zodat u kunt plannen dat het uitvoeren of u kunt de SharePoint-werkstroom programma maken en de aankondiging activeren wanneer een item wordt ingevoegd in de SharePoint-lijst rechtstreeks gebruiken. In dit voorbeeld gebruiken we de consoleprogramma gaat door de items in de SharePoint-lijst en de aankondiging te maken in Azure Mobile Engagement voor elk van deze en ten slotte markeert vervolgens de **IsProcessed** vlag waar op geslaagde aankondiging maken.
   
    ![][1]
2. We gebruiken de code van de steekproef *externe verificatie in SharePoint Online met behulp van de Client-objectmodel* [hier](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) voor verificatie met de SharePoint-lijst.
3. Eenmaal is geverifieerd, wordt de items in de lijst om erachter te komen de zojuist gemaakte items doorlopen (die zullen hebben **IsProcessed** = false). 
   
         static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);
   
                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();
   
                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;
   
                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;
   
                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";
   
                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Mobile Engagement-integratie
1. Zodra er een item dat worden verwerkt moet - gevonden we gegevens nodig voor het maken van een aankondiging van het item in de lijst en de aanroep ophalen `CreateAzMECampaign` om deze te maken en vervolgens `ActivateAzMECampaign` om deze te activeren. Dit zijn in wezen REST-API aanroepen aanroepen van de back-end van Mobile Engagement. 
2. De Mobile Engagement REST-API's vereisen een **Basic auth schema autorisatie HTTP-header** die bestaat uit de `ApplicationId` en de `ApiKey` die u krijgen via de Azure-portal. Zorg ervoor dat u van de sleutel van gebruikmaakt de **api-sleutels** sectie en *niet* van de **sdk sleutels** sectie. 
   
   ![][2]
   
       static string CreateAuthZHeader()
       {
           string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
           return BasicAuthzHeaderString;
       }
   
       static public string EncodeTo64(string toEncode)
       {
           byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
           string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
           return returnValue;
       }  
3. Voor het maken van de aankondiging type campagne - verwijzen naar de [documentatie](https://msdn.microsoft.com/library/azure/mt683750.aspx). U moet ervoor zorgen dat u de campagne opgeeft `kind` als *aankondiging* en de [nettolading](https://msdn.microsoft.com/library/azure/mt683751.aspx) en door te geven als FormUrlEncodedContent. 
   
        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";
   
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });
   
                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }
4. Nadat u de aankondiging gemaakt hebt, ziet u ongeveer als volgt op de Mobile Engagement-portal (Houd er rekening mee dat de status concept en geactiveerd = = N.V.T.)
   
    ![][3]
5. `CreateAzMECampaign`maakt een campagne aankondiging en retourneert de Id naar de aanroeper. `ActivateAzMECampaign`deze Id is vereist als de parameter om de campagne te activeren. 
   
        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });
   
                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }
6. Zodra u de aankondiging geactiveerd hebt, worden er ongeveer als volgt op de Mobile Engagement-portal:
   
    ![][4]
7. Zodra de campagne wordt geactiveerd, gaat alle apparaten die voldoen aan de criteria voor de campagne meldingen te zien. 
8. U ziet ook dat het item in de lijst gemarkeerd met IsProcessed = false is ingesteld op waar zodra de aankondiging campagne is gemaakt.  

Dit voorbeeld gemaakt een eenvoudige aankondiging campagne voornamelijk de vereiste eigenschappen opgegeven. U kunt dit aanpassen als u vanuit de portal kunt met behulp van de informatie [hier](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



