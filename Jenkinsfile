node {
  def app

  stage('Clone repository'){
    checkout scm
  }

  stage('Build image')
  {
    app = docker.build("ravikants781434/test_app");
  }

  stage('Test image')
  {
    app.inside
    {
      sh 'echo "Tests passed"'
    }
  }

  stage('Push image')
  {
    docker.withRegistry("https://registry.hub.docker.com/", "dockerhub")
    {
      app.push("${env.BUILD_NUMBER}")
    }
  }

  stage('Trigger ManifestUpdate')
  {
      echo "triggring updatemanifest job"
      build job: 'updatemanifest', parameter: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
  }
}