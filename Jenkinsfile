pipeline {
  agent any

  // this section configures Jenkins options
  options {
    // only keep 10 logs for no more than 10 days
    buildDiscarder(logRotator(daysToKeepStr: "10", numToKeepStr: "10"))

    // cause the build to time out if it runs for more than 12 hours
    timeout(time: 12, unit: "HOURS")

    // add timestamps to the log
    timestamps()
  }

    // this section configures triggers
  triggers {
    // create a cron trigger that will run the job every day at midnight
    // note that the time is based on the time zone used by the server
    // where Jenkins is running, not the user's time zone
    cron "@midnight"
  }

  // the pipeline section we all know and love: stages! :D
  stages {
    stage("Git pull") {
      steps {
        git url: "git@github.com:JuanCMerchan/Proyecto_distribuidos.git", credentialsId: "personal_key", branch: "main"
      }
    }
    stage("Stash and delete") {
      steps {
        stash includes: "Monitores/**, Sensores/**", name: "stash_test"
        sh "ls"
        sh "rm -rf ./*"
        sh "ls"
      }
    }
    stage("Unstash") {
      steps {
        unstash "stash_test"
        sh "ls"
        sh "ls Monitores"
      }
    }
  }

    // the post section is a special collection of stages
    // that are run after all other stages have completed
  post {
    // the always stage will always be run
    always {

      // the always stage can contain build steps like other stages
      // a "steps{...}" section is not needed.
      echo "This step will run after all other steps have finished.  It will always run, even in the status of the build is not SUCCESS"
    }
  }
}