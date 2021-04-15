### Script에 바로 환경 설정값을 넣은 경우

```
stage('BUILD') {
                container('maven') {
                    mavenBuild goal: 'clean package sonar:sonar -DskipTests=true -Dsonar.login=647d0d0446a639a3b2e03587c4be8d3d4c34ae5f \
                -Dsonar.host.url=http://xxx.xxx.xxx.xxxx:9000 \
                -Dsonar.projectKey=OffRev -Dsonar.projectName=OffRev \
                -Dsonar.projectVersion=Jenkins_maven_${BUILD_NUMBER} -f awesome-account-service/pom.xml', systemProperties:['maven.repo.local':"/root/.m2/${JOB_NAME}"]
                }
            }
```


### Jenkins에  Maven settings.xml 설정하여 처리하는 방법

* Dashboard dropdown menu > Jenkins 관리 > System Configuration > Managed files을 선택합니다.

```script
@Library('retort-lib') _
def label = "jenkins-${UUID.randomUUID().toString()}"

def ZCP_USERID='happicklive'
def K8S_NAMESPACE='happicklive-pilot'
def DOCKER_PROJECT='happicklive-pilot'
def DOCKER_IMAGE="${DOCKER_PROJECT}/happicklive-product"
// config/overlay/dev/sns/kustomization.yaml의 newTag 값과 맞춤. 자동화 필요함.
//def DOCKER_VERSION="0.0.1-${BUILD_NUMBER}"
def DOCKER_VERSION="latest"

def repo

timestamps {
    podTemplate(label:label,
        serviceAccount: "zcp-system-sa-${ZCP_USERID}",
        containers: [
            containerTemplate(name: 'maven', image: 'maven:3.6.3-jdk-11-slim', ttyEnabled: true, command: 'cat'),
            containerTemplate(name: 'docker', image: 'docker:17-dind', ttyEnabled: true, command: 'dockerd-entrypoint.sh', privileged: true),
            containerTemplate(name: 'tools', image: 'argoproj/argo-cd-ci-builder:v1.0.0', command: 'cat', ttyEnabled: true),
            containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.18.2', ttyEnabled: true, command: 'cat'),
            containerTemplate(name: 'kustomize', image: 'gauravgaglani/k8s-kustomize:1.1.0', ttyEnabled: true, command: 'cat')
        ],
        volumes: [
            persistentVolumeClaim(mountPath: '/root/.m2', claimName: 'zcp-jenkins-mvn-repo')
        ]) {
    
        node(label) {
            stage('SOURCE CHECKOUT') {
                repo = checkout scm
            }

            stage('CHECKOUT') {
                container('tools') {
                    echo '개발소스쪽 Git Repo로 Checkout'
                    sh 'git checkout origin1/master'
                }
            }
 
            //stage('Unit Test') {
            //     container('maven') {
            //         mavenBuild goal: 'test -f pom.xml', systemProperties:['maven.repo.local':"/root/.m2/${JOB_NAME}"]
			//	 }
            //}

            stage('Static Code Analysis') {
                    configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
                       // sh './mvnw sonar:sonar -s $MAVEN_SETTINGS'
                       container('maven') {
                           mavenBuild goal: 'clean compile sonar:sonar -DskipTests=true -f pom.xml -s $MAVEN_SETTINGS', systemProperties:['maven.repo.local':"/root/.m2/${JOB_NAME}"]
                       }
                    }
            }

            //stage('BUILD') {
            //    container('maven') {
            //        mavenBuild goal: 'clean package -DskipTests=true -f pom.xml', systemProperties:['maven.repo.local':"/root/.m2/${JOB_NAME}"]
            //    }
            //}
    
            //stage('BUILD DOCKER IMAGE') {
            //    container('docker') {
            //        dockerCmd.build tag: "${HARBOR_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_VERSION}"
            //        dockerCmd.push registry: HARBOR_REGISTRY, imageName: DOCKER_IMAGE, imageVersion: DOCKER_VERSION, credentialsId: "HARBOR_CREDENTIALS"
            //    }
            //}
        }
    }
}
```