# connectmongodb
const express = require('express')
const app = express()
const mongoose = require('mongoose')


// DO NOT SAVE YOUR PASSWORD TO GITHUB!!
if (process.argv.length<3) {
         console.log('give password as argument')
         process.exit(1)
}
const password = process.argv[2]
const url =
    `mongodb+srv://wanderi:${password}@cluster0.n06pk.mongodb.net/learningdb?retryWrites=true&w=majority`
    

mongoose.connect(url)

const noteSchema = new mongoose.Schema({
  content: String,
  date: Date,
  important: Boolean,
})

const Note = mongoose.model('Note', noteSchema)

app.get('/api/notes', (request, response) => {
    Note.find({}).then(notes => {
      response.json(notes)
    })
  })

const PORT = process.env.PORT || 3001
    app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
