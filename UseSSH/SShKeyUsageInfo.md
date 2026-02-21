
Using a **Mac (Safari user agent)** and created an **Azure VM (Ubuntu)**, here is exactly how to start SSH and use your `ssh_key`.

---

# ✅ Step 1 — Make Sure Your VM Is Running

Go to:

👉 **Azure Portal** → Virtual Machine →
Check **Status = Running**

If stopped → click **Start**

---

# ✅ Step 2 — Locate Your SSH Private Key

When you created the VM, Azure either:

* Downloaded a `.pem` file
  OR
* You created one using `ssh-keygen`

Example:

```
my_vm_key.pem
```

Move it to your home folder if needed.

---

# ✅ Step 3 — Set Correct Permissions (Very Important)

On Mac Terminal:

```bash
chmod 400 my_vm_key.pem
```

If you skip this, SSH will fail with:

> "Permissions are too open"

---

# ✅ Step 4 — Get Your Public IP

In Azure:

👉 VM → Overview → Copy **Public IP Address**

Example:

```
20.55.100.25
```

---

# ✅ Step 5 — Connect Using SSH

Now run:

```bash
ssh -i my_vm_key.pem azureuser@20.55.100.25
```

Replace:

* `my_vm_key.pem` → your key file
* `azureuser` → the username you created
* `20.55.100.25` → your VM public IP

---

# 🎉 If Successful

You will see something like:

```
azureuser@vm-name:~$
```

That means you're inside the Azure VM.

---

# ❓ If You Get "Connection Refused"

Check:

### 1️⃣ SSH Port Open?

Azure Portal → VM → Networking
Ensure inbound rule allows:

* Port 22
* Source = My IP (or Any for testing)

---

### 2️⃣ Is SSH Service Running on VM?

If you are inside Azure Portal → Serial Console, run:

```bash
sudo systemctl status ssh
```

If not running:

```bash
sudo systemctl start ssh
```

---

# 🔐 Optional: Use SSH Agent (Cleaner Way)

Instead of typing `-i` every time:

```bash
ssh-add my_vm_key.pem
ssh azureuser@20.55.100.25
```

---

# 🚀 Pro Tip (Since You're Building MVPs)

If you're planning multiple Azure VMs:

Add this to `~/.ssh/config`

```bash
Host myazurevm
    HostName 20.55.100.25
    User azureuser
    IdentityFile ~/my_vm_key.pem
```

Then you just connect using:

```bash
ssh myazurevm
```

Much cleaner.

---

If you show me:

* Your exact SSH error (if any)
* Screenshot of networking rule

I’ll debug it instantly for you.
