# BRAVE BROWSER DEBLOAT GUIDE FOR MACOS

## Introduction

This guide will help you remove unnecessary features ("bloat") from Brave Browser on macOS, resulting in a cleaner, more privacy-focused, and less distracting browsing experience. The following features will be disabled:

- Brave Rewards (cryptocurrency rewards system)
- Brave Wallet (cryptocurrency wallet)
- Brave VPN (paid VPN service)
- Leo AI (AI assistant)

Optional - with # :
- Tor private browsing
- Incognito mode (replaced by Tor in Brave)

Why debloat Brave? While Brave is privacy-focused compared to Chrome, it comes with numerous cryptocurrency and AI features that most users don't need. These features add complexity to the UI, consume resources, and potentially introduce privacy concerns. This guide keeps Brave's core privacy features while removing the extra bloat.

Note: 
- This guide preserves the password manager functionality.
- After these edits your will see "This browser is managed by an organisation" in profiles, this is intended behaviour.

Disclaimer:
- This is software modification, it is explicitly NOT provided or endorsed by Brave or any other official affiliates.
- Data and software is provided "as is". Without warranty of any kind, express or implied, including but not limited to the warranties of mechantability, fitness for a particular purpose and noninfringement. In no event shall the authors or copyright holders of this guide/note be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection to the use of suggested modifications.
- Any changes you make to your system or your browser is entirely your responsibility.

## Note regarding Browser Updates

- I've had several questions and reports about the changes getting reversed after updates and the script being future-proof.
- After a browser update, the system-level managed preferences seem to get reversed.
- If you notice that the menu uptions have appeared again, feel free to just re-run the script with your desired options.

---

## Table of Contents

- [Introduction](#introduction)
- [Method 1: User-Level Settings](#method-1-user-level-settings-simple-but-less-effective)
- [Method 2: System-Level Managed Preferences](#method-2-system-level-managed-preferences-more-effective)
- [Verifying Changes](#verifying-changes)
- [Reverting Changes](#reverting-changes)
- [Settings Explanation](#settings-explanation)
- [Troubleshooting](#troubleshooting)
- [Conclusion](#conclusion)

---

## Method 1: User-Level Settings (Simple But Less Effective)

This method uses macOS's defaults command to modify Brave's preferences. It's easy to apply but may not be as comprehensive as Method 2.

1. Open Terminal (Applications > Utilities > Terminal)

2. Copy and paste the following commands (uncomment if you want optional edits):

```bash
# Disable Incognito Mode
# defaults write com.brave.Browser IncognitoModeAvailability -integer 1

# Disable Tor browsing
# defaults write com.brave.Browser TorDisabled -bool true

# Disable Leo AI
defaults write com.brave.Browser BraveAIChatEnabled -bool false
defaults write com.brave.Browser BraveLeoEnabled -bool false
defaults write com.brave.Browser BraveChatEnabled -bool false
defaults write com.brave.Browser BraveAIEnabled -bool false

# Disable Brave Wallet
defaults write com.brave.Browser BraveWalletDisabled -bool true
defaults write com.brave.Browser CryptoWalletEnabled -bool false

# Disable Brave Rewards
defaults write com.brave.Browser BraveRewardsDisabled -bool true

# Disable Brave VPN
defaults write com.brave.Browser BraveVPNDisabled -bool true
```

3. Restart Brave Browser for changes to take effect.

---

## Method 2: System-Level Managed Preferences (More Effective)

This method creates a system-wide policy file that takes precedence over user settings. It requires administrator privileges but is more effective at hiding unwanted features.

1. Open Terminal (Applications > Utilities > Terminal)

2. Copy and paste the following commands (you'll be prompted for your password, ):

```bash
# Create a directory for managed preferences if it doesn't exist
sudo mkdir -p /Library/Managed\ Preferences

# Create a managed preferences file for Brave with all the policies
cat << EOF | sudo tee /Library/Managed\ Preferences/com.brave.Browser.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>BraveAIChatEnabled</key>
    <false/>
    <key>BraveRewardsDisabled</key>
    <true/>
    <key>BraveVPNDisabled</key>
    <true/>
    <key>BraveWalletDisabled</key>
    <true/>
#   <key>IncognitoModeAvailability</key>
#   <integer>1</integer>
#   <key>TorDisabled</key>
#   <true/>
    <key>BraveLeoEnabled</key>
    <false/>
    <key>BraveChatEnabled</key>
    <false/>
    <key>BraveAIEnabled</key>
    <false/>
    <key>CryptoWalletEnabled</key>
    <false/>
</dict>
</plist>
EOF

# Set appropriate permissions
sudo chmod 644 /Library/Managed\ Preferences/com.brave.Browser.plist
```

3. Restart Brave Browser for changes to take effect.

---

## Verifying Changes

To verify that the settings were applied correctly:

1. For Method 1 (user-level settings), run:
   ```bash
   defaults read com.brave.Browser
   ```
   You should see all the modified settings in the output.

2. For Method 2 (system-level settings), check if the file exists:
   ```bash
   ls -la /Library/Managed\ Preferences/com.brave.Browser.plist
   ```

3. Visually verify in Brave Browser:
   - The Rewards icon should be gone from the address bar
   - The Wallet icon should be gone from the address bar
   - The VPN option should be gone from settings
   - Leo AI should be gone from the sidebar
   - Incognito mode should be unavailable (if applicable)
   - Tor private browsing should be unavailable (if applicable)

---

## Reverting Changes

If you need to revert these changes:

### To revert Method 1 (user-level settings):

```bash
# Revert Incognito Mode
defaults delete com.brave.Browser IncognitoModeAvailability

# Revert Tor browsing
defaults delete com.brave.Browser TorDisabled

# Revert Leo AI
defaults delete com.brave.Browser BraveAIChatEnabled
defaults delete com.brave.Browser BraveLeoEnabled
defaults delete com.brave.Browser BraveChatEnabled
defaults delete com.brave.Browser BraveAIEnabled

# Revert Brave Wallet
defaults delete com.brave.Browser BraveWalletDisabled
defaults delete com.brave.Browser CryptoWalletEnabled

# Revert Brave Rewards
defaults delete com.brave.Browser BraveRewardsDisabled

# Revert Brave VPN
defaults delete com.brave.Browser BraveVPNDisabled
```

### To revert Method 2 (system-level settings):

```bash
sudo rm /Library/Managed\ Preferences/com.brave.Browser.plist
```

Restart Brave Browser after reverting changes.

---

## Settings Explanation

Here's what each setting controls:

- **IncognitoModeAvailability (1)**: Disables Incognito browsing mode
- **TorDisabled (true)**: Disables Tor private browsing
- **BraveAIChatEnabled, BraveLeoEnabled, BraveChatEnabled, BraveAIEnabled (false)**: Disables Leo AI assistant in all its forms
- **BraveWalletDisabled, CryptoWalletEnabled (false)**: Disables the cryptocurrency wallet
- **BraveRewardsDisabled (true)**: Disables the Brave Rewards program
- **BraveVPNDisabled (true)**: Disables the Brave VPN service

---

## Troubleshooting

If some features are still visible after applying these settings:

1. **Try both methods**: Start with Method 1, then apply Method 2 if needed
2. **Clear browser data**: Go to Settings > Clear browsing data > Advanced and select "Cached images and files"
3. **Reset Brave**: As a last resort, you can reset Brave in Settings > Reset Settings
4. **Check for updates**: Make sure you're using the latest version of Brave Browser, as policy keys may change between versions

---

## Conclusion

You now have a debloated version of Brave Browser that focuses on its core privacy features without the cryptocurrency and AI add-ons. This provides a cleaner interface and potentially better performance while maintaining the core benefits of Brave.

If you enjoyed this guide, feel free to share it with others who might benefit from a streamlined browsing experience!

---
