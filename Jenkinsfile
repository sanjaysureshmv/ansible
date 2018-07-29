pipeline{
    agent any
    stages{
        stage('Pull the changes from repo to slave'){
            agent{
                node{
                    label 'ansible'
                }
            }
            steps{

                git url: 'https://github.com/sanjaysureshmv/ansible.git'
                echo "workspace is ${WORKSPACE}"
            }


        }

        stage('Run the playbook'){
            agent{
                node{

                    label 'ansible'
                }
            }
            steps{

                ansiblePlaybook becomeUser: null, colorized: true, inventory: '${WORKSPACE}/myinventory', playbook: '${WORKSPACE}/imprv_lamp.yaml', sudoUser: null

            }
        }
    }
}