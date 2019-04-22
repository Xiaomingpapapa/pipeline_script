
pipeline {
	agent any
	environment {
		gradleHome = tool 'mygradle-4.10'     
	}
	stages {
		stage('更新项目代码') {
			steps {
				checkout([$class: 'GitSCM', branches: [[name: "${params.Branch}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '031f7c9e-cb86-45c5-ad1b-64b81174fc8b', url: 'http://192.168.1.15:8080/tfs/Mobile.Dev/Android/_git/DriverAndroid']]])
			}
		}		
	}
	post {
	        success {
	            emailext (
	                subject: "'${env.JOB_NAME} [${env.BUILD_NUMBER}]' 更新正常",
	                body: """
	                详情：
	                SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'
	                状态：${env.JOB_NAME} jenkins 更新运行正常 
	                URL ：${env.BUILD_URL}
	                项目名称 ：${env.JOB_NAME} 
	                项目更新进度：${env.BUILD_NUMBER}
	                """,
	                to: "zhiming.chen@1hai.cn",
	                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
	                )
	                }   
	        failure {
	            emailext (
	                subject: "'${env.JOB_NAME} [${env.BUILD_NUMBER}]' 更新失败",
	                body: """
	                详情：
	                FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'             
	                状态：${env.JOB_NAME} jenkins 运行失败 
	                URL ：${env.BUILD_URL}
	                项目名称 ：${env.JOB_NAME} 
	                项目更新进度：${env.BUILD_NUMBER}
	                """,
	                to: "zhiming.chen@1hai.cn",
	                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
	                )
	                }
	    }	
}
