<?xml version='1.1' encoding='UTF-8'?>
<flow-definition plugin="workflow-job@2.40">
  <actions>
    <org.jenkinsci.plugins.pipeline.modeldefinition.actions.DeclarativeJobAction plugin="pipeline-model-definition@1.8.4"/>
    <org.jenkinsci.plugins.pipeline.modeldefinition.actions.DeclarativeJobPropertyTrackerAction plugin="pipeline-model-definition@1.8.4">
      <jobProperties/>
      <triggers/>
      <parameters/>
      <options/>
    </org.jenkinsci.plugins.pipeline.modeldefinition.actions.DeclarativeJobPropertyTrackerAction>
  </actions>
  <description></description>
  <keepDependencies>false</keepDependencies>
  <properties>
    <org.conjur.jenkins.configuration.ConjurJITJobProperty plugin="conjur-credentials@1.0.2">
      <inheritFromParent>true</inheritFromParent>
      <useJustInTime>false</useJustInTime>
      <authWebServiceId></authWebServiceId>
      <hostPrefix></hostPrefix>
      <conjurConfiguration>
        <applianceURL></applianceURL>
        <account></account>
        <credentialID></credentialID>
        <certificateCredentialID></certificateCredentialID>
      </conjurConfiguration>
    </org.conjur.jenkins.configuration.ConjurJITJobProperty>
  </properties>
  <definition class="org.jenkinsci.plugins.workflow.cps.CpsFlowDefinition" plugin="workflow-cps@2.92">
    <script>pipeline {
    agent any

    stages {
        
        stage(&apos;Git Checkout&apos;)
        {
            steps{
                git branch: &apos;main&apos;, changelog: false, poll: false, url: &apos;https://github.com/rajoraamisha/DevopsBasics;
            }
        }
        
        stage(&apos;Export Integration&apos;) {
            steps {
                withCredentials([usernameColonPassword(credentialsId: &apos;OIC&apos;, variable: &apos;oiccredential&apos;)]) {
    sh &apos;&apos;&apos;curl -X GET -u $oiccredential -o \&apos;CSF_PPM_FINPROPLAN_CALLBA_RECIPE_02.00.0000.iar\&apos; https://oic-bmjmq3ynseyn-bo.integration.ocp.oraclecloud.com/ic/api/integration/v2/integrations/SAMPLE_SFTP%7C01.00.0000/archive

sleep 180&apos;&apos;&apos;
}
            }
        }
        
        stage(&apos;Import Integration&apos;) {
            steps {
                withCredentials([usernameColonPassword(credentialsId: &apos;OIC&apos;, variable: &apos;oiccredential&apos;)]) {
   sh &apos;&apos;&apos;curl -s -u $oiccredential -H &quot;Accept: application/json&quot; -X POST -F \\
&quot;file=@CSF_PPM_FINPROPLAN_CALLBA_RECIPE_02.00.0000.iar;type=application/octet-stream&quot; \\
https://oic-bmjmq3ynseyn-bo.integration.ocp.oraclecloud.com/ic/api/integration/v2/integrations/archive&apos;&apos;&apos;
}
            }
        }
               stage(&apos;Configure Connection&apos;) {
            steps {
                withCredentials([usernameColonPassword(credentialsId: &apos;OIC&apos;, variable: &apos;oiccredential&apos;)]) {
   sh &apos;&apos;&apos;curl -s -X POST -u $oiccredential \\
-H &quot;X-HTTP-Method-Override:PATCH&quot; -H &quot;Content-Type:application/json&quot; -d @SFTP.json \\
https://oic-bmjmq3ynseyn-bo.integration.ocp.oraclecloud.com/ic/api/integration/v2/connections/SFTP&apos;&apos;&apos;
}
            }
        }  
        stage(&apos;Activate Integration&apos;) {
            steps {
                withCredentials([usernameColonPassword(credentialsId: &apos;OIC&apos;, variable: &apos;oiccredential&apos;)]) {
   sh &apos;curl  -X POST -u $oiccredential -H &quot;Content-Type:application/json&quot; -H &quot;X-HTTP-Method-Override:PATCH&quot; -d @activate.json https://oic-bmjmq3ynseyn-bo.integration.ocp.oraclecloud.com/ic/api/integration/v2/integrations/SAMPLE_SFTP%7C01.00.0000&apos;
}
            }
        }  
        
    }
}
</script>
    <sandbox>true</sandbox>
  </definition>
  <triggers/>
  <disabled>false</disabled>
</flow-definition>
