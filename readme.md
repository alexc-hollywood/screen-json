# Screenplay JSON


## Why the data-driven approach is necessary
In the 1930s, screenplays were written with a typewriter, by qualified insiders who understood the Warner print formatting. In the desktop PC and web age, anyone with a copy of *Final Draft Pro* can produce a script. The market is flooded, and quality has nose-dived. Everyone is on the lookout for the new best thing out there, but overwhelmed with the material that needs coverage and triage.

**Scenario A: Too Many Scripts, Not Enough Time**

You are a development executive at a major studio. You receive 200 script submissions a week, in the post, via email, and online (e.g. the Black List). You have 10 readers, each processing 20 scripts a week, or 4 a day, at a cost of $50/script. Each reader can do 1 coverage in the morning, and another in the afternoon. Just keeping them at work costs you $40,000 a month, and you still have 200 reviews on your desk at the end.

Most get a `PASS`. 15% of them are `CONSIDER`. But 1-2 get a `RECOMMEND`. You can't produce them all - you need to prioritize. How can you run all 200 through a mill to get to the RECOMMEND pieces faster?

You'll always, always need the human element. But before all of that, there are basics.

**Scenario B: 20 Departments Doing 20 Different Breakdowns**

Not everyone needs to know everything: mainly money guys, producers, the director, and DoP. Actors just need *sides* for their audition. The armorer just needs to know when guns are featured. The Propmaster and Location Manager need lists. Background actors need the scene they're in.

So each department goes to work on their own individual breakdown of the script. This is just insanity. It takes days to manually note it all down, and it's pointless. The Director marks up his shooting script, the DoP marks up his shot list for each scene, and so on.

**Scenario C: Not Being Able to See The Forest**

A writer can sometimes take 10+ years to produce the first draft, and the script is *never finished*, even after 400 revisions/versions and twice as many mark-ups. Working on a 120 page document can be draining, and it's too easy to lose perspective.

Software can help provide a instant "Helicopter View" for editorial and scheduling, on top of providing insight into plotlines, weak spots, and opportunities a human's conditioned eyes would miss.

**Scenario D: Needing To Mark Up Screen & Interactivity Technology**

Film is no longer 35mm negative. Digital technology is changing everything every year. Amongst the things we now need to describe are:

* 3D perspective (e.g. background/foreground focus etc)
* 360 VR perspective (e.g. Oculus)
* VFX plates & CGI characters
* Multi-angle video streams (from DVD)
* Video On-Demand "Rabbit Hole" navigation through themes/screen elements
* Dynamic product placement (e.g. purchase item in this scene)

**Scenario E: Identifying Trends, Patterns, & Past Success**

Computers can process a lot of textual data very quickly: we no longer need a human to spend 2hrs on a script. We can put 500 scripts through the mill, and answer business questions of the past, and estimate future strategy/performance.

* Are we producing too much crap?
* Is the gore level getting worse and desensitizing the audience?
* What are the common elements to the most successful scripts we've produced?
* What percentage of characters are stereotypical?
* How often does a character appear across titles, and is the continuity intact?
* What subjects haven't we covered?
* Which writers produce the strongest audience response?


## What can you do with analysis?

### Quality Assurance
 - Spell checking/scoring
 - Grammar checking/scoring
 - Plagiarism checking
 - Format checking
 - Gender balance/bias (Bechdel)
 - Poor writing (cliche, alliteration etc)
 - Profanity occurrence
 - Annotation scoring

### Visualization
 - Scene cards
 - Arc structure (key plot/page markers)
 - Relationship graphing

###  Statistical Analysis (Breakdown) & Named entity recognition (NER)
 - Scenes (+ frequency/occurrence)
 - Characters (+ frequency/occurrence)
 - Locations (+ frequency/occurrence)
 - Props (+ frequency/occurrence)
 - S/VFX (+ frequency/occurrence)
 - Storyboard & asset association
 - Author contribution frequency
 - Revision comparison (Diff)

### Reprocessing
 - Import/export across applications
 - Dynamic compilation (per user)
 - Group/role access management
 - Paragraph redaction
 - Document/scene encryption

###  Advanced: Natural Language Processing
 - Story/scene pace, energy, & excitement
 - Originality
 - Dialogue sophistication
 - Descriptiveness
 - Author style analysis
 - Shortest path to efficient production
 - Morphological segmentation
 - Automatic translation
 - Alternative outcome scenarios

## Content Encryption

Encryption can be applied to any JSON value - specific *elements*, *scenes*, or the *entire document*. Each user may have their own specific private key, or password. GUI display should auto-detect an encrypted paragraph element, and attempt to decode it with a password already stored for that user.

The beauty of this approach is that no data or handshake needs to be conducted with the back-end server. An author encrypting the element can set a password in their client program, and give it to a collaborator personally, without sending it over the wire. An author can choose to hide any number of text elements to provide different selective views for different readers.

#### Example client-side AES encryption with Node

In this example, we are in a Node client like Electron or Ionic. The reader GUI is able to encrypt text, or decrypt it, by providing a specified password.

We want to hide the line of dialogue from everyone except Above-The-Line, the Director, the DoP, and the actor who is going to be saying it. Everyone gets a copy of the script, but those who are not included in the access policy, or without the specific password, see a `redacted` block.

```js
var crypto = require('crypto'), algorithm = 'aes-256-ctr', password = 'password_for_specific_individual';

function encrypt(text){
  var cipher = crypto.createCipher(algorithm,password)
  var crypted = cipher.update(text,'utf8','hex')
  crypted += cipher.final('hex');
  return crypted;
}

function decrypt(text){
  var decipher = crypto.createDecipher(algorithm,password)
  var dec = decipher.update(text,'hex','utf8')
  dec += decipher.final('utf8');
  return dec;
}

var dialogue_element = encrypt("I'm going to make him an offer he can't refuse.")
console.log(decrypt(dialogue_element));
```

Encrypted output (Base64):

```
U2FsdGVkX19voPV6ClqPfhSFJFMEALJeZSmAVl/dukvbxtfFOondqaZLVdRg0/HxBV8g8B9iZYzYY2Aa/cH7Kw==
```

The dialogue data element would then be expressed as:

```json
{
  "lang": "EN",
  "omitted": false,
  "locked": true,
  "policies" : ["ABL", "director", "cinematography", "actor-corleone"],
  "type": "dialogue",
  "version" : "0.1",
  "element": "p",
  "class": "col-md-12",
  "content" : "U2FsdGVkX19voPV6ClqPfhSFJFMEALJeZSmAVl/dukvbxtfFOondqaZLVdRg0/HxBV8g8B9iZYzYY2Aa/cH7Kw==",
  "annotations" : [
    {
      "author": "John Doe",
      "data" : "I can't read this. Can someone send me the password?"
    }
  ],
  "meta": {
	"cipher" : "aes-256-ctr",
    "auth": : "director"
  }
}
```


## Research Papers

* [*"Classifying Movie Scripts by Genre with a MEMM Using NLP-Based Features"*](http://nlp.stanford.edu/courses/cs224n/2008/reports/06.pdf) (Alex Blackstock, Matt Spitz)
* [*"SceneMaker: Automatic Visualisation of Screenplays"*](http://www.paulmckevitt.com/pubs/ki2009.pdf) (Eva Hanser, Paul Mc Kevitt, Tom Lunney,  Joan Condell)
* [*"Speaker identification from film dialogues"*](https://www.researchgate.net/publication/261044518_Speaker_identification_from_film_dialogues) (Kundu, A., Das, D., & Bandyopadhyay, S.)
* [*"Predicting Box Office from the Screenplay: An Empirical Model"*](http://repository.cmu.edu/cgi/viewcontent.cgi?article=1003&context=qatarworks) (Starling David Hunter, Susan Smith, Saba Singh)
* [*"Exploiting Structure and Conventions of Movie Scripts for Information Retrieval and Text Mining"*](https://link.springer.com/chapter/10.1007%2F978-3-540-89454-4_27) (Jhala, A)
* [*"Scene Boundary Detection from Movie Dialogue: A Genetic Algorithm Approach"*](http://www.scielo.org.mx/pdf/poli/n47/n47a7.pdf) (Amitava Kundu, Dipankar Das, Sivaji Bandyopadhyay)
* [*"Early prediction of a film’s box office success using natural language processing techniques and machine learning"*](http://trap.ncirl.ie/2531/1/seanodriscoll.pdf) (Sean O’Driscoll)
* [*"From Storyline to Box Office: A New Approach for Green-Lighting Movie Scripts"*](https://pdfs.semanticscholar.org/c928/087ef17f8820d22dcc770a82015474d1a820.pdf) (Eliashberg, Hui and Zhang)
* [*"An Annotated Corpus of Film Dialogue for Learning and Characterizing Character Style"*](https://games.soe.ucsc.edu/sites/default/files/1114_Paper.pdf) (Walker, M. A., Lin)
* [*"Gender-Distinguishing Features in Film Dialogue"*](https://aclweb.org/anthology/W/W16/W16-0204.pdf) (Alexandra Schofield, Leo Mehr, CLFL 2016)
* [*"Assessing Box Office Performance Using Movie Scripts: A Kernel-based Approach"*](https://www.deepdyve.com/lp/institute-of-electrical-and-electronics-engineers/assessing-box-office-performance-using-movie-scripts-a-kernel-based-5yzZ0sJwTL) (Jehoshua Eliashberg, Sam K. Hui, Z. John Zhang)
* [*"Pre-production forecasting of movie revenues with a dynamic artificial neural network"*](https://pdfs.semanticscholar.org/c553/03b0508ff4356ec3e20b678c9326f50e7ac8.pdf) (M. Ghiassi, David Lio, Brian Moon)
* [*"Parsing Screenplays for Extracting Social Networks from Movies"*](https://pdfs.semanticscholar.org/9b28/a69657df32cf8610d39cf1c1b9d4de3093de.pdf) ( Agarwal, A., Balasubramanian, S., Zheng, J., & Dash, S)
