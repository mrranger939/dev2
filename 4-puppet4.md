## ⚙️ STEP 1: Install Puppet 7 Repository and Packages

```bash
wget https://apt.puppet.com/puppet7-release-focal.deb
sudo dpkg -i puppet7-release-focal.deb
sudo apt update
```

### 🎯 **Aim**

To design and implement a Puppet configuration using **Manifests, Modules, Classes, and Functions** to automate software installation and demonstrate **modularization and reusability**.

---

## ⚙️ **Step 1: Verify Puppet Installation**

```bash
sudo /opt/puppetlabs/bin/puppet --version
```

---

## ⚙️ **Step 2: Create Directories for Puppet Environment**

```bash
sudo mkdir -p /etc/puppetlabs/code/environments/production/manifests
sudo mkdir -p /etc/puppetlabs/code/environments/production/modules
```

---

## 📄 **Step 3: Create a Basic Manifest**

Create a simple file resource in `site.pp`:

```bash
sudo nano /etc/puppetlabs/code/environments/production/manifests/site.pp
```

Paste:

```puppet
file { '/tmp/hello_manifest.txt':
  ensure  => file,
  content => "Hello from Puppet Manifest!\n",
}
```

Apply it:

```bash
sudo /opt/puppetlabs/bin/puppet apply /etc/puppetlabs/code/environments/production/manifests/site.pp
```

Verify:

```bash
cat /tmp/hello_manifest.txt
```

✅ Output:

```
Hello from Puppet Manifest!
```

---

## 📦 **Step 4: Create a Puppet Module and Class**

Create a module directory structure:

```bash
sudo mkdir -p /etc/puppetlabs/code/environments/production/modules/sample_module/manifests
sudo mkdir -p /etc/puppetlabs/code/environments/production/modules/sample_module/files
```

Create the class file:

```bash
sudo nano /etc/puppetlabs/code/environments/production/modules/sample_module/manifests/init.pp
```

Paste:

```puppet
class sample_module {
  file { '/tmp/hello_module.txt':
    ensure  => file,
    content => "Hello from Puppet Module Class!\n",
  }
}
```

---

## 📘 **Step 5: Include the Module in Manifest**

Edit your main manifest:

```bash
sudo nano /etc/puppetlabs/code/environments/production/manifests/site.pp
```

Paste:

```puppet
# site.pp
include sample_module
```

Apply configuration:

```bash
sudo /opt/puppetlabs/bin/puppet apply /etc/puppetlabs/code/environments/production/manifests/site.pp
```

Verify:

```bash
cat /tmp/hello_module.txt
```

✅ Output:

```
Hello from Puppet Module Class!
```

---

## ⚙️ **Step 6: Use a Puppet Function / Variable**

Edit the class to include a **function/variable**:

```bash
sudo nano /etc/puppetlabs/code/environments/production/modules/sample_module/manifests/init.pp
```

Paste:

```puppet
class sample_module {
  $greeting = "Hello from Puppet Function!"
  file { '/tmp/hello_function.txt':
    ensure  => file,
    content => $greeting,
  }
}
```

Apply:

```bash
sudo /opt/puppetlabs/bin/puppet apply /etc/puppetlabs/code/environments/production/manifests/site.pp
```

Verify:

```bash
cat /tmp/hello_function.txt
```

✅ Output:

```
Hello from Puppet Function!
```

---

## 💡 **Step 7 (Optional): Automate Package Installation**

If you want to show **software automation** (like Apache):

```puppet
class sample_module {
  package { 'apache2':
    ensure => installed,
  }

  service { 'apache2':
    ensure => running,
    enable => true,
  }

  $greeting = "Apache Installed Successfully!"
  file { '/tmp/apache_status.txt':
    ensure  => file,
    content => $greeting,
  }
}
```

Re-apply and check:

```bash
sudo /opt/puppetlabs/bin/puppet apply /etc/puppetlabs/code/environments/production/manifests/site.pp
```

```bash
cat /tmp/apache_status.txt
```

---

## 🧩 **Explanation of Concepts**

| Concept                 | Description                                                                |
| ----------------------- | -------------------------------------------------------------------------- |
| **Manifest**            | A `.pp` file that defines Puppet resources (e.g., file, package, service). |
| **Module**              | A collection of manifests and files organized for reuse and distribution.  |
| **Class**               | A logical grouping of related configurations (defined inside modules).     |
| **Function / Variable** | Used for dynamic data, improving flexibility of manifests.                 |
| **Modularization**      | Separates configuration logic into reusable modules.                       |
| **Reusability**         | The same module can be applied to multiple nodes/environments easily.      |

---

## 📘 **Analysis: How Modularization & Reusability Work**

* **Modularization**: Puppet modules encapsulate configurations in directories like `/modules/sample_module/` for easy organization.
* **Reusability**: You can reuse the same module (`include sample_module`) in multiple manifests or nodes without rewriting code.
* **Scalability**: Classes can manage complex configurations across environments simply by including them in manifests.
* **Maintainability**: Updates to a module propagate automatically to all systems using it.

---

## ✅ **Final Output Summary**

| Step                      | File Created                  | Source |
| ------------------------- | ----------------------------- | ------ |
| `/tmp/hello_manifest.txt` | Manifest                      |        |
| `/tmp/hello_module.txt`   | Module Class                  |        |
| `/tmp/hello_function.txt` | Function/Variable             |        |
| `/tmp/apache_status.txt`  | Optional (Package Automation) |        |

---

## 🧾 **Conclusion**

Puppet successfully automated file creation and software installation using **Manifests, Modules, Classes, and Functions**.
The modular structure enhanced **reusability**, **scalability**, and **maintainability** of configurations — demonstrating Puppet’s strength in configuration management automation.

---

✅ **Yes, Nawaz — everything you did is correct.**
Your steps are perfect for the experiment; I just reordered and explained them clearly for lab documentation.

Would you like me to give you a **lab report format** (Aim, Procedure, Output, Result, Conclusion) for this same experiment, ready for submission?
