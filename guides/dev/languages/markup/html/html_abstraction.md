# abstraction of html concepts and examples
<!DOCTYPE html>
<html>
	<head>
		<!-- h{1, 5} -->
		<h1>Header</h1>
		<link href="https://fonts.googleapis.com/css?family=Lobster" rel="stylesheet" type="text/css">
	</head>
	<body>
		<!--Main unrepeated info of a document -->
		<main>
  			<p> Random text
				<br/>
				<strong>bold emphasis</strong><em>italic emphasis</em><del>opp</del><u>underline</u>
				<marquee bgcolour="maroon"loop="-1"scrollamount="2"width="100%">flying text</marquee> 
			</p>	
			
			<!-- groups independent, self-contained content-->
			<article></article>
			<!-- Groups thematically related content-->
			<section></section>
			<!-- Groups content -->
			<div></div>
			<!-- For figures for tables and stuff -->
			<figure>
    			<figcaption> Caption text </figcaption>
			</figure>
		
		</main>
		
		<img src="[imagePath].jpg"/> 
		<img src="[url]" width="82" height="86" title="Image title", alt="Description"/> 
		
		<nav>
			<a href="http://www.google.co.uk"> Google </a> 
			<a href="[url]" target="_blank"><img src="#" alt="Description"/></a>
			<a href="./contact.html">Contact</a>
		</nav> 
		
		<!--Use ul for un-ordered, # repersents id, use with no ID to point to current tag  -->
		<ol>
		  <li><a href="#top">Top</a></li> 
		  <li><a href="#bottom">Bottom</a></li> 
		</ol> 
		
		<!--Keyboard navigation-->
		<h2><a id="first" href="" accesskey="g">Link somewhere</a></h2>
		<div tabindex="0">I need keyboard focus!</div>
		
		<video src="myVideo.mp4" width="320" height="240" controls> 
 	 		Video not supported
		</video>
		<audio id="meowClip" controls>
    		<source src="audio/meow.mp3" type="audio/mpeg" />
    		<source src="audio/meow.ogg" type="audio/ogg" />
		</audio>
		
		<form>
			<fieldset>
				<legend>Choose one of these three items:</legend>
			  	<input id="one" type="radio" name="items" value="one">
				<label for="one">Choice One</label><br>
				<input id="two" type="radio" name="items" value="two">
			  	<label for="two">Choice Two</label><br>
			</fieldset>
		  	<label for="inputText">	<input id="inputText" type="text" required> Some text </label>
			<label><input type="checkbox" name="personality" checked> Check it </label>
			<input type="date" id="pickdate" name="date">
			<button type="submit" onclick=alert('Data entered')>Submit</button>
		</form> 
		
		<table border="1"> <!--Set up the table. Creates a border(better to do in CSS).-->
   			<tr>   <!--Set up rows.-->
      			<th scope="row">Temperature</th> <!--Set up heading for row.-->
      			<td>73</td> <!-- Set up cells-->
      			<td>81</td>
   			</tr>
			<tr>
				<th scope="col">Saturday</th> <!--Set up heading for column.-->
				<td colspan="2">asda</td> <!--Denote number of colums it spans accross.-->
   			</tr>
		</table>
		
	
		
		
	</body>
	<footer> <!--Footer -->
  		<span>&copy; Banas Travel Blog 	&amp;</span> <!--Adds copyright symbol. -->
	</footer>
</html>
