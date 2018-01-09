 ---
titel: de omgeving Postman importeren voor Azure Media Services REST-beschrijving aanroepen: dit onderwerp bevat een definitie van de omgeving Postman voor Azure Media Services REST-aanroepen.
Services: media services documentationcenter: '' auteur: Juliako manager: cfowler-editor: ''

MS.service: media services ms.workload: media ms.tgt_pltfrm: n.v.t. ms.devlang: n.v.t. ms.topic: artikel ms.date: 01/04/2017 ms.author: juliako

---

# <a name="import-the-postman-environment"></a>De omgeving Postman importeren 

In dit artikel bevat een definitie van de **Postman** omgevingsvariabelen die worden gebruikt de [Postman verzameling](postman-collection.md) die gegroepeerde HTTP-aanvragen die aanroepen van Media Services REST-API's bevat. De omgeving en verzameling bestanden worden gebruikt door de [Postman configureren voor Media Services REST-API-aanroepen](media-rest-apis-with-postman.md) zelfstudie.

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
