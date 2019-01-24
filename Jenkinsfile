podTemplate(label: 'twistlock-example-builder', 
  containers: [
    containerTemplate(
      name: 'builder',
      image: 'docker',
      command: 'cat',
      ttyEnabled: true
    ),
  ],
  volumes: [ 
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'), 
  ]
)
{
  node ('twistlock-example-builder') {

    stage ('Build image') { 
      container('builder') {
        sh """
        mkdir build
        cd build
        echo 'FROM alpine:latest' > Dockerfile
        docker build -t myalpine:latest .
        """
      }
    }

    stage ('Twistlock scan') { 
        twistlockScan ca: '',
                    cert: '',
                    compliancePolicy: 'critical',
                    dockerAddress: 'unix:///var/run/docker.sock',
                    gracePeriodDays: 0,
                    ignoreImageBuildTime: true,
                    image: 'myalpine:latest',
                    key: '',
                    logLevel: 'true',
                    policy: 'warn',
                    requirePackageUpdate: false,
                    timeout: 10
    }

    stage ('Twistlock publish') {
        twistlockPublish ca: '',
                    cert: '',
                    dockerAddress: 'unix:///var/run/docker.sock',
                    ignoreImageBuildTime: true,
                    image: 'myalpine:latest',
                    key: '',
                    logLevel: 'true',
                    timeout: 10
    }
  }
}
