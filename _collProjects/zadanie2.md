---
layout: projects
title: Zadanie2
categories: wpub
year: [2016]
mainHeading: Zadanie2
---

V tomto zadaní je vypracovaná časť bakalárskej práce. Zadanie je vypracovávané vo formáte DocBook. Výstupné XMLko sa pomocou XSLT procesora a XSLT-FO formatera prekonvertuje na čitateľné PDF.
<br/>
Text spĺňa štandardy odborných článkov, medzi ne patrí:
<ul>
	<li>
		Členenie na kapitoly a podkapitoly až do úrovne 3 (1.2.3 nazov kapitoly.)
	</li>	
	<li>
		Obsahuje prílohy a automaticky generovaný obsah a rôzne zoznamy (Zoznam obrázkov, zoznam tabuliek.).
	</li>
	<li>
		Dôležité pojmy sú v texte výraznené či už spôsobom italic alebo boldom.
	</li>
	<li>
		Pre prehľadnosť textu sa v ňom nachádzajú časti, ktoré sú robené pomocou odrážok.
	</li>
	<li>
		Obsahuje odkazy na iné časti dokumentu a odkazy na URL stránky.
	</li>
	<li>
		Obsahuje poznámky pod čiarov.
	</li>
	<li>
		Obsahuje zoznam použitej literatúry, vrátane jej citácie v texte.
	</li>
	<li>
		Pre prehľadnosť sa v texte nechádzajú obrázky a tabuľky. Vrátane odkazov na ne v texte.
	</li>
	<li>
		Obsahuje zoznam obrázkov a tabuliek.
	</li>
	<li>
		Obsahuje slovník pojmov aj register pojmov.
	</li>

</ul>

<br/>
<p>Nižšie sú ukážky len najpodstatnejších kódov. Všetky zmeny a kódy sa nachádzajú v odovzdanom archíve.</p>


<p>Na zvýraznenie textu som použil </p>
```xml			
<emphasis role="bold">text</emphasis>
<citetitle>text</citetitle>
```

<p>Obrázky</p>
```xml		
<figure id="image1" xreflabel="popis">
	<title>Testovanie slova v systéme Memrise</title>
	<mediaobject>
		<imageobject>
			<imagedata align="center" fileref="img1.png" scale="65"/>
		</imageobject>				
	</mediaobject>
</figure>
```

<p>Poznámka pod čiarov s odkazom mimo práce</p>
 
```xml		
<title>text
	<footnote>
		<para>
			<ulink url="http://google.com/"/>						
		</para>
	</footnote>
</title>
```

<p>Záznam v slovníku pojmov</p>
```xml		
<glossentry>
	<glossterm>ACID</glossterm>
	<acronym>ACID</acronym>
		<glossdef>
			<para>Atomicity, Consistency, Isolation, Durability.</para>       
		</glossdef>
</glossentry>
```
<p>Pre odkazy som primárne používal</p>
```xml		
<xref linkend="image1"/>
```

<p>V xsl súbore som upravil formát poznámok pod čiarov</p>
```xml		
<!-- Uprava poznamok pod ciarov, nech sa pocitaju od zaciatku dokumentu -->
<xsl:template match="footnote" mode="footnote.number">
	<xsl:number level="any"
                count="footnote[not(@label)][not(ancestor::table) and not(ancestor::informaltable)]"/> 
</xsl:template>
```
<p>V xsl súbore som upravil formát obrázkov</p>
```xml		
<!-- Obrazok, nech sa pocita skrz cely dokument normalne-->
<xsl:template match="figure" mode="label.markup">
	<xsl:choose>
		<xsl:when test="@label">
			<xsl:value-of select="@label"/>
		</xsl:when>
		<xsl:otherwise>
			<xsl:number format="1" level="any"/>
		</xsl:otherwise>
	</xsl:choose>
</xsl:template>
```
<p>V xsl súbore som upravil formát tabuliek</p>
```xml		
<!-- Tabulka, nech sa pocita skrz cely dokument normalne-->
<xsl:template match="table" mode="label.markup">
	<xsl:choose>
		<xsl:when test="@label">
			<xsl:value-of select="@label"/>
		</xsl:when>
		<xsl:otherwise>
			<xsl:number format="1" level="any"/>
		</xsl:otherwise>
	</xsl:choose>
</xsl:template>
```
<p>K vytvoreniu obsahu a všetkých potrebných zoznamov som upravil túto časť xsl</p>
```xml		
<xsl:param name="generate.toc">
book      title,toc,figure,table
</xsl:param>
```
<p>K vytvoreniu registra pojmov bol použitý tento element</p>
```xml		
<index><title>Register pojmov</title></index>
```

<p>Jednotlivé pojmy v registri pojmov sa vytvárali nasledovne</p>
```xml		
<indexterm><primary>API</primary></indexterm>
```


<p>Upravil som aj výzor kapitoly (odstránil som nežiadúci nápis). Zmenil odsadenie paragrafov. Upravil začiatočné strany dokumentu. A iné.</p>




