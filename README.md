# **📦 Offline NuGet Package Management Tutorial**  
### *How to Build .NET Projects Without Internet Access*  

This guide explains how to transfer **NuGet packages** from an online PC to an offline machine for building .NET projects.  

---

## **🔧 Prerequisites**  
✅ A PC with internet access (for downloading packages)  
✅ An offline PC (where you need to build the project)  
✅ USB drive or network share (to transfer packages)  
✅ .NET SDK installed on the offline PC  

---

## **🚀 Step 1: Gather NuGet Packages on Online PC**  
First, collect all required packages from your online machine.  

### **Method A: Using `dotnet restore` (Recommended)**
1. Open **Command Prompt/PowerShell** in your project folder.  
2. Run:  
   ```sh
   dotnet restore --packages .\offline-packages
   ```
   - This downloads all dependencies into an `offline-packages` folder.  

### **Method B: Copy Global NuGet Cache**  
If your project already restored packages before:  
1. Locate the **global NuGet cache**:  
   - Windows: `%userprofile%\.nuget\packages`  
   - Linux/macOS: `~/.nuget/packages`  
2. Copy the entire folder for transfer.  

---

## **📂 Step 2: Transfer Packages to Offline PC**  
Copy the `offline-packages` (or `.nuget/packages`) folder to:  
- **USB drive** → Move to the offline PC  
- **Network share** (if available)  
- **ZIP file** (if space is limited)  

Place it in a permanent location (e.g., `C:\NuGet-Offline`).  

---

## **⚙️ Step 3: Configure NuGet on Offline PC**  
Tell NuGet to **only use your local packages** (no internet).  

### **1. Create/Modify `NuGet.Config`**  
- In your **project/solution folder**, create `NuGet.Config` (if it doesn’t exist).  
- Add this content:  

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <!-- Clear default online sources (NuGet.org) -->
  <packageSources>
    <clear />
    <!-- Add local folder as the only source -->
    <add key="local-offline" value="C:\NuGet-Offline" />
  </packageSources>
  
  <!-- Optional: Set global packages folder -->
  <config>
    <add key="globalPackagesFolder" value="C:\NuGet-Offline" />
  </config>
</configuration>
```

### **2. Verify Configuration**  
Run in **Command Prompt/PowerShell**:  
```sh
dotnet nuget list source
```
✅ You should see only your `local-offline` source.  

---

## **🔨 Step 4: Build the Project Offline**  
Now, restore and build without internet:  

```sh
dotnet restore --no-cache
dotnet build
```
- `--no-cache` ensures NuGet doesn’t try fetching online packages.  

---

## **💡 Troubleshooting**  
❌ **"Package not found" error?**  
- Ensure the package versions in `offline-packages` match `*.csproj`.  
- Manually copy missing packages from the online PC.  

❌ **NuGet still tries to connect online?**  
- Delete `obj/` and `bin/` folders, then retry `dotnet restore`.  

---

## **📜 Conclusion**  
You now have a **fully offline NuGet setup**! This is useful for:  
- Secure/air-gapped environments  
- CI/CD pipelines with restricted internet  
- Reliable builds without dependency on NuGet.org 
