pipeline{
	agent any
	
	stages{
		stage("Checkout the code"){
			steps{
				git branch: 'main', url: 'https://github.com/Ayrola/WorkShop_2_Jenkins_Task2'
			}
		}
		
		stage("Set up dot net 6"){
			steps{
				bat '''
				curl -l -o dotnet-sdk-6.0.136-win-x86.exe https://builds.dotnet.microsoft.com/dotnet/Sdk/6.0.136/dotnet-sdk-6.0.136-win-x86.exe
				dotnet-sdk-6.0.136-win-x86.exe /quiet /norestart
				'''
			}
		}
		
		stage('install nuget packages'){
			steps{
				bat 'dotnet restore SeleniumBasicExercise.sln'
			}
		}
		
		stage("Build the project"){
			steps{
				bat 'dotnet build SeleniumBasicExercise.sln'
			}
		}
		
		stage("Run tests for TestProject1"){
			steps{
				bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
			}
		}
		
		stage("Run tests for testproject2"){
			steps{
				bat 'dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
			}
		}
	}
	
	post{
		always{
			archiveArtifacts artifacts: '**/TestResults/*.trx',
			allowEmptyArchive: true
			step([
				$class: 'MSTestPublisher',
				testResultsFile: '**/TestResults/*.trx'
			])
		}
	}
}