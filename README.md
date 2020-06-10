# sam-building-custom-runtimes-java

This project is a sample about how to use [AWS SAM Building Custom Runtimes feature](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/building-custom-runtimes.html) to build a java8 maven project.
I have created this repo to help Java developers, since I couldn't find a reference specific for maven projects.

By default AWS SAM cli build command will perform an `mvn clean install` ([reference here](https://github.com/awslabs/aws-lambda-builders/blob/develop/aws_lambda_builders/workflows/java_maven/maven.py#L28)).
It may not be enough for some use cases which we need custom maven commands, for example to enable/disable some maven plugin.
More information about SAM build flow could be [found here](https://github.com/awslabs/aws-lambda-builders/blob/develop/aws_lambda_builders/workflows/java_maven/DESIGN.md).

> To build a custom runtime, declare the Metadata resource attribute with a `BuildMethod: makefile` entry. You provide a custom makefile, where you declare a build target of the form `build-function-logical-id` that contains the build commands for your runtime

```yaml
Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function
    Metadata:
      BuildMethod: makefile
    ...
```

Make sure to create a custom `makefile` and add it in the **same folder** than your lambda's code.

```makefile
build-HelloWorldFunction:
	@ mvn clean install # Add here your custom commands
	@ mvn dependency:copy-dependencies -DincludeScope=compile
	@ cp -rf ./target/classes/* $(ARTIFACTS_DIR)/
	@ mkdir -p $(ARTIFACTS_DIR)/lib
	@ cp -rf ./target/dependency/* $(ARTIFACTS_DIR)/lib
	@ rm -rf ./target
```

Also make sure to add all those steps, since SAM requires them to be able to run it locally. From AWS doc:

> Your makefile is responsible for compiling the custom runtime if necessary, and copying the build artifacts into the proper location required for subsequent steps in your workflow.


## References:

- [What is SAM?](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html)
- [AWS SAM Building Custom Runtimes feature](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/building-custom-runtimes.html)
- [AWS Lambdas builders repo](https://github.com/awslabs/aws-lambda-builders)
- [AWS SAM Cli repo](https://github.com/awslabs/aws-sam-cli)
