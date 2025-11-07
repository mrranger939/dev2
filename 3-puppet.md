
### 🧩 **Goal**

Simulate a **Puppet Master–Agent setup** (pull-based) on a single Linux system and verify configuration synchronization.

---

## ⚙️ STEP 1: Install Puppet 7 Repository and Packages

```bash
wget https://apt.puppet.com/puppet7-release-focal.deb
sudo dpkg -i puppet7-release-focal.deb
sudo apt update
```

Install both **Puppet Server (Master)** and **Puppet Agent**:

```bash
sudo apt install puppetserver puppet-agent -y
```

---

## ⚙️ STEP 2: Start and Enable Puppet Server

```bash
sudo systemctl start puppetserver
sudo systemctl enable puppetserver
```

Verify it’s running:

```bash
sudo systemctl status puppetserver
```

---

## ⚙️ STEP 3: Configure `/etc/hosts` for Local Hostname Resolution

Edit your hosts file:

```bash
sudo nano /etc/hosts
```

Add this line at the end:

```
127.0.0.1   puppetmaster.local agent1.local
```

---

## ⚙️ STEP 4: Configure Puppet Master and Agent in `puppet.conf`

Open Puppet config file:

```bash
sudo nano /etc/puppetlabs/puppet/puppet.conf
```

Replace with:

```
[main]
certname = agent1.local
server = puppetmaster.local
environment = production
runinterval = 1h

[agent]
server = puppetmaster.local
```

This tells Puppet Agent to pull configs from Puppet Master (same system here).

---

## ⚙️ STEP 5: Start Puppet Agent Service

```bash
sudo systemctl start puppet
sudo systemctl enable puppet
```

---

## 🪶 STEP 6: Generate and Sign Certificates (Master ↔ Agent)

### On the same machine (acting as both):

Request certificate:

```bash
sudo /opt/puppetlabs/bin/puppet agent --test --waitforcert=60
```

On Master side (same system):

```bash
sudo /opt/puppetlabs/bin/puppetserver ca list
sudo /opt/puppetlabs/bin/puppetserver ca sign --certname agent1.local
```

Now run agent again to fetch signed cert and pull configuration:

```bash
sudo /opt/puppetlabs/bin/puppet agent --test
```

---

## 📄 STEP 7: Create Puppet Manifest on Master

Create the main manifest file:

```bash
sudo mkdir -p /etc/puppetlabs/code/environments/production/manifests
sudo nano /etc/puppetlabs/code/environments/production/manifests/site.pp
```

Paste:

```puppet
node 'agent1.local' {
  file { '/tmp/hello.txt':
    ensure  => file,
    content => "Hello from Puppet Master–Agent setup on single system!\n",
  }
}
```

---

## 🔁 STEP 8: Run Puppet Agent to Pull Configuration

Run manually to verify:

```bash
sudo /opt/puppetlabs/bin/puppet agent --test
```

If setup is correct, you’ll see output like:

```
Notice: Applied catalog in X seconds
```

Now verify file creation:

```bash
cat /tmp/hello.txt
```

Output:

```
Hello from Puppet Master–Agent setup on single system!
```

---

## 🧾 STEP 9: Demonstrate Synchronization (Update Manifest)

Edit the manifest again:

```bash
sudo nano /etc/puppetlabs/code/environments/production/manifests/site.pp
```

Change content to:

```puppet
content => "Configuration updated successfully from Puppet Master!\n",
```

Run agent again:

```bash
sudo /opt/puppetlabs/bin/puppet agent --test
```

Then check:

```bash
cat /tmp/hello.txt
```

Output should now show the updated text — ✅ synchronization confirmed.

---

## ✅ **Expected Output Summary**

| Step                     | Result                        |
| ------------------------ | ----------------------------- |
| `puppetserver` running   | Master ready                  |
| `puppet agent --test`    | Pulls config successfully     |
| `/tmp/hello.txt` created | Manifest applied              |
| Manifest content updated | Change reflected on next pull |

---

## 🧰 **Useful Verification Commands**

| Command                                                                 | Description              |
| ----------------------------------------------------------------------- | ------------------------ |
| `sudo systemctl status puppetserver`                                    | Check master status      |
| `sudo /opt/puppetlabs/bin/puppetserver ca list`                         | List pending certs       |
| `sudo /opt/puppetlabs/bin/puppetserver ca sign --certname agent1.local` | Sign agent cert          |
| `sudo /opt/puppetlabs/bin/puppet agent --test`                          | Force configuration pull |
| `sudo cat /tmp/hello.txt`                                               | Check result of manifest |

---

## 🎯 **Final Output in Lab Report**

**Title:** Puppet Master–Agent Setup (Pull-based Configuration)
**Environment:** Ubuntu Linux (Single System Simulation)
**Observation:**

* The Puppet Agent successfully retrieved configurations from the Puppet Master.
* Configuration file `/tmp/hello.txt` was created automatically.
* Manifest update on the Master was reflected on the Agent after re-run.

**Conclusion:**
The pull-based configuration setup works successfully. Puppet ensures synchronization between Master and Agent even when running on the same system.

---

Would you like me to give you a **short lab report format (aim, procedure, output, conclusion)** for this same experiment — ready to paste into your submission file?
