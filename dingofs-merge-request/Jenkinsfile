pipeline {
    agent{
            label "jenkins_172.30.14.126"
        }

    stages {
        // stage('代码检出') {
        //     steps {
        //         checkout scm
        //     }
        // }

        // stage('安装 Python 依赖') {
        //     steps {
        //         sh '''
        //             python3 -m pip install --upgrade pip
        //             pip install pytest pytest-html pytest-cov
        //         '''
        //     }
        // }
        stage('强制失败测试') {
            steps {
                // sh 'exit 1'   // 返回非零，强制失败
                echo 123
            }
        }

        stage('运行 pytest 测试') {
            steps {
                // 运行测试，生成 JUnit 格式的测试报告（Jenkins 可读）
                // sh '''
                //     pytest --junitxml=report.xml --html=report.html --self-contained-html
                // '''
                echo "模拟 pytest 测试执行，生成 report.xml 和 report.html"
            }
            // post {
            //     always {
            //         // 无论成功还是失败，都发布测试报告
            //         junit 'report.xml'
            //         publishHTML([
            //             allowMissing: false,
            //             alwaysLinkToLastBuild: true,
            //             keepAll: true,
            //             reportDir: '',
            //             reportFiles: 'report.html',
            //             reportName: 'Pytest HTML Report'
            //         ])
            //     }
            // }
        }
    }

    post {
        success {
            // 测试通过 → 通知 GitHub 状态为成功
            setGitHubStatus("所有 pytest 测试通过", "SUCCESS")
        }
        failure {
            // 测试失败 → 通知 GitHub 状态为失败
            setGitHubStatus("pytest 测试失败，请检查报告", "FAILURE")
        }
        aborted {
            // 任务被取消 → 通知 GitHub 状态为 pending（可选）
            setGitHubStatus("构建被取消", "PENDING")
        }
    }
}

// 向 GitHub 上报状态的核心函数
def setGitHubStatus(String message, String state) {
    step([
        $class: "GitHubCommitStatusSetter",
        reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/cocadgy/dingofs-automation-framwork"],
        contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/pytest"],
        errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
        statusResultSource: [$class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]]]
    ]);
}