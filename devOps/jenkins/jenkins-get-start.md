#jenkins-get-quick start.md

## Jenkinsfile 

***Benefits***

Code review/iteration on the Pipeline
Audit trail for the Pipeline
Single source of truth [2] for the Pipeline, which can be viewed and edited by multiple members of the project.


Jenkins show SCM
---

Jenkinsfile :
---
As discussed in the Defining a Pipeline in SCM, a Jenkinsfile is a text file that contains the definition of a Jenkins Pipeline and is checked into source control. Consider the following Pipeline which implements a basic three-stage continuous delivery pipeline.


Jenkinsfile (Declarative Pipeline)

```
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
```
## use credentials

配置证书和认证主要是为了Pipeline项目（与第三方应用交互）
Note: Credentials Binding plugin.


Credentials stored in Jenkins can be used:

    anywhere applicable throughout Jenkins (i.e. global credentials),
    by a specific Pipeline project/item (read more about this in the Handling credentials section of Using a Jenkinsfile),

    by a specific Jenkins user (as is the case for Pipeline projects created in Blue Ocean).

Jenkins can store the following types of credentials:

    Secret text - a token such as an API token (e.g. a GitHub personal access token),

    Username and password - which could be handled as separate components or as a colon separated string in the format username:password (read more about this in Handling credentials),

    Secret file - which is essentially secret content in a file,

    SSH Username with private key - an SSH public/private key pair,

    Certificate - a PKCS#12 certificate file and optional password, or

    Docker Host Certificate Authentication credentials.

Credential security







