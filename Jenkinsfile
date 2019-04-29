pipeline
{



	agent any

	environment
	{
		GIT_BRANCH_NAME = "1"
		MY_BUILD_VERSION = "1"
	}

	stages
	{
		stage('Build')
		{
			steps
			{
				script
				{
					echo "another branch"
					c = checkout changelog: false, poll: true, scm: [$class: 'GitSCM', branches: [[name: '**']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git_creds', url: 'https://github.com/anuragjunghare/mvn_exer_apple.git']]]
					echo "${c}"
					MY_BUILD_VERSION = c.GIT_COMMIT[0..4]
					echo MY_BUILD_VERSION
					GIT_BRANCH_NAME = c.GIT_BRANCH

					bat "mvn -Drevision=${MY_BUILD_VERSION} clean install"
					
				}
			}

		}
		
		stage("Publish")
		{
		
			when
			 {
				    expression { 
				        GIT_BRANCH_NAME ==~ /.*master|.*feature|.*develop|.*hotfix/
				    }
			}
			
			steps
			{
				script
				{
					
					bat "mvn -Drevision=${MY_BUILD_VERSION} clean deploy"
					
				}
			}

		}
		stage('DEV')
		{
			when
				 {
						expression { 
							GIT_BRANCH_NAME ==~ /.*master|.*feature|.*develop|.*hotfix/
						}
				}
			steps
			{
				script
				{
					echo 'Deploy stage goes here'
				
					timeout(time:5, unit:'DAYS')
	                {
	                  input message : 'Approval for staging needed'
	                }
                	echo "Deleting previous jar file."
                    bat "del D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar"

                    echo "Downloading new version of jar file."

                    bat "D:\\Curl\\curl-7.64.1-win64-mingw\\bin\\curl -v -u admin:admin123 http://localhost:8081/nexus/service/local/repositories/mvn_exer_apple_rel/content/com/fruitsinfo/mvn_exer_apple/${MY_BUILD_VERSION}/mvn_exer_apple-${MY_BUILD_VERSION}.jar -o D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar"

                    bat "java -jar D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar DEV-DEPLOYMENT"
				}
			}
		}

		stage('QA')
		{
			when
				 {
						expression { 
							GIT_BRANCH_NAME ==~ /.*master|.*feature|.*develop|.*hotfix/
						}
				}

			steps
			{
				script
				{
					echo 'QA stage goes here'
					input message: 'QA Sign off', ok: 'Sign off', submitter: 'admin, testuser'

					echo "Deleting previous jar file."
                    bat "del D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar"

                    echo "Downloading new version of jar file."

                    bat "D:\\Curl\\curl-7.64.1-win64-mingw\\bin\\curl -v -u admin:admin123 http://localhost:8081/nexus/service/local/repositories/mvn_exer_apple_rel/content/com/fruitsinfo/mvn_exer_apple/${MY_BUILD_VERSION}/mvn_exer_apple-${MY_BUILD_VERSION}.jar -o D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar"

                    bat "java -jar D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar QA-DEPLOYMENT"
				}
			}
		}

		stage('PRD')
		{
			when
				 {
						expression { 
							GIT_BRANCH_NAME ==~ /.*master|.*feature|.*develop|.*hotfix/
						}
				}

			steps
			{
				script
				{
					echo 'PRD stage goes here'
				
					timeout(time:5, unit:'DAYS')
	                {
	                  input message : 'Approval for staging needed'
	                }
                	echo "Deleting previous jar file."
                    bat "del D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar"

                    echo "Downloading new version of jar file."

                    bat "D:\\Curl\\curl-7.64.1-win64-mingw\\bin\\curl -v -u admin:admin123 http://localhost:8081/nexus/service/local/repositories/mvn_exer_apple_rel/content/com/fruitsinfo/mvn_exer_apple/${MY_BUILD_VERSION}/mvn_exer_apple-${MY_BUILD_VERSION}.jar -o D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar"

                    bat "java -jar D:\\Tomcat8\\apache-tomcat-8.5.40\\webapps\\myapp\\my-app.jar PRD-DEPLOYMENT"
				}
			}
		}
	}

}
