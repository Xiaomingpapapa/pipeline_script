
pipeline {
    agent any
    tools {
        //工具名称必须在Jenkins 管理Jenkins → 全局工具配置中预配置。
        gradle 'mygradle-4.10'
    }
    stages {
        stage('git checkout') {
            steps {
            	git branch: params.Branch, credentialsId: '031f7c9e-cb86-45c5-ad1b-64b81174fc8b', url: 'http://192.168.1.15:8080/tfs/Mobile.Dev/Android/_git/DriverAndroid'
            }
        }
        stage('build') {
        	steps {
				sh "gradle clean jacocoTestReport buildDriverPadApk'${Environment}' -stacktrace -debug -PIS_JENKINS='true' -Pandroid.buildCacheDir='D:/android-build-cache'"
        	}
        }
        stage('rename apk') {
        	steps {
        		bat 'ren .\\EhaiPadClient\\build\\outputs\\apk\\%Environment%\\*.apk driverpad_%BUILD_TIMESTAMP%.apk'
        	}
        }
    }
}