# Migration Guide: CodeMagic to GitHub Actions

This guide explains how to migrate from CodeMagic to GitHub Actions for building the JIT Enabler iOS app.

## Why Migrate?

If you're experiencing issues with CodeMagic builds, such as the "Invalid encryption key" error, migrating to GitHub Actions can provide:
- Better integration with your GitHub repository
- More control over the build process
- Potentially lower costs
- Elimination of team-specific encryption key issues

## Simplified Approach

This migration takes a simplified approach:
- No code signing required (builds for simulator)
- No GitHub secrets needed (values hardcoded)
- No TestFlight deployment (artifact only)
- Simple IPA creation (zip file of the app)

## Migration Steps

### 1. Add the Build Tools

The migration includes several build tools:

1. **Makefile** (`Makefile.ios`): Provides a structured build process with various targets
2. **Build Script** (`build-ios.sh`): Offers a simplified way to run the Makefile
3. **GitHub Actions Workflow** (`.github/workflows/ios-app-build.yml`): Orchestrates the build process

### 2. Remove CodeMagic Configuration

You can now remove or disable the CodeMagic configuration:

1. Either delete the `codemagic.yaml` file
2. Or keep it for reference but disable the workflow in the CodeMagic dashboard

### 3. Trigger Your First Build

1. Push your changes to GitHub
2. Go to the Actions tab in your repository
3. You should see the "Build iOS App" workflow running
4. Check the logs for any issues

### 4. Download the Build

After the workflow completes:
1. Go to the Actions tab in your GitHub repository
2. Click on the completed workflow run
3. Scroll down to the "Artifacts" section
4. Download the "ios-app-build" artifact which contains the IPA file

## Build Process Details

### Makefile Targets

The `Makefile.ios` includes several useful targets:

- `clean`: Removes build artifacts
- `setup`: Sets up environment variables
- `build-simulator`: Builds the app for iOS simulator
- `create-simulator-ipa`: Creates an IPA from the simulator build
- `build-ipa`: Main target that builds the IPA file
- `info`: Shows build information

### Build Script

The `build-ios.sh` script:

1. Checks if Xcode is installed
2. Verifies the Xcode project exists
3. Runs the Makefile targets in sequence
4. Verifies the IPA was created successfully
5. Provides detailed output and error handling

### GitHub Actions Workflow

The workflow:

1. Sets up the macOS build environment
2. Makes the build script executable
3. Runs the build script
4. Verifies the IPA file structure
5. Uploads the build as a GitHub Actions artifact

## Troubleshooting

### Build Failures

If the build fails:
- Check the GitHub Actions logs for specific error messages
- Try building locally using the Makefile or script
- Verify that your Xcode project builds locally without issues
- Ensure all dependencies are correctly specified

### Simulator Build Limitations

Note that building for the simulator has some limitations:
- The app will only run on iOS simulators, not real devices
- Some device-specific features may not work in the simulator
- The IPA is not a standard IPA file but a zip of the app bundle

## Future Enhancements

If you need to deploy to real devices or TestFlight in the future:
1. Set up code signing with certificates and provisioning profiles
2. Configure App Store Connect API access
3. Update the workflow to build for real devices
4. Add TestFlight deployment steps

## Need Help?

If you encounter issues during migration:
1. Check the GitHub Actions logs for detailed error messages
2. Try building locally using the provided tools
3. Review Apple's documentation on Xcode builds
4. Consider consulting with an iOS CI/CD specialist if problems persist