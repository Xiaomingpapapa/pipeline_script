
pipeline {
    agent any
    environment {
      gradleHome = tool 'mygradle-4.10'     
    }
    stages {
	   stage('Preparation') {
	      steps {
			checkout([$class: 'GitSCM', branches: [[name: '*/feature/components']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '031f7c9e-cb86-45c5-ad1b-64b81174fc8b', url: 'http://192.168.1.15:8080/tfs/Mobile.Dev/Android/_git/DriverAndroid']]])
	      }
	   }
	   stage('Build APK') {
	      steps {
	      	script {
	      	  if (isUnix()) {
	          	sh "'${gradleHome}/bin/gradle' -v"
	      	  } else {
	         	bat "${gradleHome}\\bin\\gradle clean jacocoTestReport buildDriverPadApk${params.Environment} -stacktrace -debug -PIS_JENKINS=true -Pandroid.buildCacheDir=D:/android-build-cache"
	      	  }	
	      	}
	      }
	   }
	   stage('Rename APK') {
	      steps {  	
	        bat "ren .\\EhaiPadClient\\build\\outputs\\apk\\${params.Environment}\\*.apk driverpad_${BUILD_TIMESTAMP}.apk"
	  	  }
	   }
	   stage('Upload APK') {
	   	  steps {
	        sshPublisher(publishers: [sshPublisherDesc(configName: '192.168.5.140-SSH', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/jenkins_build/EhiDriverPadAPK', remoteDirectorySDF: false, removePrefix: '/EhaiPadClient/build/outputs/apk/', sourceFiles: '/EhaiPadClient/build/outputs/apk/${params.Environment}/*.apk')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	      }
	   }
	   stage('Generate QR Code') {
	   	  steps {
	   	  	bat "java -jar D:\\test\\qr.jar url=http://192.168.5.140/automationtest/script/EhiDriverPadAPK/${params.Environment}/driverpad_${BUILD_TIMESTAMP}.apk image=driverpad_${BUILD_TIMESTAMP}.jpg save=./"
	   	  }
	   }
	   stage('Upload QR Code') {
	   	  steps {
	   	  	sshPublisher(publishers: [sshPublisherDesc(configName: '192.168.5.140-SSH', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: "/jenkins_build/EhiDriverPadAPK/${params.Environment}/", remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'driverpad_${BUILD_TIMESTAMP}.jpg')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
	   	  }  
	   }
	   stage('Set Build Description') {
	   	  steps {
	   	  	script {
	          currentBuild.description = "<img src='http://192.168.5.140/automationtest/script/EhiDriverPadAPK/${params.Environment}/driverpad_${BUILD_TIMESTAMP}.jpg' alt='二维码${BUILD_TIMESTAMP}' width='200' height='200' /><a href='http://192.168.5.140/automationtest/script/EhiDriverPadAPK/${params.Environment}/driverpad_${BUILD_TIMESTAMP}.jpg'>二维码${BUILD_TIMESTAMP}</a>"
	        }
	   	  }
	   }
	   stage('Publish Unit Test Report') {
	   	  steps {
	   	    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'EhaiPadClient/build/reports/tests/testDebugUnitTest', reportFiles: 'index.html', reportName: '司机APP单元测试报告', reportTitles: ''])
	   	  }
	   }
	   stage('Publish Unit Test Coverage Report') {
	   	  steps {
	   	  	publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'EhaiPadClient/build/reports/coverage', reportFiles: 'index.html', reportName: '司机APP单元测试覆盖率报告', reportTitles: ''])
	   	  }
	   }
	   stage('Publish Jacoco Coverage Report') {
	   	  steps {
	   	  	jacoco classPattern: '**/build/intermediates/javac/debug/compileDebugJavaWithJavac/classes', execPattern: '**/build/jacoco/testDebugUnitTest.exec'
	   	  }
	   }
	}
}


