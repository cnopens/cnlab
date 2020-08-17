# Jenkins-Pipeline-Environment-Variables.md

1. 使用env-vars.html
${YOUR_JENKINS_HOST}/env-vars.html

2. Using shell command
Jekinsfile

```
pipeline {
    agent any

    stages {
        stage("Env Variables") {
            steps {
                sh "printenv | sort"
            }
        }
    }
}
```

Reading environment variables

You can access environment variables in pipeline steps through the env object, e.g., env.BUILD_NUMBER will return the current build number. You can also use a shorthand version BUILD_NUMBER, but in this variant may be confusing to some users - it misses the context that the BUILD_NUMBER comes from the environment variable.

Jenkinsfile

```
pipeline {
    agent any

    stages {
        stage("Env Variables") {
            steps {
                echo "The build number is ${env.BUILD_NUMBER}"
                echo "You can also use \${BUILD_NUMBER} -> ${BUILD_NUMBER}"
                sh 'echo "I can access $BUILD_NUMBER in shell command as well."'
            }
        }
    }
```

Setting environment variables

The environment variables can be set declaratively using environment { } block, imperatively using env.VARIABLE_NAME, or using withEnv(["VARIABLE_NAME=value"]) {} block.

```
pipeline {
    agent any

    environment {
        FOO = "bar"
    }

    stages {
        stage("Env Variables") {
            environment {
                NAME = "Alan"
            }

            steps {
                echo "FOO = ${env.FOO}"
                echo "NAME = ${env.NAME}"

                script {
                    env.TEST_VARIABLE = "some test value"
                }

                echo "TEST_VARIABLE = ${env.TEST_VARIABLE}"

                withEnv(["ANOTHER_ENV_VAR=here is some value"]) {
                    echo "ANOTHER_ENV_VAR = ${env.ANOTHER_ENV_VAR}"
                }
            }
        }
    }
}
```

Overriding environment variables

Jenkins Pipeline supports overriding environment variables. There are a few rules you need to be aware of.

    -The withEnv(["env=value]) { } block can override any environment variable.

    -The variables set using environment {} block cannot be overridden using imperative env.VAR = "value" assignment.

    -The imperative env.VAR = "value" assignment can override only environment variables created using imperative assignment.

Here is an exemplary Jenkinsfile that shows all three different use cases.

Jenkinsfile
---
```
pipeline {
    agent any

    environment {
        FOO = "bar"
        NAME = "Joe"
    }

    stages {
        stage("Env Variables") {
            environment {
                NAME = "Alan" // overrides pipeline level NAME env variable
                BUILD_NUMBER = "2" // overrides the default BUILD_NUMBER
            }

            steps {
                echo "FOO = ${env.FOO}" // prints "FOO = bar"
                echo "NAME = ${env.NAME}" // prints "NAME = Alan"
                echo "BUILD_NUMBER =  ${env.BUILD_NUMBER}" // prints "BUILD_NUMBER = 2"

                script {
                    env.SOMETHING = "1" // creates env.SOMETHING variable
                }
            }
        }

        stage("Override Variables") {
            steps {
                script {
                    env.FOO = "IT DOES NOT WORK!" // it can't override env.FOO declared at the pipeline (or stage) level
                    env.SOMETHING = "2" // it can override env variable created imperatively
                }

                echo "FOO = ${env.FOO}" // prints "FOO = bar"
                echo "SOMETHING = ${env.SOMETHING}" // prints "SOMETHING = 2"

                withEnv(["FOO=foobar"]) { // it can override any env variable
                    echo "FOO = ${env.FOO}" // prints "FOO = foobar"
                }

                withEnv(["BUILD_NUMBER=1"]) {
                    echo "BUILD_NUMBER = ${env.BUILD_NUMBER}" // prints "BUILD_NUMBER = 1"
                }
            }
        }
    }
}
```

Storing Boolean values in environment variables

There is one popular misconception when it comes to using environment variables. Every value that gets stored as an environment variable gets converted to a String. When you store boolean’s false value, it gets converted to "false". If you want to use that value in the boolean expression correctly, you need to call "false".toBoolean() method.[1]

Jenkinsfile 

```
pipeline {
    agent any

    environment {
        IS_BOOLEAN = false
    }

    stages {
        stage("Env Variables") {
            steps {
                script {
                    if (env.IS_BOOLEAN) {
                        echo "You can see this message, because \"false\" String evaluates to Boolean.TRUE value"
                    }

                    if (env.IS_BOOLEAN.toBoolean() == false) {
                        echo "You can see this message, because \"false\".toBoolean() returns Boolean.FALSE value"
                    }
                }
            }
        }
    }
}
```

Capturing sh command output in the env variable

You can also capture output of a shell command as an environment variable. Keep in mind that you need to use sh(script: 'cmd', returnStdout:true) format to force sh step[2] to return an output so it can be captured and stored in a variable. Also, if you want to avoid capturing the new line character that is usually present in the shell command output, call trim() method to remove all whitespace from the beginning and the end of the captured output.

```
pipeline {
    agent any

    environment {
        LS = "${sh(script:'ls -lah', returnStdout: true).trim()}"
    }

    stages {
        stage("Env Variables") {
            steps {
                echo "LS = ${env.LS}"
            }
        }
    }
}
```

Wrapping sh step with the Groovy string is only needed when you capture command’s output in the environment block of the declarative pipeline. If you don’t do it that way, the pipeline syntax validation fails instantly, saying that it expects string value on the right side of the assignment. When you capture the output inside the script block, however, you don’t have to call sh from the inside of the Groovy string. The following code will work just fine in this case.

```
pipeline {
    agent any

    stages {
        stage("Env Variables") {
            steps {
                script {
                    env.LS = sh(script:'ls -lah', returnStdout: true).trim()
                    echo "LS = ${env.LS}"
                    // or if you access env variable in the shell command
                    sh 'echo $LS'
                }
            }
        }
    }
}
```
