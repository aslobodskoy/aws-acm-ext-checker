#!/usr/bin/env groovy

properties([
   pipelineTriggers([cron('H 0 * * *')]),
   disableConcurrentBuilds()
])
node {
    stage ('Certificate Check') {
      sh """#!/bin/bash
      for i in \$(aws acm list-certificates | jq -r .CertificateSummaryList[].CertificateArn); do
      certexpdate=\$(aws acm describe-certificate --certificate-arn \$i|jq -r \'.[].NotAfter\')
      echo \$certexpdate
      timeleft=\$(((\$certexpdate - \$(date +%s))/86400))
      if (( \$timeleft < 30 ))  ; then
      aws sns publish --topic-arn $aws_sns_low_alert --message \"problem with following certificate \$i \$(aws acm describe-certificate --certificate-arn \$i|jq -r .[].DomainName)\" --subject ACM-Ext-Checker
      fi
      done
      """
  }
}
