node {
    stage('Preparation') {
        checkout scm
        versionNumber = VersionNumber versionNumberString: '${BUILD_DATE_FORMATTED, "yy.MM"}.${BUILDS_THIS_MONTH}', versionPrefix: '', buildsAllTime: '12'
        echo "VersionNumber: ${versionNumber}"
    }
    stage('Build Docker image') {
        sh "./mkdocker"
    }
    stage('Push Docker image') {
        sh "docker tag inomial.io/robbertkl/ipv6nat inomial.io/robbertkl/ipv6nat:${versionNumber}"

        // archive image
        sh "docker save inomial.io/robbertkl/ipv6nat:${versionNumber} | gzip > ipv6nat-${versionNumber}.tar.gz"

        // tag and push if tests pass (as $revision and as latest)
        sh "docker push inomial.io/robbertkl/ipv6nat:${versionNumber}"
        sh "docker push inomial.io/robbertkl/ipv6nat:latest"
    }
    stage('Results') {
        currentBuild.displayName = versionNumber
        archive '*.tar.gz'
        // cleanup workspace
        step([$class: 'WsCleanup'])
    }
}
