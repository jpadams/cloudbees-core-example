podTemplate(label: 'twistlock-example-builder', 
  containers: [
    containerTemplate(
      name: 'alpine',
      image: 'twistian/alpine:latest',
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

    stage ('Pull image') { 
      container('alpine') {
        sh """
        curl --unix-socket /var/run/docker.sock -X POST "http:/v1.24/images/create?fromImage=nginx:stable-alpine"
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
                    image: 'nginx:stable-alpine',
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
                    image: 'nginx:stable-alpine',
                    key: '',
                    logLevel: 'true',
                    timeout: 10
    }
  }
}
