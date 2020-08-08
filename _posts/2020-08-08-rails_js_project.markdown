---
layout: post
title:      "Rails+JS Project"
date:       2020-08-08 18:11:11 +0000
permalink:  rails_js_project
---


Although creating a project using both Rails and JavaScript seemed daunting at first, after I learned that a few of my cohort-mates were choosing to create a game for their projects I got excited. I had a few ideas but ultimately decided to create a version of a game my family and I used to play together when I lived at home. Our local newspaper had a games section and every day they would publish a new word in this section. The goal was to write down as many words as possible that could be formed from the published word. The words had to be at least 4 letters, could only use each letter from the published word once, and, of course, had to be actual words. My project is a web app version of this game.

I considered a few different options for implementing the logic for the game - I could install a dictionary gem and use that, along with validations on my word class, to determine whether or not the user's submitted word is considered valid. I started along this path, adding validations to my word class that ensure user's inputted words can't be blank and have to be at least 4 letters. Easy enough. I started to attempt to create the logic for ensuring each letter from the published word is used only one time each in the user's inputted word when I realized there was an easier way to validate user's input - create seed data that includes all possible valid words for each published word. 

I created a valid word class and a has many/belongs to relationship between that class and the word class, and created the seed data:

```
crooked = Word.create(name: "crooked")
crooked.valid_words.create([{ name: "cooked" }, { name: "cooker" }, { name: "corked" }, 
{ name: "docker" }, { name: "recook" }, { name: "redock" }, { name: "rocked" }, 
{ name: "rooked" }, { name: "coder" }, { name: "coked" }, { name: "cooed" }, 
{ name: "cooer" }, { name: "cored" }, { name: "credo" }, { name: "crook" }, 
{ name: "decko" }, { name: "decor" }, { name: "dooce" }, { name: "dreck" }, 
{ name: "drook" }, { name: "ocker" }, { name: "rodeo" }, { name: "roked" },
{ name: "cero" }, { name: "code" }, { name: "coed" }, { name: "coke" }, { name: "cook" },
{ name: "cord" }, { name: "core" }, { name: "cork" }, { name: "cred" }, { name: "deck" },
{ name: "deco" }, { name: "dero" }, { name: "dock" }, { name: "doco" }, { name: "doek" },
{ name: "doer" }, { name: "dook" }, { name: "door" }, { name: "dore" }, { name: "dork" },
{ name: "drek" }, { name: "ecod" }, { name: "kero" }, { name: "kore" }, { name: "koro" },
{ name: "odor" }, { name: "ordo" }, { name: "reck" }, { name: "redo" }, { name: "rock" },
{ name: "rode" }, { name: "roed" }, { name: "roke" }, { name: "rood" }, { name: "rook" }
]) 
```

Now we can easliy compare all words input by a user against all valid words for the published word. If the user's word has an exact match in that word's valid words it's a valid word, and if it doesn't, it's not.

To achieve this I first added the valid words' associated word to the valid words index controller action:

```
  def index
    @valid_words = ValidWord.all

    render json: @valid_words, include: [:word]
  end
```
	
I then created a fetch request to the valid words index page. This fetch request is included in the `fetchValidWords` function in the index.js file:

```
function fetchValidWords() {
    let bigWord = document.getElementById("big-word")
    let bigWordId = bigWord.getAttribute('data-id')
    let bigWordValidWords = []
    fetch(`${BASE_URL}/valid_words`)
    .then(resp => resp.json())
    .then(validWords => {
        for (const validWord of validWords) {
            let vw = new ValidWord(validWord.id, validWord.name, validWord.word.id)
            if (validWord.word.id == bigWordId) {
                bigWordValidWords.push(validWord)
            }
        }
    })

    return bigWordValidWords
}
```

In this function I'm locating the published word, setting it to a variable called `bigWord`, grabbing its id through the data-id attibute and setting that id to a variable called `bigWordId`. This allows me to identify the published word on the page from which the user is creating words. I then create an empty array that's set to a variable called `bigWordValidWords`. The function then makes a fetch request to the valid words index page (a get request, which does not need to be specified as fetch requests default to get), which returns a promise. When that promise is resolved we turn the response into JSON in the first `then` statement. The second `then` statement takes that JSON and stores it in the variable `validWords`. We then use a `for...of` loop to iterate over all of the valid words stored in `validWords` and create a new instance of the valid word JavaScript class with its id, name, and the id of its associated word. After that we compare the id of the newly created valid word instance's associated word against the id of the published word (stored in `bigWordId`) If there is a match - if the valid word is one of the published word's valid words - we push that valid word into the empty array stored in `bigWordValidWords`. Finally, in the last line of the function, we return the `bigWordValidWords` array, which now contains all of the valid words for the published word.

Because the `fetchValidWords` function returns the array of all valid words for the published word, we can set that function to a variable in the word JavaScript class so that this function is called when a game is started, making the array accessible as soon as the published word is displayed:

```
renderRandomWord() {
        let wordDisplay = document.querySelector("#word-display")
        let wordsHTML = 
        `
        <div id="big-word" data-id=${this.id}>
        <h1>${this.name}</h1>
        </div>
        `

        wordDisplay.innerHTML += wordsHTML
        validWords = fetchValidWords()
    }
```

When a user inputs a word and submits it, the `renderNewWord` method in the word JavaScript class is invoked. Within this method we iterate over the `validWords` array, which now contains all of the valid words for the published word after the `fetchValidWords` function was invoked in the `renderRandomWord` method when the game was started, and return the name of each valid word:

```
let words = validWords.map(function(element){
		return element.name
})
```

Finally, we add the word input by the user to the DOM if the word is included in the `words` array that contains the name of each valid word in the bolded lines below:

```
**if (words.includes(this.name)) {
            wordsDisplay.innerHTML += 
            `   
            <ul>
            <li id="${this.game_id}">${this.name}</li>
            </ul>
            `**
            
            findWordCount.innerHTML = newWordCount
            findGamePoints.innerHTML = newGamePoints
            findUserPoints.innerHTML = newUserPoints

        } else {
            alert("That is not a valid word!")
        }
        persistGameData()
    }
```
