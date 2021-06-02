#!/usr/bin/env groovy

properties([
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
      echo \$timeleft
      fi
      done
      """
  }
}
