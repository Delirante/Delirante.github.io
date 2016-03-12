---
layout: projects
title: UDP chat
categories: school
year: [2015]
mainHeading: UDP chat
---

{% include notEnglish.html %}

V projekte sa zaoberám UDP chatom, bez stáleho hostovacieho serveru. Jeden z pripojených používateľov, robil vždy sever a celá komunikácia prebiehala cez neho. Aplikácia mala len prístup do online databázy, v ktorej bola vždy uložená IP adresa aktuálneho používateľa, ktorý robil server. Vďaka správne aplikovanému UPnP protokolu sa mi podarilo na väčšine routerov spraviť automatický portforwarding a tak používateľ nemusel mať verejnú IP a mohol byť serverom. Aplikáciu som písal v jazyku Java, využil som Hibernate, multithreading, UPnP, UDP packetovanie a všetky protokoly s tým spojené.

<br/>
<a href="/">Link na zdrojáky</a>

Takto vyzerala trieda na forwardovanie pomocou UPnP protokolu:


{% highlight java %}

import java.io.IOException;
import javax.swing.JOptionPane;
import javax.xml.parsers.ParserConfigurationException;

import org.apache.log4j.Logger;
import org.wetorrent.upnp.GatewayDevice;
import org.xml.sax.SAXException;


public class Forwarding {
	
	private GatewayDevice d = null;
	private int port1 = 0;
	private int port2 = 0;
	
	public void forwardPort(int port1, String localIpAdress1, int port2, String localIpAdress2) 
		throws IOException, SAXException, ParserConfigurationException
	{		
		this.port1 = port1;
		this.port2 = port2;
		
		GatewayDiscover discover = new GatewayDiscover();
				
		int i = 0;
		while(d == null && i != 4)
		{								
			discover.discover();			
			d = discover.getValidGateway();
		}
		
		if (null != d) 	{	} 
		else 
		{						
			JOptionPane.showMessageDialog(null, "ERROR- UPnP is turn off
			 or you have bad connection to your router, try to connect directly."
			 +"Aplication tried make connection "+i+" times");
			System.exit(0);		    
		}				 
		
		if (!d.addPortMapping(port1, port1,localIpAdress1,"UDP","My forward1") && 			
				!d.addPortMapping(port2, port2,localIpAdress2,"UDP","My forward2")) 
		{			
			JOptionPane.showMessageDialog(null, "ERROR- Port mapping failed, restart 
			your router, if you have still problem with this, buy better router");
			System.exit(0);
		} 
	}
	
	public void shutDown() throws IOException, SAXException	
	{		
		if(d != null)
		{
			d.deletePortMapping(port1,"UDP");
			d.deletePortMapping(port2,"UDP");
		}						
	}	
}
{% endhighlight %}
