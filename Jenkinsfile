pipeline {
	agent any
	
	stages {
		
		stage("Compile") {
			steps {
				sh "./gradlew compileJava"
				}
			}
		stage("Unit test") {
			steps {
				sh "./gradlew test"
				}
			}
		stage("Code coverage") {
			steps {
				sh "./gradlew jacocoTestReport"
					publishHTML (target: [
						reportDir: 'build/reports/jacoco/test/html',
						reportFiles: 'index.html',
						reportName: "JaCoCo Report"
						])
				sh "./gradlew jacocoTestCoverageVerification"
				}
			}
		 stage('Report') { //(4)
           		 if (currentBuild.currentResult == 'UNSTABLE') {
                		currentBuild.result = "UNSTABLE"
            		} else {
                		currentBuild.result = "SUCCESS"
            		}
            		step([$class: 'InfluxDbPublisher', customData: null, customDataMap: null, customPrefix: null, target: 'grafana'])
        	}
		
		
	}
	post { 
        always { 
            step([$class: 'InfluxDbPublisher', customData: null, customDataMap: null, customPrefix: null, target: 'grafana'])
        }
    }
}
