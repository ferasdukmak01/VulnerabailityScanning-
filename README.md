# VulnerabailityScanning-
You’ll conduct a scan on two servers: an internal host running Jenkins, an automation server for building, testing and deploying code; and an external web server that hosts your company’s main website. We’ve suffixed the hostnames of these machines with .int if they are internal, and .com if they are external.
From the top navigation menu, navigate to the Tasks page ( Scans > Tasks).

OpenVAS uses Tasks to represent individual scan setups. These consist primarily of three components: the name of the task, the target (host(s) to scan), and the scan configuration to use (governs what to look for, and which vulnerability tests are run).

Multiple targets can be specified for each Task, enabling batch scans on specific groups of machines, such as by the roles they serve (e.g., web servers, file servers, mail servers) or their location within your environment (e.g., exposed servers in your DMZ vs. machines only reachable from your internal networks). Furthermore, you can elect to run Tasks manually for quick evaluations or schedule them to run at regular intervals as part of an ongoing vulnerability management program.

Click the Task Wizard icon at the top left (wand with a star), then select Advanced Task Wizard.

<img src="https://i.imgur.com/TcrFVtv.png" alt="Lab Screenshot" style="max-width: 100%; height: auto;">

Specify the following options:

Task Name: Practice
Scan Config: Full and fast
Target Host(s): 192.168.0.10,172.30.0.10

<img src="https://i.imgur.com/OmTzTc6.png" alt="Lab Screenshot" style="max-width: 100%; height: auto;">

Click the Create button to immediately initiate your scan.

Your scan will take ~10 mins. Once it has reached 100% completion, click the link under the Last Report column to view the results of your scan.
In the task table at the bottom, click the link in the Last Report column that corresponds with your Practice Task.
Click the Results tab to view the vulnerabilities discovered by your scan.

<img src="https://i.imgur.com/ihweU2k.png" alt="Lab Screenshot" style="max-width: 100%; height: auto;">

In this case, you’ve discovered an arbitrary file read vulnerability (also known as a directory traversal vulnerability), which is what it sounds like: a user can potentially read files on the Jenkins server that are not made available by design. Like the Ghostcat LFI vulnerability you discovered in the previous section, except this vuln only allows reading of the file contents, rather than opening them in the application context – not quite as worrisome.

Additionally, this server is internal, so a threat actor must first gain access to your internal network to exploit this vulnerability – in other words, exploitation is much less likely. However, your analyses should still account for the damage that can be done when one layer of your protection is bypassed.
Click the Arbitrary File Read vulnerability to view additional details OpenVAS provides about its discovery.

<img src="https://i.imgur.com/ABqWSXa.png" alt="Lab Screenshot" style="max-width: 100%; height: auto;">

OpenVAS includes any Insights it has about the vulnerability, as well as the potential Impact. In this case, the latter provides an important detail regarding the permissions a user must have to exploit this vulnerability.

A solution recommendation is also provided, and in this case a common one for vulnerabilities in software: update!! if the vendor still supports the software, chances are there’s a fix provided in a later release.
Click the Arbitrary File Read vulnerability again to collapse the additional details, then click the Cleartext Transmission of Sensitive Information via HTTP vulnerability.

Another vulnerability regarding a lack of traffic encryption. The solution recommendation here is also unsurprising: force use of HTTPS, or HTTP Secure, which enables encryption through TLS, or the Transport Layer Security protocol (you may see reference to SSL – this is, effectively, an older version of TLS).

Again, it is an internal server, so it is tempting to shortcut the additional configuration care here since transmissions won’t travel outside of your network. However, as Jenkins is an automated code deployment solution, it presumably has access to much, if not all, of your codebase. It would be easy for anybody with network access to grab the credentials used to log in to it. An initial network breach should not mean game over.

Speaking of secure HTTPS, the external web server you scanned generated some errors about TLS/SSL. You recall configuring HTTPS on this server – you certainly weren’t going to serve your public website over HTTP in cleartext – so what’s going on there? Clear this section by conducting one final analysis.
Click the Cleartext Transmission of Sensitive Information via HTTP vulnerability again to collapse the additional details, then click the TLS/SSL: OpenSSL TLS vulnerability.

Oh good, another information disclosure vulnerability…. So, the important question – what information is being disclosed? Better check out the CVE to get the full scoop on this software vulnerability.
Click the CVE for this vulnerability.

Private keys!?! You mean those cryptographic materials you’re supposed to hold close to your chest, that permit you to prove your identity and participate in encrypted communications? There’s the devil in these details.

This vulnerability is well-known in the security world, and its arrival resulted in a huge swathe of encrypted internet traffic (HTTPS) being suddenly decryptable. Despite its age, there is still a significant number of machines that remain vulnerable. C’mon, folks, patch your systems!!

So, congratulations! You’ve performed your first vulnerability scan and assessed the contents of the report. This is an important first step in the vulnerability management lifecycle. 


