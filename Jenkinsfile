node
{
def mavenHome = tool name: "maven3.6.3"
    
    stage("1. git clone")
    {
       git credentialsId: 'gitCredentials', url: 'https://github.com/WinifredZenabuin/UnityProject.git'
    }
    
    stage("2. Maven Build")
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    
    stage('3. SonarQube Code Quality report')
        {
         //sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('4. Deploy Artifacts to Nexus')
     {
        // sh "${mavenHome}/bin/mvn deploy"
    }
    
    stage('5. Build Docker Image')
    {
        
        sh "docker build -t sgwilliams/springboot-app ."

    }
    
    stage('6. Push Docker Image to Docker Hub')
    {
       withCredentials([string(credentialsId: 'DockerHubCredentials', variable: 'DockerHubCredentials')]) {
       sh " docker login -u sgwilliams -p ${DockerHubCredentials} "
     }

        sh " docker push sgwilliams/springboot-app "
    }
    
    stage('7. Deploy to EKS Kubernetes Cluster')
    {
  
        kubernetesDeploy(
            configs: 'springboot-app-deployment.yml' ,
            kubeconfigId: 'KubernetesClusterConfig' ,
            enableConfigSubstitution: true
            )
        
       // sh 'kubectl apply -f springboot-app-deployment.yml'
    }

    stage('8 EmailNotification')
 {
 mail bcc: 'suzettesgwilliams@gmail.com', body: '''Build is over
 Thanks,
 S. Williams
 9980923226.''', cc: 'suzettesgwilliams@gmail.com', from: '', replyTo: '', subject: 'Build is over!!', to: 'sgwilliams@gmail.com'
 }
 */
 
 }
