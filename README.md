<h1>🛡️ Wazuh Installation and Agent Setup Guide</h1>

<p>This guide covers the steps to install Wazuh server components and register a Windows agent for File Integrity Monitoring (FIM).</p>

<hr>

<h2>📦 Prerequisites</h2>
<p>Install the required packages (if not already installed):</p>

<pre><code>sudo apt-get install gnupg apt-transport-https
</code></pre>

<hr>

<h2>🔑 Install GPG Key</h2>

<pre><code>curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | \
gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && \
chmod 644 /usr/share/keyrings/wazuh.gpg
</code></pre>

<hr>

<h2>📚 Add Wazuh Repository</h2>

<pre><code>echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | \
sudo tee -a /etc/apt/sources.list.d/wazuh.list
</code></pre>

<hr>

<h2>🔄 Update Package Information</h2>

<pre><code>sudo apt-get update
</code></pre>

<hr>

<h2>📥 Download and Install Wazuh</h2>

<pre><code>curl -sO https://packages.wazuh.com/4.13/wazuh-install.sh
chmod 755 wazuh-install.sh
./wazuh-install.sh -a -i
</code></pre>

<p>Once the installation is complete, you'll see output like:</p>

<pre><code>INFO: You can access the web interface: https://&lt;wazuh-dashboard-ip&gt;:443
      User: admin
      Password: wMHa+FTY7rx?tYD*qN7FBLxzQC53iXGp
INFO: Installation finished.
</code></pre>

<hr>

<h2>💻 Download Wazuh Agent (Windows)</h2>

<p><a href="https://packages.wazuh.com/4.x/windows/wazuh-agent-4.13.1-1.msi" target="_blank">Download Windows Agent</a></p>

<hr>

<h2>🔐 Register Agent to the Manager</h2>

<p>On the Wazuh Manager, run:</p>

<pre><code>/var/ossec/bin/manage_agents
</code></pre>

<p>Follow the prompts to add a new agent and retrieve the registration key.</p>

<hr>

<h2>🔑 Add Key to the Windows Agent</h2>

<p>After installing the agent, use the Wazuh Agent Manager GUI or CLI to insert the registration key from the manager.</p>

<hr>

<h2>🗂️ Configure File Integrity Monitoring (FIM)</h2>

<ol>
  <li>Open the config file: <code>C:\Program Files (x86)\ossec-agent\ossec.conf</code></li>
  <li>Add the following block inside the <code>&lt;directories&gt;</code> section:</li>
</ol>

<pre><code>&lt;directories check_all="yes" whodata="yes" realtime="yes"&gt;
  C:\Users\Administrator\Desktop\wazhu-FIM
&lt;/directories&gt;
</code></pre>

<hr>

<h2>🔁 Restart the Agent</h2>

<p>Use either Services.msc or the following commands:</p>

<pre><code>net stop "Wazuh Agent"
net start "Wazuh Agent"
</code></pre>

<hr>

<h2>✅ Confirm Setup</h2>

<p>Log in to the Wazuh Dashboard and confirm that alerts and file integrity logs are appearing for the registered Windows agent.</p>

<hr>

<p><strong>ℹ️ Note:</strong> Replace <code>&lt;wazuh-dashboard-ip&gt;</code> with your actual Wazuh dashboard IP address.</p>
