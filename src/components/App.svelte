<script>
  import netdata from './NODES_LINKS.json'
  import NetworkGraph from "./NetworkGraph.svelte";
  import NetworkGraph2 from "./NetworkGraph2.svelte";
  import NetworkGraph3 from "./NetworkGraph3.svelte";
  import NetworkGraph4 from "./NetworkGraph4.svelte";
  import NetworkGraphFiltered from './NetworkGraphFiltered.svelte';
  import BubbleGraph from "./BubbleGraph.svelte";
  import ArcGraph from "./ArcGraph.svelte";
  import spiderdata from "./spiderman_network.json"
  import thordata from "./thor_network.json"
  import spiderthordata from "./FILTERED_NODES_LINKS.json"

</script>

<main>
<html lang="en">
<head>
<title>Marvel Network Graph</title>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Lato">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Montserrat">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<style>
body,h1,h2,h3,h4,h5,h6 {font-family: "Lato", sans-serif}
.w3-bar,h1,button {font-family: "Montserrat", sans-serif}
.fa-anchor,.fa-coffee {font-size:200px}
</style>
</head>
<body>

<!-- Header -->
<header class="w3-container w3-center" style="padding:128px 16px;background:#e23636;color:white;">
  <h1 class="w3-margin w3-jumbo"><b>Six Degrees of Spiderman</b></h1>
  <h2 class="w3-margin">A Marvel Comics Network Graph</h2>
  <h4 class="w3-margin">By Oren Kaplan, Ethan Lin, and Maya Que</h4>
  <p class="w3-large"><a href="http://bioinfo.uib.es/~joemiro/marvel.html">Data Source</a></p>
</header>

<!-- First Grid -->
<div class="w3-row-padding w3-light-grey w3-padding-64 w3-container">
  <div class="w3-content">
    <div class="w3-third w3-center">
      <!--<i class="fa fa-coffee w3-padding-64 w3-text-red w3-margin-right"></i>-->
      <!--https://upload.wikimedia.org/wikipedia/commons/thumb/b/b9/Marvel_Logo.svg/1280px-Marvel_Logo.svg.png-->
      <!--<img class="w3-padding-64 w3-text-red w3-margin-right" src={spiderman} width=400 height=500>-->
    </div>

    <div class="w3-twothird">
      <h1>Introduction</h1>
      <h5 class="w3-padding-32">Despite the Earth being populated by more than eight billion people, you are only six social connections away from any other person on Earth. This is the crux of the small world phenomenon. Despite the sheer size of the human population and the vast variety of cultures and lifestyles, you are indirectly connected to all other humans on this planet through a chain of social acquaintances.
      <h5 class="w3-padding-3">Intuitively, this sounds impossible. How is it possible that you know someone, who knows someone, who knows someone, who knows someone, who knows someone, who knows the President of France? Or a Tibetan monk? Or an Antarctic researcher?
      <h5> However, a series of experiments in 1969 by Stanley Milgram showed that it was in fact possible. Randomly selected participants sent packages to people they knew on a first-name basis, who would then send the package onwards, in hopes of the package eventually reaching a designated target. The average length of the completed chains was six connections, leading Milgram to conclude that all Americans are separated by a mere six degrees of separation. 
      <h5> These experiments sparked a wave of interest in social networks, which have led to thought experiments such as the Six Degrees of Kevin Bacon game. Players choose a film actor, connect them to other actors by appearance in the same film, and build a chain to Kevin Bacon. Doing so shows that all actors can be linked to Kevin Bacon in six degrees of separation.

    </div>
  </div>
</div>



<!-- Second Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>The Clusters</centered></h1>
      <div class="chart">
        <centered><BubbleGraph graph={netdata}/></centered>
        
      </div>
      <h5 class="w3-padding-32">To illustrate this concept, we considered using social network data. However, this produced ethical concerns, as it is very difficult to gather this kind of data both comprehensively and with respect for people’s privacy. Therefore, we used the Marvel Comics Universe, which consists of the stories of thousands and thousands of different characters, who often interact with each other in crossovers and collaborations. The comics therefore present us with a full picture of each of these characters’ social networks, which are best represented by a network graph. 
      <h5>By using a network graph to show the relationships between characters, as well as the strength of the relationship, we can highlight the clusters of characters that have formed. It also allows us to identify how unlikely chains can exist. By following the path between nodes, you can see the connections that link together unlikely character-pairs. For example, Harry Osborn is Spiderman’s friend, and Spiderman knows Wasp, who knows Cyclops. 
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- Spider Group Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>Spiderman's Group</centered></h1>
      <div class="chart">
        <centered><NetworkGraph graph={spiderdata}/></centered>
      </div>
      <h5 class="w3-padding-32">In our graph, we represent each character as a bubble. The color is determined by the broad affiliation of the character (i.e., Avengers, X-Men, Fantastic Four, etc.) , and the size is determined by the number of connections the character has. A connection is a link between two characters who have interacted in the comics. Interestingly, by linking size to connection count, we find that size also roughly represents how often a character appears in comics, as any appearance in a comic will likely include some form of interaction with other characters. 
      <h5> By hovering over a character’s bubble, you can see an indicator that shows their name, number of total interactions with other characters, and number of connections. Doing this also grays out the bubbles of other characters that the specified character is not connected to, highlighting who is within one degree of separation from them. By hovering over one character at a time, you can trace paths between any two characters within a couple degrees of separation.
      <h5>You can also zoom in and out and drag character bubbles around to see how the whole network reacts in a magnetic manner, keeping social clusters connected. This was the most challenging feature to implement, but it allows you to visualize the social distance between a character and others, as they shift around.</h5>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- Thor Group Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>Thor's Group</centered></h1>
      <div class="chart">
        <centered><NetworkGraph graph={thordata}/></centered>
      </div>
      
      <h5 class="w3-padding-32">In our graph, we represent each character as a bubble. The color is determined by the broad affiliation of the character (i.e., Avengers, X-Men, Fantastic Four, etc.) , and the size is determined by the number of connections the character has. A connection is a link between two characters who have interacted in the comics. Interestingly, by linking size to connection count, we find that size also roughly represents how often a character appears in comics, as any appearance in a comic will likely include some form of interaction with other characters. 
      <h5> By hovering over a character’s bubble, you can see an indicator that shows their name, number of total interactions with other characters, and number of connections. Doing this also grays out the bubbles of other characters that the specified character is not connected to, highlighting who is within one degree of separation from them. By hovering over one character at a time, you can trace paths between any two characters within a couple degrees of separation.
      <h5>You can also zoom in and out and drag character bubbles around to see how the whole network reacts in a magnetic manner, keeping social clusters connected. This was the most challenging feature to implement, but it allows you to visualize the social distance between a character and others, as they shift around.</h5>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- Combined Groups Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>How They Connect</centered></h1>
      <div class="chart">
        <centered><NetworkGraph graph={spiderthordata}/></centered>
      </div>
      <h5 class="w3-padding-32">In our graph, we represent each character as a bubble. The color is determined by the broad affiliation of the character (i.e., Avengers, X-Men, Fantastic Four, etc.) , and the size is determined by the number of connections the character has. A connection is a link between two characters who have interacted in the comics. Interestingly, by linking size to connection count, we find that size also roughly represents how often a character appears in comics, as any appearance in a comic will likely include some form of interaction with other characters. 
      <h5> By hovering over a character’s bubble, you can see an indicator that shows their name, number of total interactions with other characters, and number of connections. Doing this also grays out the bubbles of other characters that the specified character is not connected to, highlighting who is within one degree of separation from them. By hovering over one character at a time, you can trace paths between any two characters within a couple degrees of separation.
      <h5>You can also zoom in and out and drag character bubbles around to see how the whole network reacts in a magnetic manner, keeping social clusters connected. This was the most challenging feature to implement, but it allows you to visualize the social distance between a character and others, as they shift around.</h5>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- Thanos 1 Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>Thanos' First Snap</centered></h1>
      <div class="chart">
        <centered><NetworkGraph2 graph={netdata}/></centered>
      </div>
      <h5 class="w3-padding-32">In our graph, we represent each character as a bubble. The color is determined by the broad affiliation of the character (i.e., Avengers, X-Men, Fantastic Four, etc.) , and the size is determined by the number of connections the character has. A connection is a link between two characters who have interacted in the comics. Interestingly, by linking size to connection count, we find that size also roughly represents how often a character appears in comics, as any appearance in a comic will likely include some form of interaction with other characters. 
      <h5> By hovering over a character’s bubble, you can see an indicator that shows their name, number of total interactions with other characters, and number of connections. Doing this also grays out the bubbles of other characters that the specified character is not connected to, highlighting who is within one degree of separation from them. By hovering over one character at a time, you can trace paths between any two characters within a couple degrees of separation.
      <h5>You can also zoom in and out and drag character bubbles around to see how the whole network reacts in a magnetic manner, keeping social clusters connected. This was the most challenging feature to implement, but it allows you to visualize the social distance between a character and others, as they shift around.</h5>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- Thanos 2 Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>Thanos' Second Snap</centered></h1>
      <div class="chart">
        <centered><NetworkGraph3 graph={netdata}/></centered>
      </div>
      <h5 class="w3-padding-32">In our graph, we represent each character as a bubble. The color is determined by the broad affiliation of the character (i.e., Avengers, X-Men, Fantastic Four, etc.) , and the size is determined by the number of connections the character has. A connection is a link between two characters who have interacted in the comics. Interestingly, by linking size to connection count, we find that size also roughly represents how often a character appears in comics, as any appearance in a comic will likely include some form of interaction with other characters. 
      <h5> By hovering over a character’s bubble, you can see an indicator that shows their name, number of total interactions with other characters, and number of connections. Doing this also grays out the bubbles of other characters that the specified character is not connected to, highlighting who is within one degree of separation from them. By hovering over one character at a time, you can trace paths between any two characters within a couple degrees of separation.
      <h5>You can also zoom in and out and drag character bubbles around to see how the whole network reacts in a magnetic manner, keeping social clusters connected. This was the most challenging feature to implement, but it allows you to visualize the social distance between a character and others, as they shift around.</h5>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- Thanos 3 Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>Who's Left?</centered></h1>
      <div class="chart">
        <centered><NetworkGraph4 graph={netdata}/></centered>
      </div>
      <h5 class="w3-padding-32">In our graph, we represent each character as a bubble. The color is determined by the broad affiliation of the character (i.e., Avengers, X-Men, Fantastic Four, etc.) , and the size is determined by the number of connections the character has. A connection is a link between two characters who have interacted in the comics. Interestingly, by linking size to connection count, we find that size also roughly represents how often a character appears in comics, as any appearance in a comic will likely include some form of interaction with other characters. 
      <h5> By hovering over a character’s bubble, you can see an indicator that shows their name, number of total interactions with other characters, and number of connections. Doing this also grays out the bubbles of other characters that the specified character is not connected to, highlighting who is within one degree of separation from them. By hovering over one character at a time, you can trace paths between any two characters within a couple degrees of separation.
      <h5>You can also zoom in and out and drag character bubbles around to see how the whole network reacts in a magnetic manner, keeping social clusters connected. This was the most challenging feature to implement, but it allows you to visualize the social distance between a character and others, as they shift around.</h5>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- The Network  Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>The Network</centered></h1>
      <div class="chart">
        <centered><NetworkGraph graph={netdata}/></centered>
        
      </div>
      <h5 class="w3-padding-32">In our graph, we represent each character as a bubble. The color is determined by the broad affiliation of the character (i.e., Avengers, X-Men, Fantastic Four, etc.) , and the size is determined by the number of connections the character has. A connection is a link between two characters who have interacted in the comics. Interestingly, by linking size to connection count, we find that size also roughly represents how often a character appears in comics, as any appearance in a comic will likely include some form of interaction with other characters. 
      <h5> By hovering over a character’s bubble, you can see an indicator that shows their name, number of total interactions with other characters, and number of connections. Doing this also grays out the bubbles of other characters that the specified character is not connected to, highlighting who is within one degree of separation from them. By hovering over one character at a time, you can trace paths between any two characters within a couple degrees of separation.
      <h5>You can also zoom in and out and drag character bubbles around to see how the whole network reacts in a magnetic manner, keeping social clusters connected. This was the most challenging feature to implement, but it allows you to visualize the social distance between a character and others, as they shift around.</h5>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- Fourth Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>Arc Diagram</centered></h1>
      <div>
        <centered><ArcGraph graph={netdata}/></centered>
        
      </div>
      <h5 class="w3-padding-32">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</h5>

      <p class="w3-text-grey">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Excepteur sint
        occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco
        laboris nisi ut aliquip ex ea commodo consequat.</p>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>


<div class="w3-container w3-black w3-center w3-opacity w3-padding-64">
    <h1 class="w3-margin w3-xlarge">Write Up:</h1>
</div>

</body>
</html>
</main>

<style>
  
	.chart {
		width: 1225px;
    /*max-width: 440px;*/
    height: 600px;
    float: right;
    /*
		max-width: 640px;
    height: 500px;
		/*height: calc(100% - 4em);*/
		min-height: 280px;
		max-height: 4080px;
		margin: 0 auto;
    border-style: solid;
    border: 10px;
    float: left;
	}

</style>
