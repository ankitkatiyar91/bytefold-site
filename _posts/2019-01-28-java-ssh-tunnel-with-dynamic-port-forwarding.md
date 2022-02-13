---
layout: post
title: Java SSH Tunnel with Dynamic Port Forwarding
date: 2019-01-28 13:36:54.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- Java
tags:
- Dynamic port forward in java
- Java Tunnel
meta: {}
author:
  login: ankitkatiyar91
  email: ankitkatiyar67@gmail.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/java-ssh-tunnel-with-dynamic-port-forwarding/"
excerpt: Create a putty like dynamic port forward in Java. Unable to access URL on
  you network? want to bypass them over a bastion server? A running demo using JSCH
  and Apache MINA to use port forwarding.
---
<!-- wp:paragraph -->

If you have ever worked with deployment teams, you have done SSH through some clients like putty to your server and check whether a particular service reachable from there or verify something else.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

I stuck with a problem where my production team has blocked access to production URLs of individual nodes. Now I cannot check if a particular node in my cluster is working or not. After a lot of efforts, they gave us a **_bastion server_&nbsp;** (a server machine that is allowed with special rights, [more](https://en.wikipedia.org/wiki/Bastion_host)) that&nbsp;allows&nbsp;us access to these nodes.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Basically, my application URL is _www.myurl.com_ that runs 10 nodes on 8080 port. Then I am allowed to access _www.myurl.com_ URL over internet/VPN but not allowed to check individual nodes like _www.node1.myurl.com:8080_ etc. Now I have to use the new bastion server and log in through SSH then use dynamic port forward on that server. After that use SOCKS proxy of my browser to access these URLs.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Although, I am able to access these URLs from my browser but what about my JAVA utility that was doing all the verification without me doing all the manual work? So juggled around couple of libraries to see if I can still use that utitily and do all this.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

There are couple of libraries but I explored two of them

<!-- /wp:paragraph -->

<!-- wp:list -->

- JSCH (Doesn't support dynamic port forward as of now)
- Apache MINA 

<!-- /wp:list -->

<!-- wp:heading {"level":3} -->

### JSCH Example

<!-- /wp:heading -->

<!-- wp:paragraph -->

**Dependencies**

<!-- /wp:paragraph -->

<!-- wp:enlighter/codeblock {"language":"xml"} -->

```
<dependency>
  <groupId>com.jcraft</groupId>
  <artifactId>jsch</artifactId>
  <version>0.1.55</version>
</dependency>
```

<!-- /wp:enlighter/codeblock -->

<!-- wp:paragraph -->

**Example**

<!-- /wp:paragraph -->

<!-- wp:enlighter/codeblock {"language":"java"} -->

```
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;

import com.jcraft.jsch.JSch;
import com.jcraft.jsch.Session;
import com.jcraft.jsch.UserInfo;

public class TunnelJSCH {
	public static void main(String[] args) {
		TunnelJSCH t = new TunnelJSCH();
		try {
			t.go();
		} catch (Exception ex) {
			ex.printStackTrace();
		}
	}

	public void go() throws Exception {
		String host = "bastion-server-host";
		String user = "user";
		String password = "password";
		int port = 22;

		int tunnelLocalPort = 8000;
		String tunnelRemoteHost = "remote-host";
		int tunnelRemotePort = 9080;

		JSch jsch = new JSch();
		Session session = jsch.getSession(user, host, port);
		session.setPassword(password);
		localUserInfo lui = new localUserInfo();
		session.setUserInfo(lui);
		System.out.println("Creating connection to server:" + host);
		session.connect();
		System.out.println("Connected to server, initializing Port forwarding ");
		session.setPortForwardingL(tunnelLocalPort, tunnelRemoteHost, tunnelRemotePort);
		
		System.out.println("Port forward successful forwarding: localhost:" + tunnelLocalPort + " -> "
				+ tunnelRemoteHost + ":" + tunnelRemotePort);

		try {

			URL url = new URL("http://localhost:8000");
			System.out.println("Reading data from URL: " + url);
			BufferedReader in = new BufferedReader(new InputStreamReader(url.openStream()));
			System.out.println("================== Data From URL ==================\n");
			String inputLine;
			while ((inputLine = in.readLine()) != null)
				System.out.println(inputLine);
			in.close();

			System.out.println("================== Data From URL ==================\n");
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

		if (session != null && session.isConnected()) {
			System.out.println("Closing SSH Connection");
			session.disconnect();
		}

	}

	class localUserInfo implements UserInfo {
		String passwd;

		public String getPassword() {
			return passwd;
		}

		public boolean promptYesNo(String str) {
			return true;
		}

		public String getPassphrase() {
			return null;
		}

		public boolean promptPassphrase(String message) {
			return true;
		}

		public boolean promptPassword(String message) {
			return true;
		}

		public void showMessage(String message) {
		}
	}
}
```

<!-- /wp:enlighter/codeblock -->

<!-- wp:heading {"level":3} -->

### Apache MINA

<!-- /wp:heading -->

<!-- wp:paragraph -->

**Dependencies**

<!-- /wp:paragraph -->

<!-- wp:enlighter/codeblock {"language":"xml"} -->

```
<!-- https://mvnrepository.com/artifact/org.apache.mina/mina-core -->
		<dependency>
			<groupId>org.apache.mina</groupId>
			<artifactId>mina-core</artifactId>
			<version>3.0.0-M2</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.apache.sshd/sshd-core -->
		<dependency>
			<groupId>org.apache.sshd</groupId>
			<artifactId>sshd-core</artifactId>
			<version>2.1.0</version>
		</dependency>

		<dependency>
			<groupId>org.apache.sshd</groupId>
			<artifactId>sshd-putty</artifactId>
			<version>2.1.0</version>
		</dependency>

		<dependency>
			<groupId>org.apache.sshd</groupId>
			<artifactId>sshd-common</artifactId>
			<version>2.1.0</version>
		</dependency>
```

<!-- /wp:enlighter/codeblock -->

<!-- wp:paragraph -->

**Example**

<!-- /wp:paragraph -->

<!-- wp:enlighter/codeblock {"language":"java"} -->

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.InetSocketAddress;
import java.net.MalformedURLException;
import java.net.Proxy;
import java.net.URISyntaxException;
import java.net.URL;
import java.nio.file.Paths;
import java.security.GeneralSecurityException;
import java.security.KeyPair;
import java.util.Collection;
import java.util.Collections;
import java.util.List;

import org.apache.sshd.client.SshClient;
import org.apache.sshd.client.auth.hostbased.HostKeyIdentityProvider;
import org.apache.sshd.client.keyverifier.AcceptAllServerKeyVerifier;
import org.apache.sshd.client.session.ClientSession;
import org.apache.sshd.common.NamedFactory;
import org.apache.sshd.common.config.keys.loader.pem.PEMResourceParserUtils;
import org.apache.sshd.common.config.keys.loader.putty.PuttyKeyUtils;
import org.apache.sshd.common.forward.PortForwardingEventListener;
import org.apache.sshd.common.session.Session;
import org.apache.sshd.common.util.net.SshdSocketAddress;
import org.apache.sshd.server.channel.PuttyRequestHandler;
import org.apache.sshd.server.forward.AcceptAllForwardingFilter;
import org.apache.sshd.server.keyprovider.SimpleGeneratorHostKeyProvider;

/**
 * This class
 * 
 * @author Ankit Katiyar
 *
 */
public class AmazonTest {

	private static String BASTION_SERVER_PASSWORD = "P@ssword1";
	private static final String BASTION_SERVER_USER = "ec2-user";
	private static final String BASTION_SEREVR_HOST = "ec2-18-191-207-91.us-east-2.compute.amazonaws.com";
	private static final String URL_TO_ACCESS = "http://www.google.com";

	public static void main(String[] args) {

		try {
			
			
			Collection<KeyPair> keys = null;
			// OPtional loading keys from a PEM file
			//keys=PEMResourceParserUtils.getPEMResourceParserByAlgorithm("RSA").loadKeyPairs(ClassLoader.getSystemResource("local-ps-test.pem").toURI().toURL(), null);
			
			// Optional: Using Putty key for login 
			 keys=PuttyKeyUtils.DEFAULT_INSTANCE.loadKeyPairs(ClassLoader.getSystemResource("local-ps-private-key.ppk").toURI().toURL(), null);
			 
			SshClient client = SshClient.setUpDefaultClient();
			client.setForwardingFilter(AcceptAllForwardingFilter.INSTANCE);
			client.setServerKeyVerifier(AcceptAllServerKeyVerifier.INSTANCE);
			client.start();

			// using the client for multiple sessions...
			try (ClientSession session = client.connect(BASTION_SERVER_USER, BASTION_SEREVR_HOST, 22).verify()
					.getSession()) {
				// IF you use password to login provide here
				// session.addPasswordIdentity(BASTION_SERVER_PASSWORD); // for password-based
				// authentication
				
				session.addPublicKeyIdentity(keys.iterator().next());

				// authentication
				// Note: can add BOTH password AND public key identities - depends on the
				// client/server security setup

				session.auth().verify(10000);
				// start using the session to run commands, do SCP/SFTP, create local/remote
				// port forwarding, etc...

				session.addPortForwardingEventListener(new PortForwardingEventListener() {

					@Override
					public void establishedDynamicTunnel(Session session, SshdSocketAddress local,
							SshdSocketAddress boundAddress, Throwable reason) throws IOException {
						// TODO Auto-generated method stub
						PortForwardingEventListener.super.establishedDynamicTunnel(session, local, boundAddress, reason);

						System.out.println("Dynamic Forword Tunnel is Ready");
					}
				});

				SshdSocketAddress sshdSocketAddress = session
						.startDynamicPortForwarding(new SshdSocketAddress("localhost", 8000));

				System.out.println("Host: " + sshdSocketAddress.getHostName());
				System.out.println("Port: " + sshdSocketAddress.getPort());

				// Create a Proxy object to work with
				Proxy proxy = new Proxy(Proxy.Type.SOCKS,
						new InetSocketAddress(sshdSocketAddress.getHostName(), sshdSocketAddress.getPort()));

				/**
				 * Now you can use this proxy instance into any URL until this SSH session is active. 
				 */
				
				// TEST one URL
				HttpURLConnection connection = (HttpURLConnection) new URL(URL_TO_ACCESS).openConnection(proxy);

				System.out.println("Proxy work:" + connection.getURL());

				BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
				System.out.println("================== Data From URL ==================\n");
				String inputLine;
				while ((inputLine = in.readLine()) != null)
					System.out.println(inputLine);
				in.close();

				System.out.println("================== Data From URL ==================\n");
			} catch (IOException e1) {
				// TODO Auto-generated catch block
				e1.printStackTrace();
			} catch (Exception e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}

}
```

<!-- /wp:enlighter/codeblock -->

<!-- wp:paragraph -->

View complete project at [GitHub&nbsp;](https://github.com/ankitkatiyar91/java-framework-examples/tree/master/java-tunneling)

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

[https://github.com/ankitkatiyar91/java-framework-examples/tree/master/java-tunneling](https://github.com/ankitkatiyar91/java-framework-examples/tree/master/java-tunneling)

<!-- /wp:paragraph -->

