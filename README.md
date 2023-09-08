"# test" 
const express = require('express');

const app = express();

const port = 3001;

const bodyParser = require('body-parser');




app.use(express.static('public'));

app.use(bodyParser.urlencoded({ extended: true }));

const mysql = require('mysql2');




const connection = mysql.createConnection({

  host: "192.168.1.161",

  user: "daniel.nobre",

  password: 'heslonezapomenu22',

  database: 'nobre databaze',

  port: 3001 // Replace with the appropriate MySQL server port

});




app.get('/trida', (req, res) => {

  // Query the database

  connection.query('SELECT * FROM trida', (error, results, fields) => {

    if (error) {

      console.error(error);

      return;

    }

    console.log(results);




    let tableRows = `<tr><td>Jmeno</td><td>Prijmeni</td></tr>`;




    results.forEach(trida => {

      tableRows += `<tr><td style="border: 1px solid black">${trida.Jmeno}</td><td style="border: 1px solid black">${trida.Prijmeni}</td></tr>`;

    });




    const table = `<table>${tableRows}</table>`;

    console.log(table);

    res.send(table);

  });

});




app.post('/trida', function (request, response, next) {

 

  // SQL dotaz pro vložení dat do databáze

  var sql = `INSERT INTO trida (Jmeno, Prijmeni) VALUES ('${request.body.Jmeno}', '${request.body.Prijmeni}')`;

 

  connection.query(sql, (error, results, fields) => {

    if (error) {

      console.error(error);

      return;

    }

    console.log(results);

  })

  response.send(`Uživatele byli vloženi do DB`)

 

})




app.listen(port, () => {

  console.log(`Server is running on port ${port}`);

});
