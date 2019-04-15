
pipeline {
    agent any
    stages {
        stage('git checkout') {
            steps {
            	git branch: '*/feature/components', credentialsId: '031f7c9e-cb86-45c5-ad1b-64b81174fc8b', url: 'http://192.168.1.15:8080/tfs/Mobile.Dev/Android/_git/DriverAndroid'
            }
        }
        stage('build') {
        	withGradle(gradle: 'mygradle 4.10') {
                        sh "gradle clean jacocoTestReport buildDriverPadApk'${Environment}' -stacktrace -debug -PIS_JENKINS='true' -Pandroid.buildCacheDir='D:/android-build-cache'"
                    }
        }
        stage('rename apk') {
        	bat 'ren .\\EhaiPadClient\\build\\outputs\\apk\\%Environment%\\*.apk driverpad_%BUILD_TIMESTAMP%.apk'
        }
    }
}