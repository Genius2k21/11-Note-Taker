# Note Taker

## Summary
This app is designed to simply take notes. It allows you to save notes along with a title and plain text. It is powered by Express JS. The Note Taker App allows you to create, view and delete notes with ease. 

## Prerequisites
* [NodeJS](https://nodejs.org/)

## Installing

Clone the repository to your local development environment.

```
git clone
```

Navigate to the developer-profile-generator folder using the command prompt.

Run `npm install` to install all dependencies. To use the application locally, run `node server.js` in your CLI, and then open `http://localhost:3000` in your preferred browswer. The Note Taker app is https://notetaker2k21.herokuapp.com) for you to use as well.

## Preview:

![notetaker](https://user-images.githubusercontent.com/85536828/134100856-c0c45aa3-39a9-475e-beaf-dd2de8f3161a.JPG)

## Deployed Link:
[Note Taker App](https://notetaker2k21.herokuapp.com)

## Learning Points:
* This app was to demonstrate how to write backend code and integreate it with the given front end code (index.html, notes.html, and custom jQuery)
* This app especially helped in seeting up an Express server and implement express.static function to ensure proper delivery of local js/css filed used by html files that are delivered using .sendFile();


## Code Snippets

The following code snippet shows how the Express server initialization and setup. Routes are setup in a separate file for organization purposes.

```javascript
// Initialize express app
const app = express();
const PORT = process.env.PORT || 3000;

// Setup data parsing
app.use(express.urlencoded({ extended: true }));
app.use(express.json());
app.use(express.static(__dirname));

//Require routes file
require('./routes/routes')(app);

// Setup listener
app.listen(PORT, function() {
    console.log("App listening on PORT: " + PORT);
});  
```

The following code snippet shows how this app sets up Express routing and uses a JSON file for CRUD interactions.

```javascript
module.exports = app => {

    // Setup notes variable
    fs.readFile("db/db.json","utf8", (err, data) => {

        if (err) throw err;

        // Store the contents of the db.json file in a variable for better performance
        var notes = JSON.parse(data);

        // API ROUTES
        // ========================================================
    
        // Setup the /api/notes get route
        app.get("/api/notes", function(req, res) {
            // Read the db.json file and return all saved notes as JSON.
            res.json(notes);
        });

        // Setup the /api/notes post route
        app.post("/api/notes", function(req, res) {
            // Receives a new note, adds it to db.json, then returns the new note
            let newNote = req.body;
            notes.push(newNote);
            updateDb();
            return console.log("Added new note: "+newNote.title);
        });

        // Retrieves a note with specific id
        app.get("/api/notes/:id", function(req,res) {
            // display json for the notes array indices of the provided id
            res.json(notes[req.params.id]);
        });

        // Deletes a note with specific id
        app.delete("/api/notes/:id", function(req, res) {
            notes.splice(req.params.id, 1);
            updateDb();
            console.log("Deleted note with id "+req.params.id);
        });

        // VIEW ROUTES
        // ========================================================

        // Display notes.html when /notes is accessed
        app.get('/notes', function(req,res) {
            res.sendFile(path.join(__dirname, "../public/notes.html"));
        });
        
        // Display index.html when all other routes are accessed
        app.get('*', function(req,res) {
            res.sendFile(path.join(__dirname, "../public/index.html"));
        });

        //updates the json file whenever a note is added or deleted
        function updateDb() {
            fs.writeFile("db/db.json",JSON.stringify(notes,'\t'),err => {
                if (err) throw err;
                return true;
            });
        }

    });

}
```

## Built With
* [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
* [NodeJS](https://nodejs.org/)
* Node Packages:
    * [Express](https://www.npmjs.com/package/express)


## License
This project is licensed under the ISC License.
