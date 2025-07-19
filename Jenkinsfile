pipeline
{
    agent any
    tools
    {
       maven "Maven-3.9"
    }
    stages
    {
        stage('Git checkout')
        {
            steps
            {
                git branch: 'Development', url: 'https://github.com/Devopsb5/maven-webapplication-project-kkfunda.git'
            }
        }
        stage('build atifact')
        {
            steps
            {
                sh "mvn clean package"
            }
        }
        stage('sonar qube')
        {
            steps
            {
                sh "mvn sonar:sonar"
            }
        }
        stage('nexus')
        {
            steps
            {
                sh "mvn deploy"
            }
        }
        stage('tomcat')
        {
            steps
            {
                sh """
                curl -u Rohith:password \
                --upload-file /var/lib/jenkins/workspace/Declarative-pl/target/maven-web-application.war \
                "http://13.203.202.17:8080/manager/text/deploy?path=/maven-web-application&update=true"
                """
            }
        }
    }
    
} //pipeline end

// Notification method
def notifyBuild(String buildStatus = 'STARTED') {
    buildStatus = buildStatus ?: 'SUCCESS'

    def colorCode
    def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
    def summary = "${subject} (${env.BUILD_URL})"

    switch (buildStatus) {
        case 'STARTED':
            colorCode = '#FFFF00' // Yellow
            break
        case 'SUCCESS':
            colorCode = '#00FF00' // Green
            break
        default:
            colorCode = '#FF0000' // Red
    }

    slackSend(color: colorCode, message: summary)
}


