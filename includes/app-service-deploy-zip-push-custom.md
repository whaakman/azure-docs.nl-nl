## <a name="deployment-customization"></a>Implementatieaanpassing

Het implementatieproces wordt ervan uitgegaan dat het ZIP-bestand dat u push-een kant-en-klaar app bevat. Standaard worden geen aanpassingen uitvoeren. U kunt de build processen die u met continue integratie door het volgende toe te voegen aan uw toepassingsinstellingen inschakelen:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Wanneer u .zip push implementatie gebruikt, wordt deze instelling is **false** standaard. De standaardwaarde is **true** voor continue integratie-implementaties. Als de waarde **true**, uw instellingen met betrekking tot implementatie worden gebruikt tijdens de implementatie. Deze instellingen kunnen worden ingesteld als de app-instellingen of in een `.deployment` configuratiebestand dat zich bevindt in de hoofdmap van het zipbestand. Zie voor meer informatie [opslagplaats en instellingen met betrekking tot implementatie](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) in de documentatie van de implementatie.