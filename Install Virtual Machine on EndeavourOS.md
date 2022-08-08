# How-to-Linux
Things Ive learned about Linux that might help you

<h1>Setup Virtual Machine on Arch Linux (EndeavourOS) with Bridged Networking - My Experience</h1>
<p>&nbsp;</p>
<p><strong>Laptop</strong>: 2014 Macbook Air</p>
<p><strong>Processor</strong>: Intel(R) Core(TM) i5-4260U CPU @ 1.40GHz, 1400 MHz (4 cores)</p>
<p><strong>Memory</strong>: 4GB</p>
<p><strong>Network Card</strong>: Broadcom BCM4360 802.11ac Wireless Network Adapter</p>
<p><strong>OS</strong>: EndeavourOS (Arch Linux)&nbsp; 5.18.16-arch1-1</p>
<p>&nbsp;</p>
<h2><strong>Problem</strong>:</h2>
<p>I needed to setup 2 Vms for testing on my fresh install of EndeavourOS with bridged networking to run my Ansible configurations. After installing VirtualBox (Oracle) I was successful in creating one instance with networking, but not with the second one. My machine was crashing during the install. I also tried installing GNOME-Boxes which worked to create the instances but was difficult to setup any type bridged networking, as the config options are limited.</p>
<h2><strong>Solution</strong>:</h2>
<p>Use KVM Virtual Machine Manager</p>
<p>This is obviously a well-known and used hypervisor for Linux, but for someone learning or getting started with Linux I wanted to share my experience of how I managed to get it working on my machine.</p>
<h3>&nbsp;</h3>
<h3><strong>Step 1:</strong></h3>
<p>Make sure your system is updated</p>
<p>Then run the following commands in your terminal</p>
<p>&nbsp;</p>
<pre class="wp-block-code"><code>sudo pacman -S archlinux-keyring</code></pre>
<pre class="wp-block-code"><code>sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat libguestfs ebtables iptables<br /><br /><br /></code></pre>
<h3><strong>Step 2:</strong></h3>
<p>Start and enable the libvirtd.service</p>
<p>&nbsp;</p>
<pre class="wp-block-code"><code>sudo systemctl start libvirtd.service</code></pre>
<pre class="wp-block-code"><code>sudo systemctl enable libvirtd.service</code></pre>
<h3 class="wp-block-code"><code>&nbsp;</code></h3>
<h3><strong>Step 3:</strong></h3>
<p>Edit the libvirtd.conf file using nano or any editor</p>
<p>&nbsp;</p>
<pre class="wp-block-code"><code>sudo nano /etc/libvirt/libvirtd.conf<br /></code></pre>
<p>Remove the # from the following lines</p>
<p>unix_sock_group = "libvirt"</p>
<p>unix_sock_rw_perms = "0770"</p>
<p>&nbsp;</p>
<h3><strong>Step 4:</strong></h3>
<p>Create and add your account to the libvirt group</p>
<p>&nbsp;</p>
<pre class="wp-block-code"><code>sudo usermod -aG libvirt [your user name]<br /></code></pre>
<pre class="wp-block-code">newgrp libvirt</pre>
<pre class="wp-block-code">sudo systemctl restart libvirtd.service<br /><br /></pre>
<h3><strong>Step 5:</strong></h3>
<p>Reboot</p>
<p>&nbsp;</p>
<h3><strong>Step 6:</strong></h3>
<p>Start Virtual Machine Manager</p>
<p>&nbsp;</p>
<h3><strong>6a:</strong></h3>
<p>File / Add Connection</p>
<p>- Hypervisor: QEMU/KVM</p>
<p>- Autoconnect: checked</p>
<p>Connect</p>
<p>&nbsp;</p>
<p>Edit / Connection Details</p>
<p>You should see a default network , make sure to click Autostart: On Boot</p>
<p>if not, then click + at bottom left</p>
<p>and create a new one</p>
<p>give it a name (i used br10) make sure Mode is set to NAT</p>
<p>click Finish</p>
<p>&nbsp;</p>
<h3><strong>6b:</strong></h3>
<p>File / New Virtual Machine</p>
<p>(the install guide will open)</p>
<p>Choose Local install media - Forward</p>
<p>Choose Browse</p>
<p>Click + at the bottom left</p>
<p>Create pool storage Name: give it a name (Pool1 or whatever you like)</p>
<p>Target Path: - Browse to the directory with your ISO files</p>
<p>Click Finish</p>
<p>Select the Pool you just created on the left then the ISO image on the right</p>
<p>Click Choose Volume</p>
<p>Click Forward</p>
<p>&nbsp;</p>
<h3><strong>6c:</strong></h3>
<p>Select your memory (I chose 1024)</p>
<p>Select CPUs (I chose 1 )</p>
<p>These will depend on how much memory and CPU type you have.</p>
<p>Click Forward</p>
<p>&nbsp;</p>
<h3><strong>6d:</strong></h3>
<p>Change Gib to what ever size you need ( I suggest 10 at the minimum)</p>
<p>Click Forward</p>
<p>&nbsp;</p>
<h3><strong>6e:</strong></h3>
<p>Give it a name ( Something practical if you have multiple VMs)</p>
<p>Click Finish</p>
<p>&nbsp;</p>
<p>It will ask you to start, click YES</p>
<p>Follow the on screen instructions to begin the install of your OS.</p>
<p>&nbsp;</p>
<p>- Please feel free to comment or give your experiences with setting up VMs on an older Mac laptop!</p>
<p>Like I said , this is how I was able to setup my own VMs on my machine. Always make sure you have enough Disk space and RAM on your system. The more the better.</p>
<p>Thanks</p>
<p>&nbsp;</p>
