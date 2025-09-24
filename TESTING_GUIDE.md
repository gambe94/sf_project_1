# 🚀 Salesforce Environment Action - Testing Guide

## Quick Start with gh-act

Since you've installed `act` via the GitHub CLI extension, here's how to test your workflow:

### Basic Testing Commands

```bash
# Navigate to your project
cd /home/gabor-priv/Development/app_container/sf_project_1

# Test the entire workflow
gh act

# Test specific job
gh act -j validate

# Test with specific event
gh act push
gh act pull_request
gh act workflow_dispatch

# Run with verbose output for debugging
gh act -v

# Use a specific platform/runner
gh act -P ubuntu-latest=catthehacker/ubuntu:act-latest
```

### Testing Configuration

Create `.actrc` in your project root for consistent settings:

```bash
# .actrc file
-P ubuntu-latest=catthehacker/ubuntu:act-latest
--artifact-server-path /tmp/artifacts
--env-file .env.local
```

### Environment Variables for Testing

Create `.env.local` for local testing:

```bash
# .env.local
GITHUB_TOKEN=your_github_token_here
NODE_ENV=test
CI=true

# Optional: Override inputs for testing
INPUT_NODE_VERSION=18
INPUT_SF_CLI_VERSION=latest
INPUT_CACHE_DEPENDENCIES=false
```

## Action Improvements Made

### 🎯 Enhanced User Experience

1. **Clear Step Names with Emojis**
   - ⏱️ Initialize Setup Timer
   - 🔍 Validate Configuration
   - 📦 Setup Node.js Environment
   - ⚡ Install Salesforce CLI
   - 🔌 Install Salesforce Plugins
   - 🧪 Final System Verification

2. **Better Error Messages**
   ```yaml
   # Before
   echo "❌ Invalid version"
   
   # After  
   echo "❌ Invalid Node.js version format: '${{ inputs.node-version }}'"
   echo "   Valid examples: 18, 18.17, 18.17.1, latest, lts"
   ```

3. **Enhanced Input Descriptions**
   - Added examples for each input
   - Explained what each plugin does
   - Clear guidance on when to use each option

### 📊 New Outputs

The action now provides these additional outputs:

- `plugins-failed`: List of plugins that failed to install
- `setup-time`: Total time taken for setup
- Enhanced descriptions for all outputs

### 🔍 Better Validation

- More descriptive error messages
- Configuration summary display
- Working directory validation
- Format validation with examples

### 📈 Progress Tracking

- Setup timer to track duration
- Step-by-step progress indicators
- Detailed verification checks
- Comprehensive final summary

## Testing Scenarios

### 1. Full Workflow Test
```bash
gh act -j validate -P ubuntu-latest=catthehacker/ubuntu:act-latest
```

### 2. Test with Cache Disabled
```bash
# Create .env.test
echo "INPUT_CACHE_DEPENDENCIES=false" > .env.test

# Run with test env
gh act --env-file .env.test
```

### 3. Test Plugin Installation
```bash
# Test with minimal plugins
echo "INPUT_PLUGINS=@salesforce/sfdx-scanner" > .env.minimal

gh act --env-file .env.minimal
```

### 4. Test Error Handling
```bash
# Test with invalid Node version
echo "INPUT_NODE_VERSION=invalid" > .env.error

gh act --env-file .env.error
```

## Expected Output

With the improvements, you should see output like:

```
⏱️ Starting Salesforce environment setup...
Started at: Tue Sep 24 16:30:15 UTC 2024

🔍 Validating input parameters...

📋 Configuration Summary:
  • Node.js version: 18
  • SF CLI version: latest
  • Working directory: .
  • Cache dependencies: true
  • Install dependencies: true
  • Plugins: @salesforce/sfdx-scanner,sfdx-git-delta

✅ All input parameters are valid!

📦 Setup Node.js Environment...
✅ Node.js Environment Ready:
   📦 Node.js: v18.20.8
   📦 npm: 10.8.2

⚡ Installing Salesforce CLI...
   Version: latest

📦 Installing latest version...
✅ Salesforce CLI installation completed successfully!

✅ Verifying Salesforce CLI installation...
✅ Salesforce CLI verified successfully!
   ⚡ Version: @salesforce/cli/2.15.9

🔌 Installing Salesforce CLI plugins...
📦 Installing plugin: @salesforce/sfdx-scanner
✅ Successfully installed: @salesforce/sfdx-scanner

⏱️ Setup completed in 45 seconds

🧪 Running comprehensive system verification...
✓ Testing core SF CLI commands...
   ✅ sf --help: OK
   ✅ sf version: OK
✓ Testing plugin system...
   ✅ Plugin system: OK

✅ All verification checks passed successfully!
```

## Troubleshooting

### Common Issues and Solutions

1. **Docker not running**
   ```bash
   sudo systemctl start docker
   # or
   docker --version
   ```

2. **Action not found**
   - Ensure `sf_ci_shared` is pushed to GitHub
   - Check the repository URL in workflow

3. **Permission errors**
   ```bash
   sudo chown -R $USER:$USER ~/.cache/act
   ```

4. **Out of disk space**
   ```bash
   docker system prune -f
   ```

### Debug Tips

```bash
# Maximum verbosity
gh act -v --dry-run

# Run specific steps only
gh act --list  # See available jobs/steps

# Use shell for debugging
gh act --shell

# Check what act sees
gh act --list
```

## Next Steps

1. **Test the improved action locally**
2. **Push changes to GitHub**
3. **Run actual workflow to verify**
4. **Customize inputs for your specific needs**

The action is now much more user-friendly with:
- ✅ Clear progress indicators
- ✅ Better error messages  
- ✅ Comprehensive logging
- ✅ Detailed summaries
- ✅ Timing information
- ✅ Enhanced validation