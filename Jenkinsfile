elifeLibrary {
    def commit
    stage 'Checkout', {
        checkout scm
        commit = elifeGitRevision()
    }

    stage 'Build image', {
        sh './build_container.sh'
    }

    stage 'Run tests', {
        elifeLocalTests './project_tests.sh'
    }

    elifeMainlineOnly {
        stage 'Push image', {
            def image = DockerImage.elifesciences(this, 'sciencebeam', 'latest')
            image.tag(commit).push()
            image.push()
        }

        stage 'Downstream', {
            build job: '/dependencies/dependencies-sciencebeam-texture-update-sciencebeam', wait: false, parameters: [string(name: 'commit', value: commit)]
        }
    }
}
