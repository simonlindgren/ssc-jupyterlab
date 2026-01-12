# VS Code Remote on Swedish Science Cloud (GPU)

This guide describes how to set up a **single-user research machine** on Swedish Science Cloud (SSC) and work on it using VS Code Remote (SSH).

---

## 1. Get access to SSC

If you work at a Swedish university, you can apply for free computing resources from Swedish Science Cloud (via NAISS).

1. Go to https://supr.naiss.se and create an account using your university credentials.
2. Create a new proposal and wait for approval (usually 1–2 days).
3. Once approved, go to https://cloud.snic.se and open a dashboard for the appropriate region (EAST-1 or NORTH-1).

---

## 2. Create a virtual machine

1. Go to **Compute → Images**.
2. Pick a Linux version and click **Launch**.  

### Launch Instance settings

- **Details**  
  Enter an instance name. Click **Next**.

- **Source**  
  Make sure the chosen OS is selected.  
  Set **Volume Size (GB)** to up to `1000` (SSC allows max 1 TB). **Next**.

- **Flavor**  
  Choose hardware, for example:  
  `ssc.gpu.8c.32gb.1xA100` --> **Next**
  
- **Networks**  
  Select the **NAISS** network. **Next**.

- **Network Ports**  
  Skip. **Next**.

- **Security Groups**  
  Make sure `default` is selected. **Next**.

- **Key Pair**  
  Create a new key pair.  
  Give it a name and leave *Key Type* empty.  
  Copy and save the key locally as a `.pem` file (for example `key.pem`).  
  Restrict permissions:
  ```bash
  chmod 600 key.pem
  ```

- **Remaining settings**
   Leave unchanged.
   Click **Launch Instance**.

## 3. Assign floating IP and open SSH
   1.	Go to Compute → Instances and confirm the instance state is Running.
	2.	Associate a floating IP with the internal IPv4 address, using the dropdown menu in the Actions column.
	3.	Go to Network → Security Groups → Manage Rules.
	4.	Add a rule:
	   •	Protocol: SSH
	   •	Port: 22 (might not have to be stated)
	   •	CIDR: 0.0.0.0/0

   5. Test the connection from your local machine:
   ```bash
   ssh -i /path/to/key.pem ubuntu@<floating-ip>
   ```
   6. If it works, exit the connection.

## 4. VSCode

- Make sure the extension `Remote - SSH` is installed in VSCode.
- From command palette, choose: Remote-SSH: Add New SSH Host
- Enter: `ssh -i /path/to/key.pem ubuntu@<floating-ip>`
- Connect using Remote-SSH: Connect to Host

