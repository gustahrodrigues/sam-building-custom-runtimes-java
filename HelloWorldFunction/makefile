build-HelloWorldFunction:
	@ mvn clean install # Add here your custom commands
	@ mvn dependency:copy-dependencies -DincludeScope=compile
	@ cp -rf ./target/classes/* $(ARTIFACTS_DIR)/
	@ mkdir -p $(ARTIFACTS_DIR)/lib
	@ cp -rf ./target/dependency/* $(ARTIFACTS_DIR)/lib
	@ rm -rf ./target
