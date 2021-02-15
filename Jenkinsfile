pipeline {
	agent any
	
	stages {
		
		stage("Compile") {
			steps {
				//sh "./gradlew compileJava"
				bat "gradlew compileJava"
				}
			}
		stage("Unit test") {
			steps {
				// sh "./gradlew test"
				bat "gradlew test"
				script {
					
					step([$class: 'JacocoPublisher', execPattern: '**/*.exec'])
					}
				}
			}
			stage("Code coverage") {
			steps {
				//sh "./gradlew jacocoTestReport"
				bat "gradlew jacocoTestReport"
					
				
					
					publishHTML (target: [
						reportDir: 'build/reports/jacoco/test/html',
						reportFiles: 'index.html',
						reportName: "JaCoCo Report"
						])
		    		//sh "./gradlew jacocoTestCoverageVerification"
				bat "gradlew jacocoTestCoverageVerification"
								
				}
			 }  
	}
	post { 
        always { 
		script {
         		 try {
		
           			 step([$class: 'InfluxDbPublisher', customData: null, customDataMap: null, customPrefix: null, selectedTarget: 'grafana'])
				}
			finally {
           			 // echo "in finally"
         			 }
			}
		}
	   }
}
