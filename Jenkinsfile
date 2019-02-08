pipeline {
    agent { label 'master' }

        stages {
            stage('get jenkins-cli.jar') {       
                steps {
                    sh '''
                    if [ ! -f jenkins-cli.jar ]; then
                        echo "no jenkins-cli.jar found, downloading...!"
                        wget $hostname/jnlpJars/jenkins-cli.jar
                    fi
                    '''
                }
            }
            
            stage('update plugins') {       
                steps {
                    sh '''
                    UPDATE_LIST=$(java -jar jenkins-cli.jar -s $hostname -auth $auth list-plugins | grep -e ')$' | awk '{ print $1 }' ); 
                    if [ ! -z "${UPDATE_LIST}" ]; then 
                        echo Updating Jenkins Plugins: ${UPDATE_LIST}; 
                        java -jar jenkins-cli.jar -s $hostname -auth $auth install-plugin ${UPDATE_LIST};
                        java -jar jenkins-cli.jar -s $hostname -auth $auth safe-restart;
                    fi
                    '''
                }       
            }
    }
}