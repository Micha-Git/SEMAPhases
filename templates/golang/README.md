# GoLang Pipeline Template

This project template is tailored for Golang applications and is primed for both local and cluster deployments. For comprehensive instructions on integrating the deployment artifacts into your existing project and customizing them, refer to [Best Practice: Applying the DevOps Templates](../../pages/be_applying_the_cicd_templates.md).  

**Note: The templates are written for GitLab using pipeline fragments.**

## Template-Specific Customization

### Pipeline Custom Variables

Custom variables regarding the build and test stage can be set in the `variables.yaml` file.

| Parameter | Description | Default |
| ------ | ------ | ------ |
| GO_BUILD_OPTIONS | This variable defines arguments for `go build`. | "" |
| GO_EXPORT_BUILD_OPTIONS | This variable defines environment variables used during build time. | "CGO_ENABLED=0 GOOS=linux GOFLAGS=-ldflags=-extldflags=-static" |
| GO_EXPORT_TEST_OPTIONS | This variable defines environment variables for go test. | "CGO_ENABLED=0 GOOS=linux" |


