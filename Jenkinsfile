ansible_server_private_ip="<ansible-server-private-ip>"
grafana_server_private_ip="<grafana-server-private-ip>"

pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Gerardbulky/install-grafana-usx-ansible.git'
            }
        }
        stage('Sending Dockerfile to Ansible server') {
            steps {
                sshagent(['ansible-server']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${ansible_server_private_ip}"
                    sh "scp /var/lib/jenkins/workspace/grafana-automate-process/* ubuntu@${ansible_server_private_ip}:/home/ubuntu"
                }
            }
            
        }
        stage('Installing grafana using ansible'){
            steps {
                sshagent(['ansible-server']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${ansible_server_private_ip} cd /home/ubuntu/"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${ansible_server_private_ip} ansible -m ping ${grafana_server_private_ip}"
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${ansible_server_private_ip} ansible-playbook ansible-playbook.yml"
                }
            }
        }
        
    }
    
}
