pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "nadamankai/nodejs-app"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build("${env.DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    dockerImage.inside {
                         // Set execute permissions for node_modules/.bin
                        sh 'chmod +x node_modules/.bin/mocha'
                        sh 'npm test'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                    if [ ! -f "${KUBECTL_PATH}" ]; then
                        curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
                        chmod +x ./kubectl
                        mv ./kubectl ${KUBECTL_PATH}
                    fi
                    '''
                    kubeconfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUU2RENDQXRDZ0F3SUJBZ0lRY0FITlMrejBFU0JodFRVWWdWS0JEVEFOQmdrcWhraUc5dzBCQVFzRkFEQU4KTVFzd0NRWURWUVFERXdKallUQWdGdzB5TkRBME1ETXhOVE15TXpCYUdBOHlNRFUwTURRd016RTFOREl6TUZvdwpEVEVMTUFrR0ExVUVBeE1DWTJFd2dnSWlNQTBHQ1NxR1NJYjNEUUVCQVFVQUE0SUNEd0F3Z2dJS0FvSUNBUUNwCnVsbHRoRndCenprNE8wV0N2SUw0ZEV3eTV6QzJJN2N3bVlNNkhTbm9ESEZHbzAxenpDMFJiSWNOS3dKOTZNeDMKMDFyU29rNmprUHJ4Vlppa2orQndYRDc0d0NpNWQ0Z1lSMXY2TFhQY05EZ2UyRWFlRTVHZnlPVW5pRjMxRlZYYwp0aWxhZy9lTUdRb3NRZDBkQ1pYZ280Wkk3V1BjRVdXMGtlYXA4N2x4MFd4NVA3VXJETURHbG1mWitqY0ZpS2RrClpKQU5mNExhQXlXNHBpTVdETGlKd0k1T0NMeWJycTRkUDZETHNZUUlPOUQwOXR3M0M2OUI3amVUQnR6WVRvNXcKZ1VZeVlaMjdiQ3lDQVFNc2JmRjYwNFVUNFl3Znd0emtEbytlK0dLRndGNHQ1bjhOMzh0OXd6N3I3ZTc1MVlTbAp5NTUrM0oxSS9xQXMzMXBOUkpVYTBNSCt0RVRCekxkdm9SUmZjMTNSN2ttS3d0MVRLYW41bU0xeFFCUFIzREdFCkk2T0ZXMVplNWlVM3hFVm82ekc3UG9oQUJ5eFhSaFVwWDRqRFlIdmdtckZKa1ZicmpkR0xxRXowbzM5bjR6U04KSE0yRVRwU1EyWWkwN1dPblpkSVV2RXlSY2JUY2haTmw0UnBST2FaMis2K0cvak5aOXJ5aXllUC9jejJvRDJHZQpUUWdqNlVCalhySmd1cFV1SE9sWFdEcDRLQmRwajNCazVqMlN4MGJITWZseXpoSkh0ZXFqR3JuUFo4VnZpeFQ1CmVEVXhrSEF0Q3hlWitWRkx0TVV3c1k1U2ZmZlVKVkREb1dhbXZTMzJ1aW91cG1kZ3FRVWdlSDB3dnBoRnB5MFcKeFVMU0NWQXJRaHduemlqSGQ0NFFCcnFSWnFjZHp1b3lQUTIxRENTaHp3SURBUUFCbzBJd1FEQU9CZ05WSFE4QgpBZjhFQkFNQ0FxUXdEd1lEVlIwVEFRSC9CQVV3QXdFQi96QWRCZ05WSFE0RUZnUVVKUE1qZlhBVWo2a2Z4MklwCkhvVndGTWhudnRZd0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dJQkFGTW9hN2F2OHZNbjJYeks5YU9iZ3pCSVc1TmIKUjAzSGNnLzJFUThWbE53elBFSHpwQUJZdmRyUW1NeTZLWUd6UG9LRWNCTjVObGRlL0xGOGtZTDlRSXdic2dMbAorekVmdFlzRXllcTNSb0xKMGVWY1VkMXl4NGlHeHA3YkVrU1JleWtOY1IxVzd3bDNMT3NoNmtBMFdWSDBJNjN3Cnlqa1ord21KTW52ZWRBMXBlMlZSQWZEeEs1ckNFQ0w2SkhpSHkyMDRTKzV2T3V0SjdPUjJRNWoyVkpucFVZWXkKZjJ4VHFVbUFQcWlyaUt1K0hNL2NPeTJBQzhKNVR5T0hLS1dIaFpIMU05K3U2Rjk0dkw1NzYvdDhIcDJLMEgwRApWQWdFWTgrYkE1azdHWXpKczlTU09yZTNjYUFsdm0wOWxmZllnWTRFejVpdUNpQjYzUWJyUU9rZ3JlN0lWKzg4CjFJR2Z6cjdhOGwwN0hQZmVvSm9LWHJMMTF4NlRVMTlaVWRxMjBGUk5DTUZtVmQ2UkNxR0ZPUGE5bitHWXpxTHcKNS9lZXFjRmJONnZFU3Z1VW1yWFF3OUJWYnFTbk1WL0ZOcHE1OW1GdXFtclREckEzdDFlUU5xekxjR1VRa2FGWAp5R05nYkhUVjRiNVd4TFRtUDJ3NnZSOUF2VlYvWWJxckQ3Q3Foc01RVVNBempZbnRzdEZTNjlpZ0R1eFlaTXpjClhnR3VnejhEdHdkOVJhOEk0cjl5S3JSRjVDaEdPK3JjRlRVTTBaNGNwdUk3dU5PSXpGVzZvbWN4RWt5VWVnK1kKV0tOYXBQcFZSOE9CWG1lakZpZ1hkakZCZ3FqZXpJWlRCREtyWjZyME8rdnBldTJNMWtJQURybGlsQSs2eC9GaApBYmlGUkt4dlFzNmozNHIzCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K', credentialsId: 'kubeconfig', serverUrl: 'https://172.23.64.205:8443') {
                     sh "${KUBECTL_PATH} apply -f kubernetes-deployment.yaml"
                }
                }
            }
        }
    }
}