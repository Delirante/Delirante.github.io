---
layout: projects
title: Zadanie3
categories: wpub
year: [2016]
mainHeading: Zadanie3
---

V tomto zadani som vypracoval xslt sablony a ostatne dokumenty pomocou ktorych je mozne vytvorit prezentaciu v tvare xhtml a pdf.

<br/>
Vytvoril som nasledovné súbory:
<ul>
	<li>
		myDtd.dtd - Definicia xml pomocou DTD
	</li>
	<li>
		mySchema.xsd - Definicia xml pomocou XML Schema
	</li>
	<li>
		myTransformation.dat - Spustitelny subor, ktory to vsetko koordinuje
	</li>
	<li>
		presentation.css - CSS triedy ktore su potrebne pre vygenerovanie html prezentacie
	</li>
	<li>
		sablonaDefault.xsl - Sablona potrebna pre transformacie. Obsahuje spolocne casti ostatnych sablon. Ostatne sablony ju includuju.
	</li>
	<li>
		sablonaHTML.xsl - Sablona potrebna pre transformaciu do formatu html
	</li>
	<li>
		sablonaPDF.xsl - Sablona potrebna pre transformaciu do pdf
	</li>
	<li>
		vstup1.xml - Ukazkovy subor xml so vstupnymi datami
	</li>
	<li>
		vstup2.xml - Ukazkovy subor xml so vstupnymi datami
	</li>	
</ul>


<br/>
Funguje to nasledovne:
<ul>
	<li>
		Používateľ vytvorí XML ktoré bude validné vzhľadom na dodanú XML schemu.
	</li>
	<li>
		Spustí myTransformation.bat so vstupným súborom xml ktorý vytvoril.
	</li>
	<li>
		V priečinku htmlPage nájde vytvorenú HTML prezentáciu.
	</li>	
	<li>
		V priečinku pdf nájde vytvorenú PDF prezentáciu.
	</li>	
</ul>

<br/>
Používateľ môže:
<ul>
	<li>
		Menit vysku a sirku prezentacie.
	</li>	
	<li>
		Zapnut/ vypnut cislovanie stran.
	</li>	
	<li>
		Zapnut/ vypnut zobrazenie mena na posledom slajde.
	</li>	
	<li>
		Menit farbu pisma na celej prezentacii.
	</li>	
	<li>
		Menit obrazok na pozadi prezentacie.
	</li>	
	<li>
		Menit obrazok na pozadi na jednolivych slajdoch.
	</li>	
	<li>
		Rozhodovat ci sa na slajde bude nachadzat iba obrazok, iba video, iba text, video a text alebo obrazok a text.
	</li>	
	<li>
		Menit poziciu multimedialneho obsahu a textoveho obsahu.
	</li>	
	<li>
		Menit farbu pozadia na tlacidlach.
	</li>	
	<li>
		Menit text na tlacidlach.
	</li>
</ul>

<br/>
Dolezite informacie:
<ul>
	<li>
		Po vlozeni videa do prezentacie sa toto video zobrazi iba na html prezentacii. Na pdf prezentacii sa toto video
nachadzat nebude, koli neschopnosti pdf prehravat videa. PDF prez. korektne prisposobi svoj vzhlad bez videa.
	</li>
	<li>
		HTML prezentacia je plne rezponzivna a meni svoje pozicie a velkosti obrazkov/ videi vzhladom na zadanu vstupnu velkost.
	</li>
	<li>
		PDF prezentacia responzivna nieje a tak zadana vstupna velkost nehra pri jej generovani ziadnu rolu. Avsak prisposobuje velkosti obrazkov tak aby boli zobrazene korektne.
 	</li>
	
</ul>

<br/>
<p>Nižšie sú ukážky len najpodstatnejších kódov. Všetky kódy sa nachádzajú v odovzdanom archíve.</p>


<p>Generovanie contentu</p>
```xml			
<xsl:template match="slide">
	<xsl:variable name="mainPos" select="count(preceding-sibling::slide) + 1"/>
	<xsl:result-document href="htmlPage/section_{$mainPos}.html">
		<html>
			<head>
				<link rel="stylesheet" type="text/css" href="../presentation.css" />
				<link href="https://fonts.googleapis.com/css?family=Open+Sans&amp;subset=latin,latin-ext" rel="stylesheet" type="text/css"/>
				<title><xsl:value-of select="title"/></title>
			</head>
			<body>
				<div class="slide">							
					<xsl:call-template name="slideStyle2"/>	
					
					<xsl:apply-templates/>						
					<xsl:call-template name="footer2"/>
				</div>					
			</body>
		</html>
	</xsl:result-document>
</xsl:template>
```

<p>Pata</p>
```xml		
<xsl:template name="footer2">
	<xsl:variable name="position" select="count(preceding-sibling::slide) + 1 + 1"/>
	<xsl:variable name="position2" select="count(preceding-sibling::slide)"/>
	<div class="slideFoot">
		<div class="middleHorizontal middleVertical">
			<a href="C:\Users\King\Desktop\zadanie3\htmlPage\section_{$position2}.html" class="button margin_right_20">
				<xsl:call-template name="buttons"/>
				<xsl:value-of select="//buttons/prevButton/text()"/>
			</a>
			<xsl:if test="../..[@numbering='true']">
				<span class="pageNumber" style="font-weight:bold">
					<xsl:value-of select="$position -1" />
				</span>
			</xsl:if>
			<xsl:choose>
				<xsl:when test="count(preceding-sibling::slide) + 1 = $nSlides">
					<a href="C:\Users\King\Desktop\zadanie3\htmlPage\section_end.html" class="button margin_left_20">
						<xsl:call-template name="buttons"/>
						<xsl:value-of select="//buttons/nextButton"/>
					</a>
				</xsl:when>
				<xsl:otherwise>
					<a href="C:\Users\King\Desktop\zadanie3\htmlPage\section_{$position}.html" class="button margin_left_20">
						<xsl:call-template name="buttons"/>
						<xsl:value-of select="//buttons/nextButton"/>							
					</a>
				</xsl:otherwise>
			</xsl:choose>				
		</div>			
	</div>
</xsl:template>
```

<p>Farba buttonov</p> 
```xml		
<xsl:template name="buttons">
	<xsl:attribute name="style">
		background-color: <xsl:value-of select="//buttons/@backgroundColor"/>
	</xsl:attribute>
</xsl:template>
```

<p>Video</p>
```xml		
<xsl:template match="video">
	<xsl:variable name="videoSrc" select="./text()"/>
	<video controls="true" class="middleVertical middleHorizontalImage">
	  <source src="{$videoSrc}" type="video/mp4"/>
	</video>		
</xsl:template>	
```



