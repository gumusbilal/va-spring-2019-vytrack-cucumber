node {
   stage('Cloning project from Github') { // for display purposes
      // Get some code from a GitHub repository
     git 'https://github.com/CybertekSchool/va-spring-2019-vytrack-cucumber.git'
   }
   
     stage('Running Tests') {
              bat label: 'chrome', script: 'mvn clean test'
     }
     
      stage('Generating report') {
              cucumber fileIncludePattern: '**/*.json', sortingMethod: 'ALPHABETICAL'
      }
      
      stage('Moving cucumber report to the reports folder'){
             bat label: '', script: 'xcopy /E /I "%programfiles(x86)%\\Jenkins\\jobs\\vytrack-smoketest-pipeline\\builds\\%BUILD_NUMBER%\\cucumber-html-reports" "C:\\reports\\pipeline-reports\\vytrack-smoketest-pipeline\\%BUILD_NUMBER%"'
      }

    stage('Sending email notification'){
        def readContent = readFile 'C:\\Program Files (x86)\\Jenkins\\workspace\\vytrack-smoketest-pipeline\\target\\surefire-reports\\com.vytrack.automation.runners.*.txt'
        if(readContent.contains("FAILURE")){
          emailext body: '''
                                  <!DOCTYPE html>
<html>
                 <head>
                
                    <title >Smoke Test Report</title>
                 </head>
                 <body>
                
                <div class="container">
                    <br>
                    <h1 style="color: red; text-align: center">SMOKE TEST FAILED!</h1>
                    <br/>
                 <br/>
                <h2 class="h2">Click to view more details:</h2>
                <br/>
                  <a class="btn-info" href="http://ec2-54-204-67-55.compute-1.amazonaws.com/reports/pipeline-reports/vytrack-smoketest-pipeline/${BUILD_NUMBER}/overview-features.html">Report link</a>
                <br/>
                <br>
                </div>
               
                 </body>
                 <style type="text/css">
                    
                 </style>
                 </html>
                 ''', subject: 'SMOKE TEST FALIED!', to: 'vasyl@cybertekschool.com'
          } else {
                emailext body: '''<!DOCTYPE html>
<html>
                 <head>
            
                    <title >Smoke Test Report</title>
                 </head>
                 <body>
                
                <div class="container">
                    <br>
                    <h1 style="color: green; text-align: center">SMOKE TEST PASSED!</h1>
                    <br/>
                 <br/>
                
           
                <h2 class="h3">Click to view more details:</h2>
                <br/>
                  <a class="btn-info" href="http://ec2-54-204-67-55.compute-1.amazonaws.com/reports/pipeline-reports/vytrack-smoketest-pipeline/${BUILD_NUMBER}/overview-features.html">Report link</a>
                <br/>
                <br>
                </div>
               
                 </body>
                 <style type="text/css">
                    
                 </style>
                 </html>''', subject: 'SMOKE TEST PASSED!', to: 'vasyl@cybertekschool.com'
        
        } 
    }
            
}
