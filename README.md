# Subfinder-on-Windows

There are three ways to get Subfinder running on Windows. The prebuilt binary is the quickest.

**Option 1: Prebuilt binary (no Go needed)**

1. Go to the releases page at `github.com/projectdiscovery/subfinder/releases` and download `subfinder_<version>_windows_amd64.zip`.
2. Extract `subfinder.exe` somewhere permanent, such as `C:\Tools\subfinder\`.
3. Add that folder to your PATH. In PowerShell, this sets it for the current user:
   ```powershell
   [Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Tools\subfinder", "User")
   ```
4. Open a new terminal and check that it works:
   ```powershell
   subfinder -version
   ```

**Option 2: `go install`**

You need a recent version of Go installed (`winget install GoLang.Go`). Then run:
```powershell
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
```
The binary lands in `%USERPROFILE%\go\bin`. Make sure that folder is on your PATH.

**Option 3: PDTM (ProjectDiscovery's tool manager)**

This option makes sense if you also want nuclei, httpx, dnsx, and the other ProjectDiscovery tools:
```powershell
go install -v github.com/projectdiscovery/pdtm/cmd/pdtm@latest
pdtm -install subfinder
```

**Usage basics**

```powershell
subfinder -d example.com
subfinder -d example.com -all -recursive -o subs.txt
subfinder -dL domains.txt -silent | httpx -silent
```

**API keys (provider config)**

The first run creates a config file at:
```
C:\Users\<you>\.config\subfinder\provider-config.yaml
```
Add your keys there for sources like SecurityTrails, Shodan, Censys, VirusTotal, Chaos, and GitHub. Results improve a lot once keys are set:
```yaml
securitytrails:
  - YOUR_KEY
virustotal:
  - YOUR_KEY
github:
  - ghp_xxx
```
Run `subfinder -ls` to see which sources are available and which ones need keys.

**Common Windows gotchas**

- Defender or SmartScreen sometimes flags or quarantines the binary. If that happens, add an exclusion for the tools folder.
- If you get "not recognized as a cmdlet," your PATH change hasn't taken effect yet. Open a fresh terminal.
- Piping into other tools works fine in PowerShell. Use `| Out-File -Encoding utf8` instead of `>` if a downstream tool chokes on PowerShell's default UTF-16 output.
