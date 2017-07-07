pipeline {
  agent {
    label 'x86_64'
  }

  triggers {
    pollSCM("H */2 * * *")
  }

  stages {
    stage('Compile kernel') {
      steps {
        parallel(
          'arm': {
            dir("arm") {
              dir("release") {
                deleteDir()
              }
              dir("build") {
                deleteDir()
              }
              dir("kernel") {
                checkout([
                  $class: 'GitSCM',
                  poll: true,
                  branches: [[name: 'linux-4.11.y']],
                  extensions: [
                    [$class: 'CheckoutOption', timeout: 30],
                    [$class: 'CloneOption', timeout: 60],
                    [$class: 'CleanBeforeCheckout']
                  ],
                  userRemoteConfigs: [[url: "git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git" ]]
                ])
              }
              sh "make -C '${WORKSPACE}' linux TARGET_ARCH=arm KERNEL_SRC_DIR='${WORKSPACE}/arm/kernel' BUILD_DIR='${WORKSPACE}/arm/build' RELEASE_DIR='${WORKSPACE}/arm/release'"
            }
          },
          'x86_64': {
            dir("x86_64") {
              dir("release") {
                deleteDir()
              }
              dir("build") {
                deleteDir()
              }
              dir("kernel") {
                checkout([
                  $class: 'GitSCM',
                  poll: true,
                  branches: [[name: 'linux-4.11.y']],
                  extensions: [
                    [$class: 'CheckoutOption', timeout: 30],
                    [$class: 'CloneOption', timeout: 60],
                    [$class: 'CleanBeforeCheckout']
                  ],
                  userRemoteConfigs: [[url: "git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git" ]]
                ])
              }
              sh "make -C '${WORKSPACE}' linux TARGET_ARCH=x86_64 KERNEL_SRC_DIR='${WORKSPACE}/x86_64/kernel' BUILD_DIR='${WORKSPACE}/x86_64/build' RELEASE_DIR='${WORKSPACE}/x86_64/release'"
            }
          },
          'arm64': {
            dir("arm64") {
              dir("release") {
                deleteDir()
              }
              dir("build") {
                deleteDir()
              }
              dir("kernel") {
                checkout([
                  $class: 'GitSCM',
                  poll: true,
                  branches: [[name: 'linux-4.11.y']],
                  extensions: [
                    [$class: 'CheckoutOption', timeout: 30],
                    [$class: 'CloneOption', timeout: 60],
                    [$class: 'CleanBeforeCheckout']
                  ],
                  userRemoteConfigs: [[url: "git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git" ]]
                ])
              }
              sh "make -C '${WORKSPACE}' linux TARGET_ARCH=arm64 KERNEL_SRC_DIR='${WORKSPACE}/arm64/kernel' BUILD_DIR='${WORKSPACE}/arm64/build' RELEASE_DIR='${WORKSPACE}/arm64/release'"
            }
          }
        )
      }
    }
    stage('Archive kernels') {
      steps {
        archive includes: '*/release/**'
      }
    }
  }
}

