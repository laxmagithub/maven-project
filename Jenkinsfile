pipeline {
    agent {
        label "master"
    }

    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://localhost:8081"
        NEXUS_REPOSITORY = "maven-nexus-repo"
        NEXUS_CREDENTIAL_ID = "nexus-user-credentials"
    }
    stages {
        stage("Clone code from VCS") {
            steps {
                script {
                    git 'https://github.com/laxmagithub/maven-project.git';
                }
            }
        }
        stage("Maven Build") {
            steps {
                script {
                    bat "mvn package"
                }
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'maventest', classifier: '', file: 'webapp/target/webapp.war', type: 'war']], credentialsId: 'NEXUS_CREDENTIAL_ID', groupId: 'laxma', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-nexu-repo', version: '1.1-SNAPSHOT'
            }
        }
        stage("Deploy to ansible server using ssh publisher") {
            steps {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: '**/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
