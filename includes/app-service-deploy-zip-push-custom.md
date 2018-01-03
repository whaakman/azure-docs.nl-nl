## <a name="deployment-customization"></a>Implementatieaanpassing

Het implementatieproces wordt ervan uitgegaan dat het ZIP-bestand dat u push-een kant-en-klaar app bevat. Standaard worden geen aanpassingen uitvoeren. Voeg de volgende instellingen van de toepassing zodat dezelfde build processen die u met continue integratie krijgt:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Wanneer u een ZIP-push-implementatie gebruikt, wordt deze instelling is **false** standaard. De standaardwaarde is **true** voor continue integratie-implementaties. Als de waarde **true**, uw instellingen met betrekking tot implementatie worden gebruikt tijdens de implementatie. U kunt deze instellingen configureren als de app-instellingen of in een configuratiebestand .deployment dat zich in de hoofdmap van het ZIP-bestand bevindt. Zie voor meer informatie [opslagplaats en instellingen met betrekking tot implementatie](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) in de documentatie van de implementatie.