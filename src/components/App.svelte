<script>
  import netdata from './NODES_LINKS.json'
  import NetworkGraph from "./NetworkGraph.svelte";
  import NetworkGraph1 from "./NetworkGraph1.svelte";
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
      <h5 class="w3-padding-32">Despite the Earth being populated by more than eight billion people, you are only about six social connections away from any other person on Earth. This is the crux of the small world phenomenon. Despite the sheer size of the human population and the vast variety of cultures and lifestyles, you are indirectly connected to all other humans on this planet through a short chain of social acquaintances.
      <h5 class="w3-padding-3">Intuitively, this sounds impossible. How is it possible that you know someone, who knows someone, who knows someone, who knows someone, who knows someone, who knows the President of France? Or a Tibetan monk? Or an Antarctic researcher?
      <h5 class="w3-padding-3">To answer this, we can turn to the Marvel superhero comic books, which consist of the stories of thousands and thousands of different characters, who often interact with each other in crossovers and collaborations. The comics therefore present us with a full picture of each of these characters’ social networks. 
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
        <centered><NetworkGraph1 graph={spiderdata}/></centered>
      </div>
      <br><br>
      <h5 class="w3-padding-32">This is Spider-Man, his family, his friends, and his enemies. They live in New York and, for the most part, consist of regular human beings (don't worry about the second Spider-Man, that's a clone running around).
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
        <centered><NetworkGraph1 graph={thordata}/></centered>
      </div>
      
      <h5 class="w3-padding-32"> This is Thor, his family, his friends, and his enemies. They live on Asgard, the mythical home of the Norse Gods and, for the most part, consist of the aforementioned Norse Gods.
      <h5>That being the case, if Hogun, an Asgardian, wanted to get his favorite sword signed by Daredevil, a human man, how would he pass it along to make that happen?</h5>
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
        <centered><NetworkGraph1 graph={spiderthordata}/></centered>
      </div>
      <h5 class="w3-padding-32">Quite easily, actually! Due to Thor and Spider-Man being teammates on the Avengers, the two groups share a fair amount of inter-group connections. Heimdall could simply pass it to Thor, who could pass it to Spider-Man, who could pass it to Daredevil, allowing Heimdall to get his sword signed by his favorite superhero! Despite being two very different people from two very different worlds, Hogun and Daredevil are only three degrees of separation apart!
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
      <h5 class="w3-padding-32">Now let's take a look at another example. Thanos has gotten his hands on the Infinity gauntlet, along with all of the Infinity Stones, giving him god-like powers!
      <h5>Usually, he would wipe out half of all life in the universe to deal with overpopulation. But since he's feeling nice today, he's only going to erase everyone within three degrees of separation from himself. That won't be too bad, right?
      <h5>To do so, he decides that with each snap, he'll erase one degree of separation from himself. Snap! A dozen people turn to dust.</h5>
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
      <h5 class="w3-padding-32"> Snap! Uh-oh! That wiped out a lot more people than Thanos thought it would!
      <h5> Now he feels bad, so he decides to stop there.</h5>
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
      <h5 class="w3-padding-32">How are there so few people left? While the first snap only erased a small group of people, those people knew a LOT of people. Each one knew at least twenty others, and though there was some overlap, that meant more than a hundred people got erased!
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<!-- The Explanation Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>The Small-World Phenomenon</centered></h1>
      <h5>So how is it that wildly different and distant people can pass things along so quickly? And how is it that even two degrees of separation from one person can include so many people?</h5>
     <h5> A series of experiments in 1969 by Stanley Milgram showed how this phenomenon could exist in the real world. Randomly selected participants sent packages to people they knew on a first-name basis, who would then send the package onwards, in hopes of the package eventually reaching a designated target. The average length of the completed chains was six connections, leading Milgram to conclude that all Americans are separated by a mere six degrees of separation. 
        <h5> These experiments sparked a wave of interest in social networks, which have led to thought experiments such as the Six Degrees of Kevin Bacon game. Players choose a film actor, connect them to other actors by appearance in the same film, and build a chain to Kevin Bacon. Doing so shows that all actors can be linked to Kevin Bacon in six degrees of separation.
      <h5>So if you think about it, you can do the same thing with Spider-Man!</h5>
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
      <h5 class="w3-padding-32">While our data does not include every single person in the Marvel comics universe, you can still simulate Milgram's experiment with our network graph! You'll find that randomly picking any character will lead you back to Spider-Man in just a couple connections.
      <h5> If you'd like a demo explaining how we developed this network graph and how to use it, check out our demo below.</h5>
      <a href=https://youtu.be/P54tt7OG2Q0>Demo</a>

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
      <h5 class="w3-padding-32">We also have this arc diagram to show you how everyone in the dataset knows each other.
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>Conclusion</centered></h1>
      
      <h5 class="w3-padding-32">As you can see, the microcosm that is the Marvel Comics reflects the real-world small-world phenomenon. Visualizing the social network within the Marvel Comics Universe shows us how one’s social network often overlaps with a great variety of others, allowing paths to be drawn between incredibly different people. We know that the world feels impossibly large and intimidating, with billions of people on the planet, each with their own cultures, opinions, and personalities. But each person in the world is only about six social connections away. Therefore, by reaching out to the people around you, you can find an infinite amount of new connections, experiences and opportunities. It's probably a lot easier to get your favorite celebrity's signature than you'd think!

    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
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
