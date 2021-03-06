# abstraction of cascading style sheets code
## selectors 
p {} .class {} #id {} 
/*Entire DOM*/
:root{}
/*Apply when the user hovers over element*/
:hover{}

## properties
Units
	px: pixels
	em: Relative to parent elements font size
	rem: Relative to root elements font size
	%: Percentage of parent elements size
	repsonsive typography
		viewpoints (units relative to viewpoint dimensions (width & height of device))
			10vw: 10% of viewpoints witdth, 10vh: 10% of VP height, 10vmin: 10% of VPs smaller dimension (height or width), 10vmax

Generic properties:
	auto
	relative
	absolute
Second properties	
	!important: override everything
*/

## elements


.text{
	/*[colourName]: generic colour*, #Hexcolour:, rgb(0, 0, 0):,   */
	color: ${}; 
	font-size: 3rem;
		font-family: Arial;
		font-weight: bold;
	text-transform: capitalize;
	text-align: right; /* left, right or center */
	
}

.background{
	background-color: blue;
	background-position: center;  
    background-repeat: no-repeat;
    background-size: cover;
	background-image: url("[url | path]"); 
}

.size{
	height: 300px;
  	width: 500px;
	min-width: 300px; 
    max-width: 600px;
}
.positioning{
	margin: auto;
		margin-top: 20px;
		margin-bottom: 15px;
	position: absolute;
	top: auto;
		left: -1000px;
  	overflow: hidden
	/*Generally elements near end of HTML markup appear on top, edit the default order*/
	z-index: 2;	
	/*scale(2): size, skewX(-32deg): skew along coord axis, skewY(-32deg):, rotate(-45deg):, */
	transform: {};
	padding: 4px 
}

.general{
	border: 1px solid #ccc;
    	border-radius: 5px;
	/*Creates a shadow, parameters- offset-x, offset-y, blur-radius, spread-radius, colour*/
    box-shadow: 25px 10px 10px 10px green; 
	/*50% visable */
	opacity: 0.5; 
	border: 1px solid black;
	border-spacing: 5px;
	display: table;   
	clear: both;
	/*Create variable, create in :root{} for global variable*/
	--customVariable: "customVariableValue";
}

/*Responsive image*/
/*Stretchs to container but won't stretch > original width, inline element -> block element with its own line, keeps the images original aspect ratio*/
img {
    max-width: 100%;
    display: block;
    height: auto;
}

/*Retina image for higher resolution displays
One way to optimise images for retina displays is to define their width and height values as 1/2 of of the original file */

/* * selects all elements */
*, *:before, *:after {
		-moz-box-sizing: border-box;
		-webkit-box-sizing: border-box;
		box-sizing: border-box;
	}

/*selecting descendants*/
    /*select all li desecendants */
    <ul>
      <li>Item 1</li>
      <li>
        <ol>
          <li>Sub-item 2A</li>
          <li>Sub-item 2B</li>
        </ol>
      </li>
    </ul>
    ul li{} /*desendant selector: match an li elements with ul as an ancestor*/
    ul * li{} /*same as above*/ 
    /*child selector: only selects immediate descendants (first 2 li's)*/
    ul > li {}
    /*TODO add :only-child, :nth-child*/

/*to allow using invalid char in css headers, you can escape numbers so they are interpretd as their hex value in ASCII*/
.container.\31 25\25 {
		width: 100%;
		max-width: 100em;
		min-width: 80em;
    }
