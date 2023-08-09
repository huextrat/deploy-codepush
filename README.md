# Deploy CodePush

This action deploy a [CodePush](https://learn.microsoft.com/en-us/appcenter/distribution/codepush/) to AppCenter using the AppCenter CLI.

You will need a `Full Access` [API token](https://learn.microsoft.com/en-us/appcenter/api-docs/#creating-an-app-center-user-api-token) from AppCenter.

> - To delete a deployment track on AppCenter, please use [huextrat/delete-codepush](https://github.com/marketplace/actions/delete-codepush)

# Usage

<!-- start usage -->
```yaml
- uses: huextrat/deploy-codepush@v1.0.2
  with:
    # Required, as the AppCenter CLI requires authentication
    # Make it secure, using GitHub secrets for instance
    # Example: ${{ secrets.APPCENTER_API_TOKEN }}
    token: ''

    # Required, as the AppCenter CLI requires an app name
    # Format: <ownerName>/<appName>
    # Example: huextrat/MyApp
    app: ''

    # Deployment track you want to release the OTA to
    # If the track doesn't exist, it will be created
    # Default: Staging
    deploymentTrack: ''

    # Version of the app you want to release the OTA to
    # This is not required, as the AppCenter CLI will put the current version of the app as default
    # Still, it's highly recommended to specify the version you want to release the OTA to
    # You can use the semver format (https://learn.microsoft.com/en-us/appcenter/distribution/codepush/cli#target-binary-version-parameter)
    # Example: >=1.0.0 <1.2.0
    targetBinaryVersion: ''

    # Provide an optional changelog for the OTA
    description: ''

    # Indicate the percentage of users that are eligible to receive this update
    # Default: 100
    rollout: ''

    # Path to the directory containing the CodePush private key
    # This is not required, but highly recommended to code sign the OTA
    # Please read: https://learn.microsoft.com/en-us/appcenter/distribution/codepush/cli#code-signing
    privateKeyPath: ''

    # Relative path to the iOS project file (.pbxproj)
    # This is not required, as the AppCenter CLI will try to find the project file by itself
    xcodeProjectFile: ''

    # Name of the build configuration to use on iOS
    # This is iOS only to override the default configuration used by Xcode
    # Example: Staging_Release
    buildConfigurationName: ''
```
<!-- end usage -->

## Scenario

- AppCenter Android app name is: `huextrat/MyApp-Android`
- AppCenter iOS app name is: `huextrat/MyApp-iOS`


- [Minimal configuration](#Minimal-configuration)
- [Common configuration](#Common-configuration)

### Minimal configuration

#### Android
```yaml
- uses: huextrat/deploy-codepush@v1.0.2
  with:
    token: ${{ secrets.APP_CENTER_API_KEY }}
    app: "huextrat/MyApp-Android"
```

#### iOS
```yaml
- uses: huextrat/deploy-codepush@v1.0.2
  with:
    token: ${{ secrets.APP_CENTER_API_KEY }}
    app: "huextrat/MyApp-iOS"
    buildConfigurationName: "Staging_Release"
```

### Common configuration

> - `targetBinaryVersion` should be dynamic and not hardcoded to a specific version
> - `deploymentTrack` will depend on your workflow and your needs, remember that if it doesn't exist it will be created

#### Android
```yaml
- uses: huextrat/deploy-codepush@v1.0.2
  with:
    token: ${{ secrets.APP_CENTER_API_KEY }}
    app: "huextrat/MyApp-Android"
    deploymentTrack: "Production"
    targetBinaryVersion: ">=1.0.0"
    privateKeyPath: "./keys/CodePush.pem"
```

#### iOS
```yaml
- uses: huextrat/deploy-codepush@v1.0.2
  with:
    token: ${{ secrets.APP_CENTER_API_KEY }}
    app: "huextrat/MyApp-iOS"
    deploymentTrack: "Production"
    buildConfigurationName: "Production_Release"
    targetBinaryVersion: ">=1.0.0"
    privateKeyPath: "./keys/CodePush.pem"
```

> - More information about code signing [here](https://learn.microsoft.com/en-us/appcenter/distribution/codepush/cli#code-signing)
