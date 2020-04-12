#!groovy​

pipeline {
    agent { 
        node {
        label 'node-wrmsctst01'
    }
    }
    parameters {
        booleanParam(name: 'Refresh', defaultValue: false, description: 'Tylko odświerzenie konfiguracji Jenkinsa')
        choice(name: 'AKCJA', choices: ['Buduj','Usun'], description: 'Chcesz zbudować nową instancję czy ją sunąć?')
        string(name: 'INST_ID', defaultValue:'tylko-dla-usuwania', description:'Podaj ID instancji do usunięcia. <a  "target=_blank" href=https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#Instances:sort=statusChecks> Możesz to sprawdzić tutaj...</a>')
        string(name: 'INST_TAG', defaultValue:'msc_ds_worker1', description:'Podaj tag AWSowy opisujący rolę w swarmie dla budowanej instancji. <a  "target=_blank" href=https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#Instances:sort=statusChecks> Już otagowane instncje możesz sprawdzić tutaj...</a>')
        choice(name: 'INST_TYP', choices: ['t2.micro','t2.small'], description: 'Wybór typu instancji (wydajność + cena) <a "target=_blank" href=https://aws.amazon.com/ec2/instance-types/>Więcej informacji tutaj...</a>')
    }
   
    stages {
        stage('Read Jenkinsfile') {
            when {
                expression { params.Refresh == true }
            }
            steps {
                echo("Odświerzenie konfiguracji Jenkinsa.")        
            }
        }
        stage('Deploy AWS instances') {
            when {
                branch 'master'
                allOf{
                    expression { params.Refresh == false }
                    }
                }
            steps {
                withCredentials([
                    file(credentialsId: 'aws_ssh_keypair_2', variable: 'AWS_SSHFILE'),
                    file(credentialsId: 'aws_connect', variable: 'AWS_FILE')]) {
                    sh '''
                    /usr/local/bin/virtualenv msc-ansible
                    cd msc-ansible
                    . bin/activate
                    pip3 install ansible
                    ansible --version
                    . $AWS_FILE
                    cat $AWS_SSHFILE > ${WORKSPACE}/aws_ssh_keypair.pem
                    chmod 0600 ${WORKSPACE}/aws_ssh_keypair.pem
                    ansible-playbook ${WORKSPACE}/chnura_buduj.yml -i ec2_inventory.py -e instance_ids=\\\'${INST_ID}\\\' -e jaki_tag=\\\'${INST_TAG}\\\' --tags \"Prereqs, ${AKCJA}\"
                    rm ${WORKSPACE}/aws_ssh_keypair.pem
                    '''
                }
            }
        }
    }
}
