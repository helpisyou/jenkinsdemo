pipeline{
      // 定义groovy脚本中使用的环境变量
      environment{
        // 将构建任务中的构建参数转换为环境变量
        ORIGIN_REPO =  sh(returnStdout: true,script: 'echo $origin_repo').trim()
        REPO =  sh(returnStdout: true,script: 'echo $repo').trim()
        BRANCH =  sh(returnStdout: true,script: 'echo $branch').trim()
      }

      // 定义本次构建使用哪个标签的构建环境，本示例中为 “slave-pipeline”
      agent{
        node{
          label 'slave-pipeline'
        }
      }

      // "stages"定义项目构建的多个模块，可以添加多个 “stage”， 可以多个 “stage” 串行或者并行执行
      stages{
        // 定义第一个stage， 完成克隆源码的任务
        stage('Git'){
          steps{
            git branch: '${BRANCH}', credentialsId: '', url: 'https://github.com/helpisyou/jenkinsdemo.git'
          }
        }


        // 添加第四个stage, 部署应用到指定k8s集群
        stage('Deploy to Kubernetes') {
          steps {
            container('kubectl') {
              step([kubernetesDeploy configs: 'busybox.yaml', kubeConfig: [path: ''], kubeconfigId: '', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
])
            }
          }
        }
      }
}
